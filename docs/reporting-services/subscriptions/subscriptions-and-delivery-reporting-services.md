---
title: Abonnements et remise (Reporting Services) | Microsoft Docs
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
- subscriptions [Reporting Services], report distribution
- reports [Reporting Services], distributing
- distributing reports [Reporting Services]
- published reports [Reporting Services], distributing
- sending reports
- sharing reports
- delivering reports [Reporting Services]
- distributing reports [Reporting Services], subscriptions
- subscriptions [Reporting Services], about subscriptions
- subscriptions [Reporting Services]
ms.assetid: be7ec052-28e2-4558-bc09-8479e5082926
caps.latest.revision: 56
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: a3d81598480aab552b0891fb7ae1247d9149ce71
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="subscriptions-and-delivery-reporting-services"></a>Abonnements et remise (Reporting Services)
  Un abonnement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est une configuration qui remet un rapport à une heure donnée ou en réponse à un événement, et dans un format de fichier que vous définissez. Par exemple, tous les mercredis, enregistrer le rapport MonthlySales.rdl au format de document Microsoft Word sur un partage de fichiers. Vous pouvez utiliser des abonnements pour planifier et automatiser la remise d'un rapport avec un ensemble de valeurs de paramètres de rapport spécifique.  
  
 Vous pouvez créer plusieurs abonnements pour un seul rapport afin de varier les options d'abonnement. Vous pouvez ainsi spécifier différentes valeurs de paramètres pour générer trois versions du même rapport, par exemple un rapport des ventes pour la région Ouest, un autre pour la région Est et un autre pour toutes les ventes.  
  
 ![exemple de flux d’abonnement ssrs](../../reporting-services/subscriptions/media/ssrs-subscription-example-flow.png "exemple de flux d’abonnement ssrs")  
  
 Les abonnements ne sont pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 **Dans cette rubrique :**  
  
