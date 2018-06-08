---
title: Comment le magasin de requêtes collecte les données | Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Query Store, data collection
ms.assetid: 8d5eec36-0013-480a-9c11-183e162e4c8e
caps.latest.revision: 10
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c3e839b220bb8a3464d8dfbc9a7f4afa8bbcb416
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="how-query-store-collects-data"></a>Comment le magasin de requêtes collecte les données
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Le magasin de requêtes fonctionne comme un **enregistreur de données de vol** . Il collecte constamment des informations de compilation et d'exécution relatives aux requêtes et aux plans. Les données relatives aux requêtes sont conservées dans les tables internes et présentées aux utilisateurs selon différents affichages.  
  
## <a name="views"></a>Vues  
 Le diagramme suivant montre les affichages du magasin de requêtes et leurs relations logiques avec les informations de compilation présentées sous la forme d'entités bleues :  
  
 ![query-store-process-2views](../../relational-databases/performance/media/query-store-process-2views.png "query-store-process-2views")  
  
 **Description des affichages**  
  
|Affichage|Description|  
|----------|-----------------|  
|**sys.query_store_query_text**|Présente les textes de requêtes uniques exécutés sur la base de données. Les commentaires et les espaces avant et après le texte de requête sont ignorés. Les commentaires et les espaces au milieu du texte ne sont pas ignorés. Chaque instruction du lot génère une entrée de texte de requête distincte.|  
|**sys.query_context_settings**|Présente les combinaisons uniques de plan qui affectent les paramètres selon lequel les requêtes sont exécutées. Un même texte de requête exécuté avec un autre plan qui affecte les paramètres produit une entrée de requête distincte dans le magasin de requêtes car `context_settings_id` fait partie de la clé de requête.|  
|**sys.query_store_query**|Les entrées de requête qui sont suivies et forcées séparément dans le magasin de requêtes. Un même texte de requête peut produire plusieurs entrées de requête s’il est exécuté sous différents paramètres de contexte ou à l’extérieur/à l’intérieur de différents modules [!INCLUDE[tsql](../../includes/tsql-md.md)] (procédures stockées, déclencheurs, etc.).|  
|**sys.query_store_plan**|Présente le plan estimé pour la requête avec les statistiques de compilation. Un plan stocké est équivalent à ce que vous pourriez obtenir avec `SET SHOWPLAN_XML ON`.|  
|**sys.query_store_runtime_stats_interval**|Le magasin de requêtes divise le temps en périodes générées automatiquement (intervalles) et stocke les statistiques agrégées sur cet intervalle pour chaque plan exécuté. La taille de l’intervalle est contrôlée par l’option de configuration Intervalle de collecte des statistiques (dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]) ou `INTERVAL_LENGTH_MINUTES` à l’aide des [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).|  
|**sys.query_store_runtime_stats**|Statistiques d'exécution agrégées pour les plans exécutés. Toutes les métriques capturées sont exprimées sous forme de 4 fonctions statistiques : Moyenne, Minimum, Maximum et Écart type.|  
  
 Pour plus d'informations sur les affichages du magasin de requêtes, consultez la section **Affichages, fonctions et procédures associés** de [Analyse des performances à l'aide du magasin de requêtes](monitoring-performance-by-using-the-query-store.md).  
  
## <a name="query-processing"></a>Traitement des requêtes  
 Le magasin de requêtes interagit avec le pipeline de traitement des requêtes sur les points clés suivants :  
  
1.  Lorsque la requête est compilée pour la première fois, le texte de la requête et le plan initial sont envoyés au magasin de requêtes  
  
2.  Lorsque la requête est recompilée, le plan est mis à jour dans le magasin de requêtes. Si un nouveau plan est créé, le magasin de requêtes ajoute la nouvelle entrée de plan de la requête, en conservant les précédentes ainsi que leurs statistiques d'exécution.  
  
3.  Lors de l'exécution des requêtes, les statistiques d'exécution sont envoyées au magasin de requêtes. Le magasin de requêtes conserve des statistiques agrégées précises pour chaque plan exécuté dans l'intervalle actuellement actif.  
  
4.  Lors des phases de compilation et de vérification pour recompilation, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détermine s'il existe un plan dans le magasin de requêtes à appliquer pour la requête en cours d'exécution. S'il existe un plan forcé et que le plan dans le cache de procédures est différent du plan forcé, la requête est recompilée, en réalité de la même façon que si l’INDICATEUR PLAN était appliqué à cette requête. Ce processus se produit de façon transparente pour l'application utilisateur.  
  
 Le diagramme suivant illustre les points d'intégration expliqués ci-dessus :  
  
 ![query-store-process-2processor](../../relational-databases/performance/media/query-store-process-2processor.png "query-store-process-2processor")  
  
 Pour réduire la surcharge d’E/S, les nouvelles données sont capturées en mémoire. Les opérations d’écriture sont mises en file d’attente et vidées sur le disque par la suite. Les informations sur la requête et le plan (Plan Store dans le schéma ci-dessous) sont vidées avec une latence minimale. Les statistiques d’exécution (Runtime Stats) sont conservées en mémoire pendant une période définie avec l’option `DATA_FLUSH_INTERVAL_SECONDS` de l’instruction `SET QUERY_STORE` . La boîte de dialogue Magasin des requêtes de SSMS vous permet d’entrer **l’intervalle de vidage des données (en minutes)**, qui est converti en secondes.  
  
 ![query-store-process-3plan](../../relational-databases/performance/media/query-store-process-3.png "query-store-process-3plan")  
  
 Dans le cas d'un incident du système, le magasin de requêtes peut perdre des données d’exécution jusqu'à la quantité définie avec `DATA_FLUSH_INTERVAL_SECONDS`. La valeur par défaut de 900 secondes (15 minutes) représente un équilibre optimal entre les performances de capture de requête et la disponibilité des données.  
En cas de sollicitation de la mémoire, les statistiques d'exécution peuvent être vidées sur le disque plus tôt que ce qui est défini avec `DATA_FLUSH_INTERVAL_SECONDS`.  
Lors de la lecture des données du magasin de requêtes, les données en mémoire et sur disque sont unifiées en toute transparence.
En cas d’interruption de la session ou de blocage/redémarrage de l’application cliente, les statistiques sur les requêtes ne sont pas enregistrées.  
  
 ![query-store-process-4planinfo](../../relational-databases/performance/media/query-store-process-4planinfo.png "query-store-process-4planinfo")    

  
## <a name="see-also"></a> Voir aussi  
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Bonnes pratiques relatives au magasin de requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md)   
 [Affichages catalogue du magasin de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)  
  
  
