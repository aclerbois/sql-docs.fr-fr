---
title: Exportation vers un fichier PDF (Générateur de rapports et SSRS) | Microsoft Docs | Microsoft Docs
ms.custom: ''
ms.date: 10/21/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f22497b7-f6c1-4c7b-b831-8c731e26ae37
caps.latest.revision: 13
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: f609ff4110827d6165ede8cd39986d4e5dc77971
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="exporting-to-a-pdf-file-report-builder-and-ssrs"></a>Exportation vers un fichier PDF (Générateur de rapports et SSRS)
  L’extension de rendu PDF présente les rapports paginés [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] sous forme de fichiers qui peuvent être ouverts dans des visionneuses telles qu’Adobe Acrobat si elles prennent en charge le format PDF 1.3. Bien que PDF 1.3 soit compatible avec Adobe Acrobat 4.0 et versions ultérieures, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prend en charge Adobe Acrobat 11.0 ou version ultérieure. Cette extension de rendu ne nécessite pas les logiciels Adobe pour effectuer le rendu du rapport. Toutefois, les visionneuses PDF comme Adobe Acrobat sont indispensables pour afficher ou imprimer un rapport au format PDF.  
  
 L'extension de rendu PDF prend en charge les caractères ANSI et peut convertir les caractères Unicode du japonais, coréen, chinois (traditionnel et simplifié), cyrillique, hébreu et arabe. Certaines limitations s'appliquent toutefois. Pour plus d’informations sur les limitations, consultez [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
 Le convertisseur PDF est convertisseur de page physique et, par conséquent, a un comportement de pagination qui diffère d'autres convertisseurs, tels que HTML et Excel. Cette rubrique fournit des informations spécifiques au rendu PDF et décrit les exceptions aux règles.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="FontRequirements"></a> Incorporation de police  
 Lorsque cela est possible, l'extension de rendu PDF incorpore le sous-ensemble de chaque police nécessaire pour afficher le rapport dans le fichier PDF. Les polices utilisées dans le rapport doivent être installées sur le serveur de rapports. Lorsque le serveur de rapports génère un rapport au format PDF, il utilise les informations stockées dans la police référencée par le rapport pour créer les mappages de caractères dans le fichier PDF. Si la police demandée n'est pas installée sur le serveur de rapports, le fichier PDF qui est créé peut ne pas contenir les mappages requis et donc ne pas s'afficher correctement lorsqu'il est ouvert.  
  
 Les polices sont incorporées dans le fichier PDF lorsque les conditions suivantes s'appliquent :  
  
-   Les privilèges d'incorporation de police sont accordés par l'auteur de la police. Les polices installées incluent une propriété qui indique si l'auteur de la police envisage d'autoriser l'incorporation d'une police dans un document. Si la valeur de la propriété est EMBED_NOEMBEDDING, la police n'est pas incorporée dans le fichier PDF. Pour plus d'informations, consultez « TTGetEmbeddingType » sur msdn.microsoft.com.  
  
-   La police est TrueType.  
  
-   Les polices sont référencées par des éléments visibles dans un rapport. Si une police est référencée par un élément dont la propriété Hidden a la valeur True, la police n'est pas nécessaire pour afficher les données rendues et ne sera pas incluse dans le fichier. Les polices sont incorporées uniquement lorsqu'elles sont nécessaires pour afficher les données de rapport rendues.  
  
 Si toutes ces conditions sont remplies pour une police, elle est incorporée dans le fichier PDF. Si une ou plusieurs de ces conditions ne sont pas remplies, la police n'est pas incorporée dans le fichier PDF.  
  
> [!NOTE]  
>  Bien que les conditions soient réunies, il existe un cas dans lequel les polices ne sont pas incorporées dans le fichier PDF. Si les polices utilisées sont celles de la spécification PDF, généralement appelées polices standard de type 1, ou polices Base-14, les polices ne sont pas incorporées pour le contenu ANSI.  
  
  
### <a name="fonts-on-the-client-computer"></a>Polices sur l'ordinateur client  
 Lorsqu'une police est incorporée dans le fichier PDF, l'ordinateur utilisé pour consulter le rapport (l'ordinateur client) n'a pas besoin d'avoir la police installée pour que le rapport s'affiche correctement.  
  
 Lorsqu'une police n'est pas incorporée dans le fichier PDF, l'ordinateur client doit avoir la police appropriée installée pour que le rapport s'affiche correctement. Si la police n'est pas installée sur l'ordinateur client, le fichier PDF affiche un point d'interrogation (?) pour les caractères non pris en charge.  
  
### <a name="verifying-fonts-in-a-pdf-file"></a>Vérification des polices dans un fichier PDF  
 Des différences de sortie PDF se produisent le plus souvent lorsqu'une police qui ne prend pas en charge les caractères non-latins est utilisée dans un rapport et que ces caractères non-latins sont ensuite ajoutés au rapport. Vous devez contrôler la sortie de rendu PDF à la fois sur le serveur de rapports et les ordinateurs clients pour vous assurer que le rendu du rapport est conforme.  
  
 La visualisation du rapport en mode Aperçu ou son exportation en HTML n'est pas fiable, car il aura une apparence correcte en raison de la substitution des polices effectuée automatiquement par l'interface de conception graphique ou par Microsoft Internet Explorer, respectivement. Si des glyphes Unicode sont manquants sur le serveur, des caractères peuvent être remplacés par un point d'interrogation (?). Si une police n’est pas présente sur le client, il se peut que des caractères soient remplacés par des carrés (□).  
  
 Les polices incorporées dans le fichier PDF sont incluses dans la propriété Fonts qui est enregistrée avec le fichier, en tant que métadonnées.  
  
##  <a name="Metadata"></a> Métadonnées  
 En plus de la mise en page du rapport, l'extension de rendu PDF écrit les métadonnées suivantes dans le dictionnaire des informations du document PDF.  
  
|Propriété PDF|Créée à partir de|  
|------------------|------------------|  
|**Title**|Attribut **Name** de l'élément RDL **Report**|  
|**Author**|Élément RDL **Author**|  
|**Subject**|Élément RDL **Description**|  
|**Creator**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|  
|**Producer**|Nom et version de l'extension de rendu|  
|**CreationDate**|Heure de l'exécution du rapport au format **datetime** PDF|  
  
  
##  <a name="Interactivity"></a> Interactivité  
 Certains éléments interactifs sont pris en charge en PDF. Vous trouverez ci-dessous une description de comportements spécifiques.  
  
### <a name="show-and-hide"></a>Afficher et masquer  
 L'affichage et le masquage dynamiques d'éléments ne sont pas pris en charge en PDF. Le document PDF est rendu pour correspondre à l'état actuel des éléments dans le rapport. Par exemple, si l'élément est affiché lorsque le rapport est exécuté, alors l'élément est rendu. Les images qui peuvent être affichées/masquées ne sont pas rendues, si elles sont masquées lorsque le rapport est exporté.  
  
### <a name="document-map"></a>Explorateur de documents  
 Si des étiquettes d'explorateur de documents sont présentes dans le rapport, une structure du document est ajoutée au fichier PDF. Chaque étiquette de l'explorateur de documents apparaît comme une entrée dans la structure du document dans l'ordre dans lequel elle apparaît dans le rapport. Dans Acrobat, un signet cible est ajouté à la structure du document uniquement si la page sur laquelle il se trouve est rendue.  
  
 Si une seule page est rendue, aucune structure du document n'est ajoutée. L'explorateur de documents est organisé de façon hiérarchique pour refléter le niveau d'imbrication du rapport. La structure du document est accessible dans Acrobat sous l'onglet Signets. Un clic sur une entrée dans la structure du document affiche l'emplacement référencé par le signet dans le document.  
  
### <a name="bookmarks"></a>Signets  
 Les signets ne sont pas pris en charge dans le rendu PDF.  
  
### <a name="drillthrough-links"></a>Liens d'extraction  
 Les liens d'extraction ne sont pas pris en charge dans le rendu PDF. Les liens d'extraction ne sont pas rendus comme liens interactifs et les rapports d'extraction ne peuvent pas se connecter à la cible de l'extraction.  
  
### <a name="hyperlinks"></a>Liens hypertexte  
 Les liens hypertexte dans les rapports sont rendus sous forme de liens interactifs dans le fichier PDF. Lors d'un clic, Acrobat ouvrira le navigateur client par défaut et naviguera jusqu'à l'URL du lien hypertexte.  
  
  
##  <a name="Compression"></a> Compression  
 La compression d'image est basée sur le type de fichier d'origine de l'image. L'extension de rendu PDF compresse les fichiers PDF par défaut.  
  
 Pour conserver la compression des images incluses dans le fichier PDF lorsque cela est possible, les images JPEG sont stockées au format JPEG et tous les autres types d'images sont stockés au format BMP.  
  
> [!NOTE]  
>  Les fichiers PDF ne prennent pas en charge l'incorporation d'images PNG.  
  
  
##  <a name="DeviceInfo"></a> Paramètres d'informations de périphérique  
 Vous pouvez modifier certains paramètres par défaut pour ce convertisseur en modifiant les paramètres d'informations de périphérique. Pour plus d'informations, consultez [PDF Device Information Settings](../../reporting-services/pdf-device-information-settings.md).  
  
  
## <a name="see-also"></a> Voir aussi  
 [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Fonctionnalités interactives des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Rendu des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
