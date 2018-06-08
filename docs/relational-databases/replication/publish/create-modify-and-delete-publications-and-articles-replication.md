---
title: Créer, modifier et supprimer des publications et des articles (réplication) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
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
- publications [SQL Server replication], creating
- articles [SQL Server replication], defining
ms.assetid: e66d06ec-a12b-444d-875b-77f958af2f21
caps.latest.revision: 36
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 2f5d375a46a4e158311e5005dc8bf7c3b2627bc1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-modify-and-delete-publications-and-articles-replication"></a>Créer, modifier et supprimer des publications et des articles (réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette section de la documentation contient des informations procédurales sur les tâches liées à la création de publications et à la définition d'articles.  
  
## <a name="in-this-section"></a>Dans cette section  
  
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
  
## <a name="snapshot-options"></a>Options d'instantané  
  
-   [Spécifier le format d’instantané &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/specify-snapshot-format-sql-server-management-studio.md)  
  
-   [Spécifier un autre emplacement de dossier d’instantané &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/specify-an-alternate-snapshot-folder-location-sql-server-management-studio.md)  
  
-   [Compresser des fichiers d’instantanés &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/compress-snapshot-files-sql-server-management-studio.md)  
  
-   [Configurer les propriétés d’instantané &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/publish/configure-snapshot-properties-replication-transact-sql-programming.md)  
  
-   [Exécuter des scripts avant et après l’application d’un instantané &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/execute-scripts-before-and-after-a-snapshot-is-applied.md)  
  
-   [Remettre un instantané via FTP](../../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md)  
  
## <a name="filtering-data"></a>Filtrage des données  
  
-   [Définir et modifier un filtre de colonne](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)  
  
-   [Définir et modifier un filtre de lignes statiques](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)  
  
-   [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)  
  
-   [Optimiser les filtres de lignes paramétrables](../../../relational-databases/replication/publish/optimize-parameterized-row-filters.md)  
  
-   [Définir et modifier un filtre de jointure entre des articles de fusion](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)  
  
-   [Générer automatiquement un ensemble de filtres de jointure entre des articles de fusion &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md)  
  
## <a name="transactional-replication-options"></a>Options de la réplication transactionnelle  
  
-   [Définir la méthode de propagation des modifications de données des articles transactionnels](../../../relational-databases/replication/publish/set-the-propagation-method-for-data-changes-to-transactional-articles.md)  
  
-   [Activer les abonnements pouvant être mis à jour pour les publications transactionnelles](../../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
-   [Définir des options de résolution des conflits de mise à jour en attente &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/set-queued-updating-conflict-resolution-options-sql-server-management-studio.md)  
  
-   [Publier l’exécution d’une procédure stockée dans une publication transactionnelle &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/publish-execution-of-stored-procedure-in-transactional-publication.md)  
  
## <a name="merge-replication-options"></a>Options de la réplication de fusion  
  
-   [Définir une relation d’enregistrement logique entre des articles de table de fusion](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md)  
  
-   [Spécifier l’ordre de traitement d’articles de table de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/publish/specify-the-processing-order-of-merge-table-articles.md)  
  
-   [Spécifier qu’un article de table de fusion est en téléchargement seul](../../../relational-databases/replication/publish/specify-that-a-merge-table-article-is-download-only.md)  
  
-   [Indiquer que les suppressions ne doivent pas être suivies pour les articles de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/publish/specify-that-deletes-should-not-be-tracked-for-merge-articles.md)  
  
-   [Spécifier le niveau de suivi et de résolution des conflits pour des articles de fusion](../../../relational-databases/replication/publish/specify-the-conflict-tracking-and-resolution-level-for-merge-articles.md)  
  
-   [Spécifier un programme de résolution d’articles de fusion](../../../relational-databases/replication/publish/specify-a-merge-article-resolver.md)  
  
-   [Spécifier la résolution interactive des conflits pour les articles de fusion](../../../relational-databases/replication/publish/specify-interactive-conflict-resolution-for-merge-articles.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
