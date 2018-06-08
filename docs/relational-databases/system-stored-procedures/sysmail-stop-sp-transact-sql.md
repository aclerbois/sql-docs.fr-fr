---
title: sysmail_stop_sp (Transact-SQL) | Documents Microsoft
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
- sysmail_stop_sp_TSQL
- sysmail_stop_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_stop_sp
ms.assetid: 045ee36f-5bf0-4626-b5ee-e84db06ce16f
caps.latest.revision: 28
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 8f304337470d0117b6f44d03bf7e459c6b9f51d5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailstopsp-transact-sql"></a>sysmail_stop_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Arrête la messagerie de base de données en arrêtant les objets [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilisés par le programme externe.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_stop_sp  
```  
  
## <a name="arguments"></a>Arguments  
 Aucun  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Cette procédure stockée se trouve dans le **msdb** base de données.  
  
 Elle arrête la file d'attente de la messagerie de base de données contenant les demandes de messages sortants et désactive [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour le programme externe.  
  
 Dès que les files d'attente sont arrêtées, le programme externe de la messagerie de base de données ne traite plus les messages. Cette procédure stockée vous permet d'arrêter la messagerie de base de données pour résoudre des problèmes ou effectuer des tâches de maintenance.  
  
 Pour démarrer la messagerie de base de données, utilisez **sysmail_start_sp**. Notez que **sp_send_dbmail** accepte toujours les messages lorsque le [!INCLUDE[ssSB](../../includes/sssb-md.md)] objets sont arrêtés.  
  
> [!NOTE]  
>  Cette procédure stockée arrête simplement les files d'attente de la messagerie de base de données. Elle ne désactive pas la remise de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans la base de données. Cette procédure stockée ne désactive pas les procédures stockées étendues de la messagerie de base de données pour réduire la zone de surface. Pour désactiver les procédures stockées étendues, consultez la [option Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) de la **sp_configure** procédure stockée système.  
  
## <a name="permissions"></a>Autorisations  
 Autorisations d’exécution de cette procédure reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment arrêter la messagerie de base de données dans le **msdb** base de données. Il suppose que la messagerie de base de données a été activée.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sysmail_stop_sp ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [sysmail_start_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-start-sp-transact-sql.md)   
 [Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
