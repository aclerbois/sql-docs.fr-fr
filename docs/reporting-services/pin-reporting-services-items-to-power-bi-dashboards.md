---
title: Épingler des éléments Reporting Services aux tableaux de bord Power BI | Microsoft Docs
ms.custom: ''
ms.date: 09/16/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- pbi
- dashboard
- pin
- powerbi
- power bi integration
ms.assetid: 1d96c3f7-2fd4-40f7-8d1c-14a7f54cdb15
caps.latest.revision: 23
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 7e3e738ef82486f80b9f81ae8e1d1218397980d1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pin-reporting-services-items-to-power-bi-dashboards"></a>Épingler des éléments Reporting Services aux tableaux de bord Power BI
  [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] permet aux utilisateurs d’épingler des éléments de rapport [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] à partir de la barre d’outils de la visionneuse de rapports à un tableau de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] en tant que nouvelle vignette.   Pour que vous puissiez épingler des éléments, il faut que votre administrateur intègre au préalable votre serveur de rapports à Azure Active Directory et [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)].  
  
 ![rs_powerbi_icon](../reporting-services/media/ssrs-powerbi-icon.png "rs_powerbi_icon")  
  
 [!INCLUDE[applies](../includes/applies-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] Mode natif
  
##  <a name="bkmk_requirements_to_pin"></a> Conditions requises pour pouvoir épingler  
  
-   Le serveur de rapports est configuré pour l’intégration de [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] . Pour plus d’informations, consultez [Intégration du serveur de rapports Power BI &#40;Gestionnaire de configuration&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md). Si le serveur de rapports n’a pas été configuré, vous ne voyez pas le bouton **Épingler au tableau de bord Power BI** dans la barre d’outils.  
  
     ![ssRS_Report_PowerBI](../reporting-services/media/ssrs-report-powerbi.png)  
  
-   Vous épinglez les éléments à partir de la visionneuse de rapports [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] dans le [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] ; par exemple, `http://myserver/Reports`.  Vous ne pouvez pas épingler d’éléments à partir de [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion-md.md)], du Concepteur de rapports dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]ou d’une URL de serveur de rapports.  Par exemple, `http://myserver/ReportServer`.  
  
-   Votre navigateur doit être configuré pour autoriser l’affichage des fenêtres contextuelles en provenance du site de votre serveur de rapports.  
  
-   Si vous voulez que l’élément épinglé s’actualise, les rapports doivent être configurés pour les informations d’identification stockées.  Quand vous épinglez un élément, un abonnement [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] est automatiquement créé pour gérer l’actualisation des données de l’élément sur le tableau de bord.  Si le rapport n’utilise pas d’informations d’identification stockées, une fois l’abonnement actif, la page **Mes abonnements** affiche un message d’erreur similaire au suivant.  
  
        PowerBI Delivery error: dashboard: IT Spend Analysis Sample, visual: Chart2, error: The current action cannot be completed. The user data source credentials do not meet the requirements to run this report or shared dataset. Either the user data source credential.
 
    Consultez la section « Configurer des informations d’identification stockées pour une source de données propre à un rapport (mode natif) » dans [Stocker les informations d’identification dans une source de données Reporting Services](../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md).  
  
