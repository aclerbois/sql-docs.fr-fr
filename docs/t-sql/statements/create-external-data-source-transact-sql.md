---
title: CREATE EXTERNAL DATA SOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL DATA SOURCE
- CREATE_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, data source
- PolyBase, create data source
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 9da6613c69319197da22b9b4cee74e8ef0b289d6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-external-data-source-transact-sql"></a>CREATE EXTERNAL DATA SOURCE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  Crée une source de données externe pour PolyBase ou des requêtes de base de données élastique. Selon le scénario, la syntaxe diffère considérablement. Une source de données externe créée pour PolyBase ne peut pas être utilisée pour les requêtes de base de données élastique.  De même, une source de données externe créée pour les requêtes de base de données élastique ne peut pas être utilisée pour PolyBase, etc. 
  
> [!NOTE]  
>  PolyBase est pris en charge uniquement sur SQL Server 2016 (ou ultérieur), Azure SQL Data Warehouse et Parallel Data Warehouse. Les requêtes de base de données élastique sont prises en charge uniquement sur Azure SQL Database version 12 ou ultérieure.  
  
 Pour les scénarios de PolyBase, la source de données externe est un système de fichiers Hadoop (HDFS), un conteneur d’objets blob de stockage Azure ou Azure Data Lake Store. Pour plus d’informations, consultez [Prise en main de PolyBase](../../relational-databases/polybase/get-started-with-polybase.md).  
  
 Pour les scénarios de requête de base de données élastique, la source externe est un gestionnaire de cartes de partitions (sur Azure SQL Database) ou une base de données distante (sur Azure SQL Database).  Utilisez [sp_execute_remote &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-execute-remote-azure-sql-database.md) après avoir créé une source de données externe. Pour plus d’informations, consultez [Requête de base de données élastique](https://azure.microsoft.com/documentation/articles/sql-database-elastic-query-overview/).  

  La source de données externe du stockage Blob Azure prend en charge la syntaxe `BULK INSERT` et `OPENROWSET`, et est différente du stockage Blob Azure pour PolyBase.
    
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- PolyBase only:  Hadoop cluster as data source   
-- (on SQL Server 2016)  
CREATE EXTERNAL DATA SOURCE data_source_name  
    WITH (   
        TYPE = HADOOP,
        LOCATION = 'hdfs://NameNode_URI[:port]'  
        [, RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI[:port]' ]  
        [, CREDENTIAL = credential_name ]
    )
[;]  
  
-- PolyBase only: Azure Storage Blob as data source   
-- (on SQL Server 2016 and Azure SQL Data Warehouse)  
CREATE EXTERNAL DATA SOURCE data_source_name  
    WITH (   
        TYPE = HADOOP,  
        LOCATION = 'wasb[s]://container@account_name.blob.core.windows.net'
        [, CREDENTIAL = credential_name ]
    )  
[;]

-- PolyBase only: Azure Data Lake Store
-- (on Azure SQL Data Warehouse)
CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalake.net',
    CREDENTIAL = AzureStorageCredential
);

-- PolyBase only: Hadoop cluster as data source
-- (on Parallel Data Warehouse)
CREATE EXTERNAL DATA SOURCE data_source_name
    WITH ( 
        TYPE = HADOOP, 
        LOCATION = 'hdfs://NameNode_URI[:port]'
        [, JOB_TRACKER_LOCATION = 'JobTracker_URI[:port]' ]
    )
[;]

-- PolyBase only: Azure Storage Blob as data source 
-- (on Parallel Data Warehouse)
CREATE EXTERNAL DATA SOURCE data_source_name
    WITH ( 
        TYPE = HADOOP,
        LOCATION = 'wasb[s]://container@account_name.blob.core.windows.net'
    )
[;]
  
-- Elastic Database query only: a shard map manager as data source   
-- (only on Azure SQL Database)  
CREATE EXTERNAL DATA SOURCE data_source_name  
    WITH (   
        TYPE = SHARD_MAP_MANAGER,  
        LOCATION = '<server_name>.database.windows.net',  
        DATABASE_NAME = '\<ElasticDatabase_ShardMapManagerDb'>,  
        CREDENTIAL = <ElasticDBQueryCred>,  
        SHARD_MAP_NAME = '<ShardMapName>'  
    )  
[;]  
  
-- Elastic Database query only: a remote database on Azure SQL Database as data source   
-- (only on Azure SQL Database)  
CREATE EXTERNAL DATA SOURCE data_source_name  
    WITH (   
        TYPE = RDBMS,  
        LOCATION = '<server_name>.database.windows.net',  
        DATABASE_NAME = '<Remote_Database_Name>',  
        CREDENTIAL = <SQL_Credential>  
    )  
[;]  

-- Bulk operations only: Azure Storage Blob as data source   
-- (on SQL Server 2017 or later, and Azure SQL Database).
CREATE EXTERNAL DATA SOURCE data_source_name  
    WITH (   
        TYPE = BLOB_STORAGE,  
        LOCATION = 'https://storage_account_name.blob.core.windows.net/container_name'
        [, CREDENTIAL = credential_name ]
    )  
```  
  
## <a name="arguments"></a>Arguments  
 *data_source_name*   Spécifie le nom défini par l’utilisateur de la source de données. Le nom doit être unique au sein de la base de données dans SQL Server, Azure SQL Database et Azure SQL Data Warehouse. Le nom doit être unique au sein du serveur dans Parallel Data Warehouse.
  
 TYPE = [ HADOOP | SHARD_MAP_MANAGER | RDBMS | BLOB_STORAGE]  
 Spécifie le type de source de données. Utilisez HADOOP lorsque la source de données externe est Hadoop ou un objet blob de stockage Azure pour Hadoop. Utilisez SHARD_MAP_MANAGER quand vous créez une source de données externe pour une requête de base de données élastique qui servira au partitionnement dans Azure SQL Database. Utilisez SGBDR avec les sources de données externes pour les requêtes de base de données croisée avec une requête de base de données élastique sur Azure SQL Database.  Utilisez BLOB_STORAGE quand vous exécutez des opérations en bloc à l’aide de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) avec [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].
  
LOCATION = \<chemin_emplacement> **HADOOP**    
Pour HADOOP, spécifie l’URI (Uniform Resource Indicator) d’un cluster Hadoop.  
`LOCATION = 'hdfs:\/\/*NameNode\_URI*\[:*port*\]'`  
NameNode_URI : Nom ou adresse IP de l’ordinateur du Namenode du cluster Hadoop.  
port : Port de l’IPC du Namenode. Indiqué par le paramètre de configuration fs.default.name dans Hadoop. Si la valeur n’est pas spécifiée, 8020 est utilisé par défaut.  
Exemple : `LOCATION = 'hdfs://10.10.10.10:8020'`

Pour le stockage d’objets blob Azure avec Hadoop, spécifie l’URI pour la connexion au stockage d’objets blob Azure.  
`LOCATION = 'wasb[s]://container@account_name.blob.core.windows.net'`  
wasb[s] : Spécifie le protocole de stockage d’objets blob Azure. Le [s] est facultatif et spécifie une connexion SSL sécurisée ; les données envoyées à partir de SQL Server sont chiffrées de façon sécurisée via le protocole SSL. Nous vous recommandons vivement d’utiliser 'wasbs' au lieu de 'wasb'. Notez que l’emplacement peut utiliser asv[s] au lieu de wasb[s]. La syntaxe asv[s] est dépréciée et sera supprimée dans une prochaine version.  
conteneur : Spécifie le nom du conteneur de stockage d’objets blob Azure. Pour spécifier le conteneur racine d’un compte de stockage de domaine, utilisez le nom de domaine au lieu du nom de conteneur. Les conteneurs racines sont en lecture seule, donc les données ne peuvent pas être réécrites sur le conteneur.  
nom_compte : Nom de domaine complet (FQDN) du compte de stockage Azure.  
Exemple : `LOCATION = 'wasbs://dailylogs@myaccount.blob.core.windows.net/'`

Pour Azure Data Lake Store, l’emplacement spécifie l’URI pour la connexion à votre instance Azure Data Lake Store.



**SHARD_MAP_MANAGER**   
 Pour SHARD_MAP_MANAGER, spécifie le nom du serveur logique qui héberge le Gestionnaire de cartes de partitions dans Azure SQL Database ou une base de données SQL Server sur une machine virtuelle Azure.
 
 ```
 CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred  
WITH IDENTITY = '<username>',
SECRET = '<password>';

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
  (TYPE = SHARD_MAP_MANAGER,
  LOCATION = '<server_name>.database.windows.net',
  DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
  CREDENTIAL = ElasticDBQueryCred,
  SHARD_MAP_NAME = 'CustomerIDShardMap'
) ;
```

Pour un tutoriel détaillé, consultez [Bien démarrer avec les requêtes élastiques pour le partitionnement (partitionnement horizontal)](https://azure.microsoft.com/documentation/articles/sql-database-elastic-query-getting-started/).
  
**SGBDR**   
Pour le SGBDR, spécifie le nom du serveur logique de la base de données distante dans Azure SQL Database.  

```  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';  
  
CREATE DATABASE SCOPED CREDENTIAL SQL_Credential    
WITH IDENTITY = '<username>',  
SECRET = '<password>';  
  
CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH   
    (TYPE = RDBMS,   
    LOCATION = '<server_name>.database.windows.net',   
    DATABASE_NAME = 'Customers',   
    CREDENTIAL = SQL_Credential   
) ;   
```  
  
Pour un tutoriel détaillé sur le SGBDR, consultez [Prise en main des requêtes de bases de données croisées (partitionnement vertical)](https://azure.microsoft.com/documentation/articles/sql-database-elastic-query-getting-started-vertical/).  

**BLOB_STORAGE**   
Pour les opérations en bloc uniquement, `LOCATION` doit être valide dans l’URL vers le stockage d’objets blob Azure et le conteneur. Ne placez pas **/**, le nom du fichier ou les paramètres de signature d’accès partagé à la fin de l’URL `LOCATION`.   
Les informations d’identification utilisées doivent être créées avec `SHARED ACCESS SIGNATURE` comme identité. Pour plus d’informations sur les signatures d’accès partagé, consultez [Utilisation des signatures d’accès partagé (SAP)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1). Pour un exemple d’accès au stockage d’objets blob, consultez l’exemple F de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md). 

  
 RESOURCE_MANAGER_LOCATION = '*ResourceManager_URI*[:*port*]'  
 Spécifie l’emplacement du Gestionnaire de ressources Hadoop. S’il est spécifié, l’optimiseur de requête peut prendre une décision en fonction du coût pour prétraiter les données d’une requête PolyBase en utilisant les fonctionnalités de calcul d’Hadoop avec MapReduce. Appelée pushdown de prédicats, cette opération peut considérablement réduire le volume des données transférées entre Hadoop et SQL, et donc améliorer les performances des requêtes.  
  
 S’il n’est pas spécifié, le calcul de la poussée (pushing) dans Hadoop est désactivé pour les requêtes PolyBase.  
 
Si le port n’est pas spécifié, la valeur par défaut est déterminée à l’aide du paramètre actuel de la configuration de la « connexion à hadoop ».

|Connexion Hadoop|Port du Gestionnaire de ressources par défaut|
|-------------------|-----------------------------|
| 1|50300|
|2|50300|
|3|8021|
|4|8032|
|5|8050|
|6|8032|
|7|8050|

Pour une liste complète des distributions et des versions Hadoop prises en charge par chaque valeur de connexion, consultez [Configuration de la connectivité PolyBase (Transact-SQL)](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).
  
> [!IMPORTANT]  
>  La valeur RESOURCE_MANAGER_LOCATION est une chaîne qui n’est pas validée lorsque vous créez la source de données externe. L’entrée d’une valeur incorrecte peut entraîner des retards lors de l’accès à l’emplacement.  
  
 Exemples Hadoop :  
  
-   Hortonworks HDP 2.0, 2.1, 2.2. 2.3 sur Windows :   
    ```  
    RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI:8032'  
    ```  
  
-   Hortonworks HDP 1.3 sur Windows :   
    ```  
    RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI:50300'  
    ```  
  
-   Hortonworks HDP 2.0, 2.1, 2.2, 2.3 sur Linux :   
    ```  
    RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI:8050'  
    ```  
  
-   Hortonworks HDP 1.3 sur Linux :   
    ```  
    RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI:50300'  
    ```  
  
-   Cloudera 4.3 sur Linux :   
    ```  
    RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI:8021'  
    ```  
  
-   Cloudera 5.1 - 5.11 sur Linux :   
    ```  
    RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI:8032'  
    ```  
  
 CREDENTIAL = *credential_name*  
 Spécifie les informations d’identification limitées à la base de données servant à l’authentification auprès de la source de données externe. Pour un exemple, consultez [C. Créer une source de données externe du stockage d’objets blob Azure](../../t-sql/statements/create-external-data-source-transact-sql.md#credential). Pour créer des informations d’identification, consultez [CREATE CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-credential-transact-sql.md). Notez que CREDENTIAL n’est pas nécessaire pour les jeux de données publics qui autorisent l’accès anonyme. 
  
 DATABASE_NAME = *'QueryDatabaseName'*  
 Nom de la base de données qui fait office de Gestionnaire de cartes de partitions (pour SHARD_MAP_MANAGER) ou de base de données distante (pour le SGBDR).  
  
 SHARD_MAP_NAME = *'ShardMapName'*  
 Pour SHARD_MAP_MANAGER uniquement. Nom de la carte de partitions. Pour plus d’informations sur la création d’une carte de partitions, consultez [Bien démarrer avec la requête de base de données élastique](https://azure.microsoft.com/documentation/articles/sql-database-elastic-query-getting-started/)  
  
## <a name="polybase-specific-notes"></a>Notes spécifiques à PolyBase  
Pour une liste complète des sources de données externes prises en charge, consultez [Configuration de la connectivité PolyBase (Transact-SQL)](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

 Pour utiliser PolyBase, vous devez créer ces trois objets :  
  
-   Une source de données externe.  
  
-   Un format de fichier externe et  
  
-   Une table externe qui fait référence à la source de données externe et au format de fichier externe.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation CONTROL sur la base de données dans SQL DW, SQL Server, APS 2016 et SQL DB.

> [!IMPORTANT]  
>  Dans les versions précédentes de PDW, la création d’une source de données externe nécessitait des autorisations ALTER ANY EXTERNAL DATA SOURCE.
  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Une erreur d’exécution se produit si les sources de données Hadoop externes ne sont pas cohérentes sur la définition de RESOURCE_MANAGER_LOCATION. C’est-à-dire que vous ne pouvez pas spécifier deux sources de données externes qui font référence au même cluster Hadoop et fournir un emplacement de gestionnaire de ressources pour l’une et pas pour l’autre.  
  
 Le moteur SQL ne vérifie pas l’existence de la source de données externe lorsqu’il crée l’objet de source de données externe. Si la source de données n’existe pas pendant l’exécution de la requête, une erreur se produit.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
Pour PolyBase, la source de données externe est limitée à la base de données dans SQL Server et SQL Data Warehouse. Elle est limitée au serveur dans Parallel Data Warehouse.
  
Pour PolyBase, quand RESOURCE_MANAGER_LOCATION ou JOB_TRACKER_LOCATION est défini, l’optimiseur de requête optimise chaque requête en lançant une tâche de réduction de carte sur la source Hadoop externe et en poussant le calcul. C’est une décision totalement basée sur les coûts.  

Pour garantir la réussite des requêtes PolyBase en cas de basculement du NameNode Hadoop, envisagez d’utiliser une adresse IP virtuelle pour le NameNode du cluster Hadoop. Si vous n’utilisez pas une adresse IP virtuelle pour le NameNode Hadoop, en cas de basculement du NameNode Hadoop, vous devez utiliser l’objet ALTER EXTERNAL DATA SOURCE pour pointer sur le nouvel emplacement.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Toutes les sources de données définies sur le même emplacement de cluster Hadoop doivent utiliser le même paramètre pour RESOURCE_MANAGER_LOCATION ou JOB_TRACKER_LOCATION. S’il existe une incohérence, une erreur d’exécution se produit.  
  
 Si le cluster Hadoop est configuré avec un nom et si la source de données externe utilise l’adresse IP de l’emplacement du cluster, PolyBase doit quand même pouvoir résoudre le nom du cluster quand la source de données est utilisée. Pour résoudre le nom, vous devez faire appel à un redirecteur DNS.  
  
## <a name="locking"></a>Verrouillage  
 Prend un verrou partagé sur l’objet EXTERNAL DATA SOURCE.  
  
##  <a name="examples"></a> Exemples : SQL Server 2016  
  
### <a name="a-create-external-data-source-to-reference-hadoop"></a>A. Créer une source de données externe pour faire référence à Hadoop  
Pour créer une source de données externe pour faire référence à votre cluster Hortonworks ou Cloudera Hadoop, spécifiez le nom de l’ordinateur ou l’adresse IP du port et du Namenode Hadoop.  
  
```sql  
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://10.10.10.10:8050'
);

