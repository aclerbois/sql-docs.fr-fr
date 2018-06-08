---
title: sysmail_delete_log_sp (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_delete_log_sp_TSQL
- sysmail_delete_log_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_log_sp
ms.assetid: e94b37a1-70ad-46a5-86c0-721892156f7c
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: f565100bff8373839e70231b0a99a716efc76ec7
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmaildeletelogsp-transact-sql"></a>sysmail_delete_log_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime des événements du journal de la messagerie de base de données. Supprime tous les événements du journal ou uniquement les événements correspondant à des critères de date ou de type spécifiés.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_delete_log_sp  [ [ @logged_before = ] 'logged_before' ]  
    [, [ @event_type = ] 'event_type' ]  
  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@logged_before** =] **'***logged_before***'**  
 Supprime les entrées antérieures à la date et l’heure spécifiée par le *logged_before* argument. *logged_before* est **datetime** avec NULL comme valeur par défaut. La valeur NULL correspond à toutes les dates.  
  
 [ **@event_type** =] **'***event_type***'**  
 Supprime les entrées du type spécifié en tant que journal le *event_type*. *event_type* est **varchar(15)** sans valeur par défaut. Les entrées valides sont **réussite**, **avertissement**, **erreur**, et **d’information**. La valeur NULL correspond à tous les types d'événements.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Utilisez le **sysmail_delete_log_sp** procédure stockée pour supprimer définitivement les entrées de journal de la messagerie de base de données. Un argument facultatif vous permet de supprimer uniquement les enregistrements les plus anciens en fournissant une date et une heure. Les événements antérieurs à cet argument sont alors supprimés. Un argument facultatif vous permet de supprimer uniquement les événements d’un certain type, spécifié en tant que le **event_type** argument.  
  
 La suppression d'entrées dans le journal de la messagerie de base de données n'entraîne pas la suppression d'entrées de messages électroniques dans les tables de la messagerie de base de données. Utilisez [sysmail_delete_mailitems_sp](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md) pour supprimer les messages électroniques à partir des tables de la messagerie de base de données.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut accéder à cette procédure.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-deleting-all-events"></a>A. Suppression de tous les événements  
 L'exemple suivant supprime tous les événements du journal de la messagerie de base de données.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp ;  
GO  
```  
  
### <a name="b-deleting-the-oldest-events"></a>B. Suppression des événements les plus anciens  
 L'exemple suivant supprime les événements du journal de la messagerie de base de données qui sont antérieurs au 9 octobre 2005.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp  
    @logged_before = 'October 9, 2005' ;  
GO  
```  
  
### <a name="c-deleting-all-events-of-a-certain-type"></a>C. Suppression d'un certain type d'événement  
 L'exemple suivant supprime tous les messages de succès du journal de la messagerie de base de données.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp  
    @event_type = 'success' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)   
 [sysmail_delete_mailitems_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md)   
 [Créer un travail d'Agent SQL Server pour archiver les messages et les journaux d'événements de la messagerie de base de données](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)  
  
  
