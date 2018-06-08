---
title: Propriétés du serveur (page Avancé) - Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.serverproperties.advanced.f1
ms.assetid: 07b78a84-a6aa-4502-861d-349720ef790e
caps.latest.revision: 18
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: dfbfe74439ecbf3db24f1a2a32bd5ab5d528fb72
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="server-properties-advanced-page---reporting-services"></a>Propriétés du serveur (page Avancé) - Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)])

Utilisez cette page pour définir des propriétés système sur le serveur de rapports. Il existe plusieurs façons de définir des propriétés système. Cet outil fournit une interface utilisateur graphique afin que vous puissiez définir des propriétés sans devoir écrire du code.

Pour ouvrir cette page, démarrez SQL Server Management Studio, connectez-vous à une instance de serveur de rapports, cliquez avec le bouton droit sur le nom du serveur de rapports, puis sélectionnez **Propriétés**. Sélectionnez **Avancé** pour ouvrir cette page.

## <a name="options"></a>Options

**EnableMyReports**  
Indique si la fonctionnalité Mes rapports est activée. La valeur **true** indique que la fonctionnalité est activée.  

**MyReportsRole**  
Nom du rôle utilisé lors de la création des stratégies de sécurité sur le dossier Mes rapports de l'utilisateur. La valeur par défaut est **Rôle de mes rapports**.  

**EnableClientPrinting**  
Détermine si le contrôle ActiveX RSClientPrint peut être téléchargé à partir du serveur de rapports. Les valeurs valides sont **true** et **false**. La valeur par défaut est **true**. Pour plus d’informations sur les paramètres supplémentaires nécessaires pour ce contrôle, consultez [Activer et désactiver l’impression côté client pour Reporting Services](../../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md).  

**EnableExecutionLogging**  
Indique si la journalisation de l'exécution des rapports est activée. La valeur par défaut est **true**. Pour plus d’informations sur le journal des exécutions du serveur de rapports, consultez [Journal des exécutions du serveur de rapports et vue ExecutionLog3](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  

**ExecutionLogDaysKept**  
Nombre de jours pendant lesquels conserver les informations sur l'exécution du rapport dans le journal des exécutions. Les valeurs valides pour cette propriété sont comprises entre **-1** et **2** **147** **483** **647**. Si la valeur est égale à **-1** , les entrées ne sont pas supprimées de la table du journal des exécutions. La valeur par défaut est **60**.  

> [!NOTE] 
> La définition d’une valeur égale à **0** *supprime* toutes les entrées du journal d’exécution. La valeur  **-1** conserve les entrées du journal d’exécution et ne les supprime pas.

**SessionTimeout**  
Durée (en secondes) pendant laquelle une session demeure active. La valeur par défaut est **600**.  

**SharePointIntegratedMode**  
Propriété en lecture seule qui indique le mode serveur. Si cette valeur est False, le serveur de rapports s'exécute en mode natif.  

