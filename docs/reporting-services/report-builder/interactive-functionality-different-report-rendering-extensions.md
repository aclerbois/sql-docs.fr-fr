---
title: Fonctionnalité interactive - Différentes extensions de rendu de rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f0bd1c4c-e8b5-467f-b5a1-541f19c7e3e2
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: ba78a5b3c2d5058723eef41a2c6ca1b98c13160b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="interactive-functionality---different-report-rendering-extensions"></a>Fonctionnalité interactive - Différentes extensions de rendu de rapports
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] propose des fonctionnalités permettant d’interagir avec un rapport paginé au moment de l’exécution. Certains formats de rendu de rapport ne prennent pas en charge toutes les fonctionnalités interactives. Le tableau suivant permet de comprendre le fonctionnement de chaque fonctionnalité interactive dans des formats spécifiques.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="interactive-features-in-different-output-formats"></a>Fonctionnalités interactives dans différents formats de sortie  
 Les rapports paginés qui sont restitués aux formats XML, CSV ou Image ne prennent pas en charge les fonctionnalités interactives.  
  
 Les rapports que vous affichez dans le portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , les composants WebPart de SharePoint ou dans un navigateur sont restitués au format HTML. HTML et Windows Forms sont les seuls formats de sortie de rapport qui prennent entièrement en charge toutes les fonctionnalités interactives. Les autres formats HTML (par exemple MHTML) prennent en charge de nombreuses fonctionnalités interactives. S'il existe des exceptions pour MHTML, elles sont indiquées dans le tableau suivant.  
  
### <a name="document-maps"></a>Explorateur de documents  
  
|Option d'exportation|Informations de support|  
|-------------------|-------------------------|  
|Aperçu/Visionneuse de rapports, HTML|L'explorateur de documents interactif fournit un volet de navigation de liens hiérarchiques pouvant être utilisés pour accéder aux différentes sections d'un rapport.|  
|PDF|L'extension PDF restitue un plan de document sous la forme du volet Signets. Tous les éléments du plan de document sont listés l'un après l'autre dans le volet. Il inclut une hiérarchie de liens. Si une plage de pages est spécifiée, seuls les signets qui sont rendus existent dans la hiérarchie.|  
|Excel|Excel rend un explorateur de documents sous la forme de la première feuille de calcul du classeur. Il inclut une hiérarchie de liens. Lors d'un clic sur un lien dans l'explorateur de documents, la cellule cible appropriée dans la feuille de calcul correspondante est ouverte.|  
|Word|Word rend l'explorateur de documents sous la forme d'étiquettes Table des matières.|  
|Autre (par exemple, TIFF, XML et CSV)|Non disponible en MHTML, XML, CSV ou Image.|  
  
### <a name="drillthrough-links-to-other-reports"></a>Liens d'extraction vers d'autres rapports  
  
|Option d'exportation|Informations de support|  
|-------------------|-------------------------|  
|Aperçu/Visionneuse de rapports, HTML|Les utilisateurs cliquent sur des valeurs de données dans le rapport pour visualiser des données associées dans un autre rapport.|  
|PDF|Les liens d'extraction ne sont pas disponibles dans les PDF. Envisagez plutôt l'utilisation de liens hypertexte qui renvoient à d'autres pages pour les rapports PDF.|  
|Excel|Les liens d'extraction sont rendus dans Excel.<br /><br /> Le lien devient un lien hypertexte pointant vers le rapport référencé par le lien d'extraction. Un clic sur le lien ouvre un rapport dans une fenêtre de navigateur.|  
|Word|Les liens d'extraction sont rendus dans Word.<br /><br /> Le lien devient un lien hypertexte pointant vers le rapport référencé par le lien d'extraction. Un clic sur le lien ouvre un rapport dans une fenêtre de navigateur.|  
|Autres|Non disponible en XML, CSV ou Image.|  
  
### <a name="toggle-items-within-a-report"></a>Éléments bascule dans un rapport  
  
|Option d'exportation|Informations de support|  
|-------------------|-------------------------|  
|Aperçu/Visionneuse de rapports, HTML|Les utilisateurs cliquent sur des icônes de développement et de réduction pour afficher des sections du rapport.|  
|PDF|Le serveur de rapports exporte la vue actuelle du rapport au format PDF. Le basculement interactif n'est pas pris en charge|  
|Excel|Les liens et éléments d'exploration qui peuvent être basculés sont rendus sous forme de plans réductibles dans Excel. Vous pouvez développer et réduire des sections du rapport dans Excel. Pour plus d’informations sur les limitations imposées par Excel, consultez [Exportation vers Microsoft Excel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md).|  
|Word|Le serveur de rapports exporte la vue actuelle du rapport au format PDF. Le basculement interactif n'est pas pris en charge|  
|Autres|Non disponible en MHTML, XML ou CSV. Lors de l'exportation au format Image, le serveur de rapports exporte l'état actuel (afficher ou masquer) du rapport en PDF. Le basculement interactif n'est pas pris en charge.|  
  
