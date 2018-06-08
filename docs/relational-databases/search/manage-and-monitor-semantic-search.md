---
title: Gérer et surveiller la recherche sémantique | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- semantic search [SQL Server], managing
- semantic search [SQL Server], monitoring
ms.assetid: eb5c3b29-da70-42aa-aa97-7d35a3f1eb98
caps.latest.revision: 19
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 658318886e102d8d68fd5fc2d15e8b84f78dd9de
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="manage-and-monitor-semantic-search"></a>Gérer et surveiller la recherche sémantique
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Décrit le processus d'indexation sémantique et les tâches associées à la gestion et au contrôle des index.  
  
##  <a name="HowToMonitorStatus"></a> Vérifier l’état de l’indexation sémantique  
### <a name="is-the-first-phase-of-semantic-indexing-complete"></a>La première phase de l'indexation sémantique est-elle achevée ?
 Interrogez la vue de gestion dynamique, [sys.dm_fts_index_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md), et vérifiez les colonnes **status** et **status_description**.  
  
 La première phase de l'indexation inclut l'alimentation de l'index de mots clés de recherche en texte intégral et de l'index d'expressions clés sémantiques, ainsi que l'extraction de données de ressemblance de document.  
  
```sql  
USE database_name  
GO  
  
SELECT * FROM sys.dm_fts_index_population WHERE table_id = OBJECT_ID('table_name')  
GO  
```  
  
### <a name="is-the-second-phase-of-semantic-indexing-complete"></a>La seconde phase de l'indexation sémantique est-elle achevée ?
 Interrogez la vue de gestion dynamique, [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md), et vérifiez les colonnes **status** et **status_description**.  
  
 La deuxième phase de l'indexation inclut l'alimentation de l'index de ressemblance de document sémantique.  
  
```wql  
USE database_name  
GO  
  
SELECT * FROM sys.dm_fts_semantic_similarity_population WHERE table_id = OBJECT_ID('table_name')  
GO  
```  
  
##  <a name="HowToCheckSize"></a> Vérifier la taille des index sémantiques  
### <a name="what-is-the-logical-size-of-a-semantic-key-phrase-index-or-a-semantic-document-similarity-index"></a>Quelle est la taille logique d'un index d'expressions clés sémantiques ou d'un index de ressemblance de document sémantique ?
 Interrogez la vue de gestion dynamique, [sys.dm_db_fts_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-fts-index-physical-stats-transact-sql.md).  
  
 La taille logique est affichée en nombre de pages d'index.  
  
```sql  
USE database_name  
GO  
  
SELECT * FROM sys.dm_db_fts_index_physical_stats WHERE object_id = OBJECT_ID('table_name')  
GO  
```  
  
### <a name="what-is-the-total-size-of-the-full-text-and-semantic-indexes-for-a-full-text-catalog"></a>Quelle est la taille totale des index sémantiques et de recherche en texte intégral pour un catalogue de texte intégral ?  
 Interrogez la propriété **IndexSize** de la fonction de métadonnées [FULLTEXTCATALOGPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md).  
  
```sql  
SELECT FULLTEXTCATALOGPROPERTY('catalog_name', 'IndexSize')  
GO  
```  
  
### <a name="how-many-items-are-indexed-in-the-full-text-and-semantic-indexes-for-a-full-text-catalog"></a>Combien d'éléments sont indexés dans les index sémantiques et de recherche en texte intégral pour un catalogue de texte intégral ?  
 Interrogez la propriété **ItemCount** de la fonction de métadonnées [FULLTEXTCATALOGPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md).  
  
```sql  
SELECT FULLTEXTCATALOGPROPERTY('catalog_name', 'ItemCount')  
GO  
```  
  
