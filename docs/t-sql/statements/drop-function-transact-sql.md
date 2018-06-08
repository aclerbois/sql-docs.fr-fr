---
title: DROP FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DROP_FUNCTION_TSQL
- DROP FUNCTION
dev_langs:
- TSQL
helpviewer_keywords:
- user-defined functions [SQL Server], removing
- removing user-defined functions
- DROP FUNCTION statement
- dropping user-defined functions
- deleting user-defined functions
ms.assetid: ee5ad283-9e44-4109-902f-0ce12669ee11
caps.latest.revision: 49
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 81fd6b618a3c266e7c0aa75f95f6013d5f539860
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drop-function-transact-sql"></a>DROP FUNCTION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]

  Supprime une ou plusieurs fonctions définies par l'utilisateur de la base de données active. Les fonctions définies par l’utilisateur sont créées à l’aide de l’instruction [CREATE FUNCTION](../../t-sql/statements/create-function-transact-sql.md) et modifiées à l’aide de l’instruction [ALTER FUNCTION](../../t-sql/statements/alter-function-transact-sql.md).  
  
 La fonction DROP prend en charge les fonctions définies par l’utilisateur scalaires compilées en mode natif. Pour plus d’informations, consultez [Fonctions scalaires définies par l’utilisateur pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
 -- SQL Server, Azure SQL Database 

DROP FUNCTION [ IF EXISTS ] { [ schema_name. ] function_name } [ ,...n ]   
[;]
```

```  
 -- Azure SQL Data Warehouse, Parallel Data Warehouse 

DROP FUNCTION [ schema_name. ] function_name
[;] 
```  
   
  
## <a name="arguments"></a>Arguments  
 *IF EXISTS*    
 Supprime, de manière conditionnelle, la fonction uniquement si elle existe déjà. Disponible à partir de [!INCLUDE[ssnoversion_md](../../includes/ssnoversion_md.md)] 2016 et dans [!INCLUDE[sssds_md](../../includes/sssds_md.md)].
  
 *schema_name*  
 Nom du schéma auquel appartient la fonction définie par l'utilisateur.  
  
 *function_name*  
 Nom de la fonction ou des fonctions définies par l'utilisateur à supprimer. La spécification du nom de schéma est facultative. Il n'est pas possible de spécifier le nom du serveur et de la base de données.  
  
## <a name="remarks"></a>Notes   
 DROP FUNCTION échoue si la base de données contient des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou des vues qui font référence à cette fonction et qui ont été créées au moyen de SCHEMABINDING. Elle échoue également s'il existe des colonnes calculées, des contraintes CHECK ou DEFAULT qui font référence à cette fonction.  
  
 DROP FUNCTION échoue si des colonnes calculées qui ont été indexées font référence à cette fonction.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter DROP FUNCTION, un utilisateur doit avoir au minimum l'autorisation ALTER sur le schéma auquel appartient la fonction ou l'autorisation CONTROL sur la fonction.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dropping-a-function"></a>A. Suppression d'une fonction  
 L’exemple suivant supprime la fonction définie par l’utilisateur `fn_SalesByStore` du schéma `Sales` de l’exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Pour créer cette fonction, consultez l’exemple B dans [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md).  
  
```  
DROP FUNCTION Sales.fn_SalesByStore;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)  
  
  
