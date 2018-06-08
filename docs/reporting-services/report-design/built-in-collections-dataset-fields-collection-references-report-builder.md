---
title: Références à la collection Fields d’un dataset (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 006c6bd3-d776-4c20-9092-32e40688ac49
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 5c3c5f21887c6f2307f78df008340700eb5b722f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="built-in-collections---dataset-fields-collection-references-report-builder"></a>Collections intégrées - Références à la collection Fields d’un dataset (Générateur de rapports)
  Chaque dataset d’un rapport contient une collection Fields. La collection Fields comprend l’ensemble de champs spécifiés par la requête de dataset, ainsi que tous les champs calculés supplémentaires que vous créez. Après avoir créé un dataset, la collection de champs apparaît dans le volet **Données du rapport** .  
  
 Une référence de champ simple dans une expression s'affiche sur l'aire de conception comme une expression simple. Par exemple, lorsque vous faites glisser le champ `Sales` du volet des données de rapport vers une cellule de table sur l'aire de conception, `[Sales]` s'affiche. Cela représente l’expression sous-jacente `=Fields!Sales.Value` définie sur la propriété Value de la zone de texte. Lorsque le rapport s'exécute, le processeur de rapports évalue cette expression et affiche les données effectives de la source de données dans la zone de texte dans la cellule de table. Pour plus d’informations, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md) et [Collection Fields d’un dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="displaying-the-field-collection-for-a-dataset"></a>Affichage de la collection de champs pour un dataset  
 Pour afficher les valeurs individuelles pour une collection de champs, faites glisser chaque champ vers une ligne de détails du tableau et exécutez le rapport. Les références de la ligne de détails d'une région de données de table ou de liste affichent une valeur pour chaque ligne du dataset.  
  
 Pour afficher des valeurs résumées pour un champ, faites glisser chaque champ numérique vers la zone de données d'une matrice. La fonction d'agrégation par défaut pour la ligne des totaux est Sum, par exemple `=Sum(Fields!Sales.Value)`. Vous pouvez modifier la fonction par défaut pour calculer des totaux différents. Pour plus d’informations, consultez [Informations de référence sur les fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md).  
  
 Pour afficher des valeurs résumées pour une collection de champs dans une zone de texte directement sur l'aire de conception (n'appartenant à aucune région de données), vous devez spécifier le nom du dataset comme une étendue pour la fonction d'agrégation. Par exemple, pour un dataset nommé `SalesData`, l’expression suivante spécifie le total de toutes les valeurs pour le champ `Sales`: `=Sum(Fields!Sales,"SalesData")`.  
  
 Lorsque vous utilisez la boîte de dialogue **Expression** pour définir une référence de champ simple, vous pouvez sélectionner la collection Fields dans le volet Catégorie et afficher la liste des champs disponibles dans le volet **Champ** . Chaque champ a plusieurs propriétés, notamment Value et IsMissing. Les propriétés restantes sont des propriétés de champ étendues prédéfinies qui peuvent être disponibles pour le dataset en fonction du type de source de données.  
  
### <a name="detecting-nulls-for-a-dataset-field"></a>Détection de valeurs Null pour un champ de dataset  
 Pour détecter une valeur de champ Null (**Nothing** dans [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]), vous pouvez utiliser la fonction **IsNothing**. Lorsqu'elle est placée dans une zone de texte dans la ligne Détails d'un tableau, l'expression suivante teste le champ `MiddleName` et substitue le texte « No Middle Name » lorsque la valeur est Null, et la valeur de champ elle-même lorsque la valeur n'est pas Null :  
  
 `=IIF(IsNothing(Fields!MiddleName.Value),"No Middle Name",Fields!MiddleName.Value)`  
  
### <a name="detecting-missing-fields-for-dynamic-queries-at-run-time"></a>Détection de champs manquants pour les requêtes dynamiques au moment de l'exécution  
 Par défaut, les éléments de la collection Fields disposent de deux propriétés : Value et IsMissing. La propriété IsMissing indique si un champ défini pour un dataset au moment de la conception figure dans les champs récupérés au moment de l’exécution. Par exemple, votre requête peut appeler une procédure stockée dans laquelle le jeu de résultats varie en fonction d’un paramètre d’entrée, ou votre requête peut être `SELECT * FROM` *\<table>* où la définition de table a changé.  
  