##  <a name="HowToForcePopulation"></a> Imposer l’alimentation des index sémantiques  
 Vous pouvez forcer le remplissage des index sémantiques et de recherche en texte intégral à l'aide de la clause START/STOP/PAUSE ou RESUME POPULATION avec la même syntaxe et le même comportement que ceux décrits pour les index de recherche en texte intégral. Pour plus d’informations, consultez [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md) et [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md).  
  
 Étant donné que l'indexation sémantique dépend de l'indexation de texte intégral, les index sémantiques ne sont remplis que lorsque les index de recherche en texte intégral associés le sont également.  
  
 **Exemple : démarrer une alimentation complète des index sémantiques et de recherche en texte intégral**  
  
 L’exemple suivant démarre une alimentation complète des index sémantiques et de recherche en texte intégral en modifiant un index de recherche en texte intégral existant sur la table **Production.Document** dans l’exemple de base de données AdventureWorks2012.  
  
```vb  
USE AdventureWorks2012  
GO  
  
ALTER FULLTEXT INDEX ON Production.Document  
    START FULL POPULATION  
GO  
```  
  
##  <a name="HowToDisableIndexing"></a> Désactiver ou réactiver l’indexation sémantique  
 Vous pouvez activer ou désactiver l'indexation sémantique ou de texte intégral à l'aide de la clause ENABLE/DISABLE avec la même syntaxe et le même comportement que ceux décrits pour les index de recherche en texte intégral. Pour plus d’informations, consultez [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md).  
  
 Lorsque l'indexation sémantique est désactivée et suspendue, les requêtes sur les données sémantiques continuent de s'exécuter avec succès et retournent des données indexées précédemment. Ce comportement n'est pas cohérent avec le comportement de la recherche en texte intégral.  
  
```sql  
-- To disable semantic indexing on a table  
USE database_name  
GO  
  
ALTER FULLTEXT INDEX ON table_name DISABLE  
GO  
  
-- To re-enable semantic indexing on a table  
USE database_name  
GO  
  
ALTER FULLTEXT INDEX ON table_name ENABLE  
GO  
```  
  
##  <a name="SemanticIndexing"></a> À propos des phases d’indexation sémantique  
 Une recherche sémantique indexe deux types de données pour chaque colonne sur laquelle elle est activée :  
  
1.  **Expressions clés**  
  
2.  **Ressemblance de document**  
  
 L'indexation sémantique se produit en deux phases, conjointement à l'indexation de texte intégral :  
  
1.  **Phase 1**. L'index de mots clés de texte intégral et l'index d'expressions clés sémantiques sont remplis en même temps, en parallèle. Les données requises pour indexer la ressemblance de document sont également extraites à ce moment.  
  
2.  **Phase 2**. L'index de ressemblance de document sémantique est rempli à son tour. Cet index dépend des deux index remplis à la phase précédente.  
  
##  <a name="BestPracticeUnderstand"></a>   
##  <a name="ProblemNotPopulated"></a> Problème : les index sémantiques ne sont pas alimentés  
### <a name="are-the-associated-full-text-indexes-populated"></a>Les index de recherche en texte intégral associés sont-ils remplis ?  
 Étant donné que l'indexation sémantique dépend de l'indexation de texte intégral, les index sémantiques ne sont remplis que lorsque les index de recherche en texte intégral associés le sont également.  
  
### <a name="are-full-text-search-and-semantic-search-properly-installed-and-configured"></a>La recherche en texte intégral et la recherche sémantique sont-elles installées et configurées correctement ?  
 Pour plus d’informations, consultez [Installer et configurer la recherche sémantique](../../relational-databases/search/install-and-configure-semantic-search.md).  
  
### <a name="is-the-fdhost-service-not-available-or-is-there-another-condition-that-would-cause-full-text-indexing-to-fail"></a>Le service FDHOST est-il indisponible, ou existe-t-il une autre condition qui provoquerait l'échec de l'indexation de texte intégral ?  
 Pour plus d’informations, consultez [Résoudre l’indexation de recherche en texte intégral](../../relational-databases/search/troubleshoot-full-text-indexing.md).  
  
  
