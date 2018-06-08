---
title: Octroi d’autorisations sur un serveur de rapports en mode natif | Microsoft Docs
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
- roles [Reporting Services], creating
- authorization [Reporting Services]
- server security [Reporting Services]
- role-based security [Reporting Services]
- items [Reporting Services], security
- permissions [Reporting Services], native mode
- published reports [Reporting Services], security
- reports [Reporting Services], security
- report items [Reporting Services], security
- role-based security [Reporting Services], about role-based security
- security [Reporting Services], role-based
ms.assetid: 260dc2e9-546c-4f04-9fa1-977e23c9d68c
caps.latest.revision: 60
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9c20c3f24a1a6698ef86446e0be9df4f91b6ba80
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="granting-permissions-on-a-native-mode-report-server"></a>Octroi d'autorisations sur un serveur de rapports en mode natif
  SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise l'autorisation basée sur les rôles et un sous-système d'authentification pour déterminer qui est habilité à effectuer des opérations et à accéder aux éléments d'un serveur de rapports. L'autorisation basée sur les rôles catégorise en rôles l'ensemble des actions qu'un utilisateur ou groupe peut effectuer. L'authentification repose sur l'authentification Windows intégrée ou sur un module d'authentification personnalisé que vous fournissez. Vous pouvez utiliser des rôles prédéfinis ou personnalisés avec chacun de ces types d'authentifications.  
  
## <a name="using-roles-to-grant-report-server-access"></a>Utilisation de rôles pour octroyer l'accès au serveur de rapports  
 Tous les utilisateurs interagissent avec un serveur de rapports dans le contexte d'un rôle qui définit un niveau spécifique d'accès. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] inclut des rôles prédéfinis que vous pouvez affecter aux utilisateurs et aux groupes pour fournir l’accès immédiat à un serveur de rapports. **Gestionnaire de contenu**, **Serveur de publication**et **Navigateur** sont des exemples de rôles prédéfinis. Chaque rôle définit un ensemble de tâches associées. Par exemple, un **serveur de publication** est autorisé à ajouter des rapports et à créer des dossiers pour stocker ces mêmes rapports.  
  
 Les attributions de rôles sont généralement héritées d'un nœud parent, mais vous pouvez rompre l'héritage d'autorisation en créant une attribution de rôles pour un élément particulier. Un utilisateur membre du rôle **Gestionnaire de contenu** pour un rapport peut être membre du rôle **Lecteur** pour un autre rapport.  
  
 Pour accorder l'accès aux éléments et aux opérations du serveur de rapports, suivez les instructions suivantes :  
  
1.  Examinez les rôles prédéfinis pour déterminer si vous pouvez les utiliser en l'état. Si vous devez ajuster les tâches ou définir des rôles supplémentaires, vous devez le faire avant de commencer à assigner des utilisateurs à des rôles spécifiques. Pour plus d’informations sur chaque rôle, consultez [Rôles prédéfinis](../../reporting-services/security/role-definitions-predefined-roles.md).  
  
2.  Identifiez les utilisateurs et les groupes qui doivent accéder au serveur de rapports, et à quel niveau. Le rôle **Lecteur** ou le rôle **Générateur de rapports** doit être attribué à la plupart des utilisateurs. Le rôle **Serveur de publication** doit être attribué à un nombre restreint d'utilisateurs. Le rôle **Gestionnaire de contenu**ne doit être attribué qu'à un nombre très limité d'utilisateurs.  
  
3.  Utilisez le Gestionnaire de rapports pour assigner des rôles sur le dossier de base (il s'agit du dossier à la racine de l'arborescence des dossiers du serveur de rapports) à chaque utilisateur ou groupe qui requiert l'accès.  
  
4.  Au niveau du site, dans la page Paramètres du site du Gestionnaire de rapports, créez une attribution de rôles au niveau système pour chaque utilisateur ou groupe en utilisant les rôles prédéfinis **Utilisateur système** et **Administrateur système**.  
  
5.  Créez autant d'attributions de rôles supplémentaires que nécessaire pour des dossiers, des rapports et d'autres éléments spécifiques. Évitez de créer un grand nombre d'attributions de rôles. Si vous en créez trop, il sera difficile de gérer les différents niveaux d'autorisation pour chaque utilisateur.  
  
> [!NOTE]  
>  Si vous avez configuré un serveur de rapports de telle sorte qu'il s'exécute en mode intégré SharePoint, vous devez définir des autorisations sur le site SharePoint de manière à accorder l'accès aux éléments du serveur de rapports. Pour plus d’informations, consultez [Accord d’autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md).  
  
## <a name="who-sets-permissions"></a>Qui définit les autorisations ?  
 Initialement, seuls les utilisateurs qui sont membres du groupe des administrateurs locaux peuvent accéder au serveur de rapports. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est installé avec deux attributions de rôles par défaut qui accordent un accès au niveau élément et au niveau système aux membres du groupe des administrateurs locaux. Ces attributions de rôles intégrées permettent aux Administrateurs locaux d'accorder l'accès au serveur de rapports à d'autres utilisateurs et de gérer les éléments du serveur de rapports. Les attributions de rôles intégrées ne peuvent pas être supprimées. Un administrateur local a toujours l'autorisation de gérer entièrement une instance de serveur de rapports.  
  
 Dans la mesure où les autorisations complètes sur un serveur de rapports incluent des autorisations au niveau élément et au niveau système, un administrateur local est assigné aux rôles suivants :  
  
 Une configuration supplémentaire est requise avant que vous puissiez administrer une instance du serveur de rapports sur un ordinateur local qui exécute Windows Vista ou Windows Server 2008. Pour plus d’informations, consultez [Configurer un serveur de rapports en mode natif pour l’administration locale &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
## <a name="how-permissions-are-stored"></a>Stockage des autorisations  
 Les attributions et les définitions de rôles sont stockées dans la base de données du serveur de rapports. Si vous utilisez de nombreux outils clients ou interfaces de programmation, tout accès est soumis aux autorisations définies pour l'instance du serveur de rapports dans son ensemble. Si vous configurez plusieurs serveurs de rapports au sein d'un déploiement évolutif, les attributions de rôles que vous définissez sur une instance sont stockées dans une base de données partagée et utilisées par toutes les autres instances du même déploiement évolutif. Dans la mesure où les attributions de rôles sont stockées avec les éléments qu'elles sécurisent, vous pouvez déplacer la base de données vers une autre instance du serveur de rapports sans perdre les autorisations définies.  
  
## <a name="tasks-and-tools-for-managing-permissions"></a>Tâches et outils de gestion des autorisations  
 Utilisez les outils suivants pour gérer les définitions et les attributions de rôles.  
  
|Outil|Tâches|  
|----------|-----------|  
|Management Studio – Permet d'afficher, modifier, créer et supprimer des définitions de rôles.|[Créer, supprimer ou modifier un rôle &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md)|  
|Gestionnaire de rapports – Permet d'assigner des utilisateurs et des groupes aux rôles.|[Accorder à un utilisateur l’accès à un serveur de rapports &#40;Gestionnaire de rapports&#41;](../../reporting-services/security/grant-user-access-to-a-report-server-report-manager.md)<br /><br /> [Modifier ou supprimer une affectation de rôle &#40;Gestionnaire de rapports&#41;](../../reporting-services/security/role-assignments-modify-or-delete.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Rôles prédéfinis](../../reporting-services/security/role-definitions-predefined-roles.md)   
 [Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md)   
 [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Créer et gérer des attributions de rôles](../../reporting-services/security/create-and-manage-role-assignments.md)   
 [Sécurité et protection de Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)   
 [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)  
  
  
