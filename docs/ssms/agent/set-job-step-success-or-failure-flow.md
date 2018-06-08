---
title: Définir un flux en cas de réussite ou d’échec de l’étape de travail | Microsoft Docs
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
- SQL Server Agent jobs, action flow logic
- successful jobs [SQL Server]
- failed jobs [SQL Server]
- jobs [SQL Server Agent], action flow logic
ms.assetid: 23041ccf-8a07-41d3-85b9-c449a54b7e1e
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 82081372cc3f5567eba788676a32e554d1f9ef18
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-job-step-success-or-failure-flow"></a>Set Job Step Success or Failure Flow
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Lors de la création de travaux de l'Agent [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , vous pouvez spécifier l'action que doit effectuer [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] si un échec survient pendant l'exécution du travail. Déterminez l'action que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] doit effectuer en cas de réussite ou d'échec de chaque étape de travail. Procédez ensuite comme suit pour définir le déroulement logique des actions de l'étape de travail en utilisant l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
-   **Avant de commencer :**  
  
    [Sécurité](#Security)  
  
-   **Pour définir un flux en cas de réussite ou d'échec de l'étape de travail, utilisez :**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server Management Objects](#SMO)  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
### <a name="Security"></a>Sécurité  
Pour plus d'informations, consultez [Implement SQL Server Agent Security](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="SSMS"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>Pour définir un flux en cas de réussite ou d'échec de l'étape de travail  
  
1.  Dans l' **Explorateur d'objets**, développez **Agent SQL Server**, puis **Travaux**.  
  
2.  Cliquez avec le bouton droit sur le travail à modifier, puis cliquez sur **Propriétés**.  
  
3.  Cliquez sur la page **Étapes** , puis sur une étape et enfin sur **Modifier**.  
  
4.  Dans la boîte de dialogue **Propriétés de l'étape de travail** , cliquez sur la page **Avancé** .  
  
5.  Dans la boîte de dialogue **Action en cas de succès**, cliquez sur l’action à exécuter si l’étape de travail réussit.  
  
6.  Dans la zone **Tentatives de reprises** , saisissez le nombre de fois (entre 0 et 9999) qu'il faut répéter l'étape de travail avant de la considérer comme un échec. Si vous tapez une valeur supérieure à 0 dans la zone **Tentatives de reprises** , entrez dans la zone **Intervalle de reprise (minutes)** , le nombre de minutes d’attente, compris entre 1 et 9999, avant que le travail fasse l’objet d’un nouvel essai.  
  
7.  Dans la liste **Action en cas d'échec** , cliquez sur l'action à exécuter si l'étape de travail échoue.  
  
8.  Si le travail est un script [!INCLUDE[tsql](../../includes/tsql_md.md)] , vous pouvez choisir les options suivantes :  
  
    -   Dans la zone **Fichier de sortie** , tapez le nom du fichier de sortie dans lequel la sortie du script sera écrite. Par défaut, les données du fichier sont remplacées chaque fois que l'étape de travail s'exécute. Si vous ne voulez pas remplacer les données, activez **Ajouter la sortie au fichier existant**.  
  
    -   Activez **Enregistrer un journal dans la table** pour enregistrer l'étape de travail dans une table de base de données. Par défaut, le contenu de la table est remplacé chaque fois que l'étape de travail s'exécute. Si vous ne voulez pas remplacer les données, activez **Ajouter la sortie à l'entrée existante dans la table**. Une fois l'étape de travail exécutée, vous pouvez afficher le contenu de la table en cliquant sur **Afficher**.  
  
    -   Activez **Inclure la sortie de l'étape dans l'historique** pour inclure la sortie dans l'historique de l'étape. Le résultat ne sera affiché que s'il n'y a pas d'erreur. De même, le résultat peut être tronqué.  
  
9. Si la liste **Exécuter en tant qu'utilisateur** est disponible, sélectionnez le compte proxy ayant les informations d'identification que le travail utilisera.  
  
## <a name="TSQL"></a>Utilisation de Transact-SQL  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>Pour définir un flux en cas de réussite ou d'échec de l'étape de travail  
  
1.  Dans l'**Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @on_success_action = 1;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_jobstep (Transact-SQL)](http://msdn.microsoft.com/en-us/97900032-523d-49d6-9865-2734fba1c755).  
  
## <a name="SMO"></a>Utilisation de SQL Server Management Objects  
**Pour définir un flux en cas de réussite ou d'échec de l'étape de travail**  
  
Utilisez la classe **JobStep** à l’aide du langage de programmation de votre choix, tel que Visual Basic, Visual C# ou PowerShell. Pour plus d’informations, consultez [SQL Server Management Objects (SMO)](http://msdn.microsoft.com/library/ms162169.aspx).  
  
