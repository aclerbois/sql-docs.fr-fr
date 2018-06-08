---
title: sp_rename (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_rename_TSQL
- sp_rename
dev_langs:
- TSQL
helpviewer_keywords:
- renaming indexes
- renaming columns
- sp_rename
- renaming tables
ms.assetid: bc3548f0-143f-404e-a2e9-0a15960fc8ed
caps.latest.revision: 54
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 97d14dc014827310706bdea8143e41a628a666d6
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sprename-transact-sql"></a>sp_rename (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Modifie le nom d'un objet créé par l'utilisateur dans la base de données actuelle. Cet objet peut être une table, l’index, la colonne, type de données alias ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] type common language runtime (CLR) définis par l’utilisateur.  
  
> [!CAUTION]  
>  La modification d'une partie du nom de l'objet peut provoquer des problèmes dans des scripts et des procédures stockées. Nous vous recommandons de ne pas utiliser cette instruction pour renommer des procédures stockées, des déclencheurs, des fonctions définies par l'utilisateur ou des vues ; supprimez plutôt l'objet et recréez-le avec le nouveau nom.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_rename [ @objname = ] 'object_name' , [ @newname = ] 'new_name'   
    [ , [ @objtype = ] 'object_type' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [ @objname =] '*nom_objet*'  
 Nom actuel, complet ou non, de l'objet utilisateur ou du type de données. Si l’objet à renommer est une colonne dans une table, *nom_objet* doit être sous la forme *table.column* ou *configuration : schéma.table.colonne*. Si l’objet à renommer est un index, *nom_objet* doit être sous la forme *table.index* ou *schema.table.index*. Si l’objet à renommer est une contrainte, *nom_objet* doit être sous la forme *schema.constraint*.  
  
 Les guillemets ne sont nécessaires que si un nom complet d'objet est spécifié. Si un nom qualifié complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données active. *nom_objet* est **nvarchar(776)**, sans valeur par défaut.  
  
 [ @newname =] '*nouveau_nom*'  
 Nouveau nom de l'objet spécifié. *nouveau_nom* doit être un nom d’une seule partie et doit respecter les règles gouvernant les identificateurs. *NewName* est **sysname**, sans valeur par défaut.  
  
> [!NOTE]  
>  Les noms de déclencheurs ne peuvent pas commencer par # ou par ##.  
  
 [ @objtype =] '*object_type*'  
 Type d'objet renommé. *object_type* est **varchar(13)**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|COLUMN|Colonne à renommer.|  
|DATABASE|Base de données définie par l'utilisateur. Ce type d'objet est nécessaire pour renommer une base de données.|  
|INDEX|Index défini par l'utilisateur. Renommer un index avec des statistiques, renomme également automatiquement les statistiques.|  
|OBJECT|Un élément d’un type de suivi dans [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). Par exemple, OBJECT peut être utilisé pour renommer des objets, y compris des contraintes (CHECK, FOREIGN KEY, PRIMARY/UNIQUE KEY), des tables utilisateur et des règles.|  
|STATISTICS|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Statistiques créées explicitement par un utilisateur ou créées implicitement avec un index. Renommer les statistiques d'un index renomme également automatiquement l'index.|  
|USERDATATYPE|A [Types définis par l’utilisateur CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) ajouté par l’exécution de [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md) ou [sp_addtype](../../relational-databases/system-stored-procedures/sp-addtype-transact-sql.md).|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou un nombre différent de zéro (échec)  
  
## <a name="remarks"></a>Notes  
 Il n'est possible de modifier le nom d'un objet ou d'un type de données que dans la base de données active. Il est impossible de modifier les noms de la plupart des types de données système et des objets système.  
  
 sp_rename renomme automatiquement l'index associé chaque fois qu'une contrainte PRIMARY KEY ou UNIQUE est renommée. Si un index renommé est lié à une contrainte PRIMARY KEY, cette dernière est également renommée automatiquement par sp_rename.  
  
 Vous pouvez utiliser sp_rename pour renommer des index XML primaires et secondaires.  
  
 Renommer une procédure stockée, fonction, vue ou au déclencheur ne modifie pas le nom de l’objet correspondant dans la colonne de définition de la [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) affichage catalogue ou obtenus à l’aide du [OBJECT_ DÉFINITION](../../t-sql/functions/object-definition-transact-sql.md) fonction intégrée. Par conséquent, il est déconseillé d'utiliser sp_rename pour renommer ces types d'objets. Supprimez, puis recréez plutôt l'objet avec son nouveau nom.  
  
 Le fait de renommer un objet tel qu'une table ou une colonne ne renomme pas automatiquement les références à cet objet. Vous devez modifier manuellement tout objet qui référence l'objet renommé. Par exemple, si vous renommez une colonne de table et si cette colonne est référencée dans un déclencheur, vous devez modifier le déclencheur pour refléter le nouveau nom de colonne. Utilisez [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) pour obtenir la liste des dépendances de l’objet avant de le renommer.  
  
## <a name="permissions"></a>Autorisations  
 Pour renommer des objets, des colonnes et des index, il vous faut l'autorisation ALTER sur l'objet concerné. Pour renommer des types définis par l'utilisateur, il vous faut une autorisation CONTROL sur le type concerné. Pour renommer une base de données, vous devez être membre des rôles serveur fixes dbcreator ou sysadmin.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-renaming-a-table"></a>A. Modification du nom d'une table  
 L'exemple suivant renomme la table `SalesTerritory` en `SalesTerr` dans le schéma `Sales` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_rename 'Sales.SalesTerritory', 'SalesTerr';  
GO  
```  
  
### <a name="b-renaming-a-column"></a>B. Modification du nom d'une colonne  
 L’exemple suivant renomme la `TerritoryID` colonne dans la `SalesTerritory` table `TerrID`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_rename 'Sales.SalesTerritory.TerritoryID', 'TerrID', 'COLUMN';  
GO  
```  
  
### <a name="c-renaming-an-index"></a>C. Modification du nom d'un index  
 L'exemple suivant renomme l'index `IX_ProductVendor_VendorID` en `IX_VendorID`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_rename N'Purchasing.ProductVendor.IX_ProductVendor_VendorID', N'IX_VendorID', N'INDEX';  
GO  
```  
  
### <a name="d-renaming-an-alias-data-type"></a>D. Modification du nom d'un type de données d'alias  
 L'exemple suivant renomme le type de données d'alias `Phone` en `Telephone`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_rename N'Phone', N'Telephone', N'USERDATATYPE';  
GO  
```  
  
### <a name="e-renaming-constraints"></a>E. Attribution d'un nouveau nom aux contraintes  
 Les exemples suivants renomment une contrainte PRIMARY KEY, une contrainte CHECK et une contrainte FOREIGN KEY. Lors de l'attribution d'un nouveau nom à une contrainte, le schéma auquel la contrainte appartient doit être spécifié.  
  
```  
USE AdventureWorks2012;   
GO  
-- Return the current Primary Key, Foreign Key and Check constraints for the Employee table.  
SELECT name, SCHEMA_NAME(schema_id) AS schema_name, type_desc  
FROM sys.objects  
WHERE parent_object_id = (OBJECT_ID('HumanResources.Employee'))   
AND type IN ('C','F', 'PK');   
GO  
  
-- Rename the primary key constraint.  
sp_rename 'HumanResources.PK_Employee_BusinessEntityID', 'PK_EmployeeID';  
GO  
  
-- Rename a check constraint.  
sp_rename 'HumanResources.CK_Employee_BirthDate', 'CK_BirthDate';  
GO  
  
-- Rename a foreign key constraint.  
sp_rename 'HumanResources.FK_Employee_Person_BusinessEntityID', 'FK_EmployeeID';  
  
-- Return the current Primary Key, Foreign Key and Check constraints for the Employee table.  
SELECT name, SCHEMA_NAME(schema_id) AS schema_name, type_desc  
FROM sys.objects  
WHERE parent_object_id = (OBJECT_ID('HumanResources.Employee'))   
AND type IN ('C','F', 'PK');  
  
```  
  
```  
  
name                                  schema_name        type_desc  
------------------------------------- ------------------ ----------------------  
FK_Employee_Person_BusinessEntityID   HumanResources     FOREIGN_KEY_CONSTRAINT  
PK_Employee_BusinessEntityID          HumanResources     PRIMARY_KEY_CONSTRAINT  
CK_Employee_BirthDate                 HumanResources     CHECK_CONSTRAINT  
CK_Employee_MaritalStatus             HumanResources     CHECK_CONSTRAINT  
CK_Employee_HireDate                  HumanResources     CHECK_CONSTRAINT  
CK_Employee_Gender                    HumanResources     CHECK_CONSTRAINT  
CK_Employee_VacationHours             HumanResources     CHECK_CONSTRAINT  
CK_Employee_SickLeaveHours            HumanResources     CHECK_CONSTRAINT  
  
(7 row(s) affected)  
  
name                                  schema_name        type_desc  
------------------------------------- ------------------ ----------------------  
FK_Employee_ID                        HumanResources     FOREIGN_KEY_CONSTRAINT  
PK_Employee_ID                        HumanResources     PRIMARY_KEY_CONSTRAINT  
CK_BirthDate                          HumanResources     CHECK_CONSTRAINT  
CK_Employee_MaritalStatus             HumanResources     CHECK_CONSTRAINT  
CK_Employee_HireDate                  HumanResources     CHECK_CONSTRAINT  
CK_Employee_Gender                    HumanResources     CHECK_CONSTRAINT  
CK_Employee_VacationHours             HumanResources     CHECK_CONSTRAINT  
CK_Employee_SickLeaveHours            HumanResources     CHECK_CONSTRAINT  
  
(7 row(s) affected)  
  
```  
  
### <a name="f-renaming-statistics"></a>F. Renommer des statistiques  
 L’exemple suivant crée un objet de statistiques nommé contactMail1 et renomme la statistique pour NewContact à l’aide de sp_rename. Lorsque vous renommez des statistiques, l'objet doit être spécifié en respectant le format schema.table.statistics_name.  
  
```  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
  
sp_rename 'Person.Person.ContactMail1', 'NewContact','Statistics';  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
