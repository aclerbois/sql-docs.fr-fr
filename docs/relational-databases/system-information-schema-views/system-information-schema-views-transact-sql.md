---
title: Vues de schémas d’informations système (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- information schema views
- schemas [SQL Server], information schema views
- metadata [SQL Server], views
- views [SQL Server], information schema
- system views [SQL Server], information schema
ms.assetid: 7e9f1dfe-27e9-40e7-8fc7-bfc5cae6be10
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2a3c0ef6d8a3c4c774b441e807c4ca513b214f26
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="system-information-schema-views-transact-sql"></a>Vues de schémas d’informations système (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Une vue de schémas d'informations est l'une des diverses méthodes que fournit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'obtention de métadonnées. Les vues de schémas d'informations fournissent une vue interne indépendante des tables système des métadonnées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elles permettent aux applications de fonctionner correctement bien que des changements importants aient été apportés aux tables système sous-jacentes. Les vues de schémas d'informations incluses dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont conformes à la norme ISO pour INFORMATION_SCHEMA.  
  
> [!IMPORTANT]  
>  Les vues de schémas d'informations ont fait l'objet de certaines modifications qui rompent la compatibilité descendante. Ces modifications sont décrites dans les rubriques spécifiques à chaque vue.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge une convention de dénomination en trois parties pour les références au serveur actif. La norme ISO prend également en charge la convention des noms en trois parties. Cependant, les noms utilisés dans les deux conventions sont différents. Les  vues de schémas d'informations sont définies dans un schéma spécial appelé INFORMATION_SCHEMA. Ce schéma figure dans chaque base de données. Chaque vue de schémas d'informations comprend des métadonnées pour tous les objets de données stockés dans une base de données particulière. Le tableau suivant décrit la relation entre les noms [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les noms standard SQL.  
  
|Nom SQL Server|Nom standard SQL équivalent|  
|---------------------|-----------------------------------------------|  
|Base de données|Catalogue|  
|Schéma|Schéma|  
|Objet|Objet|  
|type de données défini par l'utilisateur|Domaine|  
  
 Cette convention de mappage de noms s'applique aux vues compatibles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ISO suivantes.  
  
|||  
|-|-|  
|[CHECK_CONSTRAINTS](../../relational-databases/system-information-schema-views/check-constraints-transact-sql.md)|[REFERENTIAL_CONSTRAINTS](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md)|  
|[COLUMN_DOMAIN_USAGE](../../relational-databases/system-information-schema-views/column-domain-usage-transact-sql.md)|[ROUTINES](../../relational-databases/system-information-schema-views/routines-transact-sql.md)|  
|[COLUMN_PRIVILEGES](../../relational-databases/system-information-schema-views/column-privileges-transact-sql.md)|[ROUTINE_COLUMNS](../../relational-databases/system-information-schema-views/routine-columns-transact-sql.md)|  
|[COLUMNS](../../relational-databases/system-information-schema-views/columns-transact-sql.md)|[SCHEMATA](../../relational-databases/system-information-schema-views/schemata-transact-sql.md)|  
|[CONSTRAINT_COLUMN_USAGE](../../relational-databases/system-information-schema-views/constraint-column-usage-transact-sql.md)|[TABLE_CONSTRAINTS](../../relational-databases/system-information-schema-views/table-constraints-transact-sql.md)|  
|[CONSTRAINT_TABLE_USAGE](../../relational-databases/system-information-schema-views/constraint-table-usage-transact-sql.md)|[TABLE_PRIVILEGES](../../relational-databases/system-information-schema-views/table-privileges-transact-sql.md)|  
|[DOMAIN_CONSTRAINTS](../../relational-databases/system-information-schema-views/domain-constraints-transact-sql.md)|[TABLES](../../relational-databases/system-information-schema-views/tables-transact-sql.md)|  
|[DOMAINS](../../relational-databases/system-information-schema-views/domains-transact-sql.md)|[VIEW_COLUMN_USAGE](../../relational-databases/system-information-schema-views/view-column-usage-transact-sql.md)|  
|[KEY_COLUMN_USAGE](../../relational-databases/system-information-schema-views/key-column-usage-transact-sql.md)|[VIEW_TABLE_USAGE](../../relational-databases/system-information-schema-views/view-table-usage-transact-sql.md)|  
|[PARAMETERS](../../relational-databases/system-information-schema-views/parameters-transact-sql.md)|[VIEWS](../../relational-databases/system-information-schema-views/views-transact-sql.md)|  
  
 En outre, certaines vues comprennent des références à d'autres classes de données telles que les données de type caractère ou binaire.  
  
 Lorsque vous faites référence aux vues de schémas d'informations, vous devez utiliser un nom qualifié qui inclut le nom de schéma `INFORMATION_SCHEMA`. Par exemple :  
  
```  
SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, COLUMN_DEFAULT  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = N'Product';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
