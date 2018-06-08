---
title: Opérateurs de comparaison (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], testing
- operators [Transact-SQL], comparison
- testing expressions
- Boolean data type
- Boolean expressions
- comparing expressions
- comparison operators [SQL Server]
ms.assetid: b0cc68ef-3029-484c-a917-0c15dcbc230d
caps.latest.revision: 35
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5e0e5538d69caf96e7eb8864de177d14e99ce642
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="comparison-operators-transact-sql"></a>Opérateurs de comparaison (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Les opérateurs de comparaison testent si deux expressions sont identiques. Ils peuvent s’utiliser sur toutes les expressions, à l’exception des expressions des types de données **text**, **ntext** ou **image**. Le tableau suivant répertorie les opérateurs de comparaison [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
|Opérateur|Signification|  
|--------------|-------------|  
|[= (Égal à)](../../t-sql/language-elements/equals-transact-sql.md)|Égal à|  
|[> (Supérieur à)](../../t-sql/language-elements/greater-than-transact-sql.md)|Supérieur à|  
|[< (Inférieur à)](../../t-sql/language-elements/less-than-transact-sql.md)|Inférieur à|  
|[>= (Supérieur ou égal à)](../../t-sql/language-elements/greater-than-or-equal-to-transact-sql.md)|Supérieur ou égal à|  
|[<= (Inférieur ou égal à)](../../t-sql/language-elements/less-than-or-equal-to-transact-sql.md)|Inférieur ou égal à|  
|[<> (Différent de)](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)|Différent de|  
|[\!= (Différent de)](../../t-sql/language-elements/not-equal-to-transact-sql-exclamation.md)|Différent de (hors norme ISO)|  
|[\!< (Non inférieur à)](../../t-sql/language-elements/not-less-than-transact-sql.md)|Non inférieur à (hors norme ISO)|  
|[\!> (Non supérieur à)](../../t-sql/language-elements/not-greater-than-transact-sql.md)|Non supérieur à (hors norme ISO)|  
  
## <a name="boolean-data-type"></a>Type de données Boolean  
 Le résultat d’un opérateur de comparaison est de type **booléen**. et peut prendre trois valeurs : TRUE, FALSE et UNKNOWN. Les expressions qui retournent un type de données **booléen** sont dites expressions booléennes.  
  
 À la différence des autres types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le type de données **booléen** ne peut pas être spécifié pour une colonne de table ou une variable et il ne peut pas être retourné dans un jeu de résultats.  
  
 Lorsque SET ANSI_NULLS est activé (ON), un opérateur ayant une ou deux expressions de valeur NULL retourne UNKNOWN. Si SET ANSI_NULLS est désactivé (OFF), les mêmes règles s'appliquent, mais il reste à noter qu'un opérateur d'égalité (=) retourne la valeur TRUE si les deux expressions ont pour valeur NULL. Par exemple, NULL = NULL retourne TRUE si SET ANSI_NULLS est désactivé (OFF).  
  
 Les expressions de type **booléen** s’utilisent dans la clause WHERE pour filtrer les lignes qui répondent aux critères de recherche et dans les instructions de langage de contrôle de flux, comme IF et WHILE. Par exemple :  
  
```  
-- Uses AdventureWorks  
  
DECLARE @MyProduct int;  
SET @MyProduct = 750;  
IF (@MyProduct <> 0)  
   SELECT ProductID, Name, ProductNumber  
   FROM Production.Product  
   WHERE ProductID = @MyProduct;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