```  
  
### <a name="b-create-external-data-source-to-reference-hadoop-with-pushdown-enabled"></a>B. Créer une source de données externe pour faire référence à Hadoop avec pushdown activé  
Spécifiez l’option RESOURCE_MANAGER_LOCATION pour activer le pushdown de calcul sur Hadoop pour des requêtes PolyBase. Une fois activé, PolyBase utilise une décision basée sur les coûts pour déterminer si le calcul de la requête doit être poussé vers Hadoop ou si toutes les données doivent être déplacées pour traiter la requête dans SQL Server.
  
```sql  
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://10.10.10.10:8020',
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
);

```
  
###  <a name="credential"></a> C. Créer une source de données externe pour faire référence à Hadoop sécurisé par Kerberos  
Pour vérifier si le cluster Hadoop est sécurisé par Kerberos, regardez la valeur de la propriété hadoop.security.authentication dans Hadoop core-site.xml. Pour faire référence à un cluster Hadoop sécurisé par Kerberos, vous devez spécifier des informations d’identification limitées à la base de données qui contiennent votre nom d’utilisateur et votre mot de passe Kerberos. La clé principale de la base de données est utilisée pour chiffrer le secret des informations d’identification limitées à la base de données. 
  
```sql  
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';

-- Create a database scoped credential with Kerberos user name and password.
CREATE DATABASE SCOPED CREDENTIAL HadoopUser1 
WITH IDENTITY = '<hadoop_user_name>', 
SECRET = '<hadoop_password>';

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyHadoopCluster WITH (
    TYPE = HADOOP, 
    LOCATION = 'hdfs://10.10.10.10:8050', 
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050', 
    CREDENTIAL = HadoopUser1
);
```

### <a name="d-create-external-data-source-to-reference-azure-blob-storage"></a>D. Créer une source de données externe pour faire référence au stockage d’objets blob Azure
Pour créer une source de données externe permettant de faire référence à votre conteneur de stockage d’objets blob Azure, spécifiez l’URI de stockage d’objets blob Azure et des informations d’identification limitées à la base de données qui contiennent votre clé de compte de stockage Azure.

Dans cet exemple, la source de données externe est un conteneur de stockage d’objets blob Azure appelé dailylogs sous le compte de stockage Azure nommé moncompte. La source de données externe de stockage Azure sert au transfert des données uniquement et ne prend pas en charge le pushdown de prédicats.

Cet exemple montre comment créer des informations d’identification limitées à la base de données qui serviront à l’authentification auprès du stockage Azure. Spécifiez la clé du compte de stockage Azure dans le secret des informations d’identification de la base de données. Spécifiez toute chaîne de l’identité des informations d’identification limitées à la base de données, elle n’est pas utilisée pour l’authentification auprès du stockage Azure. Les informations d’identification sont ensuite utilisées dans l’instruction qui crée une source de données externe.

```
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential 
WITH IDENTITY = 'myaccount', 
SECRET = '<azure_storage_account_key>';

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage WITH (
    TYPE = BLOB_STORAGE, 
    LOCATION = 'wasbs://dailylogs@myaccount.blob.core.windows.net/',
    CREDENTIAL = AzureStorageCredential
);
```

## <a name="examples-azure-sql-database"></a>Exemples : Azure SQL Database

### <a name="e-create-a-shard-map-manager-external-data-source"></a>E. Créer une source de données externe de Gestionnaire de cartes de partitions
Pour créer une source de données externe pour faire référence à SHARD_MAP_MANAGER, spécifiez le nom du serveur logique qui héberge le Gestionnaire de cartes de partitions dans Azure SQL Database ou une base de données SQL Server sur une machine virtuelle Azure.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred  
WITH IDENTITY = '<username>',
SECRET = '<password>';

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc 
WITH (
    TYPE = SHARD_MAP_MANAGER,
    LOCATION = '<server_name>.database.windows.net',
    DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
    CREDENTIAL = ElasticDBQueryCred,
    SHARD_MAP_NAME = 'CustomerIDShardMap'
);
```