> [!NOTE]  
>  IsMissing détecte les modifications du schéma de dataset entre le moment de la conception et celui de l’exécution pour tout type de source de données. IsMissing ne peut pas être utilisé pour détecter des membres vides dans un cube multidimensionnel et n’est pas lié aux concepts de langage de requête MDX **EMPTY** et **NON EMPTY**.  
  
 Vous pouvez tester la propriété IsMissing dans le code personnalisé pour déterminer si un champ est présent dans le jeu de résultats. Vous ne pouvez pas tester sa présence à l’aide d’une expression avec un appel de fonction [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] tel que **IIF** ou **SWITCH**, car [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] évalue tous les paramètres dans l’appel à la fonction, ce qui provoque une erreur lorsque la référence au champ manquant est évaluée.  
  
#### <a name="example-for-controlling-the-visibility-of-a-dynamic-column-for-a-missing-field"></a>Exemple de contrôle de la visibilité d'une colonne dynamique pour un champ manquant  
 Pour définir une expression qui contrôle la visibilité d’une colonne qui affiche un champ dans un dataset, vous devez d’abord définir une fonction de code personnalisé qui retourne une valeur booléenne si le champ manque. Par exemple, la fonction de code personnalisé suivante retourne la valeur True si le champ manque et la valeur False si le champ existe.  
  
```  
Public Function IsFieldMissing(field as Field) as Boolean  
 If (field.IsMissing) Then  
 Return True  
  Else   
  Return False  
 End If  
End Function  
```  
  
 Pour utiliser cette fonction pour contrôler la visibilité d’une colonne, affectez l’expression suivante à la propriété Hidden de la colonne :  
  
 `=Code.IsFieldMissing(Fields!FieldName)`  
  
 La colonne est masquée lorsque le champ n'existe pas.  
  
#### <a name="example-for-controlling-the-text-box-value-for-a-missing-field"></a>Exemple de contrôle de la valeur de zone de texte pour un champ manquant  
 Pour remplacer la valeur d’un champ manquant par un texte que vous écrivez, vous devez écrire un code personnalisé qui retourne le texte que vous pouvez utiliser à la place d’une valeur de champ lorsque le champ est manquant. Par exemple, la fonction de code personnalisé suivante retourne la valeur du champ si le champ existe et le message que vous spécifiez comme deuxième paramètre si le champ n'existe pas :  
  
```  
Public Function IsFieldMissingThenString(field as Field, strMessage as String) as String  
 If (field.IsMissing) Then  
  Return strMessage  
 Else   
  Return field.Value  
  End If  
End Function  
```  
  
 Pour utiliser cette fonction dans une zone de texte, ajoutez l’expression suivante à la propriété Value :  
  
 `=Code.IsFieldMissingThenString(Fields!FieldName,"Missing")`  
  
 La zone de texte affiche la valeur de champ ou le texte que vous avez spécifié.  
  
### <a name="using-extended-field-properties"></a>Utilisation des propriétés de champ étendues  
 Les propriétés de champ étendues sont des propriétés supplémentaires définies sur un champ par l'extension pour le traitement des données, qui est déterminée par le type de source de données du dataset. Les propriétés de champ étendues sont prédéfinies ou spécifiques à un type de source de données. Pour plus d’informations, consultez [Propriétés de champ étendues pour une base de données Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
 Si vous spécifiez une propriété qui n’est pas prise en charge pour ce champ, l’expression prend la valeur **null** (**Nothing** dans [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]). Si un fournisseur de données ne prend pas en charge les propriétés de champ étendues ou si le champ est introuvable lors de l’exécution de la requête, la valeur de la propriété est **null** (**Nothing** dans [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]) pour les propriétés de type **String** et **Object**, et zéro (0) pour les propriétés de type **Integer**. Une extension pour le traitement des données peut tirer parti des propriétés prédéfinies en optimisant les requêtes qui intègrent cette syntaxe.  
  
## <a name="see-also"></a> Voir aussi  
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
  
  
