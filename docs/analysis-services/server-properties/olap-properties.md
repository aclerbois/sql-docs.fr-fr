---
title: Propriétés OLAP | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5739c93f7a3c20960f5470c3fd2cdb24c72cf09d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="olap-properties"></a>Propriétés OLAP
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge les propriétés de serveur OLAP répertoriées dans les tableaux suivants. Pour plus d’informations sur les autres propriétés de serveur et sur la façon de les configurer, consultez [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md).  
  
 **S'applique à :** Mode serveur multidimensionnel uniquement  
  
## <a name="memory"></a>Mémoire  
 **DefaultPageSizeForData**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DefaultPageSizeForDataHeader**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DefaultPageSizeForIndex**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DefaultPageSizeForIndexHeader**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DefaultPageSizeForString**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DefaultPageSizeForHash**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DefaultPageSizeForProp**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="lazyprocessing"></a>LazyProcessing  
 **Activé**  
 Propriété booléenne qui spécifie si le traitement différé des agrégations est activé.  
  
 **SleepIntervalSecs**  
 Propriété dont la valeur est un entier 32 bits signé qui définit l'intervalle, en secondes, en fonction duquel le serveur vérifie s'il y a des travaux de traitement différé en attente.  
  
 **MaxCPUUsage**  
 Propriété dont la valeur est un nombre 64 bits signé en virgule flottante double précision qui définit sous la forme d'un pourcentage l'utilisation maximale de l'unité centrale pour les traitements différés. Le serveur surveille l'utilisation moyenne de l'unité centrale en prenant des instantanés à intervalles réguliers. Il est normal que l'unité centrale dépasse temporairement ce seuil lors des pointes d'activité.  
  
 La valeur par défaut de cette propriété, 0,5, indique qu'un maximum de 50 % de l'unité centrale sera consacré au traitement différé.  
  
 **MaxObjectsInParallel**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie le nombre maximal de partitions pouvant faire l'objet d'un traitement différé en parallèle.  
  
 **MaxRetries**  
 Propriété dont la valeur est un entier 32 bits signé qui définit le nombre de nouvelles tentatives en cas d'échec du traitement en différé avant qu'une erreur ne soit déclenchée.  
  
