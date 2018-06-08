---
title: Sys.sp_add_trusted_assembly (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_add_trusted_assembly_TSQL
- sp_add_trusted_assembly
- sys.sp_add_trusted_assembly_TSQL
- sys.sp_add_trusted_assembly
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_add_trusted_assembly
ms.assetid: ''
caps.latest.revision: ''
author: tmullaney
ms.author: thmullan
manager: craigg
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 1a3791d82f0970ec6ed3e04ede69492abbcddb59
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspaddtrustedassembly-transact-sql"></a>Sys.sp_add_trusted_assembly (Transact-SQL)  
[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

Ajoute un assembly à la liste des assemblys de confiance pour le serveur.

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


## <a name="syntax"></a>Syntaxe
```  
sp_add_trusted_assembly 
    [ @hash = ] 'value'
    [ , [ @description = ] 'description' ]
```  

## <a name="remarks"></a>Notes  

Cette procédure ajoute un assembly à [sys.trusted_assemblies](../../relational-databases/system-catalog-views/sys-trusted-assemblies-transact-sql.md).

## <a name="arguments"></a>Arguments

[ @hash =] '*valeur*'  
La valeur de hachage SHA2_512 de l’assembly à ajouter à la liste des assemblys de confiance pour le serveur. Approuvé assemblys peuvent charger [stricte de sécurité CLR](../../database-engine/configure-windows/clr-strict-security.md) est activé, même si l’assembly n’est pas signé ou la base de données n’est pas marquée comme digne de confiance.

[ @description =] '*description*'  
Description définie par l’utilisateur facultative de l’assembly. Microsoft recommande d’utiliser le nom canonique qui encode le nom simple numéro de version, culture, clé publique et architecture de l’assembly de confiance. Cette valeur identifie l’assembly sur le côté du common language runtime (CLR) de manière unique et est identique à la valeur clr_name dans sys.assemblies. 

## <a name="permissions"></a>Autorisations

Nécessite l’appartenance dans le `sysadmin` rôle serveur fixe ou `CONTROL SERVER` autorisation.

## <a name="examples"></a>Exemples  

L’exemple suivant ajoute un assembly nommé `pointudt` à la liste des assemblys de confiance pour le serveur. Ces valeurs sont disponibles à partir de [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md).     

```  
EXEC sp_add_trusted_assembly 0x8893AD6D78D14EE43DF482E2EAD44123E3A0B684A8873C3F7BF3B5E8D8F09503F3E62370CE742BBC96FE3394477214B84C7C1B0F7A04DCC788FA99C2C09DFCCC, 
N'pointudt, version=0.0.0.0, culture=neutral, publickeytoken=null, processorarchitecture=msil';
```  

## <a name="see-also"></a>Voir aussi  
  [sys.sp_drop_trusted_assembly](sys-sp-drop-trusted-assembly-transact-sql.md)  
  [sys.trusted_assemblies](../../relational-databases/system-catalog-views/sys-trusted-assemblies-transact-sql.md)  
  [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)  
  [Sécurité stricte de CLR](../../database-engine/configure-windows/clr-strict-security.md)  
  [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)  
  [sys.dm_clr_loaded_assemblies](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)  

