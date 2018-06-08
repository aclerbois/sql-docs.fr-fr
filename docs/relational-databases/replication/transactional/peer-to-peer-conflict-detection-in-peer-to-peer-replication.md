---
title: Détection de conflit dans la réplication d’égal à égal | Microsoft Docs
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
- transactional replication, peer-to-peer replication
- peer-to-peer transactional replication, conflict detection
ms.assetid: 754a1070-59bc-438d-998b-97fdd77d45ca
caps.latest.revision: 18
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 01b896a3f5c573730eea88cfb2d06dd3b5a4a733
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="peer-to-peer---conflict-detection-in-peer-to-peer-replication"></a>Égal à égal - Détection de conflit dans la réplication d’égal à égal
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication transactionnelle d'égal à égal vous permet d'insérer, de mettre à jour et de supprimer des données sur un nœud quelconque dans une topologie, puis de propager les modifications aux autres nœuds. Comme vous pouvez modifier des données sur un nœud quelconque, les modifications de données sur des nœuds différents peuvent être en conflit les unes avec les autres. Si une ligne est modifiée au niveau de plusieurs nœuds, elle peut provoquer un conflit, voire la perte de la mise à jour, lorsque la ligne est propagée à d'autres nœuds.  
  
 Dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] et les versions ultérieures, la réplication d'égal à égal propose une option permettant d'activer la détection des conflits dans une topologie d'égal à égal. Cette option permet d'éviter les problèmes causés par des conflits non détectés, notamment le comportement incohérent des applications et la perte d'une mise à jour. Lorsque cette option est activée, par défaut, une modification en conflit est traitée comme une erreur critique qui provoque l'échec de l'Agent de distribution. En cas de conflit, la topologie reste dans un état incohérent jusqu'à la résolution du conflit et jusqu'au rétablissement de la cohérence des données dans la topologie.  
  
> [!NOTE]  
>  Pour éviter l'incohérence potentielle des données, faites-en sorte d'éviter les conflits dans une topologie d'égal à égal, même si la détection des conflits est activée. Afin de garantir que les opérations d'écriture pour une ligne particulière soient réalisées au niveau d'un seul nœud, les applications qui accèdent aux données et les modifient doivent partitionner les opérations d'insertion, de mise à jour et de suppression. Ce partitionnement garantit que les modifications apportées à une ligne donnée provenant d'un nœud sont synchronisées sur tous les autres nœuds de la topologie avant que cette ligne soit modifiée par un autre nœud. Si une application requiert des fonctionnalités avancées de détection et de résolution des conflits, utilisez la réplication de fusion. Pour plus d’informations, consultez [Réplication de fusion](../../../relational-databases/replication/merge/merge-replication.md) et [Détecter et résoudre des conflits de réplication de fusion](../../../relational-databases/replication/merge/advanced-merge-replication-resolve-merge-replication-conflicts.md).  
  
## <a name="understanding-conflicts-and-conflict-detection"></a>Description des conflits et de la détection de conflit  
 Dans une base de données unique, les modifications apportées à une même ligne par des applications différentes ne provoquent pas de conflit. Cela est dû au fait que les transactions sont sérialisées et que des verrous sont utilisés pour traiter des modifications simultanées. Dans un système distribué asynchrone, tel que la réplication d'égal à égal, les transactions agissent indépendamment sur chaque nœud et aucun mécanisme ne sérialise les transactions sur plusieurs nœuds. Il est possible d'utiliser un protocole semblable à une validation en deux phases, mais cela affecte considérablement les performances.  
  
 Dans les systèmes tels que la réplication d'égal à égal, les conflits ne sont pas détectés lorsque les modifications sont validées au niveau d'homologues individuels. À la place, ils sont détectés lorsque ces modifications sont répliquées et appliquées à d'autres homologues. Dans la réplication d'égal à égal, les conflits sont détectés par les procédures stockées qui appliquent des modifications à chaque nœud en fonction d'une colonne masquée dans chaque table publiée. Cette colonne masquée stocke un ID qui associe un *ID d'appelant* que vous spécifiez pour chaque nœud et la version de la ligne. Pendant la synchronisation, l'Agent de distribution exécute des procédures pour chaque table. Ces procédures appliquent des opérations d'insertion, de mise à jour et de suppression provenant d'autres homologues. Si l'une de ces procédures détecte un conflit lorsqu'elle lit la valeur de la colonne masquée, elle déclenche l'erreur 22815 de niveau de gravité 16 :  
  
 `A conflict of type '%s' was detected at peer %d between peer %d (incoming), transaction id %s  and peer %d (on disk), transaction id %s`  
  
 Par défaut, suite à cette erreur, l'Agent de distribution cesse d'appliquer des modifications à ce nœud. Pour plus d'informations sur la façon de traiter les conflits détectés, consultez « Gestion des conflits », plus loin dans cette rubrique.  
  
> [!NOTE]  
>  Seul un utilisateur qui se connecte par le biais de la connexion administrateur dédiée peut accéder à la colonne masquée. Pour obtenir des informations sur la connexion administrateur dédiée, consultez [Connexion de diagnostic pour les administrateurs de base de données](../../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md).  
  
 La réplication d'égal à égal détecte les types de conflits suivants :  
  
