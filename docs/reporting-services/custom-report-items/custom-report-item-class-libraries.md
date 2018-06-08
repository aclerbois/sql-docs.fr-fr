---
title: Bibliothèques de classes d’éléments de rapport personnalisés | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: custom-report-items
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- custom report items, RDL
- RDL [Reporting Services], custom report items
ms.assetid: f18c5d8f-1d6b-4f0b-8657-c14896c2ce0d
caps.latest.revision: 27
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 54ce65329f2be09e833e38be75bea1ddfcdd0149
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="custom-report-item-class-libraries"></a>Bibliothèques de classes d'éléments de rapport personnalisés
  Les éléments de rapport personnalisés utilisent des classes de l’espace de noms **Microsoft.ReportDesigner**. Les classes utilisées pour implémenter un élément de rapport personnalisé peuvent être divisées en deux catégories principales : les classes uniques conçues pour prendre en charge l'infrastructure d'éléments de rapport personnalisés et les classes wrapper managées qui encapsulent les fonctionnalités d'éléments RDL (Report Definition Language) pertinents. Pour un exemple de code montrant comment utiliser ces classes, consultez [SQL Server Reporting Services Product Samples](http://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="custom-report-item-infrastructure-classes"></a>Classes d'infrastructure d'éléments de rapport personnalisés  
 Les classes suivantes sont utilisées pour implémenter un élément de rapport personnalisé.  
  
> [!NOTE]  
>  Les tableaux suivants ne contiennent pas de listes exhaustives. Ils répertorient uniquement les propriétés les plus utilisées, ainsi que les méthodes de chaque classe.  
  
### <a name="microsoftreportdesignercustomreportitemdesigner"></a>Microsoft.ReportDesigner.CustomReportItemDesigner  
 Cette classe correspond à la principale classe d'élément de rapport personnalisé. La principale classe de votre implémentation d'élément de rapport personnalisé doit hériter de cette classe.  
  
#### <a name="public-properties"></a>Propri&#233;t&#233;s publiques  
  
|||  
|-|-|  
|**Nom**|Nom de l'élément de rapport personnalisé.|  
|**Type**|Type de l'élément de rapport personnalisé.|  
|**CustomData**|Objet <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData> qui encapsule les propriétés des données de l'élément de rapport personnalisé, spécifiées au moment de la conception.|  
|**CustomProperties**|Collection de propriétés personnalisées destinées à l'élément de rapport personnalisé.|  
|**Height**|Hauteur du contrôle de l'élément de rapport personnalisé.|  
|**Width**|Largeur du contrôle de l'élément de rapport personnalisé.|  
|**Rapport**|Conteneur pour les propriétés figurant au niveau du rapport, telles que la liste des datasets contenus dans ce rapport.|  
|**AltReportItem**|Objet de remplacement d'élément de rapport, à utiliser lorsque le contrôle DTC de l'élément de rapport personnalisé n'est pas pris en charge.|  
|**Style**|Propriétés de style destinées à l'élément de rapport personnalisé.|  
|**Adornment**|Fenêtre d'ornement utilisée pour modifier de manière interactive le contrôle.|  
|**Site**|**ISite** du composant.|  
|**DesignerVerbCollection**|Tableau de verbes personnalisés destinés au menu contextuel du contrôle.|  
  
#### <a name="public-methods"></a>M&#233;thodes publiques  
  
|||  
|-|-|  
|**BeginEdit**|Active la modification interactive du contrôle.|  
|**DoDefaultAction**|Appelé en cas de double-clic ou lorsque le bouton de retour du contrôle est enfoncé.|  
|**EndEdit**|Désactive la modification interactive du contrôle.|  
|**GetService**|Retourne un objet qui représente un service.|  
|**InitializeNewComponent**|Appelé lorsqu'un nouvel élément de rapport personnalisé est créé.|  
|**Invalidate**|Repeint l'intégralité de la surface du contrôle.|  
|**OnDragEnter**<br /><br /> **OnDragDrop**|Appelé lorsqu'un objet est déplacé jusqu'au contrôle.|  
|**OnPaint**|Appelé en réponse à l’événement **Paint**.|  
  
### <a name="microsoftreportdesignercustomreportitemattribute"></a>Microsoft.ReportDesigner.CustomReportItemAttribute  
 Cet attribut est utilisé afin d'identifier le type de l'élément de rapport personnalisé. Le nom doit correspondre à la valeur de l’attribut \<**Name**> de l’élément **ReportItem** figurant dans le fichier de configuration du Concepteur de rapports.  
  
