---
title: CREATE SYNONYM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREATE_SYNONYM_TSQL
- SYNONYM_TSQL
- SYNONYM
- CREATE SYNONYM
dev_langs:
- TSQL
helpviewer_keywords:
- alternate names [SQL Server]
- names [SQL Server], synonyms
- CREATE SYNONYM statement
- synonyms [SQL Server], creating
ms.assetid: 41313809-e970-449c-bc35-85da2ef96e48
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 0ebe56d6cff65c579828f9cdce1a879317075d94
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-synonym-transact-sql"></a>CREATE SYNONYM (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Crée un synonyme.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- SQL Server Syntax  
  
CREATE SYNONYM [ schema_name_1. ] synonym_name FOR <object>  
  
<object> :: =  
{  
    [ server_name.[ database_name ] . [ schema_name_2 ]. object_name   
  | database_name . [ schema_name_2 ].| schema_name_2. ] object_name  
}  
```  
  
```  
-- Azure SQL Database Syntax  
  
CREATE SYNONYM [ schema_name_1. ] synonym_name FOR < object >  
  
< object > :: =  
{  
    [database_name. [ schema_name_2 ].| schema_name_2. ] object_name  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *schema_name_1*  
 Spécifie le schéma dans lequel est créé le synonyme. Si la valeur *schema* n’est pas spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise le schéma par défaut de l’utilisateur actuel.  
  
 *synonym_name*  
 Nom du nouveau synonyme.  
  
 *server_name*  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Nom du serveur contenant l'objet de base.  
  
 *database_name*  
 Nom de la base de données contenant l'objet de base. Si la valeur *database_name* n’est pas spécifiée, le nom de la base de données active est utilisé.  
  
 *schema_name_2*  
 Nom du schéma pour l'objet de base. Si la valeur *schema_name* n’est pas spécifiée, le schéma par défaut de l’utilisateur actuel est utilisé.  
  
 *object_name*  
 Nom de l'objet de base auquel fait référence le synonyme.  
  
 Microsoft Azure SQL Database prend en charge le format de nom en trois parties nom_bd.[nom_schéma].nom_objet lorsque nom_bd est la base de données active, ou lorsque nom_bd est la base de données tempdb et nom_objet commence par #.  
  
## <a name="remarks"></a>Notes   
 L'objet de base ne doit pas exister lors de la création du synonyme. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie l'existence de l'objet de base au cours de l'exécution.  
  
 Vous pouvez créer des synonymes pour les types d'objets suivants :  
  
|||  
|-|-|  
|Procédure stockée d’assembly (CLR)|Fonction table d’assembly (CLR)|  
|Fonction scalaire d’assembly (CLR)|Fonctions d’agrégation d’assembly (CLR)|  
|Procédure de réplication et de filtrage|Procédure stockée étendue|  
|Fonction scalaire SQL|Fonction table SQL|  
|Fonction table inline SQL|Procédure stockée SQL|  
|Affichage|Table<sup>1</sup> (définie par l’utilisateur)|  
  
 <sup>1 Inclut les tables temporaires locales et globales</sup>  
  
 Les noms en quatre parties des objets de base de fonction ne sont pas pris en charge.  
  
 Vous pouvez créer, supprimer et référencer des synonymes dans des instructions SQL dynamiques.  
  
## <a name="permissions"></a>Autorisations  
 Pour créer un synonyme dans un schéma donné, un utilisateur doit disposer de l'autorisation CREATE SYNONYM, et il doit posséder le schéma ou bénéficier de l'autorisation ALTER SCHEMA.  
  
 L'autorisation CREATE SYNONYM est octroyable.  
  
> [!NOTE]  
>  Vous n'avez pas besoin d'une autorisation sur un objet de base pour réussir une compilation de l'instruction CREATE SYNONYM, car toute vérification d'autorisation sur l'objet de base est reportée jusqu'à l'exécution.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-synonym-for-a-local-object"></a>A. Création d'un synonyme pour un objet local  
 L'exemple suivant crée d'abord un synonyme pour l'objet de base, `Product` dans la base de données `AdventureWorks2012`, puis il interroge le synonyme.  
  
```  
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
  
-- Query the Product table by using the synonym.  
SELECT ProductID, Name   
FROM MyProduct  
WHERE ProductID < 5;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ----------------------- 
 ProductID   Name 
 ----------- -------------------------- 
 1           Adjustable Race 
 2           Bearing Ball 
 3           BB Ball Bearing 
 4           Headset Ball Bearings 

 (4 row(s) affected)
``` 
  
### <a name="b-creating-a-synonym-to-remote-object"></a>B. Création d'un synonyme pour un objet distant  
 Dans l'exemple suivant, l'objet de base `Contact` se trouve sur un serveur distant appelé `Server_Remote`.  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
EXEC sp_addlinkedserver Server_Remote;  
GO  
USE tempdb;  
GO  
CREATE SYNONYM MyEmployee FOR Server_Remote.AdventureWorks2012.HumanResources.Employee;  
GO  
```  
  
### <a name="c-creating-a-synonym-for-a-user-defined-function"></a>C. Création d'un synonyme pour une fonction définie par l'utilisateur  
 L'exemple suivant crée une fonction nommée `dbo.OrderDozen` qui augmente des quantités de commandes pour obtenir des douzaines d'unités exactement. L'exemple crée ensuite le synonyme `dbo.CorrectOrder` pour la fonction `dbo.OrderDozen`.  
  
```  
-- Creating the dbo.OrderDozen function  
CREATE FUNCTION dbo.OrderDozen (@OrderAmt int)  
RETURNS int  
WITH EXECUTE AS CALLER  
AS  
BEGIN  
IF @OrderAmt % 12 <> 0  
BEGIN  
    SET @OrderAmt +=  12 - (@OrderAmt % 12)  
END  
RETURN(@OrderAmt);  
END;  
GO  
  
-- Using the dbo.OrderDozen function  
DECLARE @Amt int;  
SET @Amt = 15;  
SELECT @Amt AS OriginalOrder, dbo.OrderDozen(@Amt) AS ModifiedOrder;  
  
-- Create a synonym dbo.CorrectOrder for the dbo.OrderDozen function.  
CREATE SYNONYM dbo.CorrectOrder  
FOR dbo.OrderDozen;  
GO  
  
-- Using the dbo.CorrectOrder synonym.  
DECLARE @Amt int;  
SET @Amt = 15;  
SELECT @Amt AS OriginalOrder, dbo.CorrectOrder(@Amt) AS ModifiedOrder;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [DROP SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/drop-synonym-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
