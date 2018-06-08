---
title: Sys.dm_filestream_file_io_handles (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_filestream_file_io_handles
- sys.dm_filestream_file_io_handles
- dm_filestream_file_io_handles_TSQL
- sys.dm_filestream_file_io_handles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_file_io_handle catalog view
ms.assetid: e59632f4-3292-419f-9217-ca375749f1a5
caps.latest.revision: 10
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 87dc96a297933b6981e1e2ada432d355287b4ce2
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmfilestreamfileiohandles-transact-sql"></a>sys.dm_filestream_file_io_handles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche les descripteurs de fichiers connus par le propriétaire de l'espace de noms (NSO, Namespace Owner). Handles de FileStream un client obtenu à l’aide de **OpenSqlFilestream** sont affichés dans cette vue.  
  
|Colonne|Type| Description|  
|------------|----------|-----------------|  
|**handle_context_address**|**varbinary(8)**|Affiche l'adresse de la structure NSO interne associée au handle du client. Autorise la valeur NULL.|  
|**creation_request_id**|**int**|Affiche un champ de la requête d'E/S REQ_PRE_CREATE utilisé pour créer ce handle. N'accepte pas la valeur NULL.|  
|**creation_irp_id**|**int**|Affiche un champ de la requête d'E/S REQ_PRE_CREATE utilisé pour créer ce handle. N'accepte pas la valeur NULL|  
|**handle_id**|**int**|Affiche l'ID unique de ce handle qui est attribué par le pilote. N'accepte pas la valeur NULL.|  
|**creation_client_thread_id**|**varbinary(8)**|Affiche un champ de la requête d'E/S REQ_PRE_CREATE utilisé pour créer ce handle. Autorise la valeur NULL.|  
|**creation_client_process_id**|**varbinary(8)**|Affiche un champ de la requête d'E/S REQ_PRE_CREATE utilisé pour créer ce handle. Autorise la valeur NULL.|  
|**filestream_transaction_id**|**varbinary(128)**|Affiche l'ID de la transaction associée au handle donné. C’est la valeur retournée par la **get_filestream_transaction_context** (fonction). Ce champ permet de joindre à la **sys.dm_filestream_file_io_requests** vue. Autorise la valeur NULL.|  
|**access_type**|**nvarchar(60)**|N'accepte pas la valeur NULL.|  
|**logical_path**|**nvarchar (256)**|Affiche le nom de chemin d'accès logique du fichier ouvert par ce handle. Il s’agit du même chemin d’accès qui est retourné par la **. Chemin d’accès** méthode **varbinary**(**max**) filestream. Autorise la valeur NULL.|  
|**chemin physique**|**nvarchar (256)**|Affiche le nom de chemin d'accès NTFS réel du fichier. Il s’agit du même chemin d’accès retourné par la **. PhysicalPathName** méthode de la **varbinary**(**max**) filestream. Il est activé par l'indicateur de suivi 5556. Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [FileStream et les vues de gestion dynamique FileTable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)  
  
  
