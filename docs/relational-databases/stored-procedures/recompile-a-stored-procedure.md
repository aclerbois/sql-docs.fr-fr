---
title: Recompiler une procédure stockée | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-stored-Procs
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- sp_recompile
- WITH RECOMPILE clause
- recompiling stored procedures
- stored procedures [SQL Server], recompiling
ms.assetid: b90deb27-0099-4fe7-ba60-726af78f7c18
caps.latest.revision: 37
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6314e9f1659d73fe2e4cb7061377e9d4b21f023c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="recompile-a-stored-procedure"></a>Recompiler une procédure stockée
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Cette rubrique explique comment recompiler une procédure stockée dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il existe trois manières d’effectuer cette opération : via l’option **WITH RECOMPILE** dans la définition de la procédure ou lorsque la procédure est appelée, via l’indicateur de requête **RECOMPILE** sur des instructions ou en utilisant la procédure stockée système **sp_recompile** . Cette rubrique décrit l’utilisation de l’option WITH RECOMPILE lors de la création d’une définition de procédure et de l’exécution d’une procédure existante. Elle décrit également l’utilisation de la procédure stockée système sp_recompile pour recompiler une procédure existante.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour recompiler une procédure stockée à l'aide de :**  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Lorsqu'une procédure est compilée pour la première fois ou recompilée, son plan de requête est optimisé pour l'état actuel de la base de données et de ses objets. Si une base de données subit des modifications significatives au niveau de ses données ou de sa structure, le fait de recompiler la procédure met à jour et optimise son plan de requête en fonction ces modifications. Cela peut améliorer les performances de traitement de la procédure.  
  
-   Parfois, la recompilation de procédure doit être forcée, et parfois elle se produit automatiquement. La recompilation automatique se produit chaque fois que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] redémarre. Elle se produit également si une table sous-jacente référencée par la procédure a été modifiée au niveau de sa conception physique.  
  
-   Une autre raison pour forcer la recompilation d'une procédure est de contrer le comportement de détection des paramètres de la compilation de la procédure. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute des procédures, les valeurs des paramètres utilisés par la procédure lors de sa compilation sont incluses dans le cadre de la génération du plan de requête. Si ces valeurs représentent les valeurs standard utilisées pour appeler ultérieurement la procédure, celle-ci bénéficie du plan de requête à chaque compilation et exécution. Si les valeurs des paramètres de la procédure sont atypiques, l'application forcée d'une recompilation et un nouveau plan défini en fonction des valeurs des paramètres peuvent améliorer les performances.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet la recompilation des procédures au niveau de l’instruction. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recompile des procédures, seule l'instruction ayant provoqué la recompilation est compilée, et non la procédure toute entière.  
  
-   Si certaines requêtes d'une procédure utilisent régulièrement des valeurs atypiques ou temporaires, les performances des procédures peuvent être améliorées en utilisant l'indicateur de requête RECOMPILE à l'intérieur de ces requêtes. Étant donné que seules les requêtes utilisant l'indicateur de requête seront recompilées au lieu de la procédure complète, le comportement de recompilation de l'instruction de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]est reproduit. Cependant, en plus d'utiliser les valeurs des paramètres actuels de la procédure, l'indicateur de requête RECOMPILE utilise également les valeurs des variables locales à l'intérieur de la procédure stockée lorsque vous compilez l'instruction. Pour plus d’informations, consultez [Indicateur de requête (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 **WITH RECOMPILE**   
 Si cette option est utilisée lorsque la définition de la procédure est créée, elle nécessite l'autorisation CREATE PROCEDURE dans la base de données et l'autorisation ALTER sur le schéma dans lequel la procédure est créée.  
  
 Si cette option est utilisée dans une instruction EXECUTE, elle nécessite des autorisations EXECUTE sur la procédure. Aucune autorisation n'est requise sur l'instruction EXECUTE elle-même, mais une autorisation est requise sur la procédure référencée dans l'instruction EXECUTE. Pour plus d’informations, consultez [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Indicateur de requête **RECOMPILE**  
 Cette fonctionnalité est utilisée lorsque la procédure est créée et l’indicateur est inclus dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de la procédure. Par conséquent, elle nécessite l'autorisation CREATE PROCEDURE dans la base de données et l'autorisation ALTER sur le schéma dans lequel la procédure est créée.  
  
 Procédure stockée système**sp_recompile**   
 Nécessite l'autorisation ALTER pour la procédure spécifiée.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-recompile-a-stored-procedure-by-using-the-with-recompile-option"></a>Pour recompiler une procédure stockée à l'aide de l'option WITH RECOMPILE  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple crée la définition de la procédure.  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'dbo.uspProductByVendor', 'P' ) IS NOT NULL   
    DROP PROCEDURE dbo.uspProductByVendor;  
GO  
CREATE PROCEDURE dbo.uspProductByVendor @Name varchar(30) = '%'  
WITH RECOMPILE  
AS  
    SET NOCOUNT ON;  
    SELECT v.Name AS 'Vendor name', p.Name AS 'Product name'  
    FROM Purchasing.Vendor AS v   
    JOIN Purchasing.ProductVendor AS pv   
      ON v.BusinessEntityID = pv.BusinessEntityID   
    JOIN Production.Product AS p   
      ON pv.ProductID = p.ProductID  
    WHERE v.Name LIKE @Name;  
  
```  
  
#### <a name="to-recompile-a-stored-procedure-by-using-the-with-recompile-option"></a>Pour recompiler une procédure stockée à l'aide de l'option WITH RECOMPILE  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple crée une procédure simple qui retourne tous les employés (prénom et nom), leur poste et le nom de leur service à partir d'une vue.  
  
     Puis, copiez et collez le second exemple de code dans la fenêtre de requête et cliquez sur **Exécuter**. Cela exécute la procédure et recompile son plan de requête.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXECUTE HumanResources.uspGetAllEmployees WITH RECOMPILE;  
GO  
  
```  
  
#### <a name="to-recompile-a-stored-procedure-by-using-sprecompile"></a>Pour recompiler une procédure stockée à l'aide de sp_recompile  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple crée une procédure simple qui retourne tous les employés (prénom et nom), leur poste et le nom de leur service à partir d'une vue.  
  
     Ensuite, copiez et collez l'exemple suivant dans la fenêtre de requête et cliquez sur **Exécuter**. Cela n'exécute pas la procédure mais la marque pour la recompilation de sorte que son plan de requête sera mis à jour à la prochaine exécution.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_recompile N'HumanResources.uspGetAllEmployees';  
GO  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modifier une procédure stockée](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Renommer une procédure stockée](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [Afficher la définition d'une procédure stockée](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [Afficher les dépendances d'une procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)  
  
  
