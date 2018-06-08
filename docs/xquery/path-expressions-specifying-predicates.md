---
title: Spécification de prédicats dans une étape d’Expression de chemin d’accès | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.component: xquery
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
dev_langs:
- XML
helpviewer_keywords:
- axis step [XQuery]
- predicates [XQuery]
- qualifiers [XQuery]
- path expressions [XQuery]
ms.assetid: 2660ceca-b8b4-4a1f-98a0-719ad5f89f81
caps.latest.revision: 31
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 83e100d49f09616c429a1dd6b42550beb8bbcfa7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="path-expressions---specifying-predicates"></a>Expressions de chemin d’accès - spécification de prédicats
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Comme décrit dans la rubrique [des Expressions de chemin d’accès dans XQuery](../xquery/path-expressions-xquery.md), une étape d’axe dans une expression de chemin d’accès inclut les composants suivants :  
  
-   [Un axe](../xquery/path-expressions-specifying-axis.md).  
  
-   Un test de nœud. Pour plus d’informations, consultez [spécification de Test de nœud dans une étape d’Expression de chemin d’accès](../xquery/path-expressions-specifying-node-test.md).  
  
-   Zéro ou plusieurs prédicats. Ce paramètre est facultatif.  
  
 Le prédicat facultatif représente la troisième partie de l'étape d'axe dans une expression de chemin d'accès.  
  
## <a name="predicates"></a>Prédicats  
 Un prédicat permet de filtrer une séquence de nœuds en appliquant un test spécifié. L'expression de prédicat est incluse dans un crochet et est liée au dernier nœud d'une expression de chemin d'accès.  
  
 Par exemple, supposons qu’une valeur de paramètre SQL (x) de la **xml** type de données est déclaré, comme indiqué dans l’exemple suivant :  
  
```  
declare @x xml  
set @x = '  
<People>  
  <Person>  
    <Name>John</Name>  
    <Age>24</Age>  
  </Person>  
  <Person>  
    <Name>Goofy</Name>  
    <Age>54</Age>  
  </Person>  
  <Person>  
    <Name>Daffy</Name>  
    <Age>30</Age>  
  </Person>  
</People>  
'  
```  
  
 Dans ce cas, les expressions suivantes sont des expressions valides qui utilisent une valeur de prédicat de [1] à chacun des trois différents niveaux de nœuds :  
  
```  
select @x.query('/People/Person/Name[1]')  
select @x.query('/People/Person[1]/Name')  
select @x.query('/People[1]/Person/Name')  
```  
  
 Notez que dans chaque cas, le prédicat est lié au nœud dans l'expression de chemin d'accès où il est appliqué. Par exemple, la première expression de chemin d'accès sélectionne le premier élément <`Name`> au sein de chaque nœud /People/Person et, avec l'instance XML fournie, retourne ce qui suit :  
  
```  
<Name>John</Name><Name>Goofy</Name><Name>Daffy</Name>  
```  
  
 Toutefois, la deuxième expression de chemin d'accès sélectionne tous les éléments <`Name`> situés sous le premier nœud /People/Person. Par conséquent, il retourne ce qui suit :  
  
```  
<Name>John</Name>  
```  
  
 Des parenthèses peuvent également être utilisées pour modifier l'ordre d'évaluation du prédicat. Par exemple, dans l'expression suivante, un ensemble de parenthèses est utilisé pour séparer le chemin d'accès (/People/Person/Name) du prédicat [1] :  
  
```  
select @x.query('(/People/Person/Name)[1]')  
```  
  
 Dans cet exemple, l'ordre dans lequel le prédicat est appliqué change. Cela est dû au fait que le chemin d'accès entre parenthèses est évalué en premier (/People/Person/Name), puis l'opérateur [1] de prédicat est appliqué à l'ensemble qui contient tous les nœuds qui correspondent au chemin d'accès entre parenthèses. Sans les parenthèses, l'ordre de fonctionnement serait différent, du fait que [1] est appliqué en tant que test de nœud `child::Name`, similaire au premier exemple d'expression de chemin d'accès.  
  
### <a name="quantifiers-and-predicates"></a>Quantificateurs et prédicats  
 Les quantificateurs peuvent être utilisés et ajoutés plusieurs fois au sein des accolades du prédicat lui-même. Par exemple, en utilisant l'exemple précédent, ce qui suit est une utilisation valide de plusieurs quantificateurs au sein d'une sous-expression de prédicat complexe.  
  
```  
select @x.query('/People/Person[contains(Name[1], "J") and xs:integer(Age[1]) < 40]/Name/text()')  
```  
  
 Le résultat d'une expression de prédicat est converti en valeur booléenne et est appelé valeur de vérité du prédicat. Seuls les nœuds de la séquence pour lesquels la valeur de vérité du prédicat est True sont retournés dans le résultat. Tous les autres nœuds sont ignorés.  
  
 Par exemple, l'expression de chemin d'accès ci-dessous inclut un prédicat dans sa deuxième étape :  
  
```  
/child::root/child::Location[attribute::LocationID=10]  
```  
  
 La condition spécifiée par ce prédicat est appliquée à tous les nœuds d'élément <`Location`> enfants. Il en résulte que seuls les ateliers dont l'attribut LocationID a la valeur 10 sont retournés.  
  
 L'expression de chemin d'accès précédente est exécutée dans l'instruction SELECT suivante :  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
 /child::AWMI:root/child::AWMI:Location[attribute::LocationID=10]  
')  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
### <a name="computing-predicate-truth-values"></a>Calcul des valeurs de vérité de prédicat  
 Les règles ci-dessous s'appliquent pour déterminer la valeur de vérité de prédicat, en fonction des spécifications XQuery :  
  
