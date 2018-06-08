---
title: 'Guide du développeur : rubriques de guide pratique (réplication) | Microsoft Docs'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
ms.assetid: c6c15ae6-da52-4638-93d3-61c7242e8a0b
caps.latest.revision: 12
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8ea57ad6fed81479ee7191b683f9c79ef35fd67c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="developer39s-guide-how-to-topics-replication"></a>Guide du développeur : rubriques de guide pratique (réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique contient des liens vers des informations indiquant comment effectuer par programmation des tâches liées à la réplication.  
  
## <a name="securing-a-replication-topology"></a>Sécurisation d'une topologie de réplication  
  
-   [Afficher et modifier les paramètres de sécurité de la réplication](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
-   [Gérer des connexions dans la liste d’accès à la publication](../../../relational-databases/replication/security/manage-logins-in-the-publication-access-list.md)  
  
## <a name="configuring-modifying-and-disabling-publishing-and-distribution-replication"></a>Configuration, modification et désactivation de la publication et de la distribution (réplication)  
  
-   [Configurer la publication et la distribution](../../../relational-databases/replication/configure-publishing-and-distribution.md)  
  
-   [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)  
  
-   [Désactiver la publication et la distribution](../../../relational-databases/replication/disable-publishing-and-distribution.md)  
  
## <a name="creating-modifying-and-deleting-publications-and-articles"></a>Création, modification et suppression de publications et d'articles  
  
-   [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)  
  
-   [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)  
  
-   [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)  
  
-   [Afficher et modifier les propriétés d’un article](../../../relational-databases/replication/publish/view-and-modify-article-properties.md)  
  
-   [Supprimer une publication](../../../relational-databases/replication/publish/delete-a-publication.md)  
  
-   [Supprimer un article](../../../relational-databases/replication/publish/delete-an-article.md)  
  
-   [Créer une publication à partir d’une base de données Oracle](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md)  
  
-   [Définir la période d’expiration des abonnements](../../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md)  
  
-   [Spécifier des options de schéma](../../../relational-databases/replication/publish/specify-schema-options.md)  
  
-   [Répliquer les modifications de schéma](../../../relational-databases/replication/publish/replicate-schema-changes.md)  
  
-   [Gérer des colonnes d’identité](../../../relational-databases/replication/publish/manage-identity-columns.md)  
  
-   [Définir le niveau de compatibilité pour les publications de fusion](../../../relational-databases/replication/publish/set-the-compatibility-level-for-merge-publications.md)  
  
### <a name="snapshot-options"></a>Options d'instantané  
  
-   [Configurer les propriétés d’instantané &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/publish/configure-snapshot-properties-replication-transact-sql-programming.md)  
  
-   [Remettre un instantané via FTP](../../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md)  
  
### <a name="filtering-data"></a>Filtrage des données  
  
-   [Définir et modifier un filtre de colonne](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)  
  
-   [Définir et modifier un filtre de lignes statiques](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)  
  
-   [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)  
  
-   [Optimiser les filtres de lignes paramétrables](../../../relational-databases/replication/publish/optimize-parameterized-row-filters.md)  
  
-   [Définir et modifier un filtre de jointure entre des articles de fusion](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)  
  
### <a name="transactional-replication-options"></a>Options de la réplication transactionnelle  
  
-   [Définir la méthode de propagation des modifications de données des articles transactionnels](../../../relational-databases/replication/publish/set-the-propagation-method-for-data-changes-to-transactional-articles.md)  
  
-   [Activer les abonnements de mise à jour pour les publications transactionnelles](../../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
### <a name="merge-replication-options"></a>Options de la réplication de fusion  
  
-   [Définir une relation d’enregistrement logique entre des articles de table de fusion](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md)  
  
-   [Spécifier l’ordre de traitement d’articles de table de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/publish/specify-the-processing-order-of-merge-table-articles.md)  
  
-   [Spécifier qu’un article de table de fusion est en téléchargement seul](../../../relational-databases/replication/publish/specify-that-a-merge-table-article-is-download-only.md)  
  
-   [Indiquer que les suppressions ne doivent pas être suivies pour les articles de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/publish/specify-that-deletes-should-not-be-tracked-for-merge-articles.md)  
  
-   [Spécifier le niveau de suivi et de résolution des conflits pour des articles de fusion](../../../relational-databases/replication/publish/specify-the-conflict-tracking-and-resolution-level-for-merge-articles.md)  
  
-   [Spécifier un programme de résolution d’articles de fusion](../../../relational-databases/replication/publish/specify-a-merge-article-resolver.md)  
  
-   [Spécifier la résolution interactive des conflits pour les articles de fusion](../../../relational-databases/replication/publish/specify-interactive-conflict-resolution-for-merge-articles.md)  
  
## <a name="creating-modifying-and-deleting-subscriptions"></a>Création, modification et suppression d'abonnements  
  
-   [Créer un abonnement par extraction](../../../relational-databases/replication/create-a-pull-subscription.md)  
  
-   [Afficher et modifier les propriétés d’un abonnement par extraction](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  
  
-   [Supprimer un abonnement par extraction (pull)](../../../relational-databases/replication/delete-a-pull-subscription.md)  
  
-   [Créer un abonnement par émission de données](../../../relational-databases/replication/create-a-push-subscription.md)  
  
-   [Afficher et modifier les propriétés d’un abonnement par émission de données](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md)  
  
-   [Supprimer un abonnement par émission de données](../../../relational-databases/replication/delete-a-push-subscription.md)  
  
-   [Spécifier des planifications de synchronisation](../../../relational-databases/replication/specify-synchronization-schedules.md)  
  
-   [Créer un abonnement pouvant être mis à jour pour une publication transactionnelle](../../../relational-databases/replication/publish/create-updatable-subscription-to-transactional-publication.md)
  
-   [Créer un abonnement pour un abonné non-SQL Server](../../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)  
  
## <a name="synchronizing-subscriptions"></a>Synchronisation d'abonnements  
  
-   [Créer et appliquer l'instantané initial](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)  
  
-   [Créer un instantané d’une publication de fusion avec des filtres paramétrés](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  
  
-   [Initialiser un abonnement transactionnel à partir d’une sauvegarde &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/initialize-a-transactional-subscription-from-a-backup.md)  
  
-   [Initialiser manuellement un abonnement](../../../relational-databases/replication/initialize-a-subscription-manually.md)  
  
-   [Synchroniser un abonnement par extraction (pull)](../../../relational-databases/replication/synchronize-a-pull-subscription.md)  
  
-   [Synchroniser un abonnement par émission (push)](../../../relational-databases/replication/synchronize-a-push-subscription.md)  
  
-   [Réinitialiser un abonnement](../../../relational-databases/replication/reinitialize-a-subscription.md)  
  
-   [Exécuter des scripts pendant la synchronisation &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md)  
  
-   [Implémenter un gestionnaire de logique métier pour un article de fusion](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)  
  
-   [Déboguer un gestionnaire de logique métier &#40;programmation de la réplication&#41;](../../../relational-databases/replication/debug-a-business-logic-handler-replication-programming.md)  
  
-   [Contrôler le comportement des déclencheurs et des contraintes pendant la synchronisation &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md)  
  
-   [Implémenter un outil personnalisé de résolution des conflits pour un article de fusion](../../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)  
  
## <a name="administering-a-replication-topology"></a>Administration d'une topologie de réplication  
  
-   [Utiliser des profils d’agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
-   [Valider des données sur l’abonné](../../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
-   [Gérer les partitions d’une publication de fusion avec des filtres paramétrables](../../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)  
  
-   [Charger en masse des données dans les tables d’une publication de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/bulk-load-data-into-tables-in-a-merge-publication.md)  
  
-   [Nettoyer les métadonnées de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/clean-up-merge-metadata-replication-transact-sql-programming.md)  
  
-   [Exécuter une mise à jour factice pour un article de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/perform-a-dummy-update-for-a-merge-article-replication-transact-sql-programming.md)  
  
-   [Afficher les commandes répliquées et autres informations dans la base de données de distribution &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md)  
  
-   [Activer les sauvegardes coordonnées pour la réplication transactionnelle &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/enable-coordinated-backups-for-transactional-replication.md)  
  
-   [Administrer une topologie d’égal à égal &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/administer-a-peer-to-peer-topology-replication-transact-sql-programming.md)  
  
-   [Suspendre une topologie de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md)  
  
-   [Configurer le travail d’un jeu de transactions pour un serveur de publication Oracle &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)  
  
-   [Mettre à niveau les scripts de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md)  
  
## <a name="monitoring-a-replication-topology"></a>Analyse d'une topologie de réplication  
  
-   [Autoriser des non-administrateurs à utiliser le moniteur de réplication](../../../relational-databases/replication/monitor/allow-non-administrators-to-use-replication-monitor.md)  
  
-   [Surveiller la réplication par programmation](../../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
-   [Afficher les commandes répliquées et autres informations dans la base de données de distribution &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md)  
  
-   [Afficher les informations relatives aux conflits pour les publications de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/view-conflict-information-for-merge-publications.md)  
  
-   [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
  
