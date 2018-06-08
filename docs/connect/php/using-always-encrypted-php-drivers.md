---
title: Utilisation du chiffrement intégral avec les pilotes PHP pour SQL Server | Documents Microsoft
ms.date: 01/08/2018
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.suite: sql
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: v-kaywon
ms.author: v-kaywon
manager: mbarwin
ms.openlocfilehash: 93b14d81411e3045d9d6f3a67ce03db281f41f68
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-always-encrypted-with-the-php-drivers-for-sql-server"></a>Utilisation du chiffrement intégral avec les pilotes PHP pour SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>S’applique à
 -   5.2 les pilotes Microsoft SQL Server pour PHP
 
## <a name="introduction"></a>Introduction

Cet article fournit des informations sur la façon de développer des applications PHP à l’aide de [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) et [des pilotes PHP pour SQL Server](../../connect/php/Microsoft-php-driver-for-sql-server.md).

Always Encrypted permet aux applications clientes de chiffrer des données sensibles et de ne jamais révéler les données ou les clés de chiffrement à SQL Server ou Azure SQL Database. Un pilote avec Always Encrypted, telles que le pilote ODBC pour SQL Server, chiffre et déchiffre les données sensibles dans l’application cliente de façon transparente. Le pilote détermine automatiquement les paramètres de requêtes qui correspondent aux colonnes de base de données sensibles (protégées avec Always Encrypted) et chiffre les valeurs de ces paramètres avant de transmettre les données à SQL Server ou Azure SQL Database. De même, il déchiffre de manière transparente les données récupérées dans les colonnes de base de données chiffrées, qui figurent dans les résultats de la requête. Pour plus d’informations, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Les pilotes PHP pour SQL Server utilisent le pilote ODBC pour SQL Server pour chiffrer les données sensibles.

## <a name="prerequisites"></a>Configuration requise

 -   Configurez Always Encrypted dans votre base de données. Cette configuration implique la mise en service des clés Always Encrypted et configurer le chiffrement pour les colonnes de la base de données sélectionnée. Si vous n’avez pas déjà une base de données dans laquelle est configuré Always Encrypted, suivez les instructions de [Prise en main d’Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). En particulier, votre base de données doit contenir les définitions de métadonnées pour une table qui contient une ou plusieurs colonnes chiffrées à l’aide de cette clé, une clé de chiffrement de colonne (CEK) et une clé principale de colonne (CMK).
 -   Assurez-vous que le pilote ODBC pour SQL Server version 17 ou une version ultérieure est installé sur votre ordinateur de développement. Pour plus d’informations, consultez [pilote ODBC pour SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md).

## <a name="enabling-always-encrypted-in-a-php-application"></a>Activer le chiffrement intégral dans une Application PHP

Le moyen le plus simple pour activer le chiffrement des paramètres ciblant les colonnes chiffrées et le déchiffrement des résultats de requête est en définissant la valeur de la `ColumnEncryption` mot clé de chaîne de connexion à `Enabled`. Voici quelques exemples de l’activation du chiffrement intégral dans les pilotes SQLSRV et PDO_SQLSRV :

SQLSRV :
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled");
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV :
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

