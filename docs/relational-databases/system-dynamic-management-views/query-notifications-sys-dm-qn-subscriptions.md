---
title: Sys.dm_qn_subscriptions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_qn_subscriptions
- dm_qn_subscriptions_TSQL
- sys.dm_qn_subscriptions
- sys.dm_qn_subscriptions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_qn_subscriptions dynamic management view
ms.assetid: a3040ce6-f5af-48fc-8835-c418912f830c
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e3f6886a16b8b1d87c2864ed93fd8be764700dc5
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="query-notifications---sysdmqnsubscriptions"></a>-Les Notifications de requête sys.dm_qn_subscriptions
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les abonnements aux notifications de requêtes actifs dans le serveur. Vous pouvez utiliser cette vue pour vérifier les abonnements actifs dans le serveur ou une base de données spécifiée, ou pour vérifier un principal de serveur.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID d'un abonnement.|  
|**database_id**|**int**|ID de la base de données dans laquelle la requête de notification a été exécutée. Cette base de données stocke les informations relatives à cet abonnement.|  
|**sid**|**varbinary(85)**|ID de sécurité du principal du serveur qui a créé cet abonnement et qui en est propriétaire.|  
|**object_id**|**int**|ID de la table interne qui stocke les informations sur les paramètres de l'abonnement.|  
|**created**|**datetime**|Date et heure de création de l’abonnement.|  
|**Délai d’attente**|**int**|Délai d'expiration de l'abonnement, en secondes. La notification est marquée pour se déclencher après ce délai.<br /><br /> Remarque : L’heure de déclenchement réel peut dépasser le délai d’attente spécifié. Cependant, si une modification qui invalide l'abonnement a lieu après le délai d'expiration, mais avant le déclenchement de l'abonnement, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fait en sorte que le déclenchement ait lieu au moment de la modification.|  
|**status**|**int**|Indique l'état de l'abonnement. Consultez le tableau sous la section Remarques pour obtenir la liste de codes.|  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|From|Pour|Actif|Type|  
|----------|--------|--------|----------|  
|**sys.dm_qn_subscriptions**|**sys.databases**|**database_id**|Plusieurs-à-un|  
|**sys.dm_qn_subscriptions**|**sys.internal_tables**|**object_id**|Plusieurs-à-un|  
  
## <a name="remarks"></a>Notes  
 Le code d'état 0 indique un état indéfini.  
  
 Les codes d'état suivants indiquent qu'un abonnement a été déclenché à cause d'une modification :  
  
|Code|État secondaire|Info|  
|----------|------------------|----------|  
|65798|L'abonnement a été déclenché parce que les données ont été modifiées|Abonnement déclenché par insertion|  
|65799|L'abonnement a été déclenché parce que les données ont été modifiées|Supprimer|  
|65800|L'abonnement a été déclenché parce que les données ont été modifiées|Update|  
|65801|L'abonnement a été déclenché parce que les données ont été modifiées|Fusion|  
|65802|L'abonnement a été déclenché parce que les données ont été modifiées|Troncation de la table|  
|66048|L'abonnement a été déclenché parce que le délai d'attente a expiré|Mode d'information indéfini|  
|66315|L'abonnement a été déclenché parce que l'objet a été modifié|L'objet ou l'utilisateur a été supprimé|  
|66316|L'abonnement a été déclenché parce que l'objet a été modifié|L'objet a été modifié|  
|66565|L'abonnement a été déclenché parce que la base de données a été détachée ou supprimée|Serveur ou db redémarré|  
|66571|L'abonnement a été déclenché parce que la base de données a été détachée ou supprimée|L'objet ou l'utilisateur a été supprimé|  
|66572|L'abonnement a été déclenché parce que la base de données a été détachée ou supprimée|L'objet a été modifié|  
|67341|L'abonnement a été déclenché en raison d'un manque de ressources sur le serveur|L'abonnement a été déclenché en raison d'un manque de ressources sur le serveur|  
  
 Les codes d'état suivants indiquent que la création d'un abonnement a échoué :  
  