1.  Si la valeur de l'expression de prédicat est une séquence vide, la valeur de vérité de prédicat est False.  
  
     Par exemple :  
  
    ```  
    SELECT Instructions.query('  
    declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
     /child::AWMI:root/child::AWMI:Location[attribute::LotSize]  
    ')  
    FROM Production.ProductModel  
    WHERE ProductModelID=7  
    ```  
  
     L'expression de chemin d'accès dans cette requête retourne uniquement les nœuds d'élément <`Location`> pour lesquels un attribut LotSize est spécifié. Si le prédicat retourne une séquence vide pour un <`Location`> spécifique, cet atelier n'est pas retourné dans le résultat.  
  
2.  Les valeurs peuvent être uniquement xs : Integer, xs : Boolean ou nœud de prédicat\*. Nœud\*, le prédicat prend la valeur True s’il existe des nœuds, et False pour une séquence vide. Tout autre type numérique, tel que le type double et float, génère une erreur de type statique. La valeur de vérité de prédicat d'une expression est True si et seulement si l'entier résultant est égal à la valeur de la position du contexte. En outre, les valeurs littérales entier uniquement et **last()** fonction réduisent la cardinalité de l’expression d’étape filtrée à 1.  
  
     Par exemple, la requête ci-dessous récupère le troisième nœud d'élément enfant de l'élément <`Features`>.  
  
    ```  
    SELECT CatalogDescription.query('  
    declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    declare namespace wm="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
     /child::PD:ProductDescription/child::PD:Features/child::*[3]  
    ')  
    FROM Production.ProductModel  
    WHERE ProductModelID=19  
    ```  
  
     Notez les points suivants dans la requête précédente :  
  
    -   La troisième étape de l'expression spécifie une expression de prédicat de valeur égale à 3. Par conséquent, la valeur de vérité de prédicat de cette expression est True seulement pour les nœuds dont la position de contexte est 3.  
  
    -   La troisième étape spécifie également un caractère générique (*) qui indique tous les nœuds dans le test de nœud. Toutefois, le prédicat filtre les nœuds et retourne uniquement le nœud en troisième position.  
  
    -   La requête retourne le troisième nœud d'élément enfant des éléments enfants <`Features`> des éléments enfants <`ProductDescription`> du document racine.  
  
3.  Si la valeur de l'expression de prédicat est une valeur de type simple de type booléen, la valeur de vérité de prédicat est égale à la valeur de l'expression de prédicat.  
  
     Par exemple, la requête suivante porte sur une **xml**variable de type qui détient une instance XML, l’instance XML d’enquête client. La requête récupère les clients qui ont des enfants. Dans cette requête, qui serait \<HasChildren > 1\</HasChildren >.  
  
    ```  
    declare @x xml  
    set @x='  
    <Survey>  
      <Customer CustomerID="1" >  
      <Age>27</Age>  
      <Income>20000</Income>  
      <HasChildren>1</HasChildren>  
      </Customer>  
      <Customer CustomerID="2" >  
      <Age>27</Age>  
      <Income>20000</Income>  
      <HasChildren>0</HasChildren>  
      </Customer>  
    </Survey>  
    '  
    declare @y xml  
    set @y = @x.query('  
      for $c in /child::Survey/child::Customer[( child::HasChildren[1] cast as xs:boolean ? )]  
      return   
          <CustomerWithChildren>  
              { $c/attribute::CustomerID }  
          </CustomerWithChildren>  
    ')  
    select @y  
    ```  
  
     Notez les points suivants dans la requête précédente :  
  
    -   L’expression dans le **pour** boucle comporte deux étapes, et la seconde étape spécifie un prédicat. La valeur de ce prédicat est une valeur de type booléen. Si cette valeur est True, la valeur de vérité du prédicat est également True.  
  
    -   La requête retourne les <`Customer`> éléments enfants, dont la valeur de prédicat est True, de la \<Survey > éléments enfants de la racine du document. Voici le résultat obtenu :  
  
        ```  
        <CustomerWithChildren CustomerID="1"/>   
        ```  
  
4.  Si la valeur de l'expression de prédicat est une séquence contenant au moins un nœud, la valeur de vérité de prédicat est True.  
  
 Par exemple, la requête suivante récupère ProductModelID pour les modèles de produit dont la description du catalogue XML inclut au moins un composant, un élément enfant de la <`Features`> élément, à partir de l’espace de noms associé à la **wm** préfixe.  
  
```  
SELECT ProductModelID  
FROM   Production.ProductModel  
WHERE CatalogDescription.exist('  
             declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
             declare namespace wm="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
             /child::PD:ProductDescription/child::PD:Features[wm:*]  
             ') = 1  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La clause WHERE spécifie les [méthode exist() (type de données XML)](../t-sql/xml/exist-method-xml-data-type.md).  
  
-   L’expression de chemin d’accès à l’intérieur de la **exist()** méthode spécifie un prédicat dans la deuxième étape. Si l'expression de prédicat retourne une séquence contenant au moins une fonctionnalité, la valeur de vérité de cette expression de prédicat a la valeur True. Dans ce cas, étant donné que la **exist()** méthode retourne la valeur True, le ProductModelID est retourné.  
  
## <a name="static-typing-and-predicate-filters"></a>Déduction de type statique et filtres de prédicat  
 Les prédicats peuvent également affecter le type déduit statiquement d'une expression. Valeurs littérales entières et la **last()** fonction réduisent la cardinalité de l’expression d’étape filtrée pour au plus un.  
  
  
