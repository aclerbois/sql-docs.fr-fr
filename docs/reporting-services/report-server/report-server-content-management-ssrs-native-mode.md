---
title: Gestion du contenu du serveur de rapports (SSRS en mode natif) | Microsoft Docs
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
- administering Reporting Services
- published reports [Reporting Services], managing
- report servers [Reporting Services], content management
- content management [Reporting Services]
ms.assetid: 641961ac-53a5-4997-9d42-cf4ecce1f892
caps.latest.revision: 50
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9669a8356c4f20705bfb18e220d49f68fa93f0ee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-server-content-management-ssrs-native-mode"></a>Gestion du contenu du serveur de rapports (SSRS en mode natif)
  Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], la gestion de contenu fait référence à la gestion des éléments du serveur de rapports. Tous les éléments peuvent être gérés indépendamment les uns des autres via des propriétés et des paramètres de sécurité. Chaque élément peut être déplacé dans l'espace de noms de dossier du serveur de rapports. Pour gérer ces éléments de façon efficace, vous devez connaître les tâches effectuées par un gestionnaire de contenu. À compter de [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] CTP 3.2, le portail web  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est disponible. Cet article détaille le Gestionnaire de rapports et l’utilisation du nouveau portail web.  
  
> [!NOTE]  
>  La gestion de contenu est différente de l'administration d'un serveur de rapports. Pour plus d’informations sur la gestion de l’environnement d’exécution d’un serveur de rapports, consultez [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md).  
  
 La gestion de contenu inclut les tâches suivantes :  
  
-   Sécurisation du site de serveur de rapports et des éléments en appliquant la sécurité basée sur les rôles de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
-   Création de la hiérarchie des dossiers du serveur de rapports par l'ajout, la modification et la suppression de dossiers.  
  
-   Définition des valeurs par défaut et des propriétés qui s'appliquent aux éléments gérés par le serveur de rapports. Vous pouvez, par exemple, fixer des valeurs maximales de base qui déterminent les stratégies de stockage des historiques de rapport.  
  
-   Création d'éléments de sources de données partagées qui peuvent être utilisés à la place des connexions aux source de données spécifiques aux rapports. Un éditeur ou un gestionnaire de contenu peut sélectionner une source de données différente de celle initialement définie pour un rapport, par exemple, pour remplacer une référence à une base de données de test par une référence à une base de données de production.  
  
-   Création de planifications partagées qui peuvent être utilisées en remplacement des planifications spécifiques aux rapports et aux abonnements ; cela permet de simplifier la maintenance des informations de planification dans le temps.  
  
-   Création d’abonnements pilotés par des données qui génèrent des listes de destinataires par extraction de données d’une banque de données.  
  
-   Équilibrage des demandes de traitement de rapports adressées au serveur en planifiant le traitement des rapports, et en indiquant ceux qui peuvent être exécutés à la demande et ceux qui sont chargés à partir du cache.  
  
-   Octroi d’autorisations d’effectuer des tâches de gestion via deux rôles prédéfinis : **Administrateur système** et **Gestionnaire de contenu**. Pour permettre une gestion efficace du contenu du serveur de rapports, ces deux rôles doivent vous être attribués.  
  
 Les outils de gestion du contenu d’un serveur de rapports incluent [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], le Gestionnaire de rapports ou le portail web. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] vous permet de définir des valeurs par défaut et d'activer des fonctionnalités. Le Gestionnaire de rapports permet d'accorder aux utilisateurs l'accès à des éléments et opérations du serveur de rapports, d'afficher et utiliser des rapports, ou d'autres types de contenu, ainsi que d'afficher et utiliser toutes les fonctionnalités relatives aux éléments partagés et à la distribution de rapports. Le portail web est un site mis à jour qui autorise la plupart des fonctionnalités du Gestionnaire de rapports. Pour plus d’informations, consultez [Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md).  
  
##  <a name="bkmk_ReportServerItems"></a> Éléments du serveur de rapports  
 Les éléments du serveur de rapports incluent les rapports, les sources de données partagées, les datasets partagés, les parties d'un rapport, les ressources (éléments qui sont stockés dans un serveur de rapports mais pas traités par celui-ci) et les dossiers. Les éléments peuvent dépendre d'autres éléments, par exemple, un rapport peut dépendre des sources de données partagées qu'il référence. Si vous déplacez un élément dépendant, le serveur de rapports met à jour les informations de référence automatiquement.  
  
 Vous pouvez déplacer des éléments de serveur de rapports vers des emplacements de dossiers dans l'arborescence des dossiers du serveur de rapports. Lorsque vous déplacez un élément, toutes les propriétés (y compris les paramètres de sécurité) accompagnent l'élément vers son nouvel emplacement. Lorsque vous déplacez un dossier, tous les éléments qu'il contient l'accompagnent.  
  
