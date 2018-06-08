---
title: Utilisez les vues de gestion dynamique (DMV) pour surveiller Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 412923d24b4d48a0ebdfa11bcf60dc19d5b85368
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="use-dynamic-management-views-dmvs-to-monitor-analysis-services"></a>Utiliser des vues de gestion dynamique (DMV) pour surveiller Analysis Services
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les vues de gestion dynamique (DMV) d'Analysis Services sont des structures de requête qui exposent des informations sur les opérations de serveur local et l'intégrité du serveur. La structure de requête est une interface vers des ensembles de lignes de schéma qui retournent des métadonnées et des informations d'analyse relatives à une instance Analysis Services.  
  
 Pour la plupart des requêtes DMV, vous utilisez une instruction **SELECT** et le schéma **$System** avec un ensemble de lignes de schéma XML/A.  
  
```  
SELECT * FROM $System.<schemaRowset>  
```  
  
 Les requêtes DMV retournent des informations sur l'état du serveur actif au moment de l'exécution de la requête. Pour surveiller les opérations en temps réel, utilisez plutôt le suivi. Pour plus d’informations, consultez [Utiliser SQL Server Profiler pour contrôler Analysis Services](../../analysis-services/instances/use-sql-server-profiler-to-monitor-analysis-services.md).  
  
##  <a name="bkmk_ben"></a> Avantages des requêtes à l’aide des DMV  
 Les requêtes DMV retournent des informations sur les opérations et la consommation des ressources qui ne sont pas accessibles par d'autres biais.  
  
 Les requêtes DMV sont une alternative à l'exécution des commandes Discover XML/A. Pour la plupart des administrateurs, l'écriture d'une requête DMV est plus simple car la syntaxe de la requête est basée sur le langage SQL. En outre, le jeu de résultats est retourné dans un format tabulaire qui est plus facile à lire et à copier.  
  