## <a name="processplan"></a>ProcessPlan  
 **CacheRowsetRows**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **CacheRowsetToDisk**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DistinctBuffer**  
 Propriété dont la valeur est un entier 32 bits signé qui définit la taille d'une mémoire tampon interne utilisée pour les comptages de valeurs. Augmentez cette valeur si vous souhaitez accélérer le traitement du comptage de valeurs au prix d'une plus grande consommation de mémoire.  
  
 **EnableRolapDimQueryTableGrouping**  
 Propriété booléenne qui spécifie si le regroupement de tables est activé pour les dimensions ROLAP. Si sa valeur est True, lors de l'extraction des dimensions ROLAP au moment de l'exécution, les tables de dimension ROLAP sont consultées en entier immédiatement, au lieu d'utiliser des requêtes distinctes pour chaque attribut.  
  
 **EnableTableGrouping**  
 Propriété booléenne qui spécifie si le regroupement de tables est activé. Si sa valeur est True, lors du traitement des dimensions, les tables de dimension sont consultées en entier immédiatement, au lieu d'utiliser des requêtes distinctes pour chaque attribut.  
  
 **ForceMultiPass**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **MaxTableDepth**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **MemoryAdjustConst**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **MemoryAdjustFactor**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **MemoryLimit**  
 Propriété dont la valeur est un nombre 64 bits signé en virgule flottante double précision qui définit la capacité de mémoire maximale à consacrer au traitement, exprimée en pourcentage de la mémoire physique.  
  
 La valeur par défaut de cette propriété, 65, indique que 65 % de la mémoire physique peut être consacrée au traitement des cubes et des dimensions.  
  
 **MemoryLimitErrorEnabled**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **OptimizeSchema**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="proactivecaching"></a>ProactiveCaching  
 **DefaultRefreshInterval**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DimensionLatencyAccuracy**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **PartitionLatencyAccuracy**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="process"></a>Traiter  
 **AggregationMemoryLimitMax**  
 Propriété dont la valeur est un nombre 64 bits signé en virgule flottante double précision qui définit la capacité de mémoire maximale pouvant être consacrée au traitement des agrégations, exprimée en pourcentage de la mémoire physique.  
  
 La valeur par défaut de cette propriété, 80, indique que 80 % de la mémoire physique peut être consacrée au traitement des agrégations.  
  
 **AggregationMemoryLimitMin**  
 Propriété dont la valeur est un nombre 64 bits signé en virgule flottante double précision qui définit la capacité de mémoire minimale pouvant être consacrée au traitement des agrégations, exprimée en pourcentage de la mémoire physique. Une valeur supérieure est susceptible d'accélérer le traitement des agrégations au prix d'une plus grande consommation de mémoire.  
  
 La valeur par défaut de cette propriété, 10, indique qu'un minimum de 10 % de la mémoire physique sera consacré au traitement des agrégations.  
  
 **AggregationNewAlgo**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **AggregationPerfLog2**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **AggregationsBuildEnabled**  
 Propriété booléenne qui spécifie si la génération d'agrégations est activée. Il s'agit d'un mécanisme permettant de tester les performances de la génération d'agrégations sans modifier la conception des agrégations.  
  
 **BufferMemoryLimit**  
 Propriété dont la valeur est un nombre 64 bits signé en virgule flottante double précision qui définit la limite de la mémoire tampon de traitement, exprimée en pourcentage de la mémoire physique.  
  
 La valeur par défaut de cette propriété, 60, indique qu'il est possible d'utiliser jusqu'à 60 % de la mémoire physique comme mémoire tampon.  
  
 **BufferRecordLimit**  
 Propriété dont la valeur est un entier 32 bits signé qui définit le nombre d'enregistrements qu'il est possible de stocker en mémoire tampon durant le traitement.  
  
 La valeur par défaut de cette propriété est 1 048 576 (enregistrements).  
  
 **CacheRecordLimit**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **CheckDistinctRecordSortOrder**  
 Propriété booléenne qui définit si l'ordre de tri des résultats d'une requête de comptage de valeurs est significatif lors du traitement de partitions. La valeur True indique que l'ordre de tri n'est pas significatif et doit être « vérifié » par le serveur. Lors du traitement des partitions avec une mesure de comptage de valeurs, une requête comportant une clause ORDER BY est envoyée à SQL Server. Attribuez la valeur False à cette propriété si vous souhaitez accélérer le traitement.  
  
 La valeur par défaut de cette propriété, True, indique que l'ordre de tri n'est pas significatif et doit être vérifié.  
  
 **DatabaseConnectionPoolConnectTimeout**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie le délai d'attente en secondes lors de l'ouverture d'une nouvelle connexion.  
  
 **DatabaseConnectionPoolGeneralTimeout**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie le délai de connexion de base de données en secondes à utiliser avec les connexions OLEDB externes.  
  
 **DatabaseConnectionPoolMax**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie le nombre maximal de connexions de base de données regroupées en pool.  
  
 La valeur par défaut de cette propriété est 50 (connexions).  
  
 **DatabaseConnectionPoolTimeout**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataFileInitEnabled**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataPlacementOptimization**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataSliceInitEnabled**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DeepCompressValue**  
 Propriété booléenne s'appliquant aux mesures avec le type de données Double qui spécifie si les nombres peuvent être compressés, ce qui entraîne une perte de précision numérique. La valeur False indique l'absence de compression et la conservation de la précision.  
  
 La valeur par défaut de cette propriété, True, indique que la compression est activée au détriment de la précision.  
  
 **DimensionPropertyKeyCache**  
 Propriété booléenne qui spécifie si les clés de propriété de dimension sont stockées en cache. Cette propriété doit avoir la valeur True si les clés ne sont pas uniques.  
  
 **IndexBuildEnabled**  
 Propriété booléenne qui spécifie si les index sont générés lors du traitement. Cette propriété sert à tester les performances et à informer.  
  
 **IndexBuildThreshold**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie un seuil, exprimé en nombre de lignes, en dessous duquel les index ne seront pas générés pour les partitions.  
  
 La valeur par défaut de cette propriété est 4 096 (lignes).  
  
 **IndexFileInitEnabled**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **MapFormatMask**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **RecordsReportGranularity**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie la périodicité, exprimée en nombre de lignes, selon laquelle le serveur enregistre les événements Trace durant le traitement.  
  
 La valeur par défaut de cette propriété, 1 000, indique qu'un événement Trace est enregistré toutes les 1 000 lignes.  
  
 **ROLAPDimensionProcessingEffort**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="query"></a>Requête  
 **AggregationsUseEnabled**  
 Propriété booléenne qui définit si les agrégations stockées sont utilisées au moment de l'exécution. Cette propriété permet de désactiver les agrégations sans qu'il soit nécessaire de modifier leur conception ou de les retraiter, par exemple dans un but informatif ou pour tester les performances.  
  
 La valeur par défaut de cette propriété, True, indique que les agrégations sont activées.  
  
 **AllowSEFiltering**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **CalculationCacheRegistryMaxIterations**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **CalculationEvaluationPolicy**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ConvertDeletedToUnknown**  
 Propriété booléenne qui spécifie si les membres de dimension supprimés sont convertis en membres inconnus.  
  
 **CopyLinkedDataCacheAndRegistry**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCacheRegistryMaxIterations**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DefaultDrillthroughMaxRows**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie le nombre maximal de lignes qui seront renvoyées par une requête qui effectue une extraction.  
  
 La valeur par défaut de cette propriété est 10 000 (lignes).  
  
 **DimensionPropertyCacheSize**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie la quantité de mémoire (en octets) utilisée pour mettre en cache les membres de dimension utilisés dans une requête.  
  
 La valeur par défaut est 4 000 000 octets (ou 4 Mo) par hiérarchie d'attribut, par requête active. La valeur par défaut fournit une taille de cache bien équilibrée pour les solutions comportant des hiérarchies standard. Toutefois, les dimensions avec un grand nombre de membres (des millions) ou des hiérarchies profondes offrent de meilleures performances si vous augmentez cette valeur.  
  
 Impact de l'augmentation de la taille du cache :  
  
