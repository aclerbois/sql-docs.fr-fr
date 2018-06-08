---
title: Sys.dm_external_script_execution_stats | Documents Microsoft
ms.custom: ''
ms.date: 09/16/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_external_script_execution_stats
- sys.dm_external_script_execution_stats_TSQL
- dm_external_script_execution_stats
- dm_external_script_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_external_script_execution_stats dynamic management view
ms.assetid: 2e99f026-ceb2-42a2-a549-c71d31ed0cf4
caps.latest.revision: 5
author: jeannt
ms.author: jeannt
manager: craigg
ms.openlocfilehash: 01380a29665d848fff1620787a97aabbcdac4033
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexternalscriptexecutionstats"></a>sys.dm_external_script_execution_stats
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]


  Renvoie une ligne pour chaque type de demande de script externe. Les demandes de script externe sont regroupées par le langage de script externe pris en charge. Une ligne est générée pour chaque fonction enregistrée de script externe. Les fonctions de script externe ne sont pas enregistrées, sauf si elle sont envoyées par un processus parent, comme `rxExec`.

 
  
> [!NOTE]  
>  Cette vue de gestion dynamique est disponible uniquement si vous avez installé et activé la fonctionnalité qui prend en charge l’exécution du script externe. Pour plus d’informations sur la manière de procéder pour les scripts R, consultez [Configurer SQL Server R Services](../../advanced-analytics/r-services/set-up-sql-server-r-services-in-database.md).  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|langue|**nvarchar**|Nom du langage enregistré de script externe. Chaque script externe doit spécifier le langage dans la demande de requête utilisé pour le démarrage du lanceur associé. |  
|counter_name|**nvarchar**|Nom de la fonction enregistrée de script externe. N'accepte pas la valeur NULL.|  
|counter_value|**entier**|Nombre total d’instance appelée de la fonction enregistrée de script externe sur le serveur. Cette valeur, cumulative, commence par l’horodatage d’installation de la fonction sur l’instance. Elle ne peut pas être réinitialisée.|  

  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW SERVER STATE sur le serveur.  
  
> [!NOTE]  
>  Les utilisateurs qui exécutent des scripts externes doivent avoir l’autorisation supplémentaire EXECUTE ANY EXTERNAL SCRIPT, toutefois, cette vue de gestion dynamique peut être utilisée par les administrateurs sans cette autorisation. 
  
## <a name="remarks"></a>Notes  
  Cette vue de gestion dynamique est fournie pour la télémétrie interne, afin de contrôler l’utilisation générale de la nouvelle fonction d’exécution de script externe fournie dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. La télémétrie démarre au lancement de LaunchPad et incrémente un compteur sur disque à chaque fois qu’une fonction enregistrée de script externe est appelée.

En règle générale, les compteurs de performance demeurent valides tant que le processus qui les a générés reste actif. Par conséquent, une demande sur une vue de gestion dynamique ne peut pas faire état des données des services qui ne sont plus en cours d’exécution. Par exemple, si le lanceur exécute le script externe tout en les terminant très rapidement, une vue de gestion dynamique conventionnelle n’indiquera pas forcément les données.

Par conséquent, les compteurs suivis par cette vue de gestion dynamique sont conservés en cours d’exécution, tandis que l’état pour sys.dm_external_script_requests est préservé par l’utilisation d’écritures sur le disque, même si l’instance est arrêtée.

   
  
### <a name="r-counter-values"></a>Valeurs de compteur R
 Actuellement, le seul langage de script externe pris en charge dans [!INCLUDE[ssCurrent_md](../../includes/sscurrent-md.md)] est R. Les demandes de script externes pour le langage R sont traitées par [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)]. 

Pour R, cette DMV suit le nombre d’appels de R sont effectuées sur une instance. Par exemple, si `rxLinMod` est appelé et s’exécute en parallèle, le compteur est incrémenté d’une unité.
 
Pour le langage R, les valeurs de compteur affichées dans le champ *counter_name* représentent le nom des fonctions ScaleR enregistrées. Les valeurs du champ *counter_value* représentent le nombre cumulé des instances de fonctions spécifiques ScaleR. 

Le comptage commence quand la fonction est installée et activée sur l’instance. Il est cumulé jusqu’à ce que le fichier qui gère l’état soit supprimé ou remplacé par un administrateur. Par conséquent, il n’est généralement pas possible de réinitialiser les valeurs de *counter_value*. Si vous souhaitez contrôler l’utilisation par session, période calendaire ou tout autre intervalle, nous vous recommandons de capturer les nombres dans un tableau.

### <a name="registration-of-external-script-functions"></a>Inscription des fonctions de script externe

Le langage R prend en charge les scripts arbitraires, tandis que la communauté R fournit plusieurs milliers de packages, qui présentent tous leurs propres fonctions et méthodes. Toutefois, cette vue de gestion dynamique contrôle uniquement les fonctions ScaleR installées avec SQL Server R Services.

L’inscription de ces fonctions est effectuée lors de leur installation ; les fonctions inscrites ne peuvent pas être ajoutées ou supprimées.

## <a name="examples"></a>Exemples  
  
### <a name="viewing-the-number-of-r-scripts-run-on-the-server"></a>Affichage du nombre de scripts R exécutés sur le serveur  
 L’exemple suivant affiche le nombre cumulé d’exécutions du script externe pour le langage R.  
  
```  
SELECT counter_name, counter_value   
FROM sys.dm_external_script_execution_stats   
WHERE language = 'R';
```  

  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique et fonctions associées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md) 
[sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)  
  

