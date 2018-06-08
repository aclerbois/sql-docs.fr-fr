---
title: Utilisation du chiffrement intégral avec le pilote ODBC pour SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 10/01/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 02e306b8-9dde-4846-8d64-c528e2ffe479
caps.latest.revision: 3
ms.author: v-chojas
manager: craigg
author: MightyPen
ms.openlocfilehash: e78ced26aba0f755184d6f6bb9e16ab073a7509e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-always-encrypted-with-the-odbc-driver-for-sql-server"></a>Utilisation du chiffrement intégral avec le pilote ODBC pour SQL Server
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

### <a name="applicable-to"></a>S’applique à

- ODBC Driver 13.1 for SQL Server
- Pilote ODBC 17 pour SQL Server

### <a name="introduction"></a>Introduction

Cet article fournit des informations sur la façon de développer des applications ODBC à l’aide de [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) et [pilote ODBC pour SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md).

Always Encrypted permet aux applications clientes de chiffrer des données sensibles et de ne jamais révéler les données ou les clés de chiffrement à SQL Server ou Azure SQL Database. Un pilote avec Always Encrypted, telles que le pilote ODBC pour SQL Server, cela, le chiffrement et déchiffrement des données sensibles dans l’application cliente de façon transparente. Le pilote détermine automatiquement les paramètres de requêtes qui correspondent aux colonnes de base de données sensibles (protégées avec Always Encrypted) et chiffre les valeurs de ces paramètres avant de transmettre les données à SQL Server ou Azure SQL Database. De même, il déchiffre de manière transparente les données récupérées dans les colonnes de base de données chiffrées, qui figurent dans les résultats de la requête. Pour plus d’informations, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

### <a name="prerequisites"></a>Configuration requise

Configurez Always Encrypted dans votre base de données. Pour cela, vous devez mettre en service des clés Always Encrypted et configurer le chiffrement pour les colonnes de base de données sélectionnées. Si vous n’avez pas déjà une base de données dans laquelle est configuré Always Encrypted, suivez les instructions de [Prise en main d’Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). En particulier, votre base de données doit contenir les définitions de métadonnées pour une table qui contient une ou plusieurs colonnes chiffrées à l’aide de cette clé, une clé de chiffrement de colonne (CEK) et une clé principale de colonne (CMK).

### <a name="enabling-always-encrypted-in-an-odbc-application"></a>Activer le chiffrement intégral dans une Application ODBC

Le moyen le plus simple pour activer le paramètre chiffrement et déchiffrement de colonne chiffrée un jeu de résultats est en définissant la valeur de la `ColumnEncryption` mot clé de chaîne de connexion à **activé**. Voici un exemple de chaîne de connexion qui active le chiffrement intégral :

```
SQLWCHAR *connString = L"Driver={ODBC Driver 13 for SQL Server};Server={myServer};Trusted_Connection=yes;ColumnEncryption=Enabled;";
```

Always Encrypted peut également être activé dans la configuration de la source de données, à l’aide de la même clé et la valeur (qui sera remplacée par le paramètre de chaîne connexion, le cas échéant), ou par programme avec le `SQL_COPT_SS_COLUMN_ENCRYPTION` attribut de préconnexion. Définition de cette façon substitue la valeur définie dans la chaîne de connexion ou de la source de données :

```
 SQLSetConnectAttr(hdbc, SQL_COPT_SS_COLUMN_ENCRYPTION, (SQLPOINTER)SQL_COLUMN_ENCRYPTION_ENABLE, 0);
```

