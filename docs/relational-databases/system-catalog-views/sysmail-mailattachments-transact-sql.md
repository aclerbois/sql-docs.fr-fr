---
title: sysmail_mailattachments (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_mailattachments_TSQL
- sysmail_mailattachments
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_mailattachments database mail view
ms.assetid: aee87059-a4c1-459a-a95c-641b4e3f0e73
caps.latest.revision: 14
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 36c5433f51191004a994b1afb3486db89a317acb
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "33220690"
---
# <a name="sysmailmailattachments-transact-sql"></a>sysmail_mailattachments (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque pièce jointe soumise à la messagerie de base de données. Utilisez cette vue lorsque vous voulez des informations sur les pièces jointes de la messagerie de base de données. Pour passer en revue tous les messages électroniques traités à l’aide de la messagerie de base de données [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md).  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**attachment_id**|**Int**|Identificateur de la pièce jointe.|  
|**mailitem_id**|**Int**|Identificateur de l'élément de messagerie qui contenait la pièce jointe.|  
|**filename**|**nvarchar(520)**|Nom de fichier de la pièce jointe. Lorsque **attach_query_result** est 1 et **query_attachment_filename** est NULL, la messagerie de base de données crée un nom de fichier arbitraire.|  
|**filesize**|**Int**|Taille de la pièce jointe en octets.|  
|**attachment**|**varbinary(max)**|Contenu de la pièce jointe.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne.|  
  
## <a name="remarks"></a>Notes  
 En cas de résolution des problèmes de la messagerie de base de données, utilisez cette vue pour voir les propriétés des pièces jointes.  
  
 Les pièces jointes stockées dans les tables système peuvent entraîner la **msdb** base de données augmente. Utilisez **sysmail_delete_mailitems_sp** pour supprimer des éléments de messagerie et les pièces jointes associées. Pour plus d’informations, consultez [créer un travail de l’Agent SQL Server pour archiver les Messages de messagerie de base de données et les journaux des événements](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Autorisations  
 Accordées à la **sysadmin** rôle serveur fixe et le **DatabaseMailUserRole** rôle de base de données. Lors de l’exécution par un membre de la **sysadmin** rôle serveur fixe, cette vue affiche toutes les pièces jointes. Les autres utilisateurs voient uniquement les pièces jointes des messages qu'ils ont essayé d'envoyer.  
  
## <a name="see-also"></a>Voir aussi  
 [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [sysmail_sentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)   
 [sysmail_unsentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)   
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)  
  
  