### <a name="interactive-sorting"></a>Tri interactif  
  
|Option d'exportation|Informations de support|  
|-------------------|-------------------------|  
|Aperçu/Visionneuse de rapports, HTML|Pour les rapports tabulaires, les utilisateurs cliquent sur des flèches de tri sur une colonne pour changer le mode de tri des données.|  
|PDF|Non disponible dans le format PDF.|  
|Excel|Non disponible dans Excel.|  
|Word|Non disponible dans Word.|  
|Autres|Non disponible en MHTML, XML, CSV ou Image.|  
  
### <a name="hyperlinks-to-external-web-content-or-images"></a>Liens hypertexte vers du contenu ou des images Web externes  
  
|Option d'exportation|Informations de support|  
|-------------------|-------------------------|  
|Aperçu/Visionneuse de rapports, HTML|Les utilisateurs cliquent sur des liens pour ouvrir des pages Web externes dans une nouvelle fenêtre de navigateur.|  
|PDF|Les liens hypertexte sont rendus par l'extension de rendu PDF. Lorsqu'un utilisateur clique sur un lien hypertexte, les pages liées sont ouvertes dans le navigateur.|  
|Excel|Les liens hypertexte sont rendus dans Excel.|  
|Word|Les liens hypertexte sont rendus dans Word.|  
|Autres|Les liens hypertexte ne sont pas disponibles en MHTML, XML, CSV ou Image.<br /><br /> Pour MHTML et Image, les images externes sont rendues sous forme d'image statique.|  
  
### <a name="bookmark-or-anchor"></a>Signet ou ancrage  
  
|Option d'exportation|Informations de support|  
|-------------------|-------------------------|  
|Aperçu/Visionneuse de rapports, HTML|Les utilisateurs cliquent sur des liens pour naviguer vers une autre section du même rapport.|  
|PDF|Non disponible dans le format PDF.|  
|Excel|Les signets sont rendus dans Excel.<br /><br /> Le signet devient un lien hypertexte pointant vers le nom de l'élément du rapport.|  
|Word|Les signets sont rendus dans Word.<br /><br /> Le signet devient un lien hypertexte pointant vers l'élément du rapport marqué d'un signet. Seuls 40 caractères d'un signet ou d'un nom d'ancre sont convertis lorsque le rapport est exporté, ce qui peut créer des signets ou des noms d'ancre en double. Les espaces sont convertis en traits de soulignement (_).|  
|Autres|Non disponible en XML, CSV ou Image.|  
  
### <a name="prompted-parameters-obtained-at-run-time"></a>Paramètres demandés obtenus au moment de l'exécution  
  
|Option d'exportation|Informations de support|  
|-------------------|-------------------------|  
|Aperçu/Visionneuse de rapports, HTML|Une zone d'entrée de paramètres apparaît en haut du rapport. Cette zone fait partie de la visionneuse HTML utilisée pour afficher des rapports dans un navigateur.|  
|PDF|Le serveur de rapports exporte le rapport au format PDF en utilisant les valeurs des paramètres actuellement en vigueur pour le rapport.|  
|Excel|Le serveur de rapports exporte le rapport au format Excel en utilisant les valeurs des paramètres actuellement en vigueur pour le rapport.|  
|Word|Le serveur de rapports exporte le rapport au format Word en utilisant les valeurs des paramètres actuellement en vigueur pour le rapport.|  
|Autres|Le serveur de rapports exporte le rapport à d'autres formats en utilisant les valeurs des paramètres actuellement en vigueur pour le rapport.|  
  
### <a name="filters-applied-at-run-time"></a>Filtres appliqués au moment de l'exécution  
  
|Option d'exportation|Informations de support|  
|-------------------|-------------------------|  
|Aperçu/Visionneuse de rapports, HTML|Les données filtrées sont présentées à l'utilisateur au moment de l'exécution.|  
|PDF|Le serveur de rapports exporte le rapport au format PDF en utilisant les données filtrées dans le rapport actuel.|  
|Excel|Le serveur de rapports exporte le rapport au format Excel en utilisant les données filtrées dans le rapport actuel.|  
|Word|Le serveur de rapports exporte le rapport au format Word en utilisant les données filtrées dans le rapport actuel.|  
|Autres|Le serveur de rapports exporte le rapport à d'autres formats en utilisant les données filtrées dans le rapport actuel.|  
  
## <a name="see-also"></a> Voir aussi  
 [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)   
 [Tri interactif, Explorateurs de documents et liens &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/interactive-sort-document-maps-and-links-report-builder-and-ssrs.md)   
    
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Graphiques (Générateur de rapports et SSRS)](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)  
  
  
