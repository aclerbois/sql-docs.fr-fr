---
title: Paramètres de site SharePoint pour le composant WebPart de la visionneuse de rapports - SSRS | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: jt000
ms.author: jasontre
ms.openlocfilehash: 57e0e7ca8aa76570f70ae2279964d47f50da31aa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sharepoint-site-settings-for-the-report-viewer-web-part---reporting-services"></a>Paramètres de site SharePoint pour le composant WebPart de la visionneuse de rapports - Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)])

Le composant WebPart de la visionneuse de rapports comporte deux paramètres qui peuvent être configurés. Ces paramètres peuvent être activés et désactivés dans la page Paramètres du site SharePoint, par un administrateur de site. Notez que chaque site a ses propres paramètres. Par ailleurs, ces paramètres ne sont pas réinitialisés après la réinstallation du composant WebPart de la visionneuse de rapports.

## <a name="accessing-the-site-settings-page"></a>Accès à la page Paramètres du site

Pour accéder aux paramètres du site :

1. Dans votre site SharePoint, cliquez sur l’icône **engrenage** en haut à gauche et sélectionnez **Paramètres du site**.

    ![Paramètres du site à partir de l’icône d’engrenage.](media/sharepoint-site-settings.png)

2. En cliquant sur **Paramètres du composant WebPart de la visionneuse de rapports** dans le groupe de paramètres de site **Reporting Services**.

    > [!NOTE]
    > L’accès aux paramètres de site est également possible par un accès direct à `<site>/_layouts/15/ReportViewerWebPart/ReportViewerWebPartSettings.aspx`.

## <a name="report-viewer-web-part-settings"></a>Paramètres du composant WebPart de la visionneuse de rapports

|Paramètre|Commentaires|  
|-------------|--------------|  
|Collecter les données d’utilisation|Permet d’envoyer à Microsoft des informations relatives aux erreurs et à l’utilisation en vue d’améliorer les produits. Pour plus d’informations sur la stratégie de collecte de données pour le signalement d’erreurs Microsoft, consultez [Microsoft SQL Server Privacy Statement](https://go.microsoft.com/fwlink/?linkid=860782&clcid=0x409).|  
|Activer les métadonnées d’accessibilité pour les rapports|Définit les [informations de périphérique `AccessibleTablix`](../html-device-information-settings.md) pour les rapports rendus.| 
