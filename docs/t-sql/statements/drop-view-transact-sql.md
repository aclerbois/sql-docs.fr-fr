---
title: DROP VIEW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DROP_VIEW_TSQL
- DROP VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- dropping views
- DROP VIEW statement
- deleting views
- indexed views [SQL Server], removing
- views [SQL Server], removing
- removing views
ms.assetid: 03cea355-e39c-46e1-b7db-8832038669dd
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f1b3f55f30ae5c213ccb5373613625c46b105a12
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drop-view-transact-sql"></a>DROP VIEW (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Supprime une ou plusieurs vues de la base de données active. DROP VIEW peut être exécuté sur des vues indexées.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
DROP VIEW [ IF EXISTS ] [ schema_name . ] view_name [ ...,n ] [ ; ]  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
DROP VIEW [ schema_name . ] view_name   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)), [!INCLUDE[sssds](../../includes/sssds-md.md)].  
  
 Supprime, de manière conditionnelle, la vue uniquement si elle existe déjà.  
  
 *schema_name*  
 Nom du schéma auquel appartient la vue.  
  
 *view_name*  
 Nom de la vue à supprimer  
  
## <a name="remarks"></a>Notes   
 Lorsque vous supprimez une vue, sa définition et d'autres informations la concernant sont supprimées du catalogue système. Toutes les autorisations pour la vue sont également supprimées.  
  
 Toute vue d'une table qui est supprimée au moyen de DROP TABLE doit être supprimée de manière explicite à l'aide de DROP VIEW.  
  
 Lorsqu'elle est exécutée sur une vue indexée, l'instruction DROP VIEW supprime automatiquement tous les index de la vue. Pour afficher tous les index d’une vue, utilisez la procédure stockée [sp_helpindex](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md).  
  
 Lorsque vous effectuez une requête par l'intermédiaire d'une vue, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie que tous les objets de base de données référencés dans l'instruction existent, qu'ils sont valides dans le contexte de l'instruction, et que les instructions de modification de données ne violent pas les règles d'intégrité des données. Si une vérification échoue, le système retourne un message d'erreur. Si la vérification réussit, l'action est transformée en une action applicable dans la ou les tables sous-jacentes. Si les tables ou les vues sous-jacentes ont été modifiées depuis la création initiale de la vue, il peut être utile de supprimer puis de recréer la vue.  
  
 Pour plus d’informations sur la définition des dépendances d’une vue spécifique, consultez [sys.sql_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md).  
  
 Pour plus d’informations sur l’affichage du texte d’une vue, consultez [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **CONTROL** sur la vue, l’autorisation **ALTER** sur le schéma contenant la vue, ou l’appartenance au rôle serveur fixe **db_ddladmin**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-drop-a-view"></a>A. Supprimer une vue  
 Cet exemple supprime la vue `Reorder`.  
  
```  
DROP VIEW dbo.Reorder ;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-view-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
 
