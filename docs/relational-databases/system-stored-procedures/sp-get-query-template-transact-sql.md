---
title: sp_get_query_template (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_get_query_template
- sp_get_query_template_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_get_query_template
ms.assetid: 85e9bef7-2417-41a8-befa-fe75507d9bf2
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3fac47c5b84894f681ffc9c6729dd526f9e8488c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spgetquerytemplate-transact-sql"></a>sp_get_query_template (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne la forme paramétrable d'une requête. Les résultats retournés simulent la forme paramétrable d'une requête obtenue à l'aide d'un paramétrage forcé. sp_get_query_template sert essentiellement à créer des repères de plan TEMPLATE.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_get_query_template  
   [ @querytext = ] N'query_text'  
   , @templatetext OUTPUT   
   , @parameters OUTPUT   
```  
  
## <a name="arguments"></a>Arguments  
 '*query_text*'  
 Requête dont la version paramétrable doit être générée. '*query_text*' doit être placée entre guillemets simples et précédé du spécificateur Unicode N. N'*query_text*» est la valeur affectée à la @querytext paramètre. Il s’agit du type **nvarchar (max)**.  
  
 @templatetext  
 Est un paramètre output de type **nvarchar (max)**, fourni tel qu’indiqué, permettant de recevoir la forme paramétrable de *query_text* comme un littéral de chaîne.  
  
 @parameters  
 Est un paramètre output de type **nvarchar (max)**, fourni tel qu’indiqué, permettant de recevoir des données et les noms de types de paramètres qui ont été paramétrés dans un littéral de chaîne @templatetext.  
  
## <a name="remarks"></a>Notes  
 sp_get_query_template retourne une erreur dans les cas suivants :  
  
-   Il ne paramétrez pas toutes les valeurs littérales constantes dans *query_text*.  
  
-   *query_text* est NULL, pas une chaîne Unicode, syntaxe n’est pas valide ou ne peut pas être compilé.  
  
 Si sp_get_query_template retourne une erreur, il ne modifie pas les valeurs de la @templatetext et @parameters paramètres de sortie.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne la forme paramétrable d'une requête qui contient deux valeurs littérales de constante.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @my_templatetext nvarchar(max)  
DECLARE @my_parameters nvarchar(max)  
EXEC sp_get_query_template   
    N'SELECT pi.ProductID, SUM(pi.Quantity) AS Total  
        FROM Production.ProductModel pm   
        INNER JOIN Production.ProductInventory pi  
        ON pm.ProductModelID = pi.ProductID  
        WHERE pi.ProductID = 2  
        GROUP BY pi.ProductID, pi.Quantity  
        HAVING SUM(pi.Quantity) > 400',  
@my_templatetext OUTPUT,  
@my_parameters OUTPUT;  
SELECT @my_templatetext;  
SELECT @my_parameters;  
```  
  
 Voici les résultats paramétrés du paramètre `@my_templatetext``OUTPUT` :  
  
 `select pi . ProductID , SUM ( pi . Quantity ) as Total`  
  
 `from Production . ProductModel pm`  
  
 `inner join Production . ProductInventory pi`  
  
 `on pm . ProductModelID = pi . ProductID`  
  
 `where pi . ProductID = @0`  
  
 `group by pi . ProductID , pi . Quantity`  
  
 `having SUM ( pi . Quantity ) > 400`  
  
 Le premier littéral de constante, `2`, est converti en un paramètre. Le second littéral, `400`, n'est pas converti car il se trouve dans une clause `HAVING`. Les résultats retournés par sp_get_query_template simulent la forme paramétrable d'une requête lorsque l'option PARAMETERIZATION de ALTER DATABASE a pour valeur FORCED.  
  
 Voici les résultats paramétrés du paramètre `@my_parameters OUTPUT` :  
  
```  
@0 int  
```  
  
> [!NOTE]  
>  L'ordre et la dénomination des paramètres dans la sortie de sp_get_query_template peuvent varier suivant les correctifs QFE, les Service Packs et les mises à niveau de version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les mises à niveau peuvent également provoquer le paramétrage d'un ensemble différent de littéraux de constante pour la même requête et l'application d'un espacement différent dans les résultats de deux paramètres de sortie.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Spécifier le comportement du paramétrage de requêtes grâce aux repères de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)  
  
  
