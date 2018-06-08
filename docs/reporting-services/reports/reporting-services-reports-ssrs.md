---
title: Rapports Reporting Services (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
helpviewer_keywords:
- Reporting Services, report creation
ms.assetid: 52ed9e74-f2c8-488b-a2c2-6dfbc2a2c8cc
caps.latest.revision: 54
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 514a36e61bc452a76accdcc6319046ff168aa117
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-reports-ssrs"></a>Rapports Reporting Services (SSRS)
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Les rapports paginés sont des définitions de rapport XML qui incluent des données de rapport et des éléments de disposition du rapport. Sur un système de fichiers client, les définitions de rapport portent l'extension de fichier .rdl. Une fois publié, un rapport paginé correspond à un élément de rapport stocké sur le serveur de rapports ou le site SharePoint. Les rapports paginés constituent une partie de la plateforme de création de rapports sur le serveur fourni par [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Vous pouvez également [Créer des rapports mobiles avec l’Éditeur de rapports mobiles SQL Server](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md).  
  
 Si vous ne connaissez pas [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], veillez à vérifier les informations dans [Concepts de Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-concepts-ssrs.md).  
  
## <a name="benefits-of-reporting-services-paginated-reports"></a>Avantages des rapports Reporting Services paginés  
 Vous pouvez utiliser des solutions de rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour effectuer les opérations suivantes :  
  
-   Utiliser un ensemble de sources de données qui fournissent une seule version des faits. Baser les rapports sur ces sources de données pour fournir une vue des données unifiée afin d'aider à prendre des décisions.  
  
-   Visualiser vos données de plusieurs manières interconnectées à l'aide de régions de données. Afficher les données organisées dans des tables, des matrices ou des analyses croisées, développer/réduire les groupes, graphiques, jauges, indicateurs ou indicateur de performance clé, et cartes, avec la possibilité d'imbriquer des graphiques dans les tables.  
  
-   Afficher des rapports pour votre usage personnel ou publier des rapports sur un serveur de rapports ou un site SharePoint pour les partager avec votre équipe ou organisation.  
  
-   Définir un rapport une fois et l'afficher de diverses façons. Vous pouvez exporter le rapport vers plusieurs formats de fichiers, ou fournir le rapport aux abonnés en tant que fichier électronique ou fichier partagé. Vous pouvez créer plusieurs rapports liés qui appliquent des jeux de paramètres distincts à la même définition de rapport.  
  
-   Utiliser les parties de rapports, les sources de données partagées, les requêtes partagées et les sous-rapports pour définir des visualisations de données en vue de leur réutilisation.  
  
-   Gérer les sources de données de rapport indépendamment de la définition de rapport. Par exemple, vous pouvez passer d'une source de données de test à une source de données de production sans modifier le rapport.  
  
-   Concevoir des rapports dans une disposition de forme libre. La mise en page de rapport n'est pas limitée aux bandes d'informations. Vous pouvez organiser l'affichage des données sur la page de manière à favoriser la compréhension, l'analyse et l'action.  
  
-   Activer les actions d'extraction, développer/réduire les bascules, les boutons de tri, les info-bulles et les paramètres de rapport pour permettre l'interaction des lecteurs du rapport avec le rapport. Utiliser les paramètres de rapport associés à des expressions que vous entrez pour permettre aux lecteurs du rapport de contrôler la façon dont les données sont filtrées, regroupées et triées.  
  
-   Définir des expressions qui vous permettent de personnaliser la façon dont les données de rapport sont filtrées, regroupées et triées.  
  
 ![rs_GettingStartedReport](../../reporting-services/report-builder/media/rs-gettingstartedreport.png "rs_GettingStartedReport")  
  
##  <a name="bkmk_StagesSummary"></a> Étapes du traitement des rapports  
 Lorsque vous créez un rapport, vous définissez un fichier de définition de rapport (.rdl) au format XML. Ce fichier contient toutes les informations nécessaires pour combiner les données et la mise en page du rapport par le processeur de rapports. Lorsque vous affichez un rapport, le rapport passe par les étapes suivantes :  
  
-   **Compilation.** Évalue les expressions dans la définition de rapport et stocke le format intermédiaire compilé en interne sur le serveur de rapports.  
  
-   **Traitement.** Exécute les requêtes de dataset, puis combine le format intermédiaire avec les données et la mise en page.  
  
-   **Rendu.** Envoie le rapport traité à une extension de rendu pour déterminer la quantité d'informations pouvant tenir sur chaque page et crée le rapport paginé.  
  
-   **Exportation (facultatif).** Exporte le rapport vers un autre format de fichier.  
  
 Pour plus d’informations, consultez [Étapes des rapports](../../reporting-services/reporting-services-concepts-ssrs.md#bkmk_StagesofReports) dans [Concepts de Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-concepts-ssrs.md).  
  
## <a name="create-paginated-reports"></a>Créer des rapports paginés  
 Pour créer un rapport paginé, procédez comme suit :  
  
-   **Déterminez l'objectif du rapport.** Identifiez l'objectif du rapport pour le public qui l'utilise. Un rapport bien conçu fournit des informations qui aboutissent à une analyse et à une action. Les décisions de conception prises lors de cette étape influencent votre choix de paramètres de rapport, de mise en page de rapport et d'affichage de rapport. Pour plus d’informations, consultez [Planification d’un rapport &#40;Générateur de rapports&#41;](../../reporting-services/report-design/planning-a-report-report-builder.md) et [Conseils de création de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-design-tips-report-builder-and-ssrs.md).  
  
-   **Sélectionnez le type de requête.** Déterminez s'il faut utiliser une requête de dataset généralisée et partagée ou une requête de dataset spécifique à votre ensemble de rapports. Il est facile de gérer un dataset partagé avec une requête généralisée destinée à être utilisée par plusieurs rapports, mais chaque concepteur de rapports doit filtrer les données autant que nécessaire pour son ensemble de rapports. Pour plus d'informations, consultez [Données de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-data-ssrs.md).  
  
-   **Planifiez les vues de données associées.** Planifiez l'affichage pour les lecteurs de votre rapport. Les rapports de synthèse permettant d'explorer des données de détail constituent une approche très utile pour gérer de grands volumes de données. Pour plus d’informations, consultez [Extraction, exploration, sous-rapports et régions de données imbriquées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drillthrough-drilldown-subreports-and-nested-data-regions.md).  
  
-   **Configurez les autorisations.** Planifiez la stratégie pour accorder le niveau d'autorisations approprié. Une stratégie commune consiste à créer une structure de dossiers sur le serveur de rapports et accorder l'accès aux rapports et rôles basés sur les éléments associés aux rapports, ainsi qu'à la sécurité des dossiers. Pour plus d'informations, consultez [Sécuriser les rapports](#bkmk_SecureReportsSummary).  
  
-   **Choisissez un environnement de création.** La prise en charge des fonctionnalités varie selon les outils de création. Pour plus d’informations, consultez [Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md).  
  
-   Pour chaque rapport :  
  
    -   **Identifiez les sources de données.** Définissez les sources de données de rapport, un pour chaque source de données. Pour plus d’informations, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
    -   **Choisissez les données à utiliser à partir de chaque source.** Pour chaque source de données, définissez les datasets de rapport. Chaque dataset inclut une requête pour spécifier les données à utiliser. Si vous avez des paramètres de rapport, définissez un dataset pour remplir une liste de valeurs disponibles pour chaque paramètre. Pour plus d’informations, consultez [Jeux de données du rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md) et [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
    -   **Choisissez une visualisation des données.** Pour chaque dataset, sélectionnez la région de données à utiliser pour l'affichage des données. Choisissez dans la liste de tables, graphiques, jauges et cartes. Pour plus d'informations, consultez les rubriques suivantes :  
  
        -   [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
        -   [Graphiques (Générateur de rapports et SSRS)](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)  
  
        -   [Graphiques sparkline et barres de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md)  
  
        -   [Indicateurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
        -   [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)  
  
        -   [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)  
  
    -   **Personnalisez les données et la mise en page.** Concevez la mise en page du rapport. Une définition de rapport comporte un corps de rapport, des sources de données, des datasets, des régions de données, des zones de texte, des lignes et des images. Les rectangles sont utilisés comme conteneurs pour la mise en page ainsi que les éléments visuels. Personnalisez chaque région de données en écrivant des expressions pour contrôler le filtrage, le regroupement, le tri, le format et l'affichage des données. Ajoutez les noms de rapport, les emplacements, et d'autres informations d'identification qui permettent de gérer des dizaines ou des centaines de rapports. Ajoutez conteneurs et des éléments visuels pour organiser les éléments de mise en page sur la page. Pour plus d'informations, consultez les rubriques suivantes :  
  
        -   [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)  
  
        -   [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)  
  
        -   [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
        -   [Mise en forme des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)  
  
        -   [Images, zones de texte, rectangles et lignes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/images-text-boxes-rectangles-and-lines-report-builder-and-ssrs.md)  
  
        -   [Mise en page et rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/page-layout-and-rendering-report-builder-and-ssrs.md)  
  
    -   **Configurez les fonctionnalités d'interactivité.** Ajoutez des fonctionnalités d'interactivité pour les lecteurs de votre rapport. Par exemple, ajoutez des boutons de tri ou des éléments de bascule pour afficher les requêtes. Pour plus d’informations, consultez [Tri interactif, Explorateurs de documents et liens &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/interactive-sort-document-maps-and-links-report-builder-and-ssrs.md).  
  
    -   **Vérifiez et effectuez une itération au sein de la création.** Affichez l'aperçu du rapport. Publiez une version préliminaire pour obtenir des commentaires des lecteurs de votre rapport. Itérez au sein de la création.  
  
-   **Examinez la solution de création de rapports.** Vérifiez que l'ensemble des rapports interagissent correctement.  
  
-   **Prenez en compte les composants pouvant être réutilisés.**  Déterminez si les sources de données ou les requêtes de dataset peuvent être partagées en vue d'une réutilisation. Dans l'affirmative, sur le serveur de rapports ou site SharePoint, créez des sources de données partagées et des datasets partagés. Déterminez si les régions de données conviennent pour la réutilisation en tant que parties de rapports. Pour plus d’informations, consultez [Parties de rapports dans le Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md).  
  
## <a name="preview-reports"></a>Afficher un aperçu des rapports  
 Chaque outil de création de rapports prend en charge l'affichage de l'aperçu d'un rapport. Pour plus d’informations, consultez la section [Aperçu](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md#bkmk_Preview) dans [Concevoir des rapports à l’aide du Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md) et [Aperçu des rapports dans le Générateur de rapports](../../reporting-services/report-builder/previewing-reports-in-report-builder.md).  
  
## <a name="save-or-publish-reports"></a>Enregistrer ou publier des rapports  
 Chaque outil de création prend en charge l'enregistrement d'un rapport localement ou la publication du rapport sur un serveur de rapports ou un site SharePoint. Pour plus d’informations, consultez la section [Enregistrer et déployer](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md#bkmk_SaveandDeploy) dans [Concevoir des rapports à l’aide du Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md) et [Enregistrement des rapports &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/saving-reports-report-builder.md).  
  
## <a name="view-reports"></a>Afficher les rapports  
 En plus de l'aperçu d'un rapport enregistré localement ou publié sur un serveur de rapports, vous pouvez fournir un large éventail d'affichages pour les lecteurs de votre rapport. Pour afficher un rapport :  
  
-   **Navigateur.**  Utilisez le service Web Report Server ou le site SharePoint pour afficher les rapports publiés. Sur un site SharePoint, vous pouvez également configurer un composant WebPart pour afficher les rapports publiés. Pour plus d’informations, consultez [Planification de la prise en charge des navigateurs pour Reporting Services et Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md), [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896) et [Accès URL &#40;SSRS&#41;](../../reporting-services/url-access-ssrs.md).  
  
-   **Remise.**  Configurez un abonnement pour remettre les rapports aux personnes souhaitant les lire dans un dossier de messagerie ou dans un dossier partagé.  Pour plus d’informations, consultez [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
-   **exportation.**  Dans la barre d'outils de la visionneuse de rapports, un lecteur du rapport peut exporter un rapport dans un autre format de fichier. Les formats de fichier d'exportation peuvent être configurés par l'administrateur du serveur de rapports. Pour plus d’informations, consultez [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
-   **Impression.**  Un lecteur de rapport peut imprimer un rapport ou des pages d'un rapport selon la façon dont il est affiché. Pour plus d’informations, consultez [Imprimer des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md).  
  
-   **Application Web ou Windows Form.**  Utilisez Visual Studio pour développer une application AJAX ASP.NET ou Windows Form qui héberge le contrôle Visionneuse de rapports. Le contrôle peut indiquer les rapports publiés sur un serveur de rapports. Pour plus d'informations, consultez [Rapports Microsoft](http://go.microsoft.com/fwlink/?LinkID=205399).  
  
## <a name="manage-reports"></a>Gérer les rapports  
 Pour gérer un rapport publié :  
  
-   **Sources de données.** Les sources de données incorporées et partagées sont gérées indépendamment de la définition de rapport.  
  
-   **Datasets.**  Les datasets partagés sont gérés indépendamment de la définition de rapport.  
  
-   **Paramètres.**  Les paramètres sont gérés indépendamment de la définition de rapport. Une fois les paramètres modifiés sur le serveur de rapports, les clients de création de rapports ne peuvent pas publier sur les modifications effectuées sur le serveur.  
  
-   **Ressources.**  Les images et les données spatiales dans les fichiers de forme ESRI sont des ressources qui peuvent être publiées et gérées indépendamment de la définition de rapport.  
  
-   **Cache de rapports.**  En planifiant l'exécution de rapports volumineux durant les heures creuses, vous diminuez l'impact de leur traitement sur le serveur de rapports pendant les heures d'activité principale de l'entreprise.  
  
-   **Instantanés.**  Utilisez les instantanés de rapport lorsque vous voulez fournir des résultats homogènes à plusieurs utilisateurs devant travailler sur des jeux de données identiques. Avec des données volatiles, un rapport à la demande peut produire des résultats différents en l'espace d'une minute. En revanche, un instantané de rapport vous permet d'effectuer des comparaisons valides par rapport à d'autres rapports ou peut servir d'outil d'analyse contenant des données toutes datées d'un même point dans le temps.  
  
-   **Historique de rapport.** En créant une série d'instantanés, vous pouvez générer l'historique d'un rapport qui montre les modifications des données dans le temps.  
  
 Pour plus d’informations sur les performances, consultez [Performances, instantanés, mise en cache &#40;Reporting Services&#41;](../../reporting-services/report-server/performance-snapshots-caching-reporting-services.md).  
  
##  <a name="bkmk_SecureReportsSummary"></a> Sécuriser les rapports  
 Pour sécuriser un rapport :  
  
-   Auprès de l'administrateur du serveur de rapports, identifiez le système d'autorisation et d'authentification utilisé pour votre installation de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Par défaut, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise l'authentification Windows, la sécurité intégrée, et l'attribution de rôle pour contrôler l'accès aux rapports publiés. Pour plus d’informations, consultez [Rôles et autorisations &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md) et [Sécurité et protection de Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md).  
  
## <a name="create-notifications-based-on-report-data"></a>Créer des notifications en fonction de données de rapport  
 Vous pouvez créer des alertes de données pour les rapports publiés sur un site SharePoint. Les alertes de données sont basées sur les flux de données des régions de données dans le rapport. Par défaut, les régions de données sont nommées automatiquement. Les auteurs de rapport peuvent faciliter la création d'alertes de données dans leurs rapports en nommant les régions de données en fonction de leur objectif métier. Si vous créez une alerte de données, vous êtes averti par message électronique lorsque les données remplissent les critères que vous spécifiez. Pour plus d’informations, consultez [Génération de flux de données à partir de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md), [Créer une alerte de données dans le Concepteur d’alertes](../../reporting-services/create-a-data-alert-in-data-alert-designer.md) et [Alertes de données Reporting Services](../../reporting-services/reporting-services-data-alerts.md).  
  
## <a name="upgrade-reports"></a>Mettre à niveau des rapports  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prend en charge plusieurs versions des définitions de rapports, serveurs de rapports et sites SharePoint. Pour mettre à niveau un rapport :  
  
-   Mettez à niveau une installation du serveur de rapports. Les rapports compilés stockés sur le serveur de rapports sont automatiquement mis à niveau lors de la première utilisation. La définition de rapport (.rdl) n'est pas modifiée. Pour plus d'informations, consultez [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
-   Ouvrez u rapport dans un environnement de création de rapports. La définition de rapport est mise à niveau dans la plupart des cas. Pour plus d’informations, consultez [Mettre à niveau des rapports](../../reporting-services/install-windows/upgrade-reports.md) et [Déploiement et prise en charge des versions dans SQL Server Data Tools &#40;SSRS&#41;](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
## <a name="troubleshoot-reports"></a>Dépanner les rapports  
 Pour dépanner un rapport :  
  
-   **Déterminez où se produit le problème.** Vérifiez les informations dans [Étapes d'un rapport](#bkmk_StagesSummary).  
  
-   **Déterminez où vous pouvez rechercher plus d'informations.** Par exemple, pour la création de rapports qui inclut des expressions, l'outil Concepteur de rapports fournit plus d'informations sur les problèmes d'évaluation d'expression que l'outil Générateur de rapports. Pour les erreurs de traitement des rapports, les fichiers journaux contiennent des informations détaillées.  
  
## <a name="see-also"></a> Voir aussi  
 [Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md)   
 [Extensions &#40;SSRS&#41;](../../reporting-services/extensions-ssrs.md)   
 [Serveur de rapports Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)  
  
  
