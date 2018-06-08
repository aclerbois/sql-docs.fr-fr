---
title: Paramètres d’informations de périphérique HTML | Microsoft Docs
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.custom: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- HTML [Reporting Services], rendering
- device information settings [Reporting Services], HTML rendering
ms.assetid: f505f478-dd6d-444a-957c-34f7cfb98911
caps.latest.revision: 49
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 730d9d0e1c000eea0b066805fdd1b3eaaa22f1dd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="html-device-information-settings"></a>Paramètres d'informations de périphérique HTML
Le tableau suivant répertorie les paramètres d'informations de périphérique qui permettent un rendu du rapport au format HTML.  
  
> [!IMPORTANT]  
>  Les paramètres d’informations de périphérique répertoriés dans le tableau ci-dessous avec **(\*)** sont déconseillés et ne doivent pas être utilisés dans les nouvelles applications. Pour plus d’informations, consultez [Fonctions déconseillées de SQL Server Reporting Services dans SQL Server 2016](../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md)   
  
|Paramètre|Valeur|  
|-------------|-----------|  
|**AccessibleTablix**|Indique s'il faut effectuer le rendu avec des métadonnées d'accessibilité supplémentaires en vue de l'utilisation d'un lecteur d'écran. Les métadonnées d'accessibilité supplémentaires imposent la mise en conformité du rapport eu égard des normes techniques suivantes décrites dans la section « Intranet Web et Applications et Informations Internet » (1194.22) du document relatif aux normes d'accessibilité dans le domaine de l'électronique et des technologies de l'information (Section 508) :<br /><br /> (g) les en-têtes de ligne et de colonne doivent être identifiés pour les tables de données.<br /><br /> (h) le balisage doit être utilisé pour associer les cellules de données aux cellules d'en-tête pour les tables de données qui ont plusieurs niveaux logiques d'en-têtes de ligne ou de colonne.|  
|**ActionScript(\*)**|Spécifie le nom de la fonction JavaScript à utiliser lorsqu'un événement d'action se produit, tel qu'une extraction ou un clic pour atteindre un signet. Si ce paramètre est spécifié, un événement d'action déclenchera la fonction JavaScript nommée au lieu d'une publication sur le serveur.|  
|**BookmarkID**|Identificateur du signet à atteindre dans le rapport.|  
|**DocMap**|Indique s'il faut afficher ou masquer l'Explorateur de documents du rapport. La valeur par défaut de ce paramètre est **true**.|  
|**ExpandContent**|Indique si le rapport doit être placé dans une structure de table qui restreint la taille horizontale.|  
|**FindString**|Texte à rechercher dans le rapport. La valeur par défaut de cette propriété est une chaîne vide.|  
|**GetImage (\*)**|Obtient une icône particulière pour l'interface utilisateur de la visionneuse HTML.|  
|**HTMLFragment**|Indique si un fragment HTML est créé à la place d'un document HTML complet. Les fragments HTML font figurer le contenu du rapport dans un élément TABLE et omettent les éléments HTML et BODY. La valeur par défaut est **false**. Si vous effectuez un rendu en HTML à l’aide de la méthode **M:ReportExecution2005.ReportExecutionService.Render(System.String,System.String,System.String@,System.String@,System.String@, ReportExecution2005.Warning[]@,System.String[]@)** de l’API SOAP et que le rapport contient des images, vous devez définir cette information de périphérique sur **true** . Le rendu à l'aide de SOAP avec la propriété **HTMLFragment** définie sur **true** crée des URL contenant des informations de session qui peuvent être utilisées pour demander correctement les images. Les images doivent être des ressources téléchargées dans la base de données du serveur de rapports.|  
|**ImageConsolidation**|Indique s'il faut consolider le rendu du graphique, du plan, de la jauge ou des images d'indicateurs en une grande image. La consolidation d'aides visuelles améliore la performance du rapport dans le navigateur client lorsque le rapport contient un grand nombre d'éléments de visualisation des données. La valeur par défaut est **true** pour les navigateurs les plus récents.|  
|**JavaScript**|Indique si JavaScript est pris en charge dans le rapport rendu. La valeur par défaut est **true**.|  
|**LinkTarget**|Cible des liens hypertexte contenus dans le rapport. Vous pouvez cibler une fenêtre ou un cadre en fournissant son nom, tel que **LinkTarget**=*window_name*ou vous pouvez cibler une nouvelle fenêtre en utilisant **LinkTarget**=_blank. D'autres noms de cibles valides incluent _self, _parent et _top.|  
|**OnlyVisibleStyles(\*)**|Indique que seuls les styles partagés pour la page actuellement rendue sont générés.|  
|**OutlookCompat**|Indique s'il convient d'effectuer le rendu avec des métadonnées supplémentaires qui font que l'aspect du rapport est meilleur dans Outlook. Pour d'autres, la valeur par défaut est **false**.|  
|**Paramètres**|Indique s'il faut afficher ou masquer la zone des paramètres de la barre d'outils. Si vous affectez à ce paramètre la valeur **true**, la zone des paramètres de la barre d'outils s'affiche. La valeur par défaut de ce paramètre est **true**.|  
|**PrefixId**|Lorsque ce paramètre est utilisé avec **HTMLFragment**, ajoute le préfixe spécifié à tous les attributs **ID** dans le fragment HTML qui est créé.|  
|**ReplacementRoot(\*)**|Chaîne à ajouter à tous les liens d'extraction, de bascule et de signet dans le rapport lors du rendu hors du contrôle ReportViewer. Par exemple, ce paramètre est utilisé pour rediriger le clic d'un utilisateur vers une page personnalisée.|  
|**ResourceStreamRoot(\*)**|Chaîne à ajouter à l'URL pour toutes les ressources d'image, telles que les images bascule ou de tri.|  
|**Section**|Numéro de page du rapport dont le rendu est effectué. La valeur **0** indique que toutes les sections du rapport sont rendues. La valeur par défaut est **1**.|  
|**StreamRoot (\*)**|Chemin d'accès utilisé pour préfixer la valeur de l'attribut **src** de l'élément IMG dans le rapport HTML retourné par le serveur de rapports. Par défaut, le serveur de rapports fournit le chemin d'accès. Vous pouvez utiliser ce paramètre pour spécifier un chemin racine pour les images contenues dans un rapport (par exemple, **http://\<nom_serveur>/resources/companyimages**).|  
|**StyleStream**|Indique si les styles et les scripts sont créés en tant que flux distinct plutôt que dans le document. La valeur par défaut est **false**.|  
|**Barre d'outils**|Indique s'il faut afficher ou masquer la barre d'outils. La valeur par défaut de ce paramètre est **true**. Si la valeur de ce paramètre est **false**, toutes les options restantes (sauf le plan du document) sont ignorées. Si vous omettez ce paramètre, la barre d'outils s'affiche automatiquement pour les formats de rendu assurant sa prise en charge.<br /><br /> Le rendu de la barre d'outils de la Visionneuse de rapports est effectué lorsque vous utilisez l'accès URL pour effectuer le rendu d'un rapport. Le rendu de la barre d'outils ne s'effectue pas via l'API SOAP. Toutefois, le paramètre d'informations de périphérique **Toolbar** affecte la façon dont le rapport s'affiche lors de l'utilisation de la méthode SOAP **Render** . Si la valeur de ce paramètre est **true** lorsque vous utilisez SOAP pour effectuer un rendu au format HTML, seule la première section du rapport est rendue. Si la valeur est **false**, le rendu du rapport HTML entier est effectué sous la forme d'une page HTML unique.|  
|**UserAgent**|Chaîne **user-agent** du navigateur qui effectue la demande, qui figure dans la requête HTTP.|  
|**Zoom (\*)**|Valeur du zoom du rapport sous forme de pourcentage entier ou de constante de chaîne. Les valeurs de chaîne standard incluent les valeurs **Page Width** et **Whole Page**. Ce paramètre est ignoré par les versions de [!INCLUDE[msCoName](../includes/msconame-md.md)] Internet Explorer antérieures à la version 5.0 et par tous les navigateurs non-[!INCLUDE[msCoName](../includes/msconame-md.md)] . La valeur par défaut de ce paramètre est **100**.|  
|**DataVisualizationFitSizing**|Indique le comportement d'ajustement de la visualisation des données à l'intérieur d'un tableau matriciel. Cela inclut le graphique, la jauge et la carte.<br /><br /> Les valeurs possibles sont **Approximatif** et **Exact**.<br /><br /> La valeur par défaut est **Approximatif**. Si le paramètre est supprimé du fichier **rsreportserver.config** , le comportement par défaut est **Exact**.<br /><br /> L’activation de l’option **Exact** peut avoir un impact sur les performances, car le traitement permettant de déterminer la taille peut prendre plus de temps.|  
  
## <a name="see-also"></a> Voir aussi  
 [Transmission de paramètres d'informations de périphérique aux extensions de rendu](../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md)   
 [Personnaliser les paramètres d’extension de rendu dans RSReportServer.Config](../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../reporting-services/technical-reference-ssrs.md)  
  
  
