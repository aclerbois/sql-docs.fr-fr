---
title: Abonnements pouvant être mis à jour pour la réplication transactionnelle | Microsoft Docs
ms.custom: ''
ms.date: 07/21/2016
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
- transactional replication, updatable subscriptions
- updatable subscriptions, about updatable subscriptions
- queued updating subscriptions [SQL Server replication]
- immediate updating subscriptions
- subscriptions [SQL Server replication], updatable
- updatable subscriptions
ms.assetid: 8eec95cb-3a11-436e-bcee-bdcd05aa5c5a
caps.latest.revision: 60
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: d52ee6d23418e83fb19a9029efe615b0f796f5e8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updatable-subscriptions---for-transactional-replication"></a>Abonnements pouvant être mis à jour pour la réplication transactionnelle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

    
> [!NOTE]  
>  Cette fonctionnalité reste prise en charge dans les versions de [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] (2012 à 2016). [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 La réplication transactionnelle prend en charge les mises à jour sur les Abonnés par l'intermédiaire d'abonnements qu'il est possible de mettre à jour et la réplication d'égal à égal. Les deux types d’abonnements pouvant être mis à jour sont les suivants :  
  
-   Mise à jour immédiate. Le serveur de publication et l'Abonné doivent être connectés pour mettre à jour les données sur l'abonné.  
  
-   Mise à jour en attente. Le serveur de publication et l'abonné ne doivent pas être connectés pour mettre à jour les données sur l'abonné. Les mises à jour peuvent être effectuées lorsque l'abonné ou le serveur de publication est hors connexion.  
  
 Lorsque les données sont mises à jour sur l'abonné, elles sont d'abord propagées au serveur de publication et ensuite aux autres abonnés. Dans le cas de la mise à jour immédiate, les modifications sont propagées immédiatement à l'aide du protocole de validation en deux phases. Pour la mise à jour en attente, les modifications sont stockées dans une file d'attente et les transactions mises en file d'attente sont appliquées de façon asynchrone au niveau du serveur de publication chaque fois que la connectivité réseau est disponible. Dans la mesure où les mises à jour sont propagées de façon asynchrone sur le serveur de publication, il peut arriver que les mêmes données soient mises à jour par le serveur de publication ou par un autre abonné, et des conflits peuvent naître lors de l'application des mises à jour. Les conflits sont détectés et résolus selon une stratégie de résolution de conflits définie lors de la création de la publication.  
  
 Si vous créez une publication transactionnelle avec des abonnements qui peuvent être mis à jour dans l'Assistant Nouvel abonnement, les deux modes de mise à jour, immédiate et en attente, sont activés. Si vous créez une publication avec des procédures stockées, vous pouvez n'en activer qu'un seul ou les deux. Lorsque vous créez un abonnement à la publication, vous spécifiez le mode de mise à jour à utiliser. Vous pouvez ensuite basculer entre les modes de mise à jour, le cas échéant. Pour plus d'informations, consultez la section « Basculement d'un mode de mise à jour à un autre ».  
  
 Pour activer les abonnements pouvant être mis à jour pour les publications transactionnelles, consultez [Enable Updating Subscriptions for Transactional Publications](../../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md).  
  
 Pour créer des abonnements pouvant être mis à jour pour les publications transactionnelles, consultez [Créer un abonnement pouvant être mis à jour pour une publication transactionnelle (Management Studio)](../../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md). 
  
## <a name="switching-between-update-modes"></a>Basculement d'un mode de mise à jour à l'autre  
 Lorsque vous utilisez les abonnements pouvant être mis à jour, vous pouvez spécifier qu'un abonnement doit utiliser un mode de mise à jour puis basculer vers l'autre si l'application l'exige. Vous pouvez, par exemple, spécifier qu'un abonnement doit utiliser la mise à jour immédiate mais basculer vers la mise à jour en attente si un échec système entraîne une perte de la connectivité réseau.  
  
> [!NOTE]  
>  La réplication ne bascule pas automatiquement d'un mode de mise à jour à l'autre. Vous devez le configurer par le biais de SQL Server Management Studio ou votre application doit appeler [sp_setreplfailovermode &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-setreplfailovermode-transact-sql.md) pour basculer entre les modes.  
  
 Si vous basculez de la mise à jour immédiate à la mise à jour en attente, vous ne pouvez pas repasser à la mise à jour immédiate avant que l'abonné et le serveur de publication soient connectés et que l'Agent de lecture de la file d'attente ait appliqué tous les messages en attente dans la file d'attente sur le serveur de publication.  
  
 **Pour basculer d'un mode de mise à jour vers un autre**  
  
 Pour basculer d'un mode à l'autre, vous devez activer la publication et l'abonnement pour les deux modes de mise à jour puis basculer d'un mode à l'autre, le cas échéant. Pour plus d'informations, consultez  
[Switch Between Update Modes for an Updatable Transactional Subscription](../../../relational-databases/replication/administration/switch-between-update-modes-for-an-updatable-transactional-subscription.md).  
  
### <a name="considerations-for-using-updatable-subscriptions"></a>Considérations sur l'utilisation des abonnements pouvant être mis à jour  
  
-   Après qu'une publication est activée pour les abonnements mis à jour immédiatement ou en attente, l'option ne peut plus être désactivée pour la publication (même si les abonnements ne doivent pas nécessairement l'utiliser). Pour désactiver cette option, vous devez supprimer la publication puis en recréer une nouvelle.  
  
-   Il est impossible de republier des données.  
  
-   La réplication ajoute la colonne **msrepl_tran_version** aux tables publiées à des fins de suivi. Du fait de cette colonne supplémentaire, toutes les instructions **INSERT** doivent inclure une liste de colonnes.  
  
-   Pour modifier le schéma dans une table d'une publication qui prend en charge les abonnements mis à jour, toutes les activités relatives à la table doivent être interrompues au niveau du serveur de publication et des abonnés et les modifications de données en attente doivent être propagées à tous les nœuds avant d'effectuer toute modification du schéma. De cette façon, vous êtes assuré que les transactions en cours n'entrent pas en conflit avec la modification de schéma en attente. Après la propagation des modifications de schéma à tous les nœuds, l'activité peut reprendre dans les tables publiées. Pour plus d’informations, consultez [Suspendre une topologie de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md).  
  
-   Si vous prévoyez de basculer entre les modes de mise à jour, l'Agent de lecture de la file d'attente doit s'exécuter au moins une fois après l'initialisation de l'abonnement (par défaut, cet Agent s'exécute en continu).  
  
-   Si la base de données de l'abonné est partitionnée horizontalement et que certaines lignes de la partition existent chez l'abonné, mais pas sur le serveur de publication, l'abonné ne pourra pas mettre à jour les lignes préexistantes. Toute tentative de mise à jour de ces lignes renvoie un message d'erreur. Les lignes doivent être supprimées de la table puis ajoutées sur le serveur de publication.  

-   Les performances de la réplication transactionnelle avec des abonnés pouvant être mis à jour en file d’attente peuvent être lentes quand des index filtrés uniques sont utilisés. Si un conflit se produit sur un article qui a des index filtrés uniques, la résolution des conflits conduirait à d’autres suppressions et insertions sur l’abonné pour les lignes non couvertes par l’index filtré unique.
  
### <a name="updates-at-the-subscriber"></a>Mises à jour sur l'abonné  
  
-   Les mises à jour sur l'abonné sont propagées au serveur de publication même si un abonnement expire ou est inactif. Vérifiez que ces abonnements sont effectivement supprimés ou réinitialisés.  
  
-   Si des colonnes **TIMESTAMP** ou **IDENTITY** sont utilisées et répliquées en tant que types de données de base, les valeurs qu’elles contiennent ne doivent pas être mises à jour sur l’Abonné.  
  
-   Les Abonnés ne peuvent pas mettre à jour ou insérer des valeurs **text**, **ntext** ou **image** , car il est impossible de lire à partir des tables insérées ou supprimées dans les déclencheurs de suivi des modifications de réplication. De même, les Abonnés ne peuvent pas mettre à jour ou insérer de valeur **text** ou **image** à l’aide de **WRITETEXT** ou **UPDATETEXT** , car les données sont écrasées par le serveur de publication. En revanche, vous pouvez partitionner les colonnes **text** et **image** dans une table distincte et modifier les deux tables à l’intérieur d’une transaction.  
  
     Pour mettre à jour des objets volumineux sur un Abonné, utilisez respectivement les types de données **varchar(max)**, **nvarchar(max)** et **varbinary(max)** au lieu des types de données **text**, **ntext**et **image** .  
  
-   Les mises à jour de clés uniques (y compris les clés primaires) générant des doublons (comme une mise à jour de type `UPDATE <column> SET <column> =<column>+1` ) ne sont pas autorisées et sont rejetées en raison d’une violation d’unicité. En effet, les mises à jour de jeux appliquées sur l’Abonné sont propagées par la réplication en tant qu’instructions **UPDATE** individuelles pour chaque ligne concernée.  
  
-   Si la base de données de l'abonné est partitionnée horizontalement et que des lignes de la partition existent sur l'abonné mais non sur le serveur de publication, l'abonné ne peut pas mettre à jour les lignes pré-existantes. Toute tentative de mise à jour de ces lignes renvoie un message d'erreur. Les lignes doivent être supprimées de la table puis réinsérées.  
  
### <a name="user-defined-triggers"></a>Déclencheurs définis par l’utilisateur  
  
-   Si l’application exige la présence de déclencheurs sur l’Abonné, ceux-ci doivent être définis avec l’option `NOT FOR REPLICATION` sur le serveur de publication et l’Abonné. Cela garantit l'activation des déclencheurs pour la modification de données d'origine uniquement mais pas lors de la réplication de cette modification.  
  
     Veillez à ce que le déclencheur défini par l'utilisateur ne s'active pas lorsque le déclencheur de réplication met à jour la table. Pour ce faire, la procédure **sp_check_for_sync_trigger** doit être appelée dans le corps du déclencheur défini par l’utilisateur. Pour plus d’informations, consultez [sp_check_for_sync_trigger &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-check-for-sync-trigger-transact-sql.md).  
  
### <a name="immediate-updating"></a>Mise à jour immédiate  
  
-   Pour des abonnements mis à jour immédiatement, les modifications sur l'abonné sont propagées au serveur de publication et appliquées à l'aide du Coordinateur de transactions distribuées Microsoft (MSDTC, Microsoft Distributed Transaction Coordinator). Vérifiez que ce dernier est installé et configuré sur le serveur de publication et l'abonné. Pour plus d'informations, consultez la documentation Windows.  
  
-   Les déclencheurs utilisés par les abonnements mis à jour immédiatement exigent une connexion au serveur de publication pour répliquer les modifications.  
  
-   Si la publication autorise les abonnements mis à jour immédiatement et qu'un article de la publication possède un filtre de colonne, vous ne pouvez pas retirer les colonnes n'acceptant pas les valeurs NULL sans valeurs par défaut.  
  
### <a name="queued-updating"></a>Mise à jour en attente  
  
-   Les tables contenues dans une publication de fusion ne peuvent pas être publiées en même temps dans le cadre d'une réplication transactionnelle qui autorise les abonnements mis à jour en attente.  
  
-   Lorsque la mise à jour en attente est utilisée, il est déconseillé d'effectuer des mises à jour sur les colonnes clés primaire car la clé primaire sert de localisateur d'enregistrement pour toutes les requêtes. Lorsque la stratégie de résolution de conflit prévoit que " l'abonné gagne ", les mises à jour de clés primaires doivent être effectuées avec précaution. Si des mises à jour de la clé primaire sont effectuées à la fois sur le serveur de publication et sur l'abonné, il en résultera deux lignes avec des clés primaires différentes.  
  
-   Pour les colonnes de type de données **SQL_VARIANT**: quand des données sont insérées ou mises à jour sur l’abonné, elles sont mappées de la façon suivante par l’Agent de lecture de la file d’attente quand elles sont copiées de l’abonné vers la file d’attente :  
  
    -   **BIGINT**, **DECIMAL**, **NUMERIC**, **MONEY**et **SMALLMONEY** sont mappés à **NUMERIC**.  
  
    -   **BINARY** et **VARBINARY** sont mappés aux données **VARBINARY** .  
  
### <a name="conflict-detection-and-resolution"></a>Détection et résolution des conflits  
  
-   Dans le cas d'une stratégie de conflit où l'abonné « gagne » : la résolution de conflit n'est pas prise en charge pour les mises à jour des colonnes de clé primaire.  
  
-   Les conflits provoqués par des échecs de contraintes de clés étrangères ne sont pas résolus par la réplication :  
  
    -   Si vous ne prévoyez pas de conflits et que les données sont bien partitionnées (les abonnés ne mettent pas à jour les mêmes lignes), vous pouvez utiliser les contraintes de clés étrangères sur le serveur de publication et les abonnés.  
  
    -   Si des confits sont prévisibles : vous ne devez pas utiliser des contraintes de clés étrangères sur le serveur de publication ou l'abonné si vous optez pour la résolution de conflit où l'abonné prime ; vous ne devez pas utiliser des contraintes de clés étrangères sur l'abonné si vous utilisez la résolution de conflit où le serveur de publication prime.  
  
## <a name="see-also"></a> Voir aussi  
 [Réplication transactionnelle d’égal à égal](../../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Types de publication pour la réplication transactionnelle](../../../relational-databases/replication/transactional/publication-types-for-transactional-replication.md)   
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [S'abonner à des publications](../../../relational-databases/replication/subscribe-to-publications.md)  
  
  
