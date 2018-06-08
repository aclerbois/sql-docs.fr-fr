---
title: LIKE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ESCAPE
- LIKE
- ESCAPE_TSQL
- LIKE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ESCAPE keyword
- '% (wildcard - character(s) to match)'
- ASCII pattern matching
- pattern searching [SQL Server]
- wildcard characters [SQL Server]
- literals [SQL Server], using wildcards
- character strings [SQL Server], LIKE
- exact matches [SQL Server]
- Boolean functions
- LIKE comparisons
- matching patterns [SQL Server]
- NOT LIKE keyword
ms.assetid: 581fb289-29f9-412b-869c-18d33a9e93d5
caps.latest.revision: 50
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f97c3e301814d73ece3703a5bd32c55a7b8ab8a5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="like-transact-sql"></a>LIKE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Détermine si une chaîne de caractères donnée correspond à un modèle spécifié. Une chaîne peut comprendre des caractères normaux ainsi que des caractères génériques. Au cours de l'analyse, les caractères normaux doivent correspondre exactement aux caractères spécifiés dans la chaîne de caractères. Toutefois, les caractères génériques peuvent être associés à des portions aléatoires de la chaîne de caractères. L'utilisation de caractères génériques rend l'opérateur LIKE plus flexible que lorsque les opérateurs de comparaison des chaînes = et != sont utilisés. Si l'un de ces arguments n'est pas du type chaîne de caractères, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] essaie de le convertir, dans la mesure du possible.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
match_expression [ NOT ] LIKE pattern [ ESCAPE escape_character ]  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
match_expression [ NOT ] LIKE pattern  
```  
  
## <a name="arguments"></a>Arguments  
 *match_expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide d’un type de données caractères.  
  
 *pattern*  
 Chaîne de caractères spécifique à rechercher dans *match_expression*. Peut inclure les caractères génériques valides suivants. *pattern* peut faire au maximum 8 000 octets.  
  
|Caractère générique|Description| Exemple|  
|------------------------|-----------------|-------------|  
|%|Toute chaîne de zéro caractère ou plus.|WHERE title LIKE '%computer%' trouve tous les titres de livres comportant le terme « computer ».|  
|_ (souligné)|N'importe quel caractère.|WHERE au_fname LIKE '_ean' trouve tous les prénoms en quatre lettres terminant par « ean » (Dean, Sean, etc.).|  
|[ ]|Tout caractère de l'intervalle ([a-f]) ou de l'ensemble spécifié ([abcdef]).|WHERE au_lname LIKE '[C-P]arsen' trouve les noms d'auteurs terminant par « arsen » et commençant par un caractère compris entre C et P, tels que Carsen, Larsen, Karsen, etc. Dans une recherche de plages, les caractères compris dans la plage peuvent varier selon les règles de tri du classement.|  
|[^]|Tout caractère en dehors de l'intervalle ([^a-f]) ou de l'ensemble spécifié ([^abcdef]).|WHERE au_lname LIKE 'de[^l]%' trouve tous les noms d'auteurs commençant par « de » et dont la lettre suivante n'est pas « l ».|  
  
 *escape_character*  
 Caractère placé devant un caractère générique pour indiquer que celui-ci doit être interprété en tant que caractère régulier et non en tant que caractère générique. *escape_character* est une expression de caractères qui n’a pas de valeur par défaut et qui doit être évaluée à un seul caractère.  
  
## <a name="result-types"></a>Types des résultats  
 **Booléen**  
  
## <a name="result-value"></a>Valeur des résultats  
 LIKE renvoie TRUE si *match_expression* correspond au *pattern* spécifié.  
  
## <a name="remarks"></a>Notes   
 Lors de la comparaison de chaînes à l'aide de l'argument LIKE, tous les caractères de la chaîne modèle sont importants, y compris les espaces de début ou de fin. Si vous demandez une comparaison qui renvoie toutes les lignes contenant une chaîne LIKE « abc  » (abc suivi d'un seul espace), une ligne dont la valeur pour cette colonne est « abc » (abc sans espace) ne sera pas renvoyée. Les espaces à droite dont le profil correspond à l'expression ne sont pas pris en compte. Si vous demandez une comparaison qui renvoie toutes les lignes contenant une chaîne LIKE « abc » (abc sans espace), toutes les lignes commençant par la chaîne « abc », qu'elles contiennent ou non des espaces à droite, seront renvoyées.  
  
 Une comparaison de chaînes utilisant un modèle contenant des données de type **char** et **varchar** risque d’échouer lors d’une comparaison LIKE en raison du mode de stockage des données. Vous devez comprendre le stockage de chaque type de données et la cause possible de l'échec d'une comparaison LIKE. Dans l’exemple suivant, une variable locale de type **char** est transmise à une procédure stockée, puis une recherche générique s’effectue pour trouver tous les employés dont le nom commence par un jeu de caractères donné.  
  
```sql
-- Uses AdventureWorks  
  
