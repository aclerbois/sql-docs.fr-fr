---
title: sp_dbmmonitorhelpalert (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_dbmmonitorhelpalert_TSQL
- sp_dbmmonitorhelpalert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorhelpalert
- database mirroring [SQL Server], monitoring
ms.assetid: 43911660-b4e4-4934-8c02-35221160aaec
caps.latest.revision: 40
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: b05d7449322bd35bf924a5fb12b0cf72ff383d28
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdbmmonitorhelpalert-transact-sql"></a>sp_dbmmonitorhelpalert (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur des seuils d'avertissement sur une ou l'ensemble des métriques de performances clés du moniteur de mise en miroir de bases de données.  
 
  ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dbmmonitorhelpalert database_name   
    [ , alert_id ]   
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Spécifie la base de données.  
  
 [ *id_alerte* ]  
 Entier qui identifie l'avertissement à renvoyer. Si cet argument est omis, tous les avertissements sont renvoyés, mais pas la période de rétention.  
  
 Pour renvoyer un avertissement spécifique, indiquez l'une des valeurs suivantes :  
  
|Valeur|Mesure de performance|Seuil d'avertissement|  
|-----------|------------------------|-----------------------|  
|1|Transaction non envoyée la plus ancienne|Spécifie le nombre de minutes de transactions pouvant s'accumuler dans la file d'attente d'envoi avant qu'un avertissement ne soit généré sur l'instance de serveur principal. Cet avertissement permet de mesurer le risque de perte de données en termes de temps et s'avère particulièrement approprié en mode hautes performances. Toutefois, l'avertissement est également approprié en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue en raison de la déconnexion des partenaires.|  
|2|Journal non envoyé|Spécifie la quantité de kilo-octets (Ko) de journal non envoyé qui génère un avertissement sur l'instance de serveur principal. Cet avertissement permet de mesurer le risque de perte de données en termes de Ko, et il est particulièrement utile pour le mode hautes performances. Toutefois, l'avertissement est également approprié en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue en raison de la déconnexion des partenaires.|  
|3|Journal non restauré|Spécifie la quantité de Ko de journal non restauré qui génère un avertissement sur l'instance de serveur miroir. Cet avertissement permet de mesurer le temps de basculement. Le*temps de basculement* est principalement constitué du temps nécessaire à l'ancien serveur miroir pour restaurer par progression tout journal demeuré dans sa file d'attente de restauration par progression et d'un court laps de temps supplémentaire.|  
|4|Charge de validation par le serveur miroir|Spécifie le nombre de millisecondes de délai moyen par transaction qui sont tolérés avant qu'un avertissement soit généré sur le serveur principal. Ce délai correspond au temps de traitement pendant lequel l'instance de serveur principal attend que l'instance de serveur miroir écrive l'enregistrement du journal de transaction dans la file d'attente de restauration par progression. Cette valeur est utile uniquement en mode haute sécurité.|  
|5|Période de rétention|Métadonnées qui déterminent la durée de conservation des lignes dans la table de l'état des mises en miroir de base de données.|  
  
 Pour plus d’informations sur les ID d’événement correspondant aux avertissements, consultez [utiliser des seuils d’avertissement et d’alertes sur les métriques de performances de mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Pour chaque alerte retournée, renvoie une ligne contenant les colonnes suivantes :  
  
|Colonne|Data type| Description|  
|------------|---------------|-----------------|  
|**alert_id**|**int**|Le tableau ci-dessous répertorie les **id_alerte** valeur pour chaque mesure de performance et l’unité de mesure de la mesure affichée dans le **sp_dbmmonitorresults** jeu de résultats :|  
|**Seuil**|**int**|Valeur de seuil de l'avertissement. Si une valeur supérieure à ce seuil est renvoyée lorsque l'état des mises en miroir est mis à jour, une entrée est insérée dans le journal des événements Windows. Cette valeur est exprimée en Ko, minutes ou millisecondes, suivant l'avertissement. Si le seuil n'est pas actuellement défini, la valeur est NULL.<br /><br /> **Remarque :** pour afficher les valeurs actuelles, exécutez le [sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md) procédure stockée.|  
|**enabled**|**bit**|0 = L'événement est désactivé.<br /><br /> 1 = L'événement est activé.<br /><br /> **Remarque :** période de rétention est toujours activée.|  
  
|Valeur|Mesure de performance|Unité|  
|-----------|------------------------|----------|  
|1|Transaction non envoyée la plus ancienne|Minutes|  
|2|Journal non envoyé|Ko|  
|3|Journal non restauré|Ko|  
|4|Charge de validation par le serveur miroir|Millisecondes|  
|5|Période de rétention|Heures|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne une ligne qui indique si un avertissement est activé sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] pour la mesure de performance de la plus ancienne transaction non envoyée.  
  
```  
EXEC sp_dbmmonitorhelpalert AdventureWorks2012, 1 ;  
```  
  
 L'exemple suivant renvoie, pour chaque mesure de performance, une ligne indiquant si elle est activée sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC sp_dbmmonitorhelpalert AdventureWorks2012;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)   
 [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)   
 [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
