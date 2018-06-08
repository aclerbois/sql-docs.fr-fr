---
title: Créer un rapport mobile Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: mobile-reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e84dc855-aede-4fb4-b721-e6d8787961f4
caps.latest.revision: 10
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 65f20d39e13c3c388b5fbe873d5a3c7491a6da38
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-reporting-services-mobile-report"></a>Créer un rapport mobile Reporting Services
L’Éditeur de rapports mobiles SQL Server vous permet de créer rapidement des rapports mobiles SQL Server 2016 Reporting Services bien adaptés à la taille de votre écran, sur une aire de conception dotée de lignes et de colonnes de grille réglables, ainsi que d’éléments de rapport mobile flexibles.  
  
Quand vous créez un rapport mobile pour la première fois, vous pouvez installer Éditeur de rapports mobiles SQL Server sur votre machine locale à partir du portail web de Reporting Services. Vous pouvez aussi l’installer à partir du [Centre de téléchargement Microsoft](http://go.microsoft.com/fwlink/?LinkID=733527). Après cela, vous pouvez le démarrer à partir du portail web ou localement.   
    
1. Dans la barre située en haut du portail web Reporting Services, sélectionnez **Nouveau** > **Rapport mobile**.  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
     
2. Sous l’onglet **Disposition** de l’Éditeur de rapports mobiles SQL Server, sélectionnez un navigateur, une jauge, un graphique, une carte ou une grille de données et faites glisser l’élément vers la grille de création.  
  
3. Saisissez le coin inférieur droit de l’élément et redimensionnez-le.  
  
   ![SSMRP_ResizeChart](../../reporting-services/mobile-reports/media/ssmrp-resizechart.png)  
  
   Il s’agit de la grille de création **principale** dans laquelle vous définissez les éléments à inclure dans votre rapport. Vous pouvez ensuite [ajuster la disposition du rapport pour un affichage sur tablette ou téléphone](../../reporting-services/mobile-reports/lay-out-a-reporting-services-mobile-report-for-phone-or-tablet.md).     
     
   La zone **Propriétés visuelles** , située en dessous de la grille de création, vous permet de définir différentes propriétés.  
     
4. Sélectionnez l’onglet **Données** dans le coin supérieur gauche. Le graphique s’affiche avec des données simulées déjà associées.   
  
   ![SSMRP_SimTable](../../reporting-services/mobile-reports/media/ssmrp-simtable.png)  
  
5. Sélectionnez **Ajouter des données** dans le coin supérieur droit.  
  
6. Sélectionnez **Excel local** ou **Serveur de rapports**.  
  
   >**Conseils**: si vous ajoutez des données à partir d’Excel, veillez à :  
    >* [Préparer les données Excel](../../reporting-services/mobile-reports/prepare-excel-data-for-reporting-services-mobile-reports.md) à utiliser dans votre rapport mobile.  
    >* Fermer le fichier avant d’ajouter les données.  
7. Sélectionnez les feuilles de calcul souhaitées, puis sélectionnez **Importer**.   
   Vous pouvez ajouter plusieurs feuilles de calcul d’un classeur en même temps.  
    
     ![SSMRP_AddExcelData](../../reporting-services/mobile-reports/media/ssmrp-addexceldata.png)  
  
8. Toujours sous l’onglet **Données** , dans la zone **Propriétés des données** , sélectionnez la table et le champ de votre choix dans le graphique.  
  
   ![SSMRP_DataProps](../../reporting-services/mobile-reports/media/ssmrp-dataprops.png)  
  
9. Sous l’onglet **Disposition** , la zone **Propriétés visuelles** vous permet de définir des propriétés telles que **Titre**, **Unité de temps**et **Format de nombre**.  
  
   ![SSMRP_ChartVizProps](../../reporting-services/mobile-reports/media/ssmrp-chartvizprops.png)  
    
10. Sélectionnez **Aperçu** dans le coin supérieur gauche pour afficher un aperçu de votre rapport.  
  
11. Il est désormais temps d’enregistrer votre rapport. Sélectionnez l’icône Enregistrer dans le coin supérieur gauche, puis **Enregistrer localement** ou **Enregistrer sur le serveur**.  
  
   Pour l’enregistrer sur un serveur, vous devez avoir accès à un serveur de rapports SQL Server 2016 Reporting Services.  
     
   ### <a name="see-also"></a>Voir aussi  
     
-   [Créer et publier des rapports mobiles avec l’Éditeur de rapports mobiles SQL Server](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)  
-   [Concevoir un rapport mobile Reporting Services pour téléphone ou tablette](../../reporting-services/mobile-reports/lay-out-a-reporting-services-mobile-report-for-phone-or-tablet.md)  
  
   
