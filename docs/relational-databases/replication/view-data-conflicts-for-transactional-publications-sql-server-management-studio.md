---
title: Afficher les conflits de données pour les publications transactionnelles (SSMS) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
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
- conflict resolution [SQL Server replication], queued updating subscriptions
- queued updating subscriptions [SQL Server replication]
- viewing conflict information
ms.assetid: 9977dd75-b0de-4376-9c13-86d80567d8aa
caps.latest.revision: 36
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 3ad37a1b799ca8070a418094dfb69d2693c21b20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-data-conflicts-for-transactional-publications-sql-server-management-studio"></a>afficher les conflits de données pour les publications de fusion (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez afficher les conflits pour la réplication transactionnelle d'égal à égal et la réplication transactionnelle avec des abonnements mis à jour en attente dans l'outil de résolution des conflits de réplication de [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Pour plus d’informations sur la détection et la résolution des conflits, consultez [Détection de conflit dans la réplication d’égal à égal](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md) et [Définir des options de résolution des conflits de mise à jour en attente &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/publish/set-queued-updating-conflict-resolution-options-sql-server-management-studio.md).  
  
 La disponibilité des données de conflit dépend du type de réplication et de la période de rétention des conflits :  
  
-   Pour la réplication d'égal à égal, par défaut, l'Agent de distribution échoue lorsqu'il détecte un conflit. Une erreur de conflit est enregistrée dans le journal des erreurs, mais aucune donnée de conflit n'est enregistrée dans la table de conflits ; par conséquent, la consultation de cette erreur n'est pas possible. Si l'Agent de distribution est autorisé à continuer, un conflit est enregistré localement sur chaque nœud où il a été détecté. Pour plus d'informations, consultez « Gestion des conflits » dans [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
-   Pour des abonnements mis à jour en attente, les données sont disponibles pour chaque conflit. Les données de conflit sont disponibles dans l'outil de résolution des conflits de réplication pendant la durée définie comme période de rétention des conflits (par défaut, 14 jours). Pour définir la période de rétention des conflits, effectuez l'une des opérations suivantes :  
  
    -   Spécifiez une valeur de rétention pour le paramètre @conflict_retention de [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
    -   Spécifiez la valeur **'conflict_retention'** pour le paramètre @property et une valeur de rétention pour le paramètre @value de [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md).  
  
### <a name="to-view-conflicts"></a>Pour afficher les conflits  
  
1.  Connectez-vous au serveur approprié dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur :  
  
    -   Pour la réplication d'égal à égal, il s'agit du nœud où le conflit s'est produit.  
  
    -   Pour les abonnements mis à jour en attente, il s'agit du serveur de publication.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Cliquez avec le bouton droit sur la publication dont vous souhaitez afficher les conflits puis cliquez sur **Afficher les conflits**.  
  
4.  Dans la boîte de dialogue **Sélectionner la table de conflits** , sélectionnez une base de données et une table dont il faut afficher les conflits.  
  
5.  Dans l'outil de résolution des conflits de réplication, vous pouvez effectuer les actions suivantes :  
  
    -   Filtrer des lignes avec les boutons situés à droite de la grille supérieure.  
  
    -   Sélectionner une ligne dans la grille supérieure pour afficher des informations sur cette ligne dans la grille inférieure.  
  
    -   Sélectionner une ou plusieurs lignes dans la grille supérieure puis cliquer sur **Supprimer**, pour supprimer la ligne de la table des métadonnées des conflits.  
  
    -   Cliquer sur le bouton des propriétés (**…**) pour afficher des informations plus détaillées sur une colonne concernée par un conflit.  
  
    -   Sélectionner l'option **Consigner les détails de ce conflit** pour enregistrer les données de conflit dans un journal. Pour spécifier l'emplacement du fichier, pointez sur le menu **Affichage** puis cliquez sur **Options**. Entrez une valeur ou cliquez sur le bouton Parcourir (**...**) pour accéder au fichier approprié. Cliquez sur **OK** pour fermer la boîte de dialogue **Options** .  
  
6.  Fermer l'outil de résolution de conflits.  
  
## <a name="see-also"></a> Voir aussi  
 [Réplication transactionnelle d’égal à égal](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Queued Updating Conflict Detection and Resolution](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md)  
  
  
