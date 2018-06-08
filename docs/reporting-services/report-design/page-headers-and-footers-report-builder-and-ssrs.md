---
title: En-têtes et pieds de page (Générateur de rapports et SSRS) | Microsoft Docs
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
f1_keywords:
- "10125"
- sql13.rtp.rptdesigner.pagefooter.border.f1
- "10121"
- "10120"
- "10122"
- sql13.rtp.rptdesigner.pageheader.general.f1
- "10123"
- sql13.rtp.rptdesigner.pageheader.fill.f1
- sql13.rtp.rptdesigner.pageheader.border.f1
- sql13.rtp.rptdesigner.pagefooter.fill.f1
- sql13.rtp.rptdesigner.pagefooter.general.f1
- "10124"
ms.assetid: 4fb9faac-511e-404a-b8d7-1f2e3cb47b11
caps.latest.revision: 6
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 1835b4d7a6ede5de5d442f36fe2ea7c85e9e330d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="page-headers-and-footers-report-builder-and-ssrs"></a>En-têtes et pieds de page (Générateur de rapports et SSRS)
  Un rapport peut contenir un en-tête et un pied de page. Ces informations sont situées respectivement le long des bords supérieur et inférieur de chaque page. Les en-têtes et pieds de page peuvent contenir du texte statique, des images, des rectangles, des bordures, une couleur d'arrière-plan, des images d'arrière-plan et des expressions. Les expressions contiennent des références de champs de dataset pour des rapports qui utilisent un seul dataset, ainsi que des appels de fonction d'agrégation qui intègrent le dataset sous forme d'étendue.  
  
> [!NOTE]  
>  Chaque extension du rendu traite les pages différemment. Pour plus d’informations sur la pagination des rapports et les extensions de rendu, consultez [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
 Par défaut, les rapports contiennent des pieds de page mais pas d'en-têtes de page. Pour plus d’informations sur leur ajout ou leur suppression, consultez [Ajouter ou supprimer un en-tête ou un pied de page &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-remove-a-page-header-or-footer-report-builder-and-ssrs.md).  
  
 En règle générale, les en-têtes et pieds de page contiennent des numéros de page, des titres de rapport et d'autres propriétés de rapport. Pour plus d’informations sur l’ajout de ces éléments à un en-tête ou pied de page dans votre rapport, consultez [Afficher les numéros de page ou d’autres propriétés de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-page-numbers-or-other-report-properties-report-builder-and-ssrs.md).  
  
 Une fois créés, les en-têtes et pieds de page s'affichent sur toutes les pages du rapport. Pour plus d’informations sur la suppression des en-têtes et pieds de page figurant sur les première et dernière pages, consultez [Masquer un en-tête ou un pied de page sur la première ou la dernière page &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/hide-a-page-header-or-footer-on-the-first-or-last-page-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="report-headers-and-footers"></a>En-têtes et pieds de page de rapport  
 Les en-têtes et pieds de page ne sont pas identiques aux en-têtes et pieds de page de rapport. Les rapports ne contiennent pas en effet de zone spécialement réservée aux en-têtes ou pieds de page. Les en-têtes de rapport contiennent les éléments qui figurent en haut du corps du rapport, c'est-à-dire sur son aire de conception. Ces éléments apparaissent une fois seulement et correspondent au premier contenu du rapport. Les pieds de page de rapport contiennent les éléments qui figurent en bas du corps du rapport. Ces éléments apparaissent une fois seulement et correspondent au dernier contenu du rapport.  
  
## <a name="displaying-variable-data-in-a-page-header-or-footer"></a>Affichage de données variables dans les en-têtes ou pieds de page  
 Les en-têtes et pieds de page peuvent contenir du contenu statique, mais ils servent généralement à afficher des éléments qui varient, tels que des numéros de page ou des informations sur le contenu d'une page. Pour afficher des données variables d'une page à l'autre, vous devez définir une expression.  
  
 Quand un seul dataset est défini dans le rapport, vous pouvez ajouter des expressions simples telles que `[FieldName]` aux en-têtes ou pieds de page. Faites glisser le champ de la collection de champs du dataset du volet des données de rapportou de la collection de champs prédéfinis vers l'en-tête ou le pied de page. Une zone de texte contenant l'expression adéquate est alors automatiquement ajoutée.  
  
 Pour calculer la somme ou les autres agrégats des valeurs d'une page, vous pouvez utiliser les expressions d'agrégat qui permettent de définir les éléments apparaissant dans le rapport et le nom du dataset. La collection ReportItems correspond à la collection de zones de texte qui apparaissent sur chaque page du rapport une fois celui-ci rendu. La définition du rapport doit contenir le nom du dataset. Le tableau ci-après répertorie les éléments pris en charge en fonction de chaque type d'expression d'agrégat utilisée :  
  
|Pris en charge par l'expression|Agrégats ReportItems|Agrégats du dataset (le nom du dataset doit correspondre à l'étendue)|  
|-----------------------------|----------------------------|----------------------------------------------------------|  
|Zones de texte apparaissant dans le corps du rapport|Oui|non|  
|&PageNumber|Oui|non|  
|&TotalPages|Oui|non|  
|Fonction d'agrégation|Oui. Par exemple,<br /><br /> `=First(ReportItems!TXT_LastName.Value)`|Oui. Par exemple,<br /><br /> `=Max(Quantity.Value,"DataSet1")`|  
|Collection de champs pour les éléments figurant sur les pages|Indirectement. Par exemple,<br /><br /> `=Sum(ReportItems!Textbox1.Value)`|Oui. Par exemple,<br /><br /> `=Sum(Fields!Quantity.Value,"DataSet1")`|  
|Image liée aux données|Indirectement. Par exemple : `=ReportItems!TXT_Photo.Value`|Oui. Par exemple,<br /><br /> `=First(Fields!Photo.Value,"DataSet1")`|  
  
 Les sections suivantes de cette rubrique illustrent des expressions prêtes à l'emploi permettant d'obtenir les données variables utilisées couramment dans les en-têtes et pieds de page. Une section explique également le mode de traitement de l'extension de rendu Excel pour les en-têtes et les pieds de page. Pour plus d’informations sur les expressions, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
