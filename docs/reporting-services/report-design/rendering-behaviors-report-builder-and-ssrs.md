---
title: Comportements de rendu (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 8f873ef9-27a3-40e5-b58b-6774f8027a58
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 4ddec1ab7e8252ff65beb241cfd71602fbe00465
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rendering-behaviors-report-builder--and-ssrs"></a>Comportements de rendu (Générateur de rapports et SSRS)
  En fonction du convertisseur que vous sélectionnez, certaines règles sont appliquées au corps du rapport et à son contenu lors du rendu d'un rapport. La combinaison de ces facteurs détermine la façon dont les éléments du rapport s'ajustent sur la page :  
  
-   les règles de rendu ;  
  
-   la largeur et la hauteur des éléments de rapport ;  
  
-   la taille du corps du rapport ;  
  
-   la largeur et la hauteur de la page ;  
  
-   la prise en charge de la pagination spécifique au convertisseur.  
  
 Cette rubrique décrit les règles générales appliquées par Reporting Services. Pour plus d’informations, consultez [Rendu des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md), [Rendu des régions de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-data-regions-report-builder-and-ssrs.md) et [Rendu des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-data-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="general-behaviors-for-html-mhtml-word-and-excel-soft-page-break-renderers"></a>Comportements généraux pour HTML, MHTML, Word et Excel (convertisseurs de sauts de page conditionnels)  
 Les rapports exportés à l'aide des formats HTML et MHTML sont optimisés pour un affichage sur un écran d'ordinateur sur lequel les pages peuvent avoir différentes longueurs. Les sauts de page sont insérés verticalement à des emplacements approximatifs au sein du corps du rapport. Ces emplacements sont déterminés par le paramètre de hauteur interactif du volet Propriétés. Par exemple, la hauteur interactive est définie à 5 pouces. Lorsque le rapport est rendu, la hauteur de page est d'environ 5 pouces. Word et Excel paginent en fonction des sauts de page logiques et ignorent le paramètre de hauteur interactive.  
  
> [!NOTE]  
>  Pour déterminer comment un rapport s'affichera dans un convertisseur de saut de page conditionnellele, utilisez l'Aperçu de rapport. Le rapport s'affiche exactement comme au format HTML, MHTML, Word ou Excel.  
  
 Lors de l'exportation d'un rapport en HTML, MHTML, dans Word ou dans Excel, les règles générales suivantes s'appliquent :  
  
-   Les sauts de page logiques, les sauts de page que vous insérez explicitement, sont appliqués aux éléments de rapport. Par exemple, si vous insérez un saut de page entre chaque groupe, ils s'appliquent lorsque le rapport est rendu.  
  
-   Une mise en page approximative est créée à l'aide de la hauteur de page et du nombre d'éléments de rapport affichés. Par exemple, si une zone de texte de 0,5 pouces est répétée cinq fois dans le rapport, 2,5 pouces sont réservés.  
  
-   Plusieurs sauts de page conditionnels sont insérés en fonction du paramètre de hauteur interactive. Pour supprimer en HTML et dans les contrôles ReportViewer et la pagination de contrôle uniquement avec des sauts de page explicites, définissez la valeur de la **hauteur interactive** sur 0 ou sur un nombre très grand.  
  
    > [!NOTE]  
    >  Le paramètre de largeur interactive n'est pas utilisé dans les convertisseurs de saut de page conditionnellele.  
  
-   Les pages de rapport peuvent être agrandies pour contenir les veuves, orphelins et éléments de rapport qui doivent rester ensemble. Cela signifie que le rapport peut s'étendre au delà de la largeur de l'écran et peut être affiché en utilisant les barres de défilement.  
  
-   La pagination est appliquée aux rapports uniquement verticalement.  
  
-   Les marges de page ne sont pas appliquées.  
  
## <a name="general-behaviors-for-pdf-image-and-print-hard-page-break-renderers"></a>Comportements généraux pour PDF, Image et Impression (Convertisseurs de sauts de page manuels)  
 Les rapports exportés au format PDF et Image sont optimisés pour un affichage sous forme de livre ou pour l'impression avec des pages de taille identique. Les sauts de page sont insérés verticalement et horizontalement à des emplacements spécifiques au sein du corps du rapport. Ces emplacements spécifiques sont déterminés par les paramètres de largeur et de hauteur de la page.  
  
> [!NOTE]  
>  Pour déterminer comment un rapport s'affichera dans un convertisseur de saut de page manuel, utilisez l'Aperçu avant impression. Le rapport apparaît comme au format PDF ou Image.  
  
-   Les pages sont numérotées de façon séquentielle de gauche à droite, puis de haut en bas.  
  
-   Les sauts de page logiques, les sauts de page que vous insérez explicitement, sont appliqués aux éléments de rapport. Ces sauts de page peuvent pousser des éléments de rapport sur la page suivante.  
  
-   Si un saut de page physique se produit au milieu d'éléments de rapport qui doivent être conservés ensemble, ces éléments sont déplacés sur la page suivante.  
  
-   En raison des contraintes de taille de page, il peut ne pas être possible de conserver tous les éléments ensemble ou de répéter des éléments. Si cela se produit, le convertisseur peut ignorer certaines règles de répétition d'un élément afin que l'élément de rapport s'ajuste à la page.  
  
-   Si un élément ne peut pas être conservé sur une même page, par exemple une zone de texte qui devient trop importante pour s'ajuster dans la zone de page utilisable verticale, alors l'élément sera découpé le long de la limite physique de la page et continuera sur la page suivante.  
  
-   La pagination est appliquée aux rapports verticalement et horizontalement.  
  
    > [!NOTE]  
    >  Le paramètre de largeur interactive n'est pas utilisé dans les convertisseurs de saut de page manuel.  
  
## <a name="minimum-spacing-between-report-items"></a>Espacement minimum entre éléments de rapport  
 Les éléments de rapport grandissent au sein du corps du rapport pour s'ajuster à leur contenu. Par exemple, une région de données de matrice s'agrandit en largeur et en hauteur sur la page lorsque le rapport est rendu et la hauteur d'une zone de texte s'ajuste en fonction des données rendues à partir d'une expression.  
  
 Les convertisseurs conservent un espace minimum entre les éléments de rapport que vous définissez dans la mise en page du rapport. Lorsque vous placez un élément de rapport à côté d'un autre sur la mise en page du rapport, la distance entre les éléments de rapport correspond à la distance minimum qui doit être conservée au fur et à mesure que le rapport s'agrandit horizontalement ou verticalement. Par exemple, si vous ajoutez une région de données de matrice à un rapport puis ajoutez un rectangle de 0,25 pouces à droite de la matrice, cet espace est conservé lorsque la matrice s'agrandit. Chaque élément est déplacé vers la droite pour conserver l'espace minimal entre les éléments qui se terminent à gauche de l'élément.  
  
## <a name="page-headers-and-footers"></a>En-têtes et pieds de page  
 Les en-têtes et pieds de page apparaissent au haut et en bas de chaque page rendue. Vous pouvez mettre en forme l'en-tête et le pied de page en définissant une couleur de bordure, un style de bordure et une largeur de bordure. Vous pouvez également ajouter une couleur d'arrière-plan ou une image d'arrière-plan. Ces options de mise en forme sont toutes rendues, en fonction du format que vous choisissez.  
  
 Les règles suivantes s'appliquent aux en-têtes et pieds de page lorsqu'ils sont rendus au format HTML ou MHTML :  
  
> [!NOTE]  
>  Pour plus d’informations sur la façon dont Excel rend les en-têtes et les pieds de page, consultez [Exportation vers Microsoft Excel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md). Pour plus d’informations sur la façon dont Word rend les en-têtes et les pieds de page, consultez [Exportation vers Microsoft Word &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md).  
  
-   Lorsqu'ils existent, l'en-tête et le pied de page sont rendus au haut et en bas de chaque page au sein de la zone de page utilisable.  
  
-   Sur les pages où l'en-tête ou le pied de page est masqué, la hauteur de l'en-tête ou du pied de page est réservée au sein de la zone de page utilisable, même si l'en-tête ou le pied de page n'est pas rendu.  
  
-   Si le contenu de l'en-tête ou du pied de page grandit au delà des limites de l'en-tête ou du pied de page, la taille de l'en-tête ou du pied de page augmente pour s'ajuster au contenu.  
  
 Les règles suivantes s'appliquent aux en-têtes et aux pieds de page lorsqu'ils sont rendus au format PDF ou Image :  
  
-   L'en-tête et le pied de page sont rendus au haut et en bas de chaque page au sein de la zone de page utilisable.  
  
-   Sur les pages où l'en-tête ou le pied de page est masqué, la hauteur de l'en-tête ou du pied de page est réservée au sein de la zone de page utilisable, même si l'en-tête ou le pied de page n'est pas rendu.  
  
-   La taille de l'en-tête et du pied de page n'est ni réduite, ni agrandie. Ils sont rendus sur chaque page à la hauteur spécifiée quand vous créez l'en-tête ou le pied de page.  
  
-   Indépendamment du nombre de colonnes du rapport, il n'y a qu'un seul en-tête et pied de page par page.  
  
-   Si le contenu de l'en-tête ou du pied de page grandit au delà des limites de l'en-tête ou du pied de page, le contenu est découpé.  
  
-   Les en-têtes et les pieds de page définis dans le fichier RDL d'origine ne sont pas rendus lorsque le rapport est rendu sous la forme d'un sous-rapport.  
  
## <a name="logical-page-breaks"></a>Sauts de page logiques  
 Les sauts de page logiques sont des sauts de page que vous insérez avant ou après les éléments ou groupes de rapport. Les sauts de page permettent de déterminer la façon d'ajuster le contenu sur une page de rapport pour un affichage optimal lors du rendu ou de l'exportation du rapport.  
  
 Les règles suivantes s'appliquent lors du rendu des sauts de page logiques :  
  
-   Les sauts de page logiques sont ignorés pour les éléments de rapport masqués et pour les éléments de rapport où la visibilité est contrôlée en cliquant sur un autre élément de rapport.  
  
-   Les sauts de page logiques sont appliqués sur les éléments visibles de façon conditionnelle s'ils sont visibles au moment où le rapport est rendu.  
  
-   L'espace est conservé entre l'élément de rapport avec le saut de page logique et ses éléments de rapport homologues.  
  
-   Les sauts de page logiques insérés avant un élément de rapport décalent l'élément de rapport sur la page suivante. L'élément de rapport est rendu en haut de la page suivante.  
  
-   Les sauts de page logiques définis sur les éléments de cellules de tableau ou de matrice ne sont pas conservés. Cela ne s'applique pas aux éléments de listes.  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalité interactive des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Rendu au format HTML &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md)   
 [Mise en page et rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/page-layout-and-rendering-report-builder-and-ssrs.md)  
  
  