**SiteName**  
Nom du site du serveur de rapports affiché dans le titre de la page du portail web. La valeur par défaut est [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Cette propriété peut être une chaîne vide. La longueur maximale autorisée s’élève à 8 000 caractères.  

**StoredParametersLifetime**  
Nombre maximal de jours pendant lesquels un paramètre stocké peut être stocké. Les valeurs valides sont comprises entre **-1**, **+1** et **2147483647**. La valeur par défaut est **180** jours.  

**StoredParametersThreshold**  
Nombre maximal de valeurs de paramètres qui peuvent être stockées par le serveur de rapports. Les valeurs valides sont comprises entre **-1**, **+1** et **2147483647**. La valeur par défaut est de **1500**.  

**UseSessionCookies**  
Indique si le serveur de rapports doit utiliser les cookies de session lors la communication avec les navigateurs clients. La valeur par défaut est **true**.  

**ExternalImagesTimeout**  
Détermine la période pendant laquelle un fichier image externe doit être récupéré avant l'expiration du délai de connexion. La valeur par défaut est **600** secondes.  

**SnapshotCompression**  
Définit le mode de compression des instantanés. La valeur par défaut est **SQL**. Les valeurs valides sont les suivantes :

|Valeurs|Description|
|---------|---------|
|**SQL**|Les instantanés sont compressés quand ils sont stockés dans la base de données du serveur de rapports. Il s'agit du comportement actuel.|
|**Aucun**|Les instantanés ne sont pas compressés.|
|**Tous**|Les instantanés sont compressés pour toutes les options de stockage, qui incluent la base de données du serveur de rapports ou le système de fichiers.|

**SystemReportTimeout**  
Valeur (en secondes) du délai d'exécution du traitement du rapport par défaut pour tous les rapports gérés dans l'espace de noms du serveur de rapports. Cette valeur peut être remplacée au niveau du rapport. Si cette propriété est définie, le serveur de rapports essaie d'arrêter le traitement d'un rapport lorsque le délai spécifié est expiré. Les valeurs valides sont comprises entre **-1** et **2** **147** **483** **647**. Si la valeur est égale à **-1**, les rapports de l’espace de noms ne spécifient pas de délai d’exécution pendant le traitement. La valeur par défaut est **1800**.  

**SystemSnapshotLimit**  
Nombre maximal d'instantanés stockées pour un rapport. Les valeurs valides sont comprises entre **-1** et **2** **147** **483** **647**. Si la valeur est égale à **-1**, il n’existe aucune limite sur le nombre d’instantanés.  

**EnableIntegratedSecurity**  
Détermine si la sécurité intégrée de Windows est prise en charge pour les connexions à la source de données de rapports. La valeur par défaut est **True**. Les valeurs valides sont les suivantes :

|Valeurs|Description|
|---------|---------|
|**True**|La sécurité intégrée de Windows est activée.|
|**False**|La sécurité intégrée de Windows n’est pas activée. Les sources de données de rapports qui sont configurées de manière à utiliser la sécurité intégrée de Windows ne seront pas exécutées.|

**EnableLoadReportDefinition**  
Sélectionnez cette option pour spécifier si les utilisateurs peuvent effectuer une exécution de rapport ad hoc à partir d'un rapport du Générateur de rapports. La définition de cette option spécifie la propriété **EnableLoadReportDefinition** sur le serveur de rapports.  

Si vous désactivez cette option, la valeur False sera affectée à la propriété et le serveur de rapports ne générera pas de rapports générés interactifs pour les rapports qui utilisent un modèle de rapport comme source de données. Tout appel à la méthode LoadReportDefinition sera bloqué.  

La désactivation de cette option atténue la menace qu'un utilisateur malveillant lance une attaque par déni de service en surchargeant le serveur de rapports avec les demandes LoadReportDefinition.  

**EnableRemoteErrors**  
Inclut les informations externes sur l'erreur (par exemple, les informations d'erreur relatives aux sources de données de rapport) avec les messages d'erreur retournés pour les utilisateurs qui demandent des rapports à partir d'ordinateurs distants. Les valeurs valides sont **true** et **false**. La valeur par défaut est **false**. Pour plus d’informations, consultez [Activer les erreurs distantes &#40;Reporting Services&#41;](../../reporting-services/report-server/enable-remote-errors-reporting-services.md).  

**EnableReportDesignClientDownload**  
Spécifie si le package d'installation du Générateur de rapports peut être téléchargé à partir du serveur de rapports. Si vous effacez ce paramètre, l'URL du Générateur de rapports ne fonctionnera pas. Pour plus d’informations, consultez [Configurer l’accès au Générateur de rapports](../../reporting-services/report-server/configure-report-builder-access.md).  

**EditSessionCacheLimit**  
Spécifie le nombre des entrées de cache de données qui peuvent être actives dans une session d'édition de rapport. La valeur par défaut est 5.  

**EditSessionTimeout**  
Spécifie le nombre de secondes jusqu'à l'expiration d'une session d'édition de rapport. La valeur par défaut est 7 200 secondes (2 heures).  

**EnableCustomVisuals** ***(Power BI Report Server uniquement)***  
Indique si PowerBI Report Server doit activer l’affichage des visuels personnalisés Power BI. Les valeurs sont True, False.  La valeur par défaut est True.  

**EnablePowerBIReportExportData** ***(Power BI Report Server uniquement)***  
Indique si PowerBI Report Server doit activer l’exportation de données à partir de visuels Power BI. Les valeurs sont True, False.  La valeur par défaut est True.  

**ScheduleRefreshTimeoutMinutes** ***(Power BI Report Server uniquement)***  
Délai d’actualisation des données, en minutes, pour l’actualisation planifiée des rapports Power BI avec des modèles AS incorporés. La valeur par défaut est 120 minutes.

**EnableTestConnectionDetailedErrors**  
Indique si les messages d'erreur détaillés sont envoyés à l'ordinateur client lorsque des utilisateurs testent des connexions de la source des données à l'aide du serveur de rapports. La valeur par défaut est **true**. Si l’option est définie sur **false**, seuls les messages d’erreur génériques sont envoyés.

**AccessControlAllowCredentials**  
Indique si la réponse à la requête du client peut être exposée quand l’indicateur 'credentials' a la valeur true. La valeur par défaut est **false**.

**AccessControlAllowHeaders** Liste séparée par des virgules des en-têtes autorisés par le serveur quand un client soumet une requête. Cette propriété peut être une chaîne vide. Spécifiez * pour autoriser tous les en-têtes.

**AccessControlAllowMethods** Liste séparée par des virgules des méthodes HTTP autorisées par le serveur quand un client soumet une requête. Les valeurs par défaut sont GET, PUT, POST, PATCH, DELETE. Spécifiez * pour autoriser toutes les méthodes.

**AccessControlAllowOrigin** Liste séparée par des virgules des origines autorisées par le serveur quand un client soumet une requête. La valeur par défaut est vide, ce qui empêche toutes les requêtes. Spécifiez * pour autoriser toutes les origines quand les informations d’identification ne sont pas définies. Si les informations d’identification sont spécifiées, une liste explicite d’origines doit être indiquée.

**AccessControlExposeHeaders** Liste séparée par des virgules des en-têtes que le serveur expose aux clients. La valeur par défaut est vide.

**AccessControlMaxAge** Spécifie le nombre de secondes pendant lesquelles les résultats de la requête préliminaire peuvent être mis en cache. La valeur par défaut est 600 (10 minutes).

## <a name="see-also"></a> Voir aussi

[Définir les propriétés du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md)   
[Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
[Propriétés de Reporting Services](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties.md)   
[Aide du serveur de rapports dans Management Studio accessible par la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
[Propriétés système de Report Server](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md)   
[Écrire des scripts pour les tâches d'administration et de déploiement](../../reporting-services/tools/script-deployment-and-administrative-tasks.md)   
[Activer et désactiver Mes rapports](../../reporting-services/report-server/enable-and-disable-my-reports.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
