---
title: Spécification de chemins vers des éléments externes (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 4fe513da-f3c5-479c-9fec-8662b91a0d6d
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: e1389b60c7aac94cddea21765c41bb715063fc0c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specifying-paths-to-external-items-report-builder-and-ssrs"></a>Spécification de chemins d'accès à des éléments externes (Générateur de rapports et SSRS)
  Vous pouvez spécifier des chemins d'accès dans les propriétés d'éléments de rapport pour faire référence à des éléments externes au fichier de définition de rapport et qui sont stockés sur un serveur de rapports. Il peut s'agir notamment de rapports d'extraction, de sous-rapports et de fichiers image.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
> [!NOTE]  
>  Dans le Générateur de rapports, les chemins d'accès aux éléments doivent indiquer des éléments stockés sur un serveur de rapports. Les chemins d'accès aux éléments situés sur un système de fichiers ne sont pas pris en charge. Vous ne pouvez afficher l'aperçu d'un rapport qui utilise ces éléments que si vous êtes connecté au serveur de rapports où résident les éléments.  
  
 Lorsque vous spécifiez un chemin d'accès à un élément externe dans une boîte de dialogue pour des actions, des sous-rapports ou des images, vous pouvez accéder directement au serveur de rapports et sélectionner l'élément. La méthode recommandée pour spécifier un rapport ou sous-rapport d'extraction consiste à accéder à un élément afin de le sélectionner directement. De cette façon, les noms de paramètres appropriés vous seront présentés dans une liste déroulante au moment de spécifier les paramètres de rapport ou de sous-rapport. Lorsque vous modifiez un chemin d'accès d'élément pour en désigner un autre, vous devez mettre à jour manuellement les noms de paramètres corrects et les valeurs, le cas échéant.  
  
 Sur un serveur de rapports configuré en mode natif, spécifiez un nom de rapport d'extraction sans l'extension de fichier .rdl.  
  
 Sur un serveur de rapports configuré en mode intégré SharePoint, vous devez inclure l'extension de fichier .rdl. Le chemin d'accès peut prendre l'une des formes suivantes :  
  
-   **Un chemin d'accès relatif à l'élément du rapport principal.** Par exemple, ../AllSubreports/Subreport1. Dans cet exemple, le symbole **..** désigne le dossier situé au-dessus du dossier où réside le rapport principal.  
  
    > [!NOTE]  
    >  Les chemins d'accès relatifs ne sont pas pris en charge lorsque le rapport est exécuté à l'intérieur du Générateur de rapports. Pour afficher un rapport qui utilise des chemins d'accès relatifs à des éléments externes, enregistrez le rapport sur le serveur de rapports, puis exécutez-le depuis ce dernier.  
  
-   **Un chemin d'accès complet à l'élément.**  
  
    -   **Sur un serveur de rapports :** le chemin d’accès commence par **/**, le dossier de base. Par exemple, /Reports/AllSubreports/Subreport1.  
  
    -   **Sur un site SharePoint :** vous devez spécifier le nom de rapport dans une expression en incluant l'URL complète de l'élément et l'extension de fichier .rdl. Par exemple, `="http://server/site/library/folder/Report1.rdl"`.  
  
## <a name="see-also"></a> Voir aussi  
 [Ajouter une image externe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-an-external-image-report-builder-and-ssrs.md)   
 [Ajouter un sous-rapport et des paramètres &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-subreport-and-parameters-report-builder-and-ssrs.md)   
 [Ajouter une action d’extraction à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-drillthrough-action-on-a-report-report-builder-and-ssrs.md)  
  
  
