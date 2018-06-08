---
title: Concepts de Reporting Services (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 10/25/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 934b199c-9918-4e6b-83f4-5862b94fc904
caps.latest.revision: 37
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: edbf80dce6c749551d8955e52aa1c09794dc0cd8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-concepts-ssrs"></a>Concepts de Reporting Services (SSRS)
  Cette rubrique fournit un bref résumé des concepts de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .  
  
 **[!INCLUDE[applies](../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] | Mode SharePoint [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]    
  
##  <a name="bkmk_ReportServerConcepts"></a> Concepts du serveur de rapports  
 Un serveur de rapports est un ordinateur sur lequel une instance de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] est installée. Un serveur de rapports stocke en interne des éléments tels que des rapports paginés et mobiles, des éléments liés aux rapports et des ressources, des planifications et des abonnements. Un serveur de rapports peut être configuré en tant qu'unique serveur autonome ou comme batterie évolutive, ou il peut être intégré au serveur SharePoint. Vous interagissez avec des éléments de serveur de rapports par le service Web [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] , le fournisseur WMI, l'accès URL ou par programmation via des scripts. La façon dont vous interagissez avec un serveur de rapports dépend de la topologie de déploiement et de la configuration.  
  
### <a name="native-mode-report-servers"></a>Serveurs de rapports en mode natif
 Un serveur de rapports configuré en mode natif est un ordinateur qui a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] installé et configuré comme serveur autonome. Vous interagissez avec le serveur de rapports, les rapports et les éléments liés aux rapports à l’aide d’un navigateur avec le [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] ou les commandes d’accès URL, SQL Server Management Studio ou par programmation via des scripts. Pour plus d’informations, consultez [Serveur de rapports Reporting Services &#40;mode natif&#41;](../reporting-services/report-server/reporting-services-report-server-native-mode.md).  
  
### <a name="sharepoint-mode-report-servers"></a>Serveurs de rapports en mode SharePoint  
 Un serveur de rapports intégré à SharePoint a deux configurations possibles. Dans [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] est installé avec le serveur SharePoint en tant que service partagé SharePoint. Dans les versions antérieures, le serveur de rapports est intégré dans le serveur SharePoint quand vous installez le complément de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint. Dans les deux cas, vous interagissez avec le serveur de rapports, les rapports et les éléments liés aux rapports à l'aide des pages d'applications sur le site SharePoint. Pour stocker les types de contenu liés aux rapports, utilisez la bibliothèque de documents SharePoint et d'autres bibliothèques que vous créez. Pour plus d’informations, consultez [Serveur de rapports Reporting Services &#40;mode SharePoint&#41;](../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md).  
  
### <a name="report-server-items"></a>Éléments du serveur de rapports 
 Les éléments du serveur de rapports incluent des rapports paginés et mobiles, des indicateurs de performance clé, des sources de données partagées, des datasets partagés et d'autres éléments que vous pouvez publier, télécharger ou enregistrer sur un serveur de rapports. Organisez les éléments dans la structure hiérarchique des dossiers du serveur de rapports sur un serveur de rapports natif ou dans les bibliothèques de contenu SharePoint sur un site SharePoint. Pour plus d’informations, consultez [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md).  
  
### <a name="folders"></a>Dossiers
 Sur un serveur de rapports natif, les dossiers fournissent la structure hiérarchique de navigation et le chemin d'accès de tous les éléments adressables qui sont stockés dans un serveur de rapports. Pour contrôler l’accès aux éléments du serveur de rapports, appelé *sécurité au niveau élément*, utilisez l’arborescence des dossiers et les autorisations d’accès au dossier et au site. Par défaut, les attributions de rôles que vous définissez pour des dossiers spécifiques sont héritées par des dossiers enfants dans l'arborescence des dossiers. Si vous attribuez des rôles spécifiques à un dossier, les règles d'héritage ne sont plus applicables. La structure des dossiers comporte un nœud racine nommé **Accueil**et des dossiers réservés qui prennent en charge la fonctionnalité optionnelle **Mes rapports** . Dans un navigateur, le nœud racine est le nom du répertoire virtuel du serveur de rapports, par exemple, `http://myreportserver/reports`. Pour plus d'informations, consultez [Folders](../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md#bkmk_Folders).  
  
 Pour organiser les éléments sur un site SharePoint, utilisez les dossiers SharePoint des bibliothèques de documents et des bibliothèques de contenu.  
  
### <a name="roles-and-permissions"></a>Rôles et autorisations
 Sur un serveur de rapports natif, l'administrateur système du serveur de rapports gère les autorisations d'accès, configure le serveur de rapports de façon à traiter les demandes de rapports, à conserver des historiques d'instantanés et à gérer les autorisations pour les rapports, les sources de données, les datasets et abonnements. Par exemple, la sécurité d’un rapport publié est assurée au moyen des attributions de rôles à l’aide du modèle de sécurité basée sur les rôles de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Rôles et autorisations &#40;Reporting Services&#41;](../reporting-services/security/roles-and-permissions-reporting-services.md).  
  
 Sur un site SharePoint, utilisez la page d'administrateurs de site SharePoint pour gérer les autorisations d'accès sur les rapports et le contenu des sites en lien avec les rapports.  
  
### <a name="schedules"></a>Planifications
 Sur un serveur de rapports natif, il est possible de planifier des rapports paginés, des datasets partagés et des abonnements, afin de récupérer des données et de remettre des rapports et des requêtes de datasets à des heures déterminées ou pendant des périodes de faible activité. Les planifications peuvent être exécutées une seule fois ou sur une base périodique à des heures, des jours, des semaines ou des mois déterminés. Pour plus d'informations, consultez [Schedules](../reporting-services/subscriptions/schedules.md).  
  
### <a name="subscriptions-and-delivery"></a>Abonnements et remises  
 Un abonnement est une requête permanente de remise d'un rapport à une heure donnée ou en réponse à un événement, et dans un format de fichier d'application que vous définissez dans l'abonnement. Les abonnements offrent une alternative à l'exécution d'un rapport à la demande. La génération de rapports à la demande nécessite que vous sélectionniez le rapport chaque fois que vous souhaitez le consulter. En revanche, les abonnements peuvent être utilisés pour planifier et pour automatiser la remise d'un rapport. La remise s'effectue dans une boîte de réception de messagerie électronique ou dans un partage de fichiers. Pour plus d’informations, consultez [Abonnements et remise &#40;Reporting Services&#41;](../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
### <a name="extensions"></a>Extensions
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] fournit une architecture extensible que vous pouvez utiliser pour personnaliser des solutions de rapport. Le serveur de rapports prend en charge les extensions d'authentification personnalisées, les extensions pour le traitement des données, les extensions pour le traitement des rapports, les extensions de rendu et les extensions de remise. De plus, les extensions qui sont à la disposition des utilisateurs sont configurables dans le fichier de configuration RSReportServer.config. Par exemple, vous pouvez limiter les formats d'exportation que la visionneuse de rapport est autorisée à utiliser. Les extensions de remise et de traitement des rapports sont facultatives, mais nécessaires si vous voulez prendre en charge la diffusion des rapports ou les contrôles personnalisés. Pour plus d’informations, consultez [Extensions Reporting Services &#40;SSRS&#41;](../reporting-services/extensions-ssrs.md).  
  
### <a name="report-access"></a>Accès aux rapports 
 L'accès à la demande permet aux utilisateurs de sélectionner des rapports à partir d'un outil d'affichage de rapports. En fonction de la configuration du serveur de rapports, il est possible d’utiliser le [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], un composant WebPart SharePoint 2.0 [!INCLUDE[msCoName](../includes/msconame-md.md)] , une bibliothèque SharePoint quand [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] est installé en mode intégré SharePoint, un contrôle ReportViewer incorporé ou un navigateur utilisant un accès URL. Pour plus d’informations sur l’accès aux rapports à la demande, consultez [Recherche, affichage et gestion des rapports &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md).  
  
 Les abonnements offrent une alternative à l'exécution d'un rapport à la demande. Pour plus d’informations, consultez [Abonnements et remise &#40;Reporting Services&#41;](../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
 Pour connaître la liste des outils à utiliser pour interagir avec le serveur de rapports, consultez [Outils de Reporting Services](../reporting-services/tools/reporting-services-tools.md).  
  
  
##  <a name="bkmk_ReportsandRelatedItemConcepts"></a> Rapports et concepts des éléments associés  
### <a name="reports-and-report-definitions"></a>Rapports et définitions de rapport


 **RDL** 
 
 Une définition de rapport est un fichier XML qui respecte une grammaire XML nommée RDL (Report Definition Language). Dans [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], vous créez une définition de rapport à l'aide d'un outil tel que le Générateur de rapports ou le Concepteur de rapports. Elle inclut des éléments qui définissent les connexions aux sources de données, les requêtes destinées à récupérer des données, des expressions, des paramètres, des images, des zones de texte et des tableaux, ainsi que de tous les autres éléments de mise en page utilisés lors de la création. Pour plus d’informations, consultez [Langage de définition de rapport &#40;SSRS, Report Definition Language&#41;](../reporting-services/reports/report-definition-language-ssrs.md).  
  
 **RSMOBILE**

Vous créez des rapports Reporting Services mobiles (fichiers .rsmobile) dans l’éditeur de rapports mobiles SQL Server. Ils sont optimisés pour les appareils mobiles et connectés à des données locales, avec un large éventail de visualisations de données. En savoir plus sur les [rapports mobiles Reporting Services](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md). 
  
 **RDLC** 
 
 Le Concepteur de rapports Visual Studio produit des fichiers de définition de rapport client (.rdlc) au format XML à utiliser avec le contrôle ReportViewer.  
  
### <a name="report-data-connections-and-data-sources"></a>Connexions et sources de données de rapport 
 Les rapports utilisent des connexions de données pour récupérer les données nécessaires lorsqu'une requête s'exécute ou lorsque le rapport est traité. Dans une définition de rapport, la connexion de données est identique à la source de données. Dans la liste correspondante, choisissez un type de connexion de données intégré pour vous connecter à une base de données relationnelle, une base de données multidimensionnelle, un service Web ou toute autre source de données. Les termes suivants sont utilisés lors de la description des connexions de données.  
  
-   **Connexion de données.** Également appelée *Source de données*. Une connexion de données inclut un nom et des propriétés de connexion qui dépendent du type de connexion. Par défaut, une connexion de données n'inclut pas d'informations d'identification. Une connexion de données ne spécifie pas les données à récupérer à partir de la source de données externe. Pour ce faire, vous devez spécifier une requête lorsque vous créez un dataset.  
  
-   **Définition de source de données.** Un fichier qui contient la représentation XML d'une source de données de rapport. Lorsqu'un rapport est publié, ses sources de données sont enregistrées sur le serveur de rapports ou le site SharePoint en tant que définitions de source de données, indépendamment de la définition de rapport. Par exemple, un administrateur de serveur de rapports peut mettre à jour la chaîne de connexion ou les informations d'identification. Sur un serveur de rapports natif, le type de fichier est .rds. Sur un site SharePoint, le type de fichier est .rsds.  
  
-   **Chaîne de connexion.** Une chaîne de connexion est une version de chaîne des propriétés de connexion nécessaires à la connexion à une source de données. Les propriétés de connexion diffèrent selon le type de connexion de données.  
  
-   **Source de données partagée.** Une source de données disponible sur un serveur de rapports ou un site SharePoint, et qui est utilisable par plusieurs rapports.  
  
     Les sources de données partagées sont utiles lorsque vous disposez de sources de données que vous utilisez souvent. Il est recommandé d'utiliser des sources de données partagées dans la mesure du possible. Celles-ci permettent de gérer plus facilement les rapports et l'accès aux rapports, et de sécuriser davantage les rapports et les sources de données auxquelles ils accèdent. Si vous avez besoin d'une source de données partagée, demandez à votre administrateur système d'en créer une pour vous.  
  
     Dans le Générateur de rapports, vous ne pouvez pas créer de source de données partagée. Vous pouvez rechercher et sélectionner une source de données partagée à partir du serveur de rapports.  
  
     Dans le Concepteur de rapports, vous ne pouvez pas rechercher une source de données partagée située sur le serveur de rapports. Vous pouvez créer des sources de données partagées dans le cadre d'un projet au sein de l'Explorateur de solutions, puis déterminer s'il convient de les déployer sur un serveur de rapports. Vous pouvez choisir de les utiliser localement uniquement, en raison des différences en matière d'informations d'identification requises sur votre ordinateur ou le serveur de rapports.  
  
-   **Source de données incorporée.** Également appelée *source de données spécifique aux rapports*, une source de données incorporée est définie dans un rapport et utilisée uniquement par ce rapport.  
  
     Une source de données incorporée est une connexion de données enregistrée dans la définition de rapport. Les informations de connexion à la source de données incorporée peuvent être utilisées uniquement par le rapport dans lequel elles sont incorporées.  
  
-   **Informations d'identification.** Les informations d'identification sont des informations d'authentification qui doivent être fournies pour vous permettre d'accéder à des données externes.  
  
     Les informations d'identification sont utilisées pour créer une source de données incorporée, exécuter une requête ou récupérer des données lors du traitement d'un rapport. Le propriétaire de la source de données détermine le type d'informations d'identification à utiliser pour accéder aux données. Les informations d'identification sont gérées indépendamment de la connexion de données sur un serveur de rapports, un site SharePoint ou un ordinateur local, au sein d'un environnement de création de rapports. Selon le type de source de données, les informations d'identification peuvent être enregistrées à des fins d'automatisation, ou définies pour être demandées à chaque utilisateur. Les informations d'identification nécessaires peuvent différer selon que vous vous connectez à la source de données à partir de votre ordinateur ou à partir du serveur de rapports. Pour plus d’informations, consultez [Spécifier des informations d’identification dans le Générateur de rapports](http://msdn.microsoft.com/library/7412ce68-aece-41c0-8c37-76a0e54b6b53).  
  
### <a name="report-datasets"></a>Datasets de rapport 
 Dans un rapport, un dataset représente des données de rapport retournées comme résultat de l'exécution d'une requête sur une source de données externe. Le dataset dépend de la connexion de données qui contient des informations sur la source de données externe. Les données elles-mêmes ne sont pas intégrées dans la définition de rapport. Un dataset contient une commande de requête, une collection de champs, des paramètres, des filtres et des options de données incluant notamment le respect de la casse et le classement. Il existe deux types de datasets :  
  
-   **Datasets partagés.** Un dataset partagé est publié sur un serveur de rapports et peut être utilisé par plusieurs rapports. Un dataset partagé doit être basé sur une source de données partagée. Un dataset partagé peut être mis en cache et planifié en créant un plan d'actualisation du cache.  
  
-   **Datasets incorporés.** Les datasets incorporés sont définis dans un rapport unique et sont utilisés par un seul rapport.  
  
 Pour plus d’informations, consultez [Datasets incorporés dans les rapports et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  
  
### <a name="report-parameters"></a>Paramètres du rapport 
 Les paramètres de rapport font partie d'une définition de rapport. Vous pouvez ajouter des paramètres à des rapports Reporting Services paginés et mobiles pour lier des rapports connexes, pour contrôler l'apparence d'un rapport, pour filtrer les données du rapport, ou pour limiter l'étendue d'un rapport à des utilisateurs ou des emplacements spécifiques. Lorsqu'un rapport paginé est publié sur un serveur de rapports ou un site SharePoint natif, les paramètres de rapport sont enregistrés en tant qu'élément distinct du serveur de rapports. Les paramètres peuvent être gérés indépendamment de la définition de rapport. Pour créer plusieurs jeux de paramètres pour le même rapport, créez des *rapports liés*.  
  
### <a name="report-items"></a>Éléments de rapport 
 Un élément de rapport est un concept interne mais basique dans une définition de rapport Reporting Services paginé. Les propriétés d'un élément de rapport s'appliquent aux régions de données, aux cartes, aux zones de texte, aux images, ainsi que les autres éléments de conception que vous ajoutez à un rapport. La compréhension des propriétés d'un élément de rapport peut vous aider à concevoir un contenu et une apparence de rapport personnalisés. Par exemple, tous les éléments de rapport ont une propriété hidden pour contrôler la visibilité.  
  
### <a name="data-regions-and-maps"></a>Régions de données et cartes 
 Une région de données est un élément de mise en page qui affiche les données d'un seul dataset dans un rapport Reporting Services paginé. Les types de régions de données incluent le tableau matriciel, le graphique, la jauge, et l'indicateur. La carte est un type spécial de région de données car elle peut afficher des données de deux datasets : un contenant des données spatiales et un qui contient des données analytiques.  
  
 Utilisez les régions de données pour activer des visualisations de données courantes : nombres et texte dans une table, une matrice ou une liste ; représentations graphiques dans un graphique ou une jauge ; affichages géographiques sur une carte. Les tables, matrices et listes sont toutes basées sur la région de données du tableau matriciel, lequel peut se développer autant que nécessaire pour afficher toutes les données du dataset. Une région de données de tableau matriciel prend en charge plusieurs groupes de lignes et de colonnes statiques et dynamiques. Un graphique affiche plusieurs séries et catégories de groupes sous divers formats graphiques. Une jauge affiche une valeur unique ou une valeur agrégée pour un dataset. Une carte affiche les données spatiales en tant qu'éléments cartographiques dont l'apparence peut varier selon les données agrégées d'un dataset.  
  
-   **Table.** Une table est une région de données qui présente les données ligne par ligne. Les colonnes de table sont statiques : vous déterminez le nombre de colonnes lorsque vous concevez votre rapport. Les lignes de table sont dynamiques : elles s'étendent vers le bas pour contenir les données. Vous pouvez ajouter aux tables des groupes, qui organisent les données par champs ou expressions sélectionnés. Pour plus d’informations, consultez [Tables, matrices et listes (Générateur de rapports et SSRS)](http://msdn.microsoft.com/en-us/9dcf3fc8-bf9c-4a14-a03d-e78254aa4098).  
  
-   **Matrice.** Une matrice est également connue sous le nom d'analyse croisée. Une région de données de type matrice contient à la fois des colonnes et des lignes dynamiques : elles s'étendent pour contenir les données. Une matrice peut posséder des lignes et des colonnes dynamiques, ainsi que des lignes et des colonnes statiques. Les colonnes ou les lignes peuvent contenir d'autres colonnes ou lignes ; en outre, elles peuvent être utilisées pour regrouper des données. Pour plus d’informations, consultez [Tables, matrices et listes (Générateur de rapports et SSRS)](http://msdn.microsoft.com/en-us/9dcf3fc8-bf9c-4a14-a03d-e78254aa4098).  
  
-   **Liste.** Une liste est une région de données qui présente les données selon une disposition libre. Vous pouvez organiser les éléments de rapport de façon à créer un formulaire avec des zones de texte, des images et d'autres régions de données placées aux emplacements de votre choix dans la liste. Pour plus d’informations, consultez [Tables, matrices et listes (Générateur de rapports et SSRS)](http://msdn.microsoft.com/en-us/9dcf3fc8-bf9c-4a14-a03d-e78254aa4098).  
  
-   **Graphique.** Un graphique présente les données graphiquement. Les exemples de graphiques courants sont les graphiques à barres, à secteurs et en courbes, mais de nombreux autres styles de graphiques sont pris en charge. Pour plus d’informations, consultez [Graphiques &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/charts-report-builder-and-ssrs.md).  
  
-   **Jauge.** Une jauge présente les données sous forme de plage dans laquelle figure un indicateur qui pointe vers une valeur spécifique. Les jauges sont utilisées pour afficher des indicateurs de performance clés (KPI) et d'autres mesures. Les jauges linéaires et circulaires sont des exemples de jauges. Pour plus d’informations, consultez [Jauges &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/gauges-report-builder-and-ssrs.md).  
  
-   **Carte.** Une carte vous permet de présenter des données avec un arrière-plan géographique. Les données cartographiques peuvent être des données spatiales d'une requête [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , un fichier de forme ESRI ou des mosaïques Bing [!INCLUDE[msCoName](../includes/msconame-md.md)] . Les données spatiales sont des jeux des coordonnées qui définissent des polygones représentant des formes ou des zones, des lignes représentant des itinéraires ou des chemins d'accès, et des points représentés par des marqueurs. Vous pouvez associer des données agrégées aux éléments cartographiques pour varier automatiquement leur couleur et taille. Par exemple, vous pouvez faire varier le type de marqueur pour un magasin selon le chiffre d'affaires réalisé ou la couleur pour une route en fonction de la limite de vitesse. Pour plus d’informations, consultez [Cartes &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/maps-report-builder-and-ssrs.md).  
  
 Vous pouvez également inclure des valeurs provenant de datasets non liés à la région de données selon les méthodes suivantes :  
  
-   Des expressions qui incluent des appels aux fonctions d'agrégation qui spécifient un dataset différent comme paramètre scope, par exemple, `=Max(Fields!Sales.Value, "AnnualSales")`.  
  
-   Utilisez la fonction **Recherche** pour rechercher des valeurs dans des paires nom/valeur dans un dataset différent.  
  
### <a name="report-parts"></a>Parties de rapports
 Une définition de partie de rapport (.rsc) est un élément de serveur de rapport qui est un fragment XML d'un fichier de définition de rapport. Vous créez des parties de rapports en créant une définition de rapport, puis en sélectionnant séparément des éléments de rapport dans le rapport à publier sous forme de parties de rapports. Les parties de rapports incluent des régions de données, des rectangles et les éléments qu'ils contiennent, ainsi que des images. Vous pouvez enregistrer une partie de rapport avec ses références aux sources de données partagées et ses datasets dépendants, afin de pouvoir le réutiliser dans d'autres rapports. Pour plus d’informations, consultez [Parties de rapports dans le Concepteur de rapports &#40;SSRS&#41;](../reporting-services/report-design/report-parts-in-report-designer-ssrs.md).  
  
### <a name="data-alerts"></a>Alertes de données 
 Une alerte de données est un élément stocké en interne dans une base de données d'alerte. Une définition d'alerte de données inclut les données à utiliser à partir des flux de données de rapport existantes, les conditions à rencontrer, une planification, ainsi que les destinataires de l'alerte. Les alertes de données sont disponibles uniquement sur des rapports publiés sur un serveur de rapports intégré à SharePoint Server. Les alertes de données sont disponibles sur une installation de serveur de rapports natif. Pour plus d’informations, consultez [Alertes de données Reporting Services](../reporting-services/reporting-services-data-alerts.md).  
  
  
##  <a name="bkmk_TypesofReports"></a> Types de rapports Reporting Services paginés  
 Dans [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], *le rapport* de terme peut s'appliquer à un type spécifique d'élément de serveur de rapports, à une conception de mise en page, ou à une conception de solution. Un seul et même rapport Reporting Services paginé peut avoir des caractéristiques de plusieurs types ; par exemple, un rapport peut être, simultanément, un rapport autonome, un sous-rapport référencé par un rapport principal, la cible d'un rapport d'extraction dans un rapport principal différent, et un rapport lié.  
  
### <a name="drilldown-reports"></a>Rapports d'analyse 
 Un rapport d'analyse est une création de mise en page qui masque initialement la complexité et permet à l'utilisateur de basculer des éléments de rapport masqués de façon conditionnelle pour contrôler la quantité de données de détail affichées. Les rapports d'analyse doivent extraire toutes les données possibles pouvant être affichées dans le rapport. En présence de rapports utilisant de gros volumes de données, préférez les rapports d'extraction. Pour plus d’informations, consultez [Action d’exploration &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/drilldown-action-report-builder-and-ssrs.md).  
  
### <a name="subreports"></a>Sous-rapports
 Un sous-rapport est un élément de rapport que vous ajoutez à un rapport en tant qu'élément de mise en page. Un sous-rapport point vers un rapport différent et s'affiche dans le corps d'un rapport principal comme sous-rapport instance. Le sous-rapport peut utiliser des sources de données différentes de celles utilisées pour le rapport principal. Bien qu'un sous-rapport puisse être répété dans des régions de données en utilisant un paramètre pour filtrer les données dans chaque instance du sous-rapport, les sous-rapports sont généralement utilisés avec un rapport principal comme un dossier de synthèse ou comme un conteneur pour une collection de rapports connexes. Chaque instance d'un sous-rapport change le contexte pour le traitement des rapports entre le rapport principal et le sous-rapport. En présence de rapports utilisant de nombreuses instances de sous-rapports, préférez les rapports d'extraction. Pour plus d’informations, consultez [Sous-rapports &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/subreports-report-builder-and-ssrs.md).  
  
 ### <a name="maindetail-reports-and-drillthrough-reports"></a>Rapports principal/détaillé et d'extraction
 Une solution de rapport principal/détaillé inclut un rapport principal qui affiche des informations de résumé doté de liens hypertexte à un ou plusieurs rapports qui affichent des informations détaillées.  Le rapport détaillé fonctionne uniquement si un lecteur de rapport clique sur un lien vers celui-ci. Le rapport d'extraction s'ouvre hors du rapport principal. Un lien hypertexte peut être défini sur n'importe quel élément de rapport qui a une propriété Action, par exemple une zone de texte, d'espace réservé ou une série de graphiques. Pour plus d’informations, consultez [Rapports d’extraction &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/drillthrough-reports-report-builder-and-ssrs.md).  
  
### <a name="linked-reports"></a>rapports liés 
 Un rapport lié est un élément de serveur de rapports qui contient un pointeur vers la définition de rapport mais possède son propre jeu de propriétés de rapport et de paramètres. Ceux-ci incluent la sécurité, les paramètres, l'emplacement, les abonnements et les planifications. Les paramètres sont gérés indépendamment sur le serveur ; par conséquent, si vous republiez un rapport principal qui utilise de nouveaux paramètres, les paramètres existants du rapport principal ou du rapport lié ne sont pas remplacés.  
  
 Pour plus d’informations, consultez [Créer un rapport lié](../reporting-services/reports/create-a-linked-report.md).  
  
### <a name="history-reports"></a>Rapports d'historique
 L'historique de rapport est un ensemble d'instantanés de rapport. Vous pouvez utiliser l'historique de rapport pour conserver un enregistrement d'un rapport dans le temps. L'historique de rapport ne convient pas aux rapports contenant des données confidentielles ou des données personnelles. Pour cette raison, l'historique de rapport peut inclure uniquement les rapports qui interrogent une source de données à l'aide d'un jeu unique d'informations d'identification. Une autre solution consiste à créer l'historique d'un rapport en définissant une planification et un abonnement pour remettre le rapport dans un format de fichier exporté dans un partage de fichiers. Pour plus d’informations, consultez [Performances, instantanés, mise en cache &#40;Reporting Services&#41;](../reporting-services/report-server/performance-snapshots-caching-reporting-services.md).  
  
### <a name="cached-reports"></a>Rapports mis en cache 
 Un rapport mis en cache est une copie enregistrée d'un rapport et des données de rapport compilés. Les rapports mis en cache sont utilisés pour améliorer les performances en réduisant le nombre de demandes de traitement au processeur de rapports et le temps requis pour extraire les datasets de rapports volumineux. Ils ont une période d'expiration obligatoire, généralement exprimée en minutes. Pour plus d’informations sur l’utilisation des rapports mis en cache, consultez [Mise en cache de rapports &#40;SSRS&#41;](../reporting-services/report-server/caching-reports-ssrs.md).  
  
 Vous pouvez également mettre en cache les résultats des requêtes d'un dataset partagé. Pour plus d’informations, consultez [Mettre en cache les datasets partagés &#40;SSRS&#41;](../reporting-services/report-server/cache-shared-datasets-ssrs.md).  
  
### <a name="snapshots"></a>Instantanés
 Un instantané de rapport est un rapport contenant des informations de mise en page et des résultats de requêtes récupérés à un moment précis. Contrairement aux rapports à la demande, qui obtiennent les résultats de la requête à jour lorsque vous affichez le rapport, le serveur de rapports récupère le rapport compilé et les données de rapport qui étaient en cours au moment où l'instantané a été créé. Les instantanés de rapport ne sont pas enregistrés dans un format de rendu particulier. Ils sont générés dans un format d'affichage final (par exemple, au format HTML) uniquement à la demande d'un utilisateur ou d'une application. Pour plus d’informations, consultez [Performances, instantanés, mise en cache &#40;Reporting Services&#41;](../reporting-services/report-server/performance-snapshots-caching-reporting-services.md).  
   
### <a name="saved-reports"></a>Rapports enregistrés  
 Un rapport paginé enregistré est un fichier de définition de rapport (.rdl). Une définition de rapport peut être enregistrée localement ou téléchargée sur un serveur de rapports. Si vous téléchargez une définition de rapport au lieu de la publier, aucune validation de version ou validation d'expression ne se produit. Vous ne verrez pas les erreurs jusqu'à ce que le rapport s'exécute. Pour plus d’informations, consultez [Enregistrer et déployer les rapports Reporting Services](../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md#bkmk_SaveandDeploy).  
  
### <a name="published-reports"></a>Rapports publiés
 Un rapport publié est un élément de serveur de rapports que vous publiez sur un serveur de rapports à partir d'un outil de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Sur un serveur de rapports natif, vous publiez le rapport dans un dossier sur lequel vous disposez des autorisations d'accès. Sur un serveur de rapports SharePoint, vous pouvez publier le rapport dans une bibliothèque de documents qui est activée avec le type de contenu rapport. Pour partager le rapport qui en utilise d'autres, l'utilisateur doit avoir reçu l'autorisation d'afficher le rapport. Pour plus d’informations, consultez [Enregistrer et déployer les rapports Reporting Services](../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md#bkmk_SaveandDeploy).  
  
### <a name="upgraded-reports"></a>Rapports mis à jour
 Un rapport mis à jour est une définition de rapport publiée qui est convertie en un nouveau schéma lorsqu'un serveur de rapports est mis à niveau d'une version de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] vers une version ultérieure. La définition de rapport d'origine est conservée. Le rapport est mis à niveau en mémoire, compilé, et la version compilée est enregistrée en interne. Pour plus d'informations, consultez [Mettre à niveau des rapports Reporting Services](../reporting-services/install-windows/upgrade-reports.md).  
  
  
##  <a name="bkmk_StagesofReports"></a> Étapes des rapports Reporting Services paginés  
 Une définition de rapport peut être créée, publiée ou enregistrée, compilée, traitée, mise en cache, rendue, affiche, exportée, et enregistrée comme historique. Lorsque vous exécutez un rapport, le serveur de rapports procède en trois étapes : le traitement du rapport, le traitement des données et le rendu. Le traitement des données et du rapport sont réalisés sur une définition de rapport ; les résultats sont dans un format interne intermédiaire. Les rapports au format intermédiaire sont ensuite rendus dans un format d'affichage spécifique. Le diagramme suivant représente les étapes et les éléments de traitement des rapports.  
  
 ![diagramme de traitement des rapports](../reporting-services/media/report-execution.gif "diagramme de traitement des rapports")  
Illustration du traitement d'un rapport  
  
### <a name="report-definition"></a>Définition de rapport  
 Le fichier de définition de rapport (.rdl) stocké sur un serveur de rapports. Pour plus d’informations, consultez [Langage de définition de rapport &#40;SSRS, Report Definition Language&#41;](../reporting-services/reports/report-definition-language-ssrs.md).  
  
### <a name="compiled-report-and-intermediate-report-format"></a>Rapport compilé et format de rapports intermédiaires 
 Le rapport qui utilise des expressions évaluées, des paramètres et des propriétés de paramètres évalués.  

### <a name="snapshot-or-report-history"></a>Instantané ou historique de rapport
 Un instantané est le jeu de données de rapport à un moment spécifique, ainsi que le format intermédiaire qui contient les informations de mise en page du rapport. Pour plus d’informations, consultez [Performances, instantanés, mise en cache &#40;Reporting Services&#41;](../reporting-services/report-server/performance-snapshots-caching-reporting-services.md).  
  
### <a name="processed-report"></a>Rapports traités  
 Un rapport totalement traité qui contient des données et des informations relatives à la mise en page.  
  
### <a name="rendered-report"></a>Rapport rendu
 Un rapport totalement traité est envoyé vers un rapport rendu pour combiner les données et la mise en page de chaque page du format de rendu ciblé. Les extensions de rendu sont personnalisables et extensibles. Le format de rendu de rapport par défaut est HTML 4.0. Pour plus d’informations, consultez [Mise en page et rendu &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/page-layout-and-rendering-report-builder-and-ssrs.md) et [Extensions &#40;SSRS&#41;](../reporting-services/extensions-ssrs.md).  
  
### <a name="exported-report"></a>Rapports exportés
 Un rapport exporté est un rapport totalement paginé enregistré dans un format de fichier spécifique. Les formats d'exportation dépendent des extensions de rendu installées et peuvent être personnalisés. Par défaut, les formats d'exportation comprennent Excel, Word, XML, PDF, TIFF et CSV. Pour plus d’informations, consultez [Export Reports &#40;Report Builder and SSRS&#41;](../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités et tâches Reporting Services &#40;SSRS&#41;](../reporting-services/reporting-services-features-and-tasks-ssrs.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../reporting-services/technical-reference-ssrs.md)   
 [Reporting Services (SSRS)](../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)  
  
  
