---
title: Créer une alerte utilisant un numéro d’erreur | Microsoft Docs
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
- alerts [SQL Server], creating
- SQL Server Agent, alerts
- alerts [SQL Server], error numbers
ms.assetid: 03dd7fac-5073-4f86-babd-37e45a86023c
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 870cdc5102be974aadb82bb1e669a56469090ba5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-alert-using-an-error-number"></a>Créer une alerte avec un numéro d’erreur
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment créer une alerte [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent survenant dans [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)] qui sera déclenchée quand une erreur avec un numéro spécifique se produira à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] ou de [!INCLUDE[tsql](../../includes/tsql_md.md)].  
  
**Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
    [Limitations et restrictions](#Restrictions)  
  
    [Sécurité](#Security)  
  
-   **Pour créer une alerte avec un numéro d’erreur, utilisez :**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
## <a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="Restrictions"></a>Limitations et restrictions  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] est un outil simple, fonctionnant en mode graphique, qui permet de gérer tout le système d'alerte. Son utilisation est recommandée pour configurer une infrastructure d'alertes.  
  
-   Les événements créés à l’aide de **xp_logevent** surviennent dans la base de données master. Ainsi, la procédure **xp_logevent** ne déclenche pas d’alerte sauf si la valeur de **@database_name** pour l’alerte est **'master'** ou NULL.  
  
### <a name="Security"></a>Sécurité  
  
#### <a name="Permissions"></a>Permissions  
Par défaut, seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter la procédure **sp_add_alert**.  
  
## <a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-an-alert-using-an-error-number"></a>Pour créer une alerte avec un numéro d'erreur  
  
1.  Dans l' **Explorateur d'objets** , cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez créer une alerte avec un numéro d'erreur.  
  
2.  Cliquez sur le signe plus (+) pour développer **SQL Server Agent**.  
  
3.  Cliquez avec le bouton droit sur **Alertes** , puis sélectionnez **Nouvelle alerte**.  
  
4.  Dans la boîte de dialogue **Nouvelle alerte** , dans la zone **Nom** , entrez un nom pour cette alerte.  
  
5.  Sélectionnez la case à cocher **Activer** afin d'activer l'alerte à exécuter. Par défaut, l'option **Activer** est sélectionnée.  
  
6.  Dans la liste **Type** , sélectionnez **Alerte d'événement SQL Server**.  
  
7.  Sous **Définition d'une alerte d'événement**, dans la liste **Nom de base de données** , sélectionnez une base de données pour restreindre l'alerte à une base de données spécifique.  
  
8.  Sous **Les alertes seront déclenchées selon**, cliquez sur **Numéro d'erreur**, puis tapez un numéro d'erreur valide pour l'alerte. Vous pouvez également cliquer sur **Gravité** , puis sélectionner la gravité spécifique qui déclenchera l'alerte.  
  
9. Activez la case à cocher correspondant à **Déclencher une alerte quand le message contient** afin de limiter l'alerte à une certaine séquence de caractères, puis entrez un mot clé ou une chaîne de caractères pour le **Texte du message**. Le nombre maximal de caractères autorisé est de 100.  
  
10. Cliquez sur **OK**.  
  
## <a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-create-an-alert-using-an-error-number"></a>Pour créer une alerte avec un numéro d'erreur  
  
1.  Dans l'**Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- adds an alert (Test Alert) that runs the Back up
    -- the AdventureWorks2012 Database job when fired   
    -- assumes that the message 55001 and the Back up
    -- the AdventureWorks2012 Database job already exist.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_alert  
        @name = N'Test Alert',  
        @message_id = 55001,   
       @severity = 0,   
       @notification_message = N'Error 55001 has occurred. The DB will be backed up...',   
       @job_name = N'Back up the AdventureWorks2012 Database' ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_alert (Transact-SQL)](http://msdn.microsoft.com/en-us/d9b41853-e22d-4813-a79f-57efb4511f09).  
  
