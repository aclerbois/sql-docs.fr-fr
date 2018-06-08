---
title: Propriétés de la publication, Options d’abonnement | Microsoft Docs
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
f1_keywords:
- sql13.rep.newpubwizard.pubproperties.subscriptionoptions.f1
ms.assetid: 31abd605-b273-419d-86df-d0ecf539a507
caps.latest.revision: 39
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 451273b019c91d7bf92ae2f277bb4a7b69e668b8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publication-properties-subscription-options"></a>Propriétés de la publication, Options d'abonnement
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La page **Options d'abonnement** de la boîte de dialogue **Propriétés de la publication** vous permet d'afficher et de définir les propriétés de niveau de publication associées aux abonnements. Les propriétés sont regroupées selon les catégories suivantes :  
  
-   Propriétés qui s'appliquent à toutes les publications.  
  
-   Propriétés qui s'appliquent aux publications d'instantané et transactionnelles (y compris celles qui permettent de mettre à jour les abonnements).  
  
-   Propriétés qui s'appliquent aux publications de fusion.  
  
> [!NOTE]  
>  Certaines propriétés sont en lecture seule ; vous trouverez de plus amples explications à cet égard dans les descriptions de propriété de la présente rubrique. Certaines modifications de propriété nécessitent un nouvel instantané pour la publication, et d'autres nécessitent également la réinitialisation de tous les abonnements. Pour plus d’informations, consultez [Modifier les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
## <a name="options-for-all-publications"></a>Options pour toutes les publications  
  
### <a name="creation-and-synchronization"></a>Création et synchronisation  
 **Autoriser les abonnements anonymes**  
 Détermine s'il est nécessaire d'autoriser les extractions d'abonnements anonymes. Les abonnements anonymes sont pris en charge pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEWEd2005](../../includes/ssewed2005-md.md)], [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssMobileEd2005](../../includes/ssmobileed2005-md.md)]et [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour Windows CE. Pour utiliser cette option pour les publications d'instantané et transactionnelles, l'option **Instantané toujours disponible** doit avoir la valeur **True**.  
  
 **Base de données d'abonnement pouvant être attachée**  
 Détermine s'il est possible de créer des abonnements en attachant une copie d'une base de données d'abonnement (nécessite que l'option **Instantané toujours disponible** ait la valeur **True** pour les publications d'instantané et transactionnelles).  
  
> [!IMPORTANT]  
>  Les abonnements attachables ne seront pas disponibles dans une version ultérieure. Cette fonctionnalité est déconseillée.  
  
 **Autoriser les abonnements extraits**  
 Détermine s'il est nécessaire d'autoriser les abonnés à créer des extractions d'abonnements sur cette publication. Pour plus d’informations, consultez [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md).  
  
### <a name="schema-replication"></a>Réplication de schéma  
 **Réplique les modifications de schéma**  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures uniquement. Détermine s'il est nécessaire de répliquer les modifications de schéma (telles que l'ajout d'une colonne à une table ou la modification du type de données d'une colonne) sur les objets publiés. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
## <a name="options-for-snapshot-and-transactional-publications"></a>Options applicables aux publications d'instantané et transactionnelles  
  
