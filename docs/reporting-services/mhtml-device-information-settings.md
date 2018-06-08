---
title: Paramètres d’informations de périphérique pour le format de rendu MHTML | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- device information settings [Reporting Services], MHTML rendering
- MHTML [Reporting Services]
ms.assetid: 60b85dd8-b4fb-4ad9-be6a-e7c89ac076fe
caps.latest.revision: 39
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 740bf5e3cfab88d8dfcdac986fd4a4d33c641795
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mhtml-device-information-settings"></a>Paramètres d'informations de périphérique pour le format de rendu MHTML
  Le tableau suivant répertorie les paramètres des informations de périphérique qui permettent de restituer les rapports au format Archive Web MHTML.  
  
|Paramètre|Valeur|  
|-------------|-----------|  
|**JavaScript**|Indique si JavaScript est pris en charge dans le rapport rendu.|  
|**OutlookCompat**|Indique s'il convient d'effectuer le rendu avec des métadonnées supplémentaires qui font que l'aspect du rapport est meilleur dans Outlook. La valeur par défaut est **true**.|  
|**Fragment MHTML**|Indique si un fragment MHTML est créé à la place d'un document MHTML complet. Les fragments MHTML font figurer le contenu du rapport dans un élément TABLE et omettent les éléments HTML et BODY. La valeur par défaut est **false**.|  
|**DataVisualizationFitSizing**|Indique le comportement d'ajustement de la visualisation des données à l'intérieur d'un tableau matriciel. Cela inclut le graphique, la jauge et la carte.<br /><br /> Les valeurs possibles sont **Approximatif** et **Exact**.<br /><br /> La valeur par défaut est **Approximatif**. Si le paramètre est supprimé du fichier **rsreportserver.config** , le comportement par défaut est **Exact**.<br /><br /> L’activation de l’option **Exact** peut avoir un impact sur les performances, car le traitement permettant de déterminer la taille peut prendre plus de temps.|  
  
## <a name="see-also"></a> Voir aussi  
 <xref:ReportExecution2005.ReportExecutionService.Render%2A>   
 [Transmission de paramètres d'informations de périphérique aux extensions de rendu](../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md)   
 [Personnaliser les paramètres d’extension de rendu dans RSReportServer.Config](../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../reporting-services/technical-reference-ssrs.md)  
  
  