##  <a name="bkmk_supported_items"></a> Éléments que vous pouvez épingler  
 Vous pouvez épingler les éléments de rapport suivants à un tableau de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] .  Vous ne pouvez pas épingler des éléments imbriqués à l’intérieur d’une région de données. Par exemple, vous ne pouvez pas épingler un élément imbriqué dans une table ou une liste [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .  
  
-   Graphiques  
  
-   Panneaux de jauge  
  
-   Cartes  
  
-   Images  
  
-   Les éléments doivent se trouver dans le corps du rapport.  Vous ne pouvez pas épingler des éléments figurant dans l’en-tête ou le pied de page.  
  
-   Vous pouvez épingler des éléments figurant à l’intérieur d’un rectangle de niveau supérieur, mais vous ne pouvez les pas épingler tous comme un seul groupe.  
  
##  <a name="bkmk_to_pin"></a> Pour épingler un élément de rapport  
  
1. Vérifiez que vous êtes connecté à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)]. Dans le [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], sélectionnez l’élément de menu **Mes paramètres** et connectez-vous. Pour plus d’informations, consultez  [Mes paramètres pour l’intégration de Power BI &#40;portail web&#41;](http://msdn.microsoft.com/en-us/85c2fac7-80bf-45b7-8654-764b5f5231f5) .

    ![ssRS_WebPortal_MySettings](../reporting-services/media/ssrs-webportal-mysettings.png)  
  
2. Accédez au dossier du [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] contenant votre rapport, puis affichez le rapport.  
  
3. Une fois le rapport affiché, cliquez sur le bouton **Épingler à Power BI** dans la barre d’outils.  Si vous n’êtes pas connecté, vous êtes invité à le faire.  Si le bouton [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] n’est pas visible, cela signifie que le serveur de rapports n’a pas été intégré à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)]. Pour plus d’informations, consultez [Intégration du serveur de rapports Power BI &#40;Gestionnaire de configuration&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md).  
  
    ![ssRS_Report_PowerBI](../reporting-services/media/ssrs-report-powerbi.png)  
  
4. Sélectionnez l’élément de rapport que vous voulez épingler à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)]. Vous ne pouvez épingler qu’un seul élément à la fois.  La visionneuse de rapports présente un affichage ombré de votre rapport. Les éléments que vous pouvez épingler apparaissent en surbrillance, tandis que ceux que vous ne pouvez pas épingler, sont ombrés.  
  
    **(1)** Sélectionnez le groupe qui contient le tableau de bord sur lequel vous souhaitez épingler l’élément, **(2)** sélectionnez le tableau de bord sur lequel vous souhaitez épingler l’élément et **(3)** sélectionnez la fréquence à laquelle vous voulez que la vignette soit mise à jour sur le tableau de bord.   ![remarque](../analysis-services/instances/install-windows/media/ssrs-fyi-note.png "remarque") L’actualisation est gérée par les abonnements [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] et, une fois l’élément épinglé, vous pouvez modifier l’abonnement et configurer une autre planification de l’actualisation.  
  
    ![ssRS_Pin_to_PowerBI](../reporting-services/media/ssrs-pin-to-powerbi.png)  
  
5. Sélectionnez **Épingler**.  
  
    Dans la boîte de dialogue **Épinglé avec succès** , vous pouvez sélectionnez le lien **Retrouvez-le ici dans Power BI** pour accéder au tableau de bord et voir l’élément que vous venez d’épingler.  
  
6. Sélectionnez **Fermer** pour revenir à l’affichage normal du rapport.  
  
##  <a name="bkmk_in_the_dashboard"></a> Dans le tableau de bord

Une fois votre élément de rapport épinglé sur le tableau de bord, la vignette ressemble aux autres, rien n’indiquant qu’elle provient de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. La liste suivante récapitule la manière dont les propriétés de vignette sont définies à partir de l’élément de rapport.  
  
Dans le tableau de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] , l’élément de rapport épinglé se comporte comme d’autres vignettes :

**(1)** Vous pouvez épingler la vignette à d’autres tableaux de bord.

**(2)** Dans les **Détails de la vignette** vous pouvez constater que le titre du rapport [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] est utilisé comme titre par défaut de la vignette.

**(3)** Le sous-titre de la vignette est basé sur les date et heure auxquelles la vignette a été épinglée ou auxquelles les données ont été actualisées pour la dernière fois à partir de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. La planification de l’actualisation est gérée par l’abonnement [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] créé automatiquement lors de l’épinglage de l’élément de rapport.

**(4)** Si vous sélectionnez la vignette, [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] utilise le **(3) lien personnalisé** pour accéder à la page du [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] du serveur de rapports inscrit. Le lien a été défini lors de l’épinglage de l’élément à partir de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Si vous n’avez pas de connectivité Internet au serveur de rapports, une erreur s’affiche dans le navigateur.  

![ssrs_pinned_tile_details](../reporting-services/media/ssrs-pinned-tile-details.png "ssrs_pinned_tile_details")  
  
##  <a name="bkmk-troubleshoot"></a> Résoudre les problèmes  
  
