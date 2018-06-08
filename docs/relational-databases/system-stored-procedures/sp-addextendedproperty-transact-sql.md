---
title: sp_addextendedproperty (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_addextendedproperty
- sp_addextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addextendedproperty
ms.assetid: 565483ea-875b-4133-b327-d0006d2d7b4c
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8ebd317abcd9fec6ee6e116c0bbacc63bf547be5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddextendedproperty-transact-sql"></a>sp_addextendedproperty (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Ajoute une nouvelle propriété étendue à un objet de base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addextendedproperty  
    [ @name = ] { 'property_name' }  
    [ , [ @value = ] { 'value' }   
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
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @name ] = {'*property_name*'}  
 Est le nom de la propriété à ajouter. *property_name* est **sysname** et ne peut pas être NULL. Les noms peuvent également comporter des chaînes de caractères vides ou non alphanumériques, ainsi que des valeurs binaires.  
  
 [ @value=] {'*valeur*'}  
 Valeur à associer à la propriété. *valeur* est **sql_variant**, avec NULL comme valeur par défaut. La taille de l'argument *value* ne peut pas dépasser les 7 500 octets.  
  
 [ @level0type=] {'*level0_object_type*'}  
 Type d'objet de niveau 0. *level0_object_type* est **varchar (128)**, avec NULL comme valeur par défaut.  
  
 Les entrées valides sont ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE, PLAN GUIDE et NULL.  
  
> [!IMPORTANT]  
>  La possibilité de spécifier USER comme type de niveau 0 dans une propriété étendue d'un objet de type de niveau 1 sera supprimée dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez à la place SCHEMA en tant que type de niveau 0. Par exemple, lorsque vous définissez une propriété étendue sur une table, spécifiez le schéma de la table au lieu d'un nom d'utilisateur. La possibilité de spécifier TYPE comme type de niveau 0 sera supprimée dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour TYPE, utilisez SCHEMA comme type de niveau 0 et TYPE comme type de niveau 1.  
  
 [ @level0name=] {'*level0_object_name*'}  
 Nom du type d'objet de niveau 0 spécifié. *level0_object_name* est **sysname** avec NULL comme valeur par défaut.  
  
 [ @level1type=] {'*level1_object_type*'}  
 Type d'objet de niveau 1. *level1_object_type* est **varchar (128)**, avec NULL comme valeur par défaut. Les entrées valides sont AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE_TYPE, TYPE, VIEW, XML SCHEMA COLLECTION et NULL.  
  
 [ @level1name=] {'*level1_object_name*'}  
 Nom du type d'objet de niveau 1 spécifié. *level1_object_name* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ @level2type=] {'*level2_object_type*'}  
 Type d'objet de niveau 2. *level2_object_type* est **varchar (128)**, avec NULL comme valeur par défaut. Les entrées valides sont COLUMN, CONSTRAINT, EVENT NOTIFICATION, INDEX, PARAMETER, TRIGGER et NULL.  
  
 [ @level2name=] {'*level2_object_name*'}  
 Nom du type d'objet de niveau 2 spécifié. *level2_object_name* est **sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Les objets d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont répartis sur trois niveaux (0, 1 et 2) pour la définition des propriétés étendues. Le niveau 0 est le plus élevé et est défini en tant qu'objets contenus dans l'étendue de la base de données. Les objets de niveau 1 figurent dans l'étendue du schéma ou de l'utilisateur tandis que les objets de niveau 2 se trouvent dans les objets de niveau 1. Vous pouvez définir des propriétés étendues pour les objets de tous ces niveaux.  
  
 Les références à un objet d'un niveau donné doivent être qualifiées par les noms des objets de niveau supérieur possédant ou contenant l'objet en question. Par exemple, lorsque vous ajoutez une propriété étendue à une colonne de table (niveau 2), vous devez également indiquer le nom de la table (niveau 1) qui contient la colonne et le schéma (niveau 0) qui contient la table.  
  
 Si tous les types et noms d'objet sont NULL, la propriété appartient à la base de données active.  
  
 Les propriétés étendues ne sont pas autorisées sur les objets système, les objets se trouvant en dehors de l'étendue d'une base de données définie par l'utilisateur ou les objets non répertoriés dans les arguments comme des entrées valides.  
  
 Les propriétés étendues ne sont pas autorisées sur les tables optimisées en mémoire.  
  
## <a name="replicating-extended-properties"></a>Réplication des propriétés étendues  
 Les propriétés étendues sont répliquées uniquement dans la synchronisation initiale entre le serveur de publication et l'Abonné. Si vous ajoutez ou modifiez une propriété étendue après la synchronisation initiale, la modification n'est pas répliquée. Pour plus d’informations sur la réplication des objets de base de données, consultez [publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
## <a name="schema-vs-user"></a>Schéma ou  Utilisateur  
 Nous ne vous recommandons pas de spécifier l'argument USER comme type de niveau 0 lorsque vous appliquez une propriété étendue à un objet de base de données, car vous pourriez engendrer une ambiguïté de résolution de noms. Par exemple, supposez que l'utilisateur Marie détient deux schémas (Marie et MySchema) et que ceux-ci contiennent tous deux une table intitulée MyTable. Si Mary ajoute une propriété étendue à la table MyTable et spécifie  **@level0type @level0type ='**,  **@level0name = Mary**, il n’est pas clairement à quelle table la propriété étendue est appliquée. Pour assurer la compatibilité descendante, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique la propriété étendue à la table contenue dans le schéma nommé Mary.  
  
## <a name="permissions"></a>Autorisations  
 Les membres des rôles de base de données fixes db_owner et db_ddladmin peuvent ajouter des propriétés étendues à n'importe quel objet, avec toutefois la restriction suivante : db_ddladmin ne peut pas ajouter de propriétés à la base de données elle-même, ni aux utilisateurs ou rôles.  
  
 Les utilisateurs peuvent ajouter des propriétés étendues à des objets qu'ils possèdent ou pour lesquels ils bénéficient d'autorisations ALTER ou CONTROL.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-an-extended-property-to-a-database"></a>A. Ajout d'une propriété étendue à une base de données  
 L'exemple suivant ajoute la propriété intitulée `'Caption'` et ayant `'AdventureWorks2012 Sample OLTP Database'` pour valeur à la base de données exemple `AdventureWorks2012` .  
  
```  
USE AdventureWorks2012;  
GO  
--Add a caption to the AdventureWorks2012 Database object itself.  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'AdventureWorks2012 Sample OLTP Database';  
```  
  
### <a name="b-adding-an-extended-property-to-a-column-in-a-table"></a>B. Ajout d'une propriété étendue à une colonne dans une table  
 L'exemple suivant ajoute une propriété de légende à la colonne `PostalCode` dans la table `Address`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'Postal code is a required column.',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table',  @level1name = 'Address',  
@level2type = N'Column', @level2name = 'PostalCode';  
GO  
```  
  
### <a name="c-adding-an-input-mask-property-to-a-column"></a>C. Ajout d'une propriété de masque de saisie à une colonne  
 L'exemple suivant ajoute la propriété de masque de saisie`99999 or 99999-9999 or #### ###`à la colonne `PostalCode` dans la table `Address`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Input Mask ', @value = '99999 or 99999-9999 or #### ###',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table', @level1name = 'Address',   
@level2type = N'Column',@level2name = 'PostalCode';  
GO  
```  
  
### <a name="d-adding-an-extended-property-to-a-filegroup"></a>D. Ajout d'une propriété étendue à un groupe de fichiers  
 L'exemple suivant ajoute une propriété étendue au groupe de fichiers `PRIMARY` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Primary filegroup for the AdventureWorks2012 sample database.',   
@level0type = N'FILEGROUP', @level0name = 'PRIMARY';  
GO  
```  
  
### <a name="e-adding-an-extended-property-to-a-schema"></a>E. Ajout d'une propriété étendue à un schéma  
 L'exemple suivant ajoute une propriété étendue au schéma `HumanResources` .  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',  
@value = N'Contains objects related to employees and departments.',  
@level0type = N'SCHEMA',   
@level0name = 'HumanResources';  
```  
  
### <a name="f-adding-an-extended-property-to-a-table"></a>F. Ajout d'une propriété étendue à une table  
 L'exemple suivant ajoute une propriété étendue à la table `Address` du schéma `Person` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Street address information for customers, employees, and vendors.',   
@level0type = N'SCHEMA', @level0name = 'Person',  
@level1type = N'TABLE',  @level1name = 'Address';  
GO  
```  
  
### <a name="g-adding-an-extended-property-to-a-role"></a>G. Ajout d'une propriété étendue à un rôle  
 L'exemple suivant crée un rôle d'application et ajoute une propriété étendue à ce rôle.  
  
```  
USE AdventureWorks2012;   
GO  
CREATE APPLICATION ROLE Buyers  
WITH Password = '987G^bv876sPY)Y5m23';   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Application Role for the Purchasing Department.',  
@level0type = N'USER',  
@level0name = 'Buyers';  
```  
  
### <a name="h-adding-an-extended-property-to-a-type"></a>H. Ajout d'une propriété étendue à un type  
 L'exemple suivant ajoute une propriété étendue à un type.  
  
```  
USE AdventureWorks2012;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Data type (alias) to use for any column that represents an order number. For example a sales order number or purchase order number.',   
@level0type = N'SCHEMA',   
@level0name = N'dbo',   
@level1type = N'TYPE',   
@level1name = N'OrderNumber';  
```  
  
### <a name="i-adding-an-extended-property-to-a-user"></a>I. Ajout d'une propriété étendue à un utilisateur  
 L'exemple suivant crée un utilisateur et ajoute une propriété étendue à cet utilisateur.  
  
```  
USE AdventureWorks2012;   
GO  
CREATE USER CustomApp WITHOUT LOGIN ;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'User for an application.',   
@level0type = N'USER',   
@level0name = N'CustomApp';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Sys.fn_listextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)  
  
  
