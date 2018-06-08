---
title: sysmail_event_log (Transact-SQL) | Documents Microsoft
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
- sysmail_event_log
- sysmail_event_log_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_event_log database mail view
ms.assetid: 440bc409-1188-4175-afc4-c68e31e44fed
caps.latest.revision: 16
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: b6c43f41415f97d87cddaf2c7b57e1e8250aa65f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmaileventlog-transact-sql"></a>sysmail_event_log (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque message Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourné par le système de messagerie de base de données (dans ce contexte, le terme « message » désigne un message de type message d'erreur, pas un message électronique). Configurer le **au niveau de journalisation** paramètre à l’aide de la **configurer les paramètres du système** boîte de dialogue de l’Assistant Configuration de la messagerie de base de données, ou le [sysmail_configure_sp](../../relational-databases/system-stored-procedures/sysmail-configure-sp-transact-sql.md) une procédure stockée, à déterminer quels messages sont retournés.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**Log_id**|**int**|Identificateur d'éléments du journal.|  
|**event_type**|**varchar(11)**|Type d'avis inséré dans le journal. Les valeurs possibles sont les suivantes : erreurs, avertissements, messages d'information, messages de succès et messages internes supplémentaires.|  
|**log_date**|**datetime**|Date et l'heure de création de l'entrée du journal.|  
|**description**|**nvarchar(max)**|Texte du message en cours d'enregistrement.|  
|**process_id**|**int**|L'ID de processus du programme externe de messagerie de base de données. Cette valeur change en principe à chaque démarrage du programme externe de messagerie de base de données.|  
|**mailitem_id**|**int**|Identificateur de l'élément de messagerie dans la file d'attente des messages. La valeur est NULL si le message n'est pas associé à un élément de courrier électronique spécifique.|  
|**account_id**|**int**|Le **account_id** du compte associé à l’événement. La valeur est NULL si le message n'est pas associé à un compte spécifique.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne. Pour les messages électroniques, il s'agit de l'utilisateur qui a envoyé le message. Pour les messages générés par le programme externe de messagerie de base de données, il s'agit du contexte utilisateur du programme.|  
  
## <a name="remarks"></a>Notes  
 Lors du dépannage de la messagerie de base de données, rechercher le **sysmail_event_log** affichage des événements liés aux échecs de messagerie. Certains messages, comme ceux signalant l'échec du programme externe de messagerie de base de données, ne sont pas associés à des messages électroniques spécifiques. Pour rechercher des erreurs liées à des messages électroniques spécifiques, rechercher le **mailitem_id** du courrier électronique ayant échoué dans le **sysmail_faileditems** afficher, puis recherchez le **sysmail_event_log** pour les messages associés à cette **mailitem_id**. Lorsqu’une erreur est retournée à partir de **sp_send_dbmail**, l’adresse de messagerie n’est pas soumis au système de messagerie de base de données et l’erreur n’est pas affiché dans cette vue.  
  
 Lorsque les tentatives de remise d'un compte spécifique échouent, la messagerie de base de données conserve les messages d'erreur pendant les tentatives de reprises de comptes jusqu'à ce que la remise de l'élément de messagerie aboutisse ou échoue. En cas de réussite, toutes les erreurs accumulées sont consignées séparément le **account_id**. Il se peut alors que des avertissements s'affichent, bien que le message électronique ait été envoyé. En cas d’échec de la remise, tous les avertissements précédents sont consignés en tant que message d’une erreur sans un **account_id**, car tous les comptes ont échoué.  
  
## <a name="permissions"></a>Autorisations  
 Vous devez être un membre de la **sysadmin** rôle serveur fixe ou **DatabaseMailUserRole** rôle de base de données pour accéder à cette vue. Membres de **DatabaseMailUserRole** qui ne sont pas membres de la **sysadmin** rôle peut voir uniquement les événements pour les messages électroniques qu’ils ont envoyés.  
  
## <a name="see-also"></a>Voir aussi  
 [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [Programme externe de la messagerie de base de données](../../relational-databases/database-mail/database-mail-external-program.md)  
  
  