-   [Scénarios d’abonnement et de remise](#bkmk_subscription_scenarios)  
  
-   [Abonnements standard et pilotés par les données](#bkmk_standard_and_datadriven)  
  
-   [Conditions requises pour les abonnements](#bkmk_subscription_requirements)  
  
-   [Extensions de remise](#bkmk_delivery_extensions)  
  
-   [Composants d’un abonnement](#bkmk_parts_of_subscription)  
  
-   [Traitement des abonnements](#bkmk_subscription_processing)  
  
-   [Contrôle par programmation des abonnements](#bkmk_code)  
  
 **Rubriques de cette section :**  
  
-   [Remise par courrier électronique dans Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md) Décrit le fonctionnement et la configuration de la remise par partage de fichiers du serveur de rapports.  
  
-   [Créer et gérer des abonnements pour les serveurs de rapports en mode natif](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md) Étapes détaillées du processus de création d’abonnements avec un serveur de rapports en mode natif.  
  
-   [Créer et gérer des abonnements pour des serveurs de rapports en mode SharePoint](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-sharepoint-mode-report-servers.md) Étapes détaillées du processus de création d’abonnements avec un serveur de rapports en mode SharePoint.  
  
-   [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md) Décrit le fonctionnement et la configuration de la remise par partage de fichiers du serveur de rapports.  
  
-   [Disable or Pause Report and Subscription Processing](../../reporting-services/subscriptions/disable-or-pause-report-and-subscription-processing.md).  
  
-   [SharePoint Library Delivery in Reporting Services](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md) Décrit la remise d'abonnements dans une bibliothèque SharePoint.  
  
-   [Abonnements pilotés par les données](../../reporting-services/subscriptions/data-driven-subscriptions.md) Fournit des informations sur l’utilisation d’abonnements pilotés par les données pour personnaliser la sortie des rapports au moment de l’exécution.  
  
-   [Analyser les abonnements Reportions Services](../../reporting-services/subscriptions/monitor-reporting-services-subscriptions.md)  
  
-   [Utiliser PowerShell pour modifier et répertorier les propriétaires d’abonnements Reporting Services, et exécuter un abonnement](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md)  
  
##  <a name="bkmk_subscription_scenarios"></a> Scénarios d’abonnement et de remise  
 Pour chaque abonnement, vous configurez les options de remise et les options disponibles sont déterminées par l'extension de remise que vous choisissez. Une extension de remise est un module qui prend en charge un mode quelconque de distribution. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comprend plusieurs extensions de remise et d’autres extensions peuvent vous être proposées par des fournisseurs tiers.  
  
 Si vous êtes un développeur, vous pouvez créer des extensions de remise personnalisées pour prendre en charge des scénarios supplémentaires. Pour plus d'informations, consultez [Implémentation d'une extension de remise](../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md).  
  
 Le tableau suivant décrit les scénarios d'abonnement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] courants.  
  
|Scénario|Description|  
|--------------|-----------------|  
|Rapports par courrier électronique|Vous pouvez envoyer des rapports par courrier électronique aux utilisateurs et aux groupes. Créez un abonnement et spécifiez un alias de groupe ou un alias de messagerie pour recevoir le rapport que vous souhaitez distribuer. Vous pouvez faire en sorte que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] détermine les données d'abonnement au moment de l'exécution. Si vous souhaitez envoyer le même rapport à un groupe dont la liste des membres change, vous pouvez utiliser une requête pour dériver la liste des abonnements au moment de l'exécution.|  
|Afficher des rapports hors connexion|Les utilisateurs peuvent sélectionner l'un des formats suivants pour la sortie d'abonnement :<br /><br /> -   Fichier XML avec données de rapport<br />-   CSV (délimité par des virgules)<br />-   PDF<br />-   MHTML (archive web)<br />-   Microsoft Excel<br />-   Fichier TIFF<br />-   Microsoft Word<br /><br /> Les rapports que vous souhaitez archiver peuvent être enregistrés directement dans un dossier partagé que vous sauvegardez selon une planification nocturne. Les rapports volumineux trop longs à charger dans un navigateur peuvent être enregistrés dans un dossier partagé sous un format pouvant être affiché dans une application bureautique.|  
|Cache de pré-chargement|Si vous disposez de plusieurs instances d'un rapport paramétré ou qu'un grand nombre d'utilisateurs de rapports visionnent des rapports, vous pouvez précharger les rapports dans le cache pour réduire le temps de traitement requis pour afficher le rapport.|  
|Rapports pilotés par les données|Utilisez les abonnements pilotés par les données pour personnaliser le résultat d'un rapport, les options de remise, ainsi que les paramètres d'un rapport au moment de l'exécution. L'abonnement utilise une requête pour obtenir les valeurs d'entrée d'une source de données au moment de l'exécution. Vous pouvez utiliser les abonnements pilotés par les données pour effectuer une opération de publipostage qui envoie un rapport à une liste d'abonnés déterminée au moment où l'abonnement est traité.|  
  
##  <a name="bkmk_standard_and_datadriven"></a> Abonnements standard et pilotés par les données  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prend en charge deux types d’abonnements : les abonnements **standard** et les abonnements **pilotés par les données**. Les abonnements standard sont créés et gérés par des utilisateurs individuels. Un abonnement standard se compose de valeurs statiques qui ne peuvent pas changer au cours du traitement. Pour chaque abonnement standard, il y a exactement un jeu d'options de présentation des rapports, d'options de remise et de paramètres de rapport.  
  
 Les abonnements pilotés par les données obtiennent les informations d'abonnement au moment de l'exécution en interrogeant une source de données externe qui fournit les valeurs utilisées pour spécifier un destinataire, des paramètres de rapport ou un format d'application. Vous pouvez utiliser des abonnements pilotés par les données si la taille de votre liste de destinataires est très importante ou si vous voulez modifier la sortie du rapport pour chaque destinataire. Pour cela, vous devez savoir créer des requêtes et comprendre comment les paramètres sont utilisés. En règle générale, les administrateurs de serveur de rapports se chargent de créer et de gérer ces abonnements. Pour plus d'informations, consultez les documents suivants :  
  
-   [Abonnements pilotés par les données](../../reporting-services/subscriptions/data-driven-subscriptions.md)  
  
-   [Créer un abonnement piloté par les données &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md)  
  
##  <a name="bkmk_subscription_requirements"></a> Conditions requises pour les abonnements  
 Avant de pouvoir créer un abonnement à un rapport, les conditions préalables requises suivantes doivent être remplies :  
  
|Condition requise|Description|  
|-----------------|-----------------|  
|Autorisations|Vous devez avoir accès au rapport. Avant de pouvoir vous abonner à un rapport, vous devez être autorisé à l'afficher.<br /><br /> Pour les serveurs de rapports en mode natif, les attributions de rôles suivantes affectent les abonnements :<br /><br /> -   La tâche « Gérer les abonnements individuels » permet aux utilisateurs de créer, de modifier et de supprimer les abonnements d’un rapport spécifique. Dans les rôles prédéfinis, cette tâche fait partie des rôles Navigateur et Générateur de rapports. Les attributions de rôles incluant cette tâche autorisent un utilisateur à gérer uniquement les abonnements qu'il crée.<br />-   La tâche « Gérer tous les abonnements » permet aux utilisateurs d’accéder à tous les abonnements pour les modifier. Cette tâche est obligatoire pour créer des abonnements pilotés par les données. Dans les rôles prédéfinis, seul le rôle Gestionnaire de contenu inclut cette tâche.|  
|Informations d'identification stockées|Pour créer un abonnement, il faut que le rapport utilise des informations d'identification stockées ou qu'il n'en utilise pas du tout pour être en mesure d'extraire les données au moment de l'exécution. Vous ne pouvez pas vous abonner à un rapport configuré pour utiliser les informations d'identification empruntées ou déléguées à partir de l'utilisateur actuel pour vous connecter à une source de données externe. Les informations d'identification stockées peuvent être un compte Windows ou un compte d'utilisateur de base de données. Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).<br /><br /> Vous devez être autorisé à afficher le rapport et à créer des abonnements individuels. L'option**Événements programmés et remise du rapport** doit être activée sur le serveur de rapports. Pour plus d’informations, consultez [old_Créer et gérer des abonnements pour les serveurs de rapports en mode natif](http://msdn.microsoft.com/en-us/7f46cbdb-5102-4941-bca2-5e0ff9012c6b).|  
|Valeurs dépendantes de l'utilisateur dans un rapport|Pour les abonnements standard uniquement, vous pouvez créer des abonnements à des rapports qui intègrent des informations de compte d'utilisateur dans un filtre ou sous forme de texte qui apparaît dans le rapport. Dans le rapport, le nom de compte d’utilisateur est spécifié par le biais d’une expression **User!UserID** qui correspond à l’utilisateur actuel. Lorsque vous créez un abonnement, l'utilisateur qui crée l'abonnement est considéré comme l'utilisateur actuel.|  
|Aucune sécurité de l'élément de modèle|Vous ne pouvez pas vous abonner à un rapport du Générateur de rapports qui utilise un modèle comme source de données si le modèle contient des paramètres de sécurité de l'élément de modèle. Seuls les rapports qui utilisent la sécurité de l'élément de modèle sont inclus dans cette restriction.|  
|Valeurs de paramètre|Si le rapport utilise des paramètres, une valeur de paramètre doit être spécifiée avec le rapport lui-même ou dans l'abonnement que vous définissez. Si des valeurs par défaut ont été définies dans le rapport, vous pouvez configurer la valeur de paramètre pour les utiliser.|  
  
##  <a name="bkmk_delivery_extensions"></a> Extensions de remise  
 Les abonnements sont traités sur le serveur de rapports et sont distribués via les extensions de remise déployées sur le serveur. Par défaut, vous pouvez créer des abonnements qui envoient des rapports vers un dossier partagé ou une adresse de messagerie. Si le serveur de rapports est configuré en mode intégré SharePoint, vous pouvez également envoyer un rapport vers une bibliothèque SharePoint.  
  
 Lors de la création d'un abonnement, l'utilisateur peut choisir l'une des extensions de remise disponibles pour déterminer le mode de remise du rapport. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comprend les extensions de remise suivantes.  
  
|Extension de remise|Description|  
|------------------------|-----------------|  
|Partage de fichiers Windows|Remet un rapport en tant que fichier d'application statique dans un dossier partagé accessible sur le réseau.|  
|Messagerie électronique|Remet une notification ou un rapport en tant que pièce jointe de message électronique ou en tant qu'URL.|  
|Bibliothèque SharePoint|Remet un rapport en tant que fichier d'application statique dans une bibliothèque SharePoint accessible à partir d'un site SharePoint. Le site doit être intégré à un serveur de rapports qui s'exécute en mode intégré SharePoint.|  
|Null|Le fournisseur de remise Null est une extension de remise très spécialisée qui sert à précharger un cache à l'aide de rapports paramétrables prêts à être affichés. Dans les abonnements individuels, cette méthode n'est pas accessible aux utilisateurs. Elle est utilisée par les administrateurs dans les abonnements pilotés par les données pour améliorer les performances d'un serveur de rapports en préchargeant le cache.|  
  
> [!NOTE]  
>  La remise de rapports est un module extensible de l'architecture de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . D'autres fournisseurs peuvent créer des extensions de remise personnalisée pour acheminer les rapports vers des emplacements ou des périphériques différents. Pour plus d'informations sur les extensions de remise personnalisées, consultez [Implémentation d’une extension de remise](../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md).  
  
##  <a name="bkmk_parts_of_subscription"></a> Composants d’un abonnement  
 Une définition d'abonnement se compose des éléments suivants :  
  
-   Pointeur vers un rapport capable de s'exécuter sans assistance (c'est-à-dire un rapport qui utilise des informations d'identification stockées ou qui n'utilise aucune information d'identification).  
  
-   Mode de remise (par messagerie électronique, par exemple) et paramètres correspondants (adresse de messagerie, par exemple).  
  
-   Extension de rendu pour présenter le rapport dans un format spécifique.  
  
-   Conditions du traitement de l'abonnement, exprimé comme un événement.  
  
     Généralement, les conditions d'exécution d'un rapport sont basées sur des critères horaires. Par exemple, vous pouvez exécuter un rapport spécifique tous les mardis à 15 h 00. UTC. Toutefois, si le rapport s'exécute en tant qu'instantané, vous pouvez spécifier que l'abonnement s'exécute chaque fois que l'instantané est actualisé.  
  
-   Les paramètres utilisés lors de l'exécution du rapport.  
  
     Les paramètres sont facultatifs et ne sont spécifiés que pour les rapports acceptant des valeurs de paramètre. Étant donné qu'un abonnement appartient généralement à un utilisateur, les valeurs de paramètre spécifiées varient d'un abonnement à un autre. Ainsi, les responsables commerciaux de différents départements utiliseront des paramètres pour retourner des données propres à leur département. Tous les paramètres doivent avoir une valeur explicitement définie ou une valeur par défaut valide.  
  
 Les informations d'abonnement sont stockées individuellement avec les rapports dans une base de données du serveur de rapports. Vous ne pouvez pas gérer les abonnements indépendamment des rapports auxquels ils sont associés. Notez que les abonnements ne peuvent pas être étendus pour inclure des descriptions, du texte personnalisé ou d'autres éléments. Ils ne peuvent contenir que les éléments indiqués ci-dessus.  
  
##  <a name="bkmk_subscription_processing"></a> Traitement des abonnements  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comprend un processeur de planification et de livraison qui permet de planifier les rapports et d'assurer leur remise aux utilisateurs. Le serveur de rapports répond aux événements qu'il analyse en permanence. Lorsqu'un événement qui se produit correspond aux conditions définies d'un abonnement, le serveur de rapports lit l'abonnement afin de déterminer comment traiter et remettre le rapport. Le serveur de rapports demande l'extension de remise qui est spécifiée dans l'abonnement. Lorsque l'extension de remise s'exécute, le serveur de rapports extrait les informations de remise de l'abonnement et les transmet à l'extension de remise pour le traitement.  
  
 L'extension de remise effectue le rendu du rapport dans le format défini dans l'abonnement, puis remet le rapport ou la notification au destinataire spécifié. Si un rapport ne peut pas être remis, une entrée est consignée dans le fichier journal du serveur de rapports. Si vous voulez prendre en charge plusieurs tentatives, vous pouvez configurer le serveur de rapports de manière à ce qu'il réessaie de remettre le rapport en cas d'échec de la première tentative.  
  
### <a name="processing-a-standard-subscription"></a>Traitement d’un abonnement standard  
 Les abonnements standard produisent une instance de rapport. Le rapport est remis dans un dossier partagé unique ou aux adresses de messagerie spécifiées dans l'abonnement. La mise en page et les données ne varient pas. Si le rapport utilise des paramètres, un abonnement standard est traité avec une seule valeur pour chaque paramètre du rapport.  
  
### <a name="processing-a-data-driven-subscription"></a>Traitement d’un abonnement piloté par les données  
 Les abonnements pilotés par les données peuvent produire de nombreuses instances de rapport remises à de multiples destinataires. La mise en page du rapport ne varie pas, mais les données que le rapport contient peuvent changer si des valeurs de paramètres sont transmises d'un ensemble de résultats d'abonnés. Les options de remise, qui affectent le rendu du rapport et déterminent l'insertion du rapport dans un message électronique (sous forme de pièce jointe ou de lien hypertexte), peuvent également varier d'un abonné à un autre lorsque les valeurs sont transmises de l'ensemble de lignes.  
  
 Les abonnements pilotés par les données peuvent produire un grand nombre de remises. Le serveur de rapports crée une remise pour chaque ligne de l'ensemble de lignes retourné par la requête d'abonnement.  
  
### <a name="report-delivery-characteristics"></a>Caractéristiques de la remise des rapports  
 Les rapports qui sont remis par abonnements standard sont généralement rendus sous forme de rapports statiques. Ces rapports sont soit basés sur l'instantané d'exécution de rapport le plus récent, soit générés comme des rapports statiques dans le but d'effectuer une remise. Si vous choisissez l'option **Inclure un lien** pour un abonnement à un rapport exécuté à la demande, le serveur de rapports exécute le rapport lorsque vous cliquez sur le lien hypertexte.  
  
> [!NOTE]  
>  Les rapports qui sont remis via une URL restent connectés au serveur de rapports et peuvent être mis à jour ou supprimés entre deux consultations. Les options de remise que vous choisissez pour votre abonnement déterminent si le rapport est remis sous forme d'URL, incorporé dans le corps du message électronique ou envoyé sous forme de pièce jointe.  
  
 Les rapports qui sont remis via un abonnement piloté par les données peuvent être régénérés lors du traitement de l'abonnement. Le serveur de rapports ne verrouille pas une instance spécifique d'un rapport ou de son dataset pour exécuter un abonnement piloté par les données. Si l'abonnement utilise différentes valeurs de paramètres pour différents abonnés, le serveur de rapports régénère le rapport de manière à produire le résultat requis. Si les données sous-jacentes sont mises à jour après la création et la remise de la première copie du rapport, les rapports envoyés par la suite pourront intégrer des données basées sur un autre ensemble de résultats. Vous pouvez utiliser un rapport qui s'exécute comme un instantané pour garantir que la même instance de rapport sera remise à tous les abonnés. Toutefois, si une mise à jour planifiée de l'instantané se produit lors du traitement de l'abonnement, les utilisateurs peuvent obtenir des données différentes dans leurs rapports.  
  
### <a name="triggering-subscription-processing"></a>Déclenchement du traitement des abonnements  
 Le serveur de rapports utilise deux genres d'événements pour déclencher le traitement des abonnements : un événement piloté par le temps, spécifié dans une planification, ou un événement de mise à jour d'instantanés.  
  
 Un déclencheur piloté par le temps utilise une planification spécifique aux rapports ou une planification partagée pour indiquer le moment exact où l'abonnement doit s'exécuter. Pour les rapports mis en cache et les rapports à la demande, les planifications demeurent les seules possibilités de déclenchement.  
  
 Un événement de mise à jour d'instantanés utilise la mise à jour planifiée d'un instantané de rapport pour déclencher un abonnement. Vous pouvez définir un abonnement pour qu'il soit déclenché à chaque nouvelle mise à jour du rapport, selon les propriétés d'exécution définies pour ce rapport.  
  
##  <a name="bkmk_code"></a> Contrôle par programmation des abonnements  
 Le modèle d’objet [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vous permet d’effectuer un audit et de contrôler par programmation les abonnements et leur traitement.  Consultez les éléments suivants pour obtenir des exemples et vous lancer :  
  
-   [Utiliser PowerShell pour modifier et répertorier les propriétaires d’abonnements Reporting Services, et exécuter un abonnement](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md)  
  
-   Pour obtenir des exemples indiquant comment utiliser PowerShell pour activer et désactiver des abonnements, voir [Désactiver ou suspendre le traitement des rapports et des abonnements](../../reporting-services/subscriptions/disable-or-pause-report-and-subscription-processing.md).  
  
-   <xref:ReportService2010.ReportingService2010.ChangeSubscriptionOwner%2A>  
  
-   Pour obtenir un exemple de script PowerShell permettant de répertorier l’ensemble des abonnements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configurés pour utiliser le **compte de partage de fichiers**, consultez [Paramètres d’abonnement et compte de partage de fichiers &#40;Gestionnaire de configuration&#41;](../../reporting-services/install-windows/subscription-settings-and-a-file-share-account-configuration-manager.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un abonnement piloté par les données &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md)   
 [Planifications](../../reporting-services/subscriptions/schedules.md)   
 [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Analyser les abonnements Reportions Services](../../reporting-services/subscriptions/monitor-reporting-services-subscriptions.md)  
  
  
