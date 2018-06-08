---
title: Obtenir des données à partir de jeux de données partagés dans les rapports mobiles Reporting Services | Microsoft Docs
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
ms.assetid: 0b846451-c8d0-412c-802d-a42bb1ff8c63
caps.latest.revision: 18
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: fb83dd013101b14ad45da1fb9ef091dcbea727d1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="get-data-from-shared-datasets-in-reporting-services-mobile-reports"></a>Obtenir des données à partir de datasets partagés dans l’Éditeur de rapports mobiles
Outre le [chargement de données à partir de fichiers Excel](../../reporting-services/mobile-reports/prepare-excel-data-for-reporting-services-mobile-reports.md), l’Éditeur de rapports mobiles SQL Server peut également accéder à des données depuis pratiquement n’importe quelle source. L’accès aux données requiert une source de données partagée, configurée sur un portail web Reporting Services. En savoir plus sur la [création de sources de données partagées](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md) et sur la [création de jeux de données partagés](../../reporting-services/report-data/manage-shared-datasets.md).  
  
Dès lors que vous avez configuré les sources de données partagées et les jeux de données partagés sur le serveur Reporting Services, vous pouvez les utiliser dans les rapports mobiles que vous créez dans [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)].   
  
Une fois connecté à un serveur [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] à partir de l’ [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)], il est simple de connecter un rapport mobile à un dataset partagé.   
  
1. Sous l’onglet **Données** , sélectionnez **Ajouter des données**.  
  
2. Sélectionnez **Serveur de rapports**.   
  
3.  S’il s’agit de votre première connexion au serveur, renseignez le nom du serveur ainsi que vos nom d’utilisateur et mot de passe. Tapez le nom du serveur dans la zone d’adresse Serveur en respectant le format suivant :  
  
    \<“nom_serveur”>/reports/  
  
    Comme dans l’exemple ci-dessous :  
       
    ![SSMRP_ConnectToServer](../../reporting-services/mobile-reports/media/ssmrp-connecttoserver.png)  
      
  
4. Lorsque vous sélectionnez le serveur [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , les jeux de données disponibles s’affichent dans des dossiers. Sélectionnez un jeu de données à importer les données dans [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)].  
  
   ![SS_MRP_ServerData](../../reporting-services/mobile-reports/media/ss-mrp-serverdata.png)  
  
Une fois le jeu de données importé, vous pouvez concevoir votre rapport mobile de la même manière qu’avec des données simulées ou qu’avec des données locales provenant d’un fichier Excel.  
  
Par défaut, le jeu de données partagé reflète toujours les données les plus récentes, car chaque fois qu’un utilisateur consulte un rapport mobile dérivé de ce jeu de données, SQL Server exécute la requête sous-jacente et retourne les données les plus récentes. Cette approche n’est certainement pas idéale si un grand nombre d’utilisateurs consultent votre rapport mobile ; vous pouvez donc configurer la mise en cache pour exécuter régulièrement la requête et mettre en cache le jeu de données obtenu. Ce billet de blog explique [le principe de fonctionnement de la mise en cache et de l’actualisation des données dans le portail web](http://christopherfinlan.com/2016/02/10/so-refreshinghow-data-refresh-works-with-mobile-reports-and-kpis-in-reporting-services/).  
  
## <a name="add-edit-or-remove-a-report-server"></a>Ajouter, modifier ou supprimer un serveur de rapports  
  
Si vous êtes déjà connecté à un serveur de rapports, lorsque vous sélectionnez **Ajouter des données** sous l’onglet Données, vous ne voyez pas d’option permettant d’ajouter un autre serveur de rapports. Procédez plutôt aux étapes suivantes.  
  
1. Dans le coin supérieur gauche, sélectionnez **Connexions**.  
  
   ![SSMRP_AddConnectionIcon](../../reporting-services/mobile-reports/media/ssmrp-addconnectionicon.png)  
     
   Le volet Connexion serveur s’ouvre sur la droite.  
     
   ![SSMRP_ServerConnectnPane](../../reporting-services/mobile-reports/media/ssmrp-serverconnectnpane.png)  
     
2. Ajoutez une nouvelle connexion serveur, ou modifiez ou supprimez des connexions existantes.  
  
### <a name="see-also"></a>Voir aussi  
- [Créer et publier des rapports mobiles avec l’Éditeur de rapports mobiles SQL Server](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)  
-  [Portail web (SSRS en mode natif)](../../reporting-services/web-portal-ssrs-native-mode.md)  
-  Affichez les [rapports mobiles SQL Server et les indicateurs de performance clés dans l’application iPad](https://pbiwebprod-docs.azurewebsites.net/en-us/documentation/powerbi-mobile-ipad-kpis-mobile-reports)  (Power BI pour iOS)  
-  Affichez les [rapports mobiles SQL Server et les indicateurs de performance clés dans l’application iPhone](https://pbiwebprod-docs.azurewebsites.net/en-us/documentation/powerbi-mobile-iphone-kpis-mobile-reports) (Power BI pour iOS)  
  
  
  
  

