---
title: Bien démarrer avec PolyBase | Microsoft Docs
ms.custom: ''
ms.date: 08/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: database
ms.reviewer: ''
ms.suite: sql
ms.technology: polybase
ms.tgt_pltfrm: ''
ms.topic: get-started-article
helpviewer_keywords:
- PolyBase
- PolyBase, getting started
- Hadoop import
- Hadoop export
- Azure blob storage import
- Azure blob storage export
- Hadoop import, PolyBase getting started
- Hadoop export, Polybase getting started
caps.latest.revision: 78
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: cfde818a22771ba7bfa08259e3a34eff68fb1aea
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="get-started-with-polybase"></a>Prise en main de PolyBase
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique contient les principes de base relatifs à l’exécution de PolyBase sur une instance de SQL Server.
  
 Après exécution des étapes ci-dessous, vous aurez acquis ce qui suit :  
  
-   PolyBase installé et exécutable sur votre serveur  
  
-   Exemples d’instructions qui créent des objets PolyBase  
  
-   Compréhension de la manière de gérer des objets PolyBase dans SQL Server Management Studio (SSMS)  
  
-   Exemples de requêtes utilisant des objets PolyBase    

## <a name="install-polybase"></a>Installer PolyBase  
Si vous n’avez pas installé PolyBase, consultez [Installation de PolyBase](../../relational-databases/polybase/polybase-installation.md). Cet article décrit les prérequis pour l’installation.
  
### <a name="how-to-confirm-installation"></a>Comment vérifier l’installation  
 Après l’installation, exécutez la commande suivante pour vérifier que PolyBase a été installé avec succès. Si PolyBase est installé, la valeur est 1, sinon 0.  
  
```sql  
SELECT SERVERPROPERTY ('IsPolybaseInstalled') AS IsPolybaseInstalled;  
```  
  
##  <a name="supported"></a> Configurer PolyBase  
 Après l’installation, vous devez configurer SQL Server pour qu’il se connecte à votre version de Hadoop ou à Stockage Blob Azure. PolyBase prend en charge deux fournisseurs Hadoop, HDP (Hortonworks Data Platform) et CDH (Cloudera Distributed Hadoop).  Les sources de données externes prises en charge sont les suivantes :  
  
-   Hortonworks HDP 1.3 sur Linux/Windows Server  
  
-   Hortonworks HDP 2.1 - 2.6 sur Linux

-   Hortonworks HDP 2.1 - 2.3 sur Windows Server  
  
-   Cloudera CDH 4.3 sur Linux  
  
-   Cloudera CDH 5.1 – 5.5, 5.9 - 5.13 sur Linux  
  
-   Stockage Blob Azure  
 
Hadoop suit le modèle « Major.Minor.Version » pour les nouvelles versions. Toutes les versions d’une version majeure ou mineure prise en charge sont prises en charge.
 

>  [!NOTE]
> La connectivité Azure Data Lake Store est uniquement prise en charge par Azure SQL Data Warehouse.
  
### <a name="external-data-source-configuration"></a>Configuration de source de données externes  
  
