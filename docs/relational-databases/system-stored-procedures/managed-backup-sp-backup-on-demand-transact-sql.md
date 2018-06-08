---
title: managed_backup.sp_backup_on_demand (Transact-SQL) | Documents Microsoft
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
- smart_admin.sp_backup_on_demand
- smart_admin.sp_backup_on_demand_TSQL
- sp_backup_on_demand_TSQL
- sp_backup_on_demand
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.sp_backup_on_demand
- sp_backup_on_demand
ms.assetid: 638f809f-27fa-4c44-a549-9cf37ecc920c
caps.latest.revision: 13
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d95bdd7fa337598c289aaa7a958c33513eec12d0
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="managedbackupspbackupondemand-transact-sql"></a>managed_backup.sp_backup_on_demand (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Demande la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour effectuer une sauvegarde de la base de données spécifiée.  
  
 Utilisez cette procédure stockée pour effectuer des sauvegardes ad hoc d'une base de données configurée avec la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Cela empêche la rupture de la chaîne de sauvegarde et les processus de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] sont pris en charge, stockant la sauvegarde dans le même conteneur de stockage Blob Windows Azure.  
  
 À la fin de la sauvegarde, le chemin de fichier de sauvegarde complet est retourné. Il comprend le nom et l'emplacement du nouveau fichier de sauvegarde résultant de l'opération de sauvegarde.  
  
 Une erreur est retournée si la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est sur le point d'exécuter une sauvegarde d'un type donné pour une base de données spécifiée. Dans ce cas, le message d'erreur retourné comprend le chemin de fichier de sauvegarde complet où la sauvegarde est actuellement téléchargée.  
   
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
EXEC managed_backup.sp_backup_on_demand   
[@database_name  =]  'database name',[@type = ] { 'Database' | 'Log' }  
  
```  
  
##  <a name="Arguments"></a> Arguments  
 @database_name  
 Nom de la base de données sur laquelle la sauvegarde doit être effectuée. Le @database_name est **SYSNAME**.  
  
 @type  
 Type de sauvegarde à effectuer : base de données ou journal. Le @type paramètre est **NVARCHAR(32)**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au **db_backupoperator** de la base de données de rôle, avec **ALTER ANY CREDENTIAL** autorisations, et **EXECUTE** autorisations sur **sp_delete_backuphistory**procédure stockée.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, une sauvegarde de base de données est demandée pour la base de données « TestDB ». La [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est activée pour cette base de données.  
  
```  
Use MSDB  
Go  
EXEC managed_backup.sp_backup_on_demand  
 @database_name = 'TestDB'  
,@type = 'Database'  
  
```  
  
 Pour chaque extrait de code, sélectionnez « tsql » dans le champ d'attribut de langage.  
  
  
