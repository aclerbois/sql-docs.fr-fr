---
title: Fonctionnalités prises en charge par les éditions de SQL Server 2016 Analysis Services | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 19618fc0311de28184e3a95c5e57e423d121f085
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analysis-services-features-supported-by-sql-server-editions"></a>Fonctionnalités Analysis Services pris en charge par les éditions de SQL Server
[!INCLUDE[ssas-appliesto-sql2016-later](../includes/ssas-appliesto-sql2016-later.md)]

Cette rubrique fournit des détails sur les fonctionnalités prises en charge par les différentes éditions de SQL Server 2016 Analysis Services. Pour les fonctionnalités prises en charge par les éditions Evaluation et Developer, consultez Enterprise edition.

## <a name="analysis-services-servers"></a>Analysis Services (serveurs)
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express with Tools|Express|Développeur|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|-------------|---------------|  
|Bases de données partagées évolutives|Oui||||||Oui|  
|Sauvegarder/Restaurer et Attacher/Détacher des bases de données|Oui|Oui|||||Oui|  
|Synchroniser des bases de données|Oui||||||Oui|  
|Instances de cluster de basculement Always On|Oui<br /><br /> Le nombre de nœuds correspond au maximum du système d’exploitation|Oui<br /><br /> Prise en charge de 2 nœuds|||||Oui<br /><br /> Le nombre de nœuds correspond au maximum du système d’exploitation|  
|Programmabilité (AMO, ADOMD.Net, OLEDB, XML/A, ASSL, TMSL)|Oui|Oui|||||Oui|  
  
## <a name="tabular-models"></a>Modèles tabulaires 
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express with Tools|Express|Développeur|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|-------------|---------------|  
|Hierarchies|Oui|Oui|||||Oui|  
|Indicateurs de performance clés|Oui|Oui|||||Oui|  
|Perspectives|Oui||||||Oui|  
|Traductions|Oui|Oui|||||Oui|  
|Calculs DAX, requêtes DAX, requêtes MDX|Oui|Oui|||||Oui|  
|Sécurité au niveau des lignes|Oui|Oui|||||Oui|  
|Partitions multiples|Oui||||||Oui|  
|Mode de stockage en mémoire|Oui|Oui|||||Oui|  
|Mode de stockage DirectQuery|Oui||||||Oui|  

## <a name="multidimensional-models"></a>Modèles multidimensionnels 
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express with Tools|Express|Développeur|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|-------------|---------------|  
|Mesures semi-additives|Oui|Non <sup>1</sup>|||||Oui|  
|Hierarchies|Oui|Oui|||||Oui|  
|Indicateurs de performance clés|Oui|Oui|||||Oui|  
|Perspectives|Oui||||||Oui|  
|Actions|Oui|Oui|||||Oui|  
|Intelligence comptable|Oui|Oui|||||Oui|  
|Time Intelligence|Oui|Oui|||||Oui|  
|Cumuls personnalisés|Oui|Oui|||||Oui|  
|Cube d'écriture différée|Oui|Oui|||||Oui|  
|Dimensions d'écriture différée|Oui||||||Oui|  
|Cellules d'écriture différée|Oui|Oui|||||Oui|  
|extraction|Oui|Oui|||||Oui|  
|Types de hiérarchies avancés (parent-enfant et irrégulières)|Oui|Oui|||||Oui|  
|Dimensions avancées (dimensions de référence, plusieurs-à-plusieurs)|Oui|Oui|||||Oui|  
|Dimensions et mesures liées|Oui|Oui  <sup>2</sup> |||||Oui|  
|Traductions|Oui|Oui|||||Oui|  
|Aggregations|Oui|Oui|||||Oui|  
|Partitions multiples|Oui|Oui, jusqu'à 3|||||Oui|  
|Mise en cache proactive|Oui||||||Oui|  
|Assemblys personnalisés (procédures stockées)|Oui|Oui|||||Oui|  
|Requêtes et scripts MDX|Oui|Oui|||||Oui|  
|Requêtes DAX|Oui|Oui|||||Oui|  
|Modèle de sécurité basé sur les rôles|Oui|Oui|||||Oui|  
|Sécurité au niveau de la dimension et de la cellule|Oui|Oui|||||Oui|  
|Stockage évolutif de chaîne|Oui|Oui|||||Oui|  
|Modèles de stockage MOLAP, ROLAP et HOLAP|Oui|Oui|||||Oui|  
|Transport XML binaire et compressé|Oui|Oui|||||Oui|  
|Traitement de type envoi de données (push)|Oui||||||Oui|  
|Écriture différée directe|Oui||||||Oui|  
|Expressions de mesure|Oui||||||Oui|  
  
 <sup>1</sup> La mesure semi-additive LastChild est prise en charge dans l’édition Standard, contrairement à d’autres mesures semi-additives, telles que None, FirstChild, FirstNonEmpty, LastNonEmpty, AverageOfChildren et ByAccount. Les mesures additives, telles que Sum, Count, Min, Max, et les mesures non additives (DistinctCount) sont prises en charge dans toutes les éditions.  
  <sup>2</sup> standard edition prend en charge la liaison des mesures et des dimensions dans la même base de données, mais pas à partir de bases de données ou des autres instances.
  
## <a name="power-pivot-for-sharepoint"></a>Power Pivot pour SharePoint  
  
|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express with Tools|Express|Développeur|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|-------------|---------------|  
|Intégration de batterie de serveurs SharePoint selon l'architecture de service partagé|Oui||||||Oui|  
|Création de rapports d'utilisation|Oui||||||Oui|  
|Règles de contrôle d'intégrité|Oui||||||Oui|  
|Galerie Power Pivot|Oui||||||Oui|  
|Actualisation des données Power Pivot|Oui||||||Oui|  
|Flux de données Power Pivot|Oui||||||Oui|  
  
## <a name="data-mining"></a>Exploration de données  
  
|Nom de la fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express with Tools|Express|Développeur|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|-------------|---------------|  
|Algorithmes standard|Oui|Oui|||||Oui|  
|Outils d’exploration de données (Assistants, éditeurs, générateurs de requêtes)|Oui|Oui|||||Oui|  
|Validation croisée|Oui||||||Oui|  
|Modèles sur les sous-ensembles filtrés de données de structure d'exploration de données|Oui||||||Oui|  
|Série chronologique : fusion personnalisée entre les méthodes ARTXP et ARIMA|Oui||||||Oui|  
|Série chronologique : prédiction avec les nouvelles données|Oui||||||Oui|  
|Requêtes d’exploration de données simultanées illimitées|Oui||||||Oui|  
|Configuration avancée et options de paramétrage pour les algorithmes d’exploration de données|Oui||||||Oui|  
|Prise en charge des algorithmes de plug-in|Oui||||||Oui|  
|Traitement de modèle parallèle|Oui||||||Oui|  
|Série chronologique : prédiction inter-série|Oui||||||Oui|  
|Attributs illimités pour les règles d'association|Oui||||||Oui|  
|Prédiction de séquence|Oui||||||Oui|  
|Plusieurs cibles de prédiction pour naïve Bayes, réseau neuronal et régression logistique|Oui||||||Oui|  
  
 ## <a name="see-also"></a>Voir aussi  
 [Spécifications de produit pour SQL Server 2016](http://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)   
 [Installation de SQL Server 2016](../database-engine/install-windows/installation-for-sql-server-2016.md)  


