---
title: Rendu des éléments de rapport (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 99ebb4dc-41cc-42ac-82dd-a2b0e31155a0
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: b85b4ffe4e60e8f815e5cff0dd8953160791ed8f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rendering-report-items-report-builder-and-ssrs"></a>Rendu des éléments de rapport (Générateur de rapports et SSRS)
  Le nombre, la taille et l'emplacement des éléments de rapport affectent la façon dont les convertisseurs mettent en page le corps du rapport. Vous trouverez ci-dessous une description de la façon dont les éléments de rapport sont rendus.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="overlapping-report-items"></a>Éléments de rapport qui se chevauchent  
 Les éléments de rapport qui se chevauchent ne sont pas pris en charge en HTML, MHTML, dans Word, dans Excel, en mode Aperçu ou dans la Visionneuse de rapports. Si des éléments se chevauchent, ils sont déplacés. Les règles suivantes sont appliquées aux éléments de rapport qui se chevauchent :  
  
-   Si le chevauchement vertical d'éléments de rapport est plus grand, l'un des éléments qui se chevauchent est déplacé vers la droite. L'élément le plus de gauche reste à sa place.  
  
-   Si le chevauchement horizontal d'éléments de rapport est plus grand, l'un des éléments qui se chevauchent est déplacé vers le bas. L'élément le plus haut reste à sa place.  
  
-   Si le chevauchement vertical et le chevauchement vertical sont égaux, l'un des éléments qui se chevauchent est déplacé vers la droite. L'élément le plus de gauche reste à sa place.  
  
-   Si un élément doit être déplacé pour corriger le chevauchement, les éléments de rapport adjacents sont déplacés vers le bas et/ou vers la droite afin de conserver un espacement minimal entre l'élément et les éléments de rapport qui se terminent au-dessus et/ou à gauche de l'élément. Par exemple, deux éléments de rapport se chevauchent verticalement et un troisième élément de rapport se trouve à 2 pouces de leur droite. Lorsque l'élément de rapport dépasse est déplacé vers la droite, le troisième élément de rapport est également déplacé vers la droite afin de maintenir les 2 pouces qui le séparent de l'élément de rapport situé à sa gauche.  
  
 Les éléments de rapport qui se chevauchent sont pris en charge dans les formats de saut de page manuel, notamment l'impression.  
  
## <a name="visibility-and-report-items"></a>Visibilité et éléments de rapport  
 Les éléments de rapport peuvent être masqués ou affichés par défaut ou de façon conditionnelle à l'aide d'expressions. La visibilité peut également être basculée en cliquant sur un autre élément de rapport.  
  
 Les règles de visibilité suivantes s'appliquent lors du rendu des éléments de rapport :  
  
-   Si un élément de rapport et son contenu sont toujours masqués (il n'est pas masqué sur la base d'une expression ou sa visibilité ne peut pas être basculée en cliquant sur un autre élément de rapport), alors les autres éléments de rapport situés à droite ou au-dessous ne sont pas déplacés pour remplir l'espace vide. Par exemple, si un rectangle et l'image contenus dans l'élément sont masqués, l'élément de rapport qui commence à droite du rectangle n'est pas déplacé vers la gauche pour remplir ce qui semble être un espace vide. L'espace occupé par le rectangle est conservé.  
  
-   Si un élément de rapport et son contenu sont masqués de façon conditionnelle (il est masqué sur la base d'une expression ou sa visibilité est basculée en cliquant sur un autre élément de rapport), alors les éléments de rapport situés à droite ou au-dessous sont déplacés vers la gauche pour remplir l'espace lorsque l'élément est masqué.  
  
-   Si la visibilité d'un élément de rapport et son contenu peuvent être basculés en cliquant sur un autre élément de rapport, alors la pagination change pour accommoder l'élément de rapport et son contenu uniquement lorsqu'il est affiché au début.  
  
## <a name="keeping-report-items-together-on-a-single-page"></a>Conserver les éléments de rapport ensemble sur une page unique  
 De nombreux éléments de rapport au sein d'un rapport peuvent être conservés ensemble sur une page de façon implicite ou explicite en définissant les propriétés KeepWithGroup ou KeepTogether. Les éléments de rapport sont toujours rendus sur la même page si l'élément de rapport ne comporte pas de sauts de page logiques et est plus petit que la taille de la zone de page utilisable. Si un élément de rapport ne s'ajuste pas totalement dans la page sur laquelle il commence, un saut de page manuel est inséré avant l'élément de rapport, ce qui le force à passer sur la page suivante. Pour les convertisseurs de saut de page conditionnellele, la page est agrandie pour accommoder l'élément de rapport.  
  
 Lorsque l'élément de rapport est toujours masqué, les règles pour conserver les éléments ensemble sont ignorées.  
  
 Les éléments suivants sont toujours conservés ensemble :  
  
-   Les images.  
  
-   Lignes.  
  
-   Les graphiques, les jauges et les plans.  
  
-   Une ligne unique dans une région de données qui apparaît séparément sur une autre page, en sélectionnant l'option Conserver avec le groupe. Cela conservera implicitement ensemble la ligne avec au moins une instance du groupe afin que la ligne ne soit pas orpheline. Vous pouvez définir cette option sur une région de données ou un groupe.  
  
-   La zone d'en-tête d'une région de données.  
  
-   La zone d'en-tête d'une région de données et la première ligne de données.  
  
-   Les éléments de rapport qui peuvent être basculés dans une région de données de tableau matriciel.  
  
### <a name="priority-order"></a>Ordre de priorité  
 En raison des limitations de taille de la page, des conflits peuvent se produire entre les règles de conservation des éléments de rapport sur la même page. Lorsque des conflits se produisent, l'ordre de priorité suivant est utilisé pour conserver les éléments sur la même page lors du rendu :  
  
-   Les lignes, les graphiques et les images.  
  
-   Le contrôle veuve et orphelin.  
  
-   Les en-têtes de colonne et de ligne répétés.  
  
     Les en-têtes sont prioritaires sur les pieds de page. Les groupes répétés internes sont prioritaires sur les groupes externes. Les éléments pour lesquels la propriété **RepeatWith** est définie et qui sont plus proches de la région de données cible sont prioritaires sur les éléments plus éloignés de la région de données.  
  
-   Les petits éléments de rapport, tels que les zones de texte ou les rectangles, assortis d’une propriété KeepTogether explicite ayant pour valeur **true**.  
  
-   Les grands éléments de rapport, tels que les sous-rapports ou un membre du tableau matriciel qui n’est pas le plus profond, assortis d’une propriété KeepTogether explicite ayant la valeur **true**.  
  
-   Les régions de données de tableau matriciel assortis d’une propriété KeepTogether explicite ayant la valeur **true**.  
  
### <a name="subreports"></a>Sous-rapports  
 Un sous-rapport est rendu sous la forme d'un rectangle qui contient un autre rapport défini dans un fichier rapport .rdl séparé. Le fichier de sous-rapport doit être publié sur un serveur de rapports avant qu'il soit accessible au rapport parent.  
  
 Les règles suivantes s'appliquent lors du rendu de sous-rapports :  
  
-   Les sous-rapports peuvent atteindre la taille du corps définie dans le fichier .rdl qui définit le sous-rapport. Par exemple, si le fichier RDL pour le sous-rapport déclare que la largeur du corps du sous-rapport est de 5 pouces, la largeur du sous-rapport sera de 5 pouces au sein du rapport parent.  
  
-   Les sous-rapports héritent des paramètres de colonne du rapport parent. Les paramètres de colonne définis dans le fichier RDL d'origine sont toujours ignorés.  
  
-   Seul le corps du sous-rapport est rendu. Les sections d'en-tête et de pied de page définies dans le fichier .rdl du sous-rapport ne sont pas rendues lorsque le sous-rapport est rendu dans le rapport parent.  
  
-   Les sous-rapports ont une propriété KeepTogether explicite. Lorsqu'elle est définie à **true**, tous les éléments du sous-rapport sont conservés sur la même page lorsque cela est possible.  
  
-   Si un sous-rapport ne peut pas être exécuté, il est affiché dans le rapport sous la forme d'une zone de texte avec un message d'erreur. Les propriétés de style appliquées au sous-rapport sont appliquées à la zone de texte.  
  
-   Si le sous-rapport est divisé par un saut de page, le paramètre **Omettre la bordure sur le saut de page** contrôle si les bordures du sous-rapport sont fermées ou ouvertes.  
  
 Pour plus d’informations sur les sous-rapports, consultez [Sous-rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/subreports-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Fonctionnalités interactives des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
