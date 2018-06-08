---
title: Créer un rapport mobile tabulé à l’aide de l’extraction | Rapports mobiles Reporting Services | Microsoft Docs
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
ms.assetid: c4d5d80d-370a-4a6d-8b76-698bd5ba5ba6
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: d3632642114f84855fb1c4e00df810499d23b9d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-tabbed-mobile-report-by-using-drillthrough"></a>Créer un rapport mobile tabulé à l’aide de l’extraction
Découvrez comment créer un rapport mobile [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] qui apparaît et fonctionne comme un rapport tabulé à l’aide de l’extraction et des paramètres.

Par exemple, dans ce rapport, les jauges de la partie supérieure fonctionnent comme des onglets. Quand vous cliquez sur la jauge des transports, les données qui se trouvent dans le reste du graphique sont filtrées sur les données relatives aux transports.

![06-Mobile-Report-Web-Viewer-Transportation](../../reporting-services/mobile-reports/media/tabbed-mobile-report-web-viewer-transportation-complete.png)

En réalité, il s’agit d’un ensemble de cinq rapports, ayant chacun un paramètre différent qui permet de filtrer les données en fonction de la jauge sélectionnée en haut du rapport. Vous commencez par créer cinq rapports. Ensuite, pour chacun des cinq rapports, vous faites des quatre autres jauges des extractions pour les quatre autres rapports.

Voici les étapes de cet exemple :

## <a name="create-the-basic-report"></a>Créer le rapport de base

1. Créez un rapport appelé « Ventes » comportant cinq jauges :

    * Ventes
    * Transport
    * Carburant
    * Stockage
    * Dépenses diverses

   ![01-Sales-Mobile-Report-Publisher](../../reporting-services/mobile-reports/media/01-sales-mobile-report-publisher.png)
    
2. Définissez **Accentuation** sur **On** pour la jauge Ventes afin qu’elle contraste avec le reste du rapport ; dans le cas présent, blanc sur noir.

    ![01a-Sales-Accent-Mobile-Report-Publisher](../../reporting-services/mobile-reports/media/01a-sales-accent-mobile-report-publisher.png)
    
3. Enregistrez le rapport sur un serveur de rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].

## <a name="make-copies-of-the-report"></a>Effectuer des copies du rapport

1. Faites quatre copies du rapport Ventes et nommez-les : 

    * Transport
    * Carburant
    * Stockage
    * Dépenses diverses

3. Enregistrez-les sur le serveur de rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].

## <a name="set-the-gauge-as-a-drillthrough"></a>Définir la jauge comme extraction

Dans cette section, vous définissez chaque jauge (à part la jauge Ventes) comme extraction pour son propre rapport.

1. Dans le rapport Ventes, sélectionnez la jauge Transport.

    ![02-Sales-Create-DrillThrough-Mobile-Report-Publisher](../../reporting-services/mobile-reports/media/02-sales-create-drillthrough-mobile-report-publisher.png)

2. Sélectionnez l’onglet **Disposition**, puis dans le volet **Propriétés visuelles**, sélectionnez **Cible d’extraction**.

3. Sélectionnez **Rapport mobile**.

4. Accédez au rapport qui sera la destination de l’extraction et sélectionnez-le ; dans le cas présent « Finances - Transport ».

    ![03-Sales-Select-Dashboard-Mobile-Report-Publisher](../../reporting-services/mobile-reports/media/03-sales-select-dashboard-mobile-report-publisher.png)

5. Dans **Configurer le rapport cible**, sélectionnez le paramètre de filtrer du rapport, puis sélectionnez **Appliquer**.

   ![04-Sales-Apply-Parameters-Mobile-Report-Publisher](../../reporting-services/mobile-reports/media/04-sales-apply-parameters-mobile-report-publisher.png)
   
6. Répétez ces étapes pour chaque autre jauge du rapport Ventes. 

## <a name="set-the-gauges-for-the-other-reports"></a>Définir les jauges pour les autres rapports

1.  Ouvrez le rapport Transport, définissez la jauge Ventes comme extraction pour le rapport Ventes, et les trois autres jauges comme des extractions pour leurs propres rapports.

2. Dans le rapport Transport, définissez le paramètre **Accentuation** de la jauge Transport sur **On** de sorte qu’elle contraste avec le reste du rapport.

3. Répétez ces étapes pour les rapports Carburant, Stockage et Dépenses diverses. 

## <a name="view-the-report-in-the-web-portal"></a>Afficher le rapport dans le portail web

1. Accédez au serveur de rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] et ouvrez l’un des rapports. 

2. Notez que chaque jauge présente une icône d’extraction en haut à droite.

    ![Web-Viewer-drillthrough-icon-mobile-report-builder](../../reporting-services/mobile-reports/media/web-viewer-drillthrough-icon-mobile-report-builder.png)

3. Sélectionnez l’une des jauges pour accéder au rapport filtré des données de cette jauge.

   ![06-Mobile-Report-Web-Viewer-Transportation](../../reporting-services/mobile-reports/media/06-mobile-report-web-viewer-transportation.png)

### <a name="see-also"></a>Voir aussi
    
* [Ajouter des paramètres à un rapport mobile](../../reporting-services/mobile-reports/add-parameters-to-a-mobile-report-reporting-services.md)
* [Ajouter l’extraction à partir d’un rapport mobile à d’autres rapports mobiles ou des URL](../../reporting-services/mobile-reports/add-drillthrough-from-a-mobile-report-to-other-mobile-reports-or-urls.md)




  

