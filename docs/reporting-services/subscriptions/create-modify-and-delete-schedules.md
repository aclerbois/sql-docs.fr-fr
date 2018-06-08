---
title: Créer, modifier et supprimer des planifications | Microsoft Docs
ms.custom: ''
ms.date: 07/01/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- report-specific schedules [Reporting Services]
- shared schedules [Reporting Services]
- modifying schedules
- removing schedules
- shared schedules [Reporting Services], creating
- shared schedules [Reporting Services], modifying
- schedules [Reporting Services], deleting
- deleting schedules
- schedules [Reporting Services], creating
- schedules [Reporting Services], modifying
- shared schedules [Reporting Services], deleting
ms.assetid: 05da5f3d-9222-43a9-893b-aa10f0f690f8
caps.latest.revision: 50
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 560b4fff1069b2e29ca5847d5bf00f1a363976a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-modify-and-delete-schedules"></a>Create, Modify, and Delete Schedules
  Cette rubrique est consacrée à la création, la modification et la suppression des planifications partagées [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] .  Pour gérer des planifications partagées en mode natif, utilisez la page Planifications dans le portail web ou le dossier Planifications partagées dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Pour le mode SharePoint, utilisez les pages de gestion pour l'application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Pour déterminer si une planification partagée est utilisée de manière active, utilisez l’une des méthodes suivantes :  
  
-   **Portail web :** Dans la page Planifications partagées, passez en revue les valeurs des champs Date de la dernière exécution, Date de la prochaine exécution et États. Si une planification ne s'exécute plus parce qu'elle est arrivée à expiration, la date d'expiration apparaît dans le champ État. Pour plus d’informations, consultez [Web portal (SSRS Native Mode)](../../reporting-services/web-portal-ssrs-native-mode.md).
  
-   **[!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)]:** Consultez la page Rapports d’une planification partagée donnée. Cette page répertorie tous les rapports et datasets partagés qui utilisent la planification partagée. Pour plus d’information, consultez [Reporting Services pour SQL Server Management Studio ](../../reporting-services/tools/reporting-services-in-sql-server-management-studio-ssrs.md).
  
-  **Journaux :** Affichez les journaux des traces ou les fichiers journaux des exécutions des rapports afin de déterminer si ces derniers ont été exécutés aux heures spécifiées par la planification. Pour plus d’informations, consultez [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).  
  
## <a name="when-you-delete-a-shared-schedule"></a>Quand vous supprimez une planification partagée  
Les planifications partagées doivent être supprimées manuellement à l’aide de la page Planifications du portail web ou le dossier Planifications partagées dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Si vous supprimez une planification partagée qui est en cours d'utilisation, toutes les références à cette planification sont remplacées par des planifications spécifiques aux rapports.  
 
Si vous supprimez une planification partagée utilisée par plusieurs rapports et abonnements, le serveur de rapports créera des planifications individuelles pour chaque rapport et abonnement qui a précédemment utilisé la planification partagée. Chaque nouvelle planification individuelle contiendra la date, l'heure et la périodicité spécifiée dans la planification partagée. Notez que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne fournit pas de gestion centrale des planifications individuelles. Si vous supprimez une planification partagée, vous devez désormais gérer les informations de planification pour chaque élément individuel.  
  
**Remarque :**  Si vous n’êtes pas sûr qu’une planification partagée est utilisée, envisagez de la supprimer dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] plutôt qu’à l’aide du portail web. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] fournit les mêmes fonctionnalités de gestion des planifications partagées que le Gestionnaire de rapports, mais il fournit également une page Rapports supplémentaire qui indique le nom de chaque rapport utilisant la planification.  
   
 Le fait de supprimer une planification et le fait de provoquer son expiration sont deux opérations différentes. Une date d'expiration sert à arrêter une planification, mais ne la supprime pas. Comme les planifications servent à automatiser un grand nombre de fonctionnalités, elles ne sont jamais supprimées automatiquement. Les planifications expirées fournissent aux administrateurs de serveurs de rapports des éléments de preuve quant à la cause de l'arrêt subit d'un processus automatisé. Sans la présence de la planification expirée, un administrateur de serveur de rapports peut faire une erreur de diagnostic pour un problème ou perdre inutilement du temps à tenter de dépanner un processus qui fonctionne parfaitement.  
 
 ## <a name="when-you-delete-a-report-specific--schedule"></a>Quand vous supprimez une planification spécifique à un rapport  
Les planifications spécifiques à un rapport ou un abonnement sont supprimées lorsque vous supprimez le rapport ou l'abonnement, ou lorsque vous choisissez une autre approche pour exécuter le rapport ou l'abonnement. Par exemple, si vous choisissez **Toujours exécuter ce rapport avec les données les plus récentes** , la planification spécifique à un rapport que vous avez créée pour exécuter un rapport en tant qu’instantané d’exécution de rapport est supprimée.  

