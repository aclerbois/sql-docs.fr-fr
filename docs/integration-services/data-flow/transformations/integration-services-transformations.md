---
title: Transformations Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- transformations [Integration Services], listed
- transformations [Integration Services], types
- transformations [Integration Services]
- data flow [Integration Services], transformations
- business intelligence transformations [Integration Services]
- join transformations
- split transformations [Integration Services]
- custom transformations [Integration Services]
- row transformations [Integration Services]
- rowset transformations [Integration Services]
ms.assetid: c70c4f6e-82dd-4948-b923-fd5193f67f7e
caps.latest.revision: 56
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 10f340ee0cc9d8184c612e80de7b872356b0e962
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-transformations"></a>Transformations Integration Services
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] Les transformations sont les composants du flux de données d’un package qui agrègent, fusionnent, distribuent et modifient des données. Les transformations peuvent également effectuer des opérations de recherche et générer des échantillons de dataset. Cette section décrit les transformations incluses dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] et explique leur fonctionnement.  
  
## <a name="business-intelligence-transformations"></a>Transformations Business Intelligence  
 Les transformations suivantes effectuent des opérations décisionnelles telles que le nettoyage de données, l'exploration de texte et l'exécution de requêtes de prédiction d'exploration de données.  
  
|Transformation|Description|  
|--------------------|-----------------|  
|[Transformation de dimension à variation lente](../../../integration-services/data-flow/transformations/slowly-changing-dimension-transformation.md)|Transformation qui configure la mise à jour d'une dimension à variation lente.|  
|[Transformation de regroupement probable](../../../integration-services/data-flow/transformations/fuzzy-grouping-transformation.md)|Transformation qui normalise les valeurs en données de colonne.|  
|[Transformation de recherche floue](../../../integration-services/data-flow/transformations/fuzzy-lookup-transformation.md)|Transformation qui recherche des valeurs dans une table de référence au moyen d'une correspondance approximative.|  
|[Transformation d'extraction de terme](../../../integration-services/data-flow/transformations/term-extraction-transformation.md)|Transformation qui extrait des termes à partir du texte.|  
|[Transformation de recherche de terme](../../../integration-services/data-flow/transformations/term-lookup-transformation.md)|Transformation qui recherche des termes dans une table de référence et compte les termes extraits à partir du texte.|  
|[Transformation de requête d’exploration de données](../../../integration-services/data-flow/transformations/data-mining-query-transformation.md)|Transformation qui exécute des requêtes de prédictions d'exploration de données.|  
|[Transformation de nettoyage DQS](../../../integration-services/data-flow/transformations/dqs-cleansing-transformation.md)|Transformation qui corrige des données d'une source de données connectée en appliquant des règles créées pour la source de données.|  
  
## <a name="row-transformations"></a>Transformations de lignes  
 Les transformations suivantes mettent à jour les valeurs de colonnes et créent de nouvelles colonnes. La transformation est appliquée à chaque ligne de l'entrée de transformation.  
  
|Transformation|Description|  
|--------------------|-----------------|  
|[Transformation de la table de caractères](../../../integration-services/data-flow/transformations/character-map-transformation.md)|Transformation qui applique des fonctions de chaîne à des données de caractères.|  
|[Transformation Copie de colonnes](../../../integration-services/data-flow/transformations/copy-column-transformation.md)|Transformation qui ajoute des copies de colonnes d'entrée à la sortie de transformation.|  
|[Transformation de conversion de données](../../../integration-services/data-flow/transformations/data-conversion-transformation.md)|Transformation qui convertit le type de données d'une colonne en un type de données différent.|  
|[Transformation de colonne dérivée](../../../integration-services/data-flow/transformations/derived-column-transformation.md)|Transformation qui remplit des colonnes avec les résultats d'expressions.|  
|[Transformation d'exportation de colonne](../../../integration-services/data-flow/transformations/export-column-transformation.md)|Transformation qui insère des données dans un fichier à partir d'un flux de données.|  
|[Transformation d'importation de colonne](../../../integration-services/data-flow/transformations/import-column-transformation.md)|Transformation qui lit des données à partir d'un fichier et les ajoute à un flux de données.|  
|[Composant Script](../../../integration-services/data-flow/transformations/script-component.md)|Transformation qui utilise un script pour extraire, transformer ou charger des données.|  
|[Transformation de commande OLE DB](../../../integration-services/data-flow/transformations/ole-db-command-transformation.md)|Transformation qui exécute des commandes SQL pour chaque ligne d'un flux de données.|  
  
## <a name="rowset-transformations"></a>Transformations d'ensemble de lignes  
 Les transformations suivantes créent des ensembles de lignes. L'ensemble de lignes peut inclure des valeurs agrégées et triées, des échantillons d'ensembles de lignes ou des ensembles de lignes croisés dynamiques et non croisés dynamiques.  
  
|Transformation|Description|  
|--------------------|-----------------|  
|[Transformation d'agrégation](../../../integration-services/data-flow/transformations/aggregate-transformation.md)|Transformation qui effectue des agrégations telles que AVERAGE, SUM et COUNT.|  
|[Transformation de tri](../../../integration-services/data-flow/transformations/sort-transformation.md)|Transformation qui trie des données.|  
|[Transformation de l'échantillonnage du pourcentage](../../../integration-services/data-flow/transformations/percentage-sampling-transformation.md)|Transformation qui crée un échantillon de jeu de données avec un pourcentage spécifiant la taille d'échantillonnage.|  
|[Transformation d'échantillonnage de lignes](../../../integration-services/data-flow/transformations/row-sampling-transformation.md)|Transformation qui crée un échantillon de jeu de données en spécifiant le nombre de lignes de l'échantillon.|  
|[Transformation de tableau croisé dynamique](../../../integration-services/data-flow/transformations/pivot-transformation.md)|Transformation qui crée une version moins normalisée d'une table normalisée.|  
|[Transformation Unpivot](../../../integration-services/data-flow/transformations/unpivot-transformation.md)|Transformation qui crée une version plus normalisée d'une table non normalisée.|  
  
## <a name="split-and-join-transformations"></a>Transformations de fractionnement et de jointure  
 Les transformations suivantes distribuent des lignes vers différentes sorties, créent des copies des entrées de transformation, joignent plusieurs entrées en une même entrée et effectuent des opérations de recherche.  
  
|Transformation|Description|  
|--------------------|-----------------|  
|[Transformation de fractionnement conditionnel](../../../integration-services/data-flow/transformations/conditional-split-transformation.md)|Transformation qui achemine des lignes de données vers différentes sorties.|  
|[Transformation de multidiffusion](../../../integration-services/data-flow/transformations/multicast-transformation.md)|Transformation qui distribue des jeux de données vers différentes sorties.|  
|[Transformation d'union totale](../../../integration-services/data-flow/transformations/union-all-transformation.md)|Transformation qui fusionne plusieurs jeux de données.|  
|[Transformation de fusion](../../../integration-services/data-flow/transformations/merge-transformation.md)|Transformation qui fusionne deux jeux de données triés.|  
|[Transformation de jointure de fusion](../../../integration-services/data-flow/transformations/merge-join-transformation.md)|Transformation qui joint deux jeux de données à l'aide d'une jointure FULL, LEFT ou INNER.|  
|[Transformation de recherche](../../../integration-services/data-flow/transformations/lookup-transformation.md)|Transformation qui recherche des valeurs dans une table de référence au moyen d'une correspondance exacte.|  
|[Transformation du cache](../../../integration-services/data-flow/transformations/cache-transform.md)|Transformation qui écrit les données d'une source de données connectée du flux de données dans un gestionnaire de connexions du cache qui enregistre les données dans un fichier cache. La transformation de recherche effectue des recherches sur les données dans le fichier cache.|  
|[Transformation du distributeur de données équilibrées (BDD)](../../../integration-services/data-flow/transformations/balanced-data-distributor-transformation.md)|La transformation distribue des tampons de lignes entrantes uniformément dans les sorties sur des threads distincts pour améliorer les performances des packages SSIS en cours d'exécution sur des serveurs à plusieurs cœurs et plusieurs processeurs.|  
  
## <a name="auditing-transformations"></a>Audit des transformations  
 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] inclut les transformations suivantes pour ajouter des informations d’audit et compter le nombre de lignes.  
  
|Transformation|Description|  
|--------------------|-----------------|  
|[Transformation d'audit](../../../integration-services/data-flow/transformations/audit-transformation.md)|Transformation qui rend les informations sur l'environnement accessibles au flux de données d'un package.|  
|[Transformation de calcul du nombre de lignes](../../../integration-services/data-flow/transformations/row-count-transformation.md)|Transformation qui compte des lignes à mesure qu'elle les parcourt et stocke le nombre final dans une variable.|  
  
## <a name="custom-transformations"></a>Transformations personnalisées  
 Vous pouvez également écrire des transformations personnalisées. Pour plus d’informations, consultez [Développement d’un composant de transformation personnalisé à sorties synchrones](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md) et [Développement d’un composant de transformation personnalisé à sorties asynchrones](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md).  
  
  
