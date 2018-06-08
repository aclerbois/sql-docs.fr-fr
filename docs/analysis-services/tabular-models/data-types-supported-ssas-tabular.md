---
title: Types de données pris en charge dans les modèles tabulaires Analysis Services | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 313d3699d6ceb64bd6b96f741b60ea4f20660e3e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-types-supported-in-tabular-models"></a>Types de données pris en charge dans les modèles tabulaires
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Cet article décrit les types de données qui peuvent être utilisés dans des modèles tabulaires et aborde la conversion implicite de types de données lorsque des données sont calculées ou utilisées dans une formule DAX (Data Analysis Expressions).  

  
##  <a name="bkmk_data_types"></a> Types de données utilisés dans les modèles tabulaires  
Lorsque vous importez des données ou utilisez une valeur dans une formule, même si la source de données d'origine contient un type de données différent, les données sont converties dans l'un des types de données suivants. Les valeurs issues des formules utilisent également ces types de données.  
  
 En règle générale, ces types de données sont implémentés pour permettre des calculs exacts dans les colonnes calculées, et les mêmes restrictions s'appliquent au reste des données dans les modèles à des fins de cohérence.  
  
 Les formats utilisés pour les nombres, devises, dates et heures doivent suivre le format que les paramètres régionaux spécifiés sur le client utilisé pour travailler sur les données du modèle. Vous pouvez utiliser les options de mise en forme du modèle pour contrôler la façon dont la valeur s'affiche.  
  
||||  
|-|-|-|  
|**Type de données dans le modèle**|**Type de données dans DAX**|**Description**|  
|Nombre entier|Valeur entière de 64 bits (huit octets)*<br /><br /> Remarque :<br />         Les formules DAX ne prennent pas en charge les types de données qui sont trop petits pour contenir la valeur minimale indiquée dans la description.|Nombres qui n'ont pas de décimales. Les entiers peuvent être des nombres positifs ou négatifs, mais doivent être compris entre -9 223 372 036 854 775 808 (-2^63) et 9 223 372 036 854 775 807 (2^63-1).|  
|Nombre décimal|Nombre réel de 64 bits (huit octets)*<br /><br /> Remarque :<br />         Les formules DAX ne prennent pas en charge les types de données qui sont trop petits pour contenir la valeur minimale indiquée dans la description.|Les nombres réels sont des nombres qui peuvent avoir des décimales. Les nombres réels couvrent une large gamme de valeurs :<br /><br /> Valeurs négatives de -1.79E +308 à -2.23E -308<br /><br /> Zéro<br /><br /> Valeurs positives de 2.23E -308 à -1.79E +308<br /><br /> Toutefois, le nombre de bits significatifs est limité à 17 chiffres décimaux.|  
|Booléen|Booléen|Valeur True ou valeur False.|  
|Texte|Chaîne|Chaîne de données caractères au format Unicode. Peut être des chaînes, des nombres ou dates représentés dans un format texte.|  
|Date|Date/heure|Dates et heures dans une représentation date-heure acceptée.<br /><br /> Les dates valides sont toutes les dates après le 1er mars 1900.|  
|Monétaire (Currency)|Monétaire (Currency)|Le type de données devise autorise des valeurs entre -922 337 203 685 477,5808 et 922 337 203 685 477,5807 avec quatre chiffres décimaux à précision fixe.|  
|Néant|Vide|Le type de données Vide (Blank) de DAX représente et remplace les valeurs Null SQL. Vous pouvez créer une valeur vide à l'aide de la fonction BLANK et tester les valeurs vides à l'aide de la fonction logique ISBLANK.|  
  
 \* Si vous essayez d’importer des données qui ont des valeurs numériques élevées, l’importation peut échouer avec l’erreur suivante :  
  
 Erreur de base de données en mémoire : le '\<nom de la colonne >' colonne de la «\<nom de la table >' table contient une valeur, ' 1.7976931348623157E + 308', qui n’est pas pris en charge. L’opération a été annulée.  
  
 Cette erreur se produit parce que le générateur de modèles utilise cette valeur pour représenter des valeurs Null. Les valeurs de la liste suivante sont des synonymes de la valeur NULL indiquée précédemment :  
  