> [!NOTE]  
>  Pour CTP 3.2, si vous souhaitez déplacer l’emplacement d’un élément, vous devez effectuer cette action dans le Gestionnaire de rapports. La possibilité de déplacer un élément dans le portail web n’est pas disponible.  
  
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
  
##  <a name="bkmk_Folders"></a> Dossiers  
 Une hiérarchie de dossiers est utilisée pour l'adressage des éléments stockés et gérés par un serveur de rapports.  Par défaut, la structure des dossiers comporte un nœud racine nommé Accueil et des dossiers réservés qui prennent en charge la fonctionnalité optionnelle Mes rapports. Les dossiers supplémentaires sont définis par l'utilisateur. Les dossiers du serveur de rapports sont utiles si vous souhaitez accorder le même niveau d'accès à plusieurs éléments. Les autorisations que vous définissez sur le dossier peuvent être héritées par les éléments dans le dossier et par les dossiers supplémentaires qui dépendent de ce dossier. Par exemple, vous pouvez créer un jeu de dossiers sous le dossier de base, affecter des autorisations d'équipe à chaque dossier, puis laisser les membres de l'équipe personnaliser les dossiers sous le dossier d'équipe, si nécessaire.  
  
 Si vous utilisez un navigateur pour ouvrir directement une session sur le serveur de rapports, le nœud racine de la structure de dossiers est le nom du répertoire virtuel du serveur de rapports. À partir du nœud racine, vous pouvez créer, modifier et supprimer des dossiers en fonction de vos besoins pour organiser le contenu d'un serveur de rapports. Vous pouvez ajouter du contenu à un dossier, déplacer des éléments entre les dossiers, modifier les noms ou les emplacements de dossiers et supprimer des dossiers qui ne sont plus nécessaires.  
  
 Les dossiers sont les conteneurs virtuels des éléments publiés auxquels vous accédez via le Gestionnaire de rapports ou une connexion de navigateur au serveur de rapports. Ni les dossiers, ni leur contenu n'existent réellement dans un système de fichiers. Au lieu de cela, ils sont stockés dans la base de données du serveur de rapports et sont accessibles par l'intermédiaire du point de terminaison du service Web Report Server. L'espace de noms de dossier du serveur de rapports représente une hiérarchie qui comprend un nœud racine, des dossiers prédéfinis ainsi que des dossiers définis par les utilisateurs. Cet espace de noms identifie de manière unique les éléments stockés sur un serveur de rapports. Il fournit un schéma d'adressage permettant de spécifier des éléments dans une URL. Lorsque vous sélectionnez ou localisez un rapport, le chemin d'accès au dossier est inclus dans l'URL de ce rapport.  
  
 La façon dont vous travaillez avec des dossiers dépend des tâches faisant partie de votre attribution de rôle. Si vous utilisez la sécurité par défaut, les utilisateurs disposant des rôles Gestionnaire de contenu et Serveur de publication peuvent créer et gérer des dossiers. Si vous utilisez des attributions de rôle personnalisées, l'attribution de rôle doit inclure les tâches prenant en charge la gestion de dossiers. Pour plus d’informations sur l’attribution des rôles et les tâches, consultez [Octroi d’autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md) et [Tâches et autorisations](../../reporting-services/security/tasks-and-permissions.md).  
  
 Les dossiers du serveur de rapports peuvent contenir les éléments suivants :  
  
-   Rapports  
  
-   Sources de données partagées  
  
-   Datasets partagés  
  
-   Parties de rapports  
  
-   Indicateurs de performance clés  
  
-   Rapports mobiles  
  
-   Ressources (éléments qui sont stockés sur un serveur de rapports, mais qui ne sont pas traités par ce serveur)  
  
-   Autres dossiers  
  
### <a name="reserved-folders"></a>Dossiers réservés  
 Les dossiers prédéfinis sont réservés par Reporting Services ; ils ne peuvent être déplacés, renommés ou supprimés. Ces dossiers représentent tous les dossiers créés par un utilisateur ou un administrateur du serveur de rapports ayant l'autorisation d'ajouter des éléments à un dossier.  
  
 Le tableau ci-dessous décrit les dossiers prédéfinis qui sont ancrés dans l'arborescence des dossiers et qui fournissent un environnement pour plusieurs fonctionnalités.  
  
