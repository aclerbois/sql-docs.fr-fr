---
title: Créer et gérer des abonnements pour les serveurs de rapports en mode natif | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- standard subscriptions [Reporting Services]
- subscriptions [Reporting Services], standard
ms.assetid: 5ab1c661-9bfa-434a-b315-faac34ed12b1
caps.latest.revision: 52
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 312d082d3126bd3f0365b674177b59d32e9b613b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-and-manage-subscriptions-for-native-mode-report-servers"></a>Créer et gérer des abonnements pour les serveurs de rapports en mode natif
  L'abonnement standard est un abonnement créé par des utilisateurs individuels qui souhaitent recevoir des rapports par messagerie électronique ou dans un dossier partagé. Cette rubrique fournit des informations sur les abonnements standard qui sont créés et gérés par des utilisateurs individuels. Les abonnements pilotés par les données ne fonctionnent pas de la même façon et sont décrits dans une rubrique distincte. Pour plus d’informations, consultez [Créer, modifier ou supprimer des abonnement pilotés par les données](../../reporting-services/subscriptions/create-modify-and-delete-data-driven-subscriptions.md).  
  
 **Dans cette rubrique :**  
  
-   [Spécifications générales pour les abonnements](#bkmk_create_subscription)  
  
-   [Pour créer un abonnement pour un partage de fichiers](#bkmk_create_fileshare_subscription)  
  
-   [Pour créer un abonnement par courrier électronique](#bkmk_create_email_subscription)  
  
-   [Pour modifier un abonnement](#bkmk_modify_subscription)  
  
-   [Pour supprimer un abonnement](#bkmk_delete_subscription)  
  
##  <a name="bkmk_create_subscription"></a> Spécifications générales pour les abonnements  
 Le contenu de cette rubrique explique comment créer des abonnements sur un serveur de rapports en mode natif à l'aide du Gestionnaire de rapports de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Après avoir défini un abonnement, vous avez la possibilité d'y accéder dans le Gestionnaire de rapports via la page Mes abonnements ou l'onglet **Abonnements** d'un rapport spécifique.  
  
 [Créer et gérer des abonnements pour des serveurs de rapports en mode SharePoint](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-sharepoint-mode-report-servers.md) explique comment utiliser les pages d’application d’un site SharePoint pour vous abonner aux rapports d’un serveur de rapports en mode SharePoint.  
  
-   Pour utiliser la remise par messagerie électronique, le serveur de rapports doit être configuré pour une connexion de passerelle ou de serveur SMTP avant la création de l'abonnement.  
  
-   Si vous souhaitez utiliser la remise par partage de fichiers, le dossier cible doit avoir été défini. Pour plus d’informations, consultez [Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration de SSRS)](http://msdn.microsoft.com/en-us/b838f970-d11a-4239-b164-8d11f4581d83).  
  
 Pour que vous puissiez vous abonner à un rapport, la source de données du rapport doit être configurée pour utiliser des informations d'identification stockées ou aucune information d'identification. Pour plus d’informations, consultez [Stocker les informations d’identification dans une source de données Reporting Services](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md). Si aucune information d'identification n'est utilisée, le bouton **Nouvel abonnement** n'est pas disponible.  
  
 Cette rubrique n'explique pas comment créer un abonnement piloté par les données. Pour obtenir des instructions sur la création d’un abonnement piloté par les données, consultez [Créer un abonnement piloté par les données &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md) ou recherchez la page d’informations relatives à la création d’un abonnement piloté par les données dans l’aide en ligne du Gestionnaire de rapports.  
  
###  <a name="bkmk_create_fileshare_subscription"></a> Pour créer un abonnement pour un partage de fichiers  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
2.  Naviguez jusqu’au rapport auquel vous voulez vous abonner. Cliquez sur le menu Rapport, puis sur **S’abonner**.  
  
     ![Menu rapport](../../reporting-services/subscriptions/media/ssrs-report-menu-report-manager.png "Menu Rapport")  
  
3.  **Description**: entrez une description du rapport, 512 caractères maximum.  
  
4.  **Propriétaire**: la valeur par défaut du champ Propriétaire est l’utilisateur actuel et ne peut pas être modifiée lors de la création de l’abonnement. Toutefois, une fois l’abonnement enregistré, vous pouvez modifier les propriétés d’abonnement, notamment le propriétaire et la description.  
  
5.  **Remis par**: sélectionnez **Partage de fichiers Windows**.  
  
6.  **Nom de fichier**: spécifiez un nom de fichier pour le rapport.  
  
7.  **Ajouter une extension de fichier lorsque le fichier est créé**: cette option ajoute une extension de trois caractères au nom de fichier. L'extension de fichier est déterminée par le format de sortie sélectionné pour le rapport.  
  
8.  **Chemin** : tapez un chemin UNC (Universal Naming Convention) menant à un dossier existant qui doit contenir les rapports (par exemple, \\\\<nom_serveur\>\\<mes_rapports\>). Insérez deux barres obliques inverses au début du chemin d'accès. Ne spécifiez pas de barre oblique de fin.  
  
     ![abonnement pour partage de fichiers](../../reporting-services/subscriptions/media/ssrs-file-share-subscription.png "abonnement pour partage de fichiers")  
  
9. **Format du rendu**: sélectionnez un format de sortie du rapport pour la remise de fichier. Choisissez un format qui correspond à l'application bureautique qui sera utilisée pour ouvrir le rapport. Évitez les formats qui n'effectuent pas le rendu d'un rapport en un seul flux ou qui introduisent une interactivité non prise en charge dans un fichier statique (par exemple le format HTML 4.0).  
  
10. **Informations d’identification**: permet d’utiliser le compte Partage de fichiers ou des informations d'identification Windows spécifiques. L’option **Utiliser le compte Partage de fichiers** est désactivée si votre administrateur de rapports n’a pas configuré de compte de partage de fichiers. Pour plus d’informations, consultez [Paramètres d’abonnement et compte de partage de fichiers &#40;Gestionnaire de configuration&#41;](../../reporting-services/install-windows/subscription-settings-and-a-file-share-account-configuration-manager.md). Dans les zones de texte **Nom d’utilisateur** et **Mot de passe**, indiquez les informations d’identification nécessaires pour accéder au partage de fichiers en respectant le format *\<domaine>*\\*\<nom_utilisateur>* pour le nom d’utilisateur.  
  
11. **Options de remplacement**  
  
    -   Avec**Ne pas remplacer le fichier si une version précédente existe**, vous empêchez la remise d'avoir lieu si un fichier existant est détecté.  
  
    -   Avec**Incrémenter les noms des fichiers au fur et à mesure que des versions plus récentes sont ajoutées**, le serveur de rapports ajoute un numéro au nom du fichier pour le distinguer des fichiers existants qui portent le même nom.  
  
12. **Planifier**: indiquez quand le rapport devra être remis :  
  
    -   Pour planifier une heure de remise, cliquez sur **Lorsque l'exécution planifiée du rapport est terminée** , puis cliquez sur le bouton **Sélectionner une planification** . Une page de planification s'ouvre.  
  
    -   Pour sélectionner une planification partagée prédéfinie qui comporte déjà les informations de date, d'heure et de périodicité que vous souhaitez utiliser, cliquez sur **Suivant une planification partagée**, puis sélectionnez la planification à utiliser.  
  
    -   Pour remettre le rapport quand un instantané de rapport est mis à jour avec une nouvelle version, cliquez sur**Lors de l’actualisation du contenu du rapport**. Si vous vous abonnez à un rapport qui récupère des données à des intervalles planifiés, la planification utilisée pour actualiser les données détermine le moment où votre abonnement sera traité.  
  
        > [!NOTE]  
        >  Cette option n'est disponible que pour les instantanés déjà associés à une planification de mise à jour.  
  
13. Pour les rapports paramétrables, spécifiez les paramètres à utiliser pour le rapport correspondant à cet abonnement. Les paramètres peuvent différer de ceux utilisés pour l'exécution du rapport à la demande ou de ceux mis en œuvre dans d'autres opérations planifiées.  
  
 Le rapport est remis sous forme de fichier statique. Si le rapport comprend des fonctionnalités interactives (par exemple des liens vers des lignes et colonnes supplémentaires), ces fonctionnalités ne sont pas disponibles.  
  
###  <a name="bkmk_create_email_subscription"></a> Pour créer un abonnement par courrier électronique  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
2.  Naviguez jusqu’au rapport auquel vous voulez vous abonner. Cliquez sur le menu Rapport, puis sur **S’abonner**.  
  
     ![Menu rapport](../../reporting-services/subscriptions/media/ssrs-report-menu-report-manager.png "Menu Rapport")  
  
3.  **Description**: entrez une description du rapport, 512 caractères maximum.  
  
4.  **Propriétaire**: la valeur par défaut du champ Propriétaire est l’utilisateur actuel et ne peut pas être modifiée lors de la création de l’abonnement. Toutefois, une fois l’abonnement enregistré, vous pouvez modifier les propriétés d’abonnement, notamment le propriétaire et la description.  
  
5.  **Remis par**: sélectionnez **Messagerie électronique**. Si **Messagerie électronique** n’est pas disponible, votre serveur de rapports n’est pas configuré pour les abonnements par message électronique. Consultez [Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration de SSRS)](http://msdn.microsoft.com/en-us/b838f970-d11a-4239-b164-8d11f4581d83).  
  
6.  **À**: le nom de destinataire figurant dans le champ À : est renseigné automatiquement d’après votre compte d’utilisateur de domaine. Vérifiez que le format est [nom d’utilisateur]@[domaine.com]. Les paramètres de configuration du serveur de rapports déterminent si le champ **À** est renseigné automatiquement d’après votre compte d'utilisateur. Pour plus d’informations sur la modification des paramètres de configuration pour les adresses électroniques, consultez [Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration de SSRS)](http://msdn.microsoft.com/en-us/b838f970-d11a-4239-b164-8d11f4581d83).  
  
    > [!NOTE]  
    >  En fonction de vos autorisations, vous pouvez taper l'adresse de messagerie à laquelle le rapport doit être remis. Si vous spécifiez plusieurs adresses, séparez-les par des points-virgules (;). Vous pouvez aussi taper des adresses e-mail supplémentaires dans les zones de texte **Cc**, **Cci**et **Répondre à** . Pour cela, vous devez disposer de l'autorisation de gestion de tous les abonnements.  
  
7.  **Objet**: a pour valeur par défaut « @ReportName a été exécuté à @ExecutionTime ». Vous pouvez modifier l’objet, mais notez que @ReportName et @ExecutionTime sont les seules variables globales prises en charge dans le champ **Objet**.  
  
8.  Sélectionnez les options de remise comme suit :  
  
    -   **Inclure un rapport**: permet d’incorporer ou de joindre un exemplaire du rapport. Le format du rapport est déterminé par le format de rendu sélectionné. Ne choisissez pas cette option si vous pensez que la taille du rapport peut dépasser la limite définie pour votre système de messagerie.  
  
    -   **Inclure un lien**: sélectionnez cette option pour insérer une URL pointant vers le rapport dans le corps de texte du message électronique.  
  
    > [!NOTE]  
    >  Si vous désactivez ces deux options, seul le texte de notification de la ligne Objet est envoyé.  
  
9. Choisissez un format de rendu dans la zone de liste **Format du rendu** . Cette option est disponible si vous sélectionnez **Inclure un rapport** pour incorporer ou joindre une copie du rapport.  
  
    -   Pour incorporer le rapport au corps du message électronique, sélectionnez **Archive Web**.  
  
    -   Pour envoyer le rapport sous forme de pièce jointe, choisissez le format de rendu de votre choix.  
  
10. Dans la zone de liste **Priorité** , sélectionnez une priorité. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Exchange, ce paramètre définit un indicateur en rapport avec le niveau d’importance du message électronique.  
  
11. Indiquez quand le rapport devra être remis :  
  
    -   Pour planifier une heure de remise, cliquez sur **Lorsque l'exécution planifiée du rapport est terminée** , puis cliquez sur **Sélectionner une planification**. Une page de planification s'ouvre.  
  
    -   Pour sélectionner une planification partagée prédéfinie qui comporte déjà les informations de date, d'heure et de périodicité que vous souhaitez utiliser, cliquez sur **Suivant une planification partagée**, puis sélectionnez la planification à utiliser.  
  
    -   Pour remettre le rapport quand un instantané de rapport est mis à jour avec une nouvelle version, cliquez sur**Lors de l’actualisation du contenu du rapport**. Si vous vous abonnez à un rapport qui récupère des données à des intervalles planifiés, la planification utilisée pour actualiser les données détermine le moment où votre abonnement sera traité.  
  
    > [!NOTE]  
    >  Cette option n'est disponible que pour les instantanés déjà associés à une planification de mise à jour.  
  
12. Pour les rapports paramétrables, spécifiez les paramètres à utiliser pour le rapport correspondant à cet abonnement. Les paramètres spécifiés peuvent différer de ceux utilisés pour exécuter le rapport à la demande ou de ceux mis en œuvre dans les autres opérations planifiées.  
  
##  <a name="bkmk_modify_subscription"></a> Pour modifier un abonnement  
 Vous pouvez modifier un abonnement à tout moment. Lorsque vous modifiez un abonnement au cours de son traitement, les paramètres mis à jour sont utilisés s'ils sont enregistrés dans le serveur de rapports avant que l'extension de remise ne reçoive les données d'abonnement. Dans le cas contraire, les paramètres existants sont utilisés.  
  
 Un utilisateur qui crée un abonnement en est le propriétaire. Chaque utilisateur peut modifier ou supprimer les abonnements qu'il possède. Vous pouvez modifier le propriétaire du rapport à partir de la page des propriétés d’abonnement ou dans le programme. Pour plus d'informations, consultez les documents suivants :  
  
-   [Utiliser PowerShell pour modifier et répertorier les propriétaires d’abonnements Reporting Services, et exécuter un abonnement](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md)  
  
-   <xref:ReportService2010.ReportingService2010.ChangeSubscriptionOwner%2A>  
  
 Pour localiser un abonnement, utilisez la page **Mes abonnements** ou affichez les définitions d'abonnement associées à un rapport. Vous ne pouvez pas rechercher directement les abonnements, pas plus que vous ne pouvez rechercher un abonnement par nom de propriétaire, informations de déclencheur, informations d'état, etc.  
  
 Les abonnements peuvent également être modifiés ou supprimés par les administrateurs des serveurs de rapports.  
  
> [!NOTE]  
>  Un administrateur de serveur de rapports ne peut pas centraliser la gestion de tous les abonnements individuels en cours d'utilisation sur un serveur de rapports donné. Il peut toutefois accéder à chaque abonnement pour le modifier ou le supprimer.  
  
##  <a name="bkmk_delete_subscription"></a> Pour supprimer un abonnement  
 Pour supprimer un abonnement  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
2.  Dans le Gestionnaire de rapports, cliquez sur **Mes abonnements** dans la barre d'outils et parcourez l'arborescence jusqu'à l'abonnement à modifier ou à supprimer.  
  
3.  Ouvrez sur le menu Rapport, puis cliquez sur **Supprimer**.  
  
     ![Menu rapport](../../reporting-services/subscriptions/media/ssrs-report-menu-report-manager.png "Menu Rapport")  
  
 Pour annuler un abonnement qui est en cours de traitement sur le serveur de rapports, consultez [Gérer un processus en cours d’exécution](../../reporting-services/subscriptions/manage-a-running-process.md).  
  
 Si vous voulez mettre fin à un abonnement et si vous n'arrivez pas à le localiser, prenez note du rapport que vous recevez et faites une recherche sur son nom. Une fois que vous accédez au rapport, vous pouvez vous retirer de l'abonnement. Si vous n'arrivez pas à localiser l'abonnement, celui-ci est peut-être de type piloté par les données. Pour plus d'informations, contactez l'administrateur de votre serveur de rapports.  
  
 Un abonnement est supprimé automatiquement lorsque le rapport sous-jacent est supprimé. Si vous supprimez un abonnement lors de son traitement, l'abonnement s'arrête lorsque la suppression intervient avant que l'extension de remise n'ait reçu les données d'abonnement. Dans le cas contraire, le traitement de l'abonnement se poursuit.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer et gérer des abonnements pour des serveurs de rapports en mode SharePoint](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-sharepoint-mode-report-servers.md)   
 [Utiliser PowerShell pour modifier et répertorier les propriétaires d’abonnements Reporting Services, et exécuter un abonnement](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md)   
 [Abonnements pilotés par les données](../../reporting-services/subscriptions/data-driven-subscriptions.md)   
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [Utiliser Mes abonnements &#40;serveur de rapports en mode natif&#41;](../../reporting-services/subscriptions/use-my-subscriptions-native-mode-report-server.md)  
  
  
