---
title: Enregistrement des rapports (Générateur de rapports) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 59ddc4b8-9517-4d3f-9c88-a07e9907cecb
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: fbf40f6911d3d8a77bfc37a749a12864b775ebd1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="saving-reports-report-builder"></a>Enregistrement des rapports (Générateur de rapports)
  Dans le Générateur de rapports, vous avez la possibilité d’enregistrer un rapport paginé sur un serveur de rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , une bibliothèque SharePoint, un partage de fichiers sur lequel vous disposez d’une autorisation d’accès en écriture, ou sur votre ordinateur. 
  
Lorsque vous enregistrez un rapport, vous enregistrez en fait la définition de rapport, laquelle décrit la disposition du rapport. Les données ne sont pas enregistrées. Chaque fois que vous exécutez le rapport, les données de rapport sont actualisées et peuvent être différentes de celles obtenues lors de la dernière exécution du rapport.  
  
 Si vous souhaitez enregistrer le rapport sous un format différent ou enregistrer la définition de rapport avec les données, utilisez l'une des fonctionnalités [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] suivantes :  
  
-   Exportez un rapport rendu vers un format de fichier différent tel qu'un fichier séparé par des virgules (CSV) ou un classeur Excel et enregistrez le rapport dans ce format. Vous pouvez également générer des flux de données à partir de rapports et enregistrer les données de rapport.  
  
-   Créez des abonnements de rapport pour remettre et enregistrer des rapports sur un partage de fichiers.  
  
-   Utilisez l'historique de rapport pour enregistrer des versions de rapports rendus en tant que copies historiques.  
  
 Pour en savoir plus sur l’affichage et la gestion des rapports directement sur le serveur de rapports, consultez [Recherche, affichage et gestion des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md) et [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md).  
  
##  <a name="SavingReportDefinitions"></a> Enregistrement de rapports sur un serveur de rapports  
  L'enregistrement d'un rapport sur un serveur de rapports est également appelé « publication » d'un rapport. Bien que vous puissiez enregistrer des rapports sur votre ordinateur, l’enregistrement des rapports sur un serveur de rapports offre de nombreux avantages :  
  
-   Les rapports sont mis à la disposition des autres utilisateurs qui sont autorisés à accéder au dossier dans lequel vous avez enregistré le rapport.  
  
-   Les rapports peuvent être gérés et affichés sur le portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   Les ressources de rapport, telles que les sources de données, les images et les sous-rapports, sont stockées à un seul et même endroit pour un accès plus facile.  
  
-   Les rapports peuvent être remis aux autres grâce aux abonnements.  
  
-   Les rapports sont stockés en toute sécurité dans la base de données du serveur de rapports.  
  
-   Les exécutions de rapport peuvent être journalisées et fournir des informations d'audit et de performance.  
  
##  <a name="ExportingAndSavingReports"></a> Exportation et enregistrement des rapports  
 Si vous avez un petit nombre de rapports à archiver, optez pour la solution consistant à exporter un rapport puis à l'enregistrer dans un fichier. Une fois que vous avez exporté un rapport vers une application (au format PDF ou Excel par exemple), vous pouvez l'enregistrer en tant que fichier et le placer dans un répertoire partagé protégé sur le réseau. Vous pouvez également télécharger le fichier enregistré au format PDF ou Excel en tant qu'élément de ressource si vous souhaitez conserver toutes les copies d'un rapport, quel que soit leur format, dans la base de données du serveur de rapports. Pour plus d’informations sur l’exportation d’un rapport, consultez [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md) et [Télécharger un fichier ou un rapport](../../reporting-services/reports/upload-a-file-or-report-report-manager.md).  
  
##  <a name="UsingFileShareDelivery"></a> Utilisation de la remise par partage de fichiers  
 Si vous avez un grand nombre de rapports à archiver, créez un abonnement qui remet le rapport directement au système de fichiers. Pour cette approche, vous devez créer un abonnement pour chaque rapport, choisir un dossier partagé où stocker les rapports et définir une planification qui détermine le moment auquel le fichier est créé. Une fois l'abonnement défini, le serveur de rapports peut exécuter le rapport sans surveillance et ajouter des fichiers de rapports à l'archive à l'aide de la planification que vous fournissez. Vous pouvez également créer des planifications à usage unique si vous souhaitez archiver des rapports occasionnellement. Pour plus d’informations sur les abonnements et la remise dans un partage de fichiers, consultez [Remise par partage de fichiers dans Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md).  
  
##  <a name="UsingReportHistory"></a> Utilisation de l'historique de rapport  
 Vous pouvez également utiliser la fonctionnalité d'historique de rapport pour créer des copies historiques. Vous pouvez ensuite sauvegarder la base de données du serveur de rapports, puis stocker la sauvegarde à un emplacement sûr en vue d'une utilisation future. L'ensemble de l'historique de rapport (avec les rapports, les éléments de sources de données partagées, les dossiers, les abonnements et les planifications partagées) est stocké dans la base de données du serveur de rapports. Vous pouvez créer une sauvegarde pour conserver une copie permanente de l'historique et des métadonnées d'un rapport, notamment les informations d'abonnement indiquant ses différents destinataires. Pour plus d’informations, consultez [Créer, modifier et supprimer des instantanés dans l’historique de rapport](../../reporting-services/report-server/create-modify-and-delete-snapshots-in-report-history.md).  
 
##  <a name="HowTo"></a> Rubriques de procédures  
  
-   [Enregistrer des rapports sur un serveur de rapports &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/save-reports-to-a-report-server-report-builder.md)  
  
-   [Enregistrer un rapport dans une bibliothèque SharePoint &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/save-a-report-to-a-sharepoint-library-report-builder.md)  
   
## <a name="see-also"></a> Voir aussi  
 [Rapports, parties de rapports et définitions de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/reports-report-parts-and-report-definitions-report-builder-and-ssrs.md)   
 [Installer et désinstaller le Générateur de rapports](http://msdn.microsoft.com/library/2c9a5814-17bf-4947-8fb3-6269e7caa416)   
 [Recherche, affichage et gestion des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)   
 [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)   
 [Imprimer des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)  
  
  
