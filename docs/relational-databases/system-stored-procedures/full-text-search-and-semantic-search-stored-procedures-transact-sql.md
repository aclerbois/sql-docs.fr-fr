---
title: Recherche en texte intégral et la recherche sémantique (Transact-SQL) de procédures stockées | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], stored procedures
- full-text search [SQL Server], stored procedures
- full-text catalogs [SQL Server], stored procedures
- system stored procedures [SQL Server], full-text search
ms.assetid: 0d185a16-2b16-4958-884f-efe675e2e551
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 735006f7e640bd6b35e57aafa2b0e70cb613e544
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="full-text-search-and-semantic-search-stored-procedures-transact-sql"></a>Procédures stockées de recherche en texte intégral et de recherche sémantique (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les procédures stockées système suivantes qui sont utilisées pour implémenter et interroger des index de recherche en texte intégral et sémantique.  
  
## <a name="full-text-search-stored-procedures"></a>Procédures stockées de recherche en texte intégral  
 [sp_fulltext_catalog](../../relational-databases/system-stored-procedures/sp-fulltext-catalog-transact-sql.md)  
 Permet de créer ou de supprimer un catalogue de texte intégral, et de démarrer ou d'arrêter l'indexation d'un catalogue. Plusieurs catalogues de texte intégral peuvent être créés pour chaque base de données.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez [CREATE FULLTEXT CATALOG](../../t-sql/statements/create-fulltext-catalog-transact-sql.md), [ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md), et [DROP FULLTEXT CATALOG](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md) à la place.  
  
 [sp_fulltext_column](../../relational-databases/system-stored-procedures/sp-fulltext-column-transact-sql.md)  
 Spécifie si une colonne particulière d'une table est utilisée dans l'indexation de texte intégral.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) à la place.  
  
 [sp_fulltext_database](../../relational-databases/system-stored-procedures/sp-fulltext-database-transact-sql.md)  
 N'a aucun effet sur les catalogues de texte intégral dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures et est pris en charge uniquement à des fins de compatibilité descendante.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)  
 Retourne les mappages entre identificateurs de document (**DocId**s) et les valeurs de clé de recherche en texte intégral.  
  
 [sp_fulltext_load_thesaurus_file](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)  
 Analyse et charge les données à partir d'un fichier de dictionnaire des synonymes mis à jour qui correspond à un LCID et provoque la recompilation des requêtes de texte intégral qui utilisent le dictionnaire des synonymes.  
  
 [sp_fulltext_pendingchanges](../../relational-databases/system-stored-procedures/sp-fulltext-pendingchanges-transact-sql.md)  
 Retourne les modifications non traitées (par exemple les insertions, mises à jour et suppressions en attente) pour une table spécifiée qui utilise le suivi des modifications.  
  
 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
 Modifie les propriétés serveur de la recherche en texte intégral pour SQL Server.  
  
 [sp_fulltext_table](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)  
 Active ou désactive l'indexation de texte intégral pour une table.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md), et [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) à la place.  
  
 [sp_help_fulltext_catalog_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalog-components-transact-sql.md)  
 Retourne la liste de tous les composants (filtres, analyseurs lexicaux et gestionnaires de protocoles) utilisés pour tous les catalogues de texte intégral dans la base de données active.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 [sp_help_fulltext_catalogs](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
 Retourne l'identificateur, le nom, le répertoire racine, l'état et le nombre de tables indexées en texte intégral du catalogue de texte intégral spécifié.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez le [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) affichage du catalogue à la place.  
  
 [sp_help_fulltext_catalogs_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)  
 Utilise un curseur pour retourner l'ID, le nom, le répertoire racine, l'état et le nombre de tables indexées en texte intégral du catalogue de texte intégral spécifié.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez le [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) affichage du catalogue à la place.  
  
 [sp_help_fulltext_columns](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)  
 Renvoie les colonnes désignées pour l'indexation de texte intégral.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez le [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) affichage du catalogue à la place.  
  
 [sp_help_fulltext_columns_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-cursor-transact-sql.md)  
 Utilise un curseur pour retourner les colonnes marquées pour l'indexation de texte intégral.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez le [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) affichage du catalogue à la place.  
  
 [sp_help_fulltext_system_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)  
 Retourne des informations sur les analyseurs lexicaux, le filtre et les gestionnaires de protocoles, ainsi qu'une liste des identificateurs de bases de données et de catalogues de texte intégral qui ont utilisé un composant spécifié.  
  
 [sp_help_fulltext_tables](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)  
 Affiche la liste des tables qui sont inscrites pour l'indexation de texte intégral.  
  
 [sp_help_fulltext_tables_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)  
 Affiche la liste des tables qui sont inscrites pour l'indexation de texte intégral.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez le [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) affichage du catalogue à la place.  
  
## <a name="semantic-search-stored-procedures"></a>Procédures stockées de recherche sémantique  
 [sp_fulltext_semantic_register_language_statistics_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-register-language-statistics-db-transact-sql.md)  
 Inscrit une base de données de statistiques linguistiques de sémantique pré-remplie dans l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [sp_fulltext_semantic_unregister_language_statistics_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-unregister-language-statistics-db-transact-sql.md)  
 Annule l'inscription d'une base de données de statistiques linguistiques de sémantique existante de l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et supprime toutes les métadonnées associées.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de recherche en texte intégral et la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/full-text-search-and-semantic-search-catalog-views-transact-sql.md)   
 [Recherche en texte intégral et les fonctions et vues de gestion dynamique de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)  
  
  
