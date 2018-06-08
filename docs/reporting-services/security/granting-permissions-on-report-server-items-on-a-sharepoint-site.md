---
title: Accord d’autorisations sur des éléments de serveur de rapports sur un site SharePoint | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- permissions [Reporting Services], SharePoint integrated mode
- SharePoint integration [Reporting Services], permissions
- permissions [Reporting Services], native mode
- security [Reporting Services], SharePoint integrated mode
ms.assetid: 0eb2f34a-3643-4b03-81c2-5741ba7ebefd
caps.latest.revision: 13
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 5afd2bd25aa33ce35b719f1b32000342e47d3f20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="granting-permissions-on-report-server-items-on-a-sharepoint-site"></a>Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] fournit des fonctionnalités de sécurité intégrées qui vous permettent d’accéder aux éléments de serveur de rapports à partir des sites et des bibliothèques SharePoint. Si vous avez déjà affecté des autorisations aux utilisateurs, ces derniers auront accès aux éléments et opérations de serveur de rapport dès que vous aurez configuré les paramètres d'intégration entre [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] et un serveur de rapports. Les autorisations existantes vous permettent de télécharger des définitions de rapport et d'autres documents, d'afficher des rapports, de créer des abonnements et de gérer des éléments.  
  
 Si vous n'avez pas attribué d'autorisations ou si vous ne connaissez pas les fonctionnalités de sécurité dans [!INCLUDE[SPF2010](../../includes/spf2010-md.md)], suivez les instructions suivantes :  
  
1.  Dans la documentation du produit [!INCLUDE[SPF2010](../../includes/spf2010-md.md)], consultez les informations relatives aux paramètres de sécurité par défaut pour les groupes SharePoint standard pour savoir comment gérer les autorisations et l'accès des utilisateurs.  
  
2.  Passez en revue la liste des autorisations qui concernent spécifiquement l'accès aux éléments et aux opérations du serveur de rapports. Pour plus d’informations, consultez [Utiliser la sécurité intégrée dans Windows SharePoint Services pour les éléments de serveur de rapports](../../reporting-services/security/use-built-in-security-in-windows-sharepoint-services-for-report-server-items.md).  
  
3.  Attribuez des comptes d'utilisateur et de groupe à des groupes SharePoint prédéfinis.  
  
4.  Créez éventuellement d'autres groupes et niveaux d'autorisation, ou modifiez ceux qui existent déjà pour faire varier les autorisations d'accès au serveur selon les besoins spécifiques.  
  
 Pour utiliser les fonctionnalités de sécurité [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] avec des éléments de serveur de rapports, vous devez disposer d'un serveur de rapports qui s'exécute en mode intégré SharePoint.  
  
## <a name="about-permissions-permission-levels-and-sharepoint-groups"></a>À propos des autorisations, des niveaux d'autorisation et des groupes SharePoint  
 La liste suivante fournit une brève introduction aux fonctionnalités de sécurité dans [!INCLUDE[SPF2010](../../includes/spf2010-md.md)]. Pour plus d'informations, consultez « Aide et procédures Windows SharePoint » sur votre site SharePoint.  
  
-   Les objets sécurisables comprennent des sites, des listes, des bibliothèques, des dossiers et des documents.  
  
-   Une autorisation permet d'effectuer une tâche spécifique. [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] fournit 33 autorisations prédéfinies que vous pouvez associer dans un niveau d'autorisations.  
  
-   Un niveau d'autorisation est un ensemble d'autorisations qui peuvent être accordées à des utilisateurs ou à des groupes SharePoint sur un objet sécurisable tel qu'un site, une bibliothèque, une liste, un dossier, un élément ou un document. Il correspond à une définition de rôle dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Il existe cinq niveaux d'autorisation prédéfinis. Vous pouvez les personnaliser ou en créer de nouveaux si nécessaire.  
  
-   Un groupe SharePoint est un groupe d'utilisateurs que vous pouvez créer sur un site SharePoint pour gérer des autorisations au site et fournir une liste de distribution par messagerie pour les membres du site. Un groupe SharePoint comprend des comptes d'utilisateurs et de groupes Windows ou des connexions utilisateur si vous utilisez l'authentification par formulaire. [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] fournit trois groupes. Vous pouvez les personnaliser ou en créer de nouveaux si nécessaire.  
  
-   L'héritage des autorisations permet aux sous-sites, aux listes et aux bibliothèques ainsi qu'aux éléments d'hériter les paramètres de sécurité du site parent. Les autorisations héritées vous permettent d'accéder aux éléments de serveur de rapports stockés dans une bibliothèque SharePoint. L'utilisation de l'héritage des autorisations et des groupes SharePoint prédéfinis vous permet de simplifier votre déploiement et fournit un accès immédiat à la plupart des opérations de serveur de rapports.  
  
