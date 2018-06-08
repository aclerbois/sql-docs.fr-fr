---
title: Paramètres et fonctions du site Reporting Services (mode SharePoint) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d376ca984ee2666c8f84a46d3a7895c911d0c719
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-site-settings-and-site-features-sharepoint-mode"></a>Paramètres et fonctions du site Reporting Services (mode SharePoint)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Le mode SharePoint de Reporting Services propose plusieurs fonctionnalités personnalisées au niveau du site et une fonctionnalité de site qui peuvent être gérées à partir de la page des paramètres du site SharePoint. Les paramètres sont à l’échelle du site et affectent toutes les applications de service Reporting Services. Vous devez disposer des autorisations de gestionnaire de contenu et d'administrateur système pour consulter cette page.  

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

|Paramètre du site|Description|  
|------------------|-----------------|  
|Paramètres du site Reporting Services|Paramètres à l’échelle du site décrits dans cette rubrique.|  
|Gérer les alertes de données|Gestion de la fonctionnalité d'alertes de données.|  
|Synchronisation des fichiers du serveur de rapports|Fonctionnalité au niveau du site désactivée par défaut.<br /><br /> Synchronise les fichiers du serveur de rapports (.rdl, .rsds, .smdl, .rsd, .rsc, .rdlx) à partir d'une bibliothèque de documents SharePoint sur le serveur de rapports lorsque des fichiers sont ajoutés ou mis à jour directement dans la bibliothèque de documents.<br /><br /> Pour plus d’informations, consultez [Activer la fonctionnalité Synchronisation de fichiers de serveur de rapports dans l’Administration centrale de SharePoint](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md)|  
  
## <a name="open-the-reporting-services-site-settings-page"></a>Ouvrir la page des paramètres du site Reporting Services
  
1.  Dans le menu **Actions de site** du site SharePoint, sélectionnez **Paramètres du site**.  
  
2.  Dans la section **Reporting Services**, sélectionnez **Paramètres du site Reporting Services**.  
  
## <a name="options-for-reporting-services-site-settings"></a>Options des paramètres du site Reporting Services
  
|Option|Description|  
|------------|-----------------|  
|**Activer le téléchargement du contrôle ActiveX RSClientPrint**|Le contrôle affiche une boîte de dialogue d'impression personnalisée qui prend en charge les fonctionnalités communes aux autres boîtes de dialogue d'impression, notamment l'aperçu avant impression, les options de pages pour les choix spécifiques liés aux pages, aux plages, aux marges des pages et à l'orientation. Pour plus d’informations sur le contrôle, consultez [Utilisation du contrôle RSClientPrint dans les applications personnalisées](../../reporting-services/report-server-web-service/net-framework/using-the-rsclientprint-control-in-custom-applications.md)|  
|**Activer les erreurs distantes en mode local**|Affiche ou masque des messages d'erreur détaillés sur des ordinateurs distants en mode local. Si vous voyez s'afficher un message d'erreur semblable au suivant, l'activation des erreurs distantes peut être utile :<br /><br /> `For more information about this error navigate to the report server on the local server machine or enable remote errors`|  
|**Activer les métadonnées d'accessibilité pour les rapports**|Activer les métadonnées d'accessibilité dans la sortie HTML des rapports|  
|**Activer le dimensionnement exact d'ajustement de la visualisation des données pour les rapports**|Configurez le comportement d'ajustement de la visualisation des données à l'intérieur d'un tableau matriciel, pour l'ajuster exactement. Cela inclut le graphique, la jauge et la carte. Si désactivé, le comportement consiste à ajuster approximativement les visualisations des données, ce qui peut laisser des vides. Ce paramètre s’applique uniquement au rendu dans le composant WebPart Visionneuse de rapports. Pour gérer ce comportement pour le rendu côté serveur, vous devez modifier le fichier **rsreportserver.config**. Pour plus d'informations, consultez les documents suivants :<br /><br /> [Fichier de configuration RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).<br /><br /> [Personnaliser les paramètres d’extension de rendu dans RSReportServer.Config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md).<br /><br /> [Paramètres d’informations de périphérique HTML](../../reporting-services/html-device-information-settings.md).<br /><br /> L'activation exacte peut avoir un impact sur les performances car le traitement permettant de déterminer la taille peut prendre plus de temps qu'un ajustement approximatif.|  
  
## <a name="see-also"></a>Voir aussi

 [Gérer une application de service SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  
  
  