Une planification spécifique aux rapports arrivée à expiration reste associée au rapport. Vous pouvez déterminer si une planification est arrivée à expiration en vérifiant sa date de fin. Une planification partagée expirée reste dans la liste des planifications partagées. Le champ État indique si la planification est arrivée à expiration. Vous pouvez rétablir la planification en reportant la date de fin ou vous pouvez supprimer la référence à la planification si vous n'en n'avez plus besoin.  
  
## <a name="bkmk_native"></a> Créer, supprimer ou modifier une planification partagée (portail web)  
 La création et la modification d'une planification consiste à définir des options de fréquence qui déterminent le moment d'exécution de la planification.  
  
 Vous pouvez créer ou modifier une planification à n'importe quel moment. Toutefois, si une planification commence à s'exécuter avant que vous n'ayez terminé vos modifications, la version précédente de la planification est utilisée. La planification révisée ne prend pas effet tant que vous ne l'avez pas enregistrée.  
  
 Si vous modifiez une planification partagée, vous pouvez la suspendre avant d'apporter des modifications. Les modifications prennent effet lorsque vous reprenez la planification.  

1.  Dans le portail web, cliquez sur **Paramètres** ![ssrs_portal_settings_gear](../../reporting-services/subscriptions/media/ssrs-portal-settings-gear.png) dans la barre d’outils. **Remarque :** Si l’option **Paramètres du site** n’est pas disponible, vous n’êtes pas autorisé à accéder aux paramètres de site.
2.  Cliquez sur **Paramètres du site**.  
3.  Cliquez sur **Planifications**.  
4.  Cliquez sur **Nouvelle planification**. Pour modifier une planification existante, cliquez sur son nom.  
5.  Tapez un nom descriptif pour la planification.  
6.  Sélectionnez **Heure**, **Jour**, **Semaine**ou **Mois**. Cliquez sur **Une fois** pour créer une planification qui ne s'exécute qu'une seule fois. Des options supplémentaires s'affichent lorsque vous spécifiez la base de votre planification.  
7.  Si vous le souhaitez, sélectionnez une date de début pour la planification. Par défaut, il s'agit de la date du jour. Vous pouvez reporter le début de la planification en choisissant une date ultérieure.  
8.  Sélectionnez éventuellement une date de fin de planification. La planification cesse de s'exécuter à la date indiquée, mais elle n'est pas supprimée.  
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="to-delete-a-shared-schedule-web-portal"></a>Pour supprimer une planification partagée (portail web)  
  
1.  Dans le portail web, cliquez sur **Paramètres du site**dans la barre d’outils globale.     
2.  Dans la section **Autre** de cette page, cliquez sur **Gérer les planifications partagées**.  
3.  Activez la case à cocher située en regard de la planification à supprimer, puis cliquez sur **Supprimer**.  
  
### <a name="create-delete-or-modify-a-shared-schedule-management-studio"></a>Créer, supprimer ou modifier une planification partagée (Management Studio)  
 Une planification partagée contient les informations de planification et de périodicité qui peuvent être utilisées par tous les rapports publiés et abonnements qui s'exécutent sur un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si de nombreux rapports et abonnements s'exécutent en même temps, vous pouvez créer une planification partagée pour ces travaux. Si vous souhaitez modifier ultérieurement la périodicité ou la date de fin, vous pouvez effectuer cette modification à un seul emplacement.  
  
 La maintenance des planifications partagées est plus facile à assurer ; par ailleurs, ces dernières vous apportent une plus grande souplesse de gestion des opérations planifiées. Par exemple, vous pouvez suspendre et reprendre des planifications partagées. En outre, si vous trouvez que trop d'opérations planifiées s'exécutent en même temps, vous pouvez créer plusieurs planifications partagées qui s'exécutent à des heures différentes, puis vous pouvez ajuster les informations de planification de sorte que la charge de traitement soit répartie de manière égale sur le serveur de rapports.  
  
### <a name="to-create-or-modify-a-shared-schedule-management-studio"></a>Pour créer ou modifier une planification partagée (Management Studio)  
  
1.  Démarrez [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)] et connectez-vous à une instance de serveur de rapports.  
2.  Dans l'Explorateur d'objets, développez un nœud du serveur de rapports.  
3.  Cliquez avec le bouton droit sur le dossier **Planifications partagées** , puis cliquez sur **Nouvelle planification**. La page Général de la boîte de dialogue **Nouvelle planification partagée** s'affiche.  
  
     Pour modifier une planification partagée existante, développez le dossier Planifications partagées, cliquez avec le bouton droit sur la planification à modifier, puis cliquez sur **Propriétés**.  
  
