---
title: sysmail_delete_mailitems_sp (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_delete_mailitems_sp_TSQL
- sysmail_delete_mailitems_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_mailitems_sp
ms.assetid: f87c9f4a-bda1-4bce-84b2-a055a3229ecd
caps.latest.revision: 24
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ca730dc633f8aad10aa79fd34bb7e94870a48076
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmaildeletemailitemssp-transact-sql"></a>sysmail_delete_mailitems_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime définitivement des messages électroniques des tables internes de la messagerie de base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_delete_mailitems_sp  [ [ @sent_before = ] 'sent_before' ]  
    [ , [ @sent_status = ] 'sent_status' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@sent_before=** ] **'***sent_before***'**  
 Supprime les messages électroniques antérieurs à la date et l’heure spécifiées par le *sent_before* argument. *sent_before* est **datetime** avec NULL comme valeur par défaut. La valeur NULL correspond à toutes les dates.  
  
 [  **@sent_status=** ] **'***sent_status***'**  
 Supprime les messages électroniques du type spécifié par *sent_status*. *sent_status* est **varchar(8)** sans valeur par défaut. Les entrées valides sont **envoyé**, **non envoyé**, **une nouvelle tentative**, et **échec**. La valeur NULL correspond à tous les états.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Messages de la messagerie de base de données et leurs pièces jointes sont stockées dans le **msdb** base de données. Les messages doivent être supprimés périodiquement pour éviter **msdb** à partir de la croissance la plus élevée que prévu et se conformer à la politique de rétention de votre document organisations. Utilisez le **sysmail_delete_mailitems_sp** procédure stockée pour supprimer définitivement des messages électroniques à partir des tables de la messagerie de base de données. Un argument facultatif vous permet de supprimer uniquement les messages les plus anciens en fournissant une date et une heure. Les messages antérieurs à cet argument sont alors supprimés. Un autre argument facultatif vous permet de supprimer uniquement les messages électroniques d’un certain type, spécifié en tant que le **sent_status** argument. Vous devez fournir un argument pour **@sent_before** ou **@sent_status**. Pour supprimer tous les messages, utilisez  **@sent_before = getdate()**.  
  
 La suppression d'un message entraîne également la suppression des pièces jointes qui sont associées à ce message. Suppression du courrier électronique ne supprime pas les entrées correspondantes dans **sysmail_event_log**. Utilisez [sysmail_delete_log_sp](../../relational-databases/system-stored-procedures/sysmail-delete-log-sp-transact-sql.md) pour supprimer des éléments à partir du journal.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, cette procédure stockée est accordée pour l’exécution aux membres hors tension le **sysadmin** rôle serveur fixe et **DatabaseMailUserRole**. Membres de la **sysadmin** du rôle serveur fixe peuvent exécuter cette procédure pour supprimer les messages envoyés par tous les utilisateurs. Membres de **DatabaseMailUserRole** peuvent supprimer uniquement les messages envoyés par l’utilisateur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-deleting-all-e-mails"></a>A. Suppression de tous les messages électroniques  
 L'exemple suivant supprime tous les messages électroniques du système de la messagerie de base de données.  
  
```  
DECLARE @GETDATE datetime  
SET @GETDATE = GETDATE();  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp @sent_before = @GETDATE;  
GO  
```  
  
### <a name="b-deleting-the-oldest-e-mails"></a>B. Suppression des messages électroniques les plus anciens  
 L'exemple suivant supprime les messages électroniques du journal de la messagerie de base de données qui sont antérieurs au `October 9, 2005`.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp   
    @sent_before = 'October 9, 2005' ;  
GO  
```  
  
### <a name="c-deleting-all-e-mails-of-a-certain-type"></a>C. Suppression d'un certain type de message électronique  
 L'exemple suivant supprime tous les messages électroniques qui ont échoué du système de la messagerie de base de données.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp   
    @sent_status = 'failed' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)   
 [sysmail_mailattachments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md)   
 [Créer un travail d'Agent SQL Server pour archiver les messages et les journaux d'événements de la messagerie de base de données](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)  
  
  