-   Conflit d'insertions  
  
     Toutes les lignes de chaque table qui participe à la réplication d'égal à égal sont identifiées de façon unique à l'aide de valeurs de clés primaires. Un conflit d'insertions se produit lorsqu'une ligne avec la même valeur de clé a été insérée sur plusieurs nœuds.  
  
-   Conflit de mises à jour  
  
     Se produit lorsque la même ligne a été mise à jour sur plusieurs nœuds.  
  
-   Conflit d'insertion/mise à jour  
  
     Se produit si une ligne a été mise à jour sur un nœud et que la même ligne a été supprimée puis réinsérée sur un autre nœud.  
  
-   Conflit d'insertion/suppression  
  
     Se produit si une ligne a été supprimée sur un nœud et que la même ligne a été supprimée puis réinsérée sur un autre nœud.  
  
-   Conflit de mise à jour/suppression  
  
     Se produit si une ligne a été mise à jour sur un nœud et que la même ligne a été supprimée sur un autre nœud.  
  
-   Conflit de suppressions  
  
     Se produit lorsqu'une ligne a été supprimée sur plusieurs nœuds.  
  
## <a name="enabling-conflict-detection"></a>Activation de la détection de conflit  
 Pour utiliser la détection de conflit, tous les nœuds doivent exécuter [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] ou version ultérieure, et la détection doit être activée pour tous les nœuds. Dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] et versions ultérieures, par défaut, la détection de conflit est activée dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Nous vous recommandons d'activer la détection, même dans les scénarios où vous ne prévoyez pas de conflits. La détection de conflit peut être activée et désactivée en utilisant [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] ou les procédures stockées [!INCLUDE[tsql](../../../includes/tsql-md.md)] :  
  
-   Vous pouvez activer et désactiver la détection dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] en utilisant la page **Options d'abonnement** de la boîte de dialogue **Propriétés de la publication** ou la page **Configurer la topologie** de l'Assistant Configurer la topologie d'égal à égal. Pour plus d'informations, consultez [Conflict Detection in Peer-to-Peer Replication](../../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
     Si vous configurez la détection de conflit à l'aide de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], l'Agent de distribution est configuré pour cesser d'appliquer les modifications lorsqu'un conflit est détecté.  
  
-   Vous pouvez également activer et désactiver la détection en utilisant les procédures stockées suivantes : [sp_addpublication](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) ou [sp_configure_peerconflictdetection](../../../relational-databases/system-stored-procedures/sp-configure-peerconflictdetection-transact-sql.md).  
  
     Si vous configurez la détection de conflit à l'aide de procédures stockées, vous pouvez spécifier si l'Agent de distribution doit cesser d'appliquer les modifications lorsqu'un conflit est détecté. Par défaut, l'agent doit cesser d'appliquer les modifications. Nous vous recommandons d'utiliser le paramètre par défaut.  
  
## <a name="handling-conflicts"></a>Gestion des conflits  
 Lorsqu'un conflit se produit dans la réplication d'égal à égal, l'alerte de détection de conflit d'égal à égal est déclenchée. Nous vous recommandons de configurer cette alerte pour être notifié en cas de conflit. Pour plus d’informations sur les alertes, consultez [Utiliser les alertes pour les événements des agents de réplication](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md).  
  
 Une fois que l'Agent de distribution s'est arrêté et que l'alerte a été déclenchée, utilisez l'une des approches suivantes pour gérer les conflits qui se sont produits :  
  
-   Réinitialisez le nœud sur lequel le conflit a été détecté à partir de la sauvegarde d'un nœud qui contient les données requises (approche recommandée). Cette méthode garantit la cohérence des données.  
  
-   Essayez de synchroniser de nouveau le nœud en permettant à l'Agent de distribution de continuer à appliquer les modifications :  
  
    1.  Exécutez [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) : spécifiez 'p2p_continue_onconflict' pour le paramètre @property et **true** pour le paramètre @value.  
  
    2.  Redémarrez l'Agent de distribution.  
  
    3.  Vérifiez les conflits détectés en utilisant l'outil de résolution des conflits et déterminez les lignes qui étaient impliquées, le type de conflit et le vainqueur. Le conflit est résolu en fonction de la valeur de l'ID d'appelant que vous avez spécifiée pendant la configuration : la ligne provenant du nœud avec l'ID le plus élevé remporte le conflit. Pour plus d’informations, consultez [Afficher les conflits de données pour les publications transactionnelles &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/view-data-conflicts-for-transactional-publications-sql-server-management-studio.md).  
  
    4.  Exécutez une validation pour garantir que les lignes en conflit ont convergé correctement. Pour plus d’informations, consultez [Valider des données répliquées](../../../relational-databases/replication/validate-replicated-data.md).  
  
        > [!NOTE]  
        >  Si les données ne sont pas cohérentes après cette étape, vous devez mettre à jour manuellement les lignes sur le nœud qui a la priorité la plus élevée, puis propager les modifications à partir de ce nœud. S'il n'y a plus de modifications en conflit dans la topologie, tous les nœuds bénéficieront d'un état cohérent.  
  
    5.  Exécutez [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) : spécifiez 'p2p_continue_onconflict' pour le paramètre @property et **false** pour le paramètre @value.  
  
## <a name="see-also"></a> Voir aussi  
 [Peer-to-Peer Transactional Replication](../../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)  
  
  