4.  Tapez un nom descriptif pour la planification.  
5.  Si vous le souhaitez, sélectionnez une date de début pour la planification. Par défaut, il s'agit de la date du jour.  
6.  Si vous le souhaitez, sélectionnez une date de fin pour la planification. La planification cesse de s'exécuter à la date indiquée, mais elle n'est pas supprimée.  
7.  Pour configurer une planification périodique, sélectionnez **Heure**, **Jour**, **Semaine**ou **Mois**. Des options supplémentaires s'affichent. Utilisez ces options supplémentaires pour configurer la périodicité de la planification, en fonction de vos préférences selon l'heure, le jour, la semaine ou le mois.  
  
     Sinon, pour spécifier une planification unique (non périodique), sélectionnez **Une fois**, puis spécifiez une **Heure de début**.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##### <a name="to-delete-a-shared-schedule-management-studio"></a>Pour supprimer une planification partagée (Management Studio)  
  
1.  Dans l'Explorateur d'objets, développez un nœud du serveur de rapports.  
2.  Pour vérifier que la planification partagée n’est pas actuellement utilisée par les rapports, développez le dossier Planifications partagées, cliquez avec le bouton droit sur la planification, puis cliquez sur **Propriétés**.
3. Cliquez sur l’onglet **apports** pour afficher la liste des rapports qui utilisent actuellement la planification.
Cliquez sur **Annuler**.
4.  Développez le dossier Planifications partagées, cliquez avec le bouton droit sur la planification à supprimer, puis cliquez sur **Supprimer**. La boîte de dialogue **Suppression des éléments du catalogue** s'affiche.  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Si vous supprimez une planification partagée utilisée par plusieurs rapports et abonnements, le serveur de rapports créera des planifications individuelles pour chaque rapport et abonnement qui a précédemment utilisé la planification partagée. Chaque nouvelle planification individuelle contiendra la date, l'heure et la périodicité spécifiée dans la planification partagée.
 
##  <a name="bkmk_sharepoint"></a> Créer et gérer des planifications partagées (mode SharePoint)  
 Vous devez être un administrateur de site pour pouvoir créer, modifier ou supprimer les planifications partagées d'un site SharePoint.  
  
 Vous pouvez identifier une planification spécifique par son nom descriptif. Si aucun nom n'est spécifié, un nom par défaut est créé à partir des informations qui se rapportent à la planification, par exemple sa périodicité ou ses dates et heures d'exécution.  
  
> [!NOTE]  
>  La création de planifications partagées nécessite le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
### <a name="create-shared-schedules-sharepoint-mode"></a>Créer des planifications partagées (mode SharePoint)  
1.  Cliquez sur **Actions du site**.  
2.  Cliquez sur **Paramètres du site**.  
3.  Dans la section Reporting Services, cliquez sur **Gérer les planifications partagées**.  
4.  Cliquez sur **Ajouter une planification** pour ouvrir la page Propriétés de planification.  
5.  Entrez un nom descriptif pour la planification. Dans les pages d’application utilisées pour travailler avec des rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , ce nom apparaît dans les listes déroulantes des pages de définition de la planification sur l’ensemble du site. Évitez les noms longs difficiles à lire. Efforcez-vous de respecter une convention d'affectation des noms pour fournir la plus grande partie des informations descriptives au début du nom.  
6.  Choisissez une fréquence. Selon la fréquence choisie, les options de planification qui s’affichent sur la page peuvent varier pour prendre en charge cette fréquence (par exemple, si vous choisissez **Mois**, le nom de chaque mois s’affiche sur la page).  
7.  Définissez la planification. Toutes les combinaisons de planification ne peuvent pas être prises en charge dans une seule planification.  
8.  Définissez une date de début et de fin.  
9. Cliquez sur **OK**.  
  
### <a name="delete-shared-schedules-sharepoint-mode"></a>Supprimer des planifications partagées (mode SharePoint)  
 Toutes les planifications, qu'elles soient partagées ou spécifiques aux rapports, doivent être supprimées manuellement. Si vous supprimez une planification partagée en cours d'utilisation, toutes les références à cette dernière sont remplacées par des planifications personnalisées non spécifiées (c'est-à-dire des planifications personnalisées qui n'ont pas d'informations de date et d'heure).  
  
1.  Cliquez sur **Actions du site**.  
2.  Cliquez sur **Paramètres du site**.  
3.  Dans la section Reporting Services, cliquez sur **Gérer les planifications partagées**.  
4.  Sélectionnez la planification, puis cliquez sur **Supprimer**.  
 
  
## <a name="see-also"></a> Voir aussi  
 [Schedules](../../reporting-services/subscriptions/schedules.md)   
 [Suspendre et reprendre des planifications partagées](../../reporting-services/subscriptions/pause-and-resume-shared-schedules.md)   
 [Mettre en cache un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/report-server/cache-a-report-report-manager.md)   
 [Ajouter un instantané à un historique de rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/report-server/add-a-snapshot-to-report-history-report-manager.md)  
  
  
