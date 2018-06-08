---
title: managed_backup.fn_get_parameter (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- smart_admin.fn_get_parameter_TSQL
- smart_admin.fn_get_parameter
- fn_get_parameter_TSQL
- fn_get_parameter
dev_langs:
- TSQL
helpviewer_keywords:
- fn_get_parameter
- smart_admin.fn_get_parameter
ms.assetid: ed94e54d-4516-4806-a8ce-f013d3a04122
caps.latest.revision: 17
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2420c689dff7e344c06a6667c15e08d3d4769872
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="managedbackupfngetparameter-transact-sql"></a>managed_backup.fn_get_parameter (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Retourne une table de 0, 1 ou plusieurs lignes de paires paramètre/valeur.  
  
 Utilisez cette procédure stockée pour consulter tous les paramètres de configuration, ou seulement certains, pour Smart Admin.  
  
 Si le paramètre n'a jamais été configuré, la fonction retourne 0 ligne.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
managed_backup.fn_get_parameter ('parameter_name' | '' | NULL )  
```  
  
##  <a name="Arguments"></a> Arguments  
 parameter_name  
 Nom du paramètre. valeur de parameter_name est **nvarchar (128)**. Si la valeur NULL ou une chaîne vide est fournie comme un argument à la fonction, les paires nom-valeur de tous les paramètres Smart Admin configurés sont retournées.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|parameter_name|NVARCHAR(128)|Nom du paramètre. Vous trouverez ci-dessous la liste actuelle des paramètres retournés :<br/><br/>**FileRetentionDebugXevent**<br/><br/>**SSMBackup2WADebugXevent**<br/><br/>**SSMBackup2WANotificationEmailIds**<br/><br/>**SSMBackup2WAEnableUserDefinedPolicy**<br/><br/>**SSMBackup2WAEverConfigured**<br/><br/>**StorageOperationDebugXevent**|  
|parameter_value|NVARCHAR(128)|Valeur actuelle du paramètre.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations SELECT sur la fonction.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne tous les paramètres qui ont été configurés au moins une fois, et leurs valeurs actuelles.  
  
```  
USE MSDB  
GO  
SELECT *   
FROM managed_backup.fn_get_parameter (NULL)  
  
```  
  
 L'exemple suivant retourne l'ID de messagerie spécifié pour recevoir les notifications d'erreur. Si aucune ligne n'est retournée, cette option de notification par messagerie n'a pas été activée.  
  
```  
USE MSDB  
GO  
SELECT *  
FROM managed_backup.fn_get_parameter ('SSMBackup2WANotficationEmailIds')  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde managée SQL Server sur Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
