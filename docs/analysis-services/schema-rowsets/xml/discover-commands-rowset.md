---
title: Ensemble de lignes DISCOVER_COMMANDS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ba32ff2992e02b8b9e3fe452544f3a9b978d34f6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discovercommands-rowset"></a>Ensemble de lignes DISCOVER_COMMANDS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit des informations sur l'activité et l'utilisation des ressources des dernières commandes exécutées ou des commandes en cours d'exécution sur les connexions ouvertes sur le serveur.  
  
 **S'applique à :** modèles tabulaires, modèles multidimensionnels  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_COMMANDS** contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Restriction| Description|  
|-----------------|--------------------|-----------------|-----------------|  
|**SESSION_SPID**|**DBTYPE_I4**|Oui|ID de session.|  
|**SESSION_COMMAND_COUNT**|**DBTYPE_I4**||Nombre de commandes exécutées depuis le début de la session.|  
|**COMMAND_START_TIME**|**DBTYPE_DBTIMESTAMP**||Date et heure de démarrage de la dernière commande, exprimées en heure UTC sur le serveur.|  
|**COMMAND_ELAPSED_TIME_MS**|**DBTYPE_I8**||Durée écoulée, en millisecondes, depuis le début de l'exécution de la commande.|  
|**COMMAND_CPU_TIME_MS**|**DBTYPE_I8**||Temps processeur, en millisecondes, utilisé par la commande depuis le début de son exécution.|  
|**COMMAND_READS**|**DBTYPE_I8**||Nombre cumulé de lectures sur le disque depuis le début de l'exécution de la commande.|  
|**COMMAND_READ_KB**|**DBTYPE_I8**||Valeur cumulée des données lues à partir du disque, en kilo-octets, depuis le début de l'exécution de la commande.|  
|**COMMAND_WRITES**|**DBTYPE_I8**||Nombre cumulé d'écritures sur le disque depuis le début de l'exécution de la commande.|  
|**COMMAND_WRITE_KB**|**DBTYPE_I8**||Valeur cumulée des données écrites sur le disque, en kilo-octets, depuis le début de l'exécution de la commande.|  
|**COMMAND_TEXT**|**DBTYPE_WSTR**||Texte de la commande.|  
|**COMMAND_END_TIME**|**DBTYPE_DBTIMESTAMP**||Date et heure UTC du serveur auxquelles l'exécution de la commande s'est achevée.|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="using-adomdnet-to-return-the-rowset"></a>Utilisation d'ADOMD.NET pour retourner l'ensemble de lignes  
 Lorsque vous utilisez ADOMD.NET et l'ensemble de lignes de schéma pour récupérer des métadonnées, vous pouvez utiliser un GUID ou une chaîne pour référencer un objet d'ensemble de lignes de schéma dans la méthode GetSchemaDataSet. Pour plus d'informations, consultez [Working with Schema Rowsets in ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-working-with-schema-rowsets.md).  
  
 Le tableau suivant fournit le GUID et les valeurs de chaîne qui identifient cet ensemble de lignes.  
  
|Argument|Valeur|  
|--------------|-----------|  
|GUID|a07ccd34-8148-11d0-87bb-00c04fc33942|  
|ADOMDNAME|Commandes|  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
