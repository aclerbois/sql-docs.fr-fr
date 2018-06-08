---
title: Création de scripts de réplication | Microsoft Docs
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
- scripts [SQL Server replication], replication objects
- merge replication scripting [SQL Server replication]
- replication [SQL Server], scripting
- snapshot replication [SQL Server], scripting
- scripts [SQL Server replication]
- transactional replication, scripting
ms.assetid: e50fac44-54c0-470c-a4ea-9c111fa4322b
caps.latest.revision: 36
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: fdbef6d34beb4eedcb7a7fea6f737d07bffc62f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="scripting-replication"></a>Création de scripts de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Tous les composants de réplication dans une topologie doivent faire l'objet d'un script et s'intégrer dans un plan de récupération des données en cas de sinistre ; les scripts peuvent également être utilisés pour automatiser des tâches répétitives. Un script contient les procédures stockées système Transact-SQL nécessaires pour mettre en œuvre le ou les composants de réplication définis dans des scripts, tels qu'une publication ou un abonnement. Il est possible de créer des scripts à l'aide d'un Assistant (comme l'Assistant Nouvelle publication) ou dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] après avoir créé un composant. Vous pouvez afficher, modifier et exécuter le script à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou **sqlcmd**. Les scripts peuvent être stockés avec les fichiers de sauvegarde dans le cas où la topologie de réplication doit être reconfigurée.  
  
 Un nouveau script doit être généré pour un composant si des modifications sont apportées à ses propriétés. Si vous utilisez des procédures stockées personnalisées avec la réplication transactionnelle, une copie de chaque procédure doit être stockée avec les scripts ; la copie doit être mise à jour si la procédure change (les procédures sont généralement mises à jour suite à des modifications de schéma ou à des modifications des conditions requises par l'application). Pour plus d’informations sur les procédures personnalisées, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
 Pour les publications de fusion qui utilisent des filtres paramétrés, les scripts de publication contiennent l'appel de la procédure stockée pour créer des partitions de données. Le script fournit une référence pour les partitions créées et un moyen permettant de recréer si nécessaire une ou plusieurs partitions.  
  
## <a name="example-of-automating-a-task-with-scripts"></a>Exemple d'automatisation d'une tâche avec des scripts  
 Considérons [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)], qui met en œuvre la réplication de fusion pour distribuer des données à sa force de vente distante. Un commercial télécharge toutes les données qui se rapportent aux clients de son territoire, à l'aide d'abonnements par extraction de données (pull). En mode déconnecté, les représentants commerciaux mettent à jour des données et entrent de nouveaux clients et de nouvelles commandes. [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] employant plus de 50 représentants commerciaux sur des territoires différents, il serait donc très long de créer les différents abonnements sur chaque Abonné avec l'Assistant Nouvel abonnement. À la place, l'administrateur de réplication peut procéder selon les étapes suivantes :  
  
1.  Configurez les publications de fusion nécessaires avec des partitions basées sur le représentant commercial ou son territoire.  
  
2.  Créez un abonnement par extraction de données (pull) pour un Abonné.  
  
3.  Générez un script basé sur cet abonnement par extraction de données (pull).  
  
4.  Modifiez le script, en changeant des valeurs comme le nom de l'Abonné.  
  
5.  Exécutez le script sur plusieurs Abonnés pour générer les abonnements par extraction de données (pull) requis.  
  
## <a name="script-replication-objects"></a>Objets de réplication Script  
 Générez des scripts pour des objets de réplication à partir des Assistants de réplication ou du dossier **Réplication** de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Si vous générez des scripts à partir des Assistants, vous pouvez opter pour la création d'objets et leur intégration dans un script ou pour la génération de scripts seuls.  
  
> [!IMPORTANT]  
>  Tous les mots de passe sont définis dans le script avec la valeur NULL. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous stockez des informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher tout accès non autorisé.  
  
 Pour plus d'informations sur l'utilisation des Assistants de réplication, consultez :  
  
-   [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)  
  
-   [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)  
  
-   [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)  
  
-   [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)  
  
#### <a name="to-script-an-object-from-a-replication-wizard"></a>Pour générer un script pour un objet à partir d'un Assistant de réplication  
  
1.  Dans la page **Actions de l'Assistant** d'un Assistant, activez la case à cocher adaptée à l'Assistant :  
  
    -   **Générer un fichier de script comportant les étapes de création d'une publication**  
  
    -   **Générer un fichier de script comportant les étapes de création d'une ou de plusieurs publications**  
  
    -   **Générer un fichier de script comportant les étapes de configuration de la distribution**  
  
2.  Spécifiez les options dans la page **Propriétés du fichier de script** .  
  
3.  Terminez l'Assistant.  
  
#### <a name="to-script-an-object-from-management-studio"></a>Pour générer un script pour un objet à partir de Management Studio  
  
1.  Connectez-vous au serveur de distribution, au serveur de publication ou à l'Abonné dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis le dossier **Publications locales** ou le dossier **Abonnements locaux** .  
  
3.  Cliquez avec le bouton droit sur une publication ou un abonnement, puis cliquez sur **Générer des scripts**.  
  
4.  Spécifiez les options dans la boîte de dialogue **Générer un Script SQL - \<Objet_Réplication>**.  
  
5.  Cliquez sur **Générer un script sur fichier**.  
  
6.  Entrez un nom de fichier dans la boîte de dialogue **Emplacement du fichier de script** , puis cliquez sur **Enregistrer**. Un message d'état est affiché.  
  
7.  Cliquez sur **OK**, puis sur **Fermer**.  
  
#### <a name="to-script-multiple-objects-from-management-studio"></a>Pour générer un script pour plusieurs objets à partir de Management Studio  
  
1.  Connectez-vous au serveur de distribution, au serveur de publication ou à l'Abonné dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Cliquez avec le bouton droit sur le dossier **Réplication** , puis cliquez sur **Générer des scripts**.  
  
3.  Spécifiez les options dans la boîte de dialogue **Générer un script SQL** .  
  
4.  Cliquez sur **Générer un script sur fichier**.  
  
5.  Entrez un nom de fichier dans la boîte de dialogue **Emplacement du fichier de script** , puis cliquez sur **Enregistrer**. Un message d'état est affiché.  
  
6.  Cliquez sur **OK** , puis sur **Fermer**.  
  
  
