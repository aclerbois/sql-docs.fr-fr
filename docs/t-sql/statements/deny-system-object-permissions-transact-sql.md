---
title: DENY - Refuser des autorisations sur un objet système (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, system objects
- encryption [SQL Server], system objects
- system objects [SQL Server]
- cryptography [SQL Server], system objects
ms.assetid: 4e43f954-0982-470b-a239-08a13c61563a
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d91e264a7fc77908ff5795be3dd535f47c449995
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deny-system-object-permissions-transact-sql"></a>DENY – refus d'autorisations d'objet système (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Permet de refuser des autorisations sur des objets système tels que des procédures stockées, des procédures stockées étendues, des fonctions et des vues.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DENY { SELECT | EXECUTE } ON [ sys.]system_object TO principal   
```  
  
## <a name="arguments"></a>Arguments  
 [ **sys.**]  
 Le qualificateur **sys** est obligatoire uniquement quand vous faites référence à des vues de catalogue ou à des vues de gestion dynamique.  
  
 *system_object*  
 Spécifie l'objet sur lequel l'autorisation doit être refusée.  
  
 *principal*  
 Spécifie le principal pour lequel l'autorisation est révoquée.  
  
## <a name="remarks"></a>Notes   
 Cette instruction permet de refuser des autorisations sur des procédures stockées, procédures stockées étendues, fonctions table, fonctions scalaires, vues, affichages catalogue, vues de compatibilité, vues INFORMATION_SCHEMA, vues de gestion dynamique et tables système installées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Chacun de ces objets système existe sous la forme d’un enregistrement unique dans la base de données des ressources (**mssqlsystemresource**). La base de données des ressources est en lecture seule. Un lien à l’objet est exposé sous la forme d’un enregistrement dans le schéma **sys** de chaque base de données.  
  
 La résolution de noms par défaut permet de résoudre les noms de procédures non qualifiés dans la base de données des ressources. Par conséquent, le qualificateur **sys** est obligatoire uniquement quand vous spécifiez des vues de catalogue et des vues de gestion dynamique.  
  
> [!CAUTION]  
>  Le refus d'autorisations sur des objets système entraîne l'échec des applications qui en dépendent. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utilise les affichages catalogue et peut ne pas fonctionner comme prévu si vous modifiez les autorisations par défaut sur les affichages catalogue.  
  
 Le refus d'autorisations sur des déclencheurs et sur des colonnes d'objets système n'est pas pris en charge.  
  
 Les autorisations sur les objets système sont conservées lors d'une mise à niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les objets système sont consultables dans l’affichage catalogue [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) . Les autorisations sur les objets système sont consultables dans l’affichage catalogue [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) dans la base de données **master** .  
  
 La requête ci-dessous retourne des informations sur les autorisations des objets système :  
  
```  
SELECT * FROM master.sys.database_permissions AS dp   
    JOIN sys.system_objects AS so  
    ON dp.major_id = so.object_id  
    WHERE dp.class = 1 AND so.parent_object_id = 0 ;  
GO  
```  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, l'autorisation `EXECUTE` sur `xp_cmdshell` est refusée à `public`.  
  
```  
DENY EXECUTE ON sys.xp_cmdshell TO public;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [GRANT - Octroyer des autorisations sur un objet système &#40;Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)   
 [REVOKE - Révoquer des autorisations sur un objet système &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md)  
  
  