### <a name="adding-calculated-page-totals-to-a-header-or-footer"></a>Ajout de totaux calculés de page à un en-tête ou à un pied de page  
 Pour certains rapports, il est pratique d'inclure une valeur calculée dans l'en-tête ou le pied de page de chaque rapport (par exemple, une somme totale par page si celle-ci comprend des valeurs numériques). Étant donné que vous ne pouvez pas référencer les champs directement, l'expression placée dans l'en-tête ou le pied de page doit référencer le nom de l'élément du rapport (par exemple, une zone de texte) plutôt que le champ de données :  
  
 `=Sum(ReportItems!Textbox1.Value)`  
  
 Si la zone de texte apparaît dans une table ou dans une liste avec des lignes répétées de données, la valeur qui s'affiche dans l'en-tête ou le pied de page de la page actuelle au moment de l'exécution du rapport représente la somme de toutes les valeurs de toutes les données de l'instance `TextBox1` qui figurent dans cette table ou liste.  
  
 Lors du calcul des totaux de la page, vous pouvez noter des différences de totaux lorsque vous utilisez différentes extensions de rendu pour consulter le rapport. La sortie paginée est calculée différemment pour chaque extension de rendu. La même page que vous consultez au format HTML risque de montrer des totaux différents au format PDF si le montant de données sur la page PDF est différent. Pour plus d’informations, consultez [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md).  
  
### <a name="for-reports-with-multiple-datasets"></a>Pour les rapports contenant plusieurs datasets  
 Pour les rapports qui contiennent plusieurs datasets, vous ne pouvez pas ajouter de champs ou d'images liées aux données directement aux en-têtes ou pieds de page. Cependant, vous pouvez écrire une expression qui renvoie indirectement au champ ou à l'image liée aux données à utiliser dans les en-têtes ou pieds de page de votre choix.  
  
 Pour placer des données variables dans un en-tête ou un pied de page, procédez comme suit :  
  
-   Ajoutez une zone de texte à l'en-tête ou au pied de page.  
  
-   Dans la zone de texte, écrivez l'expression permettant de générer les données variables à afficher.  
  
