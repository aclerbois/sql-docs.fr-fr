---
title: Fonction LookupSet (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 7685acfd-1c8d-420c-993c-903236fbe1ff
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: c15c9dc2c895d3388e644ba285cf71703b00bc6c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-builder-functions---lookupset-function"></a>Fonctions du Générateur de rapports - LookupSet
  Retourne le jeu de valeurs correspondantes pour le nom spécifié d'un dataset contenant des paires nom/valeur.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
LookupSet(source_expression, destination_expression, result_expression, dataset)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *source_expression*  
 (**Variant**) Expression évaluée dans l’étendue actuelle et qui spécifie le nom ou la clé à rechercher. Par exemple, `=Fields!ID.Value`.  
  
 *destination_expression*  
 (**Variant**) Expression évaluée pour chaque ligne d’un dataset et qui spécifie le nom ou la clé de correspondance. Par exemple, `=Fields!CustomerID.Value`.  
  
 *result_expression*  
 (**Variant**) Expression qui est évaluée pour la ligne du dataset où *source_expression* = *destination_expression*, et qui spécifie la valeur à récupérer. Par exemple, `=Fields!PhoneNumber.Value`.  
  
 *dataset*  
 Constante qui spécifie le nom d'un dataset dans le rapport. Par exemple, « ContactInformation ».  
  
## <a name="return"></a>Return  
 Retourne une valeur **VariantArray**, ou **Nothing** si aucune correspondance n'est trouvée.  
  
## <a name="remarks"></a>Notes   
 Utilisez **LookupSet** pour récupérer un jeu de valeurs du dataset spécifié pour une paire nom-valeur quand il existe une relation un-à-plusieurs. Par exemple, pour un identificateur de client dans une table, vous pouvez utiliser **LookupSet** pour récupérer tous les numéros de téléphone associés à ce client à partir d'un dataset qui n'est pas lié à la région de données.  
  
 La fonction**LookupSet** effectue les actions suivantes :  
  
-   Évalue l'expression source dans l'étendue actuelle.  
  
-   Évalue l'expression de destination pour chaque ligne du dataset spécifié après avoir appliqué les filtres, en fonction du classement du dataset spécifié.  
  
-   Pour chaque correspondance des expressions source et de destination, évalue l'expression de résultat pour cette ligne dans le dataset.  
  
-   Retourne le jeu de valeurs d'expressions de résultat.  
  
 Pour récupérer une valeur unique dans un dataset avec les paires nom-valeur d’un nom spécifique, lorsqu’il existe une relation un-à-un, utilisez la [Fonction Lookup &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-lookup-function.md). Pour appeler **Lookup** pour un ensemble de valeurs, utilisez la [Fonction Multilookup &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-multilookup-function.md).  
  
 Les restrictions suivantes s'appliquent :  
  
-   **LookupSet** est évalué après que toutes les expressions de filtre ont été appliquées.  
  
-   Un seul niveau de recherche est pris en charge. Une expression source, destination ou de résultat ne peut pas inclure de référence à une fonction de recherche.  
  
-   Les expressions source et de destination doivent correspondre au même type de données.  
  
-   Les expressions source, de destination et de résultat ne peuvent pas inclure de références à des variables de groupe ou de rapport.  
  
-   **LookupSet** ne peut pas être utilisé comme expression pour les éléments de rapport suivants :  
  
    -   des chaînes de connexion dynamiques pour une source de données ;  
  
    -   des champs calculés dans un dataset ;  
  
    -   des paramètres de requête dans un dataset ;  
  
    -   des filtres dans un dataset ;  
  
    -   des paramètres de rapport ;  
  
    -   la propriété Report.Language.  
  
 Pour plus d’informations, consultez [Référence aux fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) et [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
## <a name="example"></a> Exemple  
 Dans l'exemple suivant, supposons que la table est liée à un dataset qui inclut un identificateur de secteur de vente TerritoryGroupID. Un dataset séparé appelé « Magasins » contient la liste de tous les magasins dans un secteur et inclut l'identificateur du secteur ID et le nom du magasin NomMagasin.  
  
 Dans l’expression suivante, **LookupSet** compare la valeur TerritoryGroupID à la valeur ID pour chaque ligne du dataset appelé « Stores ». Pour chaque correspondance, la valeur du champ StoreName pour cette ligne est ajoutée au jeu de résultats.  
  
```  
=LookupSet(Fields!TerritoryGroupID.Value, Fields!ID.Value, Fields!StoreName.Value, "Stores")  
```  
  
## <a name="example"></a> Exemple  
 Étant donné que **LookupSet** retourne une collection d'objets, vous ne pouvez pas afficher directement l'expression de résultat dans une zone de texte. Vous pouvez concaténer la valeur de chaque objet dans la collection en tant que chaîne.  
  
 Utilisez la fonction [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] **Join** pour créer une chaîne délimitée à partir d’un jeu d’objets. Utilisez une virgule comme séparateur pour combiner les objets en une ligne unique. Dans certains convertisseurs, vous pouvez utiliser un saut de ligne [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] (`vbCrLF`) comme séparateur pour répertorier chaque valeur sur une nouvelle ligne.  
  
 L’expression suivante, quand elle est utilisée comme propriété Value d’une zone de texte, utilise **Join** pour créer une liste.  
  
```  
=Join(LookupSet(Fields!TerritoryGroupID.Value, Fields!ID.Value, Fields!StoreName.Value, "Stores"),",")  
```  
  
## <a name="example"></a> Exemple  
 Pour les zones de texte qui seront uniquement restituées quelques fois, vous pouvez choisir d'ajouter un code personnalisé pour générer un code HTML permettant d'afficher des valeurs dans une zone de texte. Le code HTML dans une zone de texte requiert un traitement supplémentaire et ne serait donc pas un bon choix pour une zone de texte restituée des milliers de fois.  
  
 Copiez les fonctions [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] suivantes dans un bloc Code de définition de rapport. **MakeList** utilise le tableau d’objets retourné dans *result_expression* pour générer une liste non ordonnée à l’aide de balises HTML. **Length** retourne le nombre d'éléments dans le tableau d'objets.  
  
```  
Function MakeList(ByVal items As Object()) As String  
   If items Is Nothing Then  
      Return Nothing  
   End If  
  
   Dim builder As System.Text.StringBuilder =   
      New System.Text.StringBuilder()  
   builder.Append("<ul>")  
  
   For Each item As Object In items  
      builder.Append("<li>")  
      builder.Append(item)  
   Next  
   builder.Append("</ul>")  
  
   Return builder.ToString()  
End Function  
  
Function Length(ByVal items as Object()) as Integer  
   If items is Nothing Then  
      Return 0  
   End If  
   Return items.Length  
End Function  
```  
  
## <a name="example"></a> Exemple  
 Pour générer le code HTML, vous devez appeler la fonction. Collez l’expression suivante dans la propriété Value de la zone de texte et définissez le type de balise de texte sur HTML. Pour plus d’informations, consultez [Ajouter du code HTML à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-html-into-a-report-report-builder-and-ssrs.md).  
  
```  
=Code.MakeList(LookupSet(Fields!TerritoryGroupID.Value, Fields!ID.Value, Fields!StoreName.Value, "Stores"))  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Types de données dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
  
