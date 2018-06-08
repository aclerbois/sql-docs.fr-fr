---
title: Rendu au format HTML (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: cf559b0a-499a-4d74-b520-b382b87e0b17
caps.latest.revision: 10
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 63f7f18921d856753162a22e8fbf079834105164
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rendering-to-html-report-builder-and-ssrs"></a>Rendu au format HTML (Générateur de rapports et SSRS)
  L’extension de rendu HTML effectue le rendu d’un rapport paginé au format HTML. Elle peut également produire des pages HTML entièrement formées ou des fragment HTML à incorporer dans d'autres pages HTML. La sortie HTML est générée avec l'encodage UTF-8.  
  
 L’extension de rendu HTML est l’extension de rendu par défaut pour les rapports qui sont affichés dans un navigateur, notamment lors d’une exécution dans le portail web [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] .  
  
 L’extension de rendu HTML est l’extension de rendu par défaut pour les rapports qui sont affichés dans un navigateur, notamment lors d’une exécution dans le portail web [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] . L'extension de rendu HTML peut rendre un document HTML complet ou un fragment. Dans le cas d’un fragment HTML, les balises **HEAD**, **HTML**et **BODY** du document HTML sont supprimées. Seul le contenu de la balise **BODY** est affiché. Ceci est utile pour incorporer le code HTML à celui généré par une autre application.  
  
 Dans certains scénarios, les paramètres de rapport peuvent être utilisés pour lancer des attaques par injection de script lors du rendu de rapports en HTML. Pour plus d’informations sur la sécurisation des rapports, consultez [Sécurisation des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md).  
  
 Pour plus d’informations sur les navigateurs, consultez [Planification de la prise en charge des navigateurs pour Reporting Services et Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="RenderingMHTML"></a> Rendu au format MHTML  
 L'extension de rendu HTML peut également rendre des rapports au format MHTML (MIME Encapsulation of Aggregate HTML Documents). MHTML étend HTML pour intégrer des objets encodés, comme les images, dans le document HTML. Avec l'extension de rendu MHTML, vous pouvez incorporer dans un fichier unique des ressources telles que des images, des documents ou d'autres fichiers binaires en tant que structures MIME dans le rapport HTML. L'incorporation de rapports MHTML dans des messages électroniques se révèle également utile, car toutes les ressources sont incluses dans le rapport. Bien que ce soit en fait l'extension de rendu HTML qui assure le rendu MHTML, cette fonctionnalité peut également être appelée extension de rendu MHTML.  
  
  
##  <a name="BrowserSupport"></a> Prise en charge des navigateurs  
 Cette extension de rendu prend en charge les versions de navigateur suivantes :  
  
-   Internet Explorer 5.5 et versions ultérieures  
  
-   Firefox 1.5 et versions ultérieures  
  
-   Safari 3.0 et versions ultérieures  
  
 En raison des spécificités des navigateurs, le rapport rendu peut varier légèrement d'un navigateur à l'autre. Par exemple, la zone de texte contient une propriété nommée WritingMode. Cette propriété n'est pas prise en charge dans Firefox.  
  
  
##  <a name="HTMLSpecificRenderingRules"></a> Règles de rendu spécifiques à HTML  
 Les règles spécifiques à HTML suivantes s'appliquent lors du rendu :  
  
-   Le convertisseur génère une structure de table HTML destinée à contenir tous les éléments de chaque collection **ReportItems** , s’il en existe plusieurs.  
  
-   Chaque élément dans la structure de table occupe une cellule unique.  
  
-   Les cellules vides sont réduites autant que possible pour réduire la taille de la table HTML.  
  
-   Une ligne de cellules vides est ajoutée sur le bord supérieur et une autre colonne sur le bord gauche pour améliorer la vitesse à laquelle les navigateurs peuvent rendre la table.  
  
-   Des largeurs et hauteurs fixes sont attribuées aux lignes ou colonnes de la table qui ne contiennent aucun élément, seulement des espaces entre les éléments.  
  
-   Toutes les autres lignes et colonnes peuvent s'agrandir selon la taille de chaque élément de rapport.  
  
-   Toutes les coordonnées et tailles d'élément de rapport sont converties en millimètres. Toutes les autres tailles, notamment les propriétés de style, conservent leurs unités d'origine. Les différences de taille et position inférieures à 0,2 mm sont traitées comme 0 mm.  
  
  
##  <a name="Interactivity"></a> Interactivité  
 Certains éléments interactifs sont pris en charge au format HTML. Vous trouverez ci-dessous une description de comportements spécifiques.  
  
### <a name="show-and-hide"></a>Afficher et masquer  
 Un élément de rapport dont la visibilité peut être activée/désactivée est rendu par une image bascule +/- et est interactif. Lorsque l'utilisateur clique sur l'élément, un rappel au serveur a lieu pour rendre à nouveau la sortie avec l'état d'affichage ou de masquage modifié.  
  
### <a name="document-map"></a>Explorateur de documents  
 Les étiquettes Explorateur de documents sont rendues et accessibles par navigation en utilisant l'Explorateur de documents dans le contrôle de visionneuse. Pour les en-têtes de région de données omis, les étiquettes sont rendues dans la première cellule enfant. En l'absence de cellule enfant, l'étiquette est rendue dans l'enfant qui précède.  
  
### <a name="bookmarks"></a>Signets  
 Les liens de signet sont rendus et apparaissent sous la forme de liens hypertexte. Les cibles de signet sont rendues et accessibles par navigation en cliquant sur les liens de signet. Lorsque l'utilisateur clique sur un lien de signet, le rapport accède à la première occurrence de l'étiquette de signet cible et, lorsque cela est possible, un défilement a lieu dans le navigateur afin que le lien de signet soit en haut de la fenêtre. Des balises d’ancrage HTML (\<a>) sont utilisées pour marquer les cibles de signet.  
  
### <a name="interactive-sorting"></a>Tri interactif  
 Si un tri utilisateur est défini pour une zone de texte, l'extension de rendu HTML rend les icônes de tri dans la zone de texte à droite de son contenu. Si un rapport contient une zone de texte où le tri utilisateur est défini, du code JavaScript est rendu qui provoque une publication sur le serveur lorsque l'image de tri fait l'objet d'un clic.  
  
### <a name="hyperlinks-and-drillthrough"></a>Liens hypertexte et extraction  
 Les liens hypertexte et les liens d’extraction sont rendus sous la forme de liens hypertexte dans les éléments de rapport à l’aide des balises d’ancrage HTML (\<a>) placées autour de l’élément sur lequel ils sont définis.  
  
### <a name="search"></a>Recherche  
 La fonctionnalité de recherche permet aux utilisateurs de rechercher une chaîne de texte dans le rapport.  
  
 Des fonctionnalités supplémentaires de recherche sont fournies par le contrôle Web Forms ReportViewer.  
  
  
##  <a name="DeviceInfo"></a> Paramètres d'informations de périphérique  
 Vous pouvez modifier certains paramètres par défaut de ce convertisseur, notamment le mode de rendu, en modifiant les paramètres d'informations de périphérique. Pour plus d’informations, consultez [Paramètres d’informations de périphérique HTML](../../reporting-services/html-device-information-settings.md).  
  
  
## <a name="see-also"></a> Voir aussi  
 [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Fonctionnalités interactives des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Rendu des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
