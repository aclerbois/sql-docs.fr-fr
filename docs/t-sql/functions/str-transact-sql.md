---
title: STR (Transact-SQL) | Microsoft Docs
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
- STR
- STR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- converting numbers to characters
- characters [SQL Server], converting
- character data [SQL Server]
- STR function
ms.assetid: de03531b-d9e7-4c3c-9604-14e582ac20c6
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7fbc48d8cdccfb7e24d70f2f9da718f782686fa5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="str-transact-sql"></a>STR (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie des données de type caractère converties à partir de données numériques.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
STR ( float_expression [ , length [ , decimal ] ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *float_expression*  
 Expression de type de données numérique approché (**float**) avec une virgule décimale.  
  
 *length*  
 Longueur totale. Inclut la virgule décimale, le signe, les chiffres et les espaces. La valeur par défaut est 10.  
  
 *decimal*  
 Représente le nombre de décimales à droite de la virgule décimale. La valeur *decimal* doit être inférieure ou égale à 16. Si la valeur *decimal* est supérieure à 16, le résultat est tronqué à seize chiffres à droite de la virgule décimale.  
  
## <a name="return-types"></a>Types de retour  
 **varchar**  
  
## <a name="remarks"></a>Notes   
 Si elles sont spécifiées, les valeurs des paramètres *length* et *decimal* dans STR doivent être positives. Le nombre est arrondi par défaut à un entier ou si le paramètre décimal est 0. La longueur spécifiée doit être supérieure ou égale à la partie du nombre située avant la virgule décimale plus le signe du nombre (le cas échéant). Un argument *float_expression* court est justifié à droite selon la longueur spécifiée, et un argument *float_expression* long est tronqué au nombre spécifié de décimales. Par exemple, STR(12 **,** 10) donne 12 comme résultat. Il est justifié à droite dans le jeu de résultats. Toutefois, STR(1223 **,** 2) tronque le jeu de résultats à **. Il est possible d'imbriquer des fonctions de chaîne.  
  
> [!NOTE]  
>  Pour convertir en données Unicode, utilisez STR dans une fonction de conversion CONVERT ou [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant convertit une expression constituée de cinq chiffres et d'une virgule décimale en chaîne de caractères à six positions. La partie fractionnaire du nombre est arrondie à une décimale.  
  
```  
SELECT STR(123.45, 6, 1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------  
 123.5  
  
(1 row(s) affected)  
```  
  
 Lorsque l'expression a une longueur supérieure à la longueur spécifiée, la chaîne renvoie `**` pour la longueur spécifiée.  
  
```  
SELECT STR(123.45, 2, 2);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--  
**  
  
(1 row(s) affected)  
```  
  
 Même lorsque des données numériques sont imbriquées dans `STR`, le résultat correspond à des données de type caractère avec le format spécifié.  
  
```  
SELECT STR (FLOOR (123.45), 8, 3;)  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------  
 123.000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
 [FORMAT &#40;Transact-SQL&#41;](../../t-sql/functions/format-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

