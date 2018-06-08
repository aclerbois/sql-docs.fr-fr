---
title: XML for Analysis Schema Rowsets | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f85f7f1a0daa81a549441bff6a386213916f57f1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="xml-for-analysis-schema-rowsets"></a>XML for Analysis Schema Rowsets
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Le fournisseur [!INCLUDE[msCoName](../../../includes/msconame-md.md)] XML for Analysis (XMLA) inclut des ensembles de lignes de schéma qui retournent des métadonnées sur l'état du serveur, l'activité et des objets. La récupération de métadonnées est nécessaire si vous développez une application cliente qui se connecte à un modèle Analysis Services dont la structure et les caractéristiques sont variables.  
  
 Les ensembles de lignes de schéma fournissent également l'analyse des processus internes et les opérations qui peuvent vous aider à analyser le serveur et à résoudre les problèmes. Pour une meilleure prise en charge des tâches d'administration ad hoc, vous pouvez exécuter une requête de vue de gestion dynamique (DMV) sur la plupart des ensembles de lignes de schéma. Les requêtes DMV retournent des résultats dans un format lisible et tabulaire que vous pouvez afficher dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
 Le tableau suivant répertorie et décrit chaque ensemble de lignes de schéma XMLA, et identifie s'il retourne des informations spécifiques aux modèles de données tabulaires.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rowset<sup>1</sup>| Description|  
