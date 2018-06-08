---
title: sp_delete_backup (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/03/2015
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 808e50ae-ff6e-4520-9ce2-530591d3d59b
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2955570ee99eaa05d9a689ccbe62973af3208d80
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdeletebackup-transact-sql"></a>sp_delete_backup (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Supprime tous les instantanés et le fichier de sauvegarde qui composent un jeu à partir de la base de données de sauvegarde de capture instantanée. Cette procédure stockée système est la seule méthode recommandée pour la gestion des jeux de sauvegarde de capture instantanée. Pour plus d’informations, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_delete_backup   
    [ @backup_url = ] backup_metadata_file_url  
    ,[ [ @db_name = ] database_name | NULL ]  
```  
  
## <a name="arguments"></a>Arguments  
 *[ @backup_url =] backup_meta_file_url*  
 L’URL de la sauvegarde à supprimer, ce qui supprime tous les instantanés comprenant la sauvegarde spécifiée est définie, y compris le fichier de sauvegarde.  
  
 *[ @db_name =] nom_base_de_données*  
 Le nom de la base de données contenant l’instantané à supprimer. Lorsqu’un nom de base de données est fourni, le système vérifie que l’URL de sauvegarde fourni est une URL de sauvegarde pour la base de données spécifiée et utilise [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) pour supprimer chaque instantané. Si aucun nom de base de données est fourni, cette vérification de la base de données n’est pas effectuée.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation ALTER ANY DATABASE ou l’autorisation ALTER sur la base de données spécifié.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)   
 [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md)  
  
  
