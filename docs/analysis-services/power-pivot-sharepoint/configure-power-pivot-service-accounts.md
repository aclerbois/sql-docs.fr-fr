---
title: Configuration des comptes de Service Power Pivot | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ed619d7a0a4e593193f0ac3f736f059d9826512d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-power-pivot-service-accounts"></a>Configuration des comptes de service Power Pivot
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une installation [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]inclut deux services qui prennent en charge des opérations serveur. Le service **SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])** est un service Windows qui assure le traitement des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et la prise en charge des requêtes sur un serveur d’applications. Le compte de connexion pour ce service est toujours spécifié pendant l'installation de SQL Server lorsque vous installez Analysis Services en mode intégré SharePoint.  
  
 Un deuxième compte doit être spécifié pour l'application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , un service web partagé qui s'exécute sous une identité du pool d'applications dans une batterie de serveurs SharePoint. Ce compte est spécifié quand vous configurez une installation [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]à l'aide de l'outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou de PowerShell.  
  
 Chaque service doit s'exécuter sous un compte dédié afin que vous puissiez auditer les connexions ou activer le protocole d'authentification Kerberos dans la batterie.  
  
 Une fois les comptes de service définis, toute modification apportée à l'un ou l'autre compte doit être effectuée via l'Administration centrale de SharePoint. Si vous utilisez d'autres outils (comme l'application de console Services, le Gestionnaire des services Internet ou le Gestionnaire de configuration SQL Server), les autorisations ne seront pas mises à jour pour l'accès aux bases de données dans la batterie de serveurs ou pour l'accès aux fichiers locaux sur le serveur physique.  
  
 Cette rubrique contient les sections suivantes :  
  
 [Mettre à jour un mot de passe arrivé à expiration pour une instance de SQL Server Analysis Services (Power Pivot)](#bkmk_passwordssas)  
  
 [Mettre à jour un mot de passe arrivé à expiration pour l'application de service Power Pivot](../../analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts.md#bkmk_passwordapp)  
  
 [Modifier le compte sous lequel chaque service s'exécute](#bkmk_newacct)  
  
 [Créer ou modifier le pool d'applications pour une application de service Power Pivot](#bkmk_appPool)  
  
 [Spécifications relatives aux comptes et autorisations](#requirements)  
  
 [Dépannage : accorder des autorisations administratives manuellement](#updatemanually)  
  
 [Dépannage : résoudre les erreurs HTTP 503 dues à des mots de passe expirés pour l'Administration centrale ou le service de l'application Web de SharePoint Foundation](#expired)  
  
##  <a name="bkmk_passwordssas"></a> Mettre à jour un mot de passe arrivé à expiration pour une instance de SQL Server Analysis Services (Power Pivot)  
  
1.  Pointez sur Démarrer, cliquez sur **Outils d'administration**, puis cliquez sur **Services**. Double-cliquez sur **SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])**. Cliquez sur **Ouvrir une session**, puis tapez le nouveau mot de passe pour le compte.  
  
2.  Dans Administration centrale, dans la section Sécurité, cliquez sur **Configurer les comptes gérés**.  
  
3.  Cliquez sur **Modifier** pour modifier un compte spécifique.  
  
4.  Sélectionnez **Modifier le mot de passe maintenant**.  
  
5.  Sélectionnez **Attribuer une nouvelle valeur au mot de passe du compte**. Tous les services qui s'exécutent sous le compte géré utilisent les informations d'identification mises à jour.  
  
##  <a name="bkmk_passwordapp"></a> Mettre à jour un mot de passe arrivé à expiration pour l'application de service Power Pivot  
  
1.  Dans Administration centrale, dans la section Sécurité, cliquez sur **Configurer les comptes gérés**.  
  
2.  Cliquez sur **Modifier** pour modifier un compte spécifique.  
  
3.  Sélectionnez **Modifier le mot de passe maintenant**.  
  
4.  Sélectionnez **Attribuer une nouvelle valeur au mot de passe du compte**. Tous les services qui s'exécutent sous le compte géré utilisent les informations d'identification mises à jour.  
  
##  <a name="bkmk_newacct"></a> Modifier le compte sous lequel chaque service s'exécute  
  
1.  Dans Administration centrale, dans la section Sécurité, cliquez sur **Configurer les comptes de service**.  
  
2.  Sélectionnez **Service Windows - SQL Server Analysis Services** pour modifier le compte de service Analysis Services.  
  
3.  Dans **Sélectionnez un compte pour ce service**, choisissez un compte géré existant ou créez-en un. Le compte doit être un compte d'utilisateur de domaine.  
  
4.  Sélectionnez **Pool d’applications de service - Système des services web SharePoint** pour modifier l’identité du pool d’applications de l’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] par défaut. Selon la manière dont votre installation a été configurée, le service peut s'exécuter sous un pool d'applications de service existant créé pour les services SharePoint. Par défaut, l’outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] inscrit le service en tant qu’application de service **par défaut[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] (Application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])**.  
  
     Si le service a été configuré manuellement par un administrateur SharePoint, il est très probablement doté de son propre pool d'applications de service.  
  
5.  Dans **Sélectionnez un compte pour ce service**, choisissez un compte géré existant ou créez-en un. Le compte doit être un compte d'utilisateur de domaine.  
  
6.  Cliquez sur **OK**.  
  
##  <a name="bkmk_appPool"></a> Créer ou modifier le pool d'applications pour une application de service Power Pivot  
  
1.  Dans l'Administration centrale, sous Gestion des applications, cliquez sur **Gérer les applications de service**.  
  
2.  Sélectionnez l'application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sans cliquer dessus. Un clic sur le nom d'application ouvre le Tableau de bord de gestion [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , qui ne fournit pas de lien vers la page de propriétés qui spécifie le pool d'applications.  Vous pouvez cliquer sur l'espace blanc dans la ligne ou cliquer sur le nom de type pour sélectionner l'application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
3.  Cliquez sur **Propriétés** dans le ruban.  
  
4.  Sélectionnez **Créer un nouveau pool d'applications**. Spécifiez un nom pour le pool d'applications et un compte géré pour son identité.  
  
##  <a name="requirements"></a> Spécifications relatives aux comptes et autorisations  
 Lorsque vous planifiez un déploiement [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, vous devez planifier les comptes de service ci-dessous.  
  
-   Compte de service Analysis Services. Analysis Services traite les requêtes [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et les travaux d'actualisation des données de la batterie de serveurs. Ce compte est toujours spécifié lors de l'installation de SQL Server lorsque vous installez [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint.  
  
-   [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est associée à un service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui fournit l'intégration et l'infrastructure SharePoint pour le traitement des requêtes [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans une batterie de serveurs. Le pool d'applications que vous spécifiez pour une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est l'identité du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Vous pouvez avoir plusieurs applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans une batterie de serveurs. Chaque application que vous créez doit s'exécuter dans son propre pool d'applications.  
  
#### <a name="analysis-services-service-account"></a>Compte de service Analysis Services  
  
|Condition requise| Description|  
|-----------------|-----------------|  
|Spécification relative à la configuration|Ce compte doit être spécifié pendant l’installation de SQL Server dans la page **Analysis Services - Configuration** de l’Assistant Installation (ou dans le paramètre d’installation **ASSVCACCOUNT** dans le cas d’une installation en ligne de commande).<br /><br /> Vous pouvez modifier le nom d'utilisateur ou le mot de passe à l'aide de l'Administration centrale, de PowerShell ou de l'outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . L'utilisation d'autres outils pour modifier les comptes et les mots de passe n'est pas prise en charge.|  
|Spécification relative au compte d'utilisateur de domaine|Ce compte doit être un compte d'utilisateur de domaine Windows. Les comptes d'ordinateur intégrés (tels que Service réseau ou Service local) sont interdits. Le programme d'installation de SQL Server répond à la nécessité d'un compte d'utilisateur de domaine en bloquant l'installation chaque fois qu'un compte d'ordinateur est spécifié.|  
|Spécifications relatives aux autorisations|Ce compte doit être membre du SQLServerMSASUser$\<serveur > $PowerPivot les groupe de sécurité et les groupes de sécurité WSS_WPG sur l’ordinateur local. Ces autorisations doivent être accordées automatiquement. Pour plus d’informations sur la vérification ou l’octroi des autorisations, consultez [Accorder manuellement des autorisations administratives au compte de service Power Pivot](#updatemanually) dans cette rubrique et [Configuration initiale (Power Pivot pour SharePoint)](http://msdn.microsoft.com/en-us/3a0ec2eb-017a-40db-b8d4-8aa8f4cdc146).|  
|Spécifications relatives à la montée en puissance parallèle|Si vous installez plusieurs instances de serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint dans une batterie, toutes les instances de serveur Analysis Services doivent s'exécuter sous le même compte d'utilisateur de domaine. Par exemple, si vous configurez la première instance du [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)] de sorte qu’elle s’exécute sous le nom Contoso\ssas-srv01, toutes les instances du [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)] supplémentaires que vous déployez par la suite dans la même batterie doivent aussi s’exécuter sous le nom Contoso\ssas-srv01.<br /><br /> La configuration de toutes les instances de service de sorte qu'elles s'exécutent sous le même compte permet au service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] d'allouer les travaux de traitement des requêtes ou d'actualisation des données à n'importe quelle instance du service Analysis Services de la batterie de serveurs. Cette configuration permet en outre d'utiliser la fonctionnalité Compte géré de l'Administration centrale pour les instances du serveur Analysis Services. En utilisant le même compte pour toutes les instances du [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)] , vous pouvez modifier le compte ou le mot de passe une seule fois, et toutes les instances de service qui utilisent ces informations d'identification seront automatiquement mises à jour.<br /><br /> Le programme d'installation de SQL Server répond à la nécessité d'un même compte. Dans un déploiement avec montée en puissance parallèle où une instance de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint est déjà installée sur la batterie de serveurs SharePoint, le programme d’installation bloquera la nouvelle installation si vous spécifiez un compte de [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)] différent de celui déjà utilisé dans la batterie.|  
  
#### <a name="power-pivot-service-application-pool"></a>Pool d'applications de service Power Pivot  
  
|Condition requise| Description|  
|-----------------|-----------------|  
|Spécification relative à la configuration|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est une ressource partagée de la batterie de serveurs qui devient disponible lorsque vous créez une application de service. Le pool d'applications de service doit être spécifié lors de la création de l'application de service. Il peut être spécifié de deux façons, soit à l'aide de l'outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , soit par le biais de commandes PowerShell.<br /><br /> Vous pouvez avoir configuré l'identité du pool d'applications de sorte qu'il s'exécute sous un compte unique. Si ce n'est pas le cas, envisagez de le modifier maintenant de manière à l'exécuter sous un autre compte.|  
|Spécification relative au compte d'utilisateur de domaine|L'identité du pool d'applications doit être un compte d'utilisateur de domaine Windows. Les comptes d'ordinateur intégrés (tels que Service réseau ou Service local) sont interdits.|  
|Spécifications relatives aux autorisations|Ce compte n'a pas besoin d'autorisations d'administrateur système local sur l'ordinateur. Ce compte doit cependant avoir des autorisations d'administrateur système Analysis Services sur le [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)] local installé sur le même ordinateur. Ces autorisations sont accordées automatiquement par le programme d'installation de SQL Server, ou lorsque vous définissez ou modifiez l'identité du pool d'applications dans l'Administration centrale.<br /><br /> Les autorisations d'administration sont nécessaires pour l'envoi de requêtes au [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)]. Elles sont également requises pour la surveillance de l'intégrité, la fermeture de sessions inactives et l'écoute des événements de trace.<br /><br /> Le compte doit disposer d'autorisations de connexion, de lecture et d'écriture sur la base de données d'application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Ces autorisations sont accordées automatiquement lors de la création de l'application et sont mises à jour automatiquement lorsque vous modifiez des comptes ou des mots de passe dans l'Administration centrale.<br /><br /> L'application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] vérifie qu'un utilisateur SharePoint est autorisé à afficher des données avant de récupérer le fichier, mais n'emprunte pas l'identité de l'utilisateur. Il n'existe aucune spécification relative à l'emprunt d'identité.|  
|Spécifications relatives à la montée en puissance parallèle|Aucun.|  
  
##  <a name="updatemanually"></a> Dépannage : accorder des autorisations administratives manuellement  
 Les autorisations administratives ne peuvent être mises à jour si la personne qui met à jour les informations d'identification n'est pas administrateur local sur l'ordinateur. Vous pouvez alors accorder des autorisations administratives manuellement. Pour ce faire, le plus simple est d'exécuter le travail du minuteur de Configuration [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans l'Administration centrale. Avec cette approche, vous pouvez réinitialiser des autorisations pour tous les serveurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de la batterie. Notez que cette approche fonctionne uniquement si le travail du minuteur SharePoint s'exécute à la fois comme administrateur de batterie et comme administrateur local sur l'ordinateur.  
  
1.  Dans Supervision, cliquez sur **Examiner les définitions de travail**.  
  
2.  Sélectionnez **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Travail du minuteur de configuration**.  
  
3.  Cliquez sur **Exécuter maintenant**.  
  
 En dernier recours, vous pouvez garantir des autorisations appropriées en accordant des autorisations d’Administration système Analysis Services pour le [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] application de service, puis ajoutez spécifiquement l’identité d’application de service à SQLServerMSASUser$\<nom_serveur > groupe de sécurité $PowerPivot Windows. Vous devez répéter ces étapes pour chaque instance Analysis Services intégrée avec la batterie de serveurs SharePoint.  
  
 Vous devez être administrateur local pour mettre à jour des groupes de sécurité Windows.  
  
1.  Dans SQL Server Management Studio, connectez-vous à l’instance d’Analysis Services en tant que \<nom du serveur > \POWERPIVOT.  
  
2.  Cliquez avec le bouton droit sur le nom du serveur, puis sélectionnez **Propriétés**.  
  
3.  Cliquez sur **Sécurité**.  
  
4.  Cliquez sur **Ajouter**.  
  
5.  Tapez le nom du compte utilisé pour le pool d'applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , puis cliquez sur **OK**.  
  
6.  Dans Outils d'administration, cliquez sur **Gestion de l'ordinateur**.  
  
7.  Ouvrez **Utilisateurs et groupes locaux**.  
  
8.  Ouvrez **Groupes**.  
  
9. Double-cliquez sur SQLServerMSASUser$\<nom_serveur > $PowerPivot.  
  
10. Cliquez sur **Ajouter**.  
  
11. Tapez le nom du compte utilisé pour le pool d'applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , puis cliquez sur **OK**.  
  
##  <a name="expired"></a> Dépannage : résoudre les erreurs HTTP 503 dues à des mots de passe expirés pour l'Administration centrale ou le service de l'application Web de SharePoint Foundation  
 Si le service de l'Administration centrale ou le service Application Web de SharePoint Foundation cesse de fonctionner en raison de la réinitialisation du compte ou de l'expiration du mot de passe, des messages d'erreur HTTP 503 « Service non disponible » s'affichent lors de la tentative d'ouverture de l'Administration centrale de SharePoint ou d'un site SharePoint. Suivez ces étapes pour remettre votre serveur en ligne. Une fois l'Administration centrale disponible, vous pouvez passer à la mise à jour des informations de compte périmées.  
  
1.  Dans les outils d'administration, cliquez sur **Gestionnaire des services IIS**.  
  
2.  Si l'identité du site ou le pool d'applications de l'Administration centrale est un compte d'utilisateur de domaine dont le mot de passe a expiré, procédez comme suit :  
  
    1.  Cliquez avec le bouton droit sur le nom du pool d’applications et sélectionnez **Paramètres avancés**.  
  
    2.  Sélectionnez **Identité** et cliquez sur le bouton représentant une ellipse (...) pour ouvrir la boîte de dialogue Identité du pool d'applications.  
  
    3.  Cliquez sur **Définir**.  
  
    4.  Tapez le nom d'utilisateur et le mot de passe.  
  
3.  Exécutez IISRESET. Pour ce faire, ouvrez une invite de commandes d'administrateur et tapez **iisreset** à la commande.  
  
4.  Dans Administration centrale de SharePoint, dans Sécurité, sélectionnez **Configurer les comptes gérés**.  
  
5.  Cliquez sur **Modifier** pour mettre à jour les informations du compte géré dont le mot de passe a expiré.  
  
6.  Sélectionnez **Modifier le mot de passe maintenant**.  
  
7.  Cliquez sur **Utiliser le mot de passe existant**.  
  
8.  Entrez le mot de passe, puis cliquez sur **OK**.  
  
 Si Reporting Services est installé, utilisez le Gestionnaire de configuration de Reporting Services pour mettre à jour les mots de passe pour le serveur de rapports et la connexion à la base de données du serveur de rapports. Pour plus d’informations, consultez [Configuration et administration d’un serveur de rapports &#40;mode SharePoint de Reporting Services&#41;](../../reporting-services/report-server-sharepoint/configuration-and-administration-of-a-report-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer ou arrêter un serveur PowerPivot pour SharePoint](../../analysis-services/power-pivot-sharepoint/start-or-stop-a-power-pivot-for-sharepoint-server.md)   
 [Configurer le compte d’actualisation des données PowerPivot sans assistance (PowerPivot pour SharePoint)](http://msdn.microsoft.com/en-us/81401eac-c619-4fad-ad3e-599e7a6f8493)  
  
  
