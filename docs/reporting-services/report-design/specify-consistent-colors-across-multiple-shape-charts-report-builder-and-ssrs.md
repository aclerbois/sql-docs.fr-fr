---
title: Spécifier des couleurs cohérentes pour plusieurs graphiques à base de formes (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: d52f68e9-2ba7-4bff-9053-4089e5164ab4
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: fddd1c4466b3b30976bd1008ec7f6d616b71ba89
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-consistent-colors-across-multiple-shape-charts-report-builder-and-ssrs"></a>Spécifier des couleurs cohérentes pour plusieurs graphiques à base de formes (Générateur de rapports et SSRS)
  Sur les graphiques qui ne sont pas à base de formes dans un rapport paginé, [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] sélectionne une nouvelle couleur dans la palette en fonction de l’index de la série dans le graphique. Par exemple, la première série sur votre graphique sera associée à la première couleur dans la palette. Toutefois, ce comportement diffère pour les graphiques à base de formes. Sur les graphiques à base de formes, chaque couleur de la palette est associée à un point de données dans le dataset. Par exemple, le point de données 1 est associé à la première couleur de la palette, le point de données 2 est associé à la deuxième couleur et ainsi de suite.  
  
 Si un point de données n'a aucune valeur, il n'apparaît pas sur l'affichage du graphique à base de formes. Cela signifie que ce point de données n'est pas coloré. Par exemple, si le point 2 a une valeur de zéro, le point 1 sera associé à la première couleur de la palette et le point 3 sera associé à la deuxième couleur de la palette. Cette approche est utile car les points vides dans le dataset d'un graphique à secteurs n'utilisent pas inutilement une couleur de la palette lorsque le point vide ne doit pas être dessiné.  
  
 En corollaire, lorsque plusieurs graphiques à secteurs sont affichés dans un rapport, les graphiques à secteurs peuvent afficher des couleurs différentes pour des points de données correspondant aux mêmes regroupements de catégories. Pour pallier cet inconvénient, vous devez définir des couleurs individuelles associées à un groupe de catégories et non à des valeurs de données individuelles. La façon dont vous procédez varie si les graphiques à base de formes sont des graphiques sparkline dans une table ou matrice, ou s'il s'agit de graphiques à base de formes dans le rapport lui-même.  
  
 La légende est liée à la série, donc toute couleur que vous spécifiez pour la série sera automatiquement utilisée pour la légende.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-specify-consistent-colors-across-multiple-sparkline-shape-charts-in-a-table-or-matrix"></a>Pour spécifier des couleurs cohérentes pour plusieurs graphiques à base de formes sparkline dans une table ou matrice  
  
1.  Cliquez sur le graphique pour afficher le volet Données du graphique.  
  
2.  Dans la zone **Groupes de catégories** , cliquez avec le bouton droit sur une catégorie, puis cliquez sur **Propriétés du groupe de catégories**.  
  
3.  Sous l'onglet Général, dans la zone **Synchroniser les groupes dans** , cliquez sur le nom de la catégorie pour laquelle vous aimeriez synchroniser des couleurs, puis sur **OK**.  
  
## <a name="to-specify-consistent-colors-across-multiple-shape-charts"></a>Pour spécifier des couleurs cohérentes pour plusieurs graphiques à base de formes  
  
1.  Cliquez avec le bouton droit à l’extérieur du corps du rapport, puis sélectionnez **Propriétés du rapport**.  
  
2.  Dans **Code**, tapez le code suivant dans la zone de texte.  
  
    ```  
    Private colorPalette As String() = {"Color1", "Color2", "Color3"}  
    Private count As Integer = 0  
    Private mapping As New System.Collections.Hashtable()  
    Public Function GetColor(ByVal groupingValue As String) As String  
        If mapping.ContainsKey(groupingValue) Then  
            Return mapping(groupingValue)  
        End If  
        Dim c As String = colorPalette(count Mod colorPalette.Length)  
        count = count + 1  
        mapping.Add(groupingValue, c)  
        Return c  
    End Function  
    ```  
  
    > [!NOTE]  
    >  Vous devez remplacer les chaînes "Color1" par vos propres couleurs. Vous pouvez utiliser des couleurs nommées, par exemple "Rouge", ou vous pouvez utiliser valeur hexadécimale de six chiffres qui représente la couleur, par exemple "#FFFFFF" pour le noir. Si plus de trois couleurs sont définies, vous devez étendre le tableau de couleurs afin que le nombre de couleurs dans le tableau corresponde au nombre de points de votre graphique à base de formes. Vous pouvez ajouter de nouvelles couleurs au tableau en spécifiant une liste séparée par des virgules de valeurs de chaîne qui contiennent des couleurs nommées ou des représentations hexadécimales de couleurs.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Cliquez avec le bouton droit sur le graphique à base de formes, puis sélectionnez **Propriétés de la série**.  
  
5.  Dans **Remplir**, cliquez sur le bouton **Expression** (*fx*) pour modifier l’expression pour la propriété **Couleur** .  
  
6.  Tapez l'expression suivante, où « MyCategoryField » est le champ affiché dans la zone **Groupes d'abscisses** :  
  
    ```  
    =Code.GetColor(Fields!MyCategoryField)  
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en forme des couleurs des séries sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-series-colors-on-a-chart-report-builder-and-ssrs.md)   
 [Ajouter des styles de biseau, de relief et de texture à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-effects-add-bevel-emboss-or-texture-report-builder.md)   
 [Définir les couleurs d’un graphique à l’aide d’une palette &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/define-colors-on-a-chart-using-a-palette-report-builder-and-ssrs.md)   
 [Ajouter des points vides à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md)   
 [Graphiques à base de formes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/shape-charts-report-builder-and-ssrs.md)   
 [Liaison de plusieurs régions de données à un même dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md)   
 [Régions de données imbriquées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/nested-data-regions-report-builder-and-ssrs.md)   
 [Graphiques sparkline et barres de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md)  
  
  
