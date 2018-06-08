---
title: Créer une étape de travail Analysis Services | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- job steps [Analysis Services]
ms.assetid: 03d4bb86-514b-4a55-97b9-c2c0fa08b428
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 359126ee294db0c10c9c738abdd195ea999f9fa1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-analysis-services-job-step"></a>Créer une étape de travail Analysis Services
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment créer et définir les étapes de travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent dans [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)] qui exécutent les commande et les requêtes [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Analysis Services à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)], de [!INCLUDE[tsql](../../includes/tsql_md.md)] ou de SQL Server Management Objects.  
  
-   **Avant de commencer :**  
  
    [Limitations et restrictions](#Restrictions)  
  
    [Sécurité](#Security)  
  
-   **Pour créer des étapes de travail SQL Server à l'aide de commande et/ou de requêtes Analysis Services, avec :**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="Restrictions"></a>Limitations et restrictions  
  
-   Si l'étape de travail utilise une commande Analysis Services, l'instruction de commande doit être une méthode **EXECUTE** XML pour Analysis Services. L’instruction ne peut pas contenir d’enveloppe SOAP (Simple Object Access Protocol) complète ni de méthode **Discover** XML pour Analysis Services. Si [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] prend en charge les enveloppes SOAP (Simple Object Access Protocol) complètes et la méthode **Discover** , ce n'est pas le cas des étapes de travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Pour plus d’informations sur XML pour Analysis Services, consultez [Vue d’ensemble de XMLA (XML for Analysis)](http://msdn.microsoft.com/library/ms187190.aspx).  
  
-   Si l'étape de travail utilise une requête Analysis Services, l'instruction de requête doit être une requête MDX (expressions multidimensionnelles). Pour plus d’informations sur MDX, consultez [Principes de base des instructions MDX (MDX)](http://msdn.microsoft.com/en-us/a560383b-bb58-472e-95f5-65d03d8ea08b).  
  
### <a name="Security"></a>Sécurité  
  
#### <a name="Permissions"></a>Permissions  
  
-   Pour exécuter une étape de travail qui utilise le sous-système Analysis Services, un utilisateur doit être membre du rôle serveur fixe **sysadmin** ou avoir accès à un compte proxy valide, défini pour utiliser ce sous-système. De plus, le compte du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent ou du proxy doit correspondre à un compte administrateur Analysis Services et à un compte de domaine Windows valide.  
  
-   Seuls les membres du rôle serveur fixe **sysadmin** peuvent écrire les données de sortie de l'étape de travail dans un fichier. Si l'étape de travail est exécutée par des utilisateurs membres du rôle de base de données **SQLAgentUserRole** de la base de données **msdb** , les données de sortie peuvent uniquement être écrites dans une table. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] écrit les données de sortie de l'étape de travail dans la table **sysjobstepslog** de la base de données **msdb** .  
  
-   Pour plus d'informations, consultez [Implement SQL Server Agent Security](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="SSMS"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-an-analysis-services-command-job-step"></a>Pour créer une étape de travail de commande Analysis Services  
  
1.  Dans **l’Explorateur d'objets** , connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]et développez-la.  
  
2.  Développez **SQL Server Agent**, créez un travail ou cliquez avec le bouton droit de la souris sur un travail existant, puis cliquez sur **Propriétés**. Pour plus d’informations sur la création d’un travail, consultez [Créer des travaux](../../ssms/agent/create-jobs.md).  
  
3.  Dans la boîte de dialogue **Propriétés du travail** , cliquez sur la page **Étapes** , puis sur **Nouveau**.  
  
4.  Dans la boîte de dialogue **Nouvelle étape du travail** , tapez le **Nom de l'étape**du travail.  
  
5.  Dans la liste **Type** , cliquez sur **Commande SQL Server Analysis Services**.  
  
6.  Dans la liste **Exécuter en tant que** , sélectionnez un proxy qui a été défini pour utiliser le sous-système de commandes Analysis Services. Un utilisateur qui est membre du rôle serveur fixe **sysadmin** peut également sélectionner **Compte du service Agent SQL** pour exécuter cette étape de travail.  
  
7.  Sélectionnez le **Serveur** sur lequel l'étape du travail s'exécutera, ou tapez le nom du serveur.  
  
8.  Dans la zone **Commande** , tapez l'instruction à exécuter ou cliquez sur **Ouvrir** pour sélectionner une instruction.  
  
9. Cliquez sur la page **Avancé** pour définir les options de cette étape de travail, comme les actions que l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] doit mener si l'étape de travail réussit ou échoue, combien de fois l'étape de travail doit être tentée, et à quel emplacement les données de sortie de l'étape de travail doivent être écrites.  
  
#### <a name="to-create-an-analysis-services-query-job-step"></a>Pour créer une étape de travail de requête Analysis Services  
  
1.  Dans **l’Explorateur d'objets** , connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]et développez-la.  
  
2.  Développez **SQL Server Agent**, créez un travail ou cliquez avec le bouton droit de la souris sur un travail existant, puis cliquez sur **Propriétés**. Pour plus d’informations sur la création d’un travail, consultez [Créer des travaux](../../ssms/agent/create-jobs.md).  
  
3.  Dans la boîte de dialogue **Propriétés du travail** , cliquez sur la page **Étapes** , puis sur **Nouveau**.  
  
4.  Dans la boîte de dialogue **Nouvelle étape du travail** , tapez un **nom d'étape**de travail.  
  
5.  Dans la liste **Type** , cliquez sur **Requête SQL Server Analysis Services**.  
  
6.  Dans la liste **Exécuter en tant que** , sélectionnez un proxy qui a été défini pour utiliser le sous-système de requêtes Analysis Services. Un utilisateur qui est membre du rôle serveur fixe **sysadmin** peut également sélectionner **Compte du service Agent SQL** pour exécuter cette étape de travail.  
  
7.  Sélectionnez le **Serveur** et la **Base de données** sur lesquels l'étape du travail s'exécutera, ou tapez le nom du serveur ou de la base de données.  
  
8.  Dans la zone **Commande** , tapez l'instruction à exécuter ou cliquez sur **Ouvrir** pour sélectionner une instruction.  
  
9. Cliquez sur la page **Avancé** pour définir les options de cette étape de travail, comme les actions que l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] doit mener si l'étape de travail réussit ou échoue, combien de fois l'étape de travail doit être tentée, et à quel emplacement les données de sortie de l'étape de travail doivent être écrites.  
  
