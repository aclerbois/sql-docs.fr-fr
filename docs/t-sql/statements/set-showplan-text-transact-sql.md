---
title: SET SHOWPLAN_TEXT (Transact-SQL) | Microsoft Docs
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
f1_keywords:
- SHOWPLAN_TEXT
- SET_SHOWPLAN_TEXT_TSQL
- SET SHOWPLAN_TEXT
- SHOWPLAN_TEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- SET SHOWPLAN_TEXT statement
- canceling statement execution
- SHOWPLAN_TEXT option
- stopping statement execution
- estimated execution information [SQL Server]
ms.assetid: 2c4f3fc8-ff2c-4790-8b74-e7e8ef58f9a6
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 11bdddd82efcbc4f0e97b8b33d99961caa9dcfd5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-showplantext-transact-sql"></a>SET SHOWPLAN_TEXT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Empêche Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'exécuter des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Au lieu de cela, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne des informations détaillées sur l'exécution des instructions.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET SHOWPLAN_TEXT { ON | OFF }  
```  
  
## <a name="remarks"></a>Notes   
 La définition de SET SHOWPLAN_TEXT s'effectue au moment de l'exécution, et non pas durant l'analyse.  
  
 Si SET SHOWPLAN_TEXT est activé (ON), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne des informations sur l'exécution de chaque instruction [!INCLUDE[tsql](../../includes/tsql-md.md)], sans toutefois l'exécuter. Une fois cette option activée, des informations sur le plan d'exécution de toutes les instructions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivantes sont retournées jusqu'à sa désactivation (OFF). Si, par exemple, une instruction CREATE TABLE est exécutée alors que l'option SET SHOWPLAN_TEXT est activée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un message d'erreur d'une instruction SELECT ultérieure se rapportant à cette même table, lequel informe l'utilisateur qu'elle n'existe pas. Par conséquent, les prochaines références à cette table échoueront. Lorsque l'option SET SHOWPLAN_TEXT est désactivée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute les instructions sans créer de rapport contenant des informations sur le plan d'exécution.  
  
 L’option SET SHOWPLAN_TEXT est conçue pour retourner des résultats pouvant être interprétés par des applications d’invite de commandes Microsoft Win32, par exemple l’utilitaire **osql**. SET SHOWPLAN_ALL retourne des résultats plus détaillés, destinés à être utilisés par des programmes capables de gérer sa sortie.  
  
 SET SHOWPLAN_TEXT et SET SHOWPLAN_ALL ne peuvent pas être spécifiés dans une procédure stockée. Elles doivent être les seules instructions d'un traitement.  
  
 SET SHOWPLAN_TEXT retourne les informations sous la forme d'un ensemble de lignes formant une arborescence hiérarchique, qui représente les étapes effectuées par le processeur de requêtes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à mesure qu'il exécute chaque instruction. Chaque instruction reflétée dans la sortie contient une ligne unique comportant le texte de l'instruction, suivie de plusieurs lignes contenant les détails des étapes d'exécution. Le tableau suivant montre la colonne figurant dans la sortie.  
  
|Nom de colonne|Description|  
|-----------------|-----------------|  
|**StmtText**|Pour les lignes qui ne sont pas du type PLAN_ROW, cette colonne contient le texte de l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour les lignes de type PLAN_ROW, cette colonne contient une description de l'opération. Cette colonne contient l'opérateur physique et peut, en option, contenir l'opérateur logique. Elle peut également être suivie d’une description qui est déterminée par l’opérateur physique. Pour plus d’informations sur les opérateurs physiques, consultez la colonne **Argument** dans [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md).|  
  
 Pour plus d’informations sur les opérateurs physiques et logiques affichés dans des résultats de plan de requêtes, consultez [Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  
  
## <a name="permissions"></a>Autorisations  
 Pour utiliser SET SHOWPLAN_TEXT, vous devez disposer des autorisations requises pour exécuter les instructions sur lesquelles porte SET SHOWPLAN_TEXT et vous devez avoir l'autorisation SHOWPLAN pour toutes les bases de données qui contiennent les objets référencés.  
  
 Concernant les instructions SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* et EXEC *user_defined_function*, pour produire une sortie Showplan, l’utilisateur doit :  
  
-   disposer des autorisations appropriées pour exécuter les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ;  
  
-   Disposer de l'autorisation SHOWPLAN sur toutes les bases de données contenant les objets référencés par les instructions Transact-SQL, par exemple des tables, des vues, etc.  
  
 Pour toutes les autres instructions, notamment DDL, USE *database_name*, SET, DECLARE, SQL dynamique, etc., seules les autorisations appropriées pour exécuter les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont nécessaires.  
  
## <a name="examples"></a>Exemples  
 Cet exemple montre comment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les index durant le traitement d'une instruction.  
  
 Requête utilisant un index :  
  
```  
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
SELECT *  
FROM Production.Product   
WHERE ProductID = 905;  
GO  
SET SHOWPLAN_TEXT OFF;  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
StmtText                                             
---------------------------------------------------  
SELECT *  
FROM Production.Product   
WHERE ProductID = 905;   
  
StmtText                                                                                                                                                                                        
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
|--Clustered Index Seek(OBJECT:([AdventureWorks2012].[Production].[Product].[PK_Product_ProductID]), SEEK:([AdventureWorks2012].[Production].[Product].[ProductID]=CONVERT_IMPLICIT(int,[@1],0)) ORDERED FORWARD)   
```  
  
 Requête n'utilisant pas d'index :  
  
```  
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
SELECT *  
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET SHOWPLAN_TEXT OFF;  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
StmtText                                                                  
------------------------------------------------------------------------  
SELECT *  
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;   
  
StmtText                                                                                                                                                                                                  
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
|--Clustered Index Scan(OBJECT:([AdventureWorks2012].[Production].[ProductCostHistory].[PK_ProductCostHistory_ProductCostID]), WHERE:([AdventureWorks2012].[Production].[ProductCostHistory].[StandardCost]<[@1]))  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)  
  
  
