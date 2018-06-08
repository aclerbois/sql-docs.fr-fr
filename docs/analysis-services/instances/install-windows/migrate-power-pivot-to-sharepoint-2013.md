---
title: Migrer PowerPivot vers SharePoint 2013 | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 877697915df0bd082ea4972464572f4236556e36
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="migrate-power-pivot-to-sharepoint-2013"></a>Migrer PowerPivot vers SharePoint 2013
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  
  
 SharePoint 2013 ne prend pas en charge la mise à niveau sur place. Cependant, la procédure de **mise à niveau avec liaison des bases de données est prise en charge**. Le comportement est différent de la mise à niveau vers SharePoint 2010, dans laquelle un client avait le choix entre les deux méthodes de mise à niveau de base : la mise à niveau sur place et la mise à niveau avec liaison des bases de données.  
  
 Si vous avez une installation [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] intégrée à SharePoint 2010, vous ne pouvez pas effectuer une mise à niveau sur place du serveur SharePoint. Toutefois, vous pouvez migrer les bases de données de contenu et les bases de données d'application de service de la batterie de serveurs SharePoint 2010 vers une batterie de serveurs SharePoint 2013. Cette rubrique est une vue d’ensemble des étapes requises pour effectuer une mise à niveau avec liaison des bases de données et pour effectuer une migration associée à [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]:  
  
### <a name="migration-overview"></a>Vue d'ensemble de la migration  
  
|1|2|3|4|  
|-------|-------|-------|-------|  
|Préparer la batterie de serveurs SharePoint 2013|Sauvegarder, copier et restaurer les bases de données|Monter les bases de données de contenu|Migrer les planifications [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]|  
||[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|- Administration centrale de SharePoint<br /><br /> - Windows PowerShell|- Pages d’application SharePoint<br /><br /> - Windows PowerShell|  
  
##  <a name="bkmk_prepare_sharepoint2013"></a>Préparer la batterie de serveurs SharePoint 2013  
  
  > [!TIP]  
    >  Examinez la méthode d'authentification configurée pour vos applications Web existantes. Par défaut, les applications Web SharePoint 2013 utilisent l'authentification basée sur les revendications. Les applications Web SharePoint 2010 configurées pour l'authentification en mode classique requièrent des étapes supplémentaires pour migrer des bases de données SharePoint 2010 vers SharePoint 2013. Si vos applications Web sont configurées pour l'authentification en mode classique, consultez la documentation SharePoint 2013.  
  
1.  Installez une nouvelle batterie de serveurs SharePoint Server 2013.  
  
2.  Installez une instance d'un serveur [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode SharePoint. Pour plus d’informations, voir [Install Analysis Services in Power Pivot Mode](../../../analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode.md).  
  
3.  Exécutez le package d'installation [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] 2013 **spPowerPivot.msi** sur chaque serveur de la batterie SharePoint. Pour plus d’informations, consultez [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013.md).  
  
4.  Dans l'Administration centrale de SharePoint 2013, configurez l'application de service Excel Services de sorte à utiliser le serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode SharePoint créé à l'étape précédente. Pour plus d'informations, consultez la section « Configurer l'intégration SharePoint de base pour Analysis Services » de la rubrique [Install Analysis Services in Power Pivot Mode](../../../analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode.md).  
  
##  <a name="bkmk_backup_restore"></a>Sauvegarder, copier et restaurer les bases de données  
 Le processus de « mise à niveau avec liaison des bases de données SharePoint » est une séquence d’étapes permettant de sauvegarder, copier puis restaurer les bases de données de contenu et d’application de service associées à [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans la batterie de serveurs SharePoint 2013.  
  
1.  **Définir la base de données en lecture seule :** dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], cliquez avec le bouton droit sur le nom de la base de données, puis cliquez sur **Propriétés**. Dans la page **Options** , affectez à la propriété **Base de données en lecture seule** la valeur **True**.  
  
2.  **Sauvegarde :** sauvegardez chaque base de données de contenu et chaque base de données d'application de service que vous souhaitez migrer vers la batterie de serveurs SharePoint 2013. Dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], cliquez avec le bouton droit sur le nom de la base de données, cliquez sur **Tâches**, puis sur **Sauvegarder**.  
  
3.  Copiez les fichiers de sauvegarde de base de données (.bak) sur le serveur de destination souhaité.  
  
4.  **Restauration :** restaurez les bases de données sur le [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)]de destination. Cette étape peut être effectuée à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
5.  **Définir la base de données en lecture-écriture :** attribuez au paramètre **Base de données en lecture seule** la valeur **False**.  
  
