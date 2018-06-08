---
title: ERROR_STATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ERROR_STATE_TSQL
- ERROR_STATE
dev_langs:
- TSQL
helpviewer_keywords:
- messages [SQL Server], state
- ERROR_STATE function
- errors [SQL Server], state
- TRY...CATCH [SQL Server]
- CATCH block
- states [SQL Server], error numbers
ms.assetid: 6059af00-83fe-409f-ab7c-daad111bc671
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5ec35734716a2808e3b8450588498caf8a610e8d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="errorstate-transact-sql"></a>ERROR_STATE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

  Renvoie le numéro d'état de l'erreur qui a engendré l'exécution du bloc CATCH d'une construction TRY…CATCH.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ERROR_STATE ( )  
```  
  
## <a name="return-types"></a>Types de retour  
 **Int**  
  
## <a name="return-value"></a>Valeur retournée  
 Lorsqu'elle est appelée dans un bloc CATCH, elle renvoie le numéro d'état du message d'erreur à l'origine de l'exécution du bloc CATCH.  
  
 Retourne NULL si l'appel a lieu en dehors de l'étendue d'un bloc CATCH.  
  
## <a name="remarks"></a>Notes   
 Certains messages d’erreur peuvent apparaître en de multiples endroits du code du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. L'erreur « 1105 » par exemple, peut être provoquée par différentes conditions. Chaque condition qui génère l'erreur attribue un code d'état unique.  
  
 Lorsque vous consultez des bases de données de problèmes connus, telles que la Base de connaissances [!INCLUDE[msCoName](../../includes/msconame-md.md)], vous pouvez utiliser le numéro d'état pour voir si le problème enregistré est identique à l'erreur que vous avez rencontrée. Par exemple, si un article de la Base de connaissances traite de l'erreur 1105 avec un état 2 et que l'erreur 1105 reçue était associée à un état 3, votre erreur avait probablement une autre cause que celle mentionnée dans l'article.  
  
 Un ingénieur technique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut également utiliser le code d'état pour déterminer l'emplacement du code d'erreur dans le code source et réaliser ainsi plus facilement un diagnostic.  
  
 ERROR_STATE peut être appelée depuis n'importe quel emplacement dans le champ d'un bloc CATCH.  
  
 ERROR_STATE renvoie l'état de l'erreur, indépendamment du nombre d'exécutions ou de l'endroit où elle est exécutée dans le champ du bloc CATCH. En revanche, des fonctions telles que @@ERROR renvoient uniquement le numéro d’erreur dans l’instruction qui suit celle à l’origine d’une erreur ou dans la première instruction d’un bloc CATCH.  
  
 Dans les blocs CATCH imbriqués, ERROR_STATE renvoie l'état d'erreur propre au champ du bloc CATCH dans lequel elle est référencée. Par exemple, le bloc CATCH d'une construction TRY...CATCH externe peut comporter une construction TRY...CATCH imbriquée. Dans un bloc CATCH imbriqué, ERROR_STATE renvoie l'état depuis l'erreur qui a appelé ce bloc. Si ERROR_STATE est exécutée dans le bloc CATCH externe, elle renvoie l'état à partir de l'erreur qui a appelé ce bloc CATCH.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-errorstate-in-a-catch-block"></a>A. Utilisation d'ERROR_STATE dans un bloc CATCH  
 L’exemple suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. L'état de l'erreur est renvoyé.  
  
```  
BEGIN TRY  
    -- Generate a divide by zero error  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_STATE() AS ErrorState;  
END CATCH;  
GO  
```  
  
### <a name="b-using-errorstate-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilisation d'ERROR_STATE dans un bloc CATCH avec d'autres outils de gestion des erreurs  
 L’exemple suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Outre l'état de l'erreur, la procédure renvoie également les informations relatives à l'erreur.  
  
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
  
### <a name="c-using-errorstate-in-a-catch-block-with-other-error-handling-tools"></a>C. Utilisation d'ERROR_STATE dans un bloc CATCH avec d'autres outils de gestion des erreurs  
 L’exemple suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Outre l'état de l'erreur, la procédure renvoie également les informations relatives à l'erreur.  
  
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
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  

