---
title: managed_backup.sp_set_parameter (Transact-SQL) | Documents Microsoft
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
- sp_set_parameter_TSQL
- sp_set_parameter
- smart_admin.sp_set_parameter
- smart_admin.sp_set_parameter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_parameter
- smart_admin.sp_set_parameter
ms.assetid: bd8ae5fd-1337-4b7f-b0a4-153cbca9fa5f
caps.latest.revision: 15
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2a9f1d5eeec1fc5b24fbc1974d27e9f4b5efd00d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="managedbackupspsetparameter-transact-sql"></a>managed_backup.sp_set_parameter (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Définit la valeur du paramètre système Smart Admin spécifié.  
  
 Les paramètres disponibles sont associés à la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Ces paramètres sont utilisés pour définir les notifications par courrier électronique, activer les événements étendus et activer la stratégie définie par l'utilisateur en fonction des stratégies de gestion. Vous devez spécifier les paires nom/valeur des paramètres.  

  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
EXEC managed_backup.sp_set_parameter   
    [@parameter_name = ] {'SSMBackup2WANotificationEmailIds' | 'SSMBackup2WAEnableUserDefinedPolicy' | 'SSMBackup2WADebugXevent' | 'FileRetentionDebugXevent' | 'StorageOperationDebugXevent'}  
    ,[@parameter_value = ] 'parameter_value'  
```  
  
##  <a name="Arguments"></a> Arguments  
 @parameter_name  
 Nom du paramètre à créer dont vous souhaitez définir la valeur. @parameter_name est nvarchar (128). Les noms de paramètres disponibles sont **SSMBackup2WANotificationEmailIds**, **SSMBackup2WADebugXevent**, **SSMBackup2WAEnableUserDefinedPolicy**, **FileRetentionDebugXevent**, et **StorageOperationDebugXevent**.  
  
 @parameter_value  
 Valeur du paramètre que vous souhaitez définir. @parameter la valeur est nvarchar (128).  Voici les paires nom/valeur autorisées pour les paramètres :  
  
-   @parameter_name = 'SSMBackup2WANotificationEmailIds' : @parameter_value = 'email'  
  
-   @parameter_name = 'SSMBackup2WAEnableUserDefinedPolicy' : @parameter_value = {'true' | 'false'}  
  
-   @parameter_name = 'SSMBackup2WADebugXevent' : @parameter_value = {'true' | 'false'}  
  
-   @parameter_name = 'FileRetentionDebugXevent' : @parameter_value = {'true' | 'false'}  
  
-   @parameter_name = 'StorageOperationDebugXevent' = {'true' | 'false'}  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="best-practices"></a>Bonnes pratiques  
 Section facultative qui décrit les meilleures pratiques que l'utilisateur doit connaître pour l'exécution de l'instruction ou de la routine.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert **EXECUTE** autorisations sur **managed_backup.sp_set_parameter** procédure stockée.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants activent les événements étendus opérationnels et de débogage.  
  
```  
-- to enable operational events  
Use msdb;  
Go  
         EXEC managed_backup.sp_set_parameter 'FileRetentionOperationalXevent', 'True'  
--  to enable debug events  
Use msdb;  
Go  
         EXEC managed_backup.sp_set_parameter 'FileRetentionDebugXevent', 'True'  
  
```  
  
 L'exemple suivant active les notifications par courrier électronique des erreurs et avertissements, et définit l'ID de courrier électronique auquel envoyer les notifications :  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_set_parameter @parameter_name = 'SSMBackup2WANotificationEmailIds', @parameter_value = '<email address>'  
  
```  
  
  
