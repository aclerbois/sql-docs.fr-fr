---
title: Améliorer les performances des requêtes de texte intégral | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 0658dc74-25eb-4486-bbd6-e85c1f92c272
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 26267a9c302e7bdc8ed833d19e026a8a76e2e977
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="improve-the-performance-of-full-text-queries"></a>Améliorer les performances des requêtes de texte intégral
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La liste suivante comprend des recommandations destinées à améliorer les performances des requêtes de texte intégral.  
  
 Les performances des requêtes de texte intégral sont également influencées par les ressources matérielles telles que la mémoire, la vitesse du disque et du processeur, ainsi que par l'architecture de l'ordinateur.  
  
-   Défragmentez l’index de la table de base à l’aide de [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md).  
  
-   Réorganisez le catalogue de texte intégral à l’aide de [ALTER FULLTEXT CATALOG REORGANIZE](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md). Assurez-vous d'effectuer ces opérations avant de tester les performances car l'exécution de cette instruction entraîne une fusion principale des index de texte intégral dans ce catalogue.  
  
-   Limitez le choix de vos colonnes clés de texte intégral à une petite colonne. Même si une colonne de 900 octets est prise en charge, il est recommandé d'utiliser une colonne clé inférieure à cette taille dans un index de recherche en texte intégral. **int** et **bigint** offrent les meilleures performances.  
  
-   L’utilisation d’une clé de texte intégral de type entier évite une jointure avec la table de mappage **docid** . Par conséquent, une clé de texte intégral de type entier améliore considérablement les performances des requêtes et renforce les performances d'analyse. Des avantages en matière de performances supplémentaires peuvent résulter si la clé de texte intégral est également une clé d'index cluster.  
  
-   Regroupez plusieurs prédicats [CONTAINS](../../t-sql/queries/contains-transact-sql.md) dans un seul prédicat CONTAINS. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez spécifier une liste de colonnes dans la requête CONTAINS.  
  
-   Utilisez [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) ou [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) au lieu de CONTAINS ou FREETEXT respectivement, si vous n’avez besoin que d’informations sur la clé de texte intégral ou le rang.  
  
-   Pour limiter les résultats et augmenter les performances, utilisez le paramètre *top_n_by_rank* des fonctions FREETEXTTABLE et CONTAINSTABLE. *top_n_by_rank* vous permet de rappeler uniquement les accès les plus pertinents. Utilisez ce paramètre uniquement si votre scénario d’entreprise ne nécessite pas de rappeler tous les accès possibles (autrement dit, s’il ne nécessite pas un *rappel total*).  
  
    > [!NOTE]  
    >  Le rappel total est généralement requis pour les scénarios juridiques, mais il peut être moins important que les performances pour les scénarios d'entreprise tels qu'un e-business.  
  
-   Vérifiez le plan de requête de texte intégral pour vous assurer que le plan de jointure adéquat a été sélectionné. Utilisez un indicateur de jointure ou un indicateur de requête si nécessaire. Si un paramètre est utilisé dans la requête de texte intégral, la valeur initiale du paramètre détermine le plan de requête. Vous pouvez utiliser l’ [indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md) OPTIMIZE FOR pour forcer la requête à une compilation avec la valeur souhaitée. Il est ainsi possible d'obtenir un plan de requête déterministe et de meilleures performances.  
  
-   Trop de fragments d'index de recherche en texte intégral dans l'index de texte intégral peut conduire à une dégradation substantielle dans les performances des requêtes. Pour réduire le nombre de fragments, réorganisez le catalogue de texte intégral en utilisant l’option REORGANIZE de l’instruction [ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] . Cette instruction fusionne essentiellement tous les fragments en un plus grand fragment unique et supprime toutes les entrées obsolètes de l'index de recherche en texte intégral.  
  
-   Dans la recherche en texte intégral , les opérateurs logiques spécifiés dans CONTAINSTABLE (AND, OR) peuvent être implémentés en tant que jointures SQL ou bien dans des fonctions table de diffusion d'exécution de texte intégral (STVF). En général, les requêtes avec un seul type d'opérateur logique sont implémentées exclusivement par l'exécution de texte intégral, alors que les requêtes qui combinent des opérateurs logiques possèdent également des jointures SQL. L'implémentation d'un opérateur logique dans la fonction table multi-diffusion d'exécution de texte intégral utilise quelques propriétés d'index spéciales qui la rendent beaucoup plus rapide que les jointures SQL. Pour cette raison, nous recommandons que, dans toute la mesure du possible, vous encadrez des requêtes en utilisant seulement un type unique d'opérateur logique.  
  
-   Pour les applications qui contiennent des prédications de relation sélectives, les requêtes qui utilisent des prédicats relationnels sélectifs et des prédicats de texte intégral non sélectifs peuvent s'exécuter de manière optimale lorsqu'elles sont écrites pour utiliser l'optimiseur de requête. Cela permet à l'optimiseur de requête de déterminer s'il peut exploiter le prédicat ou le menu déroulant de plage pour produire un plan de requête effectif. Cette approche est plus simple et souvent plus efficace que l'indexation des données relationnelles comme données de texte intégral.  
  
## <a name="related-resources"></a>Ressources connexes  
 [SQL Server 2008 Full-Text Search: Internals and Enhancements (en anglais)](http://go.microsoft.com/fwlink/?LinkId=129544)  
  
## <a name="see-also"></a> Voir aussi  
 [sys.dm_fts_memory_buffers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)   
 [sys.dm_fts_memory_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
  
  
