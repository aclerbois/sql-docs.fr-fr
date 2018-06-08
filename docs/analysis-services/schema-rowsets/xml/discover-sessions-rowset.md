---
title: Ensemble de lignes DISCOVER_SESSIONS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 986fe462d5582f86fab56a28751b48475cbeb74e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoversessions-rowset"></a>Ensemble de lignes DISCOVER_SESSIONS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit des informations sur l'activité et l'utilisation des ressources des sessions actuellement ouvertes sur le serveur.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_SESSIONS** contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**SESSION_COMMAND_COUNT**|**DBTYPE_I4**||Nombre de commandes dont l'exécution a commencé depuis le début de la session.|  
|**SESSION_CONNECTION_ID**|**DBTYPE_I4**||Identificateur de connexion de la session.|  
|**SESSION_CPU_TIME_MS**|**DBTYPE_UI8**||Temps processeur, en millisecondes, consommé par toutes les requêtes depuis le début de la session.|  
|**SESSION_CURRENT_DATABASE**|**DBTYPE_WSTR**||Nom de la base de données utilisée par l'exécution de la commande actuelle, ou nom de la base de données qui a été utilisée par la dernière commande exécutée.|  
|**SESSION_ELAPSED_TIME_MS**|**DBTYPE_UI8**||Durée écoulée, en millisecondes, depuis le début de la session.|  
|**SESSION_ID**|**DBTYPE_WSTR**||Identificateur unique de session, tel qu'un GUID.|  
|**SESSION_IDLE_TIME_MS**|**DBTYPE_UI8**||Durée d'inactivité, en millisecondes, depuis le début de la session.|  
|**SESSION_LAST_COMMAND**|**DBTYPE_WSTR**||Texte de la commande en cours d'exécution ou de la dernière commande exécutée.|  
|**SESSION_LAST_COMMAND_CPU_TIME_MS**|**DBTYPE_UI8**||Temps processeur, en millisecondes, consommé par **SESSION_LAST_COMMAND**.|  
|**SESSION_LAST_COMMAND_ELAPSED_TIME_MS**|**DBTYPE_UI8**||Durée écoulée, en millisecondes, depuis le début de **SESSION_LAST_COMMAND**.|  
|**SESSION_LAST_COMMAND_END_TIME**|**DBTYPE_DBTIMESTAMP**||Heure UTC du serveur au moment de la fin de l'exécution de la dernière commande.|  
|**SESSION_LAST_COMMAND_START_TIME**|**DBTYPE_DBTIMESTAMP**||Heure UTC du serveur au moment du début de l'exécution de la dernière commande.|  
|**SESSION_PROPERTIES**|**DBTYPE_WSTR**||Réservé pour un usage ultérieur.|  
|**SESSION_READ_KB**|**DBTYPE_UI8**||Valeur cumulée des données lues à partir du disque, en kilo-octets, depuis le début de la session.|  
|**SESSION_READS**|**DBTYPE_UI8**||Nombre cumulé de lectures sur le disque depuis le début de la session.|  
|**SESSION_SPID**|**DBTYPE_I4**||ID de session.|  
|**SESSION_START_TIME**|**DBTYPE_DBTIMESTAMP**||Date et heure auxquelles la session a démarré, sous forme d'heure UTC du serveur.|  
|**SESSION_STATUS**|**DBTYPE_I4**||État des activités de la session.<br /><br /> 0 signifie « Inactif » : aucune activité n'est en cours.<br /><br /> 1 signifie « Actif » : la session exécute une tâche demandée.<br /><br /> 2 signifie « Bloqué » : la session attend une ressource pour continuer l'exécution de la tâche suspendue.<br /><br /> 3 signifie « Annulation » : la session a été marquée comme annulée.|  
|**SESSION_USED_MEMORY**|**DBTYPE_I4**||Quantité actuelle de mémoire utilisée par la session, en kilo-octets. La valeur indiquée est l'utilisation de la RAM par SPID, sans distinction entre la mémoire réductible et non réductible. Contrairement à d'autres vues DMVS qui indiquent l'utilisation de la mémoire, DISCOVER_SESSIONS ne détaille par l'utilisation de la mémoire par catégorie.<br /><br /> Notez que SESSION_USED_MEMORY tend à sous-estimer l'utilisation de mémoire réelle car elle exclut les objets partagés entre plusieurs sessions.  Seuls les objets qui sont uniques à la session sont représentés dans le calcul de la mémoire.|  
|**SESSION_USER_NAME**|**DBTYPE_WSTR**||Nom d'utilisateur de la session.|  
|**SESSION_WRITE_KB**|**DBTYPE_UI8**||Valeur cumulée des données écrites sur le disque, en kilo-octets, depuis le début de la session.|  
|**SESSION_WRITES**|**DBTYPE_UI8**||Nombre cumulé d'écritures sur le disque depuis le début de la session.|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **DISCOVER_SESSIONS** peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|SESSION_ID|DBTYPE_WSTR|Ce paramètre est facultatif.|  
|SESSION_SPID|DBTYPE_I4|Ce paramètre est facultatif.|  
|SESSION_CONNECTION_ID|DBTYPE_I4|Ce paramètre est facultatif.|  
|SESSION_USER_NAME|DBTYPE_WSTR|Ce paramètre est facultatif.|  
|SESSION_CURRENT_DATABASE|DBTYPE_WSTR|Ce paramètre est facultatif.|  
|SESSION_ELAPSED_TIME_MS|DBTYPE_UI8|Ce paramètre est facultatif.|  
|SESSION_CPU_TIME_MS|DBTYPE_UI8|Ce paramètre est facultatif.|  
|SESSION_IDLE_TIME_MS|DBTYPE_UI8|Ce paramètre est facultatif.|  
|SESSION_STATUS|DBTYPE_I4|Ce paramètre est facultatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
