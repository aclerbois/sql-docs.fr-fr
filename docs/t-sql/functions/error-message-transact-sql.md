---
title: ERROR_MESSAGE (Transact-SQL) | Microsoft Docs
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
- ERROR_MESSAGE_TSQL
- ERROR_MESSAGE
dev_langs:
- TSQL
helpviewer_keywords:
- ERROR_MESSAGE function
- errors [SQL Server], text of
- messages [SQL Server], text of
- TRY...CATCH [SQL Server]
- CATCH block
ms.assetid: f32877a6-5f17-418c-a32c-5a1a344b3c45
caps.latest.revision: 53
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 68439f7a9ea22078660d6e0465961c052002e8be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="errormessage-transact-sql"></a>ERROR_MESSAGE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne le texte du message de l'erreur qui a provoqué l'exécution du bloc CATCH d'une construction TRY…CATCH.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ERROR_MESSAGE ( )   
```  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar(4000)**  
  
## <a name="return-value"></a>Valeur retournée  
 Appelé dans un bloc CATCH, retourne le texte complet du message d'erreur qui a provoqué l'exécution du bloc CATCH. Le texte comprend les valeurs fournies pour tous les paramètres remplaçables, tels que les longueurs, les noms d'objet ou les heures.  
  
 Retourne NULL si l'appel a lieu en dehors de l'étendue d'un bloc CATCH.  
  
## <a name="remarks"></a>Notes   
 ERROR_MESSAGE peut être appelé partout dans l'étendue d'un bloc CATCH.  
  
 ERROR_MESSAGE retourne le message d'erreur indépendamment du nombre de fois ou de l'emplacement où il est exécuté dans l'étendue du bloc CATCH. Il s’agit là d’une différence avec des fonctions telles que @@ERROR, qui retourne seulement le numéro de l’erreur dans l’instruction immédiatement après celle qui provoque une erreur ou bien la première instruction d’un bloc CATCH.  
  
 Dans les blocs CATCH imbriqués, ERROR_MESSAGE retourne le message d'erreur spécifique à l'étendue du bloc CATCH dans lequel il est référencé. Par exemple, le bloc CATCH d'une construction TRY...CATCH externe peut comporter une construction TRY...CATCH imbriquée. Dans le bloc CATCH imbriqué, ERROR_MESSAGE retourne le message de l'erreur qui a appelé le bloc CATCH imbriqué. Si ERROR_MESSAGE est exécuté dans le bloc CATCH externe, il retourne le message de l'erreur qui a appelé ce bloc CATCH.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-errormessage-in-a-catch-block"></a>A. Utilisation de ERROR_MESSAGE dans un bloc CATCH  
 L'exemple de code suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Le message de l'erreur est retourné.  
  
```  
  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
### <a name="b-using-errormessage-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilisation de ERROR_MESSAGE dans un bloc CATCH avec d'autres outils de traitement des erreurs  
 L'exemple de code suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Des informations relatives à l'erreur sont retournées en même temps que le message d'erreur.  
  
```  
  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-errormessage-in-a-catch-block-with-other-error-handling-tools"></a>C. Utilisation de ERROR_MESSAGE dans un bloc CATCH avec d'autres outils de traitement des erreurs  
 L'exemple de code suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Des informations relatives à l'erreur sont retournées en même temps que le message d'erreur.  
  
```  
  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  

