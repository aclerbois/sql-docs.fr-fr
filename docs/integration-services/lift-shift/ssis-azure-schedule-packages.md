---
title: Planifier des packages SSIS dans Azure | Microsoft Docs
ms.date: 05/09/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.component: lift-shift
ms.suite: sql
ms.custom: ''
ms.technology:
- integration-services
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4bfad00425848189d88bd780296db00ec810b37c
ms.sourcegitcommit: 0cc2cb281e467a13a76174e0d9afbdcf4ccddc29
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2018
---
# <a name="schedule-the-execution-of-an-ssis-package-in-azure"></a>Planifier l’exécution d’un package SSIS dans Azure
Vous pouvez planifier l’exécution de packages stockés dans la base de données de catalogues SSISDB sur un serveur Azure SQL Database en choisissant l’une des options de planification suivantes :
-   [Option Planifier dans SQL Server Management Studio (SSMS)](#ssms)
-   [Activité d’exécution d’un package SSIS dans Azure Data Factory](#execute)
-   [Activité de procédure stockée SQL Server Azure Data Factory](#storedproc)
-   [Travaux élastiques SQL Database](#elastic)
-   [SQL Server Agent](#agent)

## <a name="ssms"></a> Planifier un package avec SSMS

Dans SQL Server Management Studio (SSMS), vous pouvez cliquer avec le bouton droit sur un package déployé sur la base de données du catalogue SSIS, SSISDB, puis sélectionner **Planifier** pour ouvrir la boîte de dialogue **Nouvelle planification**. Pour plus d’informations, consultez [Planifier l’exécution d’un package SSIS sur Azure avec SSMS](ssis-azure-schedule-packages-ssms.md).

Cette fonctionnalité nécessite SQL Server Management Studio version 17.7 ou ultérieure. Pour obtenir la dernière version de SSMS, consultez [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="execute"></a> Planifier un package avec l’activité Exécuter le package SSIS

Pour plus d’informations sur la façon de planifier un package SSIS à l’aide de l’activité Exécuter le package SSIS dans Azure Data Factory, consultez [Exécuter un package SSIS à l’aide de l’activité SSIS dans Azure Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="storedproc"></a> Planifier un package avec l’activité Procédure stockée

Pour plus d’informations sur la façon de planifier un package SSIS à l’aide de l’activité Procédure stockée dans Azure Data Factory, consultez [Exécuter un package SSIS à l’aide de l’activité de procédure stockée dans Azure Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity).

Pour Data Factory version 1, consultez [Appeler un package SSIS à l’aide de l’activité de procédure stockée dans Azure Data Factory](https://docs.microsoft.com/azure/data-factory/v1/how-to-invoke-ssis-package-stored-procedure-activity).

## <a name="elastic"></a> Planifier un package avec des travaux élastiques SQL Database

Pour plus d’informations sur les travaux élastiques SQL Database, consultez [Gestion des bases de données cloud avec augmentation de la taille des instances](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview).

### <a name="prerequisites"></a>Conditions préalables requises

Pour pouvoir utiliser des travaux élastiques afin de planifier des packages SSIS stockés dans la base de données de catalogues SSISDB sur un serveur Azure SQL Database, vous devez effectuer les actions suivantes :

1.  Installez et configurez les tâches de base de données élastique. Pour plus d’informations, consultez [Vue d’ensemble de l’installation des tâches de base de données élastique](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-service-installation).

2. Créez des informations d’identification au niveau de la base de données pour permettre aux travaux d’envoyer des commandes à la base de données de catalogues SSIS. Pour plus d’informations, consultez [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

### <a name="create-an-elastic-job"></a>Créer un travail élastique

Créez le travail à l’aide d’un script Transact-SQL similaire au script indiqué dans l’exemple suivant :

```sql
-- Create Elastic Jobs target group 
EXEC jobs.sp_add_target_group 'TargetGroup' 

-- Add Elastic Jobs target group member 
EXEC jobs.sp_add_target_group_member @target_group_name='TargetGroup', 
    @target_type='SqlDatabase', @server_name='YourSQLDBServer.database.windows.net',
    @database_name='SSISDB' 

-- Add a job to schedule SSIS package execution
EXEC jobs.sp_add_job @job_name='ExecutePackageJob', @description='Description', 
    @schedule_interval_type='Minutes', @schedule_interval_count=60

-- Add a job step to create/start SSIS package execution using SSISDB catalog stored procedures
EXEC jobs.sp_add_jobstep @job_name='ExecutePackageJob', 
    @command=N'DECLARE @exe_id bigint 
        EXEC [SSISDB].[catalog].[create_execution]
            @folder_name=N''folderName'', @project_name=N''projectName'',
            @package_name=N''packageName'', @use32bitruntime=0,
            @runinscaleout=1, @useanyworker=1, 
            @execution_id=@exe_id OUTPUT         
        EXEC [SSISDB].[catalog].[start_execution] @exe_id, @retry_count=0', 
    @credential_name='YourDBScopedCredentials', 
    @target_group_name='TargetGroup' 

-- Enable the job schedule 
EXEC jobs.sp_update_job @job_name='ExecutePackageJob', @enabled=1, 
    @schedule_interval_type='Minutes', @schedule_interval_count=60 
```

## <a name="agent"></a> Planifier un package avec SQL Server Agent

### <a name="prerequisite---create-a-linked-server"></a>Prérequis : Créer un serveur lié

Avant de pouvoir utiliser SQL Server Agent localement pour planifier l’exécution des packages stockés sur un serveur Azure SQL Database, vous devez ajouter le serveur SQL Database à votre serveur SQL Server local en tant que serveur lié.

1.  **Configurer le serveur lié**

    ```sql
    -- Add the SSISDB database on your Azure SQL Database as a linked server to your SQL Server on premises
    EXEC sp_addlinkedserver
        @server='myLinkedServer', -- Name your linked server
        @srvproduct='',     
        @provider='sqlncli', -- Use SQL Server native client
        @datasrc='<server_name>.database.windows.net', -- Add your Azure SQL Database server endpoint
        @location=‘’,
        @provstr=‘’,
        @catalog='SSISDB'  -- Add SSISDB as the initial catalog
    ```

2.  **Configurer les informations d’identification du serveur lié**

    ```sql
    -- Add your Azure SQL DB server admin credentials
    EXEC sp_addlinkedsrvlogin
        @rmtsrvname = 'myLinkedServer’,
        @useself = 'false’,
        @rmtuser = 'myUsername', -- Add your server admin username
        @rmtpassword = 'myPassword' -- Add your server admin password
    ```

3.  **Configurer les options du serveur lié**

    ```sql
    EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;
    ```

Pour plus d’informations, consultez [Créer des serveurs liés](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md) et [Serveurs liés](../../relational-databases/linked-servers/linked-servers-database-engine.md).

### <a name="create-a-sql-server-agent-job"></a>Créer un travail de SQL Server Agent

Pour planifier un package avec SQL Server Agent localement, créez un travail avec une étape de travail qui appelle les procédures stockées du catalogue SSIS `[catalog].[create_execution]`, puis `[catalog].[start_execution]`. Pour plus d’informations, consultez [Travaux de SQL Server Agent pour les packages](../packages/sql-server-agent-jobs-for-packages.md).

1.  Dans SQL Server Management Studio, connectez-vous à la base de données SQL Server locale sur laquelle vous souhaitez créer le travail.

2.  Cliquez avec le bouton droit sur le nœud **SQL Server Agent**, sélectionnez **Nouveau**, puis **Travail** pour ouvrir la boîte de dialogue **Nouveau travail**.

3.  Dans la boîte de dialogue **Nouveau travail**, sélectionnez la page **Étapes**, puis **Nouveau** pour ouvrir la boîte de dialogue **Nouvelle étape du travail**.

4.  Dans la boîte de dialogue **Nouvelle étape du travail**, sélectionnez `SSISDB` en tant que **Base de données**.

5.  Dans le champ **Commande**, entrez un script Transact-SQL similaire au script figurant dans l’exemple suivant :

    ```sql
    -- T-SQL script to create and start SSIS package execution using SSISDB stored procedures
    DECLARE @return_value int, @exe_id bigint 

    EXEC @return_value = [YourLinkedServer].[SSISDB].[catalog].[create_execution] 
        @folder_name=N'folderName', @project_name=N'projectName', 
        @package_name=N'packageName', @use32bitruntime=0, @runincluster=1, @useanyworker=1,
        @execution_id=@exe_id OUTPUT 

    EXEC [YourLinkedServer].[SSISDB].[catalog].[set_execution_parameter_value] @exe_id,
        @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1

    EXEC [YourLinkedServer].[SSISDB].[catalog].[start_execution] @execution_id=@exe_id
    ```

6.  Finissez de configurer et de planifier le travail.

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur SQL Server Agent, consultez [Travaux de SQL Server Agent pour les packages](../packages/sql-server-agent-jobs-for-packages.md).

Pour plus d’informations sur les travaux élastiques SQL Database, consultez [Gestion des bases de données cloud avec augmentation de la taille des instances](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview).
