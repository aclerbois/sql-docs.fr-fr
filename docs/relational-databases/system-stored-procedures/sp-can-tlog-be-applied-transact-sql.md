---
title: sp_can_tlog_be_applied (Transact-SQL) | Microsoft Docs
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-stored-procedures
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- sp_can_tlog_be_applied_TSQL
- sp_can_tlog_be_applied
dev_langs:
- TSQL
helpviewer_keywords:
- sp_can_tlog_be_applied
ms.assetid: 9c143b6c-27ac-4ab7-98d1-3b7b265f3963
caps.latest.revision: 
author: stevestein
ms.author: sstein
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 0a29fc1b86e56707d45439a87ec4a88594a1917d
ms.sourcegitcommit: c556eaf60a49af7025db35b7aa14beb76a8158c5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="spcantlogbeapplied-transact-sql"></a>sp_can_tlog_be_applied (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Vérifie si une sauvegarde du journal des transactions peut être appliquée à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **sp_can_tlog_be_applied** nécessite que la base de données soit en état de restauration.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_can_tlog_be_applied [ @backup_file_name = ] 'backup_file_name'   
        , [ @database_name = ] 'database_name'   
        , [ @result = ] result OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@backup_file_name=** ] **'***backup_file_name***'**  
 Nom d'un fichier de sauvegarde. *Nom_fichier_de_sauvegarde* est **nvarchar (128)**.  
  
 [ **@database_name=** ] **'***database_name***'**  
 Nom de la base de données. *database_name* est **sysname**.  
  
 [  **@result=** ] *résultat* **sortie**  
 Indique si le journal des transactions peut être appliqué à la base de données. *résultat* est **bits**.  
  
 1 = Le journal peut être appliqué.  
  
 0 = Le journal ne peut pas être appliqué.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_can_tlog_be_applied**.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous déclare une variable locale, `@MyBitVar`, pour stocker le résultat.  
  
```  
USE master;  
GO  
DECLARE @MyBitVar BIT;  
EXEC sp_can_tlog_be_applied  
     @backup_file_name =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\AdventureWorks2012.bak',  
     @database_name = N'AdventureWorks2012',  
     @result = @MyBitVar OUTPUT;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