### <a name="f-create-an-rdbms-external-data-source"></a>F. Créer une source de données externe de SGBDR
Pour créer une source de données externe pour faire référence à un SGBDR, spécifie le nom du serveur logique de la base de données distante dans Azure SQL Database.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

CREATE DATABASE SCOPED CREDENTIAL SQL_Credential  
WITH IDENTITY = '<username>',
SECRET = '<password>';

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc 
WITH (
    TYPE = RDBMS, 
    LOCATION = '<server_name>.database.windows.net', 
    DATABASE_NAME = 'Customers', 
    CREDENTIAL = SQL_Credential 
);
```

## <a name="examples-azure-sql-data-warehouse"></a>Exemples : Azure SQL Data Warehouse

### <a name="g-create-external-data-source-to-reference-azure-data-lake-store"></a>G. Créer une source de données externe pour faire référence à Azure Data Lake Store
La connectivité Azure Data Lake Store est basée sur votre URI ADLS et le principal de service de votre application Azure Active directory. Vous trouverez la documentation sur la création de cette application dans [Authentification auprès de Data Lake Store à l’aide d’Azure Active Directory](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).

```sql
-- If you do not have a Master Key on your DW you will need to create one.
CREATE MASTER KEY

-- These values come from your Azure Active Directory Application used to authenticate to ADLS
CREATE DATABASE SCOPED CREDENTIAL ADLUser 
WITH IDENTITY = '<clientID>@<OAuth2.0TokenEndPoint>',
SECRET = '<KEY>' ;


CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (TYPE = HADOOP,
      LOCATION = '<ADLS URI>'
)
```



## <a name="examples-parallel-data-warehouse"></a>Exemples : Parallel Data Warehouse

### <a name="h-create-external-data-source-to-reference-hadoop-with-pushdown-enabled"></a>H. Créer une source de données externe pour faire référence à Hadoop avec pushdown activé
Spécifiez l’option JOB_TRACKER_LOCATION pour activer le calcul du pushdown sur Hadoop pour des requêtes PolyBase. Une fois activé, PolyBase utilise une décision basée sur les coûts pour déterminer si le calcul de la requête doit être poussé vers Hadoop ou si toutes les données doivent être déplacées pour traiter la requête dans SQL Server. 

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://10.10.10.10:8020',
    JOB_TRACKER_LOCATION = '10.10.10.10:8050'
);
```

### <a name="i-create-external-data-source-to-reference-azure-blob-storage"></a>I. Créer une source de données externe pour faire référence au stockage d’objets blob Azure
Pour créer une source de données externe permettant de faire référence à votre conteneur de stockage d’objets blog Azure, spécifiez l’URI de stockage d’objets blob Azure comme source de données externe LOCATION. Ajoutez votre clé de compte de stockage Azure dans le fichier PDW core-site.xml pour l’authentification.

