---
title: Paramètres système (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Master Data Services, system settings
- system settings [Master Data Services]
ms.assetid: 83075cdf-f059-4646-8ba2-19be8202f130
caps.latest.revision: 17
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 19f244d7febe15f03b5510ba484b985044ad3548
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="system-settings-master-data-services"></a>Paramètres système (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Vous pouvez configurer les paramètres système de l'ensemble des applications Web et services Web associés à une base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
 un grand nombre de ces paramètres peuvent être configurés dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] sur la page **Base de données** . D'autres peuvent être configurés dans la table Paramètres système (mdm.tblSystemSetting) dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
 Les paramètres peuvent être regroupés selon les catégories suivantes :  
  
-   [Paramètres généraux](#General)  
  
-   [Paramètres de gestion de la version](#Versions)  
  
-   [Paramètres de mise en lots](#Staging)  
  
-   [Paramètres de l'explorateur](#Explorer)  
  
-   [Paramètres du complément pour Microsoft Excel](#xls)  
  
-   [Paramètres de règle d'entreprise](#BusinessRules)  
  
-   [Paramètres de notification](#Notifications)  
  
-   [Paramètres de sécurité](#Security)  
  
-   [Non utilisé](#NotUsed)  
  
##  <a name="General"></a> Paramètres généraux  
  
|Paramètre du Gestionnaire de configuration|Paramètre système|Description|  
|-----------------------------------|--------------------|-----------------|  
|**Délai d'attente de connexion de la base de données**|**DatabaseConnectionTimeOut**|Nombre de secondes accordées par la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] comme délai d'obtention d'une connexion. Si la connexion n'est pas obtenue dans cet intervalle, elle est annulée et une erreur est retournée. La valeur par défaut est **60** secondes (1 minute).|  
|**Délai d'expiration des commandes de base de données**|**DatabaseCommandTimeOut**|Nombre de secondes accordées par la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] comme délai d'exécution d'une commande. Si la commande n'est pas exécutée dans cet intervalle, elle est annulée et une erreur est retournée. La valeur par défaut est **3 600** secondes (60 minutes).|  
|**Délai d'attente du service Web**|**ServerTimeOut**|Nombre de secondes accordées par ASP.NET comme délai d'exécution d'une demande de page [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Si la demande ne se termine pas dans cet intervalle, elle est annulée et une erreur est retournée. La valeur par défaut est **120 000** secondes (2 000 minutes).|  
|**Délai d'expiration client**|**ClientTimeOut**|Nombre de secondes d'inactivité avant que [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] ne retourne à la page d'accueil. La valeur par défaut est **300** secondes (5 minutes).|  
|**Nombre de lignes par lot**|**RowsPerBatch**|Nombre d'enregistrements à récupérer dans chaque lot par le service Web. La valeur par défaut est de **50**.|  
||**ApplicationName**|Texte affiché dans les journaux des événements. La valeur par défaut est **MDM**.|  
||**SiteTitle**|Texte affiché dans la barre de titre du navigateur Web de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . La valeur par défaut est **Master Data Manager**.|  
|**Nombre de jours de rétention du journal**|**LogRentionDays**|Nombre de jours au terme desquels les journaux seront supprimés. La valeur par défaut est de -1 et indique que les tables des journaux ne seront pas nettoyées.<br /><br /> Si la valeur est de 0, les tables des journaux conservent uniquement les données du jour même. Les journaux de données des jours précédents sont tronqués.<br /><br /> Si la valeur est supérieure à 0, les données des journaux sont conservées pendant le nombre de jours spécifié par la valeur.|  
  
##  <a name="Versions"></a> Paramètres de gestion de la version  
  
|Paramètre du Gestionnaire de configuration|Paramètre système|Description|  
|-----------------------------------|--------------------|-----------------|  
|**Copier uniquement les versions activées**|**CopyOnlyCommittedVersion**|Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], détermine si les utilisateurs peuvent copier les versions de modèle avec un état **Activé**, ou les versions avec n'importe quel état. La valeur par défaut est **Oui** ou **1**, ce qui indique que les utilisateurs peuvent copier uniquement les versions **Activées** . Remplacez-la par la valeur **Non** ou **2** pour permettre aux utilisateurs de copier toutes les versions.|  
  
 Pour plus d’informations, consultez [Versions &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md).  
  
##  <a name="Staging"></a> Paramètres de mise en lots  
  
|Paramètre du Gestionnaire de configuration|Paramètre système|Description|  
|-----------------------------------|--------------------|-----------------|  
|**Journaliser toutes les transactions intermédiaires**|**StagingTransactionLogging**|S'applique à SQL Server 2008 R2 uniquement. Détermine s'il convient ou non de journaliser les transactions lorsque des enregistrements de mise en lots sont chargés dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . La valeur par défaut est **Désactivé** ou **2**. Remplacez-la par la valeur **Activé** ou **1** pour activer la journalisation.|  
|**Intervalle de lot intermédiaire**|**StagingBatchInterval**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **de** , intervalle (en secondes) entre la sélection de **Démarrer les lots** et le traitement de votre lot. La valeur par défaut est **60** secondes (1 minute).|  
  
 Pour plus d’informations, consultez [Présentation : Importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
##  <a name="Explorer"></a> Paramètres de l'explorateur  
  
|Paramètre du Gestionnaire de configuration|Paramètre système|Description|  
|-----------------------------------|--------------------|-----------------|  
|**Nombre de membres dans la hiérarchie par défaut**|**HierarchyChildNodeLimit**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **de** , nombre maximal de membres affichés dans chaque nœud de la hiérarchie avant que **…plus…** ne s'affiche. Vous pouvez cliquer sur **…plus…** pour afficher le groupe de membres suivant. La valeur par défaut est de **50**.|  
|**Afficher les noms dans la hiérarchie**|**ShowNamesInHierarchy**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **de** , détermine le paramètre par défaut sélectionné lorsque vous affichez des hiérarchies.<br /><br /> La valeur par défaut est **Oui** ou **1**, ce qui indique que le nom et le code de chaque membre sont affichés. Remplacez-la par la valeur **Non** ou **2** pour afficher le code uniquement.|  
|**Limite de la liste d'attributs basés sur un domaine**|**DBAListRowLimit**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **Explorer** functional area, the number of attributes that are displayed in a list when you double-click a domain-based attribute value in the grid. La valeur par défaut est de **50**. S'il existe plus de 50 membres, une boîte de dialogue permettant d'effectuer des recherches s'affiche à la place.|  
||**GridFilterDefaultFuzzySimilarityLevel**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **de** , niveau de ressemblance utilisé lors de l'utilisation des critères de filtre **Correspond à** . La valeur par défaut est de **0,3**. Définissez une valeur proche de **1** pour retourner une correspondance proche des critères de recherche. Définissez la valeur **1** pour une correspondance exacte.|  
  
##  <a name="xls"></a> Paramètres du complément pour Microsoft Excel  
  
|Paramètre du Gestionnaire de configuration|Paramètre système|Description|  
|-----------------------------------|--------------------|-----------------|  
|Afficher le complément pour le texte Excel sur la page d'accueil du site Web|ShowAddInText|Sur la page d'accueil [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , affichez un lien pour que les utilisateurs téléchargent [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)].|  
|Chemin d'installation du complément pour Excel sur la page d'accueil du site Web|AddInURL|Sur la page d'accueil [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , si le lien à [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] est affiché, emplacement où les utilisateurs accèdent lorsqu'ils cliquent sur le lien.|  
  
##  <a name="BusinessRules"></a> Paramètres de règle d'entreprise  
  
|Paramètre du Gestionnaire de configuration|Paramètre système|Description|  
|-----------------------------------|--------------------|-----------------|  
|**Nombre par lequel les nouvelles règles d'entreprise sont incrémentées**|**BusinessRuleDefaultPriorityIncrement**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **de** , nombre par lequel la priorité de chaque nouvelle règle d'entreprise est incrémentée. La valeur par défaut est de **10**.|  
|**Nombre de membres auxquels appliquer des règles d'entreprise**|**BusinessRuleRealtimeMemberCount**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **de** , nombre maximal de membres dans la grille auxquels appliquer des règles d'entreprise. Dans [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)], nombre maximal de membres dans la feuille de calcul active auxquels appliquer des règles d'entreprise. La valeur par défaut est de **10 000**.|  
  
 Pour plus d’informations, consultez [Règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md).  
  
##  <a name="Notifications"></a> Paramètres de notification  
  
|Paramètre du Gestionnaire de configuration|Paramètre système|Description|  
|-----------------------------------|--------------------|-----------------|  
|**URL Master Data Manager pour les notifications**|**MDMRootURL**|URL de l’application web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], utilisée dans le lien des notifications par e-mail, par exemple `http://constoso/mds`.|  
|**Intervalle de notification par courrier électronique**|**NotificationInterval**|Fréquence à laquelle les notifications par courrier électronique sont envoyées (en secondes). La valeur par défaut est **120** secondes (2 minutes).|  
|**Nombre de notifications dans un même message électronique**|**NotificationsPerEmail**|Nombre maximal de problèmes de validation qui sont répertoriés dans un seul message électronique de notification. Les problèmes supplémentaires, le cas échéant, ne sont pas inclus dans le message électronique, mais sont disponibles dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].|  
|**Format de courrier électronique par défaut**|**EmailFormat**|Format de toutes les notifications par courrier électronique. La valeur par défaut est **HTML** ou **1**. Le paramètre de base de données **2** indique **Texte**.<br /><br /> Remarque : vous pouvez remplacer cette valeur pour un utilisateur spécifique dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]en changeant et en enregistrant le **Format de courrier électronique** dans l’onglet **Général** de l’utilisateur.|  
|**Expression régulière pour l'adresse de messagerie**|**EmailRegExPattern**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **de** , expression régulière qui permet de valider l'adresse de messagerie entrée sous l'onglet **Général** de l’utilisateur. Pour plus d'informations sur les expressions régulières, consultez [Éléments du langage des expressions régulières](http://go.microsoft.com/fwlink/?LinkId=164401) dans MSDN Library.|  
|**Compte de messagerie de base de données**|**EmailProfilePrincipalAccount**|Affiche le compte de messagerie de base de données à utiliser lors de l'envoi de notifications par courrier électronique. Le profil par défaut est **mds_email_user**.|  
|**Profil de messagerie de base de données**|**DatabaseMailProfile**|Profil de messagerie de base de données à utiliser lors de l'envoi de notifications par courrier électronique. La valeur par défaut est vide.|  
||**ValidationIssueHTML**|Au format HTML, texte du courrier électronique que les utilisateurs reçoivent lors de l'échec de la validation d'une règle d'entreprise.|  
||**ValidationIssueText**|Au format texte brut, texte du courrier électronique que les utilisateurs reçoivent lors de l'échec de la validation d'une règle d'entreprise.|  
||**VersionStatusChangeText**|Au format texte brut, texte du courrier électronique que les utilisateurs reçoivent lors du changement d'état d'une version. Seuls les utilisateurs disposant de l'autorisation **Mettre à jour** sur le modèle entier reçoivent ce courrier électronique.|  
||**VersionStatusChangeHTML**|Au format HTML, texte du courrier électronique que les utilisateurs reçoivent lors du changement d'état d'une version. Seuls les utilisateurs disposant de l'autorisation **Mettre à jour** sur le modèle entier reçoivent ce courrier électronique.|  
  
 Pour plus d’informations, consultez [Notifications &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md).  
  
##  <a name="Security"></a> Paramètres de sécurité  
  
|Paramètre du Gestionnaire de configuration|Paramètre système|Description|  
|-----------------------------------|--------------------|-----------------|  
||**SecurityMemberProcessInterval**|Dans la zone fonctionnelle [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **de** , fréquence à laquelle les autorisations d'accès définies sous l'onglet **Membres de hiérarchie** sont appliquées (en secondes). La valeur par défaut est **3 600** secondes (60 minutes).|  
  
 Pour plus d’informations, consultez [Appliquer immédiatement des autorisations de membre &#40;Master Data Services&#41;](../master-data-services/immediately-apply-member-permissions-master-data-services.md).  
  
##  <a name="NotUsed"></a> Non utilisé  
 Les paramètres suivants dans la table Paramètres système ne sont pas utilisés.  
  
-   **SecurityMode**  
  
-   **MDSHubName**  
  
-   **ApplicationLogging**  
  
-   **ReportServer**  
  
-   **ReportDirectory**  
  
-   **BusinessRuleEngineIterationLimit**  
  
-   **BusinessRuleExtensibility**  
  
-   **AttributeExplorerMarkAllActionMemberCount**  
  
## <a name="see-also"></a> Voir aussi  
 [Sécurité de l’objet de base de données &#40;Master Data Services&#41;](../master-data-services/database-object-security-master-data-services.md)  
  
  
