---
title: Fonction Lookup (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: e60e5bab-b286-4897-9685-9ff12703517d
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 15d02d28676af96b52e66a0b249e119baaa3c87d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-builder-functions---lookup-function"></a>Fonctions du Générateur de rapports - Lookup
  Retourne la première valeur correspondante pour le nom spécifié d'un dataset contenant des paires nom/valeur.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Lookup(source_expression, destination_expression, result_expression, dataset)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *source_expression*  
 (**Variant**) Expression évaluée dans l’étendue actuelle et qui spécifie le nom ou la clé à rechercher. Par exemple, `=Fields!ProdID.Value`.  
  
 *destination_expression*  
 (**Variant**) Expression évaluée pour chaque ligne d’un dataset et qui spécifie le nom ou la clé de correspondance. Par exemple, `=Fields!ProductID.Value`.  
  
 *result_expression*  
 (**Variant**) Expression qui est évaluée pour la ligne du dataset où *source_expression* = *destination_expression*, et qui spécifie la valeur à récupérer. Par exemple, `=Fields!ProductName.Value`.  
  
 *dataset*  
 Constante qui spécifie le nom d'un dataset dans le rapport. Par exemple, « Products ».  
  
## <a name="return"></a>Return  
 Retourne une valeur **Variant**, ou **Nothing** si aucune correspondance n'est trouvée.  
  
## <a name="remarks"></a>Notes   
 Utilisez **Lookup** pour récupérer la valeur du dataset spécifié pour une paire nom/valeur où il y a une relation un-à-un. Par exemple, pour un champ d’ID dans une table, vous pouvez utiliser **Lookup** pour récupérer le champ Nom correspondant d’un dataset qui n’est pas lié à la région de données.  
  
 La fonction**Lookup** effectue les actions suivantes :  
  
-   Évalue l'expression source dans l'étendue actuelle.  
  
-   Évalue l'expression de destination pour chaque ligne du dataset spécifié après avoir appliqué les filtres, en fonction du classement du dataset spécifié.  
  
-   Pour la première correspondance des expressions source et de destination, évalue l'expression de résultat pour cette ligne dans le dataset.  
  
-   Renvoie la valeur d'expression de résultat.  
  
 Pour récupérer plusieurs valeurs pour un seul champ de nom ou de clé dans lequel une relation un-à-plusieurs existe, utilisez [Fonction LookupSet &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-lookupset-function.md). Pour appeler **Lookup** pour un ensemble de valeurs, utilisez la [Fonction Multilookup &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-multilookup-function.md).  
  
 Les restrictions suivantes s'appliquent :  
  
-   **Lookup** est évalué après que toutes les expressions de filtre ont été appliquées.  
  
-   Un seul niveau de recherche est pris en charge. Une expression source, destination ou de résultat ne peut pas inclure de référence à une fonction de recherche.  
  
-   Les expressions source et de destination doivent correspondre au même type de données. Le type retourné est identique au type de données de l'expression de résultat évaluée.  
  
-   Les expressions source, de destination et de résultat ne peuvent pas inclure de références à des variables de groupe ou de rapport.  
  
-   **Lookup** ne peut pas être utilisé comme expression pour les éléments de rapport suivants :  
  
    -   des chaînes de connexion dynamiques pour une source de données ;  
  
    -   des champs calculés dans un dataset ;  
  
    -   des paramètres de requête dans un dataset ;  
  
    -   des filtres dans un dataset ;  
  
    -   des paramètres de rapport ;  
  
    -   la propriété Report.Language.  
  
 Pour plus d’informations, consultez [Référence aux fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) et [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
## <a name="example"></a> Exemple  
 Dans l'exemple suivant, supposez qu'une table est liée à un dataset qui inclut un champ pour l'identificateur de produit ProductID. Un dataset distinct nommé « Product » contient l'identificateur de produit correspondant ID et le nom de produit Name.  
  
 Dans l’expression suivante, **Lookup** compare la valeur de ProductID à ID sur chaque ligne du dataset nommé « Product » et, quand une correspondance est trouvée, retourne la valeur du champ Name pour cette ligne.  
  
```  
=Lookup(Fields!ProductID.Value, Fields!ID.Value, Fields!Name.Value, "Product")  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Types de données dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
  