Dans cet exemple, la source de données externe est un conteneur de stockage d’objets blob Azure appelé dailylogs sous le compte de stockage Azure nommé myaccount. La source de données externe de stockage Azure sert au transfert des données uniquement et ne prend pas en charge le pushdown de prédicats.

```sql
CREATE EXTERNAL DATA SOURCE MyAzureStorage WITH (
        TYPE = HADOOP, 
        LOCATION = 'wasbs://dailylogs@myaccount.blob.core.windows.net/'
);
```

## <a name="examples-bulk-operations"></a>Exemples : Opérations en bloc   
### <a name="j-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-blob-storage"></a>J. Créer une source de données externe pour les opérations en bloc de récupération de données dans le stockage Blob Azure   
**S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].   
Utilisez la source de données suivante pour les opérations en bloc à l’aide de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md). Les informations d’identification utilisées doivent être créées avec `SHARED ACCESS SIGNATURE` comme identité. Pour plus d’informations sur les signatures d’accès partagé, consultez [Utilisation des signatures d’accès partagé (SAP)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).   
```sql
CREATE EXTERNAL DATA SOURCE MyAzureInvoices
    WITH  (
        TYPE = BLOB_STORAGE,
        LOCATION = 'https://newinvoices.blob.core.windows.net/week3',
        CREDENTIAL = AccessAzureInvoices
    );   
```   
Pour voir une utilisation de cet exemple, consultez [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md).
  
## <a name="see-also"></a> Voir aussi
[ALTER EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/alter-external-data-source-transact-sql.md)  
[CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
[CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
[CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
[CREATE TABLE AS SELECT &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)  
[sys.external_data_sources (Transact-SQL)](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)  
  
  

