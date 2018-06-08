---
title: Fonctionnalités de recherche en texte intégral dépréciées dans SQL Server 2016 | Microsoft Docs
ms.custom: ''
ms.date: 08/19/2016
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [full-text search]
- full-text search [SQL Server], deprecated features
- full-text queries [SQL Server], proximity
ms.assetid: ab0d799c-ba79-4459-837b-c4862730dafd
caps.latest.revision: 33
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8f3e4bad6ef6d9e35af55a6ca6f6395fd2bfde13
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="deprecated-full-text-search-features-in-sql-server-2016"></a>Fonctionnalités de recherche en texte intégral déconseillées dans SQL Server 2016
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique décrit les fonctionnalités de la recherche en texte intégral qui sont dépréciées et toujours disponibles dans SQL Server. Il est prévu que ces fonctionnalités soient supprimées dans une prochaine version. N’utilisez pas de fonctions déconseillées dans les nouvelles applications.  
  
Surveillez l’utilisation des fonctionnalités dépréciées à l’aide du compteur de performance de l’objet **SQL Server:Deprecated Features** et des événements de suivi. Pour plus d’informations, consultez [Utilisation des objets SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
## <a name="features-no-longer-supported"></a>Fonctionnalités abandonnées  

  
|Fonctionnalité déconseillée|Remplacement|Nom de la fonctionnalité|ID de la fonctionnalité|  
|------------------------|-----------------|------------------|----------------|  
|Propriété FULLTEXTCATALOGPROPERTY : LogSize|Aucun.|FULLTEXTCATALOGPROPERTY **('LogSize')**|211|  
|Propriété FULLTEXTSERVICEPROPERTY :<br /><br /> ConnectTimeout<br /><br /> DataTimeout|Aucun.|FULLTEXTSERVICEPROPERTY **('ConnectTimeout')**<br /><br /> FULLTEXTSERVICEPROPERTY **('DataTimeout'**)|210<br /><br /> 209|  
|sp_fulltext_catalog|CREATE FULL CATALOG<br /><br /> ALTER FULLTEXT CATALOG<br /><br /> DROP FULLTEXT CATALOG|sp_fulltext_catalog|84|  
|sp_fulltext_column<br /><br /> sp_fulltext_database<br /><br /> sp_fulltext_table|CREATE FULL INDEX<br /><br /> ALTER FULLTEXT INDEX<br /><br /> DROP FULLTEXT INDEX|sp_fulltext_column<br /><br /> sp_fulltext_database<br /><br /> sp_fulltext_table|86<br /><br /> 87<br /><br /> 85 %|  
|sp_help_fulltext_catalogs<br /><br /> sp_help_fulltext_catalog_components<br /><br /> sp_help_fulltext_catalogs_cursor<br /><br /> sp_help_fulltext_columns<br /><br /> sp_help_fulltext_columns_cursor<br /><br /> sp_help_fulltext_tables<br /><br /> sp_help_fulltext_tables_cursor|sys.fulltext_catalogs<br /><br /> sys.fulltext_index_columns<br /><br /> sys.fulltext_indexes|sp_help_fulltext_catalogs<br /><br /> sp_help_fulltext_catalog_components<br /><br /> sp_help_fulltext_catalogs_cursor<br /><br /> sp_help_fulltext_columns<br /><br /> sp_help_fulltext_columns_cursor<br /><br /> sp_help_fulltext_table<br /><br /> sp_help_fulltext_tables_cursor|88<br /><br /> 203<br /><br /> 90<br /><br /> 92<br /><br /> 93<br /><br /> 91<br /><br /> 89|  
|Valeurs d’action sp_fulltext_service : clean_up, connect_timeout et data_timeout retournent la valeur zéro|None|sp_fulltext_service @action=clean_up<br /><br /> sp_fulltext_service @action=connect_timeout<br /><br /> sp_fulltext_service @action=data_timeout|116<br /><br /> 117<br /><br /> 118|  
|Colonnes sys.dm_fts_active_catalogs :<br /><br /> is_paused<br /><br /> previous_status<br /><br /> previous_status_description<br /><br /> row_count_in_thousands<br /><br /> status<br /><br /> status_description<br /><br /> worker_count|Aucun.|dm_fts_active_catalogs.is_paused<br /><br /> dm_fts_active_catalogs.previous_status<br /><br /> dm_fts_active_catalogs.previous_status_description<br /><br /> dm_fts_active_catalogs.row_count_in_thousands<br /><br /> dm_fts_active_catalogs.status<br /><br /> dm_fts_active_catalogs.status_description<br /><br /> dm_fts_active_catalogs.worker_count|218<br /><br /> 221<br /><br /> 222<br /><br /> 224<br /><br /> 219<br /><br /> 220<br /><br /> 223|  
|Colonne sys.dm_fts_memory_buffers :<br /><br /> row_count|Aucun.|dm_fts_memory_buffers.row_count|225|  
|Colonnes sys.fulltext_catalogs :<br /><br /> path<br /><br /> data_space_id<br /><br /> Colonnes file_id|Aucun.|fulltext_catalogs.path<br /><br /> fulltext_catalogs.data_space_id<br /><br /> fulltext_catalogs.file_id|215<br /><br /> 216<br /><br /> 217|  
  
## <a name="features-not-supported-in-a-future-version-of-sql-server"></a>Fonctionnalités non prises en charge dans une future version de SQL Server  
 Les fonctionnalités suivantes de recherche en texte intégral sont prises en charge dans la prochaine version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais seront supprimées dans une version ultérieure. La version spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas été déterminée.  
  
 La valeur **Nom de la fonctionnalité** apparaît dans les événements de suivi comme ObjectName et dans les compteurs de performance et sys.dm_os_performance_counters comme nom de l’instance. La valeur **ID de la fonctionnalité** apparaît dans les événements de suivi comme ObjectId.  
  
|Fonctionnalité déconseillée|Remplacement|Nom de la fonctionnalité|ID de la fonctionnalité|  
|------------------------|-----------------|------------------|----------------|  
|Opérateur NEAR générique CONTAINS et CONTAINSTABLE :<br /><br /> {<terme_simple> &#124; <terme_préfixe>}<br /><br /> {<br /><br /> { { NEAR &#124; ~ }    {<terme_simple> &#124; <terme_préfixe>} } [...*n*]<br /><br /> }|Opérateur NEAR personnalisé :<br /><br /> NEAR(<br /><br /> {   {<terme_simple> &#124; <terme_préfixe>} [ ,…*n* ]<br /><br /> &#124; ( {<terme_simple> &#124; <terme_préfixe>} [,…*n*] )<br /><br /> [,<distance> [,<order>] ]<br /><br /> }<br /><br /> )<br /><br /> <distance> ::= {*entier* &#124; **MAX**}<br /><br /> <order> ::= {TRUE &#124; **FALSE**}|FULLTEXT_OLD_NEAR_SYNTAX|247|  
|Option CREATE FULLTEXT CATALOG :<br /><br /> IN PATH '*chemin_racine*'<br /><br /> ON FILEGROUP *filegroup*|Aucun.|CREATE FULLTEXT CATLOG IN PATH<br /><br /> Aucun.<sup>*</sup>|237<br /><br /> Aucun.*|  
|Propriété DATABASEPROPERTYEX : IsFullTextEnabled|Aucun.|DATABASEPROPERTYEX **('IsFullTextEnabled')**|202|  
|Option sp_detach_db :<br /><br /> [ @keepfulltextindexfile = ] '*conserver_fichier_index_recherche_en_texte-intégral*'|Aucun.|sp_detach_db @keepfulltextindexfile|226|  
|Valeurs d’action sp_fulltext_service : resource_usage n’a pas de fonction.|None|sp_fulltext_service @action=resource_usage|200|  
  
 *L’objet **SQL Server:Deprecated Features** ne surveille pas les occurrences de CREATE FULLTEXT CATLOG ON FILEGROUP *groupe_de_fichiers*.  
  
  
