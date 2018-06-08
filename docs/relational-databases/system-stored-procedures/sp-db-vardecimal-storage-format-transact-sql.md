---
title: sp_db_vardecimal_storage_format (Transact-SQL) | Documents Microsoft
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
- sp_db_vardecimal_storage_format
- sp_db_vardecimal_storage_format_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_vardecimal_storage_format
- decimal data type, compressing
- compressing decimal data
- numeric data type, compressing
- database compression [SQL Server]
- table compression [SQL Server]
ms.assetid: 9920b2f7-b802-4003-913c-978c17ae4542
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 060f5e31593456168274507cb2abe789725c586d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdbvardecimalstorageformat-transact-sql"></a>sp_db_vardecimal_storage_format (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne l'état du format de stockage vardecimal actuel d'une base de données ou active une base de données pour le format de stockage vardecimal.  Depuis [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], les bases de données utilisateur sont toujours activées. L'activation des bases de données pour le format de stockage vardecimal est nécessaire uniquement dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
> [!IMPORTANT]  
>  La modification de l'état du format de stockage vardecimal d'une base de données peut affecter la sauvegarde et la récupération, la mise en miroir de bases de données, sp_attach_db, la copie des journaux de transaction et la réplication.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_db_vardecimal_storage_format [ [ @dbname = ] 'database_name']   
    [ , [ @vardecimal_storage_format = ] { 'ON' | 'OFF' } ]   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @dbname=] '*nom_base_de_données*'  
 Nom de la base de données dont le format de stockage doit être modifié. *database_name* est **sysname**, sans valeur par défaut. Si le nom de la base de données est omis, l'état du format de stockage vardecimal de toutes les bases de données dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est retourné.  
  
 [ @vardecimal_storage_format=] {'ON' |' DÉSACTIVER '}  
 Spécifie si le format de stockage vardecimal est activé. @vardecimal_storage_format peut prendre la valeur ON ou OFF (activé ou désactivé). Le paramètre est **varchar(3)**, sans valeur par défaut. Si un nom de base de données est indiqué mais que @vardecimal_storage_format est omis, le paramètre actuel de la base de données spécifiée est retourné. Cet argument n'a aucun effet sur [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si le format de stockage de la base de données ne peut pas être modifié, sp_db_vardecimal_storage_format retourne une erreur. Si la base de données est déjà dans l'état spécifié, la procédure stockée est sans effet.  
  
 Si le @vardecimal_storage_format argument n’est pas fourni, retourne les colonnes nom de la base de données et l’état Vardecimal.  
  
## <a name="remarks"></a>Notes  
 sp_db_vardecimal_storage_format retourne l'état vardecimal mais ne peut pas le modifier.  
  
 sp_db_vardecimal_storage_format échoue dans les circonstances suivantes :  
  
-   Il existe des utilisateurs actifs dans la base de données.  
  
-   La base de données est activée pour la mise en miroir.  
  
-   L'édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge le format de stockage vardecimal.  
  
 Pour qu'il soit possible de basculer l'état du format de stockage vardecimal à OFF, une base de données doit être configurée en mode de récupération simple. Lorsqu'une base de données est configurée en mode de récupération simple, la séquence de journaux de transactions consécutifs est rompue. Procédez à une sauvegarde de base de données complète une fois que vous avez défini l'état du format de stockage vardecimal à OFF.  
  
 Le passage de l'état à OFF échouera si des tables utilisent la compression de base de données vardecimal. Pour modifier le format de stockage d’une table, utilisez [sp_tableoption](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Pour déterminer quelles sont les tables d'une base de données qui utilisent le format de stockage vardecimal, utilisez la fonction `OBJECTPROPERTY` et recherchez la propriété `TableHasVarDecimalStorageFormat`, comme indiqué dans l'exemple suivant.  
  
```  
USE AdventureWorks2012 ;  
GO  
SELECT name, object_id, type_desc  
FROM sys.objects   
 WHERE OBJECTPROPERTY(object_id,   
   N'TableHasVarDecimalStorageFormat') = 1 ;  
GO  
```  
  
## <a name="examples"></a>Exemples  
 Le code ci-dessous autorise la compression dans la base de données `AdventureWorks2012`, confirme l'état, puis compresse les colonnes décimales et numériques de la table `Sales.SalesOrderDetail`.  
  
```  
USE master ;  
GO  
  
EXEC sp_db_vardecimal_storage_format 'AdventureWorks2012', 'ON' ;  
GO  
  
-- Check the vardecimal storage format state for  
-- all databases in the instance.  
EXEC sp_db_vardecimal_storage_format ;  
GO  
  
USE AdventureWorks2012 ;  
GO  
  
EXEC sp_tableoption 'Sales.SalesOrderDetail', 'vardecimal storage format', 1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
