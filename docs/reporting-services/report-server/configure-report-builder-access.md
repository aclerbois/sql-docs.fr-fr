---
title: Configurer l’accès au Générateur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, Report Builder
- Report Builder 1.0, configuring access
- configuring servers [Reporting Services]
ms.assetid: a79003d0-c905-4d4c-9560-93a7cc1e1dd4
caps.latest.revision: 47
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 50e05be6e01c09f552fae6cf5df3d394de1cf28c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-report-builder-access"></a>Configurer l'accès au Générateur de rapports
  Le Générateur de rapports est un outil de génération d’états ad hoc qui s’installe avec un serveur de rapports [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configuré pour le mode natif ou pour le mode intégré SharePoint.  
  
 L'accès au Générateur de rapports dépend des facteurs suivants :  
  
-   Propriétés de serveur déterminant si le Générateur de rapports est disponible ou non sur le serveur de rapports.  
  
-   Attributions ou autorisations de rôles permettant de rendre le Générateur de rapports disponible pour des utilisateurs individuels ou des groupes.  
  
-   Paramètres d'authentification qui déterminent si les informations d'identification de l'utilisateur peuvent être transmises au serveur de rapports ou si l'accès anonyme est configuré sur les fichiers d'application.  
  
 Pour utiliser le Générateur de rapports, vous devez disposer d'un modèle de rapport publié avec lequel travailler.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Le Générateur de rapports n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 2.0 doit être installé sur l’ordinateur client. Le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] fournit l’infrastructure permettant d’exécuter les applications [!INCLUDE[ndptecclick](../../includes/ndptecclick-md.md)] .  
  
 Vous devez utiliser [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Explorer 6.0 ou version ultérieure.  
  
 Le Générateur de rapports s'exécute toujours en confiance totale ; vous ne pouvez pas le configurer pour qu'il s'exécute en confiance partielle. Dans les versions antérieures, il était possible d'exécuter le Générateur de rapports en mode de confiance partielle, mais cette option n'est pas prise en charge dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures.  
  
## <a name="enabling-and-disabling-report-builder"></a>Activation et désactivation du Générateur de rapports  
 Le Générateur de rapports est activé par défaut. Les administrateurs du serveur de rapports peuvent désactiver la fonctionnalité du Générateur de rapports en définissant la propriété système **EnableReportDesignClientDownload** de ce serveur sur la valeur **false**. Les téléchargements du Générateur de rapports effectués pour ce serveur de rapports sont ainsi désactivés.  
  
 Pour définir les propriétés système du serveur de rapports, vous pouvez utiliser Management Studio ou un script :  
  
-   Pour utiliser Management Studio, connectez-vous au serveur de rapports et utilisez la page de propriétés avancées du serveur pour définir **EnableReportDesignClientDownload** sur **false**. Pour plus d’informations sur l’ouverture de cette page, consultez [Définir les propriétés du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md).  
  
-   Pour voir un exemple de script qui définit une propriété du serveur de rapports, consultez [Écrire des scripts pour les tâches d’administration et de déploiement](../../reporting-services/tools/script-deployment-and-administrative-tasks.md).  
  
## <a name="role-assignments-granting-report-builder-access-on-a-native-mode-report-server"></a>Attributions de rôles qui octroient l'accès au Générateur de rapports sur un serveur de rapports en mode natif  
 Sur un serveur de rapports en mode natif, créez des attributions de rôles d'utilisateur qui incluent des tâches pour l'utilisation du Générateur de rapports. Vous devez être Gestionnaire de contenu et Administrateur système pour créer ou modifier des définitions de rôles et des attributions de rôles sur les éléments et au niveau du site.  
  
 Les instructions suivantes supposent que vous utilisez des rôles prédéfinis. Si vous avez modifié les définitions de rôle ou si vous avez effectué une mise à niveau de SQL Server 2000, examinez les rôles pour vérifier s'ils contiennent les tâches nécessaires. Pour plus d’informations sur la création d’attributions de rôles, consultez [Accorder à un utilisateur l’accès à un serveur de rapports &#40;Gestionnaire de rapports&#41;](../../reporting-services/security/grant-user-access-to-a-report-server-report-manager.md).  
  
 Une fois les attributions de rôles créées, les utilisateurs seront autorisés à effectuer les opérations suivantes :  
  
-   Les utilisateurs affectés aux rôles Utilisateur système et Visiteur peuvent afficher des rapports publiés du Générateur de rapports sur un serveur de rapports, sans qu'il soit nécessaire de lancer le Générateur de rapports.  
  
-   Les utilisateurs affectés aux rôles Utilisateur système et Générateur de rapports peuvent générer des modèles, démarrer le Générateur de rapports et créer des rapports, ainsi qu'enregistrer des rapports sur le serveur de rapports.  
  
-   Les utilisateurs attribués aux rôles Utilisateur système et Serveur de publication peuvent publier des modèles du Générateur de modèles sur le serveur de rapports. Les modèles sont utilisés comme des sources de données dans le Générateur de rapports.  
  
-   Les utilisateurs attribués aux rôles Administrateur système et Gestionnaire de contenu disposent de toutes les autorisations nécessaires pour créer, afficher et gérer les rapports du Générateur de rapports.  
  
#### <a name="to-verify-required-tasks-are-in-the-role-definitions"></a>Pour vérifier que des tâches requises se trouvent dans les définitions de rôles  
  
1.  Démarrez Management Studio et connectez-vous au serveur de rapports.  
  
2.  Ouvrez le dossier **Sécurité** .  
  
3.  Ouvrez le dossier **Rôles système** .  
  
4.  Cliquez avec le bouton droit sur **Administrateur système**, puis sélectionnez **Propriétés**.  
  
5.  Sélectionnez **Exécuter les définitions de rapport** , puis cliquez sur **OK**.  
  
6.  Cliquez avec le bouton droit sur **Utilisateur système**, puis sélectionnez **Propriétés**.  
  
7.  Sélectionnez **Exécuter les définitions de rapport** , puis cliquez sur **OK**.  
  
8.  Ouvrez le dossier **Rôles** .  
  
9. Cliquez avec le bouton droit sur **Navigateur**, puis sélectionnez **Propriétés**.  
  
10. Sélectionnez **Afficher les modèles** , puis cliquez sur **OK**.  
  
11. Cliquez avec le bouton droit sur **Gestionnaire de contenu**, puis sélectionnez **Propriétés**.  
  
12. Sélectionnez **Afficher les modèles**, **Gérer les modèles**, **Lire les rapports**, puis cliquez sur **OK**.  
  
13. Cliquez avec le bouton droit sur **Serveur de publication**, puis sélectionnez **Propriétés**.  
  
14. Sélectionnez **Gérer les modèles** , puis cliquez sur **OK**.  
  
15. Créez le rôle Générateur de rapports s’il n'existe pas :  
  
    1.  Ouvrez le dossier **Sécurité** .  
  
    2.  Cliquez avec le bouton droit sur le dossier **Rôles**, puis sélectionnez **Nouveau rôle**.  
  
    3.  Dans Nom, tapez **Générateur de rapports**.  
  
    4.  Dans Description, entrez une description du rôle afin que les utilisateurs du Gestionnaire de rapports connaissent la fonction du rôle.  
  
    5.  Ajoutez les tâches suivantes : **Lire les rapports**, **Afficher les rapports**, **Afficher les modèles**, **Afficher les ressources**, **Afficher les dossiers**et **Gérer les abonnements individuels**.  
  
    6.  Cliquez sur **OK** pour enregistrer le rôle.  
  
#### <a name="to-create-role-assignments-that-grant-access-to-report-builder"></a>Pour créer des attributions de rôles qui octroient l'accès au Générateur de rapports  
  
1.  Démarrez le Gestionnaire de rapports.  
  
2.  Cliquez sur **Paramètres du site**.  
  
3.  Cliquez sur **Sécurité**.  
  
4.  Si une attribution de rôle existe déjà pour l’utilisateur ou le groupe pour lequel vous voulez configurer l’accès au Générateur de rapports, cliquez sur **Modifier**.  
  
     Sinon, cliquez sur **Nouvelle attribution de rôle**. Dans Groupe ou utilisateur, entrez un compte d’utilisateur ou de groupe d’un domaine Windows au format suivant : \<domaine>\\<compte\>. Si vous utilisez l'authentification par formulaires ou la sécurité personnalisée, spécifiez le compte d'utilisateur ou de groupe en respectant le format approprié pour votre déploiement.  
  
5.  Sélectionnez **Utilisateur système**, puis cliquez sur **OK**.  
  
6.  Cliquez sur **Accueil**.  
  
7.  Cliquez sur l’onglet **Paramètres du dossier** .  
  
8.  Cliquez sur l’onglet **Sécurité** .  
  
9. Si une attribution de rôle existe déjà pour l’utilisateur ou le groupe pour lequel vous voulez configurer l’accès au Générateur de rapports, cliquez sur **Modifier**.  
  
     Sinon, cliquez sur **Nouvelle attribution de rôle**. Dans Groupe ou utilisateur, entrez un compte d’utilisateur ou de groupe d’un domaine Windows au format suivant : \<domaine>\\<compte\>. Si vous utilisez l'authentification par formulaires ou la sécurité personnalisée, spécifiez le compte d'utilisateur ou de groupe en respectant le format approprié pour votre déploiement.  
  
10. Sélectionnez **Générateur de rapports**, puis cliquez sur **Appliquer**.  
  
11. Répétez ces étapes pour créer ou modifier des attributions de rôles pour des utilisateurs ou des groupes supplémentaires.  
  
## <a name="permissions-granting-report-builder-access-on-a-sharepoint-integrated-mode-report-server"></a>Autorisations octroyant l'accès au Générateur de rapports sur un serveur de rapports en mode intégré SharePoint  
 Sur un serveur de rapports en mode intégré SharePoint, l'accès au Générateur de rapports est octroyé aux utilisateurs SharePoint qui disposent de niveaux d'autorisation Collaboration ou Contrôle total.  
  
 Si vous utilisez des niveaux d'autorisation personnalisés, vous devez inclure Ajouter les éléments et Modifier les éléments dans le niveau d'autorisation. Pour plus d’informations sur l’accès au Générateur de rapports par l’intermédiaire de niveaux d’autorisation prédéfinis, consultez [Utiliser la sécurité intégrée dans Windows SharePoint Services pour les éléments de serveur de rapports](../../reporting-services/security/use-built-in-security-in-windows-sharepoint-services-for-report-server-items.md). Pour plus d’informations sur les conditions requises pour les niveaux d’autorisation personnalisés, consultez [Définir des autorisations pour les opérations de serveur de rapports dans une application web SharePoint](../../reporting-services/security/set-permissions-for-report-server-operations-in-a-sharepoint-web-application.md).  
  
## <a name="authentication-considerations-and-credential-reuse"></a>Considérations relatives à l'authentification et réutilisation des informations d'identification  
 Le Générateur de rapports utilise la technologie ClickOnce pour télécharger et installer ses fichiers d'application sur un ordinateur client. La technologie ClickOnce est destinée au déploiement unidirectionnel d'applications qui place les fichiers programme sur un ordinateur client et exécute l'application en tant que processus distinct sous l'identité de l'utilisateur par défaut. Étant donné que le Générateur de rapports doit se reconnecter au serveur de rapports pour obtenir les fichiers d'application et des données du serveur de rapports, il est important de comprendre comment ClickOnce définit le contexte de sécurité et émet des demandes aux ordinateurs distants sous différents scénarios :  
  
-   ClickOnce s'exécute toujours en tant que processus distinct sur l'ordinateur client. L'identité du processus se compose des informations d'identification de l'utilisateur Windows par défaut. ClickOnce ne partage pas de données de session avec Internet Explorer ou n'obtient pas d'Internet Explorer le contexte de sécurité de l'utilisateur actuel.  
  
-   ClickOnce envoie des demandes qui spécifient la sécurité intégrée de Windows dans l'en-tête d'authentification. Si un serveur est configuré pour un type d'authentification différent, le serveur fera échouer les demandes de ClickOnce avec une erreur d'authentification. Pour contourner ce problème, vous devez configurer un serveur pour la sécurité intégrée de Windows ou activer l'accès anonyme pour éliminer le contrôle d'authentification.  
  
-   Le Générateur de rapports ouvre sa propre connexion à un serveur de rapports. Si vous n'utilisez pas la sécurité intégrée de Windows avec une authentification unique, les utilisateurs doivent retaper leurs informations d'identification pour la connexion du Générateur de rapports au serveur de rapports.  
  
 Le tableau suivant décrit les types d'authentifications pris en charge par le serveur de rapports, et indique si une configuration supplémentaire est requise pour accéder au Générateur de rapports.  
  
|Type d'authentification du serveur de rapports|Comment le Générateur de rapports et le lanceur d'applications ClickOnce répondent|  
|---------------------------------------|--------------------------------------------------------------------|  
|Negotiate (par défaut)<br /><br /> NTLM (par défaut)|Sous la sécurité intégrée de Windows, les demandes authentifiées émanant de ClickOnce et du Générateur de rapports réussissent généralement si le client et le serveur sont déployés dans le même domaine, si l'utilisateur est connecté à l'ordinateur client à l'aide d'un compte de domaine bénéficiant d'une autorisation d'accès au Générateur de rapports et si le serveur de rapports est configuré pour l'authentification Windows.<br /><br /> Les demandes réussissent car ClickOnce et la connexion du navigateur au serveur de rapports ont la même identité d'utilisateur.<br /><br /> Les demandes échoueront si l'utilisateur a ouvert Internet Explorer avec Exécuter en tant que et a spécifié des informations d'identification autres que les informations d'identification par défaut. Si la session utilisateur sur le serveur de rapports est établie sous un compte spécifique, et que ClickOnce s'exécute sous un compte différent, le serveur de rapports refusera l'accès aux fichiers.|  
|Kerberos|Internet Explorer, qui est requis pour utiliser le Générateur de rapports, ne prend pas directement en charge Kerberos.|  
|Authentification de base|ClickOnce ne prend pas en charge l'authentification de base. Il ne formulera pas de demandes qui spécifient l'authentification de base dans l'en-tête d'authentification. Il ne passera pas d'informations d'identification ou n'invitera pas l'utilisateur pour les fournir. Vous pouvez contourner ces problèmes en activant l'accès anonyme aux fichiers d'application du Générateur de rapports.<br /><br /> Les demandes réussiront si vous activez l'accès anonyme aux fichiers d'application du Générateur de rapports car le serveur de rapports ignore l'en-tête d'authentification. Pour plus d’informations sur l’activation de l’accès anonyme au Générateur de rapports, consultez [Configurer une authentification de base sur le serveur de rapports](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md).<br /><br /> Une fois que ClickOnce a extrait les fichiers d'application, le Générateur de rapports ouvre une connexion distincte à un serveur de rapports. Les utilisateurs doivent retaper leurs informations d'identification pour que le Générateur de rapports se connecte au serveur de rapports. Le Générateur de rapports ne collecte pas les informations d'identification à partir d'Internet Explorer ou de ClickOnce.<br /><br /> Les demandes échoueront si le serveur de rapports est configuré pour l'authentification de base alors que vous n'avez pas activé l'accès anonyme aux fichiers programme du Générateur de rapports. La demande échoue car ClickOnce spécifie la sécurité intégrée de Windows sur ses demandes. Si vous configurez le serveur de rapports pour l'authentification de base, le serveur rejette la demande car elle spécifie un package de sécurité non valide et il lui manque les informations d'identification que le serveur de rapports attend.<br /><br /> En outre, si le serveur de rapports est configuré pour utiliser le mode intégré SharePoint et que le site SharePoint utilise l'authentification de base, une erreur 401 se produira lorsque les utilisateurs tenteront d'utiliser ClickOnce pour installer le Générateur de rapports sur leurs ordinateurs clients. Ceci est dû au fait que SharePoint utilise un cookie pour conserver l’authentification de l’utilisateur pour toute la durée de la session, mais que ClickOnce ne prend pas en charge ce cookie. Lorsqu'un utilisateur lance une application ClickOnce, telle que le Générateur de rapports, l'application ne passe pas le cookie à SharePoint et donc SharePoint refuse l'accès et renvoie une erreur 401.<br /><br /> Vous pouvez contourner ce problème en essayant l’une des méthodes suivantes :<br /><br /> -Sélectionnez l’option **Mémoriser mon mot de passe** quand vous fournissez vos informations d’authentification utilisateur.<br /><br /> -Activez l’accès anonyme à la collection de sites SharePoint.<br /><br /> -Configurez l’environnement afin que l’utilisateur n’ait pas à fournir d’informations d’identification. Par exemple, dans un environnement intranet, vous pouvez configurer le serveur SharePoint pour qu’il appartienne à un groupe de travail, puis créer des comptes d’utilisateur sur l'ordinateur local.|  
|Custom|Lorsque vous configurez un serveur de rapports pour utiliser l'authentification personnalisée, l'accès anonyme est activé sur le serveur de rapports et les demandes sont acceptées sans contrôle d'authentification.<br /><br /> Une fois que ClickOnce a extrait les fichiers d'application, le Générateur de rapports ouvre une connexion distincte à un serveur de rapports. Les utilisateurs doivent retaper leurs informations d'identification pour que le Générateur de rapports se connecte au serveur de rapports. Le Générateur de rapports ne collecte pas les informations d'identification à partir d'Internet Explorer ou de ClickOnce.|  
  
## <a name="see-also"></a> Voir aussi  
 [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Planification de la prise en charge des navigateurs pour Reporting Services et Power View (Reporting Services 2014)](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)   
 [Démarrer le Générateur de rapports](../../reporting-services/report-builder/start-report-builder.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Propriétés système du serveur de rapports](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md)  
  
  
