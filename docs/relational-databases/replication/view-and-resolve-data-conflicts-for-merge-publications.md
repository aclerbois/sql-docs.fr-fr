---
title: Afficher et résoudre les conflits de données pour les publications de fusion | Microsoft Docs
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
- merge replication conflict resolution [SQL Server replication], viewing conflicts
- viewing conflict information
- conflict resolution [SQL Server replication], merge replication
ms.assetid: aeee9546-4480-49f9-8b1e-c71da1f056c7
caps.latest.revision: 41
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: edcff8925793a5e6751884ed9f36202284b2be77
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-and-resolve-data-conflicts-for-merge-publications"></a>Afficher et résoudre les conflits de données pour les publications de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les conflits de réplication de fusion sont résolus en fonction de l'outil de résolution spécifié pour chaque article. Par défaut, les conflits sont résolus sans que l'utilisateur doive intervenir. Mais il est possible de les afficher et de modifier le résultat de la résolution dans l'outil de résolution des conflits de réplication de [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 Les données de conflit sont disponibles dans cet outil pendant la durée définie comme période de rétention des conflits (par défaut 14 jours). Pour définir la période de rétention des conflits :  
  
-   Spécifiez une valeur de rétention pour le paramètre **@conflict_retention** de [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).  
  
-   Spécifiez la valeur **conflict_retention** pour le paramètre **@property** et une valeur de rétention pour le paramètre **@value** de [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md).  
  
 Par défaut, les informations sur les conflits sont stockées dans les emplacements suivants :  
  
-   Sur le serveur de publication et sur l'Abonné, si le niveau de compatibilité est égal ou supérieur à 90RTM.  
  
-   Sur le serveur de publication, si le niveau de compatibilité est inférieur à 80RTM.  
  
-   Sur le serveur de publication si les Abonnées exécutent [!INCLUDE[ssEW](../../includes/ssew-md.md)]. Les données conflictuelles ne peuvent pas être stockées sur les Abonnés [!INCLUDE[ssEW](../../includes/ssew-md.md)] .  
  
 Le stockage des informations de conflits est contrôlé par la propriété de publication **conflict_logging** . Pour plus d’informations, consultez [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) et [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md).  
  
 Il est également possible de résoudre les conflits de façon interactive au cours de la synchronisation à l'aide du programme de résolution interactif [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Le résolveur interactif est disponible via le Gestionnaire de synchronisation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Pour plus d’informations, consultez [Synchroniser un abonnement à l’aide du Gestionnaire de synchronisation Windows &#40;Windows Synchronization Manager&#41;](../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md).  
  
### <a name="to-view-and-resolve-conflicts-for-merge-publications"></a>Pour afficher et résoudre les conflits des publications de fusion  
  
1.  Connectez-vous au serveur de publication (ou à l'Abonné, le cas échéant) dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Cliquez avec le bouton droit sur la publication dont vous souhaitez afficher les conflits puis cliquez sur **Afficher les conflits**.  
  
    > [!NOTE]  
    >  Si vous spécifiez une valeur **'subscriber'** pour la propriété **conflict_logging** , l'option de menu **Afficher les conflits** n'est pas disponible. Pour afficher les conflits, démarrez ConflictViewer.exe à partir de l'invite de commandes. Par défaut, ConflictViewer.exe se trouve dans le répertoire suivant : Microsoft SQL Server\100\Tools\Binn\VSShell\Common7\IDE. Pour obtenir la liste des paramètres de démarrage valides, exécutez ConflictViewer.exe -?.  
  
4.  Dans la boîte de dialogue **Sélectionner la table de conflits** , sélectionnez une base de données et une table dont il faut afficher les conflits.  
  
5.  Dans l'outil de résolution des conflits de réplication, vous pouvez effectuer les actions suivantes :  
  
    -   Filtrer des lignes avec les boutons situés à droite de la grille supérieure.  
  
    -   Sélectionner une ligne dans la grille supérieure pour afficher des informations sur cette ligne dans la grille inférieure.  
  
    -   Sélectionner une ou plusieurs lignes dans la grille supérieure puis cliquer sur **Supprimer**, ce qui équivaut à cliquer sur le bouton **Soumettre le gagnant** (sans modifier les données).  
  
    -   Cliquer sur le bouton des propriétés (**…**) pour afficher des informations plus détaillées sur une colonne concernée par un conflit.  
  
    -   Modifier des données dans la colonne **Gagnant du conflit** ou **Perdant du conflit** avant de soumettre les données (les données sont en lecture seule si la colonne est grisée).  
  
    -   Cliquer sur **Soumettre le gagnant** pour accepter la ligne désignée comme gagnante du conflit.  
  
    -   Cliquer sur **Soumettre le perdant** pour substituer la résolution et propager la valeur désignée comme perdante du conflit à tous les nœuds de la topologie.  
  
    -   Sélectionner l'option **Consigner les détails de ce conflit** pour enregistrer les données de conflit dans un journal. Pour spécifier l'emplacement du fichier, pointez sur le menu **Affichage** puis cliquez sur **Options**. Entrez une valeur ou cliquez sur le bouton Parcourir (**...**) pour accéder au fichier approprié. Cliquez sur **OK** pour fermer la boîte de dialogue **Options** .  
  
6.  Fermer l'outil de résolution de conflits.  
  
## <a name="see-also"></a> Voir aussi  
 [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Spécifier un programme de résolution d’articles de fusion](../../relational-databases/replication/publish/specify-a-merge-article-resolver.md)  
  
  
