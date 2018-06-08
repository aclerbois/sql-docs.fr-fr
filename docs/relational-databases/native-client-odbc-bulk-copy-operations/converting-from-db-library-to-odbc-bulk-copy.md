---
title: Conversion de la DB-Library pour la copie en bloc ODBC | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-bulk-copy-operations
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- converting DB-Library to ODBC bulk copy
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC], DB-Library bulk copy
- ODBC, bulk copy operations
- DB-Library bulk copy
ms.assetid: 0bc15bdb-f19f-4537-ac6c-f249f42cf07f
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: bc8072a1bd0f0e3a097a01696c9d034e0acb33c7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="converting-from-db-library-to-odbc-bulk-copy"></a>Conversion à partir de la bibliothèque de bases de données vers une copie en bloc ODBC
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Conversion d’un programme de copie en bloc DB-Library pour ODBC est facile, car le bloc copie des fonctions prises en charge par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client sont similaires pour les fonctions de copie en bloc DB-Library, avec les exceptions suivantes :  
  
-   Les applications de bibliothèque de bases de données passent un pointeur à une structure DBPROCESS comme premier paramètre de fonctions de copie en bloc. Dans les applications ODBC, le pointeur DBPROCESS est remplacé par un handle de connexion ODBC.  
  
-   Appel d’applications DB-Library **BCP_SETL** avant de se connecter pour activer les opérations de copie en bloc sur un DBPROCESS. Les applications ODBC appellent plutôt [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) avant de se connecter pour activer les opérations en bloc sur un handle de connexion :  
  
    ```  
    SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP,  
        (void *)SQL_BCP_ON, SQL_IS_INTEGER);  
    ```  
  
-   Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne prend pas en charge les gestionnaires d’erreurs et les messages de DB-Library ; vous devez appeler **SQLGetDiagRec** pour obtenir les erreurs et messages générés par les fonctions de copie en bloc ODBC. Les versions ODBC de fonctions de copie en bloc retournent les codes de retour de copie en bloc standard de SUCCEED ou FAILED, et non les codes de retour de style ODBC, tels que SQL_SUCCESS ou SQL_ERROR.  
  
-   Les valeurs spécifiées pour la bibliothèque DB-Library [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)*varlen* paramètre sont interprétées différemment ODBC **bcp_bind *** cbData* paramètre.  
  
    |Condition indiquée|DB-Library *varlen* valeur|ODBC *cbData* valeur|  
    |-------------------------|--------------------------------|-------------------------|  
    |Valeurs NULL fournies|0|-1 (SQL_NULL_DATA)|  
    |Données de variables fournies|-1|-10 (SQL_VARLEN_DATA)|  
    |Caractère de longueur nulle ou chaîne binaire|N/A|0|  
  
     Dans DB-Library, un *varlen* valeur -1 indique que les données de longueur variable sont fournies, qui, dans ODBC *cbData* est interprété comme signifiant que seules les valeurs NULL sont fournies. Modifier n’importe quel DB-Library *varlen* spécifications de -1 en SQL_VARLEN_DATA et les *varlen* spécifications de 0 en SQL_NULL_DATA.  
  
-   La bibliothèque DB-Library  **bcp_colfmt *** file_collen* et ODBC [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md)* cbUserData * ont le même problème que le **bcp_bind *** varlen*et *cbData* paramètres indiqués ci-dessus. Modifier n’importe quel DB-Library *file_collen* spécifications de -1 en SQL_VARLEN_DATA et les *file_collen* spécifications de 0 en SQL_NULL_DATA.  
  
-   Le *iValue* paramètre de ODBC [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) fonction est un pointeur void. Dans DB-Library, *iValue* était un entier. Convertir les valeurs pour ODBC *iValue* en void *.  
  
