---
title: Gérer les catégories de stratégie | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.policycategories.f1
ms.assetid: d188a819-731f-4029-98aa-780d3299a0ce
caps.latest.revision: 18
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 088f033dcc51fd3cccb2342c8932ddfe911a4af3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-policy-categories"></a>Gérer les catégories de stratégie
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment appliquer une seule ou toutes les stratégies disponibles dans une catégorie à l'ensemble de l'instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour appliquer des stratégies de catégorie à une instance SQL Server à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Lorsque vous utilisez [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], si la case **Abonnements à la base de données autorisée** n'est pas sélectionnée, la catégorie de stratégie doit être appliquée individuellement à chaque partie pertinente du serveur, telle qu'une ou plusieurs bases de données ou tables.  
  
-   Si vous spécifiez une catégorie de stratégie qui n'existe pas, une nouvelle catégorie de stratégie est créée et l'abonnement est autorisé pour toutes les bases de données lorsque vous exécutez la procédure stockée. Si vous supprimez l’abonnement autorisé pour la nouvelle catégorie, il ne s’appliquera qu’à la base de données que vous avez spécifiée en tant que *target_object*. Pour plus d’informations sur la modification d’un paramètre d’abonnement autorisé, consultez [sp_syspolicy_update_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-transact-sql.md).  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Cette procédure stockée est exécutée dans le contexte du propriétaire actuel de la procédure stockée.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-apply-category-policies-to-a-sql-server-instance"></a>Pour appliquer des stratégies de catégorie à une instance SQL Server  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez appliquer les stratégies de catégorie.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez avec le bouton droit sur **Gestion de la stratégie** et sélectionnez **Gérer les catégories**.  
  
     les informations suivantes sont disponibles dans la boîte de dialogue **Administration des catégories de stratégie** :  
  
     **Nom**  
     Nom de la catégorie de stratégie.  
  
     **Abonnements à la base de données autorisée**  
     Force toutes les bases de données sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à appliquer les stratégies de la catégorie de stratégie.  
  
4.  Activez ou désactivez toutes ou une partie des cases à cocher sous **Abonnements à la base de données autorisée** pour appliquer des catégories de stratégie à l'instance de SQL Server.  
  
5.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-apply-category-policies-to-a-sql-server-instance"></a>Pour appliquer des stratégies de catégorie à une instance SQL Server  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE msdb;  
    GO  
    -- configures the specified database to subscribe to a policy category that is named 'Table Naming Policies'.  
    EXEC dbo.sp_syspolicy_add_policy_category_subscription @target_type = N'DATABASE'  
    , @target_object = N'AdventureWorks2012'  
    , @policy_category = N'Table Naming Policies';  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sp_syspolicy_add_policy_category_subscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-subscription-transact-sql.md).  
  
  