|------------------------|-----------------|  
|[Ensemble de lignes DISCOVER_CALC_DEPENDENCY](../../../analysis-services/schema-rowsets/xml/discover-calc-dependency-rowset.md)|Retourne des informations sur les dépendances entre les tables, colonnes, mesures et formules de colonne calculée.<br /><br /> S’applique aux modèles tabulaires déployés sur une instance Analysis Services et [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] modèles dans les classeurs Excel qui s’exécutent dans un environnement SharePoint.|  
|[Ensemble de lignes DISCOVER_CONNECTIONS](../../../analysis-services/schema-rowsets/xml/discover-connections-rowset.md)|Fournit des informations sur l'activité et l'utilisation des ressources des connexions actuellement ouvertes sur le serveur.|  
|[Ensemble de lignes DISCOVER_COMMAND_OBJECTS](../../../analysis-services/schema-rowsets/xml/discover-command-objects-rowset.md)|Fournit des informations sur l'activité et l'utilisation des ressources des objets actuellement utilisés par la commande référencée.|  
|[Ensemble de lignes DISCOVER_COMMANDS](../../../analysis-services/schema-rowsets/xml/discover-commands-rowset.md)|Fournit des informations sur l'activité et l'utilisation des ressources des dernières commandes exécutées ou des commandes en cours d'exécution sur les connexions ouvertes sur le serveur.|  
|[Ensemble de lignes DISCOVER_CSDL_METADATA](../../../analysis-services/schema-rowsets/xml/discover-csdl-metadata-rowset.md)|Retourne une définition XML d’une source de données à un client qui peut consommer un tabulaire ou [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] de modèle et présente les données sources dans le cadre d’un rapport.<br /><br /> S’applique aux modèles tabulaires déployés sur une instance Analysis Services et [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] modèles dans les classeurs Excel qui s’exécutent dans un environnement SharePoint.|  
|[DISCOVER_DATASOURCES, ensemble de lignes](../../../analysis-services/schema-rowsets/xml/discover-datasources-rowset.md)|Retourne la liste des sources de données du fournisseur XMLA qui sont disponibles sur le serveur ou le service Web.|  
|[Ensemble de lignes DISCOVER_DB_CONNECTIONS](../../../analysis-services/schema-rowsets/xml/discover-db-connections-rowset.md)|Fournit des informations sur l'activité et l'utilisation des ressources des connexions actuellement ouvertes du serveur à une base de données.|  
|[Ensemble de lignes DISCOVER_DIMENSION_STAT](../../../analysis-services/schema-rowsets/xml/discover-dimension-stat-rowset.md)|Retourne des statistiques sur la dimension spécifiée.|  
|[Ensemble de lignes DISCOVER_ENUMERATORS](../../../analysis-services/schema-rowsets/xml/discover-enumerators-rowset.md)|Retourne une liste des noms, types de données et valeurs d'énumération d'énumérateurs pris en charge par le fournisseur XMLA pour une source de données spécifique.|  
|[Ensemble de lignes DISCOVER_JOBS](../../../analysis-services/schema-rowsets/xml/discover-jobs-rowset.md)|Fournit des informations sur les travaux actifs exécutés sur le serveur.|  
|[Ensemble de lignes DISCOVER_KEYWORDS & #40 ; XMLA & #41 ;](../../../analysis-services/schema-rowsets/xml/discover-keywords-rowset-xmla.md)|Retourne des informations sur les mots clés réservés par le fournisseur XMLA.|  
|[Ensemble de lignes DISCOVER_LITERALS](../../../analysis-services/schema-rowsets/xml/discover-literals-rowset.md)|Retourne des informations sur les littéraux pris en charge par le fournisseur XMLA, y compris les types de données et les valeurs.|  
|[DISCOVER_LOCATIONS, ensemble de lignes](../../../analysis-services/schema-rowsets/xml/discover-locations-rowset.md)|Retourne des informations sur le contenu d'un fichier de sauvegarde.|  
|[Ensemble de lignes DISCOVER_LOCKS](../../../analysis-services/schema-rowsets/xml/discover-locks-rowset.md)|Fournit des informations sur les verrous actuellement en place sur le serveur.|  
|[Ensemble de lignes DISCOVER_MEMORYGRANT](../../../analysis-services/schema-rowsets/xml/discover-memorygrant-rowset.md)|Retourne la liste des allocations de quotas de mémoire interne qui sont prises par les travaux en cours d'exécution sur le serveur.|  
|[Ensemble de lignes DISCOVER_MEMORYUSAGE](../../../analysis-services/schema-rowsets/xml/discover-memoryusage-rowset.md)|Retourne les statistiques d'utilisation de la mémoire pour plusieurs objets alloués par le serveur.|  
|[Ensemble de lignes DISCOVER_OBJECT_ACTIVITY](../../../analysis-services/schema-rowsets/xml/discover-object-activity-rowset.md)|Fournit l'utilisation des ressources par objet depuis le démarrage du service.|  
|[Ensemble de lignes DISCOVER_OBJECT_MEMORY_USAGE](../../../analysis-services/schema-rowsets/xml/discover-object-memory-usage-rowset.md)|Fournit des informations sur les ressources mémoire utilisées par les objets.|  
|[Ensemble de lignes DISCOVER_PARTITION_DIMENSION_STAT](../../../analysis-services/schema-rowsets/xml/discover-partition-dimension-stat-rowset.md)|Retourne des statistiques sur la dimension associée à une partition.|  
|[Ensemble de lignes DISCOVER_PARTITION_STAT](../../../analysis-services/schema-rowsets/xml/discover-partition-stat-rowset.md)|Retourne des statistiques sur les agrégations dans une partition particulière.|  
|[Ensemble de lignes DISCOVER_PERFORMANCE_COUNTERS](../../../analysis-services/schema-rowsets/xml/discover-performance-counters-rowset.md)|Retourne la valeur d'un ou de plusieurs compteurs de performance spécifiés.|  
|[Ensemble de lignes DISCOVER_PROPERTIES](../../../analysis-services/schema-rowsets/xml/discover-properties-rowset.md)|Retourne une liste d'informations et valeurs sur les propriétés standard et spécifiques au fournisseur prises en charge par le fournisseur XMLA pour la source de données spécifiée.|  
|[Ensemble de lignes DISCOVER_SCHEMA_ROWSETS](../../../analysis-services/schema-rowsets/xml/discover-schema-rowsets-rowset.md)|Retourne les noms, restrictions, description et d'autres informations pour toutes les valeurs d'énumération et toutes les valeurs d'énumération supplémentaires spécifiques au fournisseur prises en charge par le fournisseur XMLA.|  
|[Ensemble de lignes DISCOVER_SESSIONS](../../../analysis-services/schema-rowsets/xml/discover-sessions-rowset.md)|Fournit des informations sur l'activité et l'utilisation des ressources des sessions actuellement ouvertes sur le serveur.|  
|[Ensemble de lignes DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS](../../../analysis-services/schema-rowsets/xml/discover-storage-table-column-segments-rowset.md)|Fournit des informations au niveau de la colonne et du segment sur les tables de stockage utilisé par un tableau ou [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] base de données.<br /><br /> S’applique aux modèles tabulaires déployés sur une instance Analysis Services et [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] modèles dans les classeurs Excel qui s’exécutent dans un environnement SharePoint.|  
|[Ensemble de lignes DISCOVER_STORAGE_TABLE_COLUMNS](../../../analysis-services/schema-rowsets/xml/discover-storage-table-columns-rowset.md)|Permet au client de déterminer l’affectation de colonnes aux tables de stockage utilisés par un tableau ou [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] base de données.<br /><br /> S’applique aux modèles tabulaires déployés sur une instance Analysis Services et [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] modèles dans les classeurs Excel qui s’exécutent dans un environnement SharePoint.|  
|[Ensemble de lignes DISCOVER_STORAGE_TABLES](../../../analysis-services/schema-rowsets/xml/discover-storage-tables-rowset.md)|Retourne des informations sur les tables utilisées dans un modèle.<br /><br /> S’applique aux modèles tabulaires déployés sur une instance Analysis Services et [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] modèles dans les classeurs Excel qui s’exécutent dans un environnement SharePoint.|  
|[Ensemble de lignes DISCOVER_TRACE_COLUMNS](../../../analysis-services/schema-rowsets/xml/discover-trace-columns-rowset.md)|Décrit les colonnes de trace fournies par le fournisseur de trace.|  
|[Ensemble de lignes DISCOVER_TRACE_DEFINITION_PROVIDERINFO](../../../analysis-services/schema-rowsets/xml/discover-trace-definition-providerinfo-rowset.md)|Retourne des informations de base sur le fournisseur de trace, telles que son nom et sa description.|  
|[Ensemble de lignes DISCOVER_TRACE_EVENT_CATEGORIES](../../../analysis-services/schema-rowsets/xml/discover-trace-event-categories-rowset.md)|Décrit les catégories d'événements fournies par le fournisseur de trace.|  
|[Ensemble de lignes DISCOVER_TRACES](../../../analysis-services/schema-rowsets/xml/discover-traces-rowset.md)|Retourne des informations sur les traces qui s'exécutent sur un serveur.|  
|[Ensemble de lignes DISCOVER_TRANSACTIONS](../../../analysis-services/schema-rowsets/xml/discover-transactions-rowset.md)|Retourne l'ensemble actuel des transactions en attente sur le système.|  
|[DISCOVER_XML_METADATA, ensemble de lignes](../../../analysis-services/schema-rowsets/xml/discover-xml-metadata-rowset.md)|Retourne un document XML qui décrit un objet demandé.|  
  
 <sup>1</sup> des ensembles de schéma répertoriés ici sont pris en charge par le fournisseur de source de données MSOLAP pour le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] fournisseur XMLA.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement avec XMLA dans Analysis Services](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)   
 [Utilisez les vues de gestion dynamique &#40;DMV&#41; pour surveiller Analysis Services](../../../analysis-services/instances/use-dynamic-management-views-dmvs-to-monitor-analysis-services.md)   
 [Récupération de métadonnées à partir d’une source de données analytiques](../../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-from-an-analytical-data-source.md)  
  
  
