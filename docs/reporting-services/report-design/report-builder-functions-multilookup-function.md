---
title: Fonction Multilookup (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1fec079e-33b3-4e4d-92b3-6b4d06a49a77
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 9934684dc787d7f258f223b2e9ba8997aa7bd009
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-builder-functions---multilookup-function"></a>Fonctions du Générateur de rapports - Multilookup
  Retourne le jeu de valeurs de première correspondance pour le jeu de noms spécifié d'un dataset contenant des paires nom/valeur.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Multilookup(source_expression, destination_expression, result_expression, dataset)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *source_expression*  
 (**VariantArray**) Expression évaluée dans l’étendue actuelle et qui spécifie le jeu de noms ou de clés à rechercher. Par exemple, pour un paramètre à valeurs multiples, `=Parameters!IDs.value`.  
  
 *destination_expression*  
 (**Variant**) Expression évaluée pour chaque ligne d’un dataset et qui spécifie le nom ou la clé de correspondance. Par exemple, `=Fields!ID.Value`.  
  
 *result_expression*  
 (**Variant**) Expression qui est évaluée pour la ligne du dataset où *source_expression* = *destination_expression*, et qui spécifie la valeur à récupérer. Par exemple, `=Fields!Name.Value`.  
  
 *dataset*  
 Constante qui spécifie le nom d'un dataset dans le rapport. Par exemple, « Couleurs ».  
  
## <a name="return"></a>Return  
 Retourne une valeur **VariantArray**, ou **Nothing** si aucune correspondance n'est trouvée.  
  
## <a name="remarks"></a>Notes   
 Utilisez **Multilookup** pour récupérer un ensemble de valeurs d’un dataset pour les paires nom-valeur, lorsqu’il existe une relation un-à-un pour chaque paire. **MultiLookup** équivaut à appeler **Lookup** pour un ensemble de noms ou de clés. Par exemple, pour un paramètre à valeurs multiples basé sur des identificateurs de clé primaire,vous pouvez utiliser **Multilookup** dans une expression d'une zone de texte d'une table pour récupérer des valeurs associées d'un dataset qui n'est pas lié au paramètre ou à la table.  
  
 La fonction**Multilookup** effectue les actions suivantes :  
  
-   Elle évalue l'expression source dans l'étendue active et génère un tableau d'objets de type Variant.  
  
-   Pour chaque objet du tableau, elle appelle la [Fonction Lookup &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-lookup-function.md) et ajoute le résultat au tableau de résultats.  
  
-   Retourne le jeu de résultats.  
  
 Pour récupérer une valeur unique dans un dataset avec les paires nom-valeur d’un nom spécifique, lorsqu’il existe une relation un-à-un, utilisez la [Fonction Lookup &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-lookup-function.md). Pour récupérer plusieurs valeurs dans un dataset avec les paires nom-valeur d’un nom, lorsqu’il existe une relation un-à-plusieurs, utilisez une [Fonction LookupSet &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-lookupset-function.md).  
  
 Les restrictions suivantes s'appliquent :  
  
-   **Multilookup** est évalué après que toutes les expressions de filtre ont été appliquées.  
  
-   Un seul niveau de recherche est pris en charge. Une expression source, destination ou de résultat ne peut pas inclure de référence à une fonction de recherche.  
  
-   Les expressions source et de destination doivent correspondre au même type de données.  
  
-   Les expressions source, de destination et de résultat ne peuvent pas inclure de références à des variables de groupe ou de rapport.  
  
-   **Multilookup** ne peut pas être utilisé comme expression pour les éléments de rapport suivants :  
  
    -   des chaînes de connexion dynamiques pour une source de données ;  
  
    -   des champs calculés dans un dataset ;  
  
    -   des paramètres de requête dans un dataset ;  
  
    -   des filtres dans un dataset ;  
  
    -   des paramètres de rapport ;  
  
    -   la propriété Report.Language.  
  
 Pour plus d’informations, consultez [Référence aux fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) et [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
## <a name="example"></a> Exemple  
 Supposons qu'un dataset nommé « Category » comprend le champ CategoryList, qui contient une liste séparée par des virgules d'identificateurs de catégorie, par exemple « 2, 4, 2, 1 ».  
  
 Le dataset CategoryNames contient l'identificateur et le nom de catégorie, comme indiqué dans le tableau ci-dessous.  
  
|ID|Nom   |  
|--------|----------|  
| 1|Accessories|  
|2|Bikes|  
|3|Clothing|  
|4|Components|  
  
 Pour rechercher les noms correspondant à la liste d'identificateurs, utilisez **Multilookup**. Vous devez tout d'abord fractionner la liste en un tableau de chaînes, appeler **Multilookup** pour récupérer les noms des catégories, puis concaténer les résultats en une chaîne.  
  
 L'expression suivante, lorsqu'elle est placée dans une zone de texte dans une région de données liée au dataset Catégorie, affiche « Bicyclettes, Composants, Bicyclettes, Accessoires » :  
  
```  
=Join(MultiLookup(Split(Fields!CategoryList.Value,","),  
   Fields!CategoryID.Value,Fields!CategoryName.Value,"Category")),  
   ", ")  
```  
  
## <a name="example"></a> Exemple  
 Supposons qu' un dataset CouleursProduits contient un champ d'identificateur de couleur IDCouleur et un champ de valeur de couleur Couleur, comme indiqué dans le tableau ci-dessous.  
  
|ColorID|Couleur|  
|-------------|-----------|  
| 1|Rouge|  
|2|Bleu|  
|3|Vert|  
  
 Supposons que le paramètre à valeurs multiples *MyColors* n'est pas lié à un dataset pour ses valeurs disponibles. Les valeurs par défaut du paramètre sont égales à 2 et 3. L'expression suivante, lorsqu'elle est placée dans une zone de texte d'un tableau, concatène les valeurs de sélection multiple pour le paramètre dans une liste séparée par des virgules et affiche « Bleu, Vert ».  
  
```  
=Join(MultiLookup(Parameters!MyColors.Value,Fields!ColorID.Value,Fields!Color.Value,"ProductColors"),", ")  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Types de données dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
  
