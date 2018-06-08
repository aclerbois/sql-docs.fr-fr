---
title: DATETIME2FROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DATETIME2FROMPARTS_TSQL
- DATETIME2FROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIME2FROMPARTS function
ms.assetid: 632b757d-d2d1-43a5-b870-792a779ae204
caps.latest.revision: 16
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1a6cee0f40ba7cd92024fd11b82a32c3de99d0e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datetime2fromparts-transact-sql"></a>DATETIME2FROMPARTS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all-md](../../includes/tsql-appliesto-ss2012-all-md.md)]

Renvoie une valeur **datetime2** pour la date et l’heure spécifiées, avec la précision spécifiée.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DATETIME2FROMPARTS ( year, month, day, hour, minute, seconds, fractions, precision )  
```  
  
## <a name="arguments"></a>Arguments  
*year*  
Expression entière spécifiant une année.
  
*month*  
Expression entière spécifiant un mois.
  
*day*  
Expression entière spécifiant un jour.
  
 *hour*  
Expression entière spécifiant des heures.
  
*minute* Expression entière spécifiant des minutes.
  
*secondes*  
Expression entière spécifiant des secondes.
  
*fractions*  
Expression entière spécifiant des fractions.
  
*precision*  
Littéral entier spécifiant la précision de la valeur **datetime2** à renvoyer.
  
## <a name="return-types"></a>Types de retour
**datetime2(** *precision* **)**
  
## <a name="remarks"></a>Notes   
**DATETIME2FROMPARTS** renvoie une valeur **datetime2** complètement initialisée. Si les arguments ne sont pas valides, une erreur est générée. Si les arguments obligatoires sont NULL, la valeur NULL est retournée. Toutefois, si l’argument *precision* est NULL, une erreur est générée.
  
L’argument *fractions* dépend de l’argument *precision*. Par exemple, si *precision* a pour valeur 7, chaque fraction représente 100 nanosecondes ; si *precision* a pour valeur 3, chaque fraction représente une milliseconde. Si la valeur de *precision* est zéro, la valeur de *fractions* doit également être zéro ; sinon, une erreur est générée.
  
Cette fonction peut être exécutée à distance sur des serveurs [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et versions ultérieures. Elle ne peut pas être exécutée à distance sur des serveurs dont la version est antérieure à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example-without-fractions-of-a-second"></a>A. Exemple simple sans fractions de seconde  
  
```sql
SELECT DATETIME2FROMPARTS ( 2010, 12, 31, 23, 59, 59, 0, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Result  
---------------------------  
2010-12-31 23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Exemple avec fractions de seconde  
L’exemple suivant illustre l’utilisation des paramètres *fractions* et *precision* :
  
1.  Lorsque *fractions* a la valeur 5 et *precision* la valeur 1, la valeur de *fractions* représente 5/10 de seconde.  
  
2.  Lorsque *fractions* a la valeur 50 et *precision* la valeur 2, la valeur de *fractions* représente 50/100 de seconde.  
  
3.  Lorsque *fractions* a la valeur 500 et *precision* la valeur 3, la valeur de *fractions* représente 500/1000 de seconde.  
  
```sql
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 5, 1 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 50, 2 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 500, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
----------------------  
2011-08-15 14:23:44.5  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.50  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.500  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)
  
  

