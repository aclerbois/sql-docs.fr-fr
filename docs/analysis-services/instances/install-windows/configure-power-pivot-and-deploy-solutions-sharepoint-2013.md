---
title: Configurer PowerPivot et déployer des Solutions (SharePoint 2013) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 93374b25f377265f1eafa09cf46714ed79927243
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-power-pivot-and-deploy-solutions-sharepoint-2013"></a>Configurer PowerPivot et déployer des solutions (SharePoint 2013)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Cette rubrique décrit le déploiement et la configuration des améliorations intermédiaires apportées aux fonctionnalités [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans [!INCLUDE[SPS2013](../../../includes/sps2013-md.md)] , dont la Galerie [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] , l’actualisation des données de planification, le tableau de bord de gestion et les fournisseurs de données. Exécutez l’outil **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013** pour effectuer les opérations suivantes :  
  
-   Déployer les fichiers de solution SharePoint.  
  
-   Créer une application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] .  
  
-   Configurer une application Excel Services pour utiliser un serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode SharePoint. Pour plus d’informations sur les services principaux et l’installation d’un serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode SharePoint, consultez [Installer Analysis Services en mode Power Pivot](../../../analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode.md).  
  
 Pour plus d’informations sur l’installation de l’outil de configuration de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013, consultez [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013.md).  
  
##  <a name="bkmk_run_configuration_tool"></a> Exécuter Configuration de PowerPivot pour SharePoint 2013  
 **Remarque :** l'Assistant Installation de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] installe deux outils de configuration différents pour [!INCLUDE[ssGeminiLong](../../../includes/ssgeminilong-md.md)]. Chacun d'eux prend en charge une version différente de SharePoint.  
  