|Code|État secondaire|Info|  
|----------|------------------|----------|  
|132609|La création d'un abonnement a échoué parce que l'instruction n'est pas prise en charge|La requête est trop complexe|  
|132610|La création d'un abonnement a échoué parce que l'instruction n'est pas prise en charge|Instruction non valide pour l'abonnement|  
|132611|La création d'un abonnement a échoué parce que l'instruction n'est pas prise en charge|Options définies non valides pour l'abonnement|  
|132612|La création d'un abonnement a échoué parce que l'instruction n'est pas prise en charge|Niveau d'isolation non valide|  
|132622|La création d'un abonnement a échoué parce que l'instruction n'est pas prise en charge|Utilisation en interne|  
|132623|La création d'un abonnement a échoué parce que l'instruction n'est pas prise en charge|Au-delà de la limite de modèle par table|  
  
 Les codes d'état suivants sont utilisés en interne et classés comme fin de contrôle et modes d'initialisation :  
  
|Code|État secondaire|Info|  
|----------|------------------|----------|  
|198656|Utilisation en interne : fin de contrôle et modes d'initialisation|Mode d'information indéfini|  
|198928|L'abonnement a été détruit|L'abonnement a été déclenché parce que db a été attaché|  
|198929|L'abonnement a été détruit|L'abonnement a été déclenché parce que l'utilisateur a été supprimé|  
|198930|L'abonnement a été détruit|L'abonnement a été supprimé à cause d'un nouvel abonnement|  
|198931|L'abonnement a été détruit|L'abonnement a été arrêté|  
|199168|L'abonnement est actif|Mode d'information indéfini|  
|199424|L'abonnement a été initialisé, mais n'est pas encore actif|Mode d'information indéfini|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW SERVER STATE sur le serveur.  
  
> [!NOTE]  
>  Si l'utilisateur n'a pas l'autorisation VIEW SERVER STATE, cette vue retourne des informations sur les abonnements qui appartiennent à l'utilisateur actuel.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-return-active-query-notification-subscriptions-for-the-current-user"></a>A. Retour des abonnements aux notifications de requêtes actives pour l'utilisateur actif  
 L'exemple suivant retourne les abonnements aux notifications de requêtes actives de l'utilisateur actif. Si l'utilisateur possède les autorisations VIEW SERVER STATE, tous les abonnements actifs du serveur sont retournés.  
  
```  
SELECT id, database_id, sid, object_id, created, timeout, status  
FROM sys.dm_qn_subscriptions;  
GO  
```  
  
### <a name="b-returning-active-query-notification-subscriptions-for-a-specified-user"></a>B. Retour des abonnements aux notifications de requêtes actives pour un utilisateur spécifié  
 L'exemple suivant retourne les abonnements aux notifications de requêtes actives auxquelles souscrit la connexion `Ruth0`.  
  
```  
SELECT id, database_id, sid, object_id, created, timeout, status  
FROM sys.dm_qn_subscriptions  
WHERE sid = SUSER_SID('Ruth0');  
GO  
```  
  
### <a name="c-returning-internal-table-metadata-for-query-notification-subscriptions"></a>C. Retour des métadonnées des tables internes pour les abonnements aux notifications de requêtes  
 L'exemple suivant retourne les métadonnées des tables internes pour les abonnements aux notifications de requêtes.  
  
```  
SELECT qn.id AS query_subscription_id  
    ,it.name AS internal_table_name  
    ,it.object_id AS internal_table_id  
FROM sys.internal_tables AS it  
JOIN sys.dm_qn_subscriptions AS qn ON it.object_id = qn.object_id  
WHERE it.internal_type_desc = 'QUERY_NOTIFICATION';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées aux Notifications de requête &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/92eb22d8-33f3-4c17-b32e-e23acdfbd8f4)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)  
  
  
