---
title: Exécuter une requête avec une recherche en texte intégral | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- queries [full-text search], about full-text queries
- queries [full-text search], predicates
- full-text queries [SQL Server], about full-text queries
- full-text search [SQL Server], querying SQL Server
- full-text queries [SQL Server]
- queries [full-text search], functions
ms.assetid: 7624ba76-594b-4be5-ac10-c3ac4a3529bd
caps.latest.revision: 80
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 418f38995d2e9a4ef773f593a972ca623d501a36
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="query-with-full-text-search"></a>Exécuter une requête avec une recherche en texte intégral
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
Écrivez des requêtes de texte intégral en utilisant les prédicats **CONTAINS** et **FREETEXT**, ainsi que les fonctions d’ensemble de lignes **CONTAINSTABLE** et **FREETEXTTABLE** avec une instruction **SELECT**. Cet article fournit des exemples de chaque prédicat et de chaque fonction, et vous aide à choisir le mieux adapté.

-   Pour rechercher des correspondances avec des mots et des expressions, utilisez **CONTAINS** et **CONTAINSTABLE**.
-   Pour rechercher des correspondances avec la signification, mais pas avec le libellé exact, utilisez **FREETEXT** et **FREETEXTTABLE**.

## <a name="examples_simple"></a> Exemples des différents prédicats et fonctions

