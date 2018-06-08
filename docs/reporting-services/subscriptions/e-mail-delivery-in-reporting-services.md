---
title: Remise par courrier électronique dans Reporting Services | Microsoft Docs
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
- subscriptions [Reporting Services], e-mail
- e-mail [Reporting Services]
- mail [Reporting Services]
ms.assetid: fda2f130-97b9-4258-9dbb-e93a70f4d08a
caps.latest.revision: 45
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 44fd22755bc91784966e1b5b27107aad1d3f2ae4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="e-mail-delivery-in-reporting-services"></a>Remise par courrier électronique dans Reporting Services
  SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comprend une extension de remise par e-mail qui permet d’envoyer par e-mail des rapports à des utilisateurs individuels ou à des groupes. Pour distribuer un rapport par courrier électronique, vous devez 1) configurer le serveur de rapports pour la remise du courrier électronique et 2) définir un abonnement standard ou piloté par les données. Un abonnement unique ne permet pas de distribuer plusieurs rapports dans un seul message électronique. Toutefois, vous pouvez créer plusieurs abonnements.  
  
 Le serveur de rapports se connecte à un serveur de messagerie à l'aide d'une connexion standard. Il n'utilise pas le chiffrement SSL (Secure Sockets Layer) pour ses communications. Le serveur de messagerie doit être un serveur SMTP (Simple Mail Transport Protocol) local ou distant situé sur le même réseau que le serveur de rapports.  
  
 Pour plus d’informations sur la procédure détaillées de création d’un abonnement, voir :  
  
-   [Créer et gérer des abonnements pour les serveurs de rapports en mode natif](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)  
  
-   [Create and Manage Subscriptions for SharePoint Mode Report Servers](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-sharepoint-mode-report-servers.md)  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint &#124; [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif|  
  
## <a name="e-mail-delivery-options"></a>Options de remise des rapports par messagerie  
 La remise de rapports par messagerie du serveur de rapports s’effectue de différentes manières :  
  
-   Envoi d'une notification et d'un lien hypertexte vers le rapport généré.  
  
-   Envoi d'une notification dans la ligne Objet d'un message électronique. Par défaut, la ligne Objet de la définition d'abonnement contient les variables suivantes qui sont remplacées par des informations spécifiques au rapport lors du traitement de l'abonnement.  
  
     **@ReportName** spécifie le nom du rapport.  
  
     **@ExecutionTime** spécifie l'heure d'exécution du rapport.  
  
     Vous pouvez combiner ces variables avec du texte statique ou modifier le texte dans la ligne Objet de chaque abonnement.  
  