|Nom| Description|  
|----------|-----------------|  
|[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013|SharePoint 2013|  
|[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] Outil de configuration|SharePoint 2010 avec SharePoint 2010 Service Pack 1 (SP1)|  
  
 **Remarque :** pour effectuer les étapes suivantes, vous devez être administrateur de batterie de serveurs. Si un message d'erreur semblable au suivant s'affiche :  
  
-   « L'utilisateur n'est pas un administrateur de batterie de serveurs. Traitez les échecs de validation et réessayez. »  
  
 Connectez-vous avec le compte qui a installé SharePoint ou configurez le compte d'installation en tant qu'administrateur principal du site Administration centrale de SharePoint.  
  
1.  Dans le menu **Démarrer** , cliquez sur **Tous les programmes**, [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)], **Outils de configuration**puis **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013**. l’outil n’est disponible que si [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint est installé sur le serveur local.  
  
2.  Cliquez sur **Configurer ou réparer [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoin**, puis sur **OK**.  
  
3.  l’outil vérifie l’état actuel de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et exécute les étapes nécessaires pour terminer la configuration. Affichez la fenêtre en plein écran. Vous devez voir une barre d'icônes en bas de la fenêtre qui comprend les commandes **Valider**, **Exécuter**et **Quitter** .  
  
4.  Dans l'onglet **Paramètres** :  
  
    1.  **Nom d'utilisateur de compte par défaut**: entrez un compte d'utilisateur de domaine pour le compte par défaut. Ce compte servira à approvisionner des services, notamment le pool d’applications du service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . Ne spécifiez pas un compte intégré tel que Service réseau ou Système local. L'outil bloque les configurations qui spécifient des comptes intégrés.  
  
    2.  **Serveur de base de données**: vous pouvez utiliser le moteur de base de données SQL Server pris en charge pour la batterie de serveurs SharePoint.  
  
    3.  **Phrase secrète**: entrez une phrase secrète. Si vous créez une batterie de serveurs SharePoint, la phrase secrète est utilisée chaque fois que vous ajoutez un serveur ou une application à la batterie de serveurs SharePoint. Si la batterie existe déjà, entrez la phrase secrète qui vous permet d'ajouter une application de serveur à la batterie.  
  
    4.  **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour services Excel**: entrez le nom d’un serveur en mode SharePoint pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] . Dans un déploiement sur un seul serveur, il est identique à celui du serveur de base de données. `[ServerName]\powerpivot`  
  
    5.  Cliquez sur **Créer une collection de sites** dans la fenêtre de gauche. Notez l' **URL du site** afin de pouvoir y faire référence dans les étapes ultérieures. Si le serveur SharePoint n'est pas déjà configuré, l'Assistant de configuration définit par défaut l'application Web et les URL de collection de sites sur la racine de `http://[ServerName]`. Pour modifier les valeurs par défaut, examinez les pages suivantes dans la fenêtre de gauche : **Créer une application Web par défaut** et **Déployer une solution d'application Web**.  
  
5.  Éventuellement, examinez les valeurs d'entrée restantes utilisées pour effectuer chaque action. Cliquez sur chaque action dans la fenêtre à gauche pour afficher et passer en revue les détails de l'action. Pour plus d’informations sur chacune d’elles, consultez la section Valeurs d’entrée utilisées pour configurer le serveur dans [Configurer ou réparer PowerPivot pour SharePoint 2010 (outil de configuration de PowerPivot)](http://msdn.microsoft.com/en-us/d61f49c5-efaa-4455-98f2-8c293fa50046) dans cette rubrique.  
  
6.  Éventuellement, supprimez toutes les actions que vous ne souhaitez pas traiter à ce stade. Par exemple, si vous souhaitez configurer le service Banque d'informations sécurisé ultérieurement, cliquez sur **Configurer le service Banque d'informations sécurisé**, puis désactivez la case à cocher **Inclure cette action dans la liste des tâches**.  
  
7.  Cliquez sur **Valider** pour vérifier que l'outil dispose d'informations suffisantes pour traiter les actions de la liste. Si des erreurs de validation s'affichent, cliquez sur les avertissements dans le volet gauche pour afficher les détails de l'erreur de validation. Corrigez toutes les erreurs de validation, puis cliquez de nouveau sur **Valider** .  
  
8.  Cliquez sur **Exécuter** pour traiter toutes les actions de la liste des tâches. Notez qu' **Exécuter** devient disponible lorsque vous avez validé les actions. Si **Exécuter** n'est pas activé, commencez par cliquer sur **Valider** .  
  
 Pour plus d’informations, consultez [Configurer ou réparer Power Pivot pour SharePoint 2010 (outil de configuration de Power Pivot)](http://msdn.microsoft.com/en-us/d61f49c5-efaa-4455-98f2-8c293fa50046).  
  
##  <a name="bkmk_verify_powerpivot"></a> Vérifier la configuration de PowerPivot  
 **Services :**  
  
1.  Dans l'Administration centrale, sous Paramètres système, cliquez sur **Gérer les services sur le serveur**.  
  
2.  Vérifiez que **SQL Server Analysis Services** et le **Service système [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] SQL Server** sont démarrés.  
  
 **Fonctionnalités de la batterie de serveurs :**  
  
1.  Dans l'Administration centrale, sous Paramètres système, cliquez sur **Gérer les fonctionnalités des batteries de serveurs**.  
  
2.  Vérifiez que l’option **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] Fonctionnalité d’intégration** a la valeur **Active**.  
  
 **Fonctionnalités de la collection de sites :**  
  
1.  Accédez à l'URL du site créé par l'outil de configuration.  
  
     Cliquez sur **paramètres**![paramètres SharePoint](../../../analysis-services/media/as-sharepoint2013-settings-gear.gif "paramètres SharePoint"), puis cliquez sur **paramètres du Site**.  
  
     Cliquez sur **Fonctionnalités de la collection de sites**.  
  
2.  Vérifiez que l’option **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour les collections de sites** a la valeur **Active**.  
  
 **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] :**  
  
1.  Dans l'Administration centrale, sous **Gestion des applications**, cliquez sur **Gérer les applications de service**.  
  
2.  Vérifiez que l'état de l'application de service est **Démarré**. Le nom par défaut est **Application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] par défaut**.  
  
     Cliquez sur le nom de l’application de service pour ouvrir son Tableau de bord de gestion [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . À la première utilisation, le tableau de bord prend plusieurs minutes à charger.  
  
 Pour plus d’informations, consultez [Verify a Power Pivot for SharePoint Installation](../../../analysis-services/instances/install-windows/verify-a-power-pivot-for-sharepoint-installation.md).  
  
##  <a name="bkmk_troubleshoot_issues"></a> Résoudre les problèmes  
 Pour aider les problèmes de dépannage, il peut être judicieux de vérifier que la journalisation du diagnostic est activée.  
  
1.  Dans l'Administration centrale de SharePoint, cliquez sur **Analyse** , puis sur **Configurer la collecte des données d'utilisation et d'intégrité**.  
  
2.  Vérifiez que l'option **Activer la collecte des données d'utilisation** est sélectionnée.  
  
3.  Vérifiez que les événements suivants sont sélectionnés :  
  
    -   Définition des champs d'utilisation de la télémesure d'éducation  
  
    -   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] Connexions  
  
    -   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] Utilisation des données de chargement  
  
    -   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] Utilisation des requêtes  
  
    -   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] Utilisation des données de déchargement  
  
4.  Vérifiez que l'option **Activer la collecte des données d'intégrité** est sélectionnée.  
  
5.  Cliquez sur **OK**.  
  
 Pour plus d’informations sur l’actualisation des données de dépannage, consultez [dépannage actualisation des données PowerPivot](http://social.technet.microsoft.com/wiki/contents/articles/3870.troubleshooting-powerpivot-data-refresh.aspx) (http://social.technet.microsoft.com/wiki/contents/articles/3870.troubleshooting-powerpivot-data-refresh.aspx).  
  
 Pour plus d'informations sur l'outil de configuration, consultez [Power Pivot Configuration Tools](../../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools.md).  
  
  
