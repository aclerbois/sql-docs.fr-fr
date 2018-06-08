---
title: ERROR_NUMBER (Transact-SQL) | Microsoft Docs
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
- ERROR_NUMBER_TSQL
- ERROR_NUMBER
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], line number
- messages [SQL Server], numbers
- TRY...CATCH [SQL Server]
- ERROR_NUMBER function
- CATCH block
ms.assetid: 1de85fff-1ca2-4b31-841b-926e571cb150
caps.latest.revision: 50
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 45ad8be1da81b29b446ed18dd0c4688013a18875
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="errornumber-transact-sql"></a>ERROR_NUMBER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie le numéro d'erreur de l'erreur qui a causé l'exécution du bloc CATCH d'une construction TRY…CATCH.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ERROR_NUMBER ( )  
```  
  
## <a name="return-types"></a>Types de retour  
 **Int**  
  
## <a name="return-value"></a>Valeur retournée  
 Lorsqu'il est appelé dans un bloc CATCH, renvoie le numéro d'erreur de l'erreur qui a causé l'exécution du bloc CATCH.  
  
 Retourne NULL si l'appel a lieu en dehors de l'étendue d'un bloc CATCH.  
  
## <a name="remarks"></a>Notes   
 Cette fonction peut être appelée n'importe où dans l'étendue d'un bloc CATCH.  
  
 ERROR_NUMBER renvoie le numéro d'erreur, quel que soit le nombre d'exécutions de la fonction, ou l'endroit où elle est exécutée au sein de l'étendue du bloc CATCH. Au contraire, @@ERROR ne renvoie que le numéro d’erreur dans l’instruction immédiatement après celle qui a causé une erreur, ou la première instruction d’un bloc CATCH.  
  
 Dans des blocs CATCH imbriqués, ERROR_NUMBER renvoie le numéro d'erreur spécifique à l'étendue du bloc CATCH dans lequel il est référencé. Par exemple, le bloc CATCH d'une construction TRY...CATCH externe peut comporter une construction TRY...CATCH imbriquée. Avec le bloc CATCH imbriqué, ERROR_NUMBER renvoie le numéro de l'erreur qui a invoqué le bloc CATCH imbriqué. Si ERROR_NUMBER est exécuté dans le bloc CATCH externe, il renvoie le numéro de l'erreur qui a invoqué le bloc CATCH imbriqué.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-errornumber-in-a-catch-block"></a>A. Utilisation de ERROR_NUMBER dans un bloc CATCH  
 L'exemple de code suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Le numéro de l'erreur est renvoyé.  
  
```  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_NUMBER() AS ErrorNumber;  
END CATCH;  
GO  
```  
  
### <a name="b-using-errornumber-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilisation de ERROR_NUMBER dans un bloc CATCH avec d'autres outils de traitement des erreurs  
 L'exemple de code suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Outre le numéro de l'erreur, des informations relatives à l'erreur sont également renvoyées.  
  
```  
  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-errornumber-in-a-catch-block-with-other-error-handling-tools"></a>C. Utilisation de ERROR_NUMBER dans un bloc CATCH avec d'autres outils de traitement des erreurs  
 L'exemple de code suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Outre le numéro de l'erreur, des informations relatives à l'erreur sont également renvoyées.  
  
```  
  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
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
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  