Activation du chiffrement intégral n’est pas suffisant pour le chiffrement ou le déchiffrement réussisse. Vous devez également vous assurer que :
 -   L’application a les autorisations de base de données VIEW ANY COLUMN MASTER KEY DEFINITION et VIEW ANY COLUMN ENCRYPTION KEY DEFINITION, requises pour accéder aux métadonnées des clés Always Encrypted dans la base de données. Pour plus d’informations, consultez [autorisation de base de données](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
 -   L’application peut accéder à la clé CMK qui protège les clés cek pour les colonnes chiffrées interrogées. Cette exigence est dépend du fournisseur de magasin de clés qui stocke la clé CMK. Pour plus d’informations, consultez [utilisation des magasins de clés principales de colonne](#working-with-column-master-key-stores).

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Récupération et modification des données dans des colonnes chiffrées

Une fois que vous activez le chiffrement intégral sur une connexion, vous pouvez utiliser les API SQLSRV standard (consultez [référence d’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)) ou les API PDO_SQLSRV (consultez [référence d’API du pilote PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)) pour récupérer ou modifier des données dans les colonnes de la base de données chiffrée. Si votre application possède les autorisations de base de données requis et peut accéder à la clé principale de colonne, le pilote chiffre tous les paramètres de requête qui ciblent des colonnes chiffrées et déchiffrement les données extraites des colonnes chiffrées, se comporte de manière transparente à la application comme si les colonnes n’étaient pas chiffrés.

Si Always Encrypted n’est pas activée, les requêtes avec des paramètres qui ciblent des colonnes chiffrées échouent. Données peuvent toujours être récupérées à partir des colonnes chiffrées, tant que la requête n’a aucun paramètre ciblant des colonnes chiffrées. Toutefois, le pilote ne tente pas de n’importe quel déchiffrement et l’application reçoit les données chiffrées binaires (en tant que tableaux d’octets).

Le tableau suivant récapitule le comportement des requêtes, selon que le chiffrement intégral est activé ou non :

| Caractéristiques de requête | Always Encrypted est activé et l’application peut accéder, les clés et les métadonnées de clé | Always Encrypted est activé et l’application ne peut pas accéder aux clés ou les métadonnées de clé | Always Encrypted est désactivé | | Paramètres ciblant des colonnes chiffrées. | Les valeurs de paramètre sont chiffrées en toute transparence. | Erreur | Erreur | | La récupération des données des colonnes chiffrées, sans paramètres ciblant des colonnes chiffrées. | Résultats des colonnes chiffrées sont déchiffrées en toute transparence. L’application reçoit des valeurs de colonne en texte clair. | Erreur | Résultats des colonnes chiffrées ne sont pas déchiffrés. L’application reçoit des valeurs chiffrées en tant que tableaux d’octets. |
 
Les exemples suivants illustrent la récupération et la modification de données dans des colonnes chiffrées. Les exemples supposent une table avec le schéma suivant. Les colonnes SSN et BirthDate sont chiffrées.
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
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="data-insertion-example"></a>Exemple d’Insertion de données

Les exemples suivants montrent comment utiliser les pilotes SQLSRV et PDO_SQLSRV pour insérer une ligne dans la table patients. Notez les points suivants :
 -   L’exemple de code ne contient aucun élément spécifique au chiffrement. Le pilote détecte automatiquement et chiffre les valeurs des paramètres SSN et BirthDate qui ciblent des colonnes chiffrées. Ce mécanisme rend le chiffrement transparent pour l’application.
 -   Les valeurs insérées dans des colonnes de la base de données, y compris les colonnes chiffrées, sont passés comme paramètres liés. Alors que l’utilisation de paramètres est facultative lors de l’envoi des valeurs pour les colonnes non chiffrées (bien qu’il est fortement recommandé, car elle contribue à empêcher l’injection SQL), il est nécessaire pour les valeurs ciblant des colonnes chiffrées. Si les valeurs insérées dans les colonnes SSN ou BirthDate ont été passés en tant que littéraux incorporés dans l’instruction de requête, la requête échoue, car le pilote ne tente pas de chiffrer ou de littéraux dans les requêtes de processus. Par conséquent, le serveur les rejettera en les considérant comme incompatibles avec les colonnes chiffrées.
 -   Lors de l’insertion de valeurs à l’aide des paramètres de liaison, un type SQL qui est identique au type de données de la colonne cible ou dont la conversion en type de données de la colonne cible est pris en charge doit être passé à la base de données. Cette exigence est car Always Encrypted prend en charge les peu de conversions de type (pour plus d’informations, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)). Les deux pilotes PHP, SQLSRV et PDO_SQLSRV, chacune a un mécanisme permettant de déterminer le type SQL de la valeur de l’utilisateur. Par conséquent, il ne dispose pas à en fournir explicitement le type SQL.
  -   Pour le pilote SQLSRV, l’utilisateur a deux options :
   -   S’appuient sur le pilote PHP pour déterminer et définir le type SQL approprié. Dans ce cas, l’utilisateur doit utiliser `sqlsrv_prepare` et `sqlsrv_execute` pour exécuter une requête paramétrable.
   -   Définir le type SQL explicitement.
  -   Pour le pilote PDO_SQLSRV, l’utilisateur ne dispose pas de l’option pour définir explicitement le type SQL d’un paramètre. Le pilote PDO_SQLSRV aide automatiquement à l’utilisateur de déterminer le type SQL lors de la liaison d’un paramètre.
 -   Pour les pilotes déterminer le type SQL, certaines restrictions s’appliquent :
  -   Pilote SQLSRV :
   -   Si l’utilisateur veut le pilote pour déterminer les types SQL pour les colonnes chiffrées, l’utilisateur doit utiliser `sqlsrv_prepare` et `sqlsrv_execute`.
   -   Si `sqlsrv_query` est par défaut, l’utilisateur est chargé pour spécifier les types SQL pour tous les paramètres. Le type SQL spécifié doit inclure la longueur de chaîne pour les types de chaîne et la mise à l’échelle et la précision pour les types décimaux.
  -   Pilote PDO_SQLSRV :
   -   L’attribut d’instruction `PDO::SQLSRV_ATTR_DIRECT_QUERY` n’est pas pris en charge dans une requête paramétrable.
   -   L’attribut d’instruction `PDO::ATTR_EMULATE_PREPARES` n’est pas pris en charge dans une requête paramétrable.
   
Pilote SQLSRV et [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel;
$birthDate = "1996-10-19";
$params = array($ssn, $firstName, $lastName, $birthDate);
// during sqlsrv_prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = sqlsrv_prepare($conn, $query, $params);
sqlsrv_execute($stmt);
```

Pilote SQLSRV et [sqlsrv_query](../../connect/php/sqlsrv-query.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel";
$birthDate = "1996-10-19";
// need to provide the SQL types for ALL parameters  
// note the SQL types (including the string length) have to be the same at the column definition
$params = array(array(&$ssn, null, null, SQLSRV_SQLTYPE_CHAR(11)),
                array(&$firstName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$lastName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$birthDate, null, null, SQLSRV_SQLTYPE_DATE));
sqlsrv_query($conn, $query, $params);
```

Pilote PDO_SQLSRV et [PDO::prepare](../../connect/php/pdo-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Able";
$birthDate = "1996-10-19";
// during PDO::prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
$stmt->bindParam(2, $firstName);
$stmt->bindParam(3, $lastName);
$stmt->bindParam(4, $birthDate);
$stmt->execute();
```

### <a name="plaintext-data-retrieval-example"></a>Exemple d’extraction de données en texte clair

Les exemples suivants montrent le filtrage des données en fonction de valeurs chiffrées et la récupération des données en texte clair à partir des colonnes chiffrées avec le pilote SQLSRV et PDO_SQLSRV. Notez les points suivants :
 -   La valeur utilisée dans la clause WHERE pour filtrer sur la colonne SSN doit être transmis à l’aide du paramètre de liaison, afin que le pilote peut chiffrer de manière transparente avant de l’envoyer au serveur.
 -   Lorsque vous exécutez une requête avec des paramètres liés, les pilotes PHP détermine automatiquement le type SQL pour l’utilisateur, sauf si l’utilisateur spécifie explicitement le type SQL lors de l’utilisation du pilote SQLSRV.
 -   Toutes les valeurs sont imprimées par le programme sont en texte clair, étant donné que le pilote déchiffre de manière transparente les données récupérées à partir des colonnes SSN et BirthDate.
 
Remarque : Les requêtes peuvent effectuer des comparaisons d’égalité sur les colonnes chiffrées uniquement si le chiffrement est déterministe. Pour plus d’informations, consultez [chiffrement en sélectionnant déterministe ou aléatoire](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

SQLSRV :
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = sqlsrv_prepare($conn, $query, array(&$ssn));
// during sqlsrv_execute, the driver encrypts the ssn value and passes it to the database
sqlsrv_execute($stmt);
// fetch like usual
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV :
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
// during PDOStatement::execute, the driver encrypts the ssn value and passes it to the database
$stmt->execute();
// fetch like usual
$row = $stmt->fetch();
```

### <a name="ciphertext-data-retrieval-example"></a>Exemple d’extraction de données de texte chiffré

Si Always Encrypted n’est pas activé, une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne ciblent des colonnes chiffrées.

Les exemples suivants illustrent la récupération des données chiffrées binaires à partir des colonnes chiffrées avec le pilote SQLSRV et PDO_SQLSRV. Notez les points suivants :
 -   Comme toujours chiffré n’est pas activé dans la chaîne de connexion, la requête retourne des valeurs SSN et BirthDate chiffrées en tant que tableaux d’octets (le programme convertit les valeurs en chaînes).
 -   Une requête qui récupère des données à partir de colonnes chiffrées lorsqu’Always Encrypted est désactivé peut avoir des paramètres, tant qu’aucun d’eux ne cible une colonne chiffrée. Les filtres de requête suivants par nom, qui n’est pas chiffrée dans la base de données. Si la requête filtre par SSN ou BirthDate, la requête échoue.
 
SQLSRV :
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = sqlsrv_prepare($conn, $query, array(&$lastName));
sqlsrv_execute($stmt);
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV :
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $lastName);
$stmt->execute();
$row = $stmt->fetch();
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Éviter les problèmes courants lors de l’interrogation de colonnes chiffrées

Cette section décrit les catégories d’erreurs courantes lors de l’interrogation des colonnes chiffrées à partir des applications PHP et quelques instructions sur la façon de les éviter.

#### <a name="unsupported-data-type-conversion-errors"></a>Erreurs liées à la conversion de types de données non pris en charge

Always Encrypted ne prend en charge que peu de conversions de types de données chiffrées. Consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) pour la liste détaillée des conversions de type pris en charge. Procédez comme suit pour éviter les erreurs de conversion de type de données :
 -   Lorsque vous utilisez le pilote SQLSRV avec `sqlsrv_prepare` et `sqlsrv_execute` le type SQL, ainsi que la taille de colonne et le nombre de décimales du paramètre est déterminé automatiquement.
 -   Lorsque vous utilisez le pilote PDO_SQLSRV pour exécuter une requête, le type SQL avec la taille de colonne et le nombre de décimales du paramètre dépend également automatiquement
 -   Lorsque vous utilisez le pilote SQLSRV avec `sqlsrv_query` pour exécuter une requête :
  -   Le type SQL du paramètre est soit exactement la même que le type de la colonne cible, ou la conversion du type SQL pour le type de la colonne est pris en charge.
  -   La précision et l’échelle des paramètres ciblant des colonnes de la `decimal` et `numeric` les types de données SQL Server est identique à la précision et l’échelle configurent pour la colonne cible.
  -   La précision des paramètres ciblant des colonnes de `datetime2`, `datetimeoffset`, ou `time` les types de données SQL Server n’est pas supérieure à la précision de la colonne cible, dans les requêtes qui modifient la colonne cible.
 -   N’utilisez pas les attributs d’instruction PDO_SQLSRV `PDO::SQLSRV_ATTR_DIRECT_QUERY` ou `PDO::ATTR_EMULATE_PREPARES` dans une requête paramétrable
 
#### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Erreurs dues au passage de texte en clair au lieu de valeurs chiffrées

Les valeurs qui ciblent une colonne chiffrée doivent être chiffré avant d’être envoyées au serveur. Tentative d’insertion, de modifier ou de filtrer par une valeur de texte en clair sur un génère une erreur d’une colonne chiffrée. Pour éviter ces erreurs, assurez-vous que :
 -   Always Encrypted est activé (dans la chaîne de connexion, définissez la `ColumnEncryption` mot clé à `Enabled`).
 -   Paramètre de liaison vous permet d’envoyer des données ciblant des colonnes chiffrées. L’exemple suivant montre une requête qui filtre incorrectement par une littéral ou d’une constante sur une colonne chiffrée (SSN) :
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

## <a name="controlling-performance-impact-of-always-encrypted"></a>Contrôle de l’impact d’Always Encrypted sur les performances

Always Encrypted étant une technologie de chiffrement côté client, la majeure partie de la surcharge des performances est observée côté client, pas dans la base de données. Outre le coût des opérations de chiffrement et le déchiffrement, les autres sources de problèmes de performances côté client sont :
 -   Allers-retours supplémentaires à la base de données pour récupérer des métadonnées pour les paramètres de requête.
 -   Appels au magasin de clés principales de colonne pour accéder à une clé principale de colonne.
 
### <a name="round-trips-to-retrieve-metadata-for-query-parameters"></a>Boucles pour récupérer des métadonnées pour les paramètres de requête

Si Always Encrypted est activé pour une connexion, le pilote ODBC, par défaut, appelle [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) pour chaque requête paramétrable, en passant l’instruction de requête (sans les valeurs de paramètre) à SQL Server . Cette procédure stockée analyse l’instruction de requête pour déterminer si tous les paramètres doivent être chiffrés et si tel est le cas, retourne les informations relatives au chiffrement pour chaque paramètre permettre au pilote de les chiffrer.

Étant donné que les pilotes PHP permettent à l’utilisateur lier un paramètre de type dans une instruction préparée, sans fournir de l’instruction SQL, lors de la liaison d’un paramètre dans une connexion de chiffrement intégral, les pilotes PHP appeler [SQLDescribeParam](../../odbc/reference/syntax/sqldescribeparam-function.md) sur le paramètre pour obtenir le type SQL, la taille de colonne et des chiffres décimaux. Les métadonnées sont ensuite utilisées pour appeler [SQLBindParameter]( ../../odbc/reference/syntax/sqlbindparameter-function.md). Ces supplémentaire `SQLDescribeParam` appels ne requièrent pas un aller-retour supplémentaire vers la base de données, comme le pilote ODBC a déjà stocké les informations sur le côté client lorsque `sys.sp_describe_parameter_encryption` a été appelée.

Les comportements précédents garantissent un haut niveau de transparence à l’application cliente (et le développeur d’applications) est inutile de connaître les requêtes qui accèdent à des colonnes chiffrées, tant que les valeurs ciblant des colonnes chiffrées sont passées au pilote dans paramètres.

À la différence du pilote ODBC pour SQL Server, l’activation de Always Encrypted à l’instruction/au niveau des requêtes n'est pas encore pris en charge dans les pilotes PHP. 

### <a name="column-encryption-key-caching"></a>Clé de chiffrement de colonne mise en cache

Pour réduire le nombre d’appels à un magasin de clés principales de colonne pour déchiffrer les clés de chiffrement de colonne (CEK), le pilote met en cache le clés cek clair dans la mémoire. Après avoir reçu la clé CEK chiffrée (ECEK) à partir des métadonnées de la base de données, le pilote ODBC tente d’abord de trouver la clé CEK en texte clair correspondant à la valeur de clé chiffrée dans le cache. Le pilote appelle le magasin de clés contenant la clé CMK uniquement s’il ne peut pas trouver le texte en clair correspondante CEK dans le cache.

Remarque : Dans le pilote ODBC pour SQL Server, les entrées dans le cache sont supprimées après un délai d’attente de deux heures. Ce comportement signifie que, pour un ECEK donné, le pilote contacte le magasin de clés qu’une seule fois pendant la durée de vie de l’application ou de toutes les deux heures, si elle est inférieure.

## <a name="working-with-column-master-key-stores"></a>Utilisation de magasins de clés principales de colonne

Pour chiffrer ou déchiffrer des données, le pilote doit obtenir une clé CEK qui est configurée pour la colonne cible. Clés cek est stockées dans un format chiffré (ECEKs) dans les métadonnées de la base de données. Chaque clé a une clé CMK correspondante ayant servie à chiffrer. Le [les métadonnées de base de données](../../t-sql/statements/create-column-master-key-transact-sql.md) ne stocke pas de la clé CMK lui-même ; il contient uniquement le nom du magasin de clés et des informations qui permet de localiser la clé CMK par le magasin de clés.

Pour obtenir la valeur de texte en clair d’un ECEK, le pilote obtient d’abord les métadonnées relatives à la clé CEK et sa clé CMK correspondant et puis il utilise ces informations pour contacter le magasin de clés contenant la clé CMK et des demandes pour déchiffrer le ECEK. Le pilote communique avec un magasin de clés à l’aide d’un fournisseur de magasin de clés.

Pour Microsoft Driver 5.2.0 SQL Server pour PHP, uniquement les fournisseur de magasin de certificats Windows est pris en charge. Les deux autres Keystore fournisseurs pris en charge par le pilote ODBC (Azure Key Vault et le fournisseur de magasin de clés personnalisé) ne sont pas encore pris en charge.

### <a name="using-the-windows-certificate-store-provider"></a>L’utilisation du fournisseur de magasin de certificats Windows

Le pilote ODBC pour SQL Server sur Windows inclut un fournisseur de magasins de clé principale de colonne intégré pour le magasin de certificats Windows nommé `MSSQL_CERTIFICATE_STORE`. (Ce fournisseur n’est pas disponible sur macOS ou Linux.) Avec ce fournisseur, la clé CMK est stockée localement sur l’ordinateur client et aucune configuration supplémentaire par l’application n’est nécessaire pour l’utiliser avec le pilote. Toutefois, l’application doit avoir accès au certificat et sa clé privée dans le magasin. Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="limitations-of-the-php-drivers-when-using-always-encrypted"></a>Limitations des pilotes PHP lors de l’utilisation de Always Encrypted

SQLSRV et PDO_SQLSRV :
 -   Prise en charge de Linux/MacOS
  -   Linux/MacOS ne prennent pas en charge le fournisseur de magasin de certificats Windows et qui est le seul fournisseur de magasin de clés actuellement pris en charge par les pilotes PHP
 -   Forcer le chiffrement des paramètres
 -   Activation du chiffrement intégral au niveau de l’instruction 
 
SQLSRV :
 -   À l’aide de `sqlsrv_query` pour le paramètre de liaison sans spécifier le type SQL
 -   À l’aide de `sqlsrv_prepare` pour la liaison de paramètres dans un lot d’instructions SQL  
 
PDO_SQLSRV :
 -   `PDO::SQLSRV_ATTR_DIRECT_QUERY` attribut d’instruction spécifiée dans une requête paramétrable
 -   `PDO::ATTR_EMULATE_PREPARE` attribut d’instruction spécifiée dans une requête paramétrable
 -   liaison de paramètres dans un lot d’instructions SQL
 
Les pilotes PHP héritent également les limitations imposées par le pilote ODBC pour SQL Server et la base de données. Consultez [Limitations du pilote ODBC lors de l’utilisation de Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) et [toujours chiffré détails sur les fonctionnalités](../../relational-databases/security/encryption/always-encrypted-database-engine.md#feature-details).  
  
## <a name="see-also"></a>Voir aussi  
[Guide de programmation pour le pilote SQL PHP](../../connect/php/programming-guide-for-php-sql-driver.md)
[Référence d’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
[Référence d’API du pilote PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)  
  
