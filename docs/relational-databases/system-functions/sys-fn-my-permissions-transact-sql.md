---
title: Sys.fn_my_permissions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.fn_my_permissions_TSQL
- fn_my_permissions_TSQL
- sys.fn_my_permissions
- fn_my_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- fn_my_permissions function
- sys.fn_my_permissions function
ms.assetid: 30f97f00-03d8-443a-9de9-9ec420b7699b
caps.latest.revision: 21
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: b837943f16a7c8882b4e35aef3f769a3d731cd38
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfnmypermissions-transact-sql"></a>sys.fn_my_permissions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie la liste des autorisations accordées effectivement au principal sur un élément sécurisable. Une fonction associée est [HAS_PERMS_BY_NAME](../../t-sql/functions/has-perms-by-name-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_my_permissions ( securable , 'securable_class' )  
```  
  
## <a name="arguments"></a>Arguments  
 *securable*  
 Indique le nom de l'élément sécurisable. Si l'élément sécurisable est le serveur ou une base de données, cette valeur doit être définie sur NULL. *securable* est une expression scalaire de type **sysname**. *élément sécurisable* peut être un nom en plusieurs parties.  
  
 '*securable_class*'  
 Nom de la classe de l'élément sécurisable pour lequel les autorisations sont affichées. *securable_class* est un **sysname**. *securable_class* doit être une des opérations suivantes : rôle d’APPLICATION, ASSEMBLY, une clé asymétrique, certificat, contrat, de base de données, point de terminaison, FULLTEXT CATALOG, LOGIN, MESSAGE TYPE, objet, REMOTE SERVICE BINDING, rôle, itinéraire, schéma, serveur, SERVICE , CLÉ SYMÉTRIQUE, TYPE, UTILISATEUR, COLLECTION DE SCHÉMAS XML.  
  
## <a name="columns-returned"></a>Colonnes retournées  
 Le tableau suivant répertorie les colonnes qui **fn_my_permissions** retourne. Chaque ligne renvoyée décrit une autorisation détenue par le contexte de sécurité actuel sur l'élément sécurisable. Renvoie NULL si la requête échoue.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|entity_name|**sysname**|Nom de l'élément sécurisable pour lequel les autorisations affichées sont effectivement accordées.|  
|subentity_name|**sysname**|Nom de colonne si l'élément sécurisable contient des colonnes, sinon NULL.|  
|permission_name|**nvarchar**|Nom de l’autorisation.|  
  
## <a name="remarks"></a>Notes  
 Cette fonction table renvoie la liste des autorisations effectives détenues par le principal appelant sur un élément sécurisable spécifié. Une autorisation effective est l’une des opérations suivantes :  
  
-   une autorisation accordée directement au principal et non refusée ;  
  
-   une autorisation impliquée par une autorisation de niveau supérieur détenue par le principal et non refusée ;  
  
-   une autorisation accordée à un rôle ou à un groupe dont le principal est membre et non refusé ;  
  
-   une autorisation détenue par un rôle ou un groupe dont le principal est membre et non refusé.  
  
 L'évaluation des autorisations a toujours lieu dans le contexte de sécurité de l'appelant. Pour déterminer si un autre principal dispose d'une autorisation effective, l'appelant doit disposer de l'autorisation IMPERSONATE sur ce principal.  
  
 Pour les entités au niveau schéma, les noms non NULL en une, deux ou trois parties sont acceptés. Pour les entités de niveau base de données, un nom en une partie est accepté, avec une valeur null signifie «*base de données actuelle*». Pour le serveur lui-même, la valeur NULL (« serveur actif ») est exigée. **fn_my_permissions** ne peut pas vérifier les autorisations sur un serveur lié.  
  
 La requête suivante renvoie la liste des classes intégrées des éléments sécurisables :  
  
```  
SELECT DISTINCT class_desc FROM fn_builtin_permissions(default)  
    ORDER BY class_desc;  
GO  
```  
  
 Si la valeur par défaut est fourni en tant que la valeur de *sécurisable* ou *securable_class*, la valeur sera interprétée comme NULL.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-effective-permissions-on-the-server"></a>A. Affichage des autorisations effectives sur le serveur  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur le serveur.  
  
```  
SELECT * FROM fn_my_permissions(NULL, 'SERVER');  
GO  
```  
  
### <a name="b-listing-effective-permissions-on-the-database"></a>B. Affichage des autorisations effectives sur la base de données  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions (NULL, 'DATABASE');  
GO  
```  
  
### <a name="c-listing-effective-permissions-on-a-view"></a>C. Affichage des autorisations effectives sur une vue  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur la vue `vIndividualCustomer` du schéma `Sales` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions('Sales.vIndividualCustomer', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;   
GO   
```  
  
### <a name="d-listing-effective-permissions-of-another-user"></a>D. Affichage des autorisations effectives d'un autre utilisateur  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'utilisateur de base de données `Wanida` sur la table `Employee` du schéma `HumanResources` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. L'appelant doit disposer de l'autorisation IMPERSONATE sur l'utilisateur `Wanida`.  
  
```  
EXECUTE AS USER = 'Wanida';  
SELECT * FROM fn_my_permissions('HumanResources.Employee', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;    
REVERT;  
GO  
```  
  
### <a name="e-listing-effective-permissions-on-a-certificate"></a>E. Affichage des autorisations effectives sur un certificat  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur un certificat nommé `Shipping47` dans la base de données active.  
  
```  
SELECT * FROM fn_my_permissions('Shipping47', 'CERTIFICATE');  
GO  
```  
  
### <a name="f-listing-effective-permissions-on-an-xml-schema-collection"></a>F. Affichage des autorisations effectives sur une collection de schémas XML  
 L’exemple suivant retourne une liste des autorisations effectives de l’appelant sur une Collection de schémas XML nommée `ProductDescriptionSchemaCollection` dans les [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] base de données.  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions('ProductDescriptionSchemaCollection',  
    'XML SCHEMA COLLECTION');  
GO  
```  
  
### <a name="g-listing-effective-permissions-on-a-database-user"></a>G. Affichage des autorisations effectives sur un utilisateur de base de données  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur un utilisateur nommé `MalikAr` dans la base de données active.  
  
```  
SELECT * FROM fn_my_permissions('MalikAr', 'USER');  
GO  
```  
  
### <a name="h-listing-effective-permissions-of-another-login"></a>H. Affichage des autorisations effectives d'un autre nom de connexion  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `WanidaBenshoof` sur la table `Employee` du schéma `HumanResources` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. L'appelant doit disposer de l'autorisation IMPERSONATE sur le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `WanidaBenshoof`.  
  
```  
EXECUTE AS LOGIN = 'WanidaBenshoof';  
SELECT * FROM fn_my_permissions('AdventureWorks2012.HumanResources.Employee', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;    
REVERT;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)  
  
  
