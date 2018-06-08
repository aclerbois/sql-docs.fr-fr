---
title: ERROR_PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ERROR_PROCEDURE_TSQL
- ERROR_PROCEDURE
dev_langs:
- TSQL
helpviewer_keywords:
- ERROR_PROCEDURE function
- messages [SQL Server], trigger where occurred
- errors [SQL Server], stored procedure where occurred
- TRY...CATCH [SQL Server]
- CATCH block
- messages [SQL Server], stored procedure where occurred
- errors [SQL Server], trigger where occurred
ms.assetid: b81edbf0-856a-498f-ba87-48ff1426d980
caps.latest.revision: 44
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 051d120a972f681879a72f3a7f2e59fb2ae63adf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="errorprocedure-transact-sql"></a>ERROR_PROCEDURE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne le nom de la procédure stockée ou du déclencheur où une erreur ayant entraîné l'exécution du bloc CATCH d'une construction TRY…CATCH s'est produite.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ERROR_PROCEDURE ( )  
```  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar(128)**  
  
## <a name="return-value"></a>Valeur retournée  
 Lors de l'appel d'un bloc CATCH, retourne le nom de la procédure stockée où l'erreur s'est produite.  
  
 Retourne la valeur NULL si l'erreur ne s'est pas produite dans une procédure stockée ou un déclencheur.  
  
 Retourne NULL si l'appel a lieu en dehors de l'étendue d'un bloc CATCH.  
  
## <a name="remarks"></a>Notes   
 ERROR_PROCEDURE peut être appelé n'importe où dans l'étendue d'un bloc CATCH.  
  
 ERROR_PROCEDURE retourne le nom de la procédure stockée ou du déclencheur où l'erreur s'est produite, indépendamment du nombre de fois qu'il ou qu'elle a été appelé ou de l'origine de l'appel dans l'étendue du bloc CATCH. Ce comportement contraste avec celui des fonctions, telles que @@ERROR, qui renvoient le code d’erreur dans l’instruction suivant immédiatement celle ayant provoqué l’erreur ou dans la première instruction du bloc CATCH.  
  
 Dans les blocs CATCH imbriqués, ERROR_PROCEDURE retourne le nom de la procédure stockée ou du déclencheur propre à l'étendue du bloc CATCH dans lequel la procédure ou le déclencheur est référencé. Par exemple, le bloc CATCH d'une construction TRY…CATCH peut inclure un bloc TRY…CATCH imbriqué. Dans le bloc CATCH imbriqué, ERROR_PROCEDURE retourne le nom de la procédure stockée ou du déclencheur erroné qui a appelé le bloc CATCH imbriqué. Si ERROR_PROCEDURE s'exécute dans le bloc CATCH externe, l'instruction retourne le nom de la procédure stockée erronée qui a appelé ce bloc CATCH.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-errorprocedure-in-a-catch-block"></a>A. Utilisation d'ERROR_PROCEDURE dans un bloc CATCH  
 L'exemple de code ci-dessous illustre une procédure stockée générant une erreur de division par zéro. `ERROR_PROCEDURE` retourne le nom de la procédure stockée dans laquelle l'erreur s'est produite.  
  
```  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_PROCEDURE() AS ErrorProcedure;  
END CATCH;  
GO  
```  
  
### <a name="b-using-errorprocedure-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilisation d'ERROR_PROCEDURE dans un bloc CATCH avec d'autres outils de gestion des erreurs  
 L'exemple de code ci-dessous illustre une procédure stockée générant une erreur de division par zéro. En plus du nom de la procédure stockée erronée, des informations sur l'erreur sont aussi précisées.  
  
```  
  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_MESSAGE() AS ErrorMessage,  
        ERROR_LINE() AS ErrorLine;  
        END CATCH;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-errorprocedure-in-a-catch-block"></a>C. Utilisation d'ERROR_PROCEDURE dans un bloc CATCH  
 L'exemple de code ci-dessous illustre une procédure stockée générant une erreur de division par zéro. `ERROR_PROCEDURE` retourne le nom de la procédure stockée dans laquelle l'erreur s'est produite.  
  
```  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_PROCEDURE() AS ErrorProcedure;  
END CATCH;  
GO  
```  
  
### <a name="d-using-errorprocedure-in-a-catch-block-with-other-error-handling-tools"></a>D. Utilisation d'ERROR_PROCEDURE dans un bloc CATCH avec d'autres outils de gestion des erreurs  
 L'exemple de code ci-dessous illustre une procédure stockée générant une erreur de division par zéro. En plus du nom de la procédure stockée erronée, des informations sur l'erreur sont aussi précisées.  
  
```  
  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_MESSAGE() AS ErrorMessage;  
        END CATCH;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  

