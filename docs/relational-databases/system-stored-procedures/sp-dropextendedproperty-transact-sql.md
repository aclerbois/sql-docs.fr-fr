---
title: sp_dropextendedproperty (Transact-SQL) | Documents Microsoft
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
- sp_dropextendedproperty_TSQL
- sp_dropextendedproperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropextendedproperty
ms.assetid: 4851865a-86ca-4823-991a-182dd1934075
caps.latest.revision: 45
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 750b1df6bff427c2e5c4931ad3007a66ddbb1917
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdropextendedproperty-transact-sql"></a>sp_dropextendedproperty (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime une propriété étendue existante.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropextendedproperty   
    [ @name = ] { 'property_name' }  
      [ , [ @level0type = ] { 'level0_object_type' }   
        , [ @level0name = ] { 'level0_object_name' }   
            [ , [ @level1type = ] { 'level1_object_type' }   
              , [ @level1name = ] { 'level1_object_name' }   
                [ , [ @level2type = ] { 'level2_object_type' }   
                  , [ @level2name = ] { 'level2_object_name' }   
                ]   
            ]   
        ]   
    ]   
```  
  
## <a name="arguments"></a>Arguments  
 [ @name=] {'*property_name*'}  
 Nom de la propriété à supprimer. *property_name* est **sysname** et ne peut pas être NULL.  
  
 [ @level0type=] {'*level0_object_type*'}  
 Nom du type d'objet de niveau 0 spécifié. *level0_object_type* est **varchar (128)**, avec NULL comme valeur par défaut.  
  
 Les entrées valides sont ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE et NULL.  
  
> [!IMPORTANT]  
>  Les types de niveau 0 USER et TYPE seront éliminés dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement. À la place de USER, utilisez SCHEMA en tant que type de niveau 0. Pour TYPE, utilisez SCHEMA comme type de niveau 0 et TYPE comme type de niveau 1.  
  
 [ @level0name=] {'*level0_object_name*'}  
 Nom du type d'objet de niveau 0 spécifié. *level0_object_name* est **sysname** avec NULL comme valeur par défaut.  
  
 [ @level1type=] {'*level1_object_type*'}  
 Type d'objet de niveau 1. *level1_object_type* est **varchar (128)** avec NULL comme valeur par défaut. Les entrées valides sont AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE_TYPE, TYPE, VIEW, XML SCHEMA COLLECTION et NULL.  
  
 [ @level1name=] {'*level1_object_name*'}  
 Nom du type d'objet de niveau 1 spécifié. *level1_object_name* est **sysname** avec NULL comme valeur par défaut.  
  
 [ @level2type=] {'*level2_object_type*'}  
 Type d'objet de niveau 2. *level2_object_type* est **varchar (128)** avec NULL comme valeur par défaut. Les entrées valides sont COLUMN, CONSTRAINT, EVENT NOTIFICATION, INDEX, PARAMETER, TRIGGER et NULL.  
  
 [ @level2name=] {'*level2_object_name*'}  
 Nom du type d'objet de niveau 2 spécifié. *level2_object_name* est **sysname** avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Les objets d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont répartis sur trois niveaux (0, 1 et 2) pour la définition des propriétés étendues. Le niveau 0 est le niveau le plus élevé et est composé d'objets relevant de l'étendue de la base de données. Les objets de niveau 1 figurent dans l'étendue du schéma ou de l'utilisateur tandis que les objets de niveau 2 se trouvent dans les objets de niveau 1. Vous pouvez définir des propriétés étendues pour les objets de tous ces niveaux. Les références à un objet d'un niveau donné doivent être qualifiées par les types et les noms de tous les objets de niveau supérieur.  
  
 Étant donné un valide *property_name*, si tous les noms et les types d’objet sont null, et une propriété existe sur la base de données en cours, que la propriété est supprimée. Consultez l'exemple B fourni plus loin dans cette rubrique.  
  
## <a name="permissions"></a>Autorisations  
 Les membres des rôles de base de données fixes db_owner et db_ddladmin peuvent supprimer des propriétés étendues de n'importe quel objet, avec toutefois la restriction suivante : db_ddladmin ne peut pas ajouter de propriétés à la base de données elle-même, ni aux utilisateurs ou rôles.  
  
 Les utilisateurs peuvent supprimer des propriétés étendues sur des objets qu'ils possèdent ou pour lesquels ils bénéficient d'autorisations ALTER ou CONTROL.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dropping-an-extended-property-on-a-column"></a>A. Suppression d'une propriété étendue d'une colonne  
 L'exemple suivant supprime la propriété `caption` de la colonne `id` de la table `T1` contenue dans le schéma `dbo`.  
  
```  
CREATE TABLE T1 (id int , name char (20));  
GO  
EXEC sp_addextendedproperty   
     @name = 'caption'   
    ,@value = 'Employee ID'   
    ,@level0type = 'schema'   
    ,@level0name = dbo  
    ,@level1type = 'table'  
    ,@level1name = 'T1'  
    ,@level2type = 'column'  
    ,@level2name = id;  
GO  
EXEC sp_dropextendedproperty   
     @name = 'caption'   
    ,@level0type = 'schema'   
    ,@level0name = dbo  
    ,@level1type = 'table'  
    ,@level1name = 'T1'  
    ,@level2type = 'column'  
    ,@level2name = id;  
GO  
DROP TABLE T1;  
GO  
```  
  
### <a name="b-dropping-an-extended-property-on-a-database"></a>B. Suppression d'une propriété étendue d'une base de données  
 L’exemple suivant supprime la propriété nommée `MS_Description` à partir de la [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] base de données exemple. Étant donné que la propriété est dans la base de données, aucun type et nom d'objet n'est spécifié.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_dropextendedproperty   
@name = N'MS_Description';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Sys.fn_listextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [Sys.extended_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