-   Le **bcp_control** option BCPMAXERRS Spécifie le nombre de lignes individuelles peuvent avoir des erreurs avant l’échec d’une opération de copie en bloc. La valeur par défaut de BCPMAXERRS est 0 (Échec de la première erreur) dans la version de DB-Library de **bcp_control** et 10 dans la version d’ODBC. Les applications DB-Library qui dépendent de la valeur par défaut de 0 pour mettre fin à une opération de copie en bloc doivent être modifiées pour appeler ODBC **bcp_control** à BCPMAXERRS la valeur 0.  
  
-   ODBC **bcp_control** fonction prend en charge les options suivantes non prises en charge par la version de DB-Library de **bcp_control**:  
  
    -   BCPODBC  
  
         Lorsque la valeur est TRUE, spécifie que **datetime** et **smalldatetime** valeurs enregistrées au format caractère auront le préfixe de séquence d’échappement ODBC timestamp et le suffixe. Cela s'applique seulement aux opérations BCP_OUT.  
  
         Avec BCPODBC défini à FALSE, un **datetime** valeur convertie en une chaîne de caractères est sortie comme :  
  
        ```  
        1997-01-01 00:00:00.000  
        ```  
  
         Avec BCPODBC défini à TRUE, la même **datetime** valeur de sortie en tant que :  
  
        ```  
        {ts '1997-01-01 00:00:00.000' }  
        ```  
  
    -   BCPKEEPIDENTITY  
  
         Si la valeur est TRUE, spécifie que les fonctions de copie en bloc insèrent les valeurs de données fournies pour les colonnes avec des contraintes d'identité. Si cela n'est pas défini, de nouvelles valeurs d'identités sont générées pour les lignes insérées.  
  
    -   BCPHINTS  
  
         Spécifie diverses optimisations de copie en bloc. Cette option ne peut pas être utilisée sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 6.5 ou antérieure.  
  
    -   BCPFILECP  
  
         Spécifie la page de codes du fichier de copie en bloc.  
  
    -   BCPUNICODEFILE  
  
         Spécifie qu'un fichier de copie en bloc en mode caractère est un fichier Unicode.  
  
-   ODBC **bcp_colfmt** (fonction) ne prend pas en charge la *file_type* indicateur de SQLCHAR car il est en conflit avec le typedef SQLCHAR ODBC. Utilisez plutôt SQLCHARACTER pour **bcp_colfmt**.  
  
-   Dans les versions ODBC des fonctions de copie en bloc, le format pour travailler avec **datetime** et **smalldatetime** des valeurs dans les chaînes de caractères est le format ODBC aaaa-mm-jj.sss ; **smalldatetime** valeurs utilisent le format ODBC aaaa-mm-jj hh : mm :.  
  
     Les versions de DB-Library de fonctions de copie en bloc acceptent **datetime** et **smalldatetime** valeurs dans des chaînes de caractères à l’aide de plusieurs formats :  
  
    -   Le format par défaut est *mmm jj aaaa hh : mmXX* où *xx* est AM ou PM.  
  
    -   **DateTime** et **smalldatetime** chaînes dans n’importe quel format pris en charge par la bibliothèque de base de données de caractères **dbconvert** (fonction).  
  
    -   Lorsque le **utiliser les paramètres internationaux** case est activée dans la bibliothèque DB-Library **Options** onglet de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilitaire réseau du Client, les fonctions de copie en bloc DB-Library acceptent également les dates au format de date régional défini pour les paramètres régionaux du Registre de l’ordinateur client.  
  
     Les fonctions de copie en bloc DB-Library n’acceptent pas ODBC **datetime** et **smalldatetime** formats.  
  
     Si l'attribut d'instruction SQL_SOPT_SS_REGIONALIZE a la valeur SQL_RE_ON, les fonctions de copie en bloc ODBC acceptent les dates au format de date régional défini pour les paramètres régionaux du Registre de l'ordinateur client.  
  
-   Lors de la sortie **money** valeurs dans le format caractère, ODBC en bloc copie fonctions fournissent quatre chiffres de précision et aucun séparateur de virgule ; Versions de DB-Library uniquement fournissent deux chiffres de précision et incluent les séparateurs virgule.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution d’opérations de copie en bloc &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)   
 [Fonctions de copie en bloc](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
