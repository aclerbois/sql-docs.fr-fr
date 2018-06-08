---
title: sp_dbcmptlevel (Transact-SQL) | Documents Microsoft
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
- sp_dbcmptlevel
- sp_dbcmptlevel_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbcmptlevel
ms.assetid: 508c686d-2bd4-41ba-8602-48ebca266659
caps.latest.revision: 110
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 416842d369700f0ac7e0ecd18f84fc0b546a49f7
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdbcmptlevel-transact-sql"></a>sp_dbcmptlevel (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Définit certains comportements de base de données pour qu'ils soient compatibles avec la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifiée.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez [modifier le niveau de compatibilité de base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dbcmptlevel [ [ @dbname = ] name ]   
    [ , [ @new_cmptlevel = ] version ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@dbname=** ] *nom*  
 Nom de la base de données dont le niveau de compatibilité doit être modifié. Les noms de base de données doivent être conformes aux règles relatives aux identificateurs. *nom* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@new_cmptlevel=** ] *version*  
 Version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec laquelle la base de données doit être compatible. *version* est **tinyint**, avec NULL comme valeur par défaut. La valeur doit être l'une des suivantes :  
  
 **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
 **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
 **110** = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
 **120** = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 **130** = [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si aucun paramètre n’est spécifié ou si le *nom* paramètre n’est pas spécifié, **sp_dbcmptlevel** renvoie une erreur.  
  
 Si *nom* est spécifié sans *version*, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] renvoie un message affichant le niveau de compatibilité actuel de la base de données spécifié.  
  
## <a name="remarks"></a>Notes  
 Pour obtenir une description des niveaux de compatibilité, voir [ALTER DATABASE Compatibility Level &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
## <a name="permissions"></a>Autorisations  
 Seuls le propriétaire de la base de données, les membres de la **sysadmin** rôle serveur fixe et le **db_owner** rôle de base de données fixe (si vous modifiez la base de données en cours) peut exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Mots clés réservés &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