-   Envoi d'un rapport incorporé ou joint. Le format de rendu et le navigateur déterminent si le rapport est incorporé ou joint.  
  
     Si votre navigateur prend en charge les formats HTML 4.0 et MHTML et si vous choisissez le format de rendu Archive Web, le rapport est incorporé au message. Tous les autres formats de rendu (CSV, PDF, etc.) remettent les rapports sous forme de pièces jointes. Pour les serveurs de rapports en mode natif, vous pouvez désactiver cette fonctionnalité dans le fichier de configuration RSReportServer.config.  
  
     [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne vérifie pas la taille de la pièce jointe ou du message avant d'envoyer le rapport. Si la pièce jointe ou le message dépasse la limite maximale autorisée par votre serveur de messagerie, le rapport n'est pas remis. Choisissez une des autres options de remise (URL ou notification) si le rapport est volumineux.  
  
 C'est au moment de la création de l'abonnement que vous définissez les options de remise d'un rapport. Par exemple, si vous sélectionnez **Inclure un lien** dans l’abonnement, le message e-mail contient un lien hypertexte vers le rapport.  
  
## <a name="native-mode-role-based-e-mail-settings"></a>Paramètres de messagerie basés sur les rôles en mode natif  
 Dans un environnement de serveur de rapports en mode natif, les paramètres de remise du courrier électronique que vous utilisez varient en fonction de votre rôle qui englobe soit la tâche « Gérer les abonnements individuels », soit la tâche « Gérer tous les abonnements ».  
  
|Tâche|Paramètres disponibles|  
|----------|------------------------|  
|Gérer les abonnements individuels|Affiche les champs qui permettent à un utilisateur d'automatiser et de se remettre un rapport. Dans ce mode, les champs qui acceptent les adresses de messagerie ne sont pas disponibles.|  
|Gérer tous les abonnements|Affiche les champs qui prennent en charge une plus large distribution, notamment À, Cc, Cci et Répondre à, qui offrent des moyens supplémentaires d'acheminer un rapport vers des abonnés plus nombreux. La disponibilité des champs d'alias de messagerie électronique est définie par le biais des paramètres du fichier de configuration RSReportServer.|  
  
## <a name="specifying-e-mail-addresses-in-a-subscription"></a>Spécification d'adresses électroniques dans un abonnement  
 Si vous effectuez la distribution des rapports au sein d'un intranet et si vous utilisez une passerelle SMTP vers un serveur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Exchange, tapez les alias de messagerie (comme si vous envoyiez du courrier électronique à un collègue). Si la remise est destinée à un compte de messagerie externe, tapez l'adresse de messagerie complète. Si vous spécifiez des adresses de messagerie supplémentaires pour ajouter d'autres personnes à votre abonnement, les abonnés reçoivent une copie exacte du rapport qui est produit à partir de cet abonnement.  
  
 Le serveur de rapports n'obtient ni ne valide les adresses de messagerie d'un serveur de messagerie. Vous devez connaître à l'avance les adresses de messagerie qui seront utilisées. Par défaut, vous pouvez envoyer des rapports par courrier électronique à n'importe quel compte de messagerie valide à l'intérieur ou à l'extérieur de votre organisation. Des paramètres de configuration peuvent toutefois être utilisés pour limiter la remise par messagerie électronique aux hôtes du serveur de courrier que vous identifiez par leur nom. Vous pouvez spécifier des hôtes supplémentaires si vous voulez prendre en charge la remise par messagerie à des personnes ne faisant pas partie de votre organisation.  
  
 Le message électronique utilisé pour remettre le rapport doit être envoyé depuis un compte de messagerie défini sur le serveur de messagerie. Un paramètre de configuration spécifie le compte de messagerie. Le compte de messagerie est utilisé pour tous les rapports remis par l'extension de remise par messagerie. Vous ne pouvez pas spécifier plusieurs comptes ou changer de compte en fonction des rapports.  
  
## <a name="controlling-e-mail-delivery"></a>Contrôle de la remise par messagerie  
 Vous pouvez configurer un serveur de rapports de façon à limiter la distribution par messagerie à des domaines hôtes spécifiques. Par exemple, vous pouvez empêcher un serveur de rapports en mode natif de remettre un rapport à tous les domaines, sauf à ceux répertoriés dans le fichier de configuration **RSReportServer.config** .  
  
 Vous pouvez également définir des paramètres de configuration pour masquer le champ **À** dans un abonnement. Dans ce cas, les rapports sont remis uniquement à l'utilisateur qui définit l'abonnement. Toutefois, une fois qu'un rapport a été envoyé à un utilisateur, vous ne pouvez pas explicitement l'empêcher d'être transféré.  
  
 La façon la plus efficace de contrôler la distribution des rapports consiste à configurer un serveur de rapports de sorte qu'il n'envoie qu'une adresse URL de serveur de rapports. Le serveur de rapports utilise l'authentification Windows et le modèle d'autorisation par rôle pour contrôler l'accès à un rapport. Si un utilisateur reçoit accidentellement par courrier électronique un rapport qu'il n'est pas autorisé à consulter, le serveur de rapports n'affiche pas le rapport. Pour plus d’informations sur les abonnements, voir ce qui suit.  
  
## <a name="e-mail-server-configuration"></a>Configuration du serveur de messagerie  
 Pour un serveur de rapports en mode natif, vous pouvez configurer l’extension de remise par e-mail via le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif et en modifiant les fichiers de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Dans le cas d’un serveur de rapports en mode SharePoint, l’extension de remise du courrier électronique est configurée dans les pages de gestion de SharePoint et dans les scripts PowerShell.  
  
 
 Pour des informations sur la configuration d’un serveur de rapports en mode natif, consultez [Paramètres de messagerie : mode natif de Reporting Services (Gestionnaire de configuration)](../install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md).
 
 
 Pour plus d'informations sur la configuration d'un serveur de rapports en mode SharePoint, consultez :  
  
  
## <a name="see-also"></a> Voir aussi  
 [Tâches et autorisations](../../reporting-services/security/tasks-and-permissions.md)   
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Abonnements pilotés par les données](../../reporting-services/subscriptions/data-driven-subscriptions.md)   
 [Attributions de rôles](../../reporting-services/security/role-assignments.md)  
  
  