### <a name="creation-and-synchronization"></a>Création et synchronisation  
 **Agent de distribution indépendant**  
 Détermine s'il est nécessaire d'utiliser un agent indépendant des autres publications de cette base de données. Cette option est en lecture seule. Par défaut, elle a la valeur **True** pour les publications créées à l'aide de l'Assistant Nouvelle publication et ne peut être modifiée une fois la publication créée. Pour plus d’informations, consultez [Administration de l’Agent de réplication](../../relational-databases/replication/agents/replication-agent-administration.md).  
  
 **Instantané toujours disponible**  
 Détermine si les fichiers d'instantanés sont créés lors de chaque exécution de l'Agent d'instantané (nécessite l'option **Agent de distribution indépendant**). Cette option est en lecture seule. Elle a la valeur **True** si vous sélectionnez **Créer un instantané immédiatement et garder cette dernière disponible pour l'initialisation des abonnements** à la page **Agent d'instantané** de l'Assistant Nouvelle publication (valeur par défaut). Pour plus d’informations, consultez [Créer et appliquer un instantané](../../relational-databases/replication/create-and-apply-the-snapshot.md).  
  
 **Autoriser l'initialisation à partir de fichiers de sauvegarde**  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures uniquement. Détermine s'il est nécessaire d'autoriser l'utilisation de fichiers de sauvegarde pour initialiser les abonnements. Pour plus d’informations, consultez [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 **Autoriser les abonnés non-SQL Server**  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures uniquement. Détermine si la publication prend en charge les abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si cette option est définie sur **True** , les autres propriétés de publication prennent également en charge les abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option est en lecture seule si des abonnements existent. Elle ne peut avoir la valeur **True** si l'option **Autoriser les abonnements mis à jour immédiatement**, **Autoriser les abonnements mis à jour en attente**ou **Autoriser les abonnements d'égal à égal** a la valeur **True**. Pour plus d’informations, consultez [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
### <a name="data-transformation"></a>Transformation des données  
 **Autoriser les transformations de données**  
 Détermine s'il est nécessaire d'utiliser DTS (Data Transformation Services) pour transformer les données avant de les distribuer à un abonné. Cette option est en lecture seule. Vous pouvez uniquement activer les transformations de données si vous créez une publication à l'aide des procédures stockées.  
  
> [!IMPORTANT]  
>  Les abonnements transformables ne seront pas disponibles dans une version ultérieure. Cette fonctionnalité est déconseillée.  
  
### <a name="peer-to-peer-replication"></a>Réplication d'égal à égal  
 **Autoriser les abonnements d'égal à égal**  
 S'applique uniquement à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures. Détermine si la publication prend en charge la réplication d'égal à égal. Si cette option est définie sur **True** , les autres propriétés de publication prennent également en charge la réplication d'égal à égal. Cette option est en lecture seule si des abonnements existent. Cette option ne peut avoir la valeur **True** si l'option **Autoriser les abonnements mis à jour immédiatement** , **Autoriser les abonnements mis à jour en attente**ou **Autoriser les Abonnés non-SQL Server** a la valeur **True**. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 **Autoriser la détection de conflit d'égal à égal**  
 S'applique uniquement à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures. Spécifie si la détection de conflit est activée pour cette publication. Pour utiliser la détection de conflit, tous les nœuds doivent exécuter [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou version ultérieure, et la détection doit être activée pour tous les nœuds. Pour utiliser la détection de conflit, vous devez également spécifier une valeur pour **ID d'appelant de l'homologue**. Pour plus d'informations, consultez [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
 **ID d'appelant de l'homologue**  
 S'applique uniquement à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures. Spécifie un ID pour un nœud dans une topologie d'égal à égal. Cet ID est utilisé pour détection de conflit si **Autoriser la détection de conflit d'égal à égal** a la valeur **True**. Spécifiez un ID positif différent de zéro qui n'a jamais été utilisé dans la topologie. Pour obtenir la liste des ID qui ont déjà été utilisés, interrogez la table système [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) .  
  
### <a name="updatable-subscriptions"></a>Abonnements pouvant être mis à jour  
 **Autoriser les abonnements mis à jour immédiatement**  
 Détermine si les modifications de données de l'abonné peuvent être immédiatement répliquées sur le serveur de publication. Cette option est en lecture seule. Vous pouvez uniquement activer la mise à jour d'abonnement lors de la création d'une publication. Pour plus d’informations, consultez [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 **Autoriser les abonnements mis à jour en attente**  
 Détermine si les modifications de données de l'abonné peuvent être mises en file d'attente et répliquées ultérieurement sur le serveur de publication. Cette option est en lecture seule. Vous pouvez uniquement activer la mise à jour d'abonnement lors de la création d'une publication. Pour plus d’informations, voir [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 **Signaler les conflits de manière centrale**  
 Détermine s'il est nécessaire de signaler les conflits de modifications de données uniquement sur le serveur de publication ou à la fois sur le serveur de publication et à l'abonné (nécessite l'option **Autoriser les abonnements mis à jour en attente**). Cette option est en lecture seule. Par défaut, elle a la valeur **True** pour les publications créées à l'aide de l'Assistant Nouvelle publication et ne peut être modifiée une fois la publication créée. Lorsque cette option a la valeur **True** , les conflits sont uniquement signalés sur le serveur de publication. Vous pouvez uniquement voir les conflits lors de leur signalement.  
  
 **Stratégie de résolution de conflit**  
 Spécifie l'action à entreprendre lorsqu'une modification d'abonné entre en conflit avec une modification de serveur de publication (nécessite l'option **Autoriser les abonnements mis à jour en attente**). Pour plus d’informations, consultez [Queued Updating Conflict Detection and Resolution](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md).  
  
 **Type de file d'attente**  
 Détermine s'il est nécessaire d'utiliser une file d'attente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing (MSMQ) pour mettre en file d'attente les modifications de l'abonné jusqu'à ce qu'elles puissent être appliquées au serveur de publication (nécessite l'option **Autoriser les abonnements mis à jour en attente**). Cette option concerne uniquement [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]; les versions ultérieures utilisent toujours les tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la mise en file d'attente.  
  
## <a name="options-for-merge-publications"></a>Options des publications de fusion  
  
### <a name="conflict-reporting"></a>Rapport de conflit  
 **Signaler les conflits de manière centrale**  
 Détermine s'il est nécessaire de signaler les conflits de modifications de données uniquement sur le serveur de publication ou à la fois sur le serveur de publication et à l'abonné. Cette option est en lecture seule. Par défaut, elle a la valeur **True** pour les publications créées à l'aide de l'Assistant Nouvelle publication et ne peut être modifiée une fois la publication créée. Lorsque cette option a la valeur **True** , les conflits sont uniquement signalés sur le serveur de publication. Vous pouvez uniquement voir les conflits lors de leur signalement. Pour plus d'informations, consultez la section relative à l'affichage des conflits de [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
### <a name="filtering"></a>Filtrage  
 **Autoriser les filtres paramétrés**  
 Dépend du fait qu'une publication utilise ou non des filtres paramétrés. Cette option est toujours en lecture seule. Pour plus d’informations, consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Valider les abonnés**  
 Détermine les fonctions à utiliser lors de la confirmation qu'un abonné dispose de la partition de données correcte. Séparez les valeurs multiples par des virgules. Pour plus d’informations, consultez [Valider des informations de partition pour un Abonné de fusion](../../relational-databases/replication/validate-partition-information-for-a-merge-subscriber.md).  
  
 **Précalculer les partitions**  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures uniquement. Détermine s'il est nécessaire d'optimiser la synchronisation en calculant à l'avance l'appartenance des lignes de données aux partitions. La valeur par défaut de ce paramètre est **True** si la publication répond aux critères des partitions précalculées. Pour plus d’informations, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
 **Optimiser la synchronisation**  
 Détermine s'il est nécessaire d'optimiser le traitement de la fusion en stockant des métadonnées supplémentaires pour chaque abonné. Cette optimisation a été remplacée par les partitions précalculées. L'option **Optimiser la synchronisation** est uniquement pertinente si le paramètre **Précalculer les partitions** a la valeur **False**. Pour plus d’informations, consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
### <a name="merge-processes"></a>Processus de fusion  
 **Limiter les processus simultanés**  
 Détermine s'il est nécessaire de limiter le nombre d'Agents de fusion pouvant s'exécuter simultanément. Cette option est généralement utilisée lorsqu'une publication compte un grand nombre d'abonnements par émission de données pouvant être synchronisés en même temps.  
  
 **Nombre maximal de processus simultanés**  
 Nombre maximal d'Agents de fusion pouvant s'exécuter simultanément (nécessite l'option **Limiter les processus simultanés**). Si le nombre d'agents en cours de synchronisation dépasse la limite maximale, ils sont placés en file d'attente jusqu'à ce que leur nombre soit inférieur au maximum.  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
