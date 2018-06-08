---
title: Sys.fn_translate_permissions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.fn_translate_permissions
- sys.fn_translate_permissions_TSQL
- fn_translate_permissions
- fn_translate_permissions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permissions bitmask [SQL Server]
- sys.fn_translate_permissions function
- fn_translate_permissions function
ms.assetid: ac97121f-2bd0-4f71-8e45-42c8584edbc5
caps.latest.revision: 18
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 56fe5c1054bdf41af5708981e6377ae7175e4210
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfntranslatepermissions-transact-sql"></a>sys.fn_translate_permissions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Convertit le masque de bits des autorisations retourné par la trace SQL en une table de noms d'autorisations.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_translate_permissions ( level , perms )  
```  
  
## <a name="arguments"></a>Arguments  
 *Niveau*  
 Type d'élément sécurisable auquel l'autorisation est appliquée. *niveau* est **nvarchar (60)**.  
  
 *Perms*  
 Masque de bits retourné dans la colonne d'autorisations. *Perms* est **varbinary (16)**.  
  
## <a name="returns"></a>Valeur renvoyée  
 **table**  
  
## <a name="remarks"></a>Notes  
 La valeur retournée dans le **autorisations** colonne d’une Trace SQL est une représentation entière du masque de bits utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour calculer les autorisations effectives. Chacun des 25 types d'éléments sécurisables possède son propre jeu d'autorisations avec des valeurs numériques correspondantes. **Sys.fn_translate_permissions** ce masque de bits se traduit par une table de noms d’autorisations.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="example"></a>Exemple  
 La requête suivante utilise `sys.fn_builtin_permissions` pour afficher les autorisations qui s’appliquent aux certificats, puis utilise `sys.fn_translate_permissions` pour retourner les résultats du masque de bits des autorisations.  
  
```  
SELECT * FROM sys.fn_builtin_permissions('CERTIFICATE');  
SELECT '0001' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0001);  
SELECT '0010' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0010);  
SELECT '0011' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0011);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