Les exemples suivants utilisent l’exemple de base de données AdventureWorks. Pour la version finale d’AdventureWorks, consultez [Bases de données AdventureWorks et scripts pour SQL Server 2016 CTP3](https://www.microsoft.com/download/details.aspx?id=49502). Pour exécuter les exemples de requêtes, vous devez également configurer la recherche en texte intégral. Pour plus d’informations, consultez [Bien démarrer avec la recherche en texte intégral](get-started-with-full-text-search.md). 

### <a name="example---contains"></a>Exemple – CONTAINS  
L’exemple suivant recherche tous les produits dont le prix est `$80.99` et qui contiennent le mot `"Mountain"` :
  
```sql
USE AdventureWorks2012  
GO  
  
SELECT Name, ListPrice  
FROM Production.Product  
WHERE ListPrice = 80.99  
   AND CONTAINS(Name, 'Mountain')  
GO  
```  
  
### <a name="example---freetext"></a>Exemple – FREETEXT 
 L’exemple suivant recherche tous les documents qui contiennent des mots relatifs à des `vital safety components` :
  
```sql
USE AdventureWorks2012  
GO  
  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components')  
GO  
```

### <a name="example---containstable"></a>Exemple – CONTAINSTABLE  
 L’exemple suivant retourne l’ID de description et la description de tous les produits dont la colonne **Description** contient le mot « aluminum » à proximité du mot « light » ou du mot « lightweight ». Seules les lignes dont le classement est supérieur ou égal à 2 sont retournées.  
  
```sql
USE AdventureWorks2012  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)'  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK > 2  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
### <a name="example---freetexttable"></a>Exemple – FREETEXTTABLE  
 L'exemple ci-après étend une requête FREETEXTTABLE afin de retourner en premier les lignes dont le niveau de classement est le plus élevé et d'ajouter le classement de chaque ligne à la liste de sélection. Pour écrire une requête similaire, vous devez savoir que **ProductDescriptionID** est la colonne clé unique de la table **ProductDescription**.  
  
```sql 
USE AdventureWorks2012  
GO  
  
SELECT KEY_TBL.RANK, FT_TBL.Description  
FROM Production.ProductDescription AS FT_TBL   
     INNER JOIN  
     FREETEXTTABLE(Production.ProductDescription, Description,  
                    'perfect all-around bike') AS KEY_TBL  
     ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
ORDER BY KEY_TBL.RANK DESC  
GO  
```  
  
Voici une extension de la même requête qui retourne seulement les lignes avec un classement égal ou supérieur à 10 :  
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT KEY_TBL.RANK, FT_TBL.Description  
FROM Production.ProductDescription AS FT_TBL   
     INNER JOIN  
     FREETEXTTABLE(Production.ProductDescription, Description,  
                    'perfect all-around bike') AS KEY_TBL  
     ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK >= 10  
ORDER BY KEY_TBL.RANK DESC  
GO  
```  

## <a name="match-words-or-match-meaning"></a>Rechercher des mots ou rechercher une signification

`CONTAINS`/`CONTAINSTABLE` et `FREETEXT`/`FREETEXTTABLE` sont utiles pour différents types de mise en correspondance. Les informations suivantes vous aident à choisir le meilleur prédicat ou la meilleure fonction pour votre requête :

### <a name="containscontainstable"></a>CONTAINS/CONTAINSTABLE

-   Recherchez des correspondances exactes ou approximatives (moins précises) de mots et d’expressions.
-   Vous pouvez également effectuer les opérations suivantes :
    -   Spécifier la proximité de mots à une certaine distance les uns des autres.
    -   Retourner des correspondances pondérées.
    -   Combiner des conditions de recherche avec des opérateurs logiques. Pour plus d’informations, consultez [Utilisation des opérateurs booléens (AND, OR et NOT)](#Using_Boolean_Operators) plus loin dans cet article.

### <a name="freetextfreetexttable"></a>FREETEXT/FREETEXTTABLE

-   Recherchez des correspondances de signification, et non pas du libellé exact, des mots, expressions ou phrases spécifiés (*chaîne en texte libre*).
-   Des correspondances sont générées si un terme ou une forme de quelque terme que ce soit est trouvé dans l'index de recherche en texte intégral d'une colonne spécifiée.

## <a name="compare-predicates-and-functions"></a>Comparer des prédicats et des fonctions

Les prédicats `CONTAINS` / `FREETEXT` et les fonctions d’ensemble de lignes `CONTAINSTABLE`/`FREETEXTTABLE` ont une syntaxe et des options différentes. Les informations suivantes vous aident à choisir le meilleur prédicat ou la meilleure fonction pour votre requête :

### <a name="predicates-contains-and-freetext"></a>Prédicats CONTAINS et FREETEXT

**Utilisation**. Utilisez les **prédicats** de texte intégral CONTAINS et FREETEXT dans la clause WHERE ou HAVING d’une instruction SELECT.

**Résultats**. Les prédicats CONTAINS et FREETEXT retournent une valeur TRUE ou FALSE qui indique si une ligne donnée correspond à la requête de texte intégral. Les lignes correspondantes sont retournées dans le jeu de résultats.

**Autres options**. Vous pouvez combiner les prédicats à l’un quelconque des autres prédicats [!INCLUDE[tsql](../../includes/tsql-md.md)], comme LIKE et BETWEEN.

Vous pouvez spécifier une colonne unique, une liste de colonnes ou toutes les colonnes de la table sur laquelle porte la recherche.

Vous pouvez éventuellement spécifier la langue dont les ressources sont utilisées par la requête de texte intégral pour l’analyse lexicale et la recherche de radical, les consultations de dictionnaire des synonymes et la suppression de mots parasites.

Vous pouvez utiliser un nom en quatre parties dans le prédicat CONTAINS ou FREETEXT pour interroger les colonnes indexées de texte intégral des tables cibles d'un serveur lié. Pour préparer un serveur distant à recevoir des requêtes de texte intégral, créez un index de recherche en texte intégral sur les tables et colonnes cibles du serveur distant, puis ajoutez le serveur distant comme serveur lié.

**Plus d’informations**. Pour plus d’informations sur la syntaxe et les arguments de ces prédicats, consultez [CONTAINS](../../t-sql/queries/contains-transact-sql.md) et [FREETEXT](../../t-sql/queries/freetext-transact-sql.md).

### <a name="rowset-valued-functions-containstable-and-freetexttable"></a>Vue d’ensemble des fonctions d’ensemble de lignes CONTAINSTABLE et FREETEXTTABLE

**Utilisation**. Utilisez les **fonctions** de texte intégral CONTAINSTABLE et FREETEXTTABLE comme un nom de table standard dans la clause FROM d’une instruction SELECT.

Vous devez spécifier la table de base à rechercher lorsque vous utilisez l’une de ces fonctions. Comme avec les prédicats, vous pouvez spécifier une colonne unique, une liste de colonnes, ou toutes les colonnes de la table sur laquelle doit porter la recherche, et éventuellement, la langue dont les ressources sont utilisées par une requête de texte intégral donnée.

En général, vous devez joindre les résultats des fonctions CONTAINSTABLE ou FREETEXTTABLE avec la table de base. Pour joindre les tables, vous devez connaître le nom de la colonne clé unique. Cette colonne, qui est présente dans toutes les tables activées pour la recherche en texte intégral, garantit l’unicité des lignes de la table (*colonne clé***unique*). Pour plus d’informations sur la colonne clé, consultez [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md).

**Résultats**. Ces fonctions retournent une table contenant zéro, une ou plusieurs lignes correspondant à la requête de texte intégral. La table retournée contient uniquement les lignes de la table de base qui correspondent aux critères de sélection spécifiés dans la condition de recherche en texte intégral de la fonction.

Les requêtes qui utilisent une de ces fonctions retournent également une valeur de classement de pertinence (RANK) et une clé de texte intégral (KEY) pour chaque ligne retournée, comme suit :

-   Colonne **KEY**. La colonne KEY retourne les valeurs uniques des lignes retournées. La colonne KEY peut être utilisée pour spécifier des critères de sélection.
-   Colonne **RANK**. La colonne RANK retourne une *valeur de classement* pour chaque ligne qui indique le degré de correspondance de cette dernière avec les critères de sélection. Plus la valeur de classement du texte ou document d'une ligne est élevée, plus celle-ci est pertinente pour la requête de texte intégral. Différentes lignes peuvent avoir le même classement. Vous pouvez limiter le nombre de correspondances à retourner en spécifiant le paramètre facultatif *top_n_by_rank* . Pour plus d’informations, consultez [Limiter les résultats de la recherche avec RANK](../../relational-databases/search/limit-search-results-with-rank.md).

**Plus d’informations**. Pour plus d’informations sur la syntaxe et les arguments de ces fonctions, consultez [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) et [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md).

## <a name="examples_specific"></a> Types spécifiques de recherches

###  <a name="Simple_Term"></a> Recherche d’un mot ou d’une expression spécifique (terme simple)  
 Vous pouvez utiliser [CONTAINS](../../t-sql/queries/contains-transact-sql.md), [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md), [FREETEXT](../../t-sql/queries/freetext-transact-sql.md) ou [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) pour rechercher un mot ou une expression spécifique dans une table. Par exemple, si vous voulez effectuer une recherche dans la table **ProductReview** de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] afin de trouver tous les commentaires de produits contenant l’expression « learning curve », vous pouvez utiliser le prédicat CONTAINS comme suit :  
  
```sql
USE AdventureWorks2012  
GO  
  
SELECT Comments  
FROM Production.ProductReview  
WHERE CONTAINS(Comments, '"learning curve"')  
GO  
```  
  
La condition de recherche, en l’occurrence « learning curve », peut être complexe et comporter un ou plusieurs termes.

#### <a name="more-info-about-simple-term-searches"></a>Plus d’informations sur la recherche de termes simples

Dans une recherche en texte intégral, un *mot* (ou *jeton*) est une chaîne dont les limites sont identifiées par les analyseurs lexicaux appropriés, qui suivent les règles linguistiques de la langue spécifiée. Une *expression* valide est constituée de plusieurs mots, avec ou sans signes de ponctuation entre eux.

Par exemple, « croissant » constitue un mot et « café au lait » une expression. De tels mots et expressions sont considérés comme des termes simples.

[CONTAINS](../../t-sql/queries/contains-transact-sql.md) et [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) recherchent une correspondance exacte pour l'expression. [FREETEXT](../../t-sql/queries/freetext-transact-sql.md) et [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) décomposent l'expression en mots distincts.

###  <a name="Prefix_Term"></a> Recherche d’un mot avec un préfixe (terme de préfixe)  
 Vous pouvez utiliser [CONTAINS](../../t-sql/queries/contains-transact-sql.md) ou [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) pour rechercher des mots ou des expressions ayant un préfixe que vous spécifiez. Toutes les entrées de la colonne qui contiennent le texte commençant par le préfixe spécifié sont retournées. Par exemple, rechercher toutes les lignes qui contiennent le préfixe `top`-, comme dans `top``ple`, `top``ping`et `top`. La requête se présente comme dans l’exemple suivant :  
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT Description, ProductDescriptionID  
FROM Production.ProductDescription  
WHERE CONTAINS (Description, '"top*"' )  
GO  
```  
  
 Tout texte correspondant au texte spécifié avant l'astérisque (*) est renvoyé. Si le texte et l'astérisque ne sont pas délimités par des guillemets doubles, comme dans `CONTAINS (DESCRIPTION, 'top*')`, la recherche en texte intégral ne considère pas l'astérisque comme un caractère générique.  
  
 Lorsque le préfixe est une expression, chaque jeton à l'intérieur de l'expression est considéré comme un préfixe distinct. Toutes les lignes qui possèdent des mots qui commencent par les préfixes seront renvoyés. Par exemple, le terme de préfixe « light bread* » trouve des lignes contenant le texte « light breaded », « lightly breaded » ou « light bread », mais il ne retourne pas « lightly toasted bread ».

#### <a name="more-info-about-prefix-searches"></a>Plus d’informations sur les recherches par préfixe

Un *terme de préfixe* fait référence à une chaîne apposée au devant d’un mot pour produire un mot dérivatif ou une forme fléchie.

-   Pour un terme de préfixe unique, tout mot qui démarre avec le terme spécifié fera partie du jeu de résultats. Par exemple, le terme « auto* » établit une correspondance avec « automatique », « automobile », etc.

-   Dans le cas d'une expression, chaque mot faisant partie de l'expression est considéré comme un terme préfixe. Par exemple, le terme « auto tran\*» établit une correspondance avec « automatic transmission » et « automobile transducer », mais pas avec « automatic motor transmission ».

Les recherches par préfixe sont prises en charge par [CONTAINS](../../t-sql/queries/contains-transact-sql.md) et par [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md).
  
###  <a name="Inflectional_Generation_Term"></a> Recherche de formes fléchies d’un mot spécifique (forme canonique)  
Vous pouvez utiliser [CONTAINS](../../t-sql/queries/contains-transact-sql.md), [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md), [FREETEXT](../../t-sql/queries/freetext-transact-sql.md)ou [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) pour rechercher tous les différents temps et conjugaisons d’un verbe, le pluriel et le singulier d’un nom (recherche des formes fléchies) ou les formes synonymes d’un mot spécifique (recherche dans le dictionnaire des synonymes).  
  
L’exemple suivant recherche toutes les formes de « foot » (« foot », « feet », etc.) dans la colonne `Comments` de la table `ProductReview` de la base de données `AdventureWorks` : 
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT Comments, ReviewerName  
FROM Production.ProductReview  
WHERE CONTAINS (Comments, 'FORMSOF(INFLECTIONAL, "foot")')  
GO  
```  
  
La recherche en texte intégral utilise des *générateurs de formes dérivées*, qui vous permettent de rechercher les différents temps et conjugaisons d’un verbe, ou à la fois le pluriel et le singulier d’un nom. Pour plus d’informations sur les générateurs de formes dérivées, consultez [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  

#### <a name="more-info-about-generation-term-searches"></a>Plus d’informations sur les recherches de termes de génération

Les *formes fléchies* correspondent aux différents temps et conjugaisons d’un verbe et au pluriel et au singulier d’un nom.

Par exemple, rechercher les formes fléchies du mot « drive ». Si plusieurs lignes de la table comportent les mots « drive », « drives », « drove », « driving » et « driven », tous ces termes apparaissent dans le jeu de résultats, dans la mesure où chacun d’entre eux peut être généré à partir du mot « drive ».

[FREETEXT](../../t-sql/queries/freetext-transact-sql.md) et [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) recherchent par défaut des termes fléchis de tous les mots spécifiés. [CONTAINS](../../t-sql/queries/contains-transact-sql.md) et [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) prennent en charge un argument `INFLECTIONAL` facultatif.

### <a name="search-for-synonyms-of-a-specific-word"></a>Rechercher les synonymes d’un mot spécifique

Un *dictionnaire des synonymes* définit des synonymes spécifiés par l’utilisateur pour les termes. Pour plus d’informations sur les fichiers de dictionnaire des synonymes, consultez [Configurer et gérer les fichiers de dictionnaire des synonymes pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md).

Par exemple, si une entrée « {car, automobile, truck, van} » est ajoutée à un dictionnaire des synonymes, vous pouvez rechercher la forme du dictionnaire des synonymes pour le mot « car ». Toutes les lignes de la table interrogée qui contiennent les mots « automobile », « truck », « van » ou « car » apparaissent dans le jeu de résultats, car chacun de ces mots appartient au jeu d’expansion des synonymes contenant le mot « car ».

[FREETEXT](../../t-sql/queries/freetext-transact-sql.md) et [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) utilisent le dictionnaire des synonymes par défaut. [CONTAINS](../../t-sql/queries/contains-transact-sql.md) et [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) prennent en charge un argument `THESAURUS` facultatif.

### <a name="search-for-a-word-near-another-word"></a>Rechercher un mot PROCHE d’un autre mot

Un *terme de proximité* indique des mots ou expressions qui sont proches les uns des autres. Vous pouvez également spécifier le nombre maximal de termes de non-recherche qui séparent le premier et le dernier terme de recherche. De plus, vous pouvez rechercher des mots ou des expressions dans n'importe quel ordre, ou dans l'ordre dans lequel vous les spécifiez.

Par exemple, vous voulez rechercher les lignes dans lesquelles le mot « ice » est proche du mot « hockey » ou celles dans lesquelles l’expression « ice skating » est voisine de « ice hockey ». 

[CONTAINS](../../t-sql/queries/contains-transact-sql.md) et [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md)

Pour plus d’informations sur les recherches de proximité, consultez [Recherche de mots dans le voisinage d’autres mots avec NEAR](search-for-words-close-to-another-word-with-near.md).

###  <a name="Weighted_Term"></a> Recherche de mots et d’expressions à l’aide de valeurs pondérées (termes pondérés)  
Vous pouvez utiliser [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) pour rechercher des mots ou des expressions et spécifier une valeur pondérée. La pondération d'un terme, qui est une mesure numérique comprise entre 0.0 et 1.0, indique l'importance de chaque mot et expression au sein d'un ensemble de mots et d'expressions. L'échelle de pondération oscille entre un minimum de 0.0 et un maximum de 1.0.  
  
L’exemple suivant montre une requête qui recherche toutes les adresses de client en utilisant des pondérations, où un texte qui commence par la chaîne « Bay » comporte aussi « Street » ou « View ». Les résultats accordent un rang plus élevé aux lignes qui contiennent le plus de mots, parmi ceux spécifiés.  
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT AddressLine1, KEY_TBL.RANK   
FROM Person.Address AS Address INNER JOIN  
CONTAINSTABLE(Person.Address, AddressLine1, 'ISABOUT ("Bay*,"   
         Street WEIGHT(0.9),   
         View WEIGHT(0.1)  
         ) ' ) AS KEY_TBL  
ON Address.AddressID = KEY_TBL.[KEY]  
ORDER BY KEY_TBL.RANK DESC  
GO  
```  
  
 Un terme pondéré peut être utilisé avec tout terme simple, terme de préfixe, terme canonique ou terme de proximité.

#### <a name="more-info-about-weighted-term-searches"></a>Plus d’informations sur la recherche de termes pondérés

Dans une recherche de termes pondérés, une *valeur pondérée* indique le degré d’importance de chaque mot et de chaque expression au sein d’un ensemble de mots ou d’expressions. L'échelle de valeurs oscille entre un minimum de 0,0 et un maximum de 1,0.

Par exemple, dans une requête de recherche de plusieurs termes, vous pouvez affecter à chaque mot de recherche une valeur de pondération indiquant son importance par rapport aux autres mots figurant dans la condition de recherche. Les résultats de ce type de requête renvoient les lignes les plus pertinentes en premier, en fonction du poids relatif affecté aux mots de recherche. Les jeux de résultats contiennent des documents ou des lignes qui contiennent chacun des termes spécifiés (ou contenu entre eux) ; toutefois, certains résultats seront considérés comme plus pertinents que d'autres à cause de la variation dans les valeurs pondérées associées aux différents termes recherchés.

Les recherches de termes pondérés sont prises en charge par [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md).

##  <a name="Using_Boolean_Operators"></a> Utiliser AND, OR et NOT (opérateurs booléens)
 
Le prédicat CONTAINS et la fonction CONTAINSTABLE utilisent les mêmes conditions de recherche. Ils permettent tous les deux de combiner plusieurs termes à rechercher via les opérateurs booléens (AND, OR et NOT), pour effectuer des opérations logiques. Par exemple, vous pouvez utiliser AND pour rechercher des lignes qui contiennent à la fois « latte » et « New York-style bagel ». Vous pouvez utiliser AND NOT pour rechercher les lignes qui contiennent « bagel » mais qui ne contiennent pas « cream cheese ».  
  
En revanche, FREETEXT et FREETEXTTABLE traitent les termes booléens comme des mots à rechercher.  
  
 Pour plus d’informations sur la façon de combiner CONTAINS avec d’autres prédicats qui utilisent les opérateurs logiques AND, OR et NOT, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
### <a name="example"></a> Exemple  
 L’exemple suivant utilise le prédicat CONTAINS pour rechercher les descriptions dont l’ID de description n’est pas égal à 5 et la description contient les mots « Aluminum » et « spindle ». La condition de recherche utilise l'opérateur booléen AND. Cet exemple utilise la table ProductDescription de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT Description  
FROM Production.ProductDescription  
WHERE ProductDescriptionID <> 5 AND  
   CONTAINS(Description, 'aluminum AND spindle')  
GO  
```  
  
##  <a name="Additional_Considerations"></a> Casse, mots vides, langue et dictionnaire des synonymes

 Quand vous écrivez des requêtes de texte intégral, vous pouvez aussi spécifier les options suivantes :
  
-   **Respect de la casse**. Les requêtes de recherche en texte intégral ne respectent pas la casse. Néanmoins, en ce qui concerne le Japonais, il existe plusieurs orthographes phonétiques pour lesquelles le concept de normalisation orthographique est apparenté au respect de la casse (par exemple kana = non respect). Ce genre de normalisation orthographique n'est pas pris en charge.  

-   **Mots vides**. Lorsqu'une requête de texte intégral est définie, le Moteur d'indexation et de recherche en texte intégral supprime les mots vides (également appelés mots parasites) des critères de recherche. Les mots vides sont des mots tels que « un », « et », « est » ou « le » dont les occurrences sont fréquentes mais qui ne sont pas utiles pour une recherche de texte spécifique. Les mots vides sont répertoriés dans une liste de mots vides. Chaque index de recherche en texte intégral est associé à une liste de mots vides spécifique, qui détermine les mots vides à omettre de la requête ou de l'index au moment de l'indexation. Pour plus d’informations, consultez [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  

-   **Langue**, avec l’option **LANGUAGE**. De nombreux termes de requête dépendent fortement du comportement de l'analyseur lexical. Pour être certain d'utiliser l'analyseur lexical (et le générateur de formes dérivées) et le dictionnaire des synonymes appropriés, nous vous recommandons de spécifier l'option LANGUAGE. Pour plus d’informations, consultez [Choisir une langue lors de la création d’un index de recherche en texte intégral](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md).  
  
-   **Dictionnaire des synonymes**. Les requêtes FREETEXT et FREETEXTTABLE utilisent le dictionnaire des synonymes par défaut. CONTAINS et CONTAINSTABLE prennent en charge un argument THESAURUS facultatif. Pour plus d’informations, consultez [Configurer et gérer les fichiers de dictionnaire des synonymes pour la recherche en texte intégral](configure-and-manage-thesaurus-files-for-full-text-search.md).
  
##  <a name="tokens"></a> Vérifier les résultats de la création de jetons

Après avoir appliqué une combinaison d’analyseur lexical, de dictionnaire des synonymes et de liste de mots vides dans une requête, vous pouvez voir comment la recherche en texte intégral analyse les résultats avec la vue de gestion dynamique **sys.dm_fts_parser**. Pour plus d’informations, consultez [sys.dm_fts_parser &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [Créer des requêtes de recherche en texte intégral &#40;Visual Database Tools&#41;](http://msdn.microsoft.com/library/537fa556-390e-4c88-9b8e-679848d94abc)   
 [Améliorer les performances des requêtes de texte intégral](../../relational-databases/search/improve-the-performance-of-full-text-queries.md)
 