##  <a name="bkmk_ex"></a> Exemples et scénarios  
 Une requête DMV peut vous aider à répondre à des questions sur les sessions et les connexions actives, ainsi que sur les objets qui consomment le plus d'UC ou de mémoire à un moment précis. Cette section fournit des exemples pour les scénarios d'utilisation de requêtes DMV les plus courants. Vous pouvez également consulter le [Guide des opérations de SQL Server 2008 R2 Analysis Services](http://go.microsoft.com/fwlink/?LinkID=225539&clcid=0x409) pour plus d’informations sur l’utilisation de requêtes DMV pour surveiller une instance de serveur.  
  
 `Select * from $System.discover_object_activity` /** Cette requête rend compte de l’activité des objets depuis le dernier démarrage du service. Pour obtenir des exemples de requêtes basées sur cette vue DMV, consultez [New System.Discover_Object_Activity](http://go.microsoft.com/fwlink/?linkid=221322).  
  
 `Select * from $System.discover_object_memory_usage` /** Cette requête rend compte de la consommation de mémoire par objet.  
  
 `Select * from $System.discover_sessions` /** Cette requête rend compte des sessions actives, notamment de la session utilisateur et de sa durée.  
  
 `Select * from $System.discover_locks` /** Cette requête retourne un instantané des verrous utilisés à un moment précis.  
  
##  <a name="bkmk_syn"></a> Syntaxe de requête  
 Le moteur d'interrogation des vues DMV est l'analyseur d'exploration de données. La syntaxe de requête DMV repose sur l’instruction [SELECT &#40;DMX&#41;](../../dmx/select-dmx.md).  
  
 Bien que la syntaxe de requête DMV soit basée sur une instruction SQL SELECT, elle ne prend pas en charge la syntaxe complète d'une instruction SELECT. Notez que JOIN, GROUP BY, LIKE, CAST et CONVERT ne sont pas pris en charge.  
  
```  
SELECT [DISTINCT] [TOP <n>] <select list>  
FROM $System.<schemaRowset>  
[WHERE <condition expression>]  
[ORDER BY <expression>[DESC|ASC]]  
```  
  
 L'exemple suivant pour DISCOVER_CALC_DEPENDENCY illustre l'utilisation de la clause WHERE pour la fourniture d'un paramètre à la requête :  
  
```  
SELECT * FROM $System.DISCOVER_CALC_DEPENDENCY  
WHERE OBJECT_TYPE = 'ACTIVE_RELATIONSHIP'  
```  
  
 Sinon, pour les ensembles de lignes de schéma soumis à des restrictions, la requête doit inclure la fonction SYSTEMRESTRICTSCHEMA. L'exemple suivant retourne des métadonnées CSDL sur les modèles tabulaires exécutés sur un serveur en mode tabulaire. Notez que CATALOG_NAME respecte la casse :  
  
```  
Select * from SYSTEMRESTRICTSCHEMA ($System.Discover_csdl_metadata, [CATALOG_NAME] = 'Adventure Works DW')  
```  
  
##  <a name="bkmk_tools"></a> Outils et autorisations  
 Vous devez disposer d'autorisations d'administrateur système sur l'instance Analysis Services pour interroger une vue DMV.  
  
 Vous pouvez utiliser toute application cliente prenant en charge les requêtes MDX ou DMX, notamment SQL Server Management Studio, un rapport Reporting Services ou un tableau de bord PerformancePoint.  
  
 Pour exécuter une requête DMV à partir de Management Studio, connectez-vous à l’instance à interroger, puis cliquez sur **Nouvelle requête**. Vous pouvez exécuter une requête à partir d'une fenêtre de requête MDX ou DMX.  
  
##  <a name="bkmk_ref"></a> Référence de la vue de gestion dynamique  
 Tous les ensembles de lignes de schéma n'ont pas d'interface DMV. Pour retourner la liste de tous les ensembles de lignes de schéma qui peuvent être interrogés à l'aide d'une vue de gestion dynamique, exécutez la requête suivante.  
  
```  
SELECT * FROM $System.DBSchema_Tables   
WHERE TABLE_TYPE = 'SCHEMA'   
ORDER BY TABLE_NAME ASC  
```  
  
> [!NOTE]  
>  Si une vue DMV n’est pas disponible pour un ensemble de lignes donné, le serveur renvoie l’erreur suivante : « la \<schemarowset > type de demande n’est pas reconnu par le serveur ». Toutes les autres erreurs signalent des problèmes de syntaxe.  
  
|Ensemble de lignes|Description|  
|------------|-----------------|  
|[Ensemble de lignes DBSCHEMA_CATALOGS](../../analysis-services/schema-rowsets/ole-db/dbschema-catalogs-rowset.md)|Retourne la liste des bases de données Analysis Services sur la connexion actuelle.|  
|[Ensemble de lignes DBSCHEMA_COLUMNS](../../analysis-services/schema-rowsets/ole-db/dbschema-columns-rowset.md)|Retourne la liste de toutes les colonnes dans la base de données active. Vous pouvez utiliser cette liste pour construire une requête DMV.|  
|[Lignes de schéma DBSCHEMA_PROVIDER_TYPES](../../analysis-services/schema-rowsets/ole-db/dbschema-provider-types-rowset.md)|Retourne les propriétés relatives aux types de données de base pris en charge par le fournisseur de données OLE DB.|  
|[Ensemble de lignes DBSCHEMA_TABLES](../../analysis-services/schema-rowsets/ole-db/dbschema-tables-rowset.md)|Retourne la liste de toutes les tables dans la base de données active. Vous pouvez utiliser cette liste pour construire une requête DMV.|  
|[Ensemble de lignes DISCOVER_CALC_DEPENDENCY](../../analysis-services/schema-rowsets/xml/discover-calc-dependency-rowset.md)|Retourne la liste des colonnes et des tables utilisées dans un modèle qui ont des dépendances sur d'autres colonnes et tables.|  
|[Ensemble de lignes DISCOVER_COMMAND_OBJECTS](../../analysis-services/schema-rowsets/xml/discover-command-objects-rowset.md)|Fournit des informations sur l'activité et l'utilisation des ressources par les objets actuellement utilisés par la commande référencée.|  
|[Ensemble de lignes DISCOVER_COMMANDS](../../analysis-services/schema-rowsets/xml/discover-commands-rowset.md)|Fournit des informations sur l'activité et l'utilisation des ressources relatives à la commande en cours d'exécution.|  
|[Ensemble de lignes DISCOVER_CONNECTIONS](../../analysis-services/schema-rowsets/xml/discover-connections-rowset.md)|Fournit à Analysis Services des informations sur l'activité et l'utilisation des ressources relatives aux connexions ouvertes.|  
|[Ensemble de lignes DISCOVER_CSDL_METADATA](../../analysis-services/schema-rowsets/xml/discover-csdl-metadata-rowset.md)|Retourne des informations sur un modèle tabulaire.<br /><br /> Nécessite l'ajout de SYSTEMRESTRICTSCHEMA et de paramètres supplémentaires.|  
|[Ensemble de lignes DISCOVER_DB_CONNECTIONS](../../analysis-services/schema-rowsets/xml/discover-db-connections-rowset.md)|Échange des informations sur l'activité et l'utilisation des ressources relatives aux connexions ouvertes entre Analysis Services et les sources de données externes, par exemple au cours des opérations de traitement et d'importation.|  
|[Ensemble de lignes DISCOVER_DIMENSION_STAT](../../analysis-services/schema-rowsets/xml/discover-dimension-stat-rowset.md)|Retourne les attributs d'une dimension ou les colonnes d'une table, selon le type de modèle.|  
|[Ensemble de lignes DISCOVER_ENUMERATORS](../../analysis-services/schema-rowsets/xml/discover-enumerators-rowset.md)|Retourne des métadonnées sur les énumérateurs pris en charge pour une source de données spécifique.|  
|[Ensemble de lignes DISCOVER_INSTANCES](../../analysis-services/schema-rowsets/ole-db-olap/discover-instances-rowset.md)|Retourne des informations sur l'instance spécifiée.<br /><br /> Nécessite l'ajout de SYSTEMRESTRICTSCHEMA et de paramètres supplémentaires.|  
|[Ensemble de lignes DISCOVER_JOBS](../../analysis-services/schema-rowsets/xml/discover-jobs-rowset.md)|Retourne des informations sur les travaux en cours.|  
|[Ensemble de lignes DISCOVER_KEYWORDS & #40 ; XMLA & #41 ;](../../analysis-services/schema-rowsets/xml/discover-keywords-rowset-xmla.md)|Retourne la liste des mots clés réservés.|  
|[Ensemble de lignes DISCOVER_LITERALS](../../analysis-services/schema-rowsets/xml/discover-literals-rowset.md)|Retourne la liste des littéraux pris en charge par XMLA, y compris les types de données et les valeurs.|  
|[Ensemble de lignes DISCOVER_LOCKS](../../analysis-services/schema-rowsets/xml/discover-locks-rowset.md)|Retourne un instantané des verrous utilisés à un instant précis.|  
|[Ensemble de lignes DISCOVER_MEMORYGRANT](../../analysis-services/schema-rowsets/xml/discover-memorygrant-rowset.md)|Retourne des informations sur la mémoire allouée par Analysis Services au démarrage.|  
|[Ensemble de lignes DISCOVER_MEMORYUSAGE](../../analysis-services/schema-rowsets/xml/discover-memoryusage-rowset.md)|Affiche l'utilisation de la mémoire par des objets spécifiques.|  
|[Ensemble de lignes DISCOVER_OBJECT_ACTIVITY](../../analysis-services/schema-rowsets/xml/discover-object-activity-rowset.md)|Rend compte de l'activité des objets depuis le dernier démarrage du service.|  
|[Ensemble de lignes DISCOVER_OBJECT_MEMORY_USAGE](../../analysis-services/schema-rowsets/xml/discover-object-memory-usage-rowset.md)|Rend compte de la consommation de mémoire par objet.|  
|[Ensemble de lignes DISCOVER_PARTITION_DIMENSION_STAT](../../analysis-services/schema-rowsets/xml/discover-partition-dimension-stat-rowset.md)|Fournit des informations sur les attributs d'une dimension.<br /><br /> Nécessite l'ajout de SYSTEMRESTRICTSCHEMA et de paramètres supplémentaires.|  
|[Ensemble de lignes DISCOVER_PARTITION_STAT](../../analysis-services/schema-rowsets/xml/discover-partition-stat-rowset.md)|Fournit des informations sur les partitions dans une dimension, une table ou un groupe de mesures.<br /><br /> Nécessite l'ajout de SYSTEMRESTRICTSCHEMA et de paramètres supplémentaires.|  
|[Ensemble de lignes DISCOVER_PERFORMANCE_COUNTERS](../../analysis-services/schema-rowsets/xml/discover-performance-counters-rowset.md)|Répertorie les colonnes utilisées dans un compteur de performance.<br /><br /> Nécessite l'ajout de SYSTEMRESTRICTSCHEMA et de paramètres supplémentaires.|  
|[Ensemble de lignes DISCOVER_PROPERTIES](../../analysis-services/schema-rowsets/xml/discover-properties-rowset.md)|Retourne des informations sur les propriétés prises en charge par XMLA pour la source de données spécifiée.|  
|[Ensemble de lignes DISCOVER_SCHEMA_ROWSETS](../../analysis-services/schema-rowsets/xml/discover-schema-rowsets-rowset.md)|Retourne des noms, des restrictions, des descriptions et d'autres informations pour toutes les valeurs d'énumération prises en charge par XMLA.|  
|[Ensemble de lignes DISCOVER_SESSIONS](../../analysis-services/schema-rowsets/xml/discover-sessions-rowset.md)|Rend compte des sessions actives, notamment l'utilisateur de la session et la durée.|  
|[Ensemble de lignes DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS](../../analysis-services/schema-rowsets/xml/discover-storage-table-column-segments-rowset.md)|Fournit des informations au niveau de la colonne et du segment concernant les tables de stockage utilisées par une base de données Analysis Services exécutée en mode tabulaire ou SharePoint.|  
|[Ensemble de lignes DISCOVER_STORAGE_TABLE_COLUMNS](../../analysis-services/schema-rowsets/xml/discover-storage-table-columns-rowset.md)|Permet au client de déterminer l'affectation de colonnes aux tables de stockage utilisées par une base de données Analysis Services exécutée en mode tabulaire ou SharePoint.|  
|[Ensemble de lignes DISCOVER_STORAGE_TABLES](../../analysis-services/schema-rowsets/xml/discover-storage-tables-rowset.md)|Retourne des informations sur les tables utilisées pour le stockage de modèles dans une base de données model tabulaire.|  
|[Ensemble de lignes DISCOVER_TRACE_COLUMNS](../../analysis-services/schema-rowsets/xml/discover-trace-columns-rowset.md)|Retourne une description XML des colonnes disponibles dans une trace.|  
|[Ensemble de lignes DISCOVER_TRACE_DEFINITION_PROVIDERINFO](../../analysis-services/schema-rowsets/xml/discover-trace-definition-providerinfo-rowset.md)|Retourne le nom et les informations de version du fournisseur.|  
|[Ensemble de lignes DISCOVER_TRACE_EVENT_CATEGORIES](../../analysis-services/schema-rowsets/xml/discover-trace-event-categories-rowset.md)|Retourne la liste des catégories disponibles.|  
|[Ensemble de lignes DISCOVER_TRACES](../../analysis-services/schema-rowsets/xml/discover-traces-rowset.md)|Retourne la liste des traces actives sur la connexion actuelle.|  
|[Ensemble de lignes DISCOVER_TRANSACTIONS](../../analysis-services/schema-rowsets/xml/discover-transactions-rowset.md)|Retourne la liste des transactions actives sur la connexion actuelle.|  
|[Ensemble de lignes DISCOVER_XEVENT_TRACE_DEFINITION](http://msdn.microsoft.com/library/e1ce2d2d-f994-4318-801a-ee0385aecd84)|Retourne la liste des traces xevent actives sur la connexion actuelle.|  
|[Ensemble de lignes DMSCHEMA_MINING_COLUMNS](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-columns-rowset.md)|Répertorie les colonnes de tous les modèles d'exploration de données disponibles sur la connexion actuelle.|  
|[Ensemble de lignes DMSCHEMA_MINING_FUNCTIONS](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-functions-rowset.md)|Retourne la liste des fonctions prises en charge par les algorithmes d'exploration de données sur le serveur.|  
|[Ensemble de lignes DMSCHEMA_MINING_MODEL_CONTENT](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-model-content-rowset.md)|Retourne un ensemble de lignes composé de colonnes qui décrivent le modèle actuel.|  
|[Ensemble de lignes DMSCHEMA_MINING_MODEL_CONTENT_PMML](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-model-content-pmml-rowset.md)|Retourne un ensemble de lignes composé de colonnes qui décrivent le modèle actuel au format PMML.|  
|[Ensemble de lignes DMSCHEMA_MINING_MODEL_XML](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-model-xml-rowset.md)|Retourne un ensemble de lignes composé de colonnes qui décrivent le modèle actuel au format PMML.|  
|[Ensemble de lignes DMSCHEMA_MINING_MODELS](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-models-rowset.md)|Retourne la liste des modèles d'exploration de données dans la base de données active.|  
|[Ensemble de lignes DMSCHEMA_MINING_SERVICE_PARAMETERS](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-service-parameters-rowset.md)|Retourne la liste des paramètres des algorithmes sur le serveur.|  
|[Ensemble de lignes DMSCHEMA_MINING_SERVICES](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-services-rowset.md)|Fournit la liste des algorithmes d'exploration de données disponibles sur le serveur.|  
|[Ensemble de lignes DMSCHEMA_MINING_STRUCTURE_COLUMNS](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-structure-columns-rowset.md)|Retourne la liste de toutes les colonnes de tous les modèles d'exploration de données disponibles dans la connexion actuelle.|  
|[Ensemble de lignes DMSCHEMA_MINING_STRUCTURES](../../analysis-services/schema-rowsets/data-mining/dmschema-mining-structures-rowset.md)|Répertorie les structures d'exploration de données disponibles dans la connexion actuelle.|  
|[Ensemble de lignes MDSCHEMA_CUBES](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-cubes-rowset.md)|Retourne des informations sur les cubes définis dans la base de données active.|  
|[Ensemble de lignes MDSCHEMA_DIMENSIONS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-dimensions-rowset.md)|Retourne des informations sur les dimensions définies dans la base de données active.|  
|[Ensemble de lignes MDSCHEMA_FUNCTIONS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-functions-rowset.md)|Retourne la liste des fonctions disponibles pour les applications clientes connectées à la base de données.|  
|[Ensemble de lignes MDSCHEMA_HIERARCHIES](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-hierarchies-rowset.md)|Retourne des informations sur les hiérarchies définies dans la base de données active.|  
|[Ensemble de lignes MDSCHEMA_INPUT_DATASOURCES](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-input-datasources-rowset.md)|Retourne des informations sur les objets source de données définis dans la base de données active.|  
|[Ensemble de lignes MDSCHEMA_KPIS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-kpis-rowset.md)|Retourne des informations sur les indicateurs de performance clés (KPI) définis dans la base de données active.|  
|[Ensemble de lignes MDSCHEMA_LEVELS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-levels-rowset.md)|Retourne des informations sur les niveaux au sein des hiérarchies définies dans la base de données active.|  
|[Ensemble de lignes MDSCHEMA_MEASUREGROUP_DIMENSIONS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-measuregroup-dimensions-rowset.md)|Répertorie la dimension des groupes de mesures.|  
|[Ensemble de lignes MDSCHEMA_MEASUREGROUPS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-measuregroups-rowset.md)|Retourne la liste de groupes de mesures dans la connexion actuelle.|  
|[Ensemble de lignes MDSCHEMA_MEASURES](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-measures-rowset.md)|Retourne la liste des mesures dans la connexion actuelle.|  
|[Ensemble de lignes MDSCHEMA_MEMBERS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-members-rowset.md)|Retourne la liste de tous les membres de la connexion actuelle, répertoriés par base de données, cube et dimension.|  
|[Ensemble de lignes MDSCHEMA_PROPERTIES](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-properties-rowset.md)|Retourne le nom complet de chaque propriété, avec le type de propriété, le type de données et d'autres métadonnées.|  
|[Ensemble de lignes MDSCHEMA_SETS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-sets-rowset.md)|Retourne la liste des ensembles qui sont définis dans la connexion actuelle.|  
  
## <a name="see-also"></a>Voir aussi   
 [New System.Discover_Object_Activity](http://go.microsoft.com/fwlink/?linkid=221322)   
 [Nouvelle fonction SYSTEMRESTRICTEDSCHEMA pour les ensembles de lignes restreints et les vues de gestion dynamique](http://go.microsoft.com/fwlink/?LinkId=231885)  
  
  
