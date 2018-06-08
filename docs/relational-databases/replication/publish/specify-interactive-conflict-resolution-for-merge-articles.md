---
title: Spécifier la résolution interactive des conflits pour les articles de fusion | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], interactive resolvers
- interactive conflict resolution [SQL Server replication]
- articles [SQL Server replication], conflict resolution
- conflict resolution [SQL Server replication], merge replication
ms.assetid: e298dea0-b5ef-4907-a745-cfad9793653f
caps.latest.revision: 40
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 96e7a808a52e72aea320726f3e56761213d800b1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-interactive-conflict-resolution-for-merge-articles"></a>Spécifier la résolution interactive des conflits pour les articles de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment spécifier la résolution interactive des conflits pour les articles de fusion dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 La réplication[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] propose un programme de résolution interactif qui vous permet de résoudre manuellement des conflits au cours d'une synchronisation à la demande dans le Gestionnaire de synchronisation [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. Quand la résolution de conflits est activée, résolvez les conflits de façon interactive au cours de la synchronisation, à l'aide du résolveur interactif. Le résolveur interactif est disponible via le Gestionnaire de synchronisation [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. Pour plus d’informations, consultez [Synchroniser un abonnement à l’aide du Gestionnaire de synchronisation Windows &#40;Gestionnaire de synchronisation Windows&#41;](../../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
-   **Pour spécifier la résolution interactive des conflits pour les articles de fusion, à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Si une synchronisation est effectuée en dehors du Gestionnaire de synchronisation Windows (comme une synchronisation planifiée ou à la demande dans SQL Server Management Studio ou le moniteur de réplication), les conflits sont résolus automatiquement sans intervention de l'utilisateur, en utilisant l'outil de résolution des conflits par défaut spécifié pour l'article. Pour plus d’informations, consultez [Interactive Conflict Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-enable-interactive-conflict-resolution-for-an-article"></a>Pour activer la résolution de conflits interactive pour un article  
  
1.  Dans la page **Articles** de l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication - \<Publication>**, sélectionnez une table. Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
2.  Cliquez sur **Propriétés de l'article**, puis cliquez sur **Définir les propriétés de l'article de Table en surbrillance** ou sur **Définir les propriétés de tous les articles de table**.  
  
3.  Dans la page **Propriétés de l’article - \<Article>** ou **Propriétés de l’article - \<TypeArticle>**, cliquez sur l’onglet **Résolveur**.  
  
4.  Sélectionnez **Autoriser l'abonné à résoudre les conflits de manière interactive au cours de la synchronisation à la demande**.  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
6.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>**, cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
#### <a name="to-specify-that-a-subscription-should-use-interactive-conflict-resolution"></a>Pour spécifier qu'un abonnement doit utiliser la résolution de conflits interactive  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Abonné> : \<Base_de_données_abonnement>**, affectez la valeur **True** à l’option **Résoudre les conflits interactivement**. Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [View and Modify Push Subscription Properties](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md) et [View and Modify Pull Subscription Properties](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Vous pouvez spécifier par programme qu'un Abonné utilisera cette interface graphique pour résoudre les conflits dans les articles lorsqu'un abonnement par extraction à une publication de fusion est créé. Seuls les conflits dans les articles qui prennent en charge cette option seront affichés dans le programme de résolution interactif.  
  
#### <a name="to-create-a-merge-pull-subscription-that-uses-the-interactive-resolver"></a>Pour créer un abonnement de fusion par extraction qui utilise le programme de résolution interactif  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), en spécifiant **@publication**. Notez la valeur de **allow_interactive_resolver** pour chaque article du jeu de résultats pour lequel le programme de résolution interactif sera utilisé.  
  
    -   Si cette valeur est **1**, le programme de résolution interactif sera utilisé.  
  
    -   Si cette valeur est **0**, vous devez tout d'abord activer le programme de résolution interactif pour chaque article. Pour cela, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), en spécifiant **@publication**, **@article**, en affectant la valeur **allow_interactive_resolver** à **@property**la valeur **true** à **@value**.  
  
2.  Dans la base de données d'abonnement de l'Abonné, exécutez [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Pour plus d’informations, consultez [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
3.  Dans la base de données d'abonnement de l'Abonné, exécutez [sp_addmergepullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md), en spécifiant les paramètres suivants :  
  
    -   **@publisher**, **@publisher_db** (la base de données publiée) et **@publication**.  
  
    -   La valeur **true** à **@enabled_for_syncmgr**.  
  
    -   La valeur **true** à **@use_interactive_resolver**.  
  
    -   Les informations de compte de sécurité requises par l'Agent de fusion. Pour plus d’informations, consultez [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
4.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md).  
  
#### <a name="to-define-an-article-that-supports-the-interactive-resolver"></a>Pour définir un article qui prend en charge le programme de résolution interactif  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Spécifiez le nom de la publication à laquelle l'article appartient pour **@publication**, le nom de l'article pour **@article**, l'objet de base de données qui est publié pour **@source_object**la valeur **true** à **@allow_interactive_resolver**. Pour plus d'informations, voir [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et résoudre les conflits de données pour les publications de fusion &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [Interactive Conflict Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)  
  
  
