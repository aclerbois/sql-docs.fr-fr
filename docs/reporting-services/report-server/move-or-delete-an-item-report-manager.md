---
title: Déplacer ou supprimer un élément (Gestionnaire de rapports) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- moving items
- items [Reporting Services], moving
ms.assetid: 980a66c7-a18b-4af7-8954-45726fa517d6
caps.latest.revision: 45
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 17849b5e7baf9c09dce0f671ce4f60381b4523ff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="move-or-delete-an-item-report-manager"></a>Déplacer ou supprimer un élément (Gestionnaire de rapports)
  Les rapports et les éléments connexes que vous publiez sur un serveur de rapports sont stockés dans des dossiers. Vous pouvez déplacer ces éléments dans un dossier différent de sorte que les références à ces éléments soient gérées automatiquement par le serveur de rapports. Avant de supprimer un élément, demandez-vous si d'autres éléments en dépendent.  
  
## <a name="move-an-item"></a>Déplacer un élément  
 Vous pouvez déplacer des éléments de serveur de rapports vers des emplacements de dossiers dans l'arborescence des dossiers du serveur de rapports. Lorsque vous déplacez un élément, toutes les propriétés (y compris les paramètres de sécurité) accompagnent l'élément vers son nouvel emplacement. Lorsque vous déplacez un dossier, tous les éléments qu'il contient l'accompagnent.  
  
 Dans le Gestionnaire de rapports, les éléments que vous pouvez déplacer sont indiqués dans l'arborescence des dossiers. Le tableau suivant indique l'icône associée à chaque élément pouvant être déplacé.  
  
|Icône|Élément pouvant être déplacé|  
|----------|-------------------|  
|![Icône Rapport](../../reporting-services/report-server/media/hlp-16doc.gif "Icône Rapport")|Rapport|  
|![Icône Rapport lié](../../reporting-services/report-server/media/hlp-16linked.gif "Icône Rapport lié")|Rapport lié|  
|![Icône Dossier](../../reporting-services/report-server/media/hlp-16folder.gif "Icône Dossier")|Dossier|  
|![Icône Ressource générique](../../reporting-services/report-server/media/hlp-16file.gif "Icône Ressource générique")|Ressource générique|  
|![Icône Source de données partagée](../../reporting-services/report-data/media/hlp-16datasource.png "Icône Source de données partagée")|Source de données partagée|  
||Dataset partagé|  
  
 Les éléments avec lesquels vous travaillez ne peuvent pas tous être déplacés. Par exemple, il n'est pas possible de déplacer les éléments qui sont associés à un rapport, comme les abonnements ou l'historique de rapport. Ces éléments se déplacent avec leurs rapports associés. Il n'est pas non plus possible de déplacer des éléments, comme les planifications partagées, qui existent à l'extérieur de l'arborescence des dossiers. Vous ne pouvez pas déplacer des éléments si vous n'avez pas l'autorisation de le faire. L'autorisation pour déplacer un élément est transmise lorsque les tâches suivantes sont sélectionnées dans votre attribution de rôle pour l'élément considéré : « Gérer les rapports », « Gérer les modèles », « Gérer les dossiers » et « Gérer les sources de données ».  
  
#### <a name="to-move-an-item-from-within-the-contents-page"></a>Pour déplacer un élément à partir de la page Contenu  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
2.  Dans le Gestionnaire de rapports, accédez à la page **Contenu** , puis recherchez l’élément à déplacer.  
  
3.  Pointez sur l'élément et cliquez sur la flèche déroulante.  
  
4.  Dans le menu déroulant, cliquez sur **Déplacer**.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Dans **Emplacement**, spécifiez le dossier dans lequel placer l’élément. Vous pouvez taper le nom de dossier complet ou utiliser l'option de navigation dans l'arborescence pour accéder au dossier.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Vous pouvez aussi accéder à l’objet à déplacer, cliquer sur **Propriétés**, puis sur **Déplacer** en haut de la page.  
  
## <a name="delete-an-item"></a>Supprimer un élément  
 Avant de supprimer un élément, déterminez s'il est utilisé par d'autres éléments. Par exemple, si vous supprimez une source de données partagée, les rapports et les modèles qui l'utilisent ne pourront plus s'exécuter. Si vous supprimez un rapport, les abonnements et l'historique de rapport associés sont également supprimés. Pour rechercher les éléments dépendants d’un élément, consultez [Page Éléments dépendants &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/4dcfb311-e9c3-4c5d-b2e0-018d79f37d2e).  
  
#### <a name="to-delete-a-report-or-item"></a>Pour supprimer un rapport ou un élément  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
2.  Dans le Gestionnaire de rapports, accédez à la page **Contenu** , puis recherchez l’élément à supprimer.  
  
3.  Pointez sur l'élément et cliquez sur la flèche déroulante.  
  
4.  Dans le menu déroulant, cliquez sur **Supprimer**.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Page Contenu &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/6b16869b-158a-4934-9c85-bee934b35378)   
 [Recherche, affichage et gestion de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)  
  
  