||  
|-|  
|Valeur|  
|9223372036854775807|  
|-9223372036854775808|  
|1.7976931348623158e+308|  
|2.2250738585072014e-308|  
  
 Supprimer la valeur de vos données, puis réessayez d’importer.  
  
> [!NOTE]  
>  Vous ne pouvez pas importer à partir d’une colonne **varchar(max)** qui contient une chaîne de plus de 131 072 caractères.  
  
### <a name="table-data-type"></a>Type de données table  
 En outre, DAX utilise un type de données *table* . Ce type de données est utilisé par DAX dans de nombreuses fonctions, comme les agrégations et les calculs Time Intelligence. Certaines fonctions requièrent une référence à une table ; d'autres retournent une table qui peut ensuite être utilisée en entrée pour d'autres fonctions. Dans certaines fonctions qui requièrent une table en entrée, vous pouvez spécifier une expression qui donne une table ; pour d'autres, une référence à une table de base est obligatoire. Pour plus d'informations sur les exigences de fonctions spécifiques, voir [Référence des fonctions DAX](http://msdn.microsoft.com/en-us/4dbb28a1-dd1a-4fca-bcd5-e90f74864a7b).  
  
##  <a name="bkmk_implicit"></a> Conversion de type de données implicites et explicites dans les formules DAX
  
 Chaque fonction DAX a des exigences spécifiques en ce qui concerne les types des données utilisés comme entrées et sorties. Par exemple, certaines fonctions requièrent des entiers pour certains arguments et des dates pour les autres ; d'autres fonctions requièrent du texte ou des tables.  
  
 Si les données dans la colonne que vous spécifiez en tant qu’argument sont incompatibles avec le type de données requis par la fonction, dans de nombreux cas DAX retourne une erreur. Toutefois, dans la mesure du possible DAX essaie implicitement convertir les données pour le type de données requis. Par exemple :  
  
-   Vous pouvez taper un nombre, par exemple « 123 », comme une chaîne. DAX analyse la chaîne et tentera pour la spécifier comme un type de données numérique.  
  
-   Vous pouvez ajouter TRUE + 1 et obtenir le résultat 2, parce que TRUE est converti implicitement en nombre 1 et que l'opération 1+1 est effectuée.  
  
-   Si vous additionnez les valeurs de deux colonnes, et qu'une valeur se trouve être représentée par du texte ("12") alors que l'autre est un nombre (12), DAX convertit implicitement la chaîne en nombre, puis fait l'addition pour produire un résultat numérique. L'expression suivante retourne 44 : = "22" + 22  
  
-   Si vous essayez de concaténer deux nombres, ils sont présentés sous forme de chaînes et ensuite concaténés. L'expression suivante retourne  "1234" : = 12 & 34  
  
 Le tableau suivant résume les conversions de types de données implicites effectuées dans les formules. En règle générale, le générateur de modèles sémantiques se comporte comme Microsoft Excel et effectue des conversions implicites chaque fois que possible lorsque nécessaire pour l'opération spécifiée.  
  
### <a name="table-of-implicit-data-conversions"></a>Tableau des conversions de données implicites  
 Le type de conversion effectué est déterminé par l'opérateur, qui convertit les valeurs dont il a besoin avant d'effectuer l'opération demandée. Ces tableaux répertorient les opérateurs et indiquent la conversion effectuée sur chaque type de données dans la colonne lorsqu'il est couplé avec le type de données de la ligne d'intersection.  
  
> [!NOTE]  
>  Les types de données textuels ne sont pas inclus dans ces tableaux. Lorsqu’un nombre est représenté dans un format texte, dans certains cas, le Générateur de modèles tente de déterminer le type de nombre et le représente sous forme de nombre.  
  
#### <a name="addition-"></a>Addition (+)  
  
||||||  
|-|-|-|-|-|  
|Opérateur (+)|INTEGER|Monétaire (Currency)|REAL|Date/heure|  
|INTEGER|INTEGER|Monétaire (Currency)|REAL|Date/heure|  
|Monétaire (Currency)|Monétaire (Currency)|Monétaire (Currency)|REAL|Date/heure|  
|REAL|REAL|REAL|REAL|Date/heure|  
|Date/heure|Date/heure|Date/heure|Date/heure|Date/heure|  
  
 Par exemple, si un nombre réel est utilisé dans une opération d'addition en association avec des données de devise, les deux valeurs sont converties en REAL, et le résultat retourné est de type REAL.  
  
#### <a name="subtraction--"></a>Soustraction (-)  
 Dans le tableau suivant, l’en-tête de ligne est le diminuende (côté gauche) et l’en-tête de colonne est le diminuteur (côté droit) :  
  
||||||  
|-|-|-|-|-|  
|Opérateur (-)|INTEGER|Monétaire (Currency)|REAL|Date/heure|  
|INTEGER|INTEGER|Monétaire (Currency)|REAL|REAL|  
|Monétaire (Currency)|Monétaire (Currency)|Monétaire (Currency)|REAL|REAL|  
|REAL|REAL|REAL|REAL|REAL|  
|Date/heure|Date/heure|Date/heure|Date/heure|Date/heure|  
  
 Par exemple, si une date est utilisée dans une opération de soustraction avec un autre type de données, les deux valeurs sont converties en dates et la valeur de retour est également une date.  
  
> [!NOTE]  
>  Les modèles tabulaires prennent également en charge l'opérateur unaire, - (négatif), mais cet opérateur ne change pas le type de données de l'opérande.  
  
#### <a name="multiplication-"></a>Multiplication (*)  
  
||||||  
|-|-|-|-|-|  
|Opérateur (*)|INTEGER|Monétaire (Currency)|REAL|Date/heure|  
|INTEGER|INTEGER|Monétaire (Currency)|REAL|INTEGER|  
|Monétaire (Currency)|Monétaire (Currency)|REAL|Monétaire (Currency)|Monétaire (Currency)|  
|REAL|REAL|Monétaire (Currency)|REAL|REAL|  
  
 Par exemple, si un entier est combiné avec un nombre réel dans une opération de multiplication, les deux nombres sont convertis en nombres réels, et la valeur de retour est également de type REAL.  
  
#### <a name="division-"></a>Division (/)  
 Dans le tableau suivant l'en-tête de ligne est le numérateur et l'en-tête de colonne est le dénominateur.  
  
||||||  
|-|-|-|-|-|  
|Opérateur (/)<br /><br /> (Ligne/Colonne)|INTEGER|Monétaire (Currency)|REAL|Date/heure|  
|INTEGER|REAL|Monétaire (Currency)|REAL|REAL|  
|Monétaire (Currency)|Monétaire (Currency)|REAL|Monétaire (Currency)|REAL|  
|REAL|REAL|REAL|REAL|REAL|  
|Date/heure|REAL|REAL|REAL|REAL|  
  
 Par exemple, si un entier est combiné avec une valeur de devise dans une opération de division, les deux valeurs sont converties en nombres réels et le résultat est également un nombre réel.  
  
#### <a name="comparison-operators"></a>Opérateurs de comparaison  
Seul un jeu limité de combinaisons de type de données mixte pour les opérations de comparaison est pris en charge. Pour plus d’informations, consultez [Référence des opérateurs DAX](https://msdn.microsoft.com/library/ee634237.aspx).  
  
## <a name="bkmk_hand_blanks"></a> Gestion des espaces, des chaînes vides et des valeurs zéro  
 Le tableau suivant résume les différences entre DAX et Microsoft Excel, dans la façon dont les valeurs vides sont traitées :  
  
||||  
|-|-|-|  
|Expression|DAX|Excel|  
|BLANK + BLANK|Vide|0 (zéro)|  
|BLANK +5|5|5|  
|BLANK * 5|Vide|0 (zéro)|  
|5/BLANK|Infini|Erreur|  
|0/BLANK|NaN|Erreur|  
|BLANK/BLANK|Vide|Erreur|  
|FALSE OR BLANK|FALSE|FALSE|  
|FALSE AND BLANK|FALSE|FALSE|  
|TRUE OR BLANK|TRUE|TRUE|  
|TRUE AND BLANK|FALSE|TRUE|  
|BLANK OR BLANK|Vide|Erreur|  
|BLANK AND BLANK|BLANK|Erreur|  
  
 Pour plus d’informations sur la façon dont une fonction ou un opérateur spécifique gèrent les valeurs vides, voir les rubriques relatives à la fonction DAX concernée dans la section [Référence des fonctions DAX](http://msdn.microsoft.com/en-us/4dbb28a1-dd1a-4fca-bcd5-e90f74864a7b).  
  