|Dossier|Fonction|  
|------------|-------------|  
|Dossier de base|Nœud racine de l'arborescence des dossiers|  
|Utilisateurs|Ce dossier s'affiche lorsque vous activez la fonctionnalité Mes Rapports. Il contient les sous-dossiers de tous les utilisateurs qui utilisent la fonctionnalité Mes rapports ; l'accès à ces dossiers est limité exclusivement aux administrateurs du serveur de rapports. Chaque nom de sous-dossier correspond au nom de l'utilisateur.|  
|Mes rapports|Offre un espace de noms personnel pour chaque utilisateur.|  
  
### <a name="creating-folders"></a>Création de dossiers  
 Vous pouvez créer un dossier dans n'importe quel dossier disponible dans l'arborescence.  
  
 Si vous créez des dossiers en vue de restreindre l'accès à des rapports et des modèles spécifiques, spécifiez des attributions de rôles qui permettent aux utilisateurs de parcourir les dossiers parents situés dans le chemin d'accès, mais pas d'en afficher le contenu.  
  
### <a name="modifying-folder-properties"></a>Modification des propriétés d'un dossier  
 Après avoir créé un dossier, vous pouvez modifier ses propriétés pour le renommer, ajouter ou modifier sa description ou le déplacer vers un autre emplacement. Ces propriétés sont disponibles dans la page Propriétés générales du dossier. Pour plus d’informations sur la définition des propriétés accordant l’accès à un dossier, consultez [Dossiers sécurisés](../../reporting-services/security/secure-folders.md).  
  
### <a name="deleting-folders-and-folder-contents"></a>Suppression de dossiers et de contenus de dossiers  
 Lorsque vous supprimez un dossier, vous supprimez tous les éléments qu'il contient. Avant de supprimer un dossier, vous devez examiner son contenu afin de déterminer s'il contient des éléments qui peuvent faire l'objet de références ou être utilisés par des éléments d'une autre partie de la hiérarchie des dossiers. Les éléments référencés sont les définitions de rapports prenant en charge les rapports liés, les sources de données partagées et les ressources.  
  
 Si vous supprimez un rapport ayant un ou plusieurs rapports liés qui lui font référence, les rapports liés deviendront non valides une fois le rapport supprimé. Vous ne pouvez pas déterminer à l'avance les rapports liés qui seront affectés car un rapport ne conserve pas d'informations sur les rapports qui lui sont liés. Vous pouvez toutefois consulter les propriétés d'un rapport lié afin d'identifier le rapport sur lequel il est basé. En revanche, les éléments de sources de données partagées indiquent tous les rapports qui utilisent l'élément afin que vous puissiez déterminer aisément si les informations de connexion sont utilisées. Pour plus d’informations, consultez [Créer, modifier et supprimer des sources de données partagées &#40;SSRS&#41;](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md). Enfin, les ressources utilisées par les rapports ne permettent pas d'identifier ces rapports.  
  
 Avant de supprimer un dossier, déterminez si vous avez besoin de conserver l'historique de rapport des rapports susceptibles d'être supprimés ou les extensions de rapport (comme des abonnements pilotés par les données) faisant partie de rapports. Si vous avez besoin de ces informations, sortez l'élément du dossier avant de supprimer ce dossier.  
  
 Dans un dossier, la visibilité d'un élément dépend des attributions de rôles (autrement dit, des autorisations d'affichage d'un élément) et des options d'affichage définies actuellement pour le dossier. Dans le Gestionnaire de rapports, vous avez la possibilité de définir la page Contenu pour utiliser le mode Liste ou Détails. Dans certains cas, un élément de rapport peut se retrouver masqué dans l'affichage des listes. Prenez soin d'afficher un dossier en mode Détails avant de procéder à la suppression de son contenu.  
  
##  <a name="bkmk_Resources"></a> Ressources  
 Une ressource est un élément géré qui est stocké sur un serveur de rapports, mais qui n'est pas traité sur ce dernier. En règle générale, une ressource fournit du contenu externe aux utilisateurs des rapports. Il peut s'agir, par exemple, d'une image dans un fichier .jpg, d'un fichier de forme ESRI qui contient des données spatiales ou d'un fichier HTML qui décrit les règles d'entreprise utilisées dans un rapport. Le fichier JPG, SHP ou HTML est stocké sur le serveur de rapports ; toutefois, le serveur de rapports passe ce fichier directement au navigateur au lieu de le traiter en premier. Pour plus d’informations, consultez [Images &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/images-report-builder-and-ssrs.md) et la section « Ajout de données à une carte » dans [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md).  
  