-   Les coûts d'utilisation de la mémoire augmentent lorsque vous autorisez l'utilisation de davantage de mémoire par le cache de dimension. L'utilisation réelle dépend de l'exécution de la requête. Certaines requêtes n'utilisent pas la quantité maximale autorisée.  
  
     Notez que la mémoire utilisée par ces caches est considérée comme non réductible et est incluse dans la propriété **TotalMemoryLimit**.  
  
-   Affecte toutes les bases de données sur le serveur. **DimensionPropertyCachesize** est une propriété à l’échelle du serveur. La modification de cette propriété affecte toutes les bases de données exécutées sur l'instance actuelle.  
  
 Approche pour estimer la configuration requise du cache de dimension :  
  
1.  Commencez par augmenter la taille en entrant un grand nombre pour déterminer si l'augmentation de la taille du cache de dimension présente un avantage. Par exemple, doublez la valeur par défaut lors de l'étape initiale.  
  
2.  Si une amélioration des performances est visible, réduisez la valeur de manière incrémentielle jusqu'à ce qu'un équilibre entre les performances et l'utilisation de la mémoire soit atteint.  
  
 **ExpressNonEmptyUseEnabled**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **IgnoreNullRolapRows**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **IndexUseEnabled**  
 Propriété booléenne qui définit si les index sont utilisés au moment de l'exécution. Cette propriété sert à tester les performances et à informer.  
  
 **MapHandleAlgorithm**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **MaxRolapOrConditions**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **UseCalculationCacheRegistry**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **UseDataCacheFreeLastPageMemory**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **UseDataCacheRegistry**  
 Propriété booléenne qui spécifie s'il faut activer le registre du cache des données, où les résultats (non calculés) des requêtes sont mis en cache.  
  
 **UseDataCacheRegistryHashTable**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **UseDataCacheRegistryMultiplyKey**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **UseDataSlice**  
 Propriété booléenne qui définit si les tranches de données de partition sont utilisées au moment de l'exécution pour l'optimisation des requêtes. Cette propriété sert à tester les performances et à informer.  
  
 **UseMaterializedIterators**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **UseSinglePassForDimSecurityAutoExist**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **UseVBANet**  
 Propriété booléenne qui définit si l'assembly .net VBA doit être utilisé pour les fonctions définies par l'utilisateur.  
  
 **CalculationPrefetchLocality\ ApplyIntersect**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **CalculationPrefetchLocality\ ApplySubtract**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **CalculationPrefetchLocality\ PrefetchLowerGranularities**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\  CachedPageAlloc\ Income**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\  CachedPageAlloc\ InitialBonus**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\  CachedPageAlloc\ MaximumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\  CachedPageAlloc\ MinimumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\  CachedPageAlloc\ Tax**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\CellStore\ Income**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\CellStore\ InitialBonus**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\CellStore\ MaximumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\CellStore\ MinimumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\CellStore\ Tax**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\ MemoryModel \ Income**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\ MemoryModel \ InitialBonus**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\ MemoryModel \ MaximumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\ MemoryModel \ MinimumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **DataCache\ MemoryModel\ Tax**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="jobs"></a>Travaux  
 **ProcessAggregation\ MemoryModel\ Income**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ MemoryModel\ InitialBonus**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ MemoryModel\ MaximumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ MemoryModel\ MinimumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ MemoryModel\ Tax**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessPartition\ Income**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessPartition \ InitialBonus**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessPartition \ MaximumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessPartition \ MinimumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessPartition \ Tax**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessProperty\ Income**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessProperty\ InitialBonus**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessProperty\ MaximumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessProperty\ MinimumBalance**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **ProcessAggregation\ ProcessProperty\ Tax**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md)   
 [Déterminer le mode serveur d’une instance Analysis Services](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md)  
  
  
