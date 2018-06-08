---
title: Éléments sécurisables | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- securable items [Reporting Services]
- roles [Reporting Services], securable items
- security [Reporting Services], securable items listed
- role-based security [Reporting Services], securable items
ms.assetid: 27f58d4c-5c7b-4947-af5b-0f1fa60faf5f
caps.latest.revision: 45
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: c6b95923b8ca5dfef2f6b98345ce914eeb11991e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="securable-items"></a>Éléments sécurisables
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise la sécurité basée sur les rôles pour contrôler l’accès aux éléments stockés sur un serveur de rapports. Lorsque vous accordez à un utilisateur l'accès à un serveur de rapports, vous le faites en général en créant une paire d'attributions de rôle :  
  
-   Au niveau du site  
  
-   Sur le dossier de base qui est le nœud racine de l'arborescence des dossiers du serveur de rapports  
  
 La sécurité est héritée au sein de la hiérarchie des dossiers du serveur de rapports. La création d'attributions de rôle au niveau du site et sur le dossier de base définit l'héritage des autorisations qui s'étend à tous les éléments et opérations sur un serveur de rapports.  
  
 Vous pouvez ignorer l'héritage des autorisations en définissant la sécurité des éléments individuels. Les éléments que vous pouvez sécuriser individuellement incluent :  
  
-   Dossiers  
  
-   Rapports  
  
-   Modèles de rapport  
  
-   Ressources  
  
-   Sources de données partagées  
  
-   Datasets partagés  
  
 D'autres constructions, telles que les planifications et les abonnements, ne sont pas explicitement sécurisées. Les planifications et les abonnements fonctionnent dans la sécurité d'un rapport.  
  
## <a name="item-descriptions"></a>Description des éléments  
 Le tableau suivant répertorie les éléments sécurisables et décrit leurs caractéristiques.  
  
|Élément|Caractéristiques|  
|----------|---------------------|  
|Dossiers|La sécurité de dossier s'applique au dossier lui-même et aux éléments qu'il contient. Le Dossier racine est le nœud racine de la hiérarchie de dossiers. La sécurité que vous définissez pour ce dossier établit les paramètres de sécurité initiaux pour tous les dossiers subordonnés, rapports, ressources et sources de données partagées qui se trouvent dans la hiérarchie de dossiers. Pour plus d’informations, consultez [Dossiers sécurisés](../../reporting-services/security/secure-folders.md).<br /><br /> Mes Rapports est un dossier particulier qui est sécurisé via une attribution de rôle impliquée basée sur un rôle dédié. Pour plus d’informations, consultez [Sécuriser Mes Rapports](../../reporting-services/security/secure-my-reports.md).|  
|Rapports|Les rapports et les rapports liés peuvent être sécurisés pour contrôler l'ensemble des actions pouvant être effectuées par les utilisateurs, telles que modifier les propriétés d'un rapport donné.<br /><br /> L'historique de rapport est sécurisé via le rapport qui contient l'historique. Vous ne pouvez pas sécuriser des instantanés individuels dans l'historique de rapport.<br /><br /> Pour plus d’informations sur la sécurité des rapports, consultez [Sécurisation des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md).|  
|Modèles de rapport|Vous pouvez spécifier une attribution de rôle sur tout ou une partie d'un modèle de rapport. Étant donné que les modèles de rapport peuvent être très étendus, vous pouvez sécuriser leurs éléments qui correspondent à des données confidentielles.|  
|Ressources|Les ressources peuvent être sécurisées pour contrôler l'accès à la ressource elle-même et à ses propriétés.<br /><br /> Seules les ressources autonomes peuvent être sécurisées comme des éléments distincts. Les ressources qui sont imbriquées dans un rapport ne peuvent pas être sécurisées séparément de ce rapport.<br /><br /> Pour plus d’informations sur la sécurité des ressources, consultez [Sécurisation des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md).|  
|Sources de données partagées|Vous pouvez sécuriser les sources de données partagées afin de limiter l'accès à l'élément et aux pages de ses propriétés. Pour plus d’informations, consultez [Sécuriser les éléments de source de données partagée](../../reporting-services/security/secure-shared-data-source-items.md).|  
|Datasets partagés|Les datasets partagés peuvent être sécurisés pour contrôler la plage d'actions que les utilisateurs peuvent effectuer, telles que l'affichage ou la modification de la définition, ou la modification des propriétés d'un dataset partagé donné.<br /><br /> Pour plus d’informations, consultez [Sécuriser les éléments de dataset partagés](../../reporting-services/security/secure-shared-dataset-items.md).|  
  
## <a name="see-also"></a> Voir aussi  
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [Créer, supprimer ou modifier un rôle &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md)   
 [Accorder à un utilisateur l’accès à un serveur de rapports &#40;Gestionnaire de rapports&#41;](../../reporting-services/security/grant-user-access-to-a-report-server-report-manager.md)   
 [Modifier ou supprimer une affectation de rôle &#40;Gestionnaire de rapports&#41;](../../reporting-services/security/role-assignments-modify-or-delete.md)  
  
  