### <a name="adding-and-viewing-a-resource"></a>Ajout et affichage d'une ressource  
 Pour ajouter une ressource à un serveur de rapports, vous devez télécharger ou publier un fichier :  
  
|Opération|Type de fichier|  
|---------------|---------------|  
|Télécharger|Pour télécharger une ressource, vous devez utiliser le Gestionnaire de rapports si le serveur de rapports s'exécute en mode natif, ou une page d'application sur un site SharePoint si le serveur s'exécute en mode intégré SharePoint. Pour plus d’informations, consultez [Charger un fichier ou un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/upload-a-file-or-report-report-manager.md) ou [Charger des documents vers une bibliothèque SharePoint &#40;Reporting Services en mode SharePoint&#41;](../../reporting-services/report-server-sharepoint/upload-documents-to-a-sharepoint-library-reporting-services-in-sharepoint-mode.md).|  
|Publier|Tous les fichiers d'un projet qui ne sont pas des rapports, des parties de rapport, des sources de données ou des datasets, sont téléchargés en tant que ressources. Pour publier une ressource, ajoutez un élément existant à un projet dans le Concepteur de rapports, puis publiez le projet sur un serveur de rapports.|  
  
 Toutes les ressources ont pour origine des fichiers situés sur un système de fichiers. Ceux-ci sont ensuite téléchargés vers un serveur de rapports. À l'exception de la limitation de la taille de fichier par défaut à 4 mégaoctets, imposée par ASP.NET, il n'y a pas de restrictions sur les types de fichiers que vous pouvez télécharger. Cependant, lorsqu'ils sont publiés sur un serveur de rapports en tant que ressources, les types de fichiers ayant des types MIME équivalents offrent une utilisation plus optimale que d'autres. Par exemple, les ressources basées sur des fichiers HTML et JPG s'ouvrent dans une fenêtre de navigateur lorsque l'utilisateur clique sur la ressource choisie ; le fichier HTML est rendu sous forme de page Web et le fichier JPG sous forme d'image à l'intention de l'utilisateur. En revanche, les ressources qui ne disposent pas de types MIME équivalents, par exemple les fichiers d'application de bureau, risquent de ne pas être rendues dans la fenêtre du navigateur.  
  
 La visualisation ou non d'une ressource par les utilisateurs d'un rapport dépend des possibilités d'affichage du navigateur. Dans la mesure où les ressources ne sont pas traitées par le serveur de rapports, le navigateur doit fournir la fonctionnalité d'affichage qui permet d'obtenir le rendu d'un type MIME spécifique. Si le navigateur ne peut pas effectuer le rendu du contenu, les utilisateurs qui affichent la ressource ne voient que ses propriétés générales.  
  
### <a name="securing-and-managing-a-resource"></a>Sécurisation et gestion d'une ressource  
 Les ressources coexistent avec les rapports, les sources de données partagées, les planifications partagées et les dossiers en tant qu'éléments nommés dans l'arborescence des dossiers du serveur de rapports. Vous pouvez rechercher, afficher, sécuriser et définir des propriétés sur les ressources à l'instar de n'importe quel autre élément stocké sur un serveur de rapports. Pour afficher ou gérer une ressource, vous devez disposer des tâches Afficher les ressources ou Gérer les ressources dans le rôle qui vous est attribué.  
  
### <a name="referencing-an-image-resource-from-a-report"></a>Référencement d'une ressource image à partir d'un rapport  
 Les ressources peuvent contenir une image que vous référencez dans un rapport. Si les spécifications d'un rapport incluent l'utilisation d'images externes, prenez en considération les avantages suivants liés au stockage de l'image en tant que ressource :  
  
-   Stockage centralisé dans la base de données du serveur de rapports. Si vous déplacez la base de données du serveur de rapports et son contenu vers un autre ordinateur, l'image externe reste avec le rapport. Vous n'avez pas à effectuer le suivi des fichiers image stockés sur les disques de différents ordinateurs.  
  