Une fois activé pour la connexion, le comportement d’Always Encrypted peut être ajusté pour des requêtes individuelles. Consultez [contrôle les performances Impact d’Always Encrypted](#controlling-the-performance-impact-of-always-encrypted) ci-dessous pour plus d’informations.

Notez que l’activation du chiffrement intégral n’est pas suffisant pour le chiffrement ou le déchiffrement réussisse. Vous devez également vous assurer que :

- L’application dispose des autorisations de base de données *VIEW ANY COLUMN MASTER KEY DEFINITION* et *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* qui sont nécessaires pour accéder aux métadonnées des clés Always Encrypted dans la base de données. Pour plus d’informations, consultez [les autorisations de base de données](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).

- L’application peut accéder à la clé CMK qui protège les clés cek pour les colonnes chiffrées interrogées. Cela dépend du fournisseur de magasins de clés qui stocke la clé CMK. Consultez [utilisation des magasins de clés principales de colonne](#working-with-column-master-key-stores) pour plus d’informations.

### <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Récupération et modification des données dans des colonnes chiffrées

Une fois que vous activez le chiffrement intégral sur une connexion, vous pouvez utiliser les API ODBC standard (consultez [exemple de code ODBC](https://code.msdn.microsoft.com/windowsapps/ODBC-sample-191624ae/sourcecode?fileId=51137&pathId=1980325953) ou [de référence du programmeur ODBC](https://msdn.microsoft.com/library/ms714177(v=vs.85).aspx)) pour récupérer ou modifier des données dans les colonnes de la base de données chiffrée. Si votre application a requis des autorisations de base de données et peut accéder à la clé principale de colonne, le pilote chiffre tous les paramètres de requête qui ciblent des colonnes chiffrées et déchiffrement les données extraites des colonnes chiffrées, se comporte de manière transparente à la application comme si les colonnes n’étaient pas chiffrés.

Si Always Encrypted n’est pas activée, les requêtes avec paramètres qui ciblent des colonnes chiffrées échouent. Données peuvent toujours être récupérées à partir des colonnes chiffrées, tant que la requête n’a aucun paramètre ciblant des colonnes chiffrées. Toutefois, le pilote ne tente pas de n’importe quel déchiffrement et l’application recevra les données chiffrées binaires (en tant que tableaux d’octets).

Le tableau ci-dessous récapitule le comportement des requêtes, selon que le chiffrement intégral est activé ou non :

|Caractéristique de la requête | Always Encrypted est activé et l’application peut accéder aux clés et à leurs métadonnées|Always Encrypted est activé et l’application ne peut pas accéder aux clés et à leurs métadonnées | Always Encrypted est désactivé|
|:---|:---|:---|:---|
| Paramètres ciblant des colonnes chiffrées. | Des valeurs de paramètres sont chiffrées en toute transparence. | Erreur | Erreur|
| La récupération des données des colonnes chiffrées, sans paramètres ciblant des colonnes chiffrées.| Les résultats de colonnes chiffrées sont déchiffrés de manière transparente. L’application reçoit des valeurs de colonne en texte clair. | Erreur | Les résultats des colonnes chiffrées ne sont pas déchiffrés. L’application reçoit des valeurs chiffrées en tant que tableaux d’octets.

Les exemples suivants illustrent la récupération et la modification de données dans des colonnes chiffrées. Les exemples supposent une table avec le schéma suivant. Notez que les colonnes SSN et BirthDate sont chiffrées.

```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] )
 GO
```

#### <a name="data-insertion-example"></a>Exemple d’Insertion de données

Cet exemple insère une ligne dans la table Patients. Notez les points suivants :

- L’exemple de code ne contient aucun élément spécifique au chiffrement. Le pilote détecte automatiquement et chiffre les valeurs des paramètres date et numéro de sécurité sociale, qui ciblent des colonnes chiffrées. Le chiffrement est donc transparent pour l’application.

- Les valeurs insérées dans des colonnes de la base de données, y compris les colonnes chiffrées, sont passés comme paramètres liés (consultez [fonction SQLBindParameter](https://msdn.microsoft.com/library/ms710963(v=vs.85).aspx)). Alors que l’utilisation de paramètres est facultative lors de l’envoi des valeurs pour les colonnes non chiffrées (bien qu’il est fortement recommandé, car elle contribue à empêcher l’injection SQL), il est nécessaire pour les valeurs ciblant des colonnes chiffrées. Si les valeurs insérées dans les colonnes SSN ou BirthDate ont été passés en tant que littéraux incorporés dans l’instruction de requête, la requête échoue, car le pilote ne tente pas de chiffrer ou de littéraux dans les requêtes de processus. Par conséquent, le serveur les rejettera en les considérant comme incompatibles avec les colonnes chiffrées.

- Le type du paramètre inséré dans la colonne SSN SQL a la valeur SQL_CHAR, ce qui correspond à la **char** type de données SQL Server (`rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);`). Si le type du paramètre a été défini à SQL_WCHAR, qui est mappée à **nchar**, la requête échoue, car Always Encrypted ne prend pas en charge les conversions de côté serveur à partir de valeurs nchar chiffrées en valeurs char chiffré. Consultez [de référence du programmeur ODBC--Appendix D: Data Types](https://msdn.microsoft.com/library/ms713607.aspx) pour plus d’informations sur les mappages de types de données.

```
    SQL_DATE_STRUCT date;
    SQLLEN cbdate;   // size of date structure  

    SQLCHAR SSN[12];
    strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

    SQLWCHAR* firstName = L"Catherine";
    SQLWCHAR* lastName = L"Abel";
    SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

    // Initialize the date structure  
    date.day = 10;
    date.month = 9;
    date.year = 1996;

    // Size of structures   
    cbdate = sizeof(SQL_DATE_STRUCT);

    SQLRETURN rc = 0;

    string queryText = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?) ";

    rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

    //SSN
    rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);
    //FirstName
    rc = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)firstName, 0, &cbFirstName);
    //LastName
    rc = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);
    //BirthDate
    rc = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 10, 0, (SQLPOINTER)&date, 0, &cbdate);

    rc = SQLExecute(hstmt);
```

#### <a name="plaintext-data-retrieval-example"></a>Exemple d’extraction de données en texte clair

L’exemple suivant montre le filtrage de données basé sur des valeurs chiffrées, ainsi que la récupération de données en texte clair à partir de colonnes chiffrées. Notez les points suivants :

- La valeur utilisée dans la clause WHERE pour filtrer sur la colonne SSN doit être transmis à l’aide de SQLBindParameter, afin que le pilote peut chiffrer de manière transparente avant de l’envoyer au serveur.

- Toutes les valeurs sont imprimées par le programme sera en texte clair, étant donné que le pilote déchiffre de manière transparente les données récupérées à partir des colonnes SSN et BirthDate.

> [!NOTE]
> Les requêtes peuvent effectuer des comparaisons d’égalité sur les colonnes chiffrées uniquement si le chiffrement est déterministe. Pour plus d’informations, consultez [chiffrement en sélectionnant déterministe ou aléatoire](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[SSN] = ? ";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//SSN
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="ciphertext-data-retrieval-example"></a>Exemple d’extraction de données de texte chiffré

Si Always Encrypted n’est pas activé, une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne ciblent des colonnes chiffrées.

L’exemple suivant illustre la récupération des données chiffrées binaires à partir des colonnes chiffrées. Notez les points suivants :

- Étant donné qu’Always Encrypted n’est pas toujours activé dans la chaîne de connexion, la requête retourne des valeurs SSN et BirthDate chiffrées sous la forme de tableaux d’octets (le programme convertit les valeurs en chaînes).
- Une requête qui récupère des données à partir de colonnes chiffrées lorsqu’Always Encrypted est désactivé peut avoir des paramètres, tant qu’aucun d’eux ne cible une colonne chiffrée. La requête ci-dessus filtre par nom (LastName), ce qui n’est pas chiffré dans la base de données. Si la requête filtre par SSN ou BirthDate, la requête échoue.


```
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[LastName] = ?";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//LastName
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Éviter les problèmes courants lors de l’interrogation de colonnes chiffrées

Cette section décrit les catégories d’erreurs courantes lors de l’interrogation des colonnes chiffrées à partir des applications ODBC et quelques instructions sur la façon de les éviter.

##### <a name="unsupported-data-type-conversion-errors"></a>Erreurs liées à la conversion de types de données non pris en charge

Always Encrypted ne prend en charge que peu de conversions de types de données chiffrées. Consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) pour la liste détaillée des conversions de type pris en charge. Pour éviter les erreurs de conversion de type de données, assurez-vous que vous observez les points suivants lors de l’utilisation de SQLBindParameter avec des paramètres ciblant des colonnes chiffrées :

- Le type SQL du paramètre est soit exactement la même que le type de la colonne cible, ou la conversion du type SQL pour le type de la colonne est pris en charge.

- La précision et l’échelle des paramètres ciblant des colonnes de la `decimal` et `numeric` les types de données SQL Server est identique à la précision et l’échelle configurée pour la colonne cible.

- La précision des paramètres ciblant des colonnes de `datetime2`, `datetimeoffset`, ou `time` les types de données SQL Server n’est pas supérieure à la précision de la colonne cible, dans les requêtes qui modifient la colonne cible.  

##### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Erreurs dues au passage de texte en clair au lieu de valeurs chiffrées

Les valeurs qui ciblent une colonne chiffrée doivent être chiffré avant d’être envoyées au serveur. Tentative d’insertion, modifier, ou filtrer sur une valeur en texte clair sur une colonne chiffrée entraîne une erreur. Pour éviter ces erreurs, assurez-vous que :

- Always Encrypted est activé (dans la source de données, la chaîne de connexion avant de vous connecter en définissant le `SQL_COPT_SS_COLUMN_ENCRYPTION` attribut de connexion pour une connexion spécifique, ou le `SQL_SOPT_SS_COLUMN_ENCRYPTION` attribut d’instruction pour une instruction spécifique).

- Vous utilisez SQLBindParameter pour envoyer des données ciblant des colonnes chiffrées. L’exemple ci-dessous illustre une requête qui filtre incorrectement par une littéral ou d’une constante sur une colonne chiffrée (SSN), au lieu de passer le littéral en tant qu’argument à SQLBindParameter.

```
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

### <a name="precautions-when-using-sqlsetpos-and-sqlmoreresults"></a>Précautions lorsque vous utilisez SQLSetPos et SQLMoreResults

#### <a name="sqlsetpos"></a>SQLSetPos

Le `SQLSetPos` API permet à une application mettre à jour des lignes dans un jeu de résultats à l’aide de mémoires tampons qui ont été lié avec SQLBindCol et en ligne qui a été extraite précédemment des données. Dû au comportement de remplissage asymétrique des types de longueur fixe chiffrés, il est possible de façon inattendue modifier les données de ces colonnes lors de l’exécution des mises à jour sur d’autres colonnes dans la ligne. Avec AE, les valeurs de caractères de longueur fixe sont complétées si la valeur est inférieure à la taille de mémoire tampon.

Pour éviter ce problème, utilisez le `SQL_COLUMN_IGNORE` indicateur permettant d’ignorer les colonnes qui ne seront pas mis à jour dans le cadre de `SQLBulkOperations` et lorsque vous utilisez `SQLSetPos` pour le curseur en fonction des mises à jour.  Toutes les colonnes qui ne sont pas être modifiées directement par l’application doivent être ignorées, à la fois pour les performances et pour éviter toute troncation des colonnes qui sont liés à une mémoire tampon *plus petits* que leur taille réelle de (DB). Pour plus d’informations, consultez [référence de fonction SQLSetPos](https://msdn.microsoft.com/library/ms713507(v=vs.85).aspx).

#### <a name="sqlmoreresults--sqldescribecol"></a>SQLMoreResults & SQLDescribeCol

Programmes d’application peuvent appeler [SQLDescribeCol](https://msdn.microsoft.com/library/ms716289(v=vs.85).aspx) pour retourner des métadonnées sur les colonnes dans les instructions préparées.  Quand le chiffrement intégral est activé, l’appel `SQLMoreResults` *avant* appel `SQLDescribeCol` entraîne [sp_describe_first_result_set](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) pour être appelée, ce qui ne renvoie pas correctement le texte en clair métadonnées pour les colonnes chiffrées. Pour éviter ce problème, appelez `SQLDescribeCol` sur les instructions préparées *avant* appel `SQLMoreResults`.

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Contrôle de l’impact sur les performances de chiffrement intégral

Always Encrypted étant une technologie de chiffrement côté client, la majeure partie de la surcharge des performances est observée côté client, pas dans la base de données. Outre le coût des opérations de chiffrement et le déchiffrement, les autres sources de problèmes de performances côté client sont :

- Allers-retours supplémentaires vers la base de données pour récupérer des métadonnées pour les paramètres de requête.

- Appels au magasin de clés principales de colonne pour accéder à une clé principale de colonne.

Cette section décrit les optimisations des performances intégrés dans le pilote ODBC pour SQL Server et comment vous pouvez contrôler l’impact de ces deux facteurs sur les performances.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Contrôle des allers-retours pour récupérer des métadonnées pour les paramètres de requête

Si Always Encrypted est activé pour une connexion, le pilote sera, par défaut, appelez [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) pour chaque requête paramétrable, en passant l’instruction de requête (sans les valeurs de paramètre) à SQL Server. Cette procédure stockée analyse l’instruction de requête pour déterminer si tous les paramètres doivent être chiffrés et si tel est le cas, retourne les informations relatives au chiffrement pour chaque paramètre permettre au pilote de les chiffrer. Ce comportement garantit un haut niveau de transparence à l’application cliente : l’application (et le développeur d’applications) sans devoir de connaître les requêtes qui accèdent à des colonnes chiffrées, tant que les valeurs ciblant des colonnes chiffrées sont passées à le pilote dans les paramètres.

### <a name="per-statement-always-encrypted-behavior"></a>Instruction Always Encrypted comportement

Pour contrôler l’impact sur les performances de la récupération des métadonnées de chiffrement pour les requêtes paramétrables, vous pouvez modifier le comportement d’Always Encrypted pour les requêtes si elle a été activée sur la connexion. De cette manière, vous pouvez vous assurer que `sys.sp_describe_parameter_encryption` est appelée uniquement pour les requêtes qui ont des paramètres ciblant des colonnes chiffrées. Toutefois, notez qu’en procédant ainsi, vous réduisez la transparence du chiffrement : Si vous chiffrez des colonnes supplémentaires dans votre base de données, vous devrez peut-être modifier le code de votre application pour l’aligner sur les modifications de schéma.

Pour contrôler le comportement d’une instruction de Always Encrypted, Appelez SQLSetStmtAttr pour définir la `SQL_SOPT_SS_COLUMN_ENCRYPTION` attribut d’instruction à une des valeurs suivantes :

|Valeur| Description|
|-|-|
|`SQL_CE_DISABLED` (0)|Always Encrypted est désactivé pour l’instruction|
|`SQL_CE_RESULTSETONLY` (1)|Déchiffrement. Jeux de résultats et valeurs de retour sont déchiffrées et les paramètres ne sont pas chiffrés.|
|`SQL_CE_ENABLED` (3) | Always Encrypted est activé et utilisé pour les paramètres et les résultats|

Nouveaux descripteurs d’instruction créés à partir d’une connexion avec le chiffrement intégral activée par défaut pour SQL_CE_ENABLED. Ces créé à partir d’une connexion avec elle désactivée par défaut pour SQL_CE_DISABLED (et il n’est pas possible d’activer le chiffrement intégral sur les.)

Si la plupart des requêtes d’une application cliente accéder à des colonnes chiffrées, les recommandations suivantes :

- Définir le `ColumnEncryption` mot clé de chaîne de connexion à `Enabled`.

- Définir le `SQL_SOPT_SS_COLUMN_ENCRYPTION` attribut `SQL_CE_DISABLED` sur des instructions qui n’accèdent pas à toutes les colonnes chiffrées. Cette opération va désactiver l’appel à la fois `sys.sp_describe_parameter_encryption` ainsi que les tentatives de déchiffrer les valeurs dans le résultat de la définir.
    
- Définir le `SQL_SOPT_SS_COLUMN_ENCRYPTION` attribut `SQL_CE_RESULTSETONLY` sur des instructions qui ne possèdent pas d’aucun paramètre exigeant un chiffrement, mais récupèrent les données des colonnes chiffrées. Cette opération va désactiver l’appel `sys.sp_describe_parameter_encryption` et chiffrement des paramètres. Résultats contenant des colonnes chiffrées continueront à déchiffrer.

## <a name="always-encrypted-security-settings"></a>Chiffrement intégral des paramètres de sécurité

### <a name="force-column-encryption"></a>Forcer le chiffrement de colonne

Pour appliquer le chiffrement d’un paramètre, définissez le `SQL_CA_SS_FORCE_ENCRYPT` champ de descripteur de paramètre d’implémentation (IPD) via un appel à la fonction SQLSetDescField. Une valeur différente de zéro, le pilote à retourner une erreur lorsque aucune métadonnée de chiffrement n’est retournée pour le paramètre associé.

```
SQLHDESC ipd;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, &ipd, 0, 0);
SQLSetDescField(ipd, paramNum, SQL_CA_SS_FORCE_ENCRYPT, (SQLPOINTER)TRUE, SQL_IS_SMALLINT);   
```

Si SQL Server informe le pilote que le paramètre ne doit pas être chiffré, les requêtes à l’aide de ce paramètre échoue. Cela fournit une protection supplémentaire contre les attaques de sécurité qui impliquent un serveur SQL Server compromis fournissant des métadonnées de chiffrement incorrectes au client, qui peut entraîner la divulgation de données.

### <a name="column-encryption-key-caching"></a>Clé de chiffrement de colonne mise en cache

Pour réduire le nombre d’appels à un magasin de clés principales de colonne pour déchiffrer les clés de chiffrement de colonne, le pilote met en cache le clés cek clair dans la mémoire. Le cache de la clé CEK est global pour le pilote et n’est pas associé avec une connexion. Après avoir reçu le ECEK à partir des métadonnées de la base de données, le pilote tente d’abord de trouver la clé CEK en texte clair correspondant à la valeur de clé chiffrée dans le cache. Le pilote appelle le magasin de clés contenant la clé CMK uniquement s’il ne peut pas trouver le texte en clair correspondante CEK dans le cache.

> [!NOTE]
> Dans le pilote ODBC pour SQL Server, les entrées dans le cache sont supprimées après un délai d’attente de deux heures. Cela signifie que, pour un ECEK donné, le pilote contacte le magasin de clés qu’une seule fois pendant la durée de vie de l’application ou de toutes les deux heures, si elle est inférieure.

À compter du 17.1 du pilote ODBC pour SQL Server, le délai d’expiration du cache de clé CEK peut être ajusté à l’aide de la `SQL_COPT_SS_CEKCACHETTL` attribut de connexion, qui spécifie le nombre de secondes pendant lesquelles une clé CEK restent dans le cache. En raison de la nature globale du cache, cet attribut peut être ajusté à partir d’un handle de connexion valide pour le pilote. Lorsque le cache de durée de vie est réduite, clés cek existant qui dépasse la nouvelle durée de vie les sont également supprimés. Si la valeur est 0, aucun clés cek n’est mis en cache.

### <a name="trusted-key-paths"></a>Chemins de clés approuvés

À compter du 17.1 du pilote ODBC pour SQL Server, le `SQL_COPT_SS_TRUSTEDCMKPATHS` attribut de connexion permet à une application pour que les opérations de chiffrement intégral utilisent uniquement une liste de clés de migration certifiables, identifiés par leurs chemins d’accès de clé spécifiée. Par défaut, cet attribut est NULL, ce qui signifie que le pilote accepte n’importe quel chemin de la clé. Pour utiliser cette fonctionnalité, définissez `SQL_COPT_SS_TRUSTEDCMKPATHS` pour pointer vers une chaîne à caractères larges délimitée par null, se terminant par null qui répertorie les chemins d’accès clé autorisées. La mémoire vers lequel pointée cet attribut doit rester valide pendant les opérations de chiffrement ou déchiffrement utilisant le descripteur de connexion sur lequel il a la valeur---sur laquelle le pilote vérifie si le chemin de clé CMK tel que spécifié par les métadonnées du serveur n’est pas la casse dans cette liste. Si le chemin d’accès de la clé CMK n’est pas dans la liste, l’opération échoue. L’application peut modifier le contenu de mémoire que pointe de cet attribut, pour modifier la liste des clés de migration certifiables approuvés, sans définir l’attribut à nouveau.

## <a name="working-with-column-master-key-stores"></a>Utilisation de magasins de clés principales de colonne

Pour chiffrer ou déchiffrer des données, le pilote doit obtenir une clé CEK qui est configurée pour la colonne cible. Clés cek est stockées dans un format chiffré (ECEKs) dans les métadonnées de la base de données. Chaque clé a une clé CMK correspondante ayant servie à chiffrer. Le [les métadonnées de base de données](../../t-sql/statements/create-column-master-key-transact-sql.md) ne stocke pas de la clé CMK lui-même ; il contient uniquement le nom du magasin de clés et des informations qui le magasin de clés permet de localiser la clé CMK.

Pour obtenir la valeur de texte en clair d’un ECEK, le pilote obtient d’abord les métadonnées relatives à la clé CEK et sa clé CMK correspondant et puis il utilise ces informations pour contacter le magasin de clés contenant la clé CMK et des demandes pour déchiffrer le ECEK. Le pilote communique avec un magasin de clés à l’aide d’un fournisseur de magasin de clés.

### <a name="built-in-keystore-providers"></a>Fournisseurs de magasins de clés intégrée

Le pilote ODBC pour SQL Server est fourni avec les fournisseurs de magasin de clés intégrés suivants :

| Nom |  Description | Nom du fournisseur (métadonnées) |Disponibilité|
|:---|:---|:---|:---|
|Coffre de clé Azure |Clés de migration certifiables magasins dans un coffre de clés Azure | `AZURE_KEY_VAULT` |Windows, macOS, Linux|
|Magasin de certificats Windows|Stocke les clés de migration certifiables localement dans le magasin de clés de Windows| `MSSQL_CERTIFICATE_STORE`|Windows|

- Vous (ou votre administrateur) doivent s’assurer que le nom du fournisseur, configuré dans les métadonnées de clé principale de colonne, est correct et que le chemin d’accès de la clé principale de la colonne est compatible avec le format du chemin de la clé pour le fournisseur spécifié. Il est recommandé de configurer les clés à l’aide d’outils tels que SQL Server Management Studio, qui génère automatiquement des noms de fournisseurs et des chemins de clés valides lors de l’émission de l’instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/create-column-master-key-transact-sql.md) .

- Vous devez vous assurer de votre application peut accéder à la clé dans le magasin de clés. Cela peut impliquer l’octroi de votre application à accéder à la clé ou le magasin de clés, selon le magasin de clés, ou effectuer d’autres étapes de configuration du magasin de clés spécifique. Par exemple, pour accéder à un coffre de clés Azure, vous devez fournir les informations d’identification correctes pour le magasin de clés.

### <a name="using-the-azure-key-vault-provider"></a>L’utilisation du fournisseur Azure Key Vault

Azure Key Vault est un outil est très pratique qui permet de stocker et de gérer des clés principales de colonne Always Encrypted, en particulier si vos applications sont hébergées dans Azure. Le pilote ODBC pour SQL Server sur Windows, Linux et macOS inclut un fournisseur de magasins de clé principale de colonne intégré pour Azure Key Vault. Consultez [le coffre de clés Azure - étape par étape](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/), [mise en route avec le coffre de clés](https://azure.microsoft.com/documentation/articles/key-vault-get-started/), et [création des clés de principales de colonne dans le coffre de clés Azure](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_2) pour plus d’informations sur la configuration d’une clé Azure Coffre pour Always Encrypted.

Le pilote prend en charge l’authentification auprès d’Azure Key Vault à l’aide des types d’informations d’identification suivants :

- Nom d’utilisateur/mot de passe – avec cette méthode, les informations d’identification sont le nom d’un utilisateur Azure Active Directory et son mot de passe.

- ID de client/Secret : avec cette méthode, les informations d’identification sont un ID client d’application et un secret d’application.

Pour autoriser le pilote à utiliser des clés de migration certifiables stockés dans AKV pour le chiffrement de colonne, utilisez les mots clés de chaîne de connexion uniquement suivants :

|Type d'informations d'identification| `KeyStoreAuthentication` |`KeyStorePrincipalId`| `KeyStoreSecret` |
|-|-|-|-|
|Nom d’utilisateur/mot de passe| `KeyVaultPassword`|Nom d’utilisateur Principal|Mot de passe|
|ID de client/secret| `KeyVaultClientSecret`|ID client|Clé secrète|

#### <a name="example-connection-strings"></a>Exemples de chaînes de connexion

Les chaînes de connexion suivantes montrent comment s’authentifier auprès d’Azure Key Vault avec les deux types d’informations d’identification :

**ClientID/Secret**:

```
DRIVER=ODBC Driver 13 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<clientId>;KeyStoreSecret=<secret>
```

**Nom d’utilisateur/mot de passe**

```
DRIVER=ODBC Driver 13 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultPassword;KeyStorePrincipalId=<username>;KeyStoreSecret=<password>
```

Aucune autre modification des applications ODBC ne doivent utiliser AKV pour le stockage de clés CMK.

### <a name="using-the-windows-certificate-store-provider"></a>L’utilisation du fournisseur de magasin de certificats Windows

Le pilote ODBC pour SQL Server sur Windows inclut un fournisseur de magasins de clé principale de colonne intégré pour le magasin de certificats Windows nommé `MSSQL_CERTIFICATE_STORE`. (Ce fournisseur n’est pas disponible sur macOS ou Linux.) Avec ce fournisseur, la clé CMK est stockée localement sur l’ordinateur client et aucune configuration supplémentaire par l’application n’est nécessaire pour l’utiliser avec le pilote. Toutefois, l’application doit avoir accès au certificat et sa clé privée dans le magasin. Consultez [créer et stocker des clés des principales de colonne (Always Encrypted)](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted) pour plus d’informations.

### <a name="using-custom-keystore-providers"></a>À l’aide de fournisseurs de magasins de clés personnalisé

Le pilote ODBC pour SQL Server prend également en charge les fournisseurs de magasins de clés tiers personnalisé à l’aide de l’interface CEKeystoreProvider. Cela permet à une application charger, la requête et de configurer les fournisseurs de magasins de clés afin qu’ils peuvent être utilisés par le pilote pour accéder aux colonnes chiffrées. Les applications peuvent interagir directement avec un fournisseur de magasins de clés afin de chiffrer les clés cek pour le stockage dans SQL Server et effectuer des tâches au-delà de l’accès à des colonnes chiffrées avec ODBC ; Pour plus d’informations, consultez [fournisseurs de magasins de clés personnalisés](../../connect/odbc/custom-keystore-providers.md).

Deux attributs de connexion sont utilisées pour interagir avec les fournisseurs de magasins de clés personnalisés. Celles-ci sont les suivantes :

- `SQL_COPT_SS_CEKEYSTOREPROVIDER`

- `SQL_COPT_SS_CEKEYSTOREDATA`

Le premier est utilisé pour charger et répertorier les fournisseurs de chargé du magasin de clés, alors que ce dernier autorise les communications de fournisseur de l’application. Ces attributs de connexion peuvent être utilisés à tout moment, avant ou après avoir établi une connexion, étant donné que l’interaction du fournisseur de l’application n’implique pas la communication avec SQL Server. Toutefois, étant donné que le pilote n’a pas encore été chargé, paramètre et l’obtention de ces attributs avant de vous connecter entraînent à traiter par le Gestionnaire de pilotes et ne peuvent pas générer les résultats attendus.

#### <a name="loading-a-keystore-provider"></a>Chargement d’un fournisseur de magasin de clés

Définition de la `SQL_COPT_SS_CEKEYSTOREPROVIDER` attribut de connexion permet à une application cliente pour charger une bibliothèque de fournisseur mise à disposition pour utiliser les fournisseurs de magasins de clés qui s’y trouvent.

```
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```
| Argument |  Description |
|:---|:---|
|`ConnectionHandle`|[Entrée] Handle de connexion. Doit être un handle de connexion valide, mais les fournisseurs chargés via un handle d’une seule connexion sont accessibles à partir de n’importe quel autre dans le même processus.|
|`Attribute`|[Entrée] Attribut à définir : le `SQL_COPT_SS_CEKEYSTOREPROVIDER` constante.|
|`ValuePtr`|[Entrée] Pointeur vers une chaîne de caractères terminée par null spécifiant le nom de fichier de la bibliothèque du fournisseur. Pour SQLSetConnectAttrA, il s’agit d’une chaîne (codée) ANSI. Pour SQLSetConnectAttrW, il s’agit d’une chaîne Unicode (wchar_t).|
|`StringLength`|[Entrée] La longueur de la chaîne de ValuePtr ou SQL_NTS.|

Le pilote tente de charger la bibliothèque identifiée par le paramètre ValuePtr à l’aide de la bibliothèque dynamique définie par la plateforme, mécanisme de chargement (`dlopen()` sur Linux et macOS, `LoadLibrary()` sur Windows), et ajoute tous les fournisseurs qui y sont définis à la liste des fournisseurs connus pour le pilote. Les erreurs suivantes peuvent se produire :

| Erreur |  Description |
|:--|:--|
|`CE203`|La bibliothèque dynamique n’a pas pu être chargée.|
|`CE203`|Le symbole « CEKeyStoreProvider » exporté est introuvable dans la bibliothèque.|
|`CE203`|Un ou plusieurs fournisseurs dans la bibliothèque sont déjà chargés.|

`SQLSetConnectAttr` Renvoie l’erreur habituel ou valeurs de succès et des informations supplémentaires est disponible pour toutes les erreurs qui se sont produits par le biais du mécanisme de diagnostic ODBC standard.

> [!NOTE]
> Le programmeur de l’application doit garantir que des fournisseurs personnalisés sont chargés avant toute requête qui en a besoin est envoyé via une connexion. Cela entraîne l’erreur :

| Erreur |  Description |
|:--|:--|
|`CE200`|Fournisseur de magasin de clés %1 est introuvable. Assurez-vous que la bibliothèque de fournisseur de magasin de clés approprié a été chargée.|

> [!NOTE]
> Les implémenteurs de fournisseur de magasins de clés doivent éviter l’utilisation de `MSSQL` le nom de leurs fournisseurs personnalisés. Ce terme est réservé exclusivement à des fins de Microsoft et peut entraîner des conflits avec les fournisseurs intégrés futures. L’utilisation de ce terme dans le nom d’un fournisseur personnalisé peut entraîner un avertissement de ODBC.

#### <a name="getting-the-list-of-loaded-providers"></a>Obtention de la liste des fournisseurs chargés

Mise en route de cet attribut de connexion permet à une application cliente déterminer les fournisseurs de magasins de clés actuellement chargés dans le pilote (y compris celles intégrées.) Cela ne peut être effectuée après la connexion.

```
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```
| Argument |  Description |
|:---|:---|
|`ConnectionHandle`|[Entrée] Handle de connexion. Doit être un handle de connexion valide, mais les fournisseurs chargés via un handle d’une seule connexion sont accessibles à partir de n’importe quel autre dans le même processus.|
|`Attribute`|[Entrée] Attribut à récupérer : le `SQL_COPT_SS_CEKEYSTOREPROVIDER` constante.|
|`ValuePtr`|[Sortie] Pointeur vers la mémoire dans lequel retourner le nom du fournisseur chargé suivant.|
|`BufferLength`|[Entrée] La longueur de la mémoire tampon ValuePtr.|
|`StringLengthPtr`|[Sortie] Un pointeur vers une mémoire tampon dans lequel retourner le nombre total d’octets (sans le caractère de fin de la valeur null) disponibles à renvoyer dans \*ValuePtr. Si ValuePtr est un pointeur null, aucune longueur n’est retournée. Si la valeur d’attribut est une chaîne de caractères et le nombre d’octets à retourner est supérieur à BufferLength moins la longueur de l’arrêt de valeur null, les données caractères dans \*ValuePtr est tronqué à BufferLength moins la longueur de la caractère de fin de la valeur null et se termine par null par le pilote.|

Pour permettre la récupération de l’intégralité de la liste, chaque opération Get retourne le nom du fournisseur de l’actif et incrémente un compteur interne à la suivante. Une fois ce compteur atteint la fin de la liste, une chaîne vide (" ») est retournée, et le compteur est réinitialisé ; les opérations Get successives alors à nouveau à partir du début de la liste.

### <a name="communicating-with-keystore-providers"></a>Communication avec les fournisseurs de magasins de clés

Le `SQL_COPT_SS_CEKEYSTOREDATA` attribut de connexion permet à une application cliente communiquer avec les fournisseurs de magasins de clés chargé pour la configuration des paramètres supplémentaires, etc. d’incrustation. La communication entre une application cliente et un fournisseur suit un protocole de demande-réponse simple, basé sur Get et Set des demandes à l’aide de cet attribut de connexion. La communication est établie uniquement par l’application cliente.

> [!NOTE]
> En raison de la nature de ODBC appelle répondre de CEKeyStoreProvider à (SQLGet/SetConnectAttr), le ODBC interface prend en charge uniquement la définition de données dans la résolution du contexte de connexion.

L’application communique avec les fournisseurs de magasins de clés via le pilote via la structure CEKeystoreData :

```
typedef struct CEKeystoreData {
wchar_t *name;
unsigned int dataSize;
char data[];
} CEKEYSTOREDATA;
```
| Argument |  Description |
|:---|:---|
|`name`|[Entrée] Fonction Set, le nom du fournisseur pour lequel les données sont envoyées. Ignoré sur Get. Chaîne se terminant par null, les caractères larges.|
|`dataSize`|[Entrée] La taille du tableau de données conformément à la structure.|
|`data`|[InOut] Lors de l’ensemble, les données à envoyer au fournisseur. Il peut s’agir de données arbitraires ; le pilote effectue aucune tentative pour l’interpréter. Fonction Get, la mémoire tampon pour recevoir les données lues à partir du fournisseur.|

#### <a name="writing-data-to-a-provider"></a>L’écriture de données à un fournisseur

A `SQLSetConnectAttr` appeler à l’aide de la `SQL_COPT_SS_CEKEYSTOREDATA` attribut écrit un « paquet » de données dans le fournisseur de magasins de clés.
```
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```
| Argument |  Description |
|:---|:---|
|`ConnectionHandle`| [Entrée] Handle de connexion. Doit être un handle de connexion valide, mais les fournisseurs chargés via un handle d’une seule connexion sont accessibles à partir de n’importe quel autre dans le même processus.|
|`Attribute`|[Entrée] Attribut à définir : le `SQL_COPT_SS_CEKEYSTOREDATA` constante.|
|`ValuePtr`|[Entrée] Pointeur vers une structure CEKeystoreData. Le champ nom de la structure identifie le fournisseur de données auquel sont destinées.|
|`StringLength`|[Entrée] Constante SQL_IS_POINTER|

Informations d’erreur détaillées supplémentaires peuvent être obtenues [SQLGetDiacRec](https://msdn.microsoft.com/library/ms710921(v=vs.85).aspx).

> [!NOTE]
> Le fournisseur peut utiliser le handle de connexion pour associer les données écrites pour une connexion spécifique, s’il le souhaite. Cela est utile pour implémenter la configuration de chaque connexion. Il peut également ignorer le contexte de connexion et de traiter les données de la même manière, quelle que soit la connexion utilisée pour envoyer les données. Consultez [Association de contexte](../../connect/odbc/custom-keystore-providers.md#context-association) pour plus d’informations.

#### <a name="reading-data-from-a-provider"></a>La lecture des données à partir d’un fournisseur

Un appel à `SQLGetConnectAttr` à l’aide de la `SQL_COPT_SS_CEKEYSTOREDATA` attribut lit un « paquet » de données à partir de *le dernier-écrites-to* fournisseur. S’il y a aucune, une erreur de séquence de fonction se produit. Les implémenteurs de fournisseur de magasins de clés sont encouragés à prendre en charge le « factice écrit « de 0 octet comme un moyen de la sélection du fournisseur pour les opérations de lecture sans causer d’effets secondaires, s’il convient de le faire.

```
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```
| Argument |  Description |
|:---|:---|
|`ConnectionHandle`|[Entrée] Handle de connexion. Doit être un handle de connexion valide, mais les fournisseurs chargés via un handle d’une seule connexion sont accessibles à partir de n’importe quel autre dans le même processus.|
|`Attribute`|[Entrée] Attribut à récupérer : le `SQL_COPT_SS_CEKEYSTOREDATA` constante.|
|`ValuePtr`|[Sortie] Un pointeur vers une structure CEKeystoreData dans lequel les données lues à partir du fournisseur est placé.|
|`BufferLength`|[Entrée] Constante SQL_IS_POINTER|
|`StringLengthPtr`|[Sortie] Pointeur vers une mémoire tampon dans lequel retourner BufferLength. Si * ValuePtr est un pointeur null, aucune longueur n’est retournée.|

L’appelant doit s’assurer qu’une mémoire tampon de longueur suffisante, conformément à la structure CEKEYSTOREDATA est allouée pour le fournisseur écrire dans. Au retour, son champ dataSize est mis à jour avec la longueur réelle des données lues à partir du fournisseur. Informations d’erreur détaillées supplémentaires peuvent être obtenues [SQLGetDiacRec](https://msdn.microsoft.com/library/ms710921(v=vs.85).aspx).

Cette interface n’impose aucune exigence supplémentaire sur le format des données transférées entre une application et un fournisseur de magasin de clés. Chaque fournisseur peut définir son propre protocole/format, en fonction de ses besoins.

Pour obtenir un exemple d’implémentation de votre propre fournisseur de magasins de clés, consultez [fournisseurs de magasins de clés personnalisés](../../connect/odbc/custom-keystore-providers.md)

## <a name="limitations-of-the-odbc-driver-when-using-always-encrypted"></a>Limitations du pilote ODBC lors de l’utilisation de Always Encrypted

### <a name="asynchronous-operations"></a>Opérations asynchrones
Bien que le pilote ODBC autorise l’utilisation de [opérations asynchrones](../../relational-databases/native-client/odbc/creating-a-driver-application-asynchronous-mode-and-sqlcancel.md) avec Always Encrypted, a un impact sur les performances sur les opérations de chiffrement intégral est activé. L’appel à `sys.sp_describe_parameter_encryption` pour déterminer les métadonnées de chiffrement pour l’instruction bloque et entraîne le pilote à attendre que le serveur retourner les métadonnées avant de retourner `SQL_STILL_EXECUTING`.

### <a name="retrieve-data-in-parts-with-sqlgetdata"></a>Récupérer des données dans des parties avec SQLGetData
Avant le 17 du pilote ODBC pour SQL Server, cryptage caractère et les colonnes de type binary ne sont pas accessibles dans des parties avec SQLGetData. Un seul appel de SQLGetData peut être effectué, avec une mémoire tampon de longueur suffisante pour contenir les données de la colonne entière.

### <a name="send-data-in-parts-with-sqlputdata"></a>Envoyer des données dans des parties avec SQLPutData
Impossible d’envoyer les données de comparaison ou insertion dans des parties avec SQLPutData. Un seul appel à SQLPutData peut être effectué, avec une mémoire tampon contenant la totalité des données. Pour insérer des données de type long dans les colonnes chiffrées, utilisez l’API de copie en bloc, décrit dans la section suivante, avec un fichier de données d’entrée.

### <a name="encrypted-money-and-smallmoney"></a>Smallmoney et money chiffrée
Chiffré **money** ou **smallmoney** colonnes ne peut pas être ciblés par les paramètres, car il n’existe aucun spécifique qui correspond à ces types, ce qui entraîne des erreurs de conflit de Type opérande de type de données ODBC.

## <a name="bulk-copy-of-encrypted-columns"></a>Copie en bloc des colonnes chiffrées

Utilisation de la [des fonctions de copie en bloc SQL](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md) et le **bcp** utilitaire est pris en charge avec Always Encrypted depuis le 17 du pilote ODBC pour SQL Server. Texte en clair (insertion chiffrée sur et récupération déchiffrée sur) et texte chiffré (transféré textuellement) peuvent être insérés et récupérés à l’aide de la copie en bloc (bcp_ *) API et la **bcp** utilitaire.

- Pour récupérer du texte chiffré sous forme de varbinary (max) (par exemple, pour le chargement en masse dans une autre base de données), de se connecter sans le `ColumnEncryption` option (ou la valeur `Disabled`) et effectuer une opération BCP OUT.

- Pour insérer, extraire en texte clair et permettent d’effectuer en toute transparence le chiffrement et le déchiffrement en tant que paramètre requis, le pilote `ColumnEncryption` à `Enabled` est suffisante. Les fonctionnalités de l’API BCP sont inchangée.

- Pour insérer du texte chiffré sous forme de varbinary (max) (par exemple, tel que récupéré ci-dessus), définissez la `BCPMODIFYENCRYPTED` option sur TRUE et effectuer une opération BCP IN. Dans l’ordre pour les données résultantes être decryptable, assurez-vous que la destination clé de la colonne est la même que celle à partir de laquelle le texte chiffré obtenu à l’origine.

Lorsque vous utilisez la **bcp** utilitaire : au contrôle le `ColumnEncryption` paramètre, utilisez l’option -D et spécifier une source de données qui contient la valeur souhaitée. Pour insérer du texte chiffré, vérifiez le `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` de l’utilisateur est activée.

Le tableau suivant fournit un résumé des actions lorsqu’il fonctionne sur une colonne chiffrée :

|`ColumnEncryption`|Direction BCP| Description|
|----------------|-------------|-----------|
|`Disabled`|OUT (au client)|Récupère le texte chiffré. Le type de données observée est **varbinary (max)**.|
|`Enabled`|OUT (au client)|Récupère le texte en clair. Le pilote de déchiffrer les données de la colonne.|
|`Disabled`|(Pour le serveur)|Insère du texte chiffré. Cela est destiné opaque déplacement des données chiffrées sans l’exiger pour être déchiffrée. L’opération échoue si le `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` option n’est pas définie sur l’utilisateur ou BCPMODIFYENCRYPTED n’est pas défini sur le handle de connexion. Pour plus d’informations, voir ci-dessous.|
|`Enabled`|(Pour le serveur)|Insère le texte en clair. Le pilote chiffre les données de colonne.|

### <a name="the-bcpmodifyencrypted-option"></a>L’option BCPMODIFYENCRYPTED

Pour éviter une altération des données, le serveur normalement n’autorise pas l’insertion de texte chiffré directement dans une colonne chiffrée, et ainsi faire tenteront ; Toutefois, pour le chargement en masse des données chiffrées à l’aide de l’API BCP, en définissant le `BCPMODIFYENCRYPTED` [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) option true permettra de texte chiffré à insérer directement et réduit le risque d’endommager les données chiffrées sur un paramètre de la `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` option sur le compte d’utilisateur. Toutefois, les clés doivent correspondre les données et il est judicieux d’effectuer des vérifications en lecture seule des données insérées après l’insertion en bloc et avant leur utilisation.

Consultez [migrer des données sensibles protégées par Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md) pour plus d’informations.

## <a name="always-encrypted-api-summary"></a>Résumé des API de chiffrement intégral

### <a name="connection-string-keywords"></a>Mots clés de chaîne de connexion

|Nom| Description|  
|----------|-----------------|  
|`ColumnEncryption`|Valeurs acceptées sont `Enabled` / `Disabled`.<br>`Enabled` --Active la fonctionnalité de chiffrement intégral pour la connexion.<br>`Disabled` --désactiver la fonctionnalité Always Encrypted pour la connexion. <br><br>La valeur par défaut est `Disabled`.|  
|`KeyStoreAuthentication` | Valeurs valides : `KeyVaultPassword`, `KeyVaultClientSecret` |
|`KeyStorePrincipalId` | Lorsque `KeyStoreAuthentication`  =  `KeyVaultPassword`, définissez cette valeur sur un nom Principal d’utilisateur Active Directory de Azure valide. <br>Lorsque `KeyStoreAuthetication`  =  `KeyVaultClientSecret` cette valeur pour l’ID Client d’Application Active Directory Azure valide |
|`KeyStoreSecret` | Lorsque `KeyStoreAuthentication`  =  `KeyVaultPassword` cette valeur au mot de passe pour le nom d’utilisateur correspondant. <br>Lorsque `KeyStoreAuthentication`  =  `KeyVaultClientSecret` définir cette valeur sur la clé secrète d’Application associé avec l’ID Client d’Application Active Directory Azure valide|

### <a name="connection-attributes"></a>Attributs de connexion

|Nom|Type| Description|  
|----------|-------|----------|  
|`SQL_COPT_SS_COLUMN_ENCRYPTION`|De préconnexion|`SQL_COLUMN_ENCRYPTION_DISABLE` (0)--désactiver Always Encrypted <br>`SQL_COLUMN_ENCRYPTION_ENABLE` (1)--activer Always Encrypted|
|`SQL_COPT_SS_CEKEYSTOREPROVIDER`|Post-connexion|[Set] Tentative de chargement CEKeystoreProvider<br>[Get] Retourne un nom CEKeystoreProvider|
|`SQL_COPT_SS_CEKEYSTOREDATA`|Post-connexion|[Set] Écrire des données CEKeystoreProvider<br>[Get] Lire les données de CEKeystoreProvider|
|`SQL_COPT_SS_CEKCACHETTL`|Post-connexion|[Set] Définir la durée de vie du cache de clé CEK<br>[Get] Obtenir le cache en cours de la clé CEK TTL|
|`SQL_COPT_SS_TRUSTEDCMKPATHS`|Post-connexion|[Set] Définir le pointeur de chemins d’accès de clé CMK approuvé<br>[Get] Obtenir le pointeur de chemins d’accès de clé CMK approuvé en cours|

### <a name="statement-attributes"></a>Attributs d'instruction

|Nom| Description|  
|----------|-----------------|  
|`SQL_SOPT_SS_COLUMN_ENCRYPTION`|`SQL_CE_DISABLED` (0)--always Encrypted est désactivé pour l’instruction <br>`SQL_CE_RESULTSETONLY` (1)--uniquement le déchiffrement. Jeux de résultats et valeurs de retour sont déchiffrées et les paramètres ne sont pas chiffrés. <br>`SQL_CE_ENABLED` (3)--toujours chiffré est activé et utilisé pour les paramètres et les résultats|

### <a name="descriptor-fields"></a>Champs de descripteur

|Champ IPD|Type de la taille|Valeur par défaut| Description|
|-|-|-|-|  
|`SQL_CA_SS_FORCE_ENCRYPT` (1236)|WORD (2 octets)|0|Lorsque 0 (valeur par défaut) : la décision pour chiffrer ce paramètre est déterminée par la disponibilité des métadonnées de chiffrement.<br><br>Lorsqu’elle est différente de zéro : si les métadonnées de chiffrement sont disponible pour ce paramètre, il est chiffré. Dans le cas contraire, la demande échoue avec l’erreur [CE300] le cryptage [Microsoft] [ODBC Driver 13 for SQL Server] obligatoire a été spécifié pour un paramètre, mais aucune métadonnée de chiffrement a été fournie par le serveur.|

### <a name="bcpcontrol-options"></a>bcp_control Options

|Nom de l’option|Valeur par défaut| Description|
|-|-|-|
|`BCPMODIFYENCRYPTED` (21)|FALSE|Lorsque la valeur est TRUE, autorise les valeurs de varbinary (max) à insérer dans une colonne chiffrée. Si la valeur est FALSE, empêche l’insertion, sauf si les métadonnées correctes de type et le chiffrement sont fournie.|

## <a name="see-also"></a>Voir aussi

- [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Blog sur Always Encrypted](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

