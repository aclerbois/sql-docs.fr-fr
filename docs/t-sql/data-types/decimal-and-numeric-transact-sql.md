---
title: decimal et numeric (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 7/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- decimal
- decimal_TSQL
- numeric
- numeric_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- decimal data type
- decimal data type, about decimal data type
- numeric data type
- numeric data type, about numeric data type
ms.assetid: 9d862a90-e6b7-4692-8605-92358dccccdf
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 2c86af3c5e32a92b3210327881cdc140f6fe7d20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="decimal-and-numeric-transact-sql"></a>decimal et numeric (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Types de données numériques ayant une précision et une échelle fixes. Les types decimal et numeric sont synonymes et peuvent être utilisés indifféremment.
  
## <a name="arguments"></a>Arguments  
**decimal**[ **(***p*[ **,***s*] **)**] and **numeric**[ **(***p*[ **,***s*] **)**]  
Valeurs de précision et d'échelle fixes. Lorsque la précision maximale est utilisée, les valeurs valides sont comprises entre - 10^38 +1 et 10^38 - 1. Les synonymes ISO de **decimal** sont **dec** et **dec(***p*, *s***)**. **numeric** est fonctionnellement équivalent à **decimal**.
  
p (précision)  
Nombre maximal de chiffres décimaux à gauche et à droite de la virgule. La précision doit être une valeur comprise entre 1 et la précision maximale de 38. La précision par défaut est 18.
  
> [!NOTE]  
>  Informatica prend uniquement en charge 16 chiffres significatifs, quelles que soient la précision et l’échelle spécifiées.  
  
*s* (échelle)  
Nombre de chiffres décimaux à droite de la virgule. Ce nombre est soustrait de *p* afin de déterminer le nombre maximal de chiffres à gauche de la virgule décimale. Nombre maximal de chiffres décimaux à droite de la virgule. La valeur de l’échelle doit être comprise entre 0 et *p*. L'échelle ne peut être spécifiée que si la précision l'est également. La valeur par défaut de l’échelle est 0 ; par conséquent, 0 <= *s* \<= *p*. Les tailles de stockage maximales varient en fonction de la précision.
  
|Précision|Taille de stockage (octets)|  
|---|---|
|1-9|5|  
|10-19|9|  
|20-28|13|  
|29-38|17|  
  
> [!NOTE]  
>  Informatica (connecté via le connecteur Informatica SQL Server PDW) prend uniquement en charge 16 chiffres significatifs, quelles que soient la précision et l’échelle spécifiées.  
  
## <a name="converting-decimal-and-numeric-data"></a>Conversion de données decimal et numeric
Pour les types de données **decimal** et **numeric**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère chaque combinaison de précision et d’échelle comme un type de données différent. Par exemple, **decimal(5,5)** et **decimal(5,0)** sont considérés comme des types de données différents.
  
Dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], une constante avec une virgule décimale est automatiquement convertie en valeur de donnée **numeric**, en utilisant la précision et l’échelle requises minimales. Par exemple, la constante 12,345 est convertie en valeur **numeric** avec une précision de 5 et une échelle de 3.
  
La conversion de **decimal** ou **numeric** en **float** ou **real** peut entraîner une certaine perte de précision. La conversion de **int**, **smallint**, **tinyint**, **float**, **real**, **money** ou **smallmoney** en **decimal** ou **numeric** peut entraîner un dépassement de capacité.
  
Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les arrondis lors de la conversion d’un nombre en valeur **decimal** ou **numeric** avec une précision et une échelle plus faibles. Toutefois, si l'option SET ARITHABORT a la valeur ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déclenche une erreur lors du dépassement de capacité. Perdre uniquement la précision et l'échelle ne suffit pas à engendrer une erreur.
  
Lors de la conversion de valeurs flottantes ou réelles en valeurs décimales ou numériques, la valeur décimale n'aura jamais plus de 17 décimales. Toute valeur flottante < 5E-18 sera toujours convertie en 0.
  
## <a name="examples"></a>Exemples  
L’exemple suivant crée une table à l’aide des types de données **decimal** et **numeric**.  Les valeurs sont insérées dans chaque colonne et les résultats retournés à l'aide de l'instruction SELECT.
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyDecimalColumn decimal(5,2)  
,MyNumericColumn numeric(10,5)
  
);  
  
GO  
INSERT INTO dbo.MyTable VALUES (123, 12345.12);  
GO  
SELECT MyDecimalColumn, MyNumericColumn  
FROM dbo.MyTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
MyDecimalColumn                         MyNumericColumn  
--------------------------------------- ---------------------------------------  
123.00                                  12345.12000  
  
(1 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Voir aussi
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  