-   Sécurisation via des attributions de rôles à la place de la sécurité du système de fichiers. Les mêmes autorisations utilisées pour afficher un rapport peuvent être appliquées à la ressource. En revanche, si vous stockez l'image sur disque, vous devez vous assurer que le compte d'utilisateur anonyme ou le compte d'exécution sans assistance est autorisé à accéder au fichier.  
  
 Pour utiliser une ressource de type image dans un rapport, ajoutez le fichier image au projet et publiez-le avec le rapport. Une fois l'image publiée, vous pouvez mettre à jour la référence de l'image dans le rapport, de sorte qu'elle pointe vers la ressource du serveur de rapports ; il vous suffit ensuite de publier à nouveau le rapport pour enregistrer vos modifications. Vous pouvez désormais mettre à jour l'image indépendamment du rapport en publiant à nouveau la ressource. Le rapport utilise la version la plus actuelle de l'image disponible sur le serveur de rapports.  
  
 Pour plus d’informations, consultez [Mise à jour d’une ressource &#40;Gestionnaire de rapports&#41;](../../reporting-services/report-server/update-a-resource-report-manager.md).  
  
##  <a name="bkmk_MyReports"></a> Mes rapports  
 Le dossier Mes Rapports est un espace de travail personnel pour chaque utilisateur qui ouvre une session sur un serveur de rapports avec un compte de domaine valide. Ce dossier à usage spécial assure le stockage des rapports en cours d'élaboration, des rapports qui ne sont pas destinés à une large distribution ou des rapports qui ont été modifiés pour un besoin particulier. Vous ne pouvez pas restreindre le nombre ou la taille des éléments qui sont stockés dans un dossier Mes Rapports, ni configurer le dossier Mes Rapports pour qu'il soit partagé entre plusieurs utilisateurs.  
  
 Techniquement, Mes Rapports associe le nom d'un dossier virtuel que voit chaque utilisateur (Mes Rapports) à un dossier principal Dossiers des utilisateurs et à un sous-dossier unique basé sur le nom de l'utilisateur. Lorsqu'un utilisateur accède à son dossier Mes Rapports, il est en réalité redirigé vers son sous-dossier dans Dossiers des utilisateurs. Chaque sous-dossier assure le stockage des rapports et des éléments que l'utilisateur ajoute à son dossier Mes Rapports. Dans le portail web, vous ne verrez pas Mes rapports au niveau racine. Vous devez accéder au dossier Utilisateurs.  
  
 Le dossier Dossiers des utilisateurs est créé lors de l'installation du serveur de rapports. Les sous-dossiers d'utilisateurs sont ensuite créés lorsqu'un utilisateur ouvre Mes Rapports pour la première fois (par exemple, en cliquant sur Mes Rapports dans le Gestionnaire de rapports). Chaque nom de dossier respecte le format suivant :  
  
```  
/Users Folders/<username>/My Reports  
```  
  
 Des dossiers ne sont alloués qu'aux utilisateurs dotés de comptes système valides. Si un nom d'utilisateur contient des caractères spéciaux, il est créé avec les caractères d'échappement équivalents. Les caractères d'échappement équivalents sont indiqués dans le tableau suivant.  
  
|Caractère|Valeur d'échappement| Exemple|  
|---------------|------------------|-------------|  
|(espace)|[ ]|*Prénom Nom* devient *Prénom[ ]Nom*|  
|\ (barre oblique inverse)|Remplacé par une espace|*NomDomaine\nom_utilisateur* devient *NomDomaine nom_utilisateur*|  
|@ (symbole at)|[at]|*nom_utilisateur*@hotmail.com devient *nom_utilisateur*[at]hotmail.com|  
|& (esperluette)|[amp]|*nom_utilisateur*@*société*&*société.com* devient *nom_utilisateur*[at]*société*[amp]*société.com*|  
|$ (signe $)|[dollar]|*Nom* $*Utilisateur* devient *Nom*[ ][dollar]*Utilisateur*|  
  
 La fonctionnalité Mes Rapports est facultative. Lorsque vous installez un serveur de rapports, la fonctionnalité Mes Rapports est désactivée par défaut. Pour plus d’informations sur l’activation de cette fonctionnalité, consultez [Activer et désactiver Mes rapports](../../reporting-services/report-server/enable-and-disable-my-reports.md). Pour plus d’informations, consultez [Sécuriser Mes Rapports](../../reporting-services/security/secure-my-reports.md).  
  
## <a name="tasks"></a>Tâches  
 [Télécharger des fichiers dans un dossier](../../reporting-services/report-server/upload-files-to-a-folder.md)  
  
 [Création, suppression ou modification d’un dossier &#40;Gestionnaire de rapports&#41;](../../reporting-services/report-server/create-delete-or-modify-a-folder-report-manager.md)  
  
 [Mise à jour d’une ressource &#40;Gestionnaire de rapports&#41;](../../reporting-services/report-server/update-a-resource-report-manager.md)  
  
 [Télécharger des fichiers dans un dossier](../../reporting-services/report-server/upload-files-to-a-folder.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md)   
 [Rôles et autorisations &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md)   
 [Rapports Reporting Services &#40;SSRS&#41;](../../reporting-services/reports/reporting-services-reports-ssrs.md)  
  
  