-   Dans cette expression, insérez des références aux éléments de rapport apparaissant sur la page (vous pouvez, par exemple, référencer une zone de texte contenant des données issues d'un champ particulier). N'insérez pas de référence directe aux champs dans un dataset. Par exemple, vous ne pouvez pas utiliser l’expression `[LastName]`. L'expression suivante permet d'afficher le contenu de la première instance de la zone de texte intitulée `TXT_LastName`:  
  
     `=First(ReportItems!TXT_LastName.Value)`  
  
 Vous ne pouvez pas utiliser de fonctions d'agrégation pour des champs figurant dans les en-têtes ou pieds de page du rapport. Vous pouvez utiliser uniquement une fonction d'agrégation pour les éléments qui apparaissent effectivement dans le corps du rapport. Pour plus d’informations sur les expressions couramment utilisées dans les en-têtes et pieds de page, consultez [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md).  
  
#### <a name="adding-a-data-bound-image-to-a-header-or-footer"></a>Ajout d'une image liée aux données à un en-tête ou à un pied de page  
 Vous pouvez utiliser des données d'image stockées dans la base de données d'en-têtes ou de pieds de page spécifiques. Cependant, vous ne pouvez pas référencer les champs de base de données directement à partir d'un élément d'image figurant dans le rapport. Pour ce faire, vous devez d'abord ajouter une zone de texte dans le corps du rapport et la définir sur un champ de données contenant l'image (notez que l'encodage de cette valeur doit être de type base64). Vous pouvez masquer la zone de texte dans le corps du rapport pour éviter de montrer l'image à l'encodage base64. Vous pouvez ensuite référencer la valeur de la zone de texte masquée depuis l'élément de rapport d'image dans l'en-tête ou le pied de page de la page.  
  
 Imaginons, par exemple, que vous avez un rapport constitué de pages d'informations sur le produit. Dans l'en-tête de chaque page, vous souhaitez afficher une photo du produit. Pour imprimer une image stockée dans l’en-tête du rapport, définissez une zone de texte masquée intitulée `TXT_Photo` dans le corps du rapport qui récupère cette image à partir de la base de données, puis utilisez une expression pour lui affecter une valeur :  
  
 `=Convert.ToBase64String(Fields!Photo.Value)`  
  
 Dans l’en-tête, ajoutez un élément de rapport d’image qui utilise la zone de texte `TXT_Photo` décodée afin que l’image puisse être affichée :  
  
 `=Convert.FromBase64String(ReportItems!TXT_Photo.Value)`  
  
## <a name="using-headers-and-footers-to-position-text"></a>Utilisation d'en-têtes et de pieds de page pour positionner du texte  
 Vous pouvez utiliser des en-têtes et des pieds de page pour positionner du texte dans une page. Imaginons, par exemple, que vous créez un rapport à envoyer aux clients. Vous pouvez utiliser un en-tête ou un pied de page pour positionner l'adresse du client de manière à ce qu'elle s'affiche dans la fenêtre d'une enveloppe pliée.  
  
 Si vous utilisez uniquement la zone de texte pour remplir un en-tête ou un pied de page, vous pouvez masquer la zone de texte dans le corps du rapport. Le positionnement de la zone de texte dans le corps du rapport peut affecter l'affichage de la valeur sur l'en-tête ou le pied de page pour la première ou la dernière page d'un rapport. Par exemple, si le rapport couvre plusieurs pages en raison de la présence de tables, matrices ou listes, la valeur de la zone de texte masquée s'affiche sur la dernière page. Si vous souhaitez un affichage sur la première page, placez la zone de texte masquée en haut du corps du rapport.  
  
## <a name="designing-reports-with-page-headers-and-footers-for-specific-renderers"></a>Conception de rapports à l'aide d'en-têtes et de pieds de page pour des convertisseurs spécifiques  
 Une fois le rapport traité, les données et les informations de disposition sont associées. Lorsque vous consultez un rapport, les informations associées sont passées à un convertisseur qui détermine la quantité de données qui s'affiche sur chacune des pages du rapport.  
  
 Si vous consultez un rapport stocké sur le serveur de rapports à l'aide d'un navigateur, le convertisseur HTML contrôle le contenu qui s'affiche sur les pages consultées. Si vous envisagez de créer des rapports dans un format autre que celui utilisé pour les afficher ou si vous souhaitez les imprimer dans un format spécifique, vous souhaiterez probablement optimiser leur disposition en fonction du convertisseur que vous envisagez d'utiliser pour leur format final. Pour plus d’informations sur la pagination des rapports, consultez [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
### <a name="working-with-page-headers-and-footers-in-excel"></a>Utilisation des en-têtes et des pieds de page dans Excel  
 Lors de la définition des en-têtes et des pieds de page pour les rapports qui ciblent l'extension de rendu Excel, suivez ces directives pour maximiser les résultats :  
  
-   Utilisez les pieds de page pour afficher les numéros de page.  
  
-   Utilisez les en-têtes de page pour afficher les images, les titres ou un autre contenu textuel. N'insérez pas de numéros de page dans l'en-tête.  
  
 Dans Excel, la mise en page des pieds de page est limitée. Si vous définissez un rapport comportant des éléments complexes dans le pied de page, son traitement ne répondra pas à vos attentes lorsqu'il sera consulté dans Excel.  
  
 L'extension de rendu Excel peut accueillir des images et un positionnement absolu pour des éléments de rapports simples ou complexes dans l'en-tête de page. Une prise en charge restreinte des numéros de pages calculés dans l'en-tête représente un effet secondaire associé à la prise en charge d'une mise en page plus élaborée de l'en-tête de page. Dans l'extension de rendu Excel, les paramètres par défaut se soldent par le calcul des numéros de pages en fonction du nombre de classeurs. En fonction de votre mode de définition du rapport, vos numéros de pages risquent d'être erronés. Imaginons, par exemple, que votre rapport rend un seul grand classeur qui s'imprime sur quatre pages. Si vous insérez des informations sur les numéros de page dans l'en-tête, chaque page imprimée affichera « Page 1 de 1 » dans l'en-tête.  
  
 Un nombre de pages plus précis dépend des pages logiques qui mettent en corrélation les dimensions d'une page imprimée. Dans Excel, le pied de page utilise les numéros de pages logiques automatiquement. Pour placer le nombre de pages logique dans l'en-tête, vous devez configurer les paramètres d'informations du dispositif en vue d'utiliser des en-têtes simples. Sachez que lorsque vous utilisez des en-têtes simples, il devient impossible de gérer une mise en page de rapport complexe dans la région de l'en-tête.  
  
 Pour plus d’informations, consultez [Exportation vers Microsoft Excel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Incorporer une image dans un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/embed-an-image-in-a-report-report-builder-and-ssrs.md)   
 [Rectangles et lignes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rectangles-and-lines-report-builder-and-ssrs.md)  
  
  