##  <a name="bkmk_prepare_mount_databases"></a>Préparer des Applications Web et les bases de données de contenu de montage  
 Pour obtenir une explication plus détaillée des procédures suivantes, consultez [mise à niveau des bases de données à partir de SharePoint 2010 vers SharePoint 2013](http://go.microsoft.com/fwlink/p/?LinkId=256690) (http://go.microsoft.com/fwlink/p/?LinkId=256690).  
  
1.  **Mettre les bases de données hors connexion :**  
  
     Mettez toutes les bases de données de contenu SharePoint 2013 hors connexion à l'aide de l'Administration centrale de SharePoint. Les bases de données de contenu sont remplacées par les bases de données que vous avez copiées. Déterminez la meilleure séquence pour votre environnement. Envisagez de mettre chaque base de données hors connexion et de monter la base de données de remplacement correspondante avant de mettre la base de données de contenu suivante hors connexion. Une autre possibilité consiste à mettre toutes les bases de données hors connexion par groupe.  
  
    1.  Dans l'Administration centrale de SharePoint, cliquez sur **Gestion des applications**.  
  
    2.  Cliquez sur **Gérer les bases de données de contenu**.  
  
    3.  Cliquez sur le nom de la base de données.  
  
    4.  Dans **Gérer les paramètres de la base de données de contenu**, attribuez à **État de la base de données** la valeur **Hors connexion**.  
  
    5.  Sélectionnez **Supprimer la base de données de contenu**. Prenez note de l'avertissement indiquant que les sites stockés dans la base de données de contenu ne sont plus accessibles.  
  
-   **Monter les bases de données de contenu :**  
  
     Utilisez les applets de commande PowerShell dans le shell de gestion SharePoint 2013 pour monter la base de données de contenu migrée. La base de données n’a pas besoin d’être montée, seules les bases de données contenus : ![contenu relatif à PowerShell](../../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "contenu relatif à PowerShell")  
  
    ```  
    Mount-SPContentDatabase "SharePoint_Content_O14-KJSP1" -DatabaseServer "[server name]\powerpivot" -WebApplication [web application URL]  
    ```  
  
     Pour plus d’informations, consultez [attacher ou détacher des bases de données de contenu (SharePoint Server 2010)](http://technet.microsoft.com/library/ff628582.aspx) (http://technet.microsoft.com/library/ff628582.aspx).  
  
     **État lorsque l'étape est terminée :**  lorsque l'opération de montage est terminée, les utilisateurs peuvent voir les fichiers qui étaient dans l'ancienne base de données de contenu. Par conséquent, les utilisateurs peuvent voir et ouvrir des classeurs dans la bibliothèque de documents.  
  
    -   > [!TIP]  
        >  À ce stade du processus de migration, il est possible de créer des planifications pour les classeurs migrés. Toutefois, les planifications sont créées dans la nouvelle base de données d'application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] , et non dans la base de données copiée depuis l'ancienne batterie de serveurs SharePoint. Par conséquent, il ne contient pas les anciennes planifications. Après avoir terminé les étapes suivantes pour utiliser l'ancienne base de données ou migrer les anciennes planifications, les nouvelles planifications ne sont pas disponibles.  
  
### <a name="troubleshoot-issues-when-you-attempt-to-mount-databases"></a>Résoudre les problèmes lors du montage des bases de données  
 Cette section résume les problèmes que vous pouvez rencontrer lors du montage de la base de données.  
  
1.  **Erreurs d'authentification :** si des erreurs liées à l'authentification s'affichent, vérifiez quel mode d'authentification est utilisé par les applications Web source. L'erreur peut être due à une incohérence entre l'authentification de l'application Web SharePoint 2013 et celle de l'application Web SharePoint 2010. Pour plus d'informations, consultez [1) Préparer la batterie de serveurs SharePoint 2013](#bkmk_prepare_sharepoint2013) .  
  
2.  **Fichiers PowerPivot absents :** si des erreurs liées aux fichiers manquants .dll [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] s’affichent, **spPowerPivot.msi** n’a pas été installé ou l’outil de configuration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] n’a pas été utilisé pour configurer [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)].  
  
##  <a name="bkmk_upgrade_powerpivot_schedules"></a>Mettre à niveau les planifications PowerPivot  
 Cette section décrit les détails et les options de migration des planifications [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . La migration des planifications est un processus en deux étapes. Configurez tout d’abord l’application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour qu’elle utilise la base de données d’application de service migrée. Ensuite, choisissez l'une des deux options pour la migration des planifications.  
  
 **Configurez l'application de service pour qu'elle utilise la base de données d'application de service migrée.**  
  
 Dans l’Administration centrale de SharePoint, configurez l’application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour qu’elle utilise l’ancienne base de données d’application de service que vous avez copiée. Le service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] met à niveau la base de données d’application de service vers le nouveau schéma.  
  
1.  Dans l'Administration centrale de SharePoint, cliquez sur **Gérer les applications de service**.  
  
