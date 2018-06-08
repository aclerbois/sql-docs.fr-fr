---
title: sp_syscollector_set_cache_directory (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_syscollector_set_cache_directory_TSQL
- sp_syscollector_set_cache_directory
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_set_cache_directory stored procedure
ms.assetid: df56d5a5-8961-494f-a745-d752ca63805a
caps.latest.revision: 18
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2fec21ffdac0ee58f6927935942fedadda563f0f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsyscollectorsetcachedirectory-transact-sql"></a>sp_syscollector_set_cache_directory (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Spécifie le répertoire où les données collectées sont stockées avant d'être téléchargées dans l'entrepôt de données de gestion.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_set_cache_directory [ @cache_directory = ] 'cache_directory'  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@cache_directory =** ] **'***cache_directory***'**  
 Répertoire du système de fichiers où les données collectées sont stockées temporairement. *cache_directory* est **nvarchar (255)**, avec NULL comme valeur par défaut. Si aucune valeur n'est spécifiée, le répertoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] temporaire par défaut est utilisé.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez désactiver le collecteur de données avant de modifier la configuration du répertoire de cache. Cette procédure stockée échoue si le collecteur de données est activé. Pour plus d’informations, consultez [activer ou désactiver la collecte de données](../../relational-databases/data-collection/enable-or-disable-data-collection.md), et [gérer la collecte de données](../../relational-databases/data-collection/manage-data-collection.md).  
  
 Le répertoire spécifié n'a pas besoin d'exister au moment où le sp_syscollector_set_cache_directory est exécuté ; toutefois, les données ne peuvent pas être correctement mises en cache et téléchargées tant que le répertoire n'a pas été créé. Nous vous recommandons de créer le répertoire avant d'exécuter cette procédure stockée.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant désactive le collecteur de données, définit le répertoire de cache pour le collecteur de données `D:\tempdata`, puis il active le collecteur de données.  
  
```sql  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_disable_collector;  
GO  
EXEC dbo.sp_syscollector_set_cache_directory N'D:\tempdata';  
GO  
EXECUTE dbo.sp_syscollector_enable_collector;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [sp_syscollector_set_cache_window &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-window-transact-sql.md)  
  
  
