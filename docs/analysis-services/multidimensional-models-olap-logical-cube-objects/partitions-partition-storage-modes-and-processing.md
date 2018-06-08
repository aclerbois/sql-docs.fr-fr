---
title: Modes de stockage et le traitement de partition | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f69a290875de9e210b825a38ba52a271d770ab42
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="partitions---partition-storage-modes-and-processing"></a>Partitions - traitement et Modes de stockage de Partition
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Le mode de stockage d'une partition affecte les performances de traitement et des requêtes, les besoins en espace de stockage, ainsi que les emplacements de stockage de la partition, de son cube et de son groupe de mesures parents. Le mode de stockage a également une incidence sur les options de traitement.  
  
 Une partition peut utiliser l'un des trois modes de stockage de base :  
  
-   OLAP multidimensionnel (MOLAP)  
  
-   OLAP relationnel (ROLAP)  
  
-   Hybrid OLAP (HOLAP)  
  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge trois modes de stockage de base. Il prend également en charge la mise en cache proactive, qui vous permet de combiner les caractéristiques du stockage ROLAP et MOLAP pour bénéficier de la rapidité des données et des performances des requêtes. Pour plus d’informations, consultez [Mise en cache proactive &#40;partitions&#41;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-proactive-caching.md).  
  
## <a name="molap"></a>MOLAP  
 Dans le mode de stockage MOLAP, les agrégations de la partition et une copie de ses données sources sont stockées dans une structure multidimensionnelle dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] lors du traitement de la partition. Cette structure MOLAP est hautement optimisée pour renforcer les performances des requêtes. L'emplacement de stockage peut se trouver sur l'ordinateur où est définie la partition ou sur un autre ordinateur exécutant [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Étant donné qu'une copie des données sources réside dans la structure multidimensionnelle, les requêtes peuvent être résolues sans accéder aux données sources de la partition. Les temps de réponse aux requêtes peuvent être réduits de façon significative à l'aide des agrégations. Les données dans la structure MOLAP de la partition reflètent le traitement le plus récent de la partition.  
  
 À mesure que les données sources changent, les objets d'un stockage MOLAP doivent être périodiquement traités pour intégrer ces changements et les rendre disponibles aux utilisateurs. Le traitement permet la mise à jour, totale ou incrémentielle, des données dans la structure MOLAP. L'intervalle de temps qui sépare un traitement d'un autre crée une période de latence durant laquelle les données des objets OLAP peuvent ne pas correspondre aux données sources. Vous pouvez procéder à une mise à jour incrémentielle ou totale des objets du stockage MOLAP sans mettre la partition ou le cube hors connexion. Cependant, certains cas nécessitent la mise hors connexion du cube pour traiter certaines modifications structurelles dans les objets OLAP. Vous pouvez réduire le temps d'immobilisation requis pour mettre à jour le stockage MOLAP en mettant à jour et en traitant les cubes sur un serveur de test et en utilisant la synchronisation de base de données pour copier les objets traités sur le serveur de production. Vous pouvez également utiliser la mise en cache proactive pour réduire le temps de latence et accroître la disponibilité tout en conservant les avantages du stockage MOLAP en termes de performances. Pour plus d’informations, consultez [mise en cache Proactive &#40;Partitions&#41;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-proactive-caching.md), [synchroniser de bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md), et [du traitement d’un modèle multidimensionnel &#40; Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md).  
  
## <a name="rolap"></a>ROLAP  
 Dans le mode de stockage ROLAP, les agrégations de la partition sont stockées dans des vues indexées de la base de données relationnelle spécifiée dans la source de données de la partition. Contrairement au mode de stockage MOLAP, le mode ROLAP ne débouche pas sur le stockage d'une copie des données sources dans les dossiers de données d'[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Au lieu de cela, lorsque les résultats ne peuvent pas être dérivés du cache de requête, le système accède aux vues indexées de la source de données pour répondre aux requêtes. Les temps de réponses aux requêtes sont généralement plus longs dans le mode de stockage ROLAP que dans les modes de stockage MOLAP ou HOLAP. De la même façon, le temps de traitement est généralement plus lent avec le mode ROLAP. Cependant, le mode ROLAP permet d'afficher les données en temps réel et d'économiser l'espace de stockage si vous utilisez des ensembles de données volumineux fréquemment interrogés, notamment des données purement historiques.  
  
> [!NOTE]  
>  Lorsque le mode ROLAP est utilisé, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut retourner des informations incorrectes sur le membre inconnu si une jointure est combinée avec une clause GROUP BY. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] élimine les erreurs d'intégrité relationnelles plutôt que de retourner la valeur du membre inconnu.  
  
 Si une partition utilise le mode de stockage ROLAP et sa source de données est stocké dans [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tente de créer des vues indexées pour contenir les agrégations de la partition. Si [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne peut pas créer de vues indexées, il ne crée pas de tables d'agrégation. Bien que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] gère les besoins de session pour la création de vues indexées sur [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], les conditions suivantes doivent être remplies par la partition ROLAP et les tables de son schéma remplissent [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour créer des vues indexées pour les agrégations :  
  
-   La partition ne peut pas contenir de mesures qui utilisent la **Min** ou **Max** fonctions d’agrégation.  
  
-   Chaque table du schéma de la partition ROLAP ne doit être utilisée qu'une seule fois. Ainsi, le schéma ne peut pas contenir [dbo].[address] AS "Customer Address" et [dbo].[address] AS "SalesRep Address".  
  
-   Chaque table doit être une table et non une vue.  
  
-   Tous les noms de table du schéma de la partition doivent comprendre le nom du propriétaire, par exemple, [dbo].[customer].  
  
-   Toutes les tables du schéma de la partition doivent avoir le même propriétaire ; par exemple, vous ne pouvez avoir de clause FROM qui fait référence aux tables [tk].[customer], [john].[store] et [dave].[sales_fact_2004].  
  
-   Les colonnes source des mesures de la partition ne doivent pas accepter de valeurs NULL.  
  
-   Les options suivantes devaient être activées (ON) lors de la création de chaque table utilisée dans la vue :  
  
    -   ANSI_NULLS  
  
    -   QUOTED_IDENTIFIER  
  
-   La taille totale de l’index de clé, en [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], ne peut pas dépasser 900 octets. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] vérifie cette condition basée sur les colonnes clés de longueur fixe lors du traitement de l’instruction CREATE INDEX. Toutefois, s’il existe des colonnes de longueur variable dans la clé d’index, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] vérifie également cette condition pour chaque mise à jour les tables de base. Dans la mesure où des agrégations différentes correspondent à des définitions de vues différentes, le traitement ROLAP à l'aide de vues indexées peut réussir ou échouer en fonction de la structure de l'agrégation.  
  
-   Les options suivantes doivent être activées (ON) pour la session de création de la vue indexée : ARITHABORT, CONCAT_NULL_YEILDS_NULL, QUOTED_IDENTIFIER, ANSI_NULLS, ANSI_PADDING et ANSI_WARNING. Ce paramétrage peut être effectué dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
-   L'option suivante doit être désactivée (OFF) pour la session de création de la vue indexée : NUMERIC_ROUNDABORT. Ce paramétrage peut être effectué dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="holap"></a>HOLAP  
 Le mode de stockage HOLAP combine les attributs des modes de stockage MOLAP et ROLAP. Comme MOLAP, HOLAP les agrégations de la partition sont stockées dans une structure multidimensionnelle dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance. Dans le mode HOLAP, aucune copie des données source n'est stockée. Pour les requêtes qui accèdent uniquement aux données de synthèse dans les agrégations d'une partition, le stockage HOLAP est similaire au stockage MOLAP. Les requêtes qui accèdent aux données sources, comme celles qui descendent jusqu'au niveau d'une cellule de cube atomique pour laquelle aucune donnée d'agrégation n'existe, doivent extraire les données de la base de données relationnelle. Le temps de réponse à ces requêtes ne sera pas aussi rapide que si les données source avaient été stockées dans une structure MOLAP. Dans le mode HOLAP, les temps de requête sont généralement très différents selon que la requête peut être résolue dans le cache ou les agrégations, ou dans les données sources.  
  
 Les partitions stockées dans le mode HOLAP sont plus petites que leurs homologues MOLAP car elles ne contiennent pas de données sources, et elles permettent de répondre plus rapidement que les partitions ROLAP aux requêtes portant sur des données de synthèse. Le mode de stockage HOLAP convient généralement pour les partitions de cubes qui nécessitent des réponses rapides aux requêtes sur des données de synthèse calculées à partir d'un volume important de données source. Toutefois, lorsque les utilisateurs génèrent des requêtes touchant des données de niveau feuille, comme pour le calcul de valeurs médianes, le stockage MOLAP constitue généralement le meilleur choix.  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en cache proactive & #40 ; Partitions & #41 ;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-proactive-caching.md)   
 [Synchroniser les bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md)   
 [Partitions & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-analysis-services-multidimensional-data.md)  
  
  
