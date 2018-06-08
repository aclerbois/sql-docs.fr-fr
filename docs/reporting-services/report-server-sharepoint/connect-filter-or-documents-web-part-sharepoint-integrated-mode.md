---
title: Connecter un composant WebPart Filtre ou Documents à un composant WebPart Visionneuse de rapports Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 10/05/2017
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
ms.openlocfilehash: f736a1bd51628f5a03dc2fb29b53b1a3a8eb3b26
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-filter-or-documents-web-part-with-a-reporting-services-report-viewer-web-part"></a>Connecter un composant WebPart Filtre ou Documents à un composant WebPart Visionneuse de rapports Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Si vous utilisez un produit SharePoint, vous pouvez créer un tableau de bord ou une page WebPart qui inclut un composant WebPart Filtre ou un composant WebPart Documents et un composant WebPart Visionneuse de rapports. Les versions prises en charge sont [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] et [!INCLUDE[SPS2010](../../includes/sps2010-md.md)]. [!INCLUDE[winSPServ3](../../includes/winspserv3-md.md)] et [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007 sont également pris en charge. En connectant un composant WebPart Filtre, les utilisateurs qui sélectionnent des valeurs de filtre dans un composant WebPart Filtre peuvent envoyer ces valeurs à un rapport paramétrable sur la même page. En connectant un composant WebPart Documents, les utilisateurs qui cliquent sur des rapports dans la bibliothèque de documents peuvent afficher ces rapports dans un composant WebPart Visionneuse de rapports adjacent.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

 Le composant WebPart Filtre sert à envoyer des valeurs à un ou plusieurs paramètres d’un rapport. Pour utiliser un composant WebPart Filtre, le rapport doit avoir des paramètres relatifs à ce composant qui sont compatibles avec les valeurs, le type de données et le format envoyés par le composant WebPart.  
  
 Le composant WebPart Documents est associé à la bibliothèque de documents du site d’accueil. Pour afficher, ajouter ou supprimer des éléments de la bibliothèque de documents, cliquez sur **Afficher tout le contenu du site**. Dans Bibliothèques, cliquez sur **Documents**. Vous pouvez utiliser les menus **Nouveau**, **Télécharger**et **Actions** pour gérer les éléments de la bibliothèque de documents.  
  
## <a name="connect-a-filter-web-part"></a>Connecter un composant WebPart Filtre
  
1.  Ouvrez ou créez la page ou le tableau de bord de composant WebPart.  
  
2.  Dans le menu **Actions de site** , cliquez sur **Modifier la page**.  
  
3.  Cliquez sur **Ajouter un composant WebPart**.  
  
4.  Dans **Tous les composants WebPart**, dans la catégorie **Divers**, sélectionnez **Visionneuse de rapports SQL Server Reporting Services**.  
  
5.  Cliquez sur **Ajouter**. Le composant WebPart est ajouté en haut de la zone.  
  
6.  Dans une autre zone de la même page ou du même tableau de bord de composant WebPart, cliquez sur **Ajouter un composant WebPart**.  
  
7.  Dans **Tous les composants WebPart**, dans la section **Filtres**, sélectionnez un composant WebPart.  
  
8.  Cliquez sur **Ajouter**. Le composant WebPart est ajouté en haut de la zone.  
  
9. Dans la zone qui contient le composant WebPart, cliquez sur le menu **modifier** du composant WebPart, pointez sur **Connexions**, puis sur **Envoyer les valeurs de filtre à** et sélectionnez **Visionneuse de rapports** - *nom du rapport*.  
  
10. Vérifiez vos modifications et enregistrez la page.  
  
## <a name="connect-a-documents-web-part"></a>Pour connecter un composant WebPart Documents  
  
1.  Ouvrez ou créez la page ou le tableau de bord de composant WebPart.  
  
2.  Dans le menu **Actions de site** , cliquez sur **Modifier la page**.  
  
3.  Cliquez sur **Ajouter un composant WebPart**.  
  
4.  Dans **Tous les composants WebPart**, dans la section **Listes et bibliothèque**, sélectionnez **Documents**.  
  
5.  Cliquez sur **Ajouter**. Le composant WebPart est ajouté en haut de la zone.  
  
6.  Cliquez au bas du volet d’outils sur **Appliquer** , puis sur **OK** pour fermer le volet.  
  
7.  Dans une autre zone de la même page ou du même tableau de bord de composant WebPart, cliquez sur **Ajouter un composant WebPart**.  
  
8.  Dans **Tous les composants WebPart**, dans la catégorie **Divers**, sélectionnez **Visionneuse de rapports SQL Server Reporting Services**.  
  
9. Cliquez sur **Ajouter**. Le composant WebPart est ajouté en haut de la zone.  
  
10. Dans la zone qui contient le composant WebPart, cliquez sur le menu **modifier** du composant WebPart, pointez sur **Connexions**, puis sur **Source des définitions de rapport**, puis sélectionnez **Documents**.  
  
11. Vérifiez vos modifications et enregistrez la page.  
  
## <a name="see-also"></a>Voir aussi

 [Ajouter le composant WebPart Visionneuse de rapports à une page web](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md)   
 [Composant WebPart Visionneuse de rapports sur un site SharePoint](../../reporting-services/report-server-sharepoint/report-viewer-web-part-on-a-sharepoint-site.md)   
 [Personnaliser le composant WebPart de la Visionneuse de rapports](../../reporting-services/report-server-sharepoint/customize-the-report-viewer-web-part.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
