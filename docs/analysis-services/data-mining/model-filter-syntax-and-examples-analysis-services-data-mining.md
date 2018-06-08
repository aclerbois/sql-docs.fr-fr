---
title: Syntaxe de filtre et des exemples de modèle (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 467d3efbe979bf2ea58c700409913ef0767457ab
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="model-filter-syntax-and-examples-analysis-services---data-mining"></a>Syntaxe de filtre de modèle et exemples (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Cette section fournit des informations détaillées sur la syntaxe des filtres de modèle, avec quelques exemples d'expressions.  
  
 [Syntaxe de filtre](#bkmk_Syntax)  
  
 [Filtres sur les attributs de cas](#bkmk_Ex1)  
  
 [Filtres sur les attributs de tables imbriquées](#bkmk_Ex2)  
  
 [Filtres sur plusieurs attributs de tables imbriquées](#bkmk_Ex3)  
  
 [Filtre sur les attributs absents de la table imbriquée](#bkmk_Ex4)  
  
 [Filtres sur plusieurs valeurs de tables imbriquées](#bkmk_Ex5)  
  
 [Filtres sur les attributs de tables imbriquées et EXISTS](#bkmk_Ex6)  
  
 [Combinaisons de filtres](#bkmk_Ex7)  
  
 [Filtres sur les dates](#bkmk_Ex8)  
  
##  <a name="bkmk_Syntax"></a> Filter Syntax  
 Les expressions de filtre sont généralement équivalentes au contenu d'une clause WHERE. Vous pouvez connecter plusieurs conditions à l'aide des opérateurs logiques **AND**, **OR**et **NOT**.  
  
 Dans les tables imbriquées, vous pouvez également utiliser les opérateurs **EXISTS** et **NOT EXISTS** . Une condition **EXISTS** est évaluée à **true** si la sous-requête retourne au moins une ligne. Ceci est utile dans les cas où vous souhaitez restreindre le modèle aux cas qui contiennent une valeur particulière dans la table imbriquée : par exemple, les clients qui ont acheté un élément au moins une fois.  
  
 Une condition **NOT EXISTS** est évaluée à **true** si la condition spécifiée dans la sous-requête n'existe pas. Un exemple est lorsque vous souhaitez restreindre le modèle aux clients qui n'ont jamais acheté un élément particulier.  
  
 La syntaxe générale est la suivante :  
  
```  
<filter>::=<predicate list>  | ( <predicate list> )  
<predicate list>::= <predicate> | [<logical_operator> <predicate list>]   
<logical_operator::= AND| OR  
<predicate>::= NOT <predicate>|( <predicate> ) <avPredicate> | <nestedTablePredicate> | ( <predicate> )   
<avPredicate>::= <columnName> <operator> <scalar> | <columnName> IS [NOT] NULL  
<operator>::= = | != | <> | > | >= | < | <=  
<nestedTablePredicate>::= EXISTS (<subquery>)  
<subquery>::=SELECT * FROM <columnName>[ WHERE  <predicate list> ]  
```  
  
 *Filter*  
 Contient un ou plusieurs prédicats, connectés par des opérateurs logiques.  
  
 *predicate list*  
 Une ou plusieurs expressions de filtre valides, séparées par des opérateurs logiques.  
  
 *columnName*  
 Nom d'une colonne de structure d'exploration de données.  
  
 opérateur logique  
 **AND**, **OR**, **NOT**  
  
 *avPredicate*  
 Expression de filtre qui peut s'appliquer uniquement à une colonne de structure d'exploration de données scalaire. Une expression *avPredicate* peut être utilisée dans des filtres de modèle ou des filtres de table imbriquée.  
  
 Une expression qui utilise l'un des opérateurs suivants peut être appliquée uniquement à une colonne continue. :  
  
-   **\<** (inférieur à)  
  
-   **>** (supérieur à)  
  
-   **>=** (supérieur ou égal à)  
  
-   **\<=** (inférieur ou égal à)  
  
> [!NOTE]  
>  Quel que soit le type de données, ces opérateurs ne peuvent pas être appliqués à une colonne qui a le type **Discrete**, **Discretized** **Key**.  
  
 Une expression qui utilise l'un des opérateurs suivants peut être appliquée uniquement à une colonne continue, discrète, discrétisée ou clé :  
  
-   **=** (égal à)  
  
-   **!=** (différent de)  
  
-   **IS NULL**  
  
 Si l'argument, *avPredicate*, s'applique à une colonne discrétisée, la valeur utilisée dans le filtre peut être toute valeur dans un compartiment spécifique.  
  
 En d’autres termes, vous ne définissez pas la condition comme `AgeDisc = ’25-35’`; au lieu de cela, vous calculez et utilisez une valeur à partir de cet intervalle.  
  
 Exemple :  `AgeDisc = 27`  signifie toute valeur dans le même intervalle que 27, ce qui dans ce cas correspond à 25-35.  
  
 *nestedTablePredicate*  
 Expression de filtre qui s'applique à une table imbriquée. Peut être utilisée uniquement dans les filtres de modèle.  
  
 L’argument de sous-requête de l’argument, *nestedTablePredicate*, peut s’appliquer uniquement à une colonne de structure d’exploration de données de table.  
  
 sous-requête  
 Instruction SELECT suivie d'un prédicat ou d'une liste de prédicats valide.  
  
 Tous les prédicats doivent être du type décrit dans *avPredicates*. En outre, les prédicats peuvent faire référence uniquement à des colonnes incluses dans la table imbriquée actuelle, identifiées par l'argument, *columnName*.  
  
### <a name="limitations-on-filter-syntax"></a>Limitations relatives à la syntaxe de filtre  
 Les restrictions suivantes s'appliquent aux filtres :  
  
-   Un filtre peut contenir uniquement des prédicats simples. Cela comprend les opérateurs mathématiques, les scalaires et les noms de colonnes.  
  
-   Les fonctions définies par l'utilisateur ne sont pas prises en charge dans la syntaxe de filtre.  
  
-   Les opérateurs non booléens, tels que les signes plus ou moins, ne sont pas pris en charge dans la syntaxe de filtre.  
  
## <a name="examples-of-filters"></a>Exemples de filtres  
 Les exemples suivants montrent l'utilisation de filtres appliqués à un modèle d'exploration de données. Si vous créez l'expression de filtre à l'aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], dans la fenêtre **Propriété** et le volet **Expression** de la boîte de dialogue de filtre, vous obtiendriez uniquement la chaîne qui apparaît après les mots clés WITH FILTER. Ici, la définition de la structure d'exploration de données est incluse afin de simplifier la compréhension du type et de l'utilisation de colonne.  
  
###  <a name="bkmk_Ex1"></a> Exemple 1 : Filtrage par défaut au niveau du cas  
 Cet exemple montre un filtre simple qui restreint les cas utilisés dans le modèle aux clients de plus de trente ans exerçant la profession (occupation) d'architecte.  
  
```  
ALTER MINING STRUCTURE MyStructure  ADD MINING MODEL MyModel_1  
(  
CustomerId,  
Age,  
Occupation,  
MaritalStatus PREDICT  
)  
WITH FILTER (Age > 30 AND Occupation=’Architect’)  
```  
  
  
###  <a name="bkmk_Ex2"></a> Exemple 2 : Filtrage au niveau du cas à l'aide d'attributs de tables imbriquées  
 Si votre structure d'exploration de données contient des tables imbriquées, vous pouvez filtrer sur l'existence d'une valeur dans une table imbriquée ou filtrer sur des lignes de table imbriquée qui contiennent une valeur spécifique. Cet exemple restreint les cas utilisés pour le modèle aux clients dont l'âge est supérieur à 30 et qui ont effectué au moins un achat incluant du lait.  
  
 Comme le montre cet exemple, il n'est pas nécessaire que le filtre utilise uniquement des colonnes incluses dans le modèle. La table imbriquée **Products** fait partie de la structure d'exploration de données, mais elle n'est pas incluse dans le modèle d'exploration de données. Toutefois, vous pouvez toujours filtrer sur des valeurs et des attributs dans la table imbriquée. Pour afficher les détails de ces cas, l'extraction doit être activée.  
  
```  
ALTER MINING STRUCTURE MyStructure  ADD MINING MODEL MyModel_2  
(  
CustomerId,  
Age,  
Occupation,  
MaritalStatus PREDICT  
)  
WITH DRILLTHROUGH,   
FILTER (Age > 30 AND EXISTS (SELECT * FROM Products WHERE ProductName=’Milk’)  
)  
```  
  
  
###  <a name="bkmk_Ex3"></a> Exemple 3 : Filtrage au niveau du cas sur plusieurs attributs de tables imbriquées  
 Cet exemple montre un filtre en trois parties : une condition s'applique à la table de cas, une autre condition à un attribut dans la table imbriquée, et une autre condition sur une valeur spécifique dans l'une des colonnes de table imbriquée.  
  
 La première condition du filtre, `Age > 30`, s'applique à une colonne dans la table de cas. Les conditions restantes s'appliquent à la table imbriquée.  
  
 La deuxième condition, `EXISTS (SELECT * FROM Products WHERE ProductName=’Milk’`, vérifie la présence d’au moins un achat dans la table imbriquée incluant du lait. La troisième condition, `Quantity>=2`, signifie que le client doit avoir acheté au moins deux unités de lait dans une transaction unique.  
  
```  
ALTER MINING STRUCTURE MyStructure  ADD MINING MODEL MyModel_3  
(  
CustomerId,  
Age,  
Occupation,  
MaritalStatus PREDICT,  
Products PREDICT  
(  
ProductName KEY,  
Quantity        
)  
)  
FILTER (Age > 30 AND EXISTS (SELECT * FROM Products WHERE ProductName=’Milk’  AND Quantity >= 2)   
)  
```  
  
  
###  <a name="bkmk_Ex4"></a> Exemple 4 : Filtrage au niveau du cas en l'absence d'attributs de tables imbriquées  
 Cet exemple indique comment limiter les cas aux clients qui n'ont pas acheté d'élément spécifique, en filtrant sur l'absence d'un attribut dans la table imbriquée. Dans cet exemple, l'apprentissage du modèle s'effectue à l'aide de clients dont l'âge est supérieur à 30 et qui n'ont jamais acheté de lait.  
  
```  
ALTER MINING STRUCTURE MyStructure  ADD MINING MODEL MyModel_4  
(  
CustomerId,  
Age,  
Occupation,  
MaritalStatus PREDICT,  
Products PREDICT  
(  
ProductName  
)  
)  
FILTER (Age > 30 AND NOT EXISTS (SELECT * FROM Products WHERE ProductName=’Milk’) )  
```  
  
  
###  <a name="bkmk_Ex5"></a> Exemple 5 : Filtrage sur plusieurs valeurs de tables imbriquées  
 Le but de l'exemple est d'illustrer le filtrage de table imbriquée. Le filtre de table imbriquée est appliqué après le filtre de cas et il restreint uniquement des lignes de table imbriquée.  
  
 Ce modèle pourrait contenir plusieurs cas avec des tables imbriquées vides car EXISTS n'est pas spécifié.  
  
```  
ALTER MINING STRUCTURE MyStructure  ADD MINING MODEL MyModel_5  
(  
CustomerId,  
Age,  
Occupation,  
MaritalStatus PREDICT,  
Products PREDICT  
(  
ProductName KEY,  
Quantity        
) WITH FILTER(ProductName=’Milk’ OR ProductName=’bottled water’)  
)  
WITH DRILLTHROUGH  
```  
  
  
###  <a name="bkmk_Ex6"></a> Exemple 6 : Filtrage sur des attributs de tables imbriquées et EXISTS  
 Dans cet exemple, le filtre sur la table imbriquée restreint les lignes à celles qui contiennent du lait ou de l'eau en bouteille. Ensuite, les cas dans le modèle sont restreints à l'aide d'une instruction **EXISTS** . Cela permet de s'assurer que la table imbriquée n'est pas vide.  
  
```  
ALTER MINING STRUCTURE MyStructure  ADD MINING MODEL MyModel_6  
(  
CustomerId,  
Age,  
Occupation,  
MaritalStatus PREDICT,  
Products PREDICT  
(  
ProductName KEY,  
Quantity        
) WITH FILTER(ProductName=’Milk’ OR ProductName=’bottled water’)  
)  
FILTER (EXISTS (Products))  
```  
  
  
###  <a name="bkmk_Ex7"></a> Exemple 7 : Combinaisons de filtres complexes  
 Le scénario de ce modèle ressemble à celui de l'Exemple 4, mais il est beaucoup plus complexe. La table imbriquée, **ProductsOnSale**, a la condition de filtre `(OnSale)` , ce qui signifie que la valeur de **OnSale** doit être **true** pour le produit répertorié dans **ProductName**. Ici, **OnSale** est une colonne de structure.  
  
 La deuxième partie du filtre, pour **ProductsNotOnSale**, répète cette syntaxe, mais filtre les produits pour lesquels la valeur de **OnSale** est **not true**`(!OnSale)`.  
  
 Pour finir, les conditions sont combinées et une restriction supplémentaire est ajoutée à la table de cas. Le résultat est la prédiction des achats de produits dans la liste **ProductsNotOnSale** , en fonction des cas inclus dans la liste **ProductsOnSale** , pour tous les clients dont l'âge est supérieur à 25.  
  
 `ALTER MINING STRUCTURE MyStructure  ADD MINING MODEL MyModel_7`  
  
 `(`  
  
 `CustomerId,`  
  
 `Age,`  
  
 `Occupation,`  
  
 `MaritalStatus,`  
  
 `ProductsOnSale`  
  
 `(`  
  
 `ProductName KEY`  
  
 `) WITH FILTER(OnSale),`  
  
 `ProductsNotOnSale PREDICT ONLY`  
  
 `(`  
  
 `ProductName KEY`  
  
 `) WITH FILTER(!OnSale)`  
  
 `)`  
  
 `WITH DRILLTHROUGH,`  
  
 `FILTER (EXISTS (ProductsOnSale) AND EXISTS(ProductsNotOnSale) AND Age > 25)`  
  
  
###  <a name="bkmk_Ex8"></a> Exemple 8 : Filtrage sur les dates  
 Vous pouvez filtrer des colonnes d'entrée sur les dates, comme vous le feriez avec d'autres données. Les dates contenues dans une colonne de type date/heure sont des valeurs continues. Vous pouvez par conséquent spécifier une plage de dates en utilisant des opérateurs tels que supérieur à (>) ou inférieur à (<). Si votre source de données ne représente pas les dates par un type de données Continuous, mais comme des valeurs texte ou discrètes, vous ne pouvez pas filtrer sur une plage de dates ; vous devez spécifier des valeurs discrètes individuelles.  
  
 Toutefois, vous ne pouvez pas créer de filtre sur la colonne de date d'un modèle de série chronologique si cette colonne est aussi la colonne clé du modèle. En effet, dans les modèles de série chronologique et les modèles Sequence Clustering, la colonne de date peut être gérée en tant que type **KeyTime** ou **KeySequence**.  
  
 Si vous devez filtrer sur des dates continues dans un modèle de série chronologique, vous pouvez créer une copie de la colonne dans la structure d'exploration de données et filtrer le modèle sur la nouvelle colonne.  
  
 Par exemple, l’expression suivante représente un filtre sur une colonne de date de type **Continuous** qui a été ajoutée au modèle de prévision.  
  
 `=[DateCopy] > '12:31:2003:00:00:00'`  
  
> [!NOTE]  
>  Notez que toutes les colonnes que vous ajoutez au modèle peuvent affecter les résultats. Par conséquent, si vous ne souhaitez pas que la colonne soit utilisée dans le calcul de la série, vous devez ajouter la colonne uniquement à la structure d'exploration de données et non au modèle. Vous pouvez également définir **PredictOnly** ou **Ignore**pour l'indicateur de modèle de la colonne. Pour plus d’informations, consultez [Indicateurs de modélisation &#40;Exploration de données&#41;](../../analysis-services/data-mining/modeling-flags-data-mining.md).  
  
 Pour les autres types de modèles, vous pouvez comme dans toute autre colonne utiliser des dates en tant que critères d'entrée ou de filtre. Toutefois, si vous devez utiliser un niveau spécifique de granularité, non pris en charge par un type de données **Continuous** , vous pouvez créer une valeur dérivée dans la source de données en utilisant des expressions afin d'extraire l'unité à utiliser pour le filtrage et l'analyse.  
  
> [!WARNING]  
>  Lorsque vous spécifiez des dates comme critères de filtre, vous devez utiliser le format suivant, indépendamment du format de date du système d'exploitation actuel : `mm/dd/yyyy`. Tout autre format provoque une erreur.  
  
 Par exemple, si vous souhaitez filtrer les résultats de votre centre d'appels de façon à n'afficher que les week-ends, vous pouvez, dans la vue de source de données, créer une expression qui extrait le nom du jour de la semaine correspondant à chaque date, puis utiliser ce nom comme valeur d'entrée ou comme valeur discrète pour le filtrage. Rappelez-vous simplement que des valeurs répétées peuvent affecter le modèle, de sorte qu'il est préférable de n'utiliser que l'une des colonnes, et non la colonne de date plus la valeur dérivée.  
  
  
## <a name="see-also"></a>Voir aussi  
 [Filtres pour les modèles d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md)   
 [Test et Validation & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-data-mining.md)  
  
  
