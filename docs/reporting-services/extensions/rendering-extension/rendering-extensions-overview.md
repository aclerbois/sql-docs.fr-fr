---
title: Vue d’ensemble des extensions de rendu | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: extensions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- formats [Reporting Services], rendering extensions
- rendering extensions [Reporting Services], about extensions
ms.assetid: 909356a0-4709-43e5-b597-33bd9bb22882
caps.latest.revision: 41
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: e8ba9ec6f9e6e629a7f6019c362824363e546583
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rendering-extensions-overview"></a>Vue d'ensemble des extensions de rendu
  Une extension de rendu est un composant ou un module d'un serveur de rapports qui transforme les données de rapport et les informations de disposition dans un format spécifique au périphérique. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] comprend sept extensions de rendu : HTML, Excel, Word, CSV ou texte, XML, image et PDF. Vous pouvez créer des extensions de rendu supplémentaires pour créer des rapports dans d'autres formats.  
  
> [!NOTE]  
>  Pour déterminer quelles extensions de rendu sont disponibles, vous pouvez consulter la liste des extensions installées dans le fichier RSReportServer.config.  
  
 Le tableau suivant décrit les extensions de rendu incluses avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Extension Name|Description|  
|--------------------|-----------------|  
|**XML**|Effectue le rendu d'un rapport au format XML. Le rapport peut être ouvert dans un navigateur. Les transformations supplémentaires qui s'appliquent à cette sortie XML offrent éventuellement une méthode économique pour éviter d'avoir à développer votre propre extension de rendu.|  
|**CSV**|Effectue le rendu d'un rapport dans lequel les données sont délimitées par des virgules. Le rapport s'ouvre dans un outil d'affichage associé aux formats de fichiers CSV.|  
|**IMAGE**|Effectue le rendu d'un rapport au format page. Le format est affiché comme **TIFF** dans le menu déroulant Exporter de la barre d’outils Rapport.|  
|**PDF**|Effectue le rendu d'un rapport dans Adobe Acrobat Reader. Le format est affiché comme **Fichier Acrobat (PDF)** dans le menu déroulant Exporter de la barre d’outils Rapport.|  
|**EXCEL**|Effectue le rendu d'un rapport dans [!INCLUDE[ofprexcel](../../../includes/ofprexcel-md.md)].|  
|**WORD**|Effectue le rendu d’un rapport dans [!INCLUDE[ofprword](../../../includes/ofprword-md.md)].|  
|**HTML 4.0** (partie de l’extension de rendu HTML)|HTML est le format de restitution utilisé initialement pour le rapport. Si votre navigateur prend en charge le HTML 4.0, il s'agit du format utilisé. Autrement, il s'agit du HTML 3.2.|  
|**MHTML** (partie de l’extension de rendu HTML)|Effectue le rendu d'un rapport au format MHTML. Le rapport peut être ouvert dans Internet Explorer. Le format est affiché comme **Archive web** dans le menu déroulant Exporter de la barre d’outils Rapport.|  
|**NULL**|N'effectue pas le rendu d'un rapport dans un format spécifique. Cette extension de rendu est utile pour placer des rapports dans le cache. Le rendu Null doit être utilisé conjointement avec une exécution ou remise planifiée.|  
  
 Pour plus d’informations sur les formats recommandés et leurs utilisations, consultez [Exporter les rapports &#40;Générateur de rapports et SSRS&#41;](../../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
 Chacune des extensions de rendu implémentée par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] et fournie avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] fait appel à un ensemble commun d'interfaces. Cela garantit que chaque extension implémente des fonctionnalités comparables et simplifie le code de rendu au cœur du serveur de rapports.  
  
## <a name="rendering-object-model"></a>Modèle objet de rendu  
 Lorsqu'un rapport est traité, le résultat est un modèle objet publiquement exposé connu sous le nom de mdèle objet de rendu (ROM, Rendering Object Model). Le modèle objet de rendu est une collection de classes qui définissent le contenu, la disposition et les données d'un rapport ayant été traité. Le modèle est disponible pour les développeurs qui souhaitent concevoir, développer et déployer des extensions de rendu personnalisées pour [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Le modèle est produit lorsque le serveur de rapports traite la définition XML d'un rapport avec les données de rapport définies par l'utilisateur. Au terme du traitement, le modèle objet public est utilisé par une extension de rendu pour définir la sortie du rapport. Les classes publiques disponibles du modèle sont définies dans l’espace de noms **Microsoft.ReportingServices.OnDemandReportRendering**.  
  
## <a name="writing-custom-rendering-extensions"></a>Écriture d'extensions de rendu personnalisées  
 Avant de décider de créer une extension de rendu personnalisée, vous devez réfléchir à des alternatives plus simples. Vous pouvez :  
  
-   Personnaliser la sortie rendue en spécifiant des paramètres d'informations de périphérique pour les extensions existantes.  
  
-   Ajouter des fonctionnalités de présentation et de mise en forme personnalisées en associant des Transformations XSL (XSLT) à la sortie du format de rendu XML.  
  
 L'écriture d'une extension de rendu personnalisée est difficile. Une extension de rendu doit généralement prendre en charge toutes les combinaisons possibles d'éléments de rapport et nécessite d'implémenter des centaines de classes, interfaces, méthodes et propriétés. Si vous devez effectuer le rendu d’un rapport dans un format qui n’est pas inclus avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] et que vous décidez d’écrire votre propre implémentation de code managé d’une extension de rendu, le code de l’extension de rendu doit implémenter l’interface **Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension** que nécessite le serveur de rapports.  
  
 Pour une documentation supplémentaire et des livres blancs sur [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], consultez les ressources techniques les plus récentes sur le [site web Reporting Services](http://go.microsoft.com/fwlink/?LinkId=19951).  
  
## <a name="see-also"></a> Voir aussi  
 [Implémentation d’une extension de rendu](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