## <a name="TSQL"></a>Utilisation de Transact-SQL  
  
#### <a name="to-create-an-analysis-services-command-job-step"></a>Pour créer une étape de travail de commande Analysis Services  
  
1.  Dans l'**Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Creates a job step that uses XMLA to create a relational data source that
    -- references the AdventureWorks2012 Microsoft SQL Server database.  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name =
            N'Create a relational data source that references the AdventureWorks2012 Microsoft SQL Server database',  
        @subsystem = N'ANALYSISCOMMAND',  
        @command =
            N' <Create xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
        <ParentObject>  
            <DatabaseID>AdventureWorks2012</DatabaseID>  
        </ParentObject>  
        <ObjectDefinition>  
            <DataSource xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:type="RelationalDataSource">  
                <ID>AdventureWorks2012</ID>  
                <Name>Adventure Works 2012</Name>  
                <ConnectionString>Data Source=localhost;Initial Catalog=AdventureWorks2012;Integrated Security=True</ConnectionString>  
                <ImpersonationInfo>  
                    <ImpersonationMode>ImpersonateServiceAccount</ImpersonationMode>  
                </ImpersonationInfo>  
                <ManagedProvider>System.Data.SqlClient</ManagedProvider>  
                <Timeout>PT0S</Timeout>  
            </DataSource>  
        </ObjectDefinition>  
    </Create>', ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_jobstep (Transact-SQL)](http://msdn.microsoft.com/en-us/97900032-523d-49d6-9865-2734fba1c755).  
  
#### <a name="to-create-an-analysis-services-query-job-step"></a>Pour créer une étape de travail de requête Analysis Services  
  
1.  Dans l'**Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Creates a job step that uses MDX to return data  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Returns the Internet sales amount by state',  
        @subsystem = N'ANALYSISQUERY',  
        @command = N' SELECT  
       [Measures].[Internet Sales Amount] ON COLUMNS,  
       [Customer].[State-Province].Members ON ROWS  
    FROM [AdventureWorks2012]',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_jobstep (Transact-SQL)](http://msdn.microsoft.com/en-us/97900032-523d-49d6-9865-2734fba1c755).  
  
## <a name="SMO"></a>Utilisation de SQL Server Management Objects  
**Pour créer une étape de travail exécutant un script PowerShell**  
  
Utilisez la classe **JobStep** à l’aide d’un langage de programmation que vous choisissez, comme XMLA ou MDX. Pour plus d’informations, consultez [SQL Server Management Objects (SMO)](http://msdn.microsoft.com/library/ms162169.aspx).  
  
