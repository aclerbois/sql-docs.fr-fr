---
title: DROP EVENT NOTIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DROP EVENT NOTIFICATION
- DROP_EVENT_NOTIFICATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event notifications [SQL Server], removing
- deleting event notifications
- dropping event notifications
- DROP EVENT NOTIFICATION statement
- removing event notifications
ms.assetid: 0ffd8f47-4ea3-4238-9e73-c318df710cf7
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a47ff204433d49e76fe18f772d37fcc8589123aa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drop-event-notification-transact-sql"></a>DROP EVENT NOTIFICATION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime un déclencheur de notification d'événement de la base de données active.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DROP EVENT NOTIFICATION notification_name [ ,...n ]  
ON { SERVER | DATABASE | QUEUE queue_name }  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 *notification_name*  
 Nom de la notification d'événement à supprimer. Plusieurs notifications d'événement peuvent être spécifiées. Pour afficher la liste des notifications d’événements actuellement créées, utilisez [sys.event_notifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md).  
  
 SERVER  
 Indique que l'étendue de la notification d'événement s'applique au serveur actif. L'argument SERVER doit être spécifié s'il a été indiqué lors de la création de la notification d'événement.  
  
 DATABASE  
 Indique que l'étendue de la notification d'événement s'applique à la base de données active. L'argument DATABASE doit être spécifié s'il a été indiqué lors de la création de la notification d'événement.  
  
 QUEUE *queue_name*  
 Indique que l’étendue de la notification d’événement s’applique à la file d’attente spécifiée par *queue_name*. L'argument QUEUE doit être spécifié s'il a été indiqué lors de la création de la notification d'événement. *queue_name* est le nom de la file d’attente et doit également être spécifié.  
  
## <a name="remarks"></a>Notes   
 Si une notification d'événement se déclenche dans une transaction et est supprimée dans la même transaction, l'instance de notification d'événement est envoyée, puis la notification d'événement est supprimée.  
  
## <a name="permissions"></a>Autorisations  
 Pour supprimer une notification d'événement qui s'étend au niveau de la base de données, un utilisateur doit, au minimum, être propriétaire de la notification d'événement ou posséder une autorisation ALTER ANY DATABASE EVENT NOTIFICATION sur la base de données active.  
  
 Pour supprimer une notification d'événement étendue au niveau du serveur, un utilisateur doit, au minimum, être propriétaire de la notification d'événement ou posséder une autorisation ALTER ANY EVENT NOTIFICATION sur le serveur.  
  
 Pour supprimer une notification d'événement portant sur une file d'attente spécifique, un utilisateur doit, au minimum, être propriétaire de la notification d'événement ou posséder une autorisation ALTER sur la file d'attente parent.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une notification d'événement étendue au niveau de la base de données, puis la supprime :  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE EVENT NOTIFICATION NotifyALTER_T1  
ON DATABASE  
FOR ALTER_TABLE  
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984';  
GO  
DROP EVENT NOTIFICATION NotifyALTER_T1  
ON DATABASE;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-notification-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.event_notifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md)   
 [sys.events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-events-transact-sql.md)  
  
  
