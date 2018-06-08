---
title: char et varchar (Transact-SQL) | Microsoft Docs
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
- varchar
- varchar_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fixed-length data types [SQL Server]
- character data type
- char data type
- varchar(max) data type
- variable-length data types [SQL Server]
- varchar data type
ms.assetid: 282cd982-f4fb-4b22-b2df-9e8478f13f6a
caps.latest.revision: 48
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 0d07b9fec8168a21ff492ac216f08881ff278932
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="char-and-varchar-transact-sql"></a>char et varchar (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Ces types de données sont de longueur fixe ou de longueur variable.  
  
## <a name="arguments"></a>Arguments  
**char** [ ( *n* ) ] Données de type chaîne non Unicode de longueur fixe. *n* définit la longueur de chaîne et doit être une valeur comprise entre 1 et 8 000. La taille de stockage est égale à *n* octets. Le synonyme ISO de **char** est **character**.
  
**varchar** [ ( *n* | **max** ) ] Données de type chaîne non Unicode de longueur variable. *n* définit la longueur de chaîne et peut être une valeur comprise entre 1 et 8 000. **max** indique que la taille de stockage maximale est de 2^31-1 octets (2 Go). La taille mémoire est la longueur réelle des données entrées, plus deux octets. Les synonymes ISO de **varchar** sont **charvarying** ou **charactervarying**.
  
## <a name="remarks"></a>Notes   
Quand la valeur de *n* n’est spécifiée ni dans une définition de données ni dans une instruction de déclaration de variable, la longueur par défaut est 1. Quand la valeur de *n* n’est pas précisée avec les fonctions CAST et CONVERT, la longueur par défaut est 30.
  
Les objets qui utilisent **char** ou **varchar** reçoivent le classement par défaut de la base de données, sauf si un classement spécifique est affecté à l’aide de la clause COLLATE. Le classement contrôle la page de codes utilisée pour stocker les données de caractères.
  
Afin de limiter les problèmes de conversion de caractères si vos sites prennent en charge plusieurs langues, envisagez d’utiliser les types de données Unicode **nchar** ou **nvarchar**. Si vous utilisez **char** ou **varchar**, nous vous recommandons de suivre les instructions suivantes :
- Utilisez **char** quand les tailles des entrées de données de la colonne sont cohérentes.  
- Utilisez **varchar** quand les tailles des entrées de données de la colonne varient considérablement.  
- Utilisez **varchar(max)** quand les tailles des entrées de données de la colonne varient considérablement et que la taille peut dépasser 8 000 octets.  
  
Si SET ANSI_PADDING a la valeur OFF lors de l’exécution de CREATE TABLE ou d’ALTER TABLE, une colonne de type **char** définie comme Null est gérée comme une colonne de type **varchar**.
  
Quand la page de codes du classement utilise des caractères sur deux octets, la taille de stockage est toujours de *n* octets. En fonction de la chaîne de caractères, la taille de stockage de *n* octets peut être inférieure à *n* caractères.

> [!WARNING]
> Chaque colonne varchar(max) ou nvarchar(max) non Null demande 24 octets d’allocation fixe supplémentaire calculée par rapport à la limite de 8 060 octets par ligne pendant une opération de tri. Cela peut produire une limite implicite du nombre de colonnes varchar(max) ou nvarchar(max) non Null pouvant être créées dans une table.  
Aucune erreur spéciale n'est fournie quand la table est créée (mis à part l’avertissement habituel indiquant que la taille maximale de ligne dépasse la taille maximale autorisée de 8 060 octets) ou quand les données sont insérées. Cette grande taille de ligne peut provoquer des erreurs (comme l’erreur 512) au cours des opérations normales, telles que la mise à jour de la clé d’index cluster ou le tri de l’intégralité des colonnes, que les utilisateurs ne peuvent pas anticiper tant qu’elles n’ont pas été effectuées.
  
##  <a name="_character"></a> Conversion de données caractères  
Lorsque des expressions de caractères sont converties en type caractère de taille différente, les valeurs trop longues pour le nouveau type de données sont tronquées. Le type **uniqueidentifier** est considéré comme un type caractère pour les besoins de la conversion à partir d’une expression de caractères ; il est par conséquent soumis aux règles de troncation pour la conversion en un type caractère. Consultez la section Exemples qui suit.
  
Quand une expression de caractères est convertie en une expression de caractères de taille ou de type de données différent (par exemple, de **char(5)** en **varchar(5)**, ou **char(20)** en **char(15)**), le classement de la valeur d’entrée est affecté à la valeur convertie. Si une expression de type non caractère est convertie en un type de données caractère, le classement par défaut de la base de données active est affecté à la valeur convertie. Dans les deux cas, vous pouvez affecter un classement spécifique à l’aide de la clause [COLLATE](http://msdn.microsoft.com/library/4ba6b7d8-114a-4f4e-bb38-fe5697add4e9).
  
> [!NOTE]  
>  Les traductions de pages de codes sont prises en charge pour les types de données **char** et **varchar**, mais pas pour **text**. De manière identique aux versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la perte de données lors de l'interprétation d'une page de codes n'est pas mentionnée.  
  
Les expressions de caractères converties en un type de données **numeric** approximatif peuvent éventuellement inclure une indication exponentielle (un « e » majuscule ou minuscule éventuellement suivi d’un plus (+) ou d’un moins (-) et du nombre).
  
Les expressions de caractères converties en un type de données **numeric** exact doivent être composées de chiffres, d’une virgule décimale et éventuellement d’un plus (+) ou d’un moins (-). Les espaces à gauche sont ignorés. Les séparateurs virgule (séparateurs de milliers en notation anglaise comme dans 123,456.00) ne sont pas autorisés dans la chaîne.
  
Les expressions de caractères converties en types de données **money** ou **smallmoney** peuvent également comprendre une virgule décimale et le symbole du dollar ($). Les séparateurs virgule (séparateurs de milliers en notation anglaise comme dans $123,456.00) sont autorisés.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-showing-the-default-value-of-n-when-used-in-variable-declaration"></a>A. Affichage de la valeur par défaut de n utilisée dans une déclaration de variable.  
L’exemple suivant montre que la valeur par défaut de *n* est 1 pour les types de données `char` et `varchar` quand ils sont utilisés dans une déclaration de variable.
  
```sql
DECLARE @myVariable AS varchar = 'abc';  
DECLARE @myNextVariable AS char = 'abc';  
--The following returns 1  
SELECT DATALENGTH(@myVariable), DATALENGTH(@myNextVariable);  
GO  
```  
  
### <a name="b-showing-the-default-value-of-n-when-varchar-is-used-with-cast-and-convert"></a>B. Affichage de la valeur par défaut de n lorsque varchar est utilisé dans CAST et CONVERT.  
L’exemple suivant montre que la valeur par défaut de *n* est 30 quand les types de données `char` ou `varchar` sont utilisés avec les fonctions `CAST` et `CONVERT`.
  
```sql
DECLARE @myVariable AS varchar(40);  
SET @myVariable = 'This string is longer than thirty characters';  
SELECT CAST(@myVariable AS varchar);  
SELECT DATALENGTH(CAST(@myVariable AS varchar)) AS 'VarcharDefaultLength';  
SELECT CONVERT(char, @myVariable);  
SELECT DATALENGTH(CONVERT(char, @myVariable)) AS 'VarcharDefaultLength';  
```  
  
### <a name="c-converting-data-for-display-purposes"></a>C. Conversion de données à des fins d'affichage  
L'exemple suivant convertit deux colonnes en types caractères et applique un style qui applique un format spécifique aux données affichées. Un type **money** est converti en données caractères et le style 1 est appliqué, ce qui affiche les valeurs avec une virgule tous les trois chiffres à gauche de la virgule décimale et deux chiffres à droite de la virgule décimale. Un type **datetime** est converti en données caractères et le style 3 est appliqué, ce qui affiche les données au format jj/mm/aa. Dans la clause WHERE, un type **money** est converti en un type caractère pour effectuer une opération de comparaison de chaînes.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT  BusinessEntityID,   
   SalesYTD,   
   CONVERT (varchar(12),SalesYTD,1) AS MoneyDisplayStyle1,   
   GETDATE() AS CurrentDate,   
   CONVERT(varchar(12), GETDATE(), 3) AS DateDisplayStyle3  
FROM Sales.SalesPerson  
WHERE CAST(SalesYTD AS varchar(20) ) LIKE '1%';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
BusinessEntityID SalesYTD              DisplayFormat CurrentDate             DisplayDateFormat  
---------------- --------------------- ------------- ----------------------- -----------------  
278              1453719.4653          1,453,719.47  2011-05-07 14:29:01.193 07/05/11  
280              1352577.1325          1,352,577.13  2011-05-07 14:29:01.193 07/05/11  
283              1573012.9383          1,573,012.94  2011-05-07 14:29:01.193 07/05/11  
284              1576562.1966          1,576,562.20  2011-05-07 14:29:01.193 07/05/11  
285              172524.4512           172,524.45    2011-05-07 14:29:01.193 07/05/11  
286              1421810.9242          1,421,810.92  2011-05-07 14:29:01.193 07/05/11  
288              1827066.7118          1,827,066.71  2011-05-07 14:29:01.193 07/05/11  
```  
  
### <a name="d-converting-uniqueidentifer-data"></a>D. Conversion de données Uniqueidentifer  
L'exemple suivant convertit une valeur `uniqueidentifier` en type de données `char`.
  
```sql
DECLARE @myid uniqueidentifier = NEWID();  
SELECT CONVERT(char(255), @myid) AS 'char';  
```  
  
L'exemple suivant illustre la troncation des données lorsque la valeur est trop longue pour le type de données vers lequel la conversion est effectuée. Étant donné que le type **uniqueidentifier** est limité à 36 caractères, les caractères qui dépassent cette longueur sont tronqués.
  
```sql
DECLARE @ID nvarchar(max) = N'0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong';  
SELECT @ID, CONVERT(uniqueidentifier, @ID) AS TruncatedValue;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
String                                       TruncatedValue  
-------------------------------------------- ------------------------------------  
0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong    0E984725-C51C-4BF4-9960-E1C80E27ABA0  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[nchar et nvarchar &#40;Transact-SQL&#41;](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[COLLATE &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/4ba6b7d8-114a-4f4e-bb38-fe5697add4e9)  
[Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Estimer la taille d'une base de données](../../relational-databases/databases/estimate-the-size-of-a-database.md)
  
  
