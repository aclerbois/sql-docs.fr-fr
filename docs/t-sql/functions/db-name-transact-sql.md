---
title: DB_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DB_NAME
- DB_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database names [SQL Server], DB_NAME
- names [SQL Server], databases
- viewing database names
- displaying database names
- DB_NAME function
ms.assetid: e21fb33a-a3ea-49b0-bb6b-8f789a675a0e
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1c2a2e92fdef5cae1b2404d18c2c5fb18b3de1ba
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dbname-transact-sql"></a>DB_NAME (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Retourne le nom de la base de données.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DB_NAME ( [ database_id ] )  
```  
  
## <a name="arguments"></a>Arguments  
*database_id*  
Numéro d'identification de la base de données à retourner. *database_id* est de type **int**, sans valeur par défaut. Si l'ID n'est pas spécifié, la fonction renvoie le nom de la base de données active.
  
## <a name="return-types"></a>Types de retour
**nvarchar(128)**
  
## <a name="permissions"></a>Autorisations  
Si l’appelant de **DB_NAME** n’est pas le propriétaire de la base de données et que la base de données n’est pas de type **master** ni **tempdb**, les autorisations minimales requises pour consulter la ligne correspondante sont les autorisations ALTER ANY DATABASE ou VIEW ANY DATABASE au niveau du serveur, ou encore l’autorisation CREATE DATABASE dans la base de données **master**. La base de données à laquelle l'appelant est connecté peut toujours être vue dans **sys.databases**.
  
> [!IMPORTANT]  
>  Par défaut, le rôle public a l’autorisation VIEW ANY DATABASE, qui permet à toutes les connexions de consulter les informations de base de données. Pour empêcher une connexion de détecter une base de données, révoquez l’autorisation publique VIEW ANY DATABASE ou refusez l’autorisation VIEW ANY DATABASE pour les connexions individuelles.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-current-database-name"></a>A. Renvoi du nom de la base de données active  
L'exemple suivant renvoie le nom de la base de données active.
  
```sql
SELECT DB_NAME() AS [Current Database];  
GO  
```  
  
### <a name="b-returning-the-database-name-of-a-specified-database-id"></a>B. Renvoi du nom de la base de données correspondant à un ID de base de données spécifié.  
L'exemple suivant renvoie le nom de la base de données ayant l'ID `3`.
  
```sql
USE master;  
GO  
SELECT DB_NAME(3)AS [Database Name];  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-return-the-current-database-name"></a>C. Renvoi du nom de la base de données active  
  
```sql
SELECT DB_NAME() AS [Current Database];  
```  
  
### <a name="d-return-the-name-of-a-database-by-using-the-database-id"></a>D. Renvoi du nom d’une base de données à l’aide de l’ID de base de données  
L’exemple suivant retourne le nom de la base de données et database_id pour chaque base de données.
  
```sql
SELECT DB_NAME(database_id) AS [Database], database_id  
FROM sys.databases;  
```  
  
## <a name="see-also"></a>Voir aussi
[DB_ID &#40;Transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md)  
[Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
  
  

