---
title: Résoudre les problèmes de conception de rapports avec Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a0d103da-5a3e-475c-a71a-9e23476095e2
caps.latest.revision: 5
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3e81a3b98bbfe6507c768e541ac0fc613d28d52e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-report-design-issues-with-reporting-services"></a>Résoudre les problèmes de conception de rapports avec Reporting Services
Des problèmes de conception de rapport peuvent se produire lorsque vous créez la mise en page de rapport en mode Conception dans une application de création de rapport. Utilisez cette rubrique pour vous aider à résoudre ces problèmes.   
  
## <a name="why-does-my-text-box-show-only-a-single-value-and-not-repeat-for-every-row"></a>Pourquoi ma zone de texte affiche-t-elle seulement une valeur unique qui ne se répète pas à chaque ligne ?  
Une zone de texte avec une référence de champ dataset est rendue une seule fois et affiche la première valeur dans le dataset.   
  
**Le parent d'une zone de texte est le corps du rapport**  
  
  
Une zone de texte ajoutée directement à l'aire de conception peut uniquement afficher une valeur d'agrégation pour un dataset.  
  
Pour vérifier le conteneur parent d’une zone de texte, sélectionnez la zone de texte, puis dans le volet Propriétés, faites défiler la liste jusqu’à **Parent**.   
  
Si vous souhaitez que les zones de texte affichent chaque valeur dans un dataset, utilisez une région de données, telle qu'une table ou une matrice. Par défaut, chaque cellule dans une table ou une matrice contient une zone de texte. Faites glisser des champs de dataset dans chaque cellule.   
  
## <a name="why-cant-i-add-total-pages-to-my-report"></a>Pourquoi ne puis-je pas ajouter le nombre total de pages à mon rapport ?  
Les champs intégrés `[&PageNumber]` et `[&TotalPages]` ne sont pas valides dans le corps du rapport.   
  
PageNumber et TotalPages sont uniquement valides dans l’en-tête de page et le pied de page.  
  
  
Les champs intégrés [&PageNumber] et [&TotalPages] sont uniquement valides dans l'en-tête de page et le pied de page.   
  
Pour ajouter [&PageNumber] ou [&TotalPages] à un rapport, vous devez d'abord ajouter un en-tête de page ou un pied de page. Pour plus d’informations, voir [Ajouter ou supprimer un en-tête de page](../../reporting-services/report-design/add-or-remove-a-page-header-or-footer-report-builder-and-ssrs.md).  
  
> [!NOTE]  
> Le fait d'inclure [&TotalPages] dans l'en-tête de page ou le pied de page peut avoir des conséquences sur le traitement du rapport. Pour plus d’informations, voir « Dépannage de rapports : rapports exportés dans un format de fichier spécifique».  
[Résoudre les problèmes de traitement des rapports Reporting Services](../../reporting-services/troubleshooting/troubleshoot-processing-of-reporting-services-reports.md).  
  
## <a name="how-do-i-design-two-tables-or-a-chart-and-a-table-to-display-side-by-side"></a>Comment concevoir deux tables ou un graphique et une table affichés côte à côte ?  
La conception d'un rapport ne constitue pas une expérience WYSISYG (« tel écrit, tel écran »). Le processeur de rapports combine les données, les éléments de rapport, les informations de mise en page du rapport (telles que l'espace blanc), les conteneurs et les expressions pour produire un rapport compilé. Celui-ci est ensuite passé à un convertisseur de rapport qui « met en page » ce rapport pour l'affichage spécifié : soit interactif pour un navigateur HTML, soit en tant que format de fichier. Les algorithmes de disposition automatiques peuvent produire une mise en page de rapport que vous souhaitez modifier.   
  
### <a name="rendering-rules-use-page-size-containers-peer-objects-relative-placement-and-white-space-to-determine-layout"></a>Les règles de rendu utilisent la taille de page, les conteneurs, les objets homologues, le positionnement relatif et l'espace blanc pour déterminer la mise en page  
En général, un rapport s'élargit pour accommoder ses données et met de côté d'autres éléments de rapport.   
  
Pour grouper plusieurs régions de données ou éléments de rapport, placez-les dans le même conteneur parent. Par exemple, placez un graphique et une table dans un conteneur rectangulaire et alignez leurs bords supérieurs pour les afficher côte par côte. Pour plus d’informations, voir [Comportement de rendu (Générateur de rapports et SSRS)](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
[Résoudre les problèmes de récupération des données avec des rapports Reporting Services](../../reporting-services/troubleshooting/troubleshoot-data-retrieval-issues-with-reporting-services-reports.md)  
[Résolution des problèmes d’abonnements et de remise de Reporting Services](../../reporting-services/troubleshooting/troubleshoot-reporting-services-subscriptions-and-delivery.md)  
  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect.md)]

