---
title: Aperçu des rapports | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Report Designer [Reporting Services], previewing reports
- previewing reports [Reporting Services]
- printing previews
- test servers [Reporting Services]
ms.assetid: 85117f6c-828e-45c9-810f-e700d9bfba67
caps.latest.revision: 44
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2cdd46a01c2151a828d58f851e3a4bdcb02e7483
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="previewing-reports"></a>Aperçu des rapports
  Quand vous concevez un rapport     [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous pouvez le visualiser avant de le publier sur l’environnement de production. Pour ce faire, vous disposez de plusieurs méthodes : en passant en mode aperçu dans le Concepteur de rapports, en utilisant la fenêtre d'aperçu du Concepteur de rapports et en publiant le rapport sur un serveur de rapports dans un environnement de test.  
  
> [!NOTE]  
>  Lorsque vous prévisualisez un rapport, les données de ce rapport sont mises en cache dans un fichier sur l'ordinateur local. Ainsi, lorsque vous prévisualisez une nouvelle fois ce rapport (au moyen des mêmes requête, paramètres et informations d'identification), le Concepteur de rapports récupère l'exemplaire mis en cache au lieu d'exécuter à nouveau la requête. Le fichier de données est enregistré sous *\<nom_rapport>*.rdl.data dans le même répertoire que le fichier de définition de rapport. Le fichier n'est pas supprimé lorsque vous fermez le Générateur de rapports.  
  
## <a name="preview-mode"></a>Mode Aperçu  
 Pour prévisualiser un rapport dans le Concepteur de rapports, cliquez sur ![ssrs_ssdt_preview](../../reporting-services/media/ssrs-ssdt-preview.png "ssrs_ssdt_preview"). Cette opération entraîne l'exécution du rapport localement. Elle utilise les mêmes fonctionnalités de traitement et de rendu de rapport que celles fournies par le serveur de rapports. Le rapport qui est affiché est une image interactive. Vous pouvez sélectionner des paramètres, cliquer sur des liens, afficher l'explorateur de documents, et développer ou réduire des zones masquées du rapport. Vous pouvez aussi exporter le rapport dans n'importe quel format de rendu installé.  
  
## <a name="standalone-preview"></a>Afficheur  
 Une autre manière d'afficher l'aperçu d'un rapport consiste à exécuter le projet de rapport dans une configuration de débogage, par exemple pour déboguer les assemblys personnalisés que vous écrivez. Le rapport s’ouvre dans votre navigateur par défaut. Vous pouvez exécuter un projet de trois manières :  
  
-   Dans le menu **Déboguer** , en cliquant sur **Démarrer le débogage** .  
  
-   En cliquant sur le bouton **Démarrer** de la barre d’outils standard [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ![ssrs_ssdt_startdebug](../../reporting-services/reports/media/ssrs-ssdt-startdebug.png "ssrs_ssdt_startdebug").  
  
-   En appuyant sur **F5**.  
  
 Si vous utilisez une configuration de projet qui crée le rapport mais ne le déploie pas, le rapport spécifié dans la propriété **StartItem** de la configuration actuelle s'ouvre dans une fenêtre d'aperçu distincte. La fenêtre d'aperçu affiche le rapport de la même manière et avec les mêmes fonctionnalités que le mode Aperçu.  
  
> [!NOTE]  
>  Avant de déboguer un rapport, vous devez définir un élément de départ. C’est par exemple le cas si vous exécutez le mode débogage et que le navigateur ouvre la page de serveur de rapports principale et pas votre rapport en mode Aperçu. Pour ce faire, dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet de rapport, cliquez sur **Propriétés**, puis sur le nom du rapport à afficher dans la zone **StartItem**.  
  
 Si vous souhaitez afficher l’aperçu d’un rapport particulier, sans qu’il soit l’élément de départ du projet, sélectionnez une configuration qui crée le rapport mais ne le déploie pas (par exemple, la configuration DebugLocal), cliquez avec le bouton droit sur le rapport, puis sélectionnez **Exécuter**. Vous devez choisir une configuration qui ne déploie pas le rapport, sinon il sera publié sur le serveur de rapports au lieu de s'afficher localement dans une fenêtre d'aperçu.  
  
## <a name="publishing-to-a-test-server"></a>Publication sur un serveur de test  
 Vous pouvez également tester les rapports en les publiant sur un serveur de test, accéder aux rapports et en afficher un aperçu. La publication d'un rapport sur un serveur de test revient, au niveau de la procédure, à publier un rapport sur un serveur de production. Pour plus d’informations sur la publication d’un rapport, consultez [Publication de rapports sur un serveur de rapports](../../reporting-services/reports/publishing-reports-to-a-report-server.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Imprimer des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
 [Imprimer un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-a-report-report-builder-and-ssrs.md)   
 [Publier des rapports](http://msdn.microsoft.com/library/ef5a514e-e818-4041-a8b0-15835f9a046b)   
 [Utilisation d'assemblages personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
