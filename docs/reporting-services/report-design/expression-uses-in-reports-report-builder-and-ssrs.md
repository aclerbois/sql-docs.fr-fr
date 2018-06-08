---
title: Utilisation d’expressions dans les rapports (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- expressions [Reporting Services], about expressions
ms.assetid: 76b9ed31-5aec-40fc-bb88-a1c1b0ab3fc3
caps.latest.revision: 59
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: d3f8432e1e1ce62e37d2f875eb071db76afd685f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="expression-uses-in-reports-report-builder-and-ssrs"></a>Utilisation d'expressions dans les rapports (Générateur de rapport et SSRS)
Dans les rapports paginés [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , les expressions sont utilisées dans la définition des rapports pour spécifier ou calculer les valeurs des paramètres, requêtes, filtres, propriétés d’éléments de rapport, définitions de groupe et de tri, propriétés de zone de texte, signets, explorateurs de documents, contenu d’en-tête et de pied de page dynamique, images et définitions de source de données dynamiques. Cette rubrique fournit des exemples des nombreux emplacements où vous pouvez utiliser des expressions pour varier le contenu ou l'apparence d'un rapport. Cette liste n'est pas exhaustive. Vous pouvez définir une expression pour toute propriété dans une boîte de dialogue qui affiche le bouton d’expression (**fx**) ou dans une liste déroulante qui affiche **\<Expression...>**.  
  
 Les expressions peuvent être simples ou complexes. Les*expressions simples* contiennent une référence à un champ de dataset, paramètre ou champ intégré unique. Les expressions complexes peuvent contenir plusieurs références intégrées, opérateurs et appels de fonction. Par exemple, une expression complexe peut inclure la fonction Sum appliquée au champ Sales.  
  
 Les expressions sont écrites en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Une expression commence par un signe égal (=) suivi d'une combinaison de références à des collections intégrées, telles que des paramètres et champs de dataset, constantes, fonctions et opérateurs.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="Simple"></a> Utilisation d'expressions simples  
 Les expressions simples apparaissent entre crochets sur l'aire de conception et dans les boîtes de dialogue ; par exemple, un champ de dataset apparaît comme `[ProductID]`. Elles sont créées automatiquement lorsque vous faites glisser un champ d'un dataset vers une zone de texte. Un espace réservé est créé et l'expression définit la valeur sous-jacente. Vous pouvez également taper les expressions directement dans une cellule de région de données ou zone de texte sur l'aire de conception ou dans une boîte de dialogue (par exemple, `[ProductID]`).  
  
 Le tableau suivant répertorie des exemples d'utilisations des expressions simples. Le tableau décrit la fonctionnalité, la propriété à définir, la boîte de dialogue utilisée en général pour la définir et la valeur de la propriété. Vous pouvez taper l'expression simple directement sur l'aire de conception, dans une boîte de dialogue ou dans le volet Propriétés, ou vous pouvez la modifier dans la boîte de dialogue Expression, comme vous le feriez avec toute expression.  
  
|Fonctionnalité|Propriété, contexte et boîte de dialogue|Valeur de propriété|  
|-------------------|---------------------------------------|--------------------|  
|Spécifier un champ de dataset à afficher dans une zone de texte.|Propriété Value d’un espace réservé à l’intérieur d’une zone de texte. Utilisez la **boîte de dialogue Propriétés de l'espace réservé, Général**.|`[Sales]`|  
|Agréger des valeurs pour un groupe.|Propriété Value d’un espace réservé à l’intérieur d’une ligne associée à un groupe de tableaux matriciels. Utilisez la **boîte de dialogue Propriétés de la zone de texte**.|`[Sum(Sales)]`|  
|Inclure un numéro de page.|Propriété Value d’un espace réservé à l’intérieur d’une zone de texte placée dans un en-tête de page. Utilisez la **boîte de dialogue Propriétés de la zone de texte, Général**.|`[&PageNumber]`|  
|Afficher une valeur de paramètre sélectionnée.|Propriété Value d’un espace réservé à l’intérieur d’une zone de texte dans l’aire de conception. Utilisez la **boîte de dialogue Propriétés de la zone de texte, Général**.|`[@SalesThreshold]`|  
|Spécifier une définition de groupe pour une région de données.|Expression de groupe sur le groupe de tableaux matriciels. Utilisez la **boîte de dialogue Propriétés du groupe de tableaux matriciels, Général**.|`[Category]`|  
|Exclure une valeur de champ spécifique d'une table.|Équation de filtre sur le tableau matriciel. Utilisez la **boîte de dialogue Propriétés du tableau matriciel, Filtres**.|Pour le type de données, sélectionnez **Entier**.<br /><br /> `[Quantity]`<br /><br /> `>`<br /><br /> `100`|  
|Inclure uniquement une valeur spécifique pour un filtre de groupe.|Équation de filtre sur le groupe de tableaux matriciels. Utilisez la **boîte de dialogue Propriétés du groupe de tableaux matriciels, Filtres**.|`[Category]`<br /><br /> `=`<br /><br /> `Clothing`|  
|Exclure des valeurs spécifiques pour plusieurs champs d'un dataset.|Équation de filtre pour un groupe dans un tableau matriciel. Utilisez la **boîte de dialogue Propriétés du tableau matriciel, Filtres**.|`=[Color]`<br /><br /> `<>`<br /><br /> `Red`<br /><br /> `=[Color]`<br /><br /> `<>`<br /><br /> `Blue`|  
|Spécifier l'ordre de tri selon un champ existant dans une table.|Expression de tri sur le tableau matriciel. Utilisez la **boîte de dialogue Propriétés du tableau matriciel, Tri**.|`[SizeSortOrder]`|  
|Lier un paramètre de requête à un paramètre de rapport.|Collection de paramètres sur le dataset. Utilisez la **boîte de dialogue Propriétés du dataset, Paramètres**.|`[@Category]`<br /><br /> `[@Category]`|  
|Transmettre un paramètre d'un rapport principal à un sous-rapport.|Collection de paramètres sur le sous-rapport. Utilisez la **boîte de dialogue Propriétés du sous-rapport, Paramètres**.|`[@Category]`<br /><br /> `[@Category]`|  
  
##  <a name="Complex"></a> Utilisation d'expressions complexes  
 Les expressions complexes peuvent contenir plusieurs références intégrées, opérateurs et appels de fonction, et apparaissent dans l’aire de conception sous la forme `<<Expr>>`. Pour voir ou modifier le texte de l'expression, vous devez ouvrir la boîte de dialogue **Expression** ou taper directement dans le volet Propriétés. Le tableau suivant répertorie les utilisations classiques d'une expression complexe pour afficher ou organiser des données, ou modifier l'apparence du rapport, y compris la propriété à définir, la boîte de dialogue utilisée en général pour la définir et la valeur de la propriété. Vous pouvez taper une expression directement dans une boîte de dialogue, sur l'aire de conception ou dans le volet Propriétés.  
  
|Fonctionnalité|Propriété, contexte et boîte de dialogue|Valeur de propriété|  
|-------------------|---------------------------------------|--------------------|  
|Calculer les valeurs d'agrégat pour un dataset.|Propriété Value d’un espace réservé à l’intérieur d’une zone de texte. Utilisez la **boîte de dialogue Propriétés de l'espace réservé, Général**.|`=First(Fields!Sales.Value,"DataSet1")`|  
|Concaténer du texte et des expressions dans la même zone de texte.|Propriété Value d’un espace réservé à l’intérieur d’une zone de texte placée dans un en-tête ou un pied de page. Utilisez la **boîte de dialogue Propriétés de l'espace réservé, Général**.|`="This report began processing at " & Globals!ExecutionTime`|  
|Calculer une valeur d'agrégat pour un dataset dans une étendue différente.|Propriété Value d’un espace réservé à l’intérieur d’une zone de texte placée dans un groupe de tableaux matriciels. Utilisez la **boîte de dialogue Propriétés de l'espace réservé, Général**.|`=Max(Fields!Total.Value,"DataSet2)`|  
|Mettre en forme des données dans une zone de texte selon la valeur.|Propriété Color d’un espace réservé à l’intérieur d’une zone de texte sur la ligne Détails d’un tableau matriciel. Utilisez la **boîte de dialogue Propriétés de la zone de texte, Police**.|`=IIF(Fields!TotalDue.Value < 10000,"Red","Black")`|  
|Calculer une valeur une fois pour s'y reporter dans tout le rapport.|Propriété Value d’une variable de rapport. Utilisez la **boîte de dialogue Propriétés du rapport, Variables**.|`=Variables!MyCalculation.Value`|  
|Inclure des valeurs spécifiques pour plusieurs champs d'un dataset.|Équation de filtre pour un groupe dans un tableau matriciel. Utilisez la **boîte de dialogue Propriétés du tableau matriciel, Filtres**.|Pour le type de données, sélectionnez **Booléen**.<br /><br /> `=IIF(InStr(Fields!Subcat.Value,"Shorts")=0 AND (Fields!Size.Value="M" OR Fields!Size.Value="S"),TRUE, FALSE)`<br /><br /> `=`<br /><br /> `TRUE`|  
|Masquer une zone de texte sur l'aire de conception, qui peut être activée ou désactivée par l'utilisateur à l'aide d'un paramètre booléen nommé *Show*.|Propriété masquée dans une zone de texte. Utilisez la **boîte de dialogue Propriétés de la zone de texte, Visibilité**.|`=Not Parameters!` *Afficher\<paramètre_booléen>* `.Value`|  
|Spécifier un contenu dynamique de l'en-tête de page ou du pied de page.|Valeur d’un espace réservé à l’intérieur d’une zone de texte placée dans l’en-tête ou le pied de page.|`="Page " & Globals!PageNumber & " of "  & Globals!TotalPages`|  
|Spécifier une source de données de façon dynamique à l'aide d'un paramètre.|Chaîne de connexion sur la source de données. Utilisez la **boîte de dialogue Propriétés de la source de données, Général**.|`="Data Source=" & Parameters!ServerName.Value & ";initial catalog=AdventureWorks2012"`|  
|Identifier toutes les valeurs pour un paramètre à valeurs multiples choisi par l'utilisateur.|Valeur d’un espace réservé à l’intérieur d’une zone de texte. Utilisez la **boîte de dialogue Propriétés du tableau matriciel, Filtres**.|`=Join(Parameters!MyMultivalueParameter.Value,", ")`|  
|Spécifier des sauts de page toutes les 20 lignes dans un tableau matriciel sans aucun autre groupe.|Expression de groupe pour un groupe dans un tableau matriciel. Utilisez la **boîte de dialogue Propriétés du groupe de tableaux matriciels, Sauts de page**. Sélectionnez l'option **Entre chaque instance d'un groupe**.|`=Ceiling(RowNumber(Nothing)/20)`|  
|Spécifier une visibilité conditionnelle basée sur un paramètre.|Propriété masquée d’un tableau matriciel. Utilisez la **boîte de dialogue Propriétés du tableau matriciel, Visibilité**.|`=Not Parameters!<` *boolean parameter* `>.Value`|  
|Spécifier une date mise en forme pour une culture spécifique.|Valeur d’un espace réservé à l’intérieur d’une zone de texte dans une région de données. Utilisez la **boîte de dialogue Propriétés de la zone de texte, Général**.|`=Fields!OrderDate.Value.ToString(System.Globalization.CultureInfo.CreateSpecificCulture("de-DE"))`|  
|Concaténer une chaîne et un nombre mis en forme comme un pourcentage à deux décimales.|Valeur d’un espace réservé à l’intérieur d’une zone de texte dans une région de données. Utilisez la **boîte de dialogue Propriétés de la zone de texte, Général**.|`="Growth Percent: " & Format(Fields!Growth.Value,"p2")`|  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [En-têtes et pieds de page &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/page-headers-and-footers-report-builder-and-ssrs.md)   
 [Mise en forme du texte et des espaces réservés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md)   
 [Masquer un élément &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/hide-an-item-report-builder-and-ssrs.md)  
  
  