#### <a name="public-methods"></a>M&#233;thodes publiques  
  
|||  
|-|-|  
|**CustomReportItemAttribute**|Construit l'objet CustomReportItemAttribute.|  
  
### <a name="microsoftreportdesignerlocalizednameattribute"></a>Microsoft.ReportDesigner.LocalizedNameAttribute  
 Cet attribut est utilisé pour spécifier le nom d'affichage à utiliser pour le concepteur d'éléments de rapport personnalisés.  
  
#### <a name="public-methods"></a>M&#233;thodes publiques  
  
|||  
|-|-|  
|**LocalizedNameAttribute**|Construit l'objet LocalizedNameAttribute.|  
  
### <a name="microsoftreportdesigneradornment"></a>Microsoft.ReportDesigner.Adornment  
 La classe **Adornment** est utilisée par le composant de conception de l’élément de rapport personnalisé pour définir des zones en dehors du rectangle principal de l’aire de conception. Ces zones permettent de gérer les événements de l'interface utilisateur, tels que les clics de souris et les opérations de glisser-déplacer.  
  
#### <a name="public-methods"></a>M&#233;thodes publiques  
  
|||  
|-|-|  
|**OnShow**|Appelé quand **Adornment** est activé.|  
|**OnHide**|Appelé quand **Adornment** est désactivé.|  
|**Paint**|Appelé en réponse à l’événement **Paint**.|  
|**OnDragEnter**<br /><br /> **OnDragOver**<br /><br /> **OnDragLeave**<br /><br /> **OnDragDrop**|Appelé quand un objet est déplacé jusqu’à **Adornment**.|  
  
### <a name="microsoftreportdesigneradornerservice"></a>Microsoft.ReportDesigner.AdornerService  
 Cette classe est utilisée pour fournir la collection de services d’affichage utilisée par l’élément de rapport personnalisé, et ce afin de prendre en charge les objets **Adornment** destinés à son composant de conception.  
  
#### <a name="public-properties"></a>Propri&#233;t&#233;s publiques  
  
|||  
|-|-|  
|**AdornerWindowBounds**|Limites de la fenêtre de dispositif de décoration.|  
|**AdornerWindowRegion**|Région de la fenêtre de dispositif de décoration.|  
|**AdornerWindowGraphics**|Contexte graphique de la fenêtre de dispositif de décoration.|  
  
#### <a name="public-methods"></a>M&#233;thodes publiques  
  
|||  
|-|-|  
|**ComponentRectInDesignerFrame**|Retourne les limites du composant sous la forme de coordonnées de cadre compréhensibles par le concepteur.|  
|**InvalidateAdorner**|Invalide la fenêtre de dispositif de décoration.|  
|**PointToAdorner**|Retourne un point exprimé en coordonnées d'écran sous la forme de coordonnées compréhensibles par la fenêtre de dispositif de décoration.|  
  
### <a name="microsoftreportdesignerexpressioneditor"></a>Microsoft.ReportDesigner.ExpressionEditor  
 Cette classe peut être utilisée à partir du contrôle DTC de l'élément de rapport personnalisé afin d'appeler l'Éditeur d'expressions.  
  
#### <a name="public-methods"></a>M&#233;thodes publiques  
  
|||  
|-|-|  
|**EditValue**|Appelle l'Éditeur d'expressions, alors initialisé avec une valeur d'objet donnée.|  
  
### <a name="microsoftreportdesignerifieldsdataobject"></a>Microsoft.ReportDesigner.IFieldsDataObject  
 Cette classe est une collection de champs [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et est utilisée pour prendre en charge les événements glissé-déplacé dans un environnement de conception. Hérite de **IReportItemDataObject**.  
  
#### <a name="public-properties"></a>Propri&#233;t&#233;s publiques  
  
|||  
|-|-|  
|**DataSetName**|Nom du dataset qui contient les champs à déplacer.|  
|**Fields**|Collection de champs (**Microsoft.ReportDesigner.Field**) à déplacer.|  
  
## <a name="see-also"></a> Voir aussi  
 [Langage de définition de rapport &#40;SSRS, Report Definition Language&#41;](../../reporting-services/reports/report-definition-language-ssrs.md)   
 [Création d’un composant d’exécution d’éléments de rapport personnalisé](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)   
 [Création d'un composant au moment de la conception d'éléments de rapport personnalisé](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)  
  
  