## <a name="who-sets-permissions"></a>Qui définit les autorisations ?  
 L'administrateur qui installe [!INCLUDE[SPF2010](../../includes/spf2010-md.md)], exécute l'Assistant Configuration SharePoint et crée le site portail, devient le propriétaire du site portail par défaut. Le propriétaire du site peut définir des autorisations dans l'administration centrale pour une ferme ou une application Web SharePoint autonome et peut définir des autorisations au niveau supérieur du site pour chaque application Web SharePoint. Cette personne peut aussi désigner d'autres propriétaires de site.  
  
 Au niveau supérieur du site d'une application Web SharePoint, les administrateurs de la collection de sites peuvent définir des autorisations pour plusieurs sites dans l'arborescence de sites. Les propriétaires de site individuels peuvent effectuer les mêmes tâches propres à un sous-site.  
  
 Un administrateur de serveur ou un administrateur de la collection de sites peut définir des options qui déterminent si d'autres propriétaires de site peuvent définir des autorisations. Selon votre niveau d'autorisations, vous pourrez ou non créer ou personnaliser des niveaux d'autorisation ou des groupes SharePoint.  
  
## <a name="using-predefined-sharepoint-groups-and-permission-levels"></a>Utilisation des niveaux d'autorisation et des groupes SharePoint prédéfinis  
 Dans la documentation produit [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] , il est recommandé de faire appel aux groupes standard SharePoint (qui comprennent *Nom de site* **Propriétaires**, *Nom de site* **Membres**, et *Nom de site* **Visiteurs**) et d’affecter les autorisations au niveau du site. La plupart des utilisateurs auxquels vous affectez des autouisations doivent appartenir aux groupes *Nom de site* **Visiteurs** ou *Nom de site* **Membres** . Les autorisations sur le site parent sont héritées sur l'ensemble de l'arborescence de site. Vous pouvez rompre l'héritage des autorisations pour des éléments spécifiques qui nécessitent des restrictions supplémentaires.  
  
 Ces groupes SharePoint possèdent les niveaux d'autorisation prédéfinis suivants :  
  
-   Le groupe **Propriétaires** possède les autorisations Contrôle total qui permettent aux membres du groupe d'apporter des modifications au contenu, aux pages ou aux fonctionnalités du site. L'accès Contrôle total doit être limité aux administrateurs de site uniquement.  
  
-   Le groupe **Membres** possède les autorisations de niveau Collaboration qui permettent aux membres du groupe d'afficher des pages, de modifier des éléments, de soumettre à l'approbation des modifications, d'ajouter et de supprimer des éléments dans une liste.  
  
-   Le groupe **Visiteurs** possède les autorisations de niveau Lecture qui permettent aux membres du groupe d'afficher des pages, des éléments de liste et des documents.  
  
 Les groupes SharePoint possèdent des niveaux d'autorisation qui fournissent un accès immédiat à un grand nombre d'opérations de serveur de rapports. Si vous estimez que les paramètres de sécurité intégrée n'apportent pas le niveau d'accès dont vous avez besoin, vous pouvez créer des groupes ou des niveaux d'autorisation personnalisés.  
  
 Pour plus d’informations sur les opérations de serveur de rapports prises en charge par les fonctionnalités de sécurité par défaut, consultez [Utiliser la sécurité intégrée dans Windows SharePoint Services pour les éléments de serveur de rapports](../../reporting-services/security/use-built-in-security-in-windows-sharepoint-services-for-report-server-items.md).  
  
 Pour faire appel aux fonctionnalités de sécurité intégrée, vous devez affecter des comptes d'utilisateur ou de groupes Windows aux groupes SharePoint. Hormis l'administrateur du serveur et le propriétaire du site portail qui ont un accès automatique à [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] lors de l'installation du logiciel, tous les autres utilisateurs doivent recevoir les autorisations pour accéder au serveur.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Utiliser la sécurité intégrée dans Windows SharePoint Services pour les éléments de serveur de rapports](../../reporting-services/security/use-built-in-security-in-windows-sharepoint-services-for-report-server-items.md)  
 Explique comment utiliser les groupes et les niveaux d'autorisation SharePoint prédéfinis pour accéder aux éléments de serveur de rapports.  
  
 [Référence autorisations de sites et de listes SharePoint pour des éléments de serveur de rapports](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md)  
 Fournit une référence de l'ensemble des autorisations de produit SharePoint permettant d'accéder aux opérations de serveur de rapports.  
  
 [Définir des autorisations pour les opérations de serveur de rapports dans une application Web SharePoint](../../reporting-services/security/set-permissions-for-report-server-operations-in-a-sharepoint-web-application.md)  
 Décrit les autorisations requises pour la génération d'états ad hoc et offre des stratégies de mise à disposition des fonctionnalités.  
  
 [Compare Roles and Tasks in Reporting Services to SharePoint Groups and Permissions](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md)  
 Fournit une brève comparaison des groupes SharePoint avec les définitions de rôles prédéfinis dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 [Définir les autorisations sur les éléments de serveur de rapports sur un site SharePoint &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/security/set-permissions-for-report-server-items-on-a-sharepoint-site.md)  
 Contient des instructions pour la création de nouveaux groupes SharePoint autorisés à démarrer le Générateur de rapports et à définir la sécurité des éléments d'un modèle. Cette rubrique contient aussi des instructions générales sur la définition d'autorisations personnalisées pour des opérations ou des éléments de serveur de rapports.  
  
## <a name="see-also"></a> Voir aussi  
 [Sécurité et protection de Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)  
  
  