CREATE PROCEDURE FindEmployee @EmpLName char(20)  
AS  
SELECT @EmpLName = RTRIM(@EmpLName) + '%';  
SELECT p.FirstName, p.LastName, a.City  
FROM Person.Person p JOIN Person.Address a ON p.BusinessEntityID = a.AddressID  
WHERE p.LastName LIKE @EmpLName;  
GO  
EXEC FindEmployee @EmpLName = 'Barb';  
GO  
```  
  
 Dans la procédure `FindEmployee`, aucune ligne n’est retournée car la variable **char** (`@EmpLName`) contient des espaces à droite en fin de chaîne pour chaque nom comprenant moins de 20 caractères. Comme la colonne `LastName` est de type **varchar**, elle ne contient aucun espace à droite. Cette procédure échouera car les espaces de droite sont significatifs.  
  
 L’exemple suivant réussit car les espaces à droite en fin de chaîne ne sont pas ajoutés à une variable **varchar**.  
  
```sql
-- Uses AdventureWorks  
  
CREATE PROCEDURE FindEmployee @EmpLName varchar(20)  
AS  
SELECT @EmpLName = RTRIM(@EmpLName) + '%';  
SELECT p.FirstName, p.LastName, a.City  
FROM Person.Person p JOIN Person.Address a ON p.BusinessEntityID = a.AddressID  
WHERE p.LastName LIKE @EmpLName;  
GO  
EXEC FindEmployee @EmpLName = 'Barb';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 FirstName      LastName            City
 ----------     -------------------- --------------- 
 Angela         Barbariol            Snohomish
 David          Barber               Snohomish
 (2 row(s) affected)  
 ``` 
 
## <a name="pattern-matching-by-using-like"></a>Recherche générique à l'aide de LIKE  
 Le mot clé LIKE prend en charge la recherche générique ASCII ainsi que la recherche générique Unicode. Lorsque tous les arguments (*match_expression*, *pattern* et *escape_character*, le cas échéant) sont des types de données caractères ASCII, une correspondance de modèles ASCII est effectuée. Si l'un des arguments est de type de données Unicode, tous les arguments sont convertis en Unicode et une recherche générique Unicode est effectuée. Lors de l’utilisation des données Unicode (types de données **nchar** ou **nvarchar**) avec LIKE, les espaces à droite sont significatifs ; pour les autres données, cependant, ils ne le sont pas. Unicode LIKE est compatible avec la version standard de ISO. ASCII LIKE est compatible avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Voici une série d'exemples illustrant les différences dans les lignes renvoyées entre une recherche générique ASCII LIKE et une recherche générique Unicode LIKE.  
  
```sql  
-- ASCII pattern matching with char column  
CREATE TABLE t (col1 char(30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE col1 LIKE '% King';   -- returns 1 row  
  
-- Unicode pattern matching with nchar column  
CREATE TABLE t (col1 nchar(30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE col1 LIKE '% King';   -- no rows returned  
  
-- Unicode pattern matching with nchar column and RTRIM  
CREATE TABLE t (col1 nchar (30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE RTRIM(col1) LIKE '% King';   -- returns 1 row  
```  
  
> [!NOTE]  
>  Les comparaisons LIKE sont affectées par le classement. Pour plus d’informations, consultez [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
## <a name="using-the--wildcard-character"></a>Utilisation du caractère générique %  
 Si le symbole LIKE « 5% » est spécifié, [!INCLUDE[ssDE](../../includes/ssde-md.md)] recherche le nombre 5 suivi d'une chaîne de zéro caractère ou plus.  
  
 Par exemple, la requête suivante affiche toutes les vues de gestion dynamique dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], parce qu'elles commencent par les lettres `dm`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT Name  
FROM sys.system_views  
WHERE Name LIKE 'dm%';  
GO  
```  
  
 Pour voir tous les objets qui ne sont pas des vues de gestion dynamique, utilisez `NOT LIKE 'dm%'`. S'il existe 32 objets au total et que LIKE extrait 13 noms correspondant au modèle, NOT LIKE trouvera 19 objets ne correspondant pas au modèle LIKE.  
  
 Il se peut qu'une recherche telle que `LIKE '[^d][^m]%'` ne vous renvoie pas toujours les mêmes noms. Au lieu de 19 noms, il se peut que vous n'en trouviez que 14 (tous les noms commençant par `d` ou dont la deuxième lettre est `m` sont exclus des résultats) avec, en prime, le nom des vues de gestion dynamique. Ce résultat est dû au fait que les chaînes correspondantes avec des caractères génériques de négation sont évaluées par étapes, un caractère générique après l'autre. Si la correspondance échoue à l'une des étapes de l'évaluation, la chaîne de caractères est exclue des résultats.  
  
## <a name="using-wildcard-characters-as-literals"></a>Utilisation de caractères génériques comme termes littéraux  
 Vous pouvez utiliser les caractères génériques comme termes littéraux. Pour utiliser un caractère générique en tant que terme littéral, mettez-le entre parenthèses. Le tableau ci-après propose plusieurs exemples d'utilisation du mot clé LIKE et des caractères génériques [ ].  
  
|Symbole|Signification|  
|------------|-------------|  
|LIKE '5[%]'|5%|  
|LIKE '[_]n'|_n|  
|LIKE '[a-cdf]'|a, b, c, d ou f|  
|LIKE '[-acdf]'|-, a, c, d, ou f|  
|LIKE '[ [ ]'|[|  
|LIKE ']'|]|  
|LIKE 'abc[_]d%'|abc_d et abc_e|  
|LIKE 'abc[def]'|abcd, abce et abcf|  
  
## <a name="pattern-matching-with-the-escape-clause"></a>Recherche générique avec la clause ESCAPE  
 Vous pouvez rechercher des chaînes de caractères comprenant un ou plusieurs caractères génériques particuliers. À titre d'exemple, la table discounts de la base de données customers peut stocker les valeurs de remises comprenant un pourcentage (%). Pour rechercher un pourcentage en tant que caractère et non en tant que caractère générique, le mot clé ESCAPE et le caractère d'échappement doivent être fournis. Par exemple, une base de données exemple contient une colonne dénommée comment comprenant la chaîne 30 %. Pour rechercher les lignes contenant la chaîne 30% n'importe où dans la colonne comment, spécifiez une clause WHERE telle que `WHERE comment LIKE '%30!%%' ESCAPE '!'`. Si le mot clé ESCAPE et le caractère ne sont pas spécifiés, [!INCLUDE[ssDE](../../includes/ssde-md.md)] retourne toutes les lignes comprenant la chaîne 30.  
  
 Si le modèle LIKE ne contient aucun caractère après un caractère d'échappement, il n'est pas valide et le mot clé LIKE renvoie la valeur FALSE. Si le caractère se trouvant après un caractère d'échappement n'est pas un caractère générique, le caractère d'échappement est annulé et le caractère suivant est traité comme un caractère normal. Il s'agit notamment du signe de pourcentage (%), du trait de soulignement (_) et du crochet gauche ([) lorsqu'ils sont placés entre deux crochets ([ ]). En outre, à l'intérieur des doubles crochets ([ ]), les caractères d'échappement peuvent être utilisés et l'accent circonflexe (^), le trait d'union (-) et le crochet (]) droit peuvent être transformés en caractères d'échappement.  
  
 0x0000 (**char(0)**) est un caractère non défini dans les classements Windows et ne peut pas être inclus dans LIKE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-like-with-the--wildcard-character"></a>A. Utilisation de LIKE avec le caractère générique %  
 L'exemple suivant renvoie tous les numéros de téléphone comportant l'indicatif `415` dans la table `PersonPhone`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, ph.PhoneNumber  
FROM Person.PersonPhone AS ph  
INNER JOIN Person.Person AS p  
ON ph.BusinessEntityID = p.BusinessEntityID  
WHERE ph.PhoneNumber LIKE '415%'  
ORDER by p.LastName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
 
 ```
 FirstName             LastName             Phone
 -----------------     -------------------  ------------
 Ruben                 Alonso               415-555-124  
 Shelby                Cook                 415-555-0121  
 Karen                 Hu                   415-555-0114  
 John                  Long                 415-555-0147  
 David                 Long                 415-555-0123  
 Gilbert               Ma                   415-555-0138  
 Meredith              Moreno               415-555-0131  
 Alexandra             Nelson               415-555-0174  
 Taylor                Patterson            415-555-0170  
 Gabrielle              Russell             415-555-0197  
 Dalton                 Simmons             415-555-0115  
 (11 row(s) affected)  
 ``` 
 
### <a name="b-using-not-like-with-the--wildcard-character"></a>B. Utilisation de NOT LIKE avec le caractère générique %  
 L'exemple ci-après renvoie tous les numéros de téléphone de la table `PersonPhone` dont l'indicatif n'est pas `415`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, ph.PhoneNumber  
FROM Person.PersonPhone AS ph  
INNER JOIN Person.Person AS p  
ON ph.BusinessEntityID = p.BusinessEntityID  
WHERE ph.PhoneNumber NOT LIKE '415%' AND p.FirstName = 'Gail'  
ORDER BY p.LastName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
 
 ```
FirstName              LastName            Phone
---------------------- -------------------- -------------------
Gail                  Alexander            1 (11) 500 555-0120  
Gail                  Butler               1 (11) 500 555-0191  
Gail                  Erickson             834-555-0132  
Gail                  Erickson             849-555-0139  
Gail                  Griffin              450-555-0171  
Gail                  Moore                155-555-0169  
Gail                  Russell              334-555-0170  
Gail                  Westover             305-555-0100  
(8 row(s) affected)  
```  

### <a name="c-using-the-escape-clause"></a>C. Utilisation de la clause ESCAPE  
 L'exemple suivant utilise la clause `ESCAPE` et le caractère d'échappement pour trouver la chaîne de caractères exacte `10-15%` dans la colonne `c1` de la table `mytbl2`.  
  
```sql
USE tempdb;  
GO  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
      WHERE TABLE_NAME = 'mytbl2')  
   DROP TABLE mytbl2;  
GO  
USE tempdb;  
GO  
CREATE TABLE mytbl2  
(  
 c1 sysname  
);  
GO  
INSERT mytbl2 VALUES ('Discount is 10-15% off'), ('Discount is .10-.15 off');  
GO  
SELECT c1   
FROM mytbl2  
WHERE c1 LIKE '%10-15!% off%' ESCAPE '!';  
GO  
```  
  
### <a name="d-using-the---wildcard-characters"></a>D. Utilisation des caractères génériques [ ]  
 L’exemple suivant recherche les employés figurant dans la table `Person` dont le prénom est `Cheryl` ou `Sheryl`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, FirstName, LastName   
FROM Person.Person   
WHERE FirstName LIKE '[CS]heryl';  
GO  
```  
  
 L'exemple suivant recherche les lignes correspondant aux employés de la table `Person` dont le nom est `Zheng` ou `Zhang`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, FirstName  
FROM Person.Person  
WHERE LastName LIKE 'Zh[ae]ng'  
ORDER BY LastName ASC, FirstName ASC;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-like-with-the--wildcard-character"></a>E. Utilisation de LIKE avec le caractère générique %  
 L’exemple suivant recherche tous les employés figurant dans la table `DimEmployee` dont le numéro de téléphone commence par `612`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone LIKE '612%'  
ORDER by LastName;  
```  
  
### <a name="f-using-not-like-with-the--wildcard-character"></a>F. Utilisation de NOT LIKE avec le caractère générique %  
 L’exemple suivant recherche tous les numéros de téléphone figurant dans la table `DimEmployee` qui ne commencent pas par `612`.  .  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone NOT LIKE '612%'  
ORDER by LastName;  
```  
  
### <a name="g-using-like-with-the--wildcard-character"></a>G. Utilisation de LIKE avec le caractère générique _  
 L’exemple suivant recherche tous les numéros de téléphone dont l’indicatif commence par `6` et se termine par `2` dans la table `DimEmployee`. Notez que le caractère générique % est également inclus à la fin du modèle de recherche, car l’indicatif est la première partie du numéro de téléphone et il existe d’autres caractères après dans la valeur de colonne.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone LIKE '6_2%'  
ORDER by LastName;   
```  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
 
