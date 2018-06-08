---
title: sysmail_unsentitems (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_unsentitems_TSQL
- sysmail_unsentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_unsentitems database mail view
ms.assetid: 993c12da-41e5-4e53-a188-0323feb70c67
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 01f464cb32067af6ca89253b0ad43700a914deb8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailunsentitems-transact-sql"></a>sysmail_unsentitems (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque message de la messagerie de base de données avec le **non envoyé** ou **une nouvelle tentative** état. Les messages dont l'état est 'unsent' ou 'retrying' se trouvent toujours dans la file d'attente des messages et peuvent être envoyés à tout moment. Les messages peuvent avoir le **non envoyé** état pour les raisons suivantes :  
  
-   il s'agit d'un nouveau message et, bien qu'il ait été placé dans la file d'attente des messages, la messagerie de base de données est en train d'envoyer d'autres messages et n'a pas encore atteint ce message ;  
  
-   le programme externe de messagerie de base de données n'est pas en cours d'exécution et aucun message n'est envoyé.  
  
 Les messages peuvent avoir le **une nouvelle tentative** état pour les raisons suivantes :  
  
-   La messagerie de base de données a essayé d'envoyer le message mais n'a pas réussi à contacter le serveur de messagerie SMTP. La messagerie de base de données va continuer à essayer d'envoyer le message en utilisant d'autres comptes de messagerie de base de données attribués au profil qui a envoyé le message. Si aucun compte ne peut envoyer le message, la messagerie de base de données attend pendant la durée de temps défini pour le **délai entre reprises de comptes** paramètre et que vous essayez d’envoyer le message. Utilisation de la messagerie de base de données le **tentatives de reprises de comptes** paramètre pour déterminer le nombre de tentatives d’essayer d’envoyer le message. Conservent les messages **une nouvelle tentative** tant que la messagerie de base de données tente d’envoyer le message d’état.  
  
 Utilisez cette vue lorsque vous voulez voir combien de messages se trouvent dans la file d'attente des messages et depuis combien de temps ils s'y trouvent. Normalement, le nombre de **non envoyé** messages sera faibles. Procédez à un test d'évaluation au cours d'opérations normales pour déterminer un nombre approprié de messages en file d'attente pour vos opérations.  
  
 Pour afficher tous les messages traités par la messagerie de base de données, utilisez [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Pour afficher uniquement les messages avec l’état d’échec, utilisez [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Pour afficher uniquement les messages qui ont été envoyés, utilisez [sysmail_sentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md).  
  
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
|**send_request_user**|**sysname**|Utilisateur qui a envoyé le message. Ce n’est pas le contexte de l’utilisateur de la procédure de messagerie de base de données, le **de** champ du message.|  
|**sent_account_id**|**int**|Identificateur du compte de messagerie de base de données utilisé pour envoyer le message. La valeur est toujours NULL pour cette vue.|  
|**sent_status**|**varchar(8)**|Sera **non envoyé** si la messagerie de base de données n’a pas essayé d’envoyer le message. Sera **une nouvelle tentative** si la messagerie de base de données n’a pas pu envoyer le message, mais elle essaie à nouveau.|  
|**sent_date**|**datetime**|Date et heure de la dernière tentative d'envoi du message par la messagerie de base de données. La valeur est NULL si la messagerie de base de données n'a pas essayé d'envoyer le message.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne.|  
  
## <a name="remarks"></a>Notes  
 En cas de dépannage de la messagerie de base de données, cette vue peut vous aider à identifier la nature du problème en vous montrant le nombre de messages se trouvant en file d'attente et leur temps d'attente. Si aucun message n'est envoyé, soit le programme externe de messagerie de base de données n'est pas en cours d'exécution, soit un problème réseau empêche la messagerie de base de données de contacter les serveurs SMTP. Si la plupart des messages non envoyés ont la même **profile_id**, il peut y avoir un problème avec le serveur SMTP. Pensez à ajouter des comptes supplémentaires au profil. Si les messages sont envoyés mais qu'ils restent trop longtemps dans la file d'attente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite peut-être des ressources plus importantes pour traiter le volume de messages dont vous avez besoin.  
  
## <a name="permissions"></a>Autorisations  
 Accordées à **sysadmin** rôle serveur fixe et **DatabaseMailUserRole** rôle de base de données. Lors de l’exécution par un membre de la **sysadmin** rôle serveur fixe, cette vue affiche toutes les **non envoyé** ou **une nouvelle tentative** messages. Tous les autres utilisateurs voient uniquement les **non envoyé** ou **une nouvelle tentative** les messages qu’ils ont envoyés.  
  
  