1.  Exécutez [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) ‘hadoop connectivity’ et définissez une valeur appropriée. Par défaut, la connectivité hadoop est définie sur 7. Pour trouver la valeur, consultez [Configuration de la connectivité PolyBase &#40;Transact-SQL&#41;](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).  
      ```sql  
    -- Values map to various external data sources.  
    -- Example: value 7 stands for Azure blob storage and Hortonworks HDP 2.3 on Linux.  
    sp_configure @configname = 'hadoop connectivity', @configvalue = 7;   
    GO   
  
    RECONFIGURE   
    GO   
    ```  
  
2.  Vous devez redémarrer SQL Server à l’aide de **services.msc**. Le redémarrage de SQL Server redémarre ces services :  
  
    -   Service de déplacement des données SQL Server PolyBase  
  
    -   Moteur SQL Server PolyBase  
  
 ![arrêter et démarrer les services PolyBase dans services.msc](../../relational-databases/polybase/media/polybase-stop-start.png "arrêter et démarrer les services PolyBase dans services.msc")  
  
### <a name="pushdown-configuration"></a>Configuration de la poussée vers le bas  
 Pour améliorer les performances des requêtes, activez le calcul de poussée vers le bas sur un cluster Hadoop :  
  
1.  Recherchez le fichier **yarn-site.XML** dans le chemin d’installation de SQL Server. En règle générale, le chemin d’accès est le suivant :  
  
    ```  
  
    C:Program FilesMicrosoft SQL ServerMSSQL13.MSSQLSERVERMSSQLBinnPolybaseHadoopconf  
  
    ```  
  
2.  Sur l’ordinateur Hadoop, recherchez le fichier analogue dans le répertoire de configuration Hadoop. Dans le fichier, recherchez et copiez la valeur de la clé de configuration yarn.application.classpath.  
  
3.  Sur l’ordinateur SQL Server, dans le **fichier yarn.site.xml** , recherchez la propriété **yarn.application.classpath** . Collez la valeur de l’ordinateur Hadoop dans l’élément de valeur.  
  
4. Pour toutes les versions 5.X de CDH, vous devez ajouter les paramètres de configuration mapreduce.application.classpath, soit à la fin de votre fichier yarn.site.xml, soit dans le fichier mapred-site.xml. HortonWorks inclut ces configurations dans les configurations yarn.application.classpath. Consultez [Configuration PolyBase](../../relational-databases/polybase/polybase-configuration.md) pour voir des exemples.

 
## <a name="scale-out-polybase"></a>Montée en puissance parallèle (« scale-out ») de PolyBase  
 La fonctionnalité Groupe PolyBase vous permet de créer un cluster d’instances SQL Server pour traiter des jeux de données volumineux issus de sources de données externes s’apparentant à une montée en puissance parallèle (« scale-out ») pour des performances de requête optimisées.  
  
1.  Installez SQL Server avec PolyBase sur plusieurs machines.  
  
2.  Sélectionnez un serveur SQL Server comme nœud principal.  
  
3.  Ajoutez d’autres instances jouant le rôle de nœuds de calcul en exécutant [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).  
  
    ```  
    -- Enter head node details:   
    -- head node machine name, head node dms control channel port, head node sql server name  
    EXEC sp_polybase_join_group 'PQTH4A-CMP01', 16450, 'MSSQLSERVER';  
  
    ```  
  
4.  Redémarrez le service de déplacement de données PolyBase sur les nœuds de calcul.  
  
 Pour plus d’informations, consultez [Groupes de scale-out PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
  
## <a name="create-t-sql-objects"></a>Créer des objets T-SQL  
 Créez des objets en fonction de la source de données externe, Hadoop ou Azure Storage.  
  
### <a name="hadoop"></a>Hadoop  
  
```sql  
-- 1: Create a database scoped credential.  
-- Create a master key on the database. This is required to encrypt the credential secret.  
  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
  
-- 2: Create a database scoped credential  for Kerberos-secured Hadoop clusters.  
-- IDENTITY: the Kerberos user name.  
-- SECRET: the Kerberos password  
  
CREATE DATABASE SCOPED CREDENTIAL HadoopUser1   
WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
  
-- 3:  Create an external data source.  
-- LOCATION (Required) : Hadoop Name Node IP address and port.  
-- RESOURCE MANAGER LOCATION (Optional): Hadoop Resource Manager location to enable pushdown computation.  
-- CREDENTIAL (Optional):  the database scoped credential, created above.  
  
CREATE EXTERNAL DATA SOURCE MyHadoopCluster WITH (  
        TYPE = HADOOP,   
        LOCATION ='hdfs://10.xxx.xx.xxx:xxxx',   
        RESOURCE_MANAGER_LOCATION = '10.xxx.xx.xxx:xxxx',   
        CREDENTIAL = HadoopUser1        
);  
  
-- 4: Create an external file format.  
-- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).    
CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
        FORMAT_TYPE = DELIMITEDTEXT,   
        FORMAT_OPTIONS (FIELD_TERMINATOR ='|',   
                USE_TYPE_DEFAULT = TRUE)  
  
-- 5:  Create an external table pointing to data stored in Hadoop.  
-- LOCATION: path to file or directory that contains the data (relative to HDFS root).  
  
CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
        [SensorKey] int NOT NULL,   
        [CustomerKey] int NOT NULL,   
        [GeographyKey] int NULL,   
        [Speed] float NOT NULL,   
        [YearMeasured] int NOT NULL  
)  
WITH (LOCATION='/Demo/',   
        DATA_SOURCE = MyHadoopCluster,  
        FILE_FORMAT = TextFileFormat  
);  
  
-- 6:  Create statistics on an external table.   
CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
  
```  
  
### <a name="azure-blob-storage"></a>Stockage Blob Azure  
  
```sql  
--1: Create a master key on the database.  
-- Required to encrypt the credential secret.  
  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
  
-- Create a database scoped credential  for Azure blob storage.  
-- IDENTITY: any string (this is not used for authentication to Azure storage).  
-- SECRET: your Azure storage account key.  
  
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential   
WITH IDENTITY = 'user', Secret = '<azure_storage_account_key>';  
  
--2:  Create an external data source.  
-- LOCATION:  Azure account storage account name and blob container name.  
-- CREDENTIAL: The database scoped credential created above.  
  
CREATE EXTERNAL DATA SOURCE AzureStorage with (  
        TYPE = HADOOP,   
        LOCATION ='wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',  
        CREDENTIAL = AzureStorageCredential  
);  
  
--3:  Create an external file format.  
-- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).  
  
CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH (  
       FORMAT_TYPE = DELIMITEDTEXT,   
       FORMAT_OPTIONS (
         FIELD_TERMINATOR ='|',   
         USE_TYPE_DEFAULT = TRUE
       )
);
         
  
--4: Create an external table.  
-- The external table points to data stored in Azure storage.  
-- LOCATION: path to a file or directory that contains the data (relative to the blob container).  
-- To point to all files under the blob container, use LOCATION='/'   
  
CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
        [SensorKey] int NOT NULL,   
        [CustomerKey] int NOT NULL,   
        [GeographyKey] int NULL,   
        [Speed] float NOT NULL,   
        [YearMeasured] int NOT NULL  
)  
WITH (LOCATION='/Demo/',   
        DATA_SOURCE = AzureStorage,  
        FILE_FORMAT = TextFileFormat  
);  
  
--5: Create statistics on an external table.   
CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
  
```  
  
## <a name="polybase-queries"></a>Requêtes PolyBase  
 PolyBase est approprié pour trois fonctions :  
  
-   requêtes ad hoc sur des tables externes ;  
  
-   importation de données ;  
  
-   exportation de données.  
  
### <a name="query-examples"></a>Exemples de requêtes  
  
-   Requêtes ad hoc  
  
    ```sql  
    -- PolyBase Scenario 1: Ad-Hoc Query joining relational with Hadoop data   
    -- Select customers who drive faster than 35 mph: joining structured customer data stored   
    -- in SQL Server with car sensor data stored in Hadoop.  
    SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,   
            Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
    FROM Insured_Customers, CarSensor_Data  
    WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35   
    ORDER BY CarSensor_Data.Speed DESC  
    OPTION (FORCE EXTERNALPUSHDOWN);    -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
    ```  
  
-   Importation de données  
  
    ```sql  
    -- PolyBase Scenario 2: Import external data into SQL Server.  
    -- Import data for fast drivers into SQL Server to do more in-depth analysis and  
    -- leverage Columnstore technology.  
  
    SELECT DISTINCT   
            Insured_Customers.FirstName, Insured_Customers.LastName,   
            Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
    INTO Fast_Customers from Insured_Customers INNER JOIN   
    (  
            SELECT * FROM CarSensor_Data where Speed > 35   
    ) AS SensorD  
    ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
    ORDER BY YearlyIncome  
  
    CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
    ```  
  
-   Exportation de données  
  
    ```  
    -- PolyBase Scenario 3: Export data from SQL Server to Hadoop.  
  
    -- Enable INSERT into external table  
    sp_configure ‘allow polybase export’, 1;  
    reconfigure  
  
    -- Create an external table.   
    CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
            [FirstName] char(25) NOT NULL,   
            [LastName] char(25) NOT NULL,   
            [YearlyIncome] float NULL,   
            [MaritalStatus] char(1) NOT NULL  
    )  
    WITH (  
            LOCATION='/old_data/2009/customerdata',  
            DATA_SOURCE = HadoopHDP2,  
            FILE_FORMAT = TextFileFormat,  
            REJECT_TYPE = VALUE,  
            REJECT_VALUE = 0  
    );  
  
    -- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
    INSERT INTO dbo.FastCustomer2009  
    SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
    ON (T1.CustomerKey = T2.CustomerKey)  
    WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
    ```  
  
## <a name="managing-polybase-objects-in-ssms"></a>Gestion d’objets PolyBase dans SSMS  
 Dans SSMS, les tables externes sont affichées dans un dossier distinct, **Tables externes**. Les sources de données externes et les formats de fichiers externes figurent dans des sous-dossiers du dossier **Ressources externes**.  
  
 ![Objets PolyBase dans SSMS](../../relational-databases/polybase/media/polybase-management.png "Objets PolyBase dans SSMS")  
  
## <a name="troubleshooting"></a>Dépannage  
 Utilisez des vues de gestion dynamique (DMV) pour résoudre les problèmes de performances et de requêtes. Pour plus d’informations, consultez [Résolution des problèmes liés à PolyBase](../../relational-databases/polybase/polybase-troubleshooting.md).  
  
 Après une mise à niveau de SQL Server 2016 RC1 vers RC2 ou RC3, les requêtes peuvent échouer. Pour en savoir plus sur ce problème et pour obtenir une solution, consultez [Notes de publication de SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md) et recherchez « PolyBase ».  
  
## <a name="next-steps"></a>Étapes suivantes  
 Pour comprendre la fonctionnalité de scale-out, consultez [Groupes de scale-out Polybase](../../relational-databases/polybase/polybase-scale-out-groups.md).  Pour surveiller PolyBase, consultez [Résolution des problèmes liés à PolyBase](../../relational-databases/polybase/polybase-troubleshooting.md). Pour résoudre les problèmes de performances de PolyBase, consultez [Résolution des problèmes de PolyBase avec des vues de gestion dynamiques](http://msdn.microsoft.com/library/ce9078b7-a750-4f47-b23e-90b83b783d80).  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de PolyBase](../../relational-databases/polybase/polybase-guide.md)   
 [Groupes de scale-out PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md)   
 [Procédures stockées PolyBase](http://msdn.microsoft.com/library/a522b303-bd1b-410b-92d1-29c950a15ede)   
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)  
  
  
