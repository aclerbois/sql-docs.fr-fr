---
title: Spécifier un programme de résolution d’articles de fusion | Microsoft Docs
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
- articles [SQL Server replication], conflict resolution
- conflict resolution [SQL Server replication], merge replication
- merge replication conflict resolution [SQL Server replication], merge article resolvers
ms.assetid: a40083b3-4f7b-4a25-a5a3-6ef67bdff440
caps.latest.revision: 39
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: b062f6dc200d2024a1bb08c93847304ef9f6a5e2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-a-merge-article-resolver"></a>Spécifier un programme de résolution d'articles de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment spécifier un programmes de résolution d'articles de fusion dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
-   **Pour spécifier un programme de résolution d'articles de fusion à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   La réplication de fusion accepte les types de programmes de résolution d'articles suivants :  
  
    -   Programme de résolution par défaut. Le comportement du programme de résolution par défaut varie selon que l'abonnement est un abonnement client ou serveur. Pour plus d’informations sur la spécification du type d’abonnement, consultez [Spécifier un type d’abonnement de fusion et une priorité pour la résolution des conflits &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/specify-a-merge-subscription-type-and-conflict-resolution-priority.md).  
  
    -   Programme de résolution personnalisé que vous avez écrit. Il peut s'agir d'un gestionnaire de logique métier (écrit en code managé) ou d'un programme de résolution COM personnalisé. Pour plus d'informations, voir [Advanced Merge Replication Conflict Detection and Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md). Si vous devez implémenter une logique personnalisée qui est exécutée pour chaque ligne répliquée, pas seulement pour les lignes en conflit, consultez [Implement a Business Logic Handler for a Merge Article](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md).  
  
    -   Programme de résolution COM standard, inclus avec [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Pour utiliser un programme de résolution autre que le programme de résolution par défaut, vous devez le copier sur l'ordinateur exécutant l'Agent de fusion et l'inscrire (si vous utilisez un gestionnaire de logique métier, il doit être également inscrit sur le serveur de publication). L'Agent de fusion s'exécute sur :  
  
    -   le serveur de distribution pour un abonnement envoyé ;  
  
    -   l'Abonné pour un abonnement extrait ;  
  
    -   le serveur [!INCLUDE[msCoName](../../../includes/msconame-md.md)] IIS (Internet Information Services) pour un abonnement extrait qui utilise la synchronisation Web.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Après l’inscription du programme de résolution, spécifiez qu’un article doit l’utiliser sous l’onglet **Résolveur** de la boîte de dialogue **Propriétés de l’article - \<Article>**, accessible dans l’Assistant Nouvelle publication et dans la boîte de dialogue **Propriétés de la publication - \<Publication>**. Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-a-resolver"></a>Pour spécifier un programme de résolution  
  
1.  Dans la page **Articles** de l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication - \<Publication>**, sélectionnez une table.  
  
2.  Cliquez sur **Propriétés de l'article**puis sur **Définir les propriétés de l'article de la table en surbrillance**.  
  
3.  Dans la page **Propriétés de l’article - \<Article>**, cliquez sur l’onglet **Résolveur**.  
  
4.  Sélectionnez **Utiliser un résolveur personnalisé (inscrit sur le serveur de distribution)** puis, dans la liste, cliquez sur le programme de résolution.  
  
5.  Si le programme de résolution nécessite des entrées (par exemple un nom de colonne), indiquez- les dans la zone de texte **Entrez les informations requises par le résolveur** .  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
7.  Répétez la procédure pour chaque article exigeant un programme de résolution.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-register-a-custom-conflict-resolver"></a>Pour inscrire un outil personnalisé de résolution des conflits  
  
1.  Si vous projetez d'inscrire votre propre outil personnalisé de résolution des conflits, créez-en un répondant à l'un des types suivants :  
  
    -   Programme de résolution s'appuyant sur le code managé, comme un gestionnaire de logique métier. Pour plus d'informations, voir [Implement a Business Logic Handler for a Merge Article](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md).  
  
    -   Programme de résolution s'appuyant sur des procédures stockées et sur COM. Pour plus d'informations, voir [Implement a Custom Conflict Resolver for a Merge Article](../../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md).  
  
2.  Pour déterminer si le programme de résolution souhaité est déjà inscrit, exécutez [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) au niveau du serveur de publication dans toute base de données. Une description du programme de résolution personnalisé est alors affichée, de même que le CLSID de chaque programme de résolution s'appuyant sur l'architecture COM inscrit sur le serveur de distribution ou des informations sur l'assembly managé pour chaque gestionnaire de logique métier inscrit sur le serveur de distribution.  
  
3.  Si le programme de résolution personnalisé souhaité n’est pas encore inscrit, exécutez [sp_registercustomresolver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md) au niveau du serveur de distribution. Spécifiez le nom du programme de résolution pour **@article_resolver**; pour un gestionnaire de logique métier, il s'agit du nom convivial de l'assembly. Pour les programmes de résolution s'appuyant sur l'architecture COM, spécifiez le CLSID de la DLL pour **@resolver_clsid**, et pour un gestionnaire de logique métier, affectez la valeur **true** à **@is_dotnet_assembly**, spécifiez le nom de l'assembly pour **@dotnet_assembly_name**et le nom complet de la classe qui remplace <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> à **@dotnet_class_name**.  
  
    > [!NOTE]  
    >  Si un assembly de gestionnaire de logique métier n'est pas déployé dans le même répertoire que l'exécutable de l'Agent de fusion, que l'application qui démarre de façon synchronisée l'Agent de fusion ou dans le GAC (Global Assembly Cache), vous devez spécifier le chemin d'accès complet avec le nom de l'assembly pour **@dotnet_assembly_name**.  
  
4.  Si le programme de résolution est un programme de résolution s'appuyant sur l'architecture COM :  
  
    -   Copiez la DLL de programme de résolution personnalisé sur le serveur de distribution pour les abonnements par émission de données ou sur l'Abonné pour les abonnements par extraction.  
  
        > [!NOTE]  
        >  Les programmes de résolution personnalisés[!INCLUDE[msCoName](../../../includes/msconame-md.md)] se trouvent dans le répertoire [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM.  
  
    -   Utilisez regsvr32.exe pour inscrire la DLL du programme de résolution personnalisé auprès du système d'exploitation. Par exemple, l'exécution de la commande suivante à partir de l'invite de commandes inscrit l'Outil de résolution des conflits d'addition [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
        ```  
        regsvr32 ssradd.dll  
        ```  
  
5.  Si le programme de résolution est un gestionnaire de logique métier, déployez l'assembly dans le même dossier que le fichier exécutable de l'Agent de fusion (replmerg.exe), dans le même dossier qu'une application qui appelle l'Agent de fusion ou dans le dossier spécifié pour le **@dotnet_assembly_name** à l'étape 3.  
  
    > [!NOTE]  
    >  L'emplacement d'installation par défaut du fichier exécutable de l'Agent de fusion est [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM.  
  
#### <a name="to-specify-a-custom-resolver-when-defining-a-merge-article"></a>Pour spécifier un programme de résolution personnalisé lors de la définition d'un article de fusion  
  
1.  Si vous projetez d'utiliser un outil personnalisé de résolution des conflits, créez et inscrivez le programme de résolution à l'aide de la procédure précitée.  
  
2.  Sur le serveur de publication, exécutez [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) et notez le nom du programme de résolution personnalisé souhaité dans le champ **value** du jeu de résultats.  
  
3.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Spécifiez le nom du programme de résolution de l'étape 2 pour **@article_resolver** et toute entrée requise pour le programme de résolution personnalisé à l'aide du paramètre **@resolver_info** . Pour les programmes de résolution personnalisés s'appuyant sur des procédures stockées, **@resolver_info** est le nom de la procédure stockée. Pour plus d’informations sur l’entrée nécessaire aux programmes de résolution fournis par [!INCLUDE[msCoName](../../../includes/msconame-md.md)], consultez [Programmes de résolution COM Microsoft](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).  
  
#### <a name="to-specify-or-change-a-custom-resolver-for-an-existing-merge-article"></a>Pour spécifier ou modifier un programme de résolution personnalisé pour un article de fusion existant  
  
1.  Pour déterminer si un programme de résolution personnalisé a été défini pour un article, ou pour obtenir le nom du programme de résolution, exécutez [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md). Si un programme de résolution personnalisé a été défini pour l'article, son nom est affiché dans le champ **article_resolver** . Toute entrée fournie au programme de résolution est affichée dans le champ **resolver_info** du jeu de résultats.  
  
2.  Sur le serveur de publication, exécutez [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) et notez le nom du programme de résolution personnalisé souhaité dans le champ **value** du jeu de résultats.  
  
3.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Affectez la valeur **article_resolver**, y compris le chemin d'accès complet pour les gestionnaires de logique métier, à **@property**, et le nom du programme de résolution personnalisé souhaité de l'étape 2 à **@value**.  
  
4.  Pour modifier toute entrée requise pour le programme de résolution personnalisé, réexécutez [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Affectez la valeur **resolver_info** à **@property** et toute entrée requise pour le programme de résolution personnalisé à **@value**. Pour les programmes de résolution personnalisés s'appuyant sur des procédures stockées, **@resolver_info** est le nom de la procédure stockée. Pour plus d’informations sur l’entrée nécessaire, consultez [Programmes de résolution COM Microsoft](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).  
  
#### <a name="to-unregister-a-custom-conflict-resolver"></a>Pour annuler l'inscription d'un outil personnalisé de résolution des conflits  
  
1.  Sur le serveur de publication, exécutez [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) et notez le nom du programme de résolution personnalisé à supprimer dans le champ **value** du jeu de résultats.  
  
2.  Exécutez [sp_unregistercustomresolver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md) sur le serveur de distribution. Spécifiez le nom complet du programme de résolution personnalisé de l'étape 1 pour **@article_resolver**.  
  
###  <a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple crée un article et spécifie que l'Outil de résolution des conflits de moyenne [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être utilisé pour calculer la moyenne de la colonne **UnitPrice** en cas de conflit.  
  
 [!code-sql[HowTo#sp_addmerge_resolver](../../../relational-databases/replication/codesnippet/tsql/specify-a-merge-article-_1.sql)]  
  
 Cet exemple modifie un article de manière à spécifier que l'Outil de résolution des conflits d'addition [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être utilisé pour calculer la somme de la colonne **UnitsOnOrder** en cas de conflit.  
  
 [!code-sql[HowTo#sp_changemerge_resolver](../../../relational-databases/replication/codesnippet/tsql/specify-a-merge-article-_2.sql)]  
  
## <a name="see-also"></a> Voir aussi  
 [Advanced Merge Replication Conflict Detection and Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Implémenter un gestionnaire de logique métier pour un article de fusion](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)  
  
  
