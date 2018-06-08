---
title: sysmail_sentitems (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_sentitems_TSQL
- sysmail_sentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_sentitems database mail view
ms.assetid: 16eb2a44-cebb-4cec-93ac-e2498c39989f
caps.latest.revision: 18
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: de4547bec20880375fc29fc746588983b2ec9d2e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailsentitems-transact-sql"></a>sysmail_sentitems (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque message envoyé par la messagerie de base de données. Utilisez **sysmail_sentitems** lorsque vous souhaitez afficher les messages qui ont été correctement envoyés.  
  
 Pour afficher tous les messages traités par la messagerie de base de données, utilisez [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Pour afficher uniquement les messages avec l’état d’échec, utilisez [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Pour voir uniquement en attente ou une nouvelle tentative de messages, utilisez [sysmail_unsentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Pour afficher les pièces jointes, utilisez [sysmail_mailattachments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md).  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificateur de l'élément de messagerie dans la file d'attente des messages.|  
|**profile_id**|**int**|Identificateur du profil utilisé pour envoyer le message.|  
|**destinataires**|**varchar(max)**|Adresses de messagerie des destinataires du message.|  
|**copy_recipients**|**varchar(max)**|Adresses de messagerie des personnes qui reçoivent une copie du message.|  
|**blind_copy_recipients**|**varchar(max)**|Adresses de messagerie des personnes qui reçoivent une copie du message mais dont le nom n'apparaît pas dans l'en-tête du message.|  
|**subject**|**nvarchar(510)**|Ligne d'objet du message.|  
|**Corps**|**varchar(max)**|Le corps du message.|  
|**body_format**|**varchar(20)**|Le format du corps du message. Les valeurs possibles sont **texte** et **HTML**.|  
|**importance**|**varchar(6)**|Le **importance** paramètre du message.|  
|**Respect de la**|**varchar(12)**|Le **sensibilité** paramètre du message.|  
|**file_attachments**|**varchar(max)**|Liste des noms de fichiers joints au message électronique (délimitée par des points-virgules).|  
|**Attachment_encoding**|**varchar(20)**|Type de pièce jointe.|  
|**Requête**|**varchar(max)**|Requête exécutée par le programme de messagerie.|  
|**execute_query_database**|**sysname**|Contexte de base de données dans lequel le programme de messagerie a exécuté la requête.|  
|**attach_query_result_as_file**|**bit**|Lorsque la valeur est 0, les résultats de la requête ont été inclus dans le corps du message électronique, après le contenu du corps. Lorsque la valeur est 1, les résultats ont été renvoyés sous forme de pièce jointe.|  
|**query_result_header**|**bit**|Lorsque la valeur est 1, cela signifie que les résultats de la requête contenaient des en-têtes de colonne. Lorsque la valeur est 0, cela signifie que les résultats de la requête ne contenaient pas d'en-têtes de colonne.|  
|**query_result_width**|**int**|Le **query_result_width** paramètre du message.|  
|**query_result_separator**|**char(1)**|Caractère utilisé pour séparer les colonnes dans la sortie de la requête.|  
|**exclude_query_output**|**bit**|Le **exclude_query_output** paramètre du message. Pour plus d’informations, consultez [sp_send_dbmail &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|Le **append_query_error** paramètre du message. La valeur 0 indique que la messagerie de base de données ne doit pas envoyer le message électronique s'il existe une erreur dans la requête.|  
|**send_request_date**|**datetime**|Date et heure d'arrivée du message dans la file d'attente des messages.|  
|**send_request_user**|**sysname**|L’utilisateur qui a envoyé le message. Il s'agit du contexte utilisateur de la procédure de la messagerie de base de données, et non du champ De : du message.|  
|**sent_account_id**|**int**|Identificateur du compte de messagerie de base de données utilisé pour envoyer le message.|  
|**sent_status**|**varchar(8)**|État du message. Toujours **envoyé** pour cette vue.|  
|**sent_date**|**datetime**|Date et heure d'envoi du message.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne.|  
  
## <a name="remarks"></a>Notes  
 En cas de dépannage de la messagerie de base de données, cette vue peut vous aider à identifier la nature du problème en vous montrant les attributs des messages qui ont été correctement envoyés. La messagerie de base de données marque les messages comme envoyés ('sent') lorsqu'ils sont soumis avec succès à un serveur de messagerie SMTP. En principe, le message est reçu en l'espace de quelques minutes, mais il peut être retardé en raison de problèmes avec le serveur SMTP. La messagerie de base de données marque le message comme envoyé lorsque celui-ci est accepté par le serveur de messagerie SMTP. Les erreurs qui se produisent sur le serveur de messagerie SMTP, par exemple lorsque le message ne peut pas être remis à l'adresse de messagerie du destinataire, ne sont pas renvoyées à la messagerie de base de données. Ces messages sont donc considérés comme envoyés, bien qu'ils n'aient pas été remis. Vous devez résoudre ce type d'erreur sur le serveur SMTP. Le serveur de messagerie SMTP peut également envoyer un avis de non remise à l'adresse de réponse d'un compte de messagerie de base de données.  
  
## <a name="permissions"></a>Autorisations  
 Accordées à **sysadmin** rôle serveur fixe et **databasemailuserrole** rôle de base de données. Lors de l’exécution par un membre de la **sysadmin** rôle serveur fixe, cette vue affiche tous les messages envoyés. Les autres utilisateurs voient uniquement les messages qu'ils ont envoyés.  
  
## <a name="see-also"></a>Voir aussi  
 [Objets de messagerie de base de données](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
  
