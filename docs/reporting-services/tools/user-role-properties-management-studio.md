---
title: Propriétés de rôle d’utilisateur (Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.userroleproperties.f1
ms.assetid: c8b22236-a8b1-4e15-b1ff-4e1909b602d3
caps.latest.revision: 27
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 5c000e40c7ec5633a24d7edb018a028774ca20a8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="user-role-properties-management-studio"></a>Propriétés de rôle d'utilisateur (Management Studio)
  Utilisez cette page pour afficher les tâches qui figurent dans une définition de rôle au niveau élément. Vous pouvez également utiliser cette page pour modifier la liste des tâches ou la description d'un rôle.  
  
 Une définition de rôle au niveau élément est une collection nommée de tâches que les utilisateurs effectuent sur un élément spécifique (c'est-à-dire un dossier, un rapport, une ressource ou une source de données partagée). Les définitions de rôles sont attribuées à un utilisateur ou à un groupe pour créer une attribution de rôle dans le Gestionnaire de rapports. Les tâches contenues dans la définition de rôle décrivent les opérations que peuvent effectuer l'utilisateur ou le groupe.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] contient plusieurs définitions de rôles au niveau élément prédéfinies que vous pouvez utiliser. Vous pouvez changer les définitions de rôles en modifiant la liste des tâches de chacune. La modification d'une définition de rôle affecte toutes les attributions de rôles qui contiennent la définition de rôle.  
  
> [!NOTE]  
>  Les attributions de rôles d'utilisateur sont utilisées uniquement sur un serveur de rapports qui s'exécute en mode natif. Si le serveur de rapports est configuré pour l'intégration SharePoint, cette page affiche des informations en lecture seule sur les rôles et niveaux d'autorisation définis sur le site SharePoint.  
  
## <a name="options"></a>Options  
 **Nom**  
 Spécifie le nom de la définition de rôle.  
  
 **Description**  
 Affiche une description de la définition du rôle. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cette description n'est visible que sur cette page. Dans le Gestionnaire de rapports, cette description aide les utilisateurs à décider s'il convient d'utiliser le rôle dans une attribution de rôle.  
  
 **Tâche**  
 Répertorie toutes les tâches au niveau élément qui peuvent être sélectionnées pour la définition de ce rôle. Vous pouvez ajouter ou supprimer des éléments dans la liste des tâches prédéfinies pour spécifier comment les utilisateurs accèdent à un élément donné par le biais de ce rôle. Vous ne pouvez ni créer de nouvelles tâches, ni modifier les tâches existantes. La liste des tâches d'une définition de rôle apparaît uniquement dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Description de la tâche**  
 Donne des informations sur chaque tâche. Vous ne pouvez pas modifier la description des tâches.  
  
## <a name="see-also"></a> Voir aussi  
 [Tâches au niveau élément](../../reporting-services/security/tasks-and-permissions-item-level-tasks.md)   
 [Définitions de rôles](../../reporting-services/security/role-definitions.md)   
 [Aide du serveur de rapports dans Management Studio accessible par la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Tâches et autorisations](../../reporting-services/security/tasks-and-permissions.md)   
 [Rôles prédéfinis](../../reporting-services/security/role-definitions-predefined-roles.md)  
  
  
