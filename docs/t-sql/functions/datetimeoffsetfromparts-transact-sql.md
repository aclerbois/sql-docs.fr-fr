---
title: DATETIMEOFFSETFROMPARTS (Transact-SQL) | Microsoft Docs
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
- DATETIMEOFFSETFROMPARTS_TSQL
- DATETIMEOFFSETFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEOFFSETFROMPARTS function
ms.assetid: 463da1f4-b4b6-45a3-9a95-ea1f99575542
caps.latest.revision: 19
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5549f258e758cc91a228d5fee707120b57963d46
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datetimeoffsetfromparts-transact-sql"></a>DATETIMEOFFSETFROMPARTS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all-md](../../includes/tsql-appliesto-ss2012-all-md.md)]

Renvoie une valeur **datetimeoffset** pour la date et l’heure spécifiées et avec la précision et les décalages spécifiés.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision )  
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
  
*minute*  
Expression entière spécifiant des minutes.
  
*secondes*  
Expression entière spécifiant des secondes.
  
*fractions*  
Expression entière spécifiant des fractions.
  
*hour_offset*  
Expression entière spécifiant la partie heure du décalage de fuseau horaire.
  
*minute_offset*  
Expression entière spécifiant la partie minute du décalage de fuseau horaire.
  
*precision*  
Littéral entier spécifiant la précision de la valeur **datetimeoffset** à renvoyer.
  
## <a name="return-types"></a>Types de retour
**datetimeoffset(** *precision* **)**
  
## <a name="remarks"></a>Notes   
**DATETIMEOFFSETFROMPARTS** renvoie un type de données **datetimeoffset** complètement initialisé. Les arguments de décalage sont utilisés pour représenter le décalage de fuseau horaire. Si les arguments de décalage sont omis, il est supposé que le décalage de fuseau horaire est 0h00, autrement dit il n'y a aucun décalage de fuseau horaire. Si les arguments de décalage sont spécifiés, les deux arguments doivent être présents et tous deux doivent être positifs ou négatifs. Si l’argument *minute_offset* est spécifié sans *hour_offset*, une erreur est générée. Si d'autres arguments ne sont pas valides, une erreur est générée. Si les arguments requis sont NULL, la valeur NULL est renvoyée. Toutefois, si l’argument *precision* est NULL, une erreur est générée.
  
L’argument *fractions* dépend de l’argument *precision*. Par exemple, si *precision* a pour valeur 7, chaque fraction représente 100 nanosecondes ; si *precision* a pour valeur 3, chaque fraction représente une milliseconde. Si la valeur de *precision* est zéro, la valeur de *fractions* doit également être zéro ; sinon, une erreur est générée.
  
Cette fonction peut être exécutée à distance sur des serveurs [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et versions ultérieures. Elle ne peut pas être exécutée à distance sur des serveurs dont la version est antérieure à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example-without-fractions-of-a-second"></a>A. Exemple simple sans fractions de seconde  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2010, 12, 31, 14, 23, 23, 0, 12, 0, 7 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Result  
-------------------------------------------  
2010-12-07 00:00:00.0000000 +00:00  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Exemple avec fractions de seconde  
L’exemple suivant illustre l’utilisation des paramètres *fractions* et *precision* :
1.   Lorsque *fractions* a la valeur 5 et *precision* la valeur 1, la valeur de *fractions* représente 5/10 de seconde.  
1.   Lorsque *fractions* a la valeur 50 et *precision* la valeur 2, la valeur de *fractions* représente 50/100 de seconde.  
1.   Lorsque *fractions* a la valeur 500 et *precision* la valeur 3, la valeur de *fractions* représente 500/1000 de seconde.  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 5, 12, 30, 1 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 50, 12, 30, 2 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 500, 12, 30, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
----------------------------------  
2011-08-15 14:30:00.5 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.50 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.500 +12:30  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  


