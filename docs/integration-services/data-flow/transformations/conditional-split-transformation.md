---
title: Fractionnement conditionnel, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.conditionalsplittrans.f1
- sql13.dts.designer.conditionalsplittransformation.f1
helpviewer_keywords:
- Conditional Split transformation
- route rows to different outputs [Integration Services]
ms.assetid: 3f8b5825-226f-413c-ba8f-0bb931ca3770
caps.latest.revision: 51
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fbb3488bcd17087caf11096a788312fdfa147df7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="conditional-split-transformation"></a>transformation de fractionnement conditionnel
  La transformation de fractionnement conditionnel peut aiguiller les lignes de données vers différentes sorties, suivant le contenu des données. La mise en œuvre de la transformation de fractionnement conditionnel s'apparente à une structure de décision CASE dans un langage de programmation. La transformation évalue les expressions puis, sur la base des résultats, dirige la ligne de données vers la sortie spécifiée. Cette transformation offre également une sortie par défaut, vers laquelle sont dirigées les lignes qui ne correspondent à aucune expression.  
  
## <a name="configuration-of-the-conditional-split-transformation"></a>Configuration de la transformation de fractionnement conditionnel  
 Vous pouvez configurer la transformation de fractionnement conditionnel comme suit :  
  
-   Indiquez une expression renvoyant une valeur booléenne pour chaque condition que la transformation doit tester.  
  
-   Spécifiez l'ordre dans lequel les conditions sont évaluées. L'ordre est significatif car une ligne est envoyée à la sortie correspondant à la première condition qui renvoie True.  
  
-   Spécifiez la sortie par défaut de la transformation. Il est nécessaire de spécifier une sortie par défaut pour la transformation.  
  
 Chaque ligne d'entrée ne peut être envoyée qu'à une sortie, en l'occurrence celle correspondant à la première condition qui renvoie True. Par exemple, les conditions suivantes dirigent toutes les lignes de la colonne **FirstName** commençant par la lettre *A* vers une sortie, celles commençant par la lettre *B* vers une autre sortie et toutes les autres vers la sortie par défaut.  
  
 Sortie 1  
  
 `SUBSTRING(FirstName,1,1) == "A"`  
  
 Sortie 2  
  
 `SUBSTRING(FirstName,1,1) == "B"`  
  
 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] comprend des fonctions et des opérateurs permettant de créer les expressions qui évaluent les données d’entrée et dirigent les données de sortie. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md).  
  
 La transformation de fractionnement conditionnel inclut la propriété personnalisée **FriendlyExpression**. La propriété peut être mise à jour par une expression de propriété lors du chargement du package. Pour plus d’informations, consultez [Expressions de propriété dans des packages](../../../integration-services/expressions/use-property-expressions-in-packages.md) et [Propriétés personnalisées des transformation](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
 Cette transformation possède une entrée, une ou plusieurs sorties et une sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d'informations sur la définition des propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   [Fractionner un dataset à l'aide de la transformation de fractionnement conditionnel](../../../integration-services/data-flow/transformations/split-a-dataset-by-using-the-conditional-split-transformation.md)  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 [Fractionner un dataset à l'aide de la transformation de fractionnement conditionnel](../../../integration-services/data-flow/transformations/split-a-dataset-by-using-the-conditional-split-transformation.md)  
  
## <a name="conditional-split-transformation-editor"></a>Éditeur de transformation de fractionnement conditionnel
  Utilisez la boîte de dialogue **Éditeur de transformation de fractionnement conditionnel** pour créer des expressions, définir l'ordre dans lequel les expressions sont évaluées et nommer les sorties d'un fractionnement conditionnel. Cette boîte de dialogue comprend des fonctions mathématiques, de chaînes de caractères et de date/heure, ainsi que des opérateurs utilisés pour créer des expressions. La première condition évaluée comme vraie détermine la sortie vers laquelle une ligne est dirigée.  
  
> [!NOTE]  
>  La transformation de fractionnement conditionnel dirige chaque ligne d'entrée vers une seule sortie. Si vous entrez plusieurs conditions, la transformation envoie chaque ligne à la première sortie pour laquelle la condition est remplie et ne tient pas compte des conditions suivantes pour cette ligne. Si vous devez évaluer successivement plusieurs conditions, vous devrez peut-être enchaîner plusieurs transformations de fractionnement conditionnel dans le flux de données.  
  
### <a name="options"></a>Options  
 **JSON**  
 Sélectionnez une ligne et utilisez les touches de direction à droite pour modifier l'ordre dans lequel les expressions sont évaluées.  
  
 **Nom de sortie**  
 Donnez un nom à la sortie. Par défaut, il s'agit d'une liste de cas numérotée ; vous pouvez néanmoins choisir un nom unique et illustratif.  
  
 **Condition**  
 Tapez une expression ou créez-en une en faisant glisser les colonnes, variables et fonctions disponibles.  
  
 Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.  
  
 **Rubriques connexes :**  [Expressions Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md), [Opérateurs &#40;Expression SSIS&#41;](../../../integration-services/expressions/operators-ssis-expression.md), et [Fonctions &#40;Expression SSIS&#41;](../../../integration-services/expressions/functions-ssis-expression.md)  
  
 **Nom de sortie par défaut**  
 Tapez un nom pour la sortie par défaut ou utilisez le nom par défaut.  
  
 **Configurer l'affichage des erreurs**  
 Spécifiez comment gérer les erreurs dans la boîte de dialogue [Configurer la sortie d’erreur](http://msdn.microsoft.com/library/5f8da390-fab5-44f8-b268-d8fa313ce4b9) .  
  
## <a name="see-also"></a> Voir aussi  
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