2.  Recherchez l’application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] , par exemple « Application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] par défaut », cliquez sur le nom de l’application de service, puis sur **Propriétés** dans le ruban SharePoint.  
  
3.  Mettez à jour l'instance nommée du serveur de base de données et le nom de la base de données avec les noms corrects pour la base de données que vous avez sauvegardée, copiée et restaurée. Une fois que vous cliquez sur **OK**, la base de données d'application de service est mise à niveau. Les erreurs figurent dans le journal ULS.  
  
 **Mettre à niveau les planifications [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]**  
  
 Configurez l’application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour migrer les planifications d’actualisation.  
  
-   **Migrer les planifications, option 1 : administrateur de batterie de serveurs SharePoint**  
  
    1.  Dans SharePoint 2013 Management, exécutez le `Set-PowerPivotServiceApplication` applet de commande avec le `-StartMigratingRefreshSchedules` commutateur d’activation automatique lors de la migration de planification de la demande ![contenu relatif à PowerShell](../../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "contenu relatif à PowerShell"). Le script Windows PowerShell suivant suppose qu’il existe une seule application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] .  
  
        ```  
        $app=Get-PowerPivotServiceApplication  
        Set-PowerPivotServiceApplication $app -StartMigratingRefreshSchedules  
        ```  
  
         Une fois le script Windows PowerShell exécuté, les planifications sont actives et seront exécutées au moment opportun. Toutefois, l'état de la page de planification d'actualisation n'est pas activé. Lorsque la planification s'exécute pour la première fois, elle est migrée et **Activé**  s'affiche sur la page de la planification de l'actualisation.  
  
    2.  Si vous souhaitez vérifier la valeur actuelle de la propriété StartMigratingRefreshSchedules, exécutez le script PowerShell suivant. Le script parcourt tous les objets d’application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et affiche le nom et les valeurs des propriétés :  
  
        ```  
        $apps = Get-PowerPivotServiceApplication  
        foreach ($app in $apps){}  
        Get-PowerPivotServiceApplication $appp | format-table -property displayname,id,StartMigratingRefreshSchedules  
        ```  
  
     **Migrer les planifications, option 2 : l'utilisateur met à jour tous les classeurs**  
  
    1.  Une autre possibilité pour migrer les planifications consiste à activer l'actualisation planifiée pour chaque classeur. Naviguez jusqu'à la bibliothèque de documents qui contient les classeurs.  
  
    2.  Ouvrez le menu contextuel et cliquez sur **Gérer l’actualisation des données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]**.  
  
    3.  Dans la section **Actualisation planifiée** , cliquez sur **Activer**.  
  
    4.  Vous pouvez sélectionner **Aussi actualiser dès que possible**. Cette option ajoute une instance de l'actualisation à la file d'attente dès que vous cliquez sur OK. L'actualisation planifiée normale se déclenche toujours au moment opportun.  
  
    5.  Cliquez sur **OK**. L'historique d'actualisation est désormais visible dans la page d'actualisation, la planification se déclenche alors à l'heure normale.  
  
 **Classeurs [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] SQL Server 2008 R2**  
  
-   Classeurs [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] SQL Server 2008 R2 ne sont pas mis à niveau automatiquement lorsqu’ils sont utilisés dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)][!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013. Après avoir migré une base de données de contenu qui contient des classeurs 2008 R2, vous pouvez utiliser les classeurs et les planifications ne sont pas mises à niveau.  
  
-   Pour plus d’informations, consultez [Mettre à niveau les classeurs et l’actualisation planifiée des données &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/upgrade-workbooks-and-scheduled-data-refresh-sharepoint-2013.md).  
  
##  <a name="bkmk_additional_resources"></a> Ressources supplémentaires  
  
> [!NOTE]  
>  Pour plus d'informations sur la mise à niveau avec liaison des bases de données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et SharePoint, consultez les rubriques suivantes :  
  
-   [Mettre à niveau les classeurs et l’actualisation planifiée des données &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/upgrade-workbooks-and-scheduled-data-refresh-sharepoint-2013.md).  
  
-   [Vue d’ensemble de la mise à niveau vers SharePoint 2013](http://go.microsoft.com/fwlink/p/?LinkId=256688) (http://go.microsoft.com/fwlink/p/?LinkId=256688).  
  
-   [Nettoyer les préparations avant une mise à niveau vers SharePoint 2013](http://go.microsoft.com/fwlink/p/?LinkId=256689) (http://go.microsoft.com/fwlink/p/?LinkId=256689).  
  
-   [Mise à niveau des bases de données à partir de SharePoint 2010 vers SharePoint 2013](http://go.microsoft.com/fwlink/p/?LinkId=256690) (http://go.microsoft.com/fwlink/p/?LinkId=256690).  
  
  
