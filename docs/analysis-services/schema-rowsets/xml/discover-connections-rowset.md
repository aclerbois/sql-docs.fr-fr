---
title: Ensemble de lignes DISCOVER_CONNECTIONS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4bb236b6d69199bd4c488c365ba108fc3913f02c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverconnections-rowset"></a>Ensemble de lignes DISCOVER_CONNECTIONS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit des informations sur l'activité et l'utilisation des ressources des connexions actuellement ouvertes sur le serveur.  
  
 **S'applique à :** modèles tabulaires, modèles multidimensionnels  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_CONNECTIONS** contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Restrictions| Description|  
|-----------------|--------------------|------------------|-----------------|  
|**ID_CONNEXION**|**DBTYPE_I4**|Oui|Numéro unique qui identifie la connexion.|  
|**CONNECTION_USER_NAME**|**DBTYPE_WSTR**|Oui|Nom d'utilisateur de la connexion.|  
|**CONNECTION_IMPERSONATED_USER_NAME**|**DBTYPE_WSTR**|Oui|Réservé pour un usage ultérieur. Analysis Services retourne toujours NULL pour la valeur de CONNECTION_IMPERSONATED_USER_NAME.|  
|**CONNECTION_HOST_NAME**|**DBTYPE_WSTR**|Oui|Nom de l'ordinateur qui a initialisé la connexion.|  
|**CONNECTION_HOST_APPLICATION**|**DBTYPE_WSTR**||Nom de l'application qui a initialisé la connexion.|  
|**CONNECTION_START_TIME**|**DBTYPE_DBTIMESTAMP**||Date et heure UTC du serveur auxquelles la connexion a été établie.|  
|**CONNECTION_ELAPSED_TIME_MS**|**DBTYPE_I8**|Oui|Durée écoulée, en millisecondes, depuis l'établissement de la connexion.|  
|**CONNECTION_LAST_COMMAND_START_TIME**|**DBTYPE_DBTIMESTAMP**||Date et heure UTC du serveur auxquelles l'exécution de la dernière commande a débuté.|  
|**CONNECTION_LAST_COMMAND_END_TIME**|**DBTYPE_DBTIMESTAMP**||Date et heure UTC du serveur auxquelles l'exécution de la dernière commande s'est achevée.|  
|**CONNECTION_LAST_COMMAND_ELAPSED_TIME_MS**|**DBTYPE_I8**|Oui|Durée écoulée, en millisecondes, depuis la fin de la dernière commande exécutée.|  
|**CONNECTION_IDLE_TIME_MS**|**DBTYPE_I8**|Oui|Durée d'inactivité, en millisecondes, depuis l'établissement de la connexion.|  
|**CONNECTION_BYTES_SENT**|**DBTYPE_I8**||Nombre cumulé d'octets envoyés par la connexion depuis l'établissement de la connexion.|  
|**CONNECTION_DATA_BYTES_SENT**|**DBTYPE_I8**||Nombre cumulé d'octets de données envoyés par la connexion depuis l'établissement de la connexion.<br /><br /> Les données transitent au format compressé dans la connexion ; cette valeur représente les données étendues envoyées.|  
|**CONNECTION_BYTES_RECEIVED**|**DBTYPE_I8**||Nombre cumulé d'octets reçus par la connexion depuis l'établissement de la connexion.|  
|**CONNECTION_DATA_BYTES_RECEIVED**|**DBTYPE_I8**||Nombre cumulé d'octets de données reçus par la connexion depuis l'établissement de la connexion.<br /><br /> Les données transitent au format compressé dans la connexion ; cette valeur représente les données étendues reçues.|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="using-adomdnet-to-return-the-rowset"></a>Utilisation d'ADOMD.NET pour retourner l'ensemble de lignes  
 Lorsque vous utilisez ADOMD.NET et l'ensemble de lignes de schéma pour récupérer des métadonnées, vous pouvez utiliser un GUID ou une chaîne pour référencer un objet d'ensemble de lignes de schéma dans la méthode GetSchemaDataSet. Pour plus d'informations, consultez [Working with Schema Rowsets in ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-working-with-schema-rowsets.md).  
  
 Le tableau suivant fournit le GUID et les valeurs de chaîne qui identifient cet ensemble de lignes.  
  
|Argument|Valeur|  
|--------------|-----------|  
|GUID|a07ccd25-8148-11d0-87bb-00c04fc33942|  
|ADOMDNAME|Connexions|  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
