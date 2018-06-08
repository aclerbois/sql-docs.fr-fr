---
title: Créer un rapport de tableau de base (didacticiel SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 11/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
helpviewer_keywords:
- walkthroughs [Reporting Services]
- tutorials [Reporting Services]
- reports [Reporting Services], creating
ms.assetid: 3b539b4b-26f2-4c0b-b506-80f175679a46
caps.latest.revision: 67
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 6b147c1051bbf841bb54b88dd7d71221ddfa2ce2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-basic-table-report-ssrs-tutorial"></a>Créer un rapport de tableau de base (didacticiel SSRS)

Dans ce didacticiel, vous utilisez le Concepteur de rapports dans SQL Server Data Tools pour créer un rapport paginé [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] de base avec un tableau, basé sur la base de données **[!INCLUDE[ssSampleDBAdventureworks2014_md](../includes/sssampledbadventureworks2014-md.md)]**. Vous pouvez également créer des rapports paginés [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] avec le Générateur de rapports. 

À mesure que vous parcourez ce didacticiel, vous allez créer un projet de rapport, configurer des informations de connexion, définir une requête, ajouter une région de données de table, regrouper certains champs et calculer leurs totaux, ainsi qu’afficher un aperçu du rapport.  
  
## <a name="requirements"></a>Spécifications  
Les éléments suivants doivent cependant être installés sur votre système :  
  
-   Moteur de base de données [!INCLUDE[msCoName](../includes/msconame-md.md)] SQL Server.  
  
-   [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] en mode natif.  
  
-   La base de données [!INCLUDE[ssSampleDBAdventureworks2014_md](../includes/sssampledbadventureworks2014-md.md)] .  Pour plus d’informations, consultez [Exemples de bases de données Adventure Works 2014](https://github.com/Microsoft/sql-server-samples/releases).  
  
 -   [SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md) doté des composants SQL Server Reporting Services, afin que vous disposiez du Concepteur de rapports.    
  
Vous devez également disposer d’autorisations en lecture seule pour pouvoir récupérer les données de la base de données [!INCLUDE[ssSampleDBAdventureworks2014_md](../includes/sssampledbadventureworks2014-md.md)] .

**Durée estimée pour effectuer ce didacticiel :** 30 minutes.
  
## <a name="tasks"></a>Tâches  
[Leçon 1 : Création d’un projet Report Server &#40;Reporting Services&#41;](../reporting-services/lesson-1-creating-a-report-server-project-reporting-services.md)  
  
[Leçon 2 : Spécification des informations de connexion &#40;Reporting Services&#41;](../reporting-services/lesson-2-specifying-connection-information-reporting-services.md)  
  
[Leçon 3 : Définition d’un dataset destiné à un rapport de table &#40;Reporting Services&#41;](../reporting-services/lesson-3-defining-a-dataset-for-the-table-report-reporting-services.md)  
  
[Leçon 4 : Ajout d’une table au rapport &#40;Reporting Services&#41;](../reporting-services/lesson-4-adding-a-table-to-the-report-reporting-services.md)  
  
[Leçon 5 : Mise en forme d’un rapport &#40;Reporting Services&#41;](../reporting-services/lesson-5-formatting-a-report-reporting-services.md)  
  
[Leçon 6 : Ajout d’un regroupement et de totaux &#40;Reporting Services&#41;](../reporting-services/lesson-6-adding-grouping-and-totals-reporting-services.md)  

## <a name="next-steps"></a>Étapes suivantes

[Didacticiels sur Reporting Services](../reporting-services/reporting-services-tutorials-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
