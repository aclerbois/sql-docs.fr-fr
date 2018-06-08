---
title: Serveur de rapports Reporting Services (mode natif) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- administering Reporting Services
- administering [Reporting Services]
- Reporting Services, administration
ms.assetid: fa0d84e2-4c21-432c-aa7c-23517da75253
caps.latest.revision: 24
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 09163d1990100b24b01305968fe7996476150f1e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-report-server-native-mode"></a>Serveur de rapports Reporting Services (mode natif)
  Un serveur de rapports configuré en mode natif s’exécute comme un serveur d’applications qui fournit toutes les fonctions de traitement et de gestion exclusivement par le biais de composants [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Vous pouvez utiliser soit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , soit le Gestionnaire de rapports pour gérer des rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Utilisez le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour gérer un serveur de rapports en mode natif.  
  
 Si le serveur de rapports est configuré pour le mode SharePoint, vous devez utiliser les pages de gestion du contenu du site SharePoint pour gérer les rapports, les sources de données partagées et les autres éléments du serveur de rapports.  
  
 Cette rubrique contient les informations suivantes :  
  
-   [Récapitulatif du mode natif](#bkmk_sum)  
  
-   [Gestion du contenu](#bkmk_managecontent)  
  
-   [Sécurisation et gestion d'une ressource](#bkmk_manageresources)  
  
-   [Référencement d'une ressource image à partir d'un rapport](#bkmk_referenceimage)  
  
##  <a name="bkmk_sum"></a> Récapitulatif du mode natif  
 Une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif comprend plusieurs fonctionnalités côté serveur à gérer et à entretenir. Les fonctionnalités du serveur incluent les suivantes :  
  
-   Le service web Report Server qui s'exécute au sein du service Report Server.  
  
-   Les applications de traitement en arrière-plan, qui gèrent les opérations planifiées et la remise des rapports.  
  
-   La base de données du serveur de rapports.  
  
 Pour administrer intégralement une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous devez disposer des autorisations suivantes :  
  
-   Membre du groupe Administrateurs local sur le serveur de rapports. Si votre installation comporte des fonctionnalités serveur exécutées sur des ordinateurs distants, vous devez disposer d'autorisations d'administrateur pour ces ordinateurs afin de gérer les serveurs sur une connexion distante.  
  
-   Autorisations d'administrateur de base de données pour l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chargée d'héberger la base de données.  
  
-   Si vous installez [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur un contrôleur de domaine, vous devez être administrateur de domaine.  
  
##  <a name="bkmk_managecontent"></a> Gestion du contenu  
 Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], la gestion de contenu fait référence à la gestion des rapports, des modèles, des dossiers, des ressources et des sources de données partagées. Tous ces éléments peuvent être gérés indépendamment les uns des autres via des propriétés et des paramètres de sécurité. Chaque élément peut être déplacé dans l'espace de noms de dossier du serveur de rapports. Pour gérer ces éléments de façon efficace, vous devez connaître les tâches effectuées par un gestionnaire de contenu.  
  
> [!NOTE]  
>  La gestion de contenu est différente de l'administration d'un serveur de rapports. Pour plus d’informations sur la gestion de l’environnement où un serveur de rapports s’exécute, consultez [Configuration et administration d’un serveur de rapports &#40;mode SharePoint de Reporting Services&#41;](../../reporting-services/report-server-sharepoint/configuration-and-administration-of-a-report-server.md).  
  
 La gestion de contenu inclut les tâches suivantes :  
  
-   Sécurisation du site et des éléments du serveur de rapports en appliquant la sécurité basée sur les rôles de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
-   Structuration de la hiérarchie des dossiers du serveur de rapports par l'ajout, la modification et la suppression de dossiers.  
  
-   Définition des valeurs par défaut et des propriétés qui s'appliquent aux éléments gérés par le serveur de rapports. Vous pouvez, par exemple, fixer des valeurs maximales de base qui déterminent les stratégies de stockage des historiques de rapport.  
  
-   Création d'éléments de sources de données partagées qui peuvent être utilisées à la place de connexions source de données spécifiques aux rapports. Un éditeur ou un gestionnaire de contenu peut sélectionner une source de données différente de celle initialement définie pour un rapport, par exemple, pour remplacer une référence à une base de données de test par une référence à une base de données de production.  
  
-   La création de planifications partagées peut être utilisée en remplacement des planifications spécifiques aux rapports et aux abonnements ; cela permet de simplifier la maintenance des informations de planification dans le temps.  
  
-   Création d'abonnements pilotés par des données qui génèrent des listes de destinataires par extraction de données d'une banque de données.  
  
-   Équilibrage des demandes de traitement de rapports adressées au serveur en planifiant le traitement des rapports, et en indiquant ceux qui peuvent être exécutés à la demande et ceux qui sont chargés à partir du cache.  
  
 L'autorisation d'effectuer des tâches de gestion est accordée via deux rôles prédéfinis : **Administrateur système** et **Gestionnaire de contenu**. Pour permettre une gestion efficace du contenu du serveur de rapports, ces deux rôles doivent vous être attribués. Pour plus d’informations sur ces rôles prédéfinis, consultez [Rôles et autorisations &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md).  
  
 Les outils de gestion du contenu d'un serveur de rapports sont [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou le gestionnaire de rapports. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] vous permet de définir des valeurs par défaut et d'activer des fonctionnalités. Le Gestionnaire de rapports permet d'accorder aux utilisateurs l'accès à des éléments et opérations du serveur de rapports, d'afficher et utiliser des rapports, ou d'autres types de contenu, ainsi que d'afficher et utiliser toutes les fonctionnalités relatives aux éléments partagés et à la distribution de rapports.  
  
##  <a name="bkmk_manageresources"></a> Sécurisation et gestion d'une ressource  
 Une ressource est un élément géré qui est stocké sur un serveur de rapports, mais qui n'est pas traité sur ce dernier. En règle générale, une ressource fournit du contenu externe aux utilisateurs des rapports. Il peut s'agir, par exemple, d'une image dans un fichier .jpg ou d'un fichier HTML qui décrit les règles d'entreprise utilisées dans un rapport. Le fichier JPG ou HTML est stocké sur le serveur de rapports ; toutefois, le serveur de rapports passe ce fichier directement au navigateur au lieu de le traiter en premier.  
  
 Pour ajouter une ressource à un serveur de rapports, vous devez télécharger ou publier un fichier :  
  
|Opération|Type de fichier|  
|---------------|---------------|  
|Télécharger|Tous les fichiers sont téléchargés en tant que ressources, sauf les fichiers de définitions de rapports (.rdl) et les fichiers de modèles de rapports (.smdl).<br /><br /> Pour télécharger une ressource, vous devez utiliser le Gestionnaire de rapports si le serveur de rapports s'exécute en mode natif, ou une page d'application sur un site SharePoint si le serveur s'exécute en mode intégré SharePoint. Pour plus d’informations, consultez [Charger un fichier ou un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/upload-a-file-or-report-report-manager.md) ou [Charger des documents vers une bibliothèque SharePoint &#40;Reporting Services en mode SharePoint&#41;](../../reporting-services/report-server-sharepoint/upload-documents-to-a-sharepoint-library-reporting-services-in-sharepoint-mode.md).|  
|Publier|Tous les fichiers d'un projet sont téléchargés en tant que ressources, sauf les fichiers de source de données .rdl, .smdl et .rds. Pour publier une ressource, ajoutez un élément existant à un projet dans le Concepteur de rapports, puis publiez le projet sur un serveur de rapports.|  
  
 Toutes les ressources ont pour origine des fichiers situés sur un système de fichiers. Ceux-ci sont ensuite téléchargés vers un serveur de rapports. À l'exception de la limitation de la taille de fichier par défaut à 4 mégaoctets, imposée par ASP.NET, il n'y a pas de restrictions sur les types de fichiers que vous pouvez télécharger. Cependant, lorsqu'ils sont publiés sur un serveur de rapports en tant que ressources, les types de fichiers ayant des types MIME équivalents offrent une utilisation plus optimale que d'autres. Par exemple, les ressources basées sur des fichiers HTML et JPG s'ouvrent dans une fenêtre de navigateur lorsque l'utilisateur clique sur la ressource choisie ; le fichier HTML est rendu sous forme de page Web et le fichier JPG sous forme d'image à l'intention de l'utilisateur. En revanche, les ressources qui ne disposent pas de types MIME équivalents, par exemple les fichiers d'application de bureau, risquent de ne pas être rendues dans la fenêtre du navigateur.  
  
 La visualisation ou non d'une ressource par les utilisateurs d'un rapport dépend des possibilités d'affichage du navigateur. Dans la mesure où les ressources ne sont pas traitées par le serveur de rapports, le navigateur doit fournir la fonctionnalité d'affichage qui permet d'obtenir le rendu d'un type MIME spécifique. Si le navigateur ne peut pas effectuer le rendu du contenu, les utilisateurs qui affichent la ressource ne voient que ses propriétés générales.  
  
 Les ressources coexistent avec les rapports, les sources de données partagées, les planifications partagées et les dossiers en tant qu'éléments nommés dans l'arborescence des dossiers du serveur de rapports. Vous pouvez rechercher, afficher, sécuriser et définir des propriétés sur les ressources à l'instar de n'importe quel autre élément stocké sur un serveur de rapports. Pour afficher ou gérer une ressource, vous devez disposer des tâches Afficher les ressources ou Gérer les ressources dans le rôle qui vous est attribué.  
  
##  <a name="bkmk_referenceimage"></a> Référencement d'une ressource image à partir d'un rapport  
 Les ressources peuvent contenir une image que vous référencez dans un rapport. Si les spécifications d'un rapport incluent l'utilisation d'images externes, prenez en considération les avantages suivants liés au stockage de l'image en tant que ressource :  
  
-   Stockage centralisé dans la base de données du serveur de rapports. Si vous déplacez la base de données du serveur de rapports et son contenu vers un autre ordinateur, l'image externe reste avec le rapport. Vous n'avez pas à effectuer le suivi des fichiers image stockés sur les disques de différents ordinateurs.  
  
-   Sécurisation via des attributions de rôles à la place de la sécurité du système de fichiers. Les mêmes autorisations utilisées pour afficher un rapport peuvent être appliquées à la ressource. En revanche, si vous stockez l'image sur disque, vous devez vous assurer que le compte d'utilisateur anonyme ou le compte d'exécution sans assistance est autorisé à accéder au fichier.  
  
 Pour utiliser une ressource de type image dans un rapport, ajoutez le fichier image au projet et publiez-le avec le rapport. Une fois l'image publiée, vous pouvez mettre à jour la référence de l'image dans le rapport, de sorte qu'elle pointe vers la ressource du serveur de rapports ; il vous suffit ensuite de publier à nouveau le rapport pour enregistrer vos modifications. Vous pouvez désormais mettre à jour l'image indépendamment du rapport en publiant à nouveau la ressource. Le rapport utilise la version la plus actuelle de l'image disponible sur le serveur de rapports.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer et administrer un serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [Résoudre les problèmes d’une installation de Reporting Services](../../reporting-services/install-windows/troubleshoot-a-reporting-services-installation.md)  
  
  