-   **Aucun bouton [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] dans la barre d’outils de la visionneuse de rapports :** cela indique que le serveur de rapports n’a pas été intégré à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)]. Pour plus d’informations, consultez [Intégration du serveur de rapports Power BI &#40;Gestionnaire de configuration&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md).  
  
- **Impossible d’épingler**: quand vous tentez d’épingler un élément, le message d’erreur suivant s’affiche. Consultez la section [Éléments que vous pouvez épingler](#bkmk_supported_items).  
  
      Cannot Pin: There are no report items on this page that you can pin to [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)].  
  
-   **Les éléments épinglés affichent des données caduques** dans un tableau de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] n’ont pas été mises à jour depuis un certain temps.  Le jeton d’informations d’identification de l’utilisateur a expiré et vous devez vous reconnecter.  L’inscription des informations d’identification de l’utilisateur auprès d’Azure et de [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] est valable pendant 90 jours. Dans le[!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], cliquez sur **Mes paramètres**. Pour plus d’informations, consultez [Mes paramètres pour l’intégration de Power BI &#40;portail web&#41;](http://msdn.microsoft.com/en-us/85c2fac7-80bf-45b7-8654-764b5f5231f5).  
  
-   **Les éléments épinglés affichent des données caduques** dans un tableau de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] et elle n’ont jamais été actualisées.  Le problème est que le rapport n’est pas configuré pour utiliser des informations d’identification stockées. Un rapport doit utiliser des informations d’identification, car l’action d’épinglage d’un élément de rapport crée un abonnement [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] pour gérer la planification de l’actualisation des vignettes. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] nécessitent des informations d’identification stockées. La page **Mes abonnements** affiche une message d’erreur similaire au suivant :  
  
        PowerBI Delivery error: dashboard: SSRS items, visual: Image3, error: The current action cannot be completed. The user data source credentials do not meet the requirements to run this report or shared dataset. Either the user data source credentials are not stored in the report server database, or the user data source is configured not to require credentials but the unattended execution account is not specified. (rsInvalidDataSourceCredentialSetting)
  
-   **Informations d’identification Power BI expirées :**  vous tentez d’épingler un élément et voyez le message d’erreur suivant s’afficher. Dans le [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], cliquez sur **Mes paramètres** , puis, dans la page Mes paramètres, cliquez sur **Se connecter**. Pour plus d’informations, consultez  [Mes paramètres pour l’intégration de Power BI &#40;portail web&#41;](http://msdn.microsoft.com/en-us/85c2fac7-80bf-45b7-8654-764b5f5231f5) .  
  
        Cannot Pin : Unexpected Server Error: Missing, invalid or expired Power BI credentials.  
  
-   **Impossible d’épingler**: si vous tentez d’épingler un élément à un tableau de bord qui est en lecture seule, un message d’erreur similaire au suivant s’affiche :  
  
        Server Error : The item 'Dashboard deleted 015cf022-8e2f-462e-88e5-75ab0a04c4d0' cannot be found. (rsItemNotFound)  
  
##  <a name="bkmk_subscription_management"></a> Gestion des abonnements  
 En plus des problèmes d’abonnement décrits dans la section de dépannage, les informations suivantes vous aideront à gérer les abonnements liés à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] .
  
-   **Nom de l’élément modifié :** si un élément de rapport épinglé est renommé ou supprimé, la vignette [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] n’est plus mise à jour et un message d’erreur similaire au suivant s’affiche.  Si vous rétablissez le nom d’origine de l’élément, l’abonnement opère à nouveau et la vignette s’actualise conformément à la planification de celui-ci.  
  
        PowerBI Delivery error: dashboard: SSRS items, visual: Image1, error: Error: Report item 'Image1' cannot be found.  
  
     Vous pouvez également modifier les propriétés de l’abonnement et remplacer le **Nom de l’élément visuel du rapport** par le nom d’élément de rapport approprié. ![changer les éléments visuels utilisés pour l’actualisation de Power BI](../reporting-services/media/ssrs-powerbi-subscription-visual.png "changer les éléments visuels utilisés pour l’actualisation de Power BI")  
  
-   **Supprimer une vignette**. Si vous supprimez une vignette dans [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)], l’abonnement associé n’est pas supprimé dans [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] et dans la page **Mes abonnements**. Un message d’erreur similaire au suivant s’affiche. Vous pouvez supprimer l’abonnement  
  
        PowerBI Delivery error: dashboard: SSRS items, visual: Image3, error: The item 'Tile deleted af7131d9-5eaf-480f-ba45-943a07d19c9f' cannot be found.  

## <a name="video"></a>Vidéo

<iframe width="560" height="315" src="https://www.youtube.com/embed/QhPQObqmMPc" frameborder="0" allowfullscreen></iframe>

## <a name="see-also"></a> Voir aussi  
 [Intégration du serveur de rapports Power BI &#40;Gestionnaire de configuration&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md)   
 [Mes paramètres pour l’intégration de Power BI &#40;portail web&#41;](http://msdn.microsoft.com/en-us/85c2fac7-80bf-45b7-8654-764b5f5231f5)  
 [Tableaux de bord dans Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)  
  
  
[!INCLUDE[feedback_stackoverflow_msdn_connect_md](../includes/feedback-stackoverflow-msdn-connect-md.md)]

