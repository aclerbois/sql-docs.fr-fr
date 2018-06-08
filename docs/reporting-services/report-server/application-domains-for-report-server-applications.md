---
title: Domaines d’application des applications du serveur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- application domains [Reporting Services]
- recycling application domains
ms.assetid: a455e2e6-8764-493d-a1bc-abe80829f543
caps.latest.revision: 18
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: fad2f5fd529550e7d5ed1f6101f271d15753364a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="application-domains-for-report-server-applications"></a>Domaines d'application des applications du serveur de rapports
  Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], le serveur de rapports est implémenté comme un service unique qui contient le service Web Report Server, le Gestionnaire de rapports et une application de traitement en arrière-plan. Chaque application s'exécute dans son propre domaine d'application au sein du processus unique du serveur de rapports. Pour la plupart, les domaines d'application sont créés, configurés et gérés de façon interne. Toutefois, il peut s'avérer utile de savoir comment les opérations de recyclage se produisent pour les domaines d'application du serveur de rapports, si vous cherchez à résoudre des problèmes de performances ou de mémoire, ou si vous dépannez des interruptions de service.  
  
> [!NOTE]  
>  Si vous configurez l'accès au Générateur de rapports sur un serveur de rapports qui utilise l'authentification de base, le Générateur de rapports s'exécute dans son propre domaine d'application. Ce domaine d'application est différent d'autres domaines d'application qui s'exécutent dans le processus serveur. Il est géré par le Contrôleur de services et n'est pas affecté par les fonctionnalités de gestion de la mémoire qui réajustent l'allocation de la mémoire en réponse à sa sollicitation sur le serveur de rapports.  
  
 La liste suivante décrit les événements qui provoquent les opérations de recyclage de domaine d'application pour les applications [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Opérations de recyclage planifiées qui se produisent à des intervalles prédéfinis.  
  
-   Modifications de configuration sur le serveur de rapports.  
  
-   [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .  
  
-   Échecs d'allocation de mémoire.  
  
 Le tableau suivant résume le comportement du recyclage de domaine d'application en réponse à ces événements :  
  
|Événement|Description de l'événement|S'applique à|Configurable|Description de l'opération de recyclage|  
|-----------|-----------------------|----------------|------------------|-----------------------------------|  
|Opérations de recyclage planifiées qui se produisent à des intervalles prédéfinis|Par défaut, les domaines d'application sont recyclés toutes les 12 heures.<br /><br /> Les opérations de recyclage planifiées sont fréquentes pour les applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] qui favorisent l'intégrité de l'ensemble du processus.|Service Web Report Server<br /><br /> Gestionnaire de rapports<br /><br /> Application de traitement en arrière-plan|Oui. Le paramètre de configuration**RecycleTime** du fichier RSReportServer.config détermine l'intervalle de recyclage.<br /><br /> **MaxAppDomainUnloadTime** définit le temps d'attente pendant lequel le traitement en arrière-plan est autorisé à s'effectuer.|[!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] gère l'opération de recyclage pour le service Web et le Gestionnaire de rapports.<br /><br /> Pour l'application de traitement en arrière-plan, le serveur de rapports crée un domaine d'application des nouveaux travaux ayant démarré à partir de planifications. Les travaux actifs sont autorisés à terminer leur exécution dans le domaine d'application actuel jusqu'à l'expiration du délai.|  
|Modifications de configuration sur le serveur de rapports|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] recycle les domaines d'application en réponse aux modifications du fichier RSReportServer.config.|Service Web Report Server<br /><br /> Gestionnaire de rapports<br /><br /> Application de traitement en arrière-plan|Non.|Vous ne pouvez pas empêcher les opérations de recyclage d'avoir lieu. Toutefois, les opérations de recyclage qui se produisent en réponse à des modifications de configuration sont gérées de la même façon que les opérations de recyclage planifiées. Des domaines d'application sont créés pour les nouvelles requêtes tandis que les requêtes et travaux actifs finissent de s'exécuter dans le domaine d'application actuel.|  
|[!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] Modifications de configuration|[!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] recycle les domaines d’application si des modifications sont apportées aux fichiers qu’il surveille (par exemple les fichiers machine.config et Web.config, ainsi que les fichiers programme [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] ).|Service Web Report Server<br /><br /> Gestionnaire de rapports|Non.|[!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] gère l'opération.<br /><br /> Les opérations de recyclage débutées par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] n'affectent pas le domaine d'application de traitement en arrière-plan.|  
|Sollicitation de la mémoire et échecs d'allocation de mémoire|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recycle immédiatement les domaines d'application en cas d'échec d'allocation de mémoire ou lorsque le serveur est soumis à une forte sollicitation de la mémoire.|Service Web Report Server<br /><br /> Gestionnaire de rapports<br /><br /> Application de traitement en arrière-plan|Non.|Lorsque la sollicitation de la mémoire est élevée, le serveur de rapports n'accepte pas de nouvelles requêtes dans le domaine d'application actuel. Pendant la période où le serveur refuse les nouvelles requêtes, des erreurs HTTP 503 se produisent. Aucun domaine d'application n'est créé tant que l'ancien domaine d'application n'est pas déchargé. Cela signifie que si vous modifiez un fichier de configuration alors que le serveur est soumis à une forte sollicitation de la mémoire, les requêtes et travaux en cours d'exécution risquent de ne pas démarrer ou de ne pas se terminer.<br /><br /> En cas d'échec d'allocation de mémoire, tous les domaines d'application redémarrent immédiatement. Les travaux et requêtes en cours d'exécution sont supprimés. Vous devez les redémarrer manuellement.|  
  
## <a name="planned-and-unplanned-recycle-operations"></a>Opérations de recyclage planifiées et non planifiées  
 Les opérations de recyclage sont planifiées ou non planifiées en fonction des conditions qui en sont à l'origine :  
  
-   Les opérations de recyclage planifiées se produisent à des intervalles réguliers définis dans le fichier RSReportServer.config. L'intervalle par défaut est de 12 heures. Il s'agit d'une pratique courante pour les applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] qui favorisent l'intégrité de l'ensemble du processus. Pour les opérations de recyclage planifiées, le serveur de rapports crée des domaines d'application supplémentaires pour les nouvelles requêtes. Les requêtes actives sont autorisées à terminer leur exécution dans le domaine d'application actuel jusqu'à l'expiration du délai. Les paramètres de configuration qui régissent les opérations de recyclage planifiées sont définis pour l'ensemble du serveur. Vous ne pouvez pas configurer une autre planification du recyclage ou un autre seuil de mémoire pour chaque application.  
  
-   Les opérations de recyclage non planifiées se produisent de manière arbitraire en réponse à des modifications de configuration, une sollicitation de la mémoire et des échecs d'allocation de mémoire :  
  
    -   Pour les modifications de configuration, le serveur de rapports essaie d'utiliser un recyclage léger qui redirige les nouvelles requêtes vers une nouvelle instance du domaine d'application. En cas d'échec du recyclage léger, le serveur démarre un recyclage forcé du domaine d'application, ce qui entraîne l'annulation de toutes les requêtes actives, l'arrêt des domaines d'application actuels et le redémarrage des domaines d'application.  
  
    -   Les échecs d'allocation de mémoire indiquent que les ressources système sont insuffisantes pour la quantité de rapports à traiter par le serveur. Une opération de recyclage forcée pour tous les domaines d'application se produit en réponse à un échec d'allocation de mémoire. Toutes les files d'attente des requêtes sont effacées. Les requêtes annulées ne sont pas redémarrées. Les utilisateurs qui consultaient un rapport de manière interactive doivent actualiser ou rouvrir ce dernier. Un traitement planifié se produira à la prochaine heure planifiée. Si le délai est inacceptable, vous pouvez actualiser un instantané de rapport manuellement, ou modifier une planification d'abonnement ou une planification d'instantané de rapport, afin qu'elle s'exécute immédiatement.  
  
 Les domaines d'application du service Web Report Server, du Gestionnaire de rapports et de l'application de traitement en arrière-plan peuvent être recyclés ensemble ou individuellement, selon les circonstances dans lesquelles le recyclage se produit :  
  
-   Les opérations de recyclage débutées par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] affectent uniquement les applications [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] : service Web Report Server et Gestionnaire de rapports. [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] recycle les domaines d'application si des modifications sont apportées aux fichiers qu'il surveille. Les opérations de recyclage débutées par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] sont généralement indépendantes des opérations de recyclage de l'application de traitement en arrière-plan.  
  
-   En règle générale, les opérations de recyclage débutées par le serveur de rapports affectent le service Web Report Server, le Gestionnaire de rapports et l'application de traitement en arrière-plan. Les opérations de recyclage se produisent en réponse aux modifications des paramètres de configuration et aux redémarrages du service.  
  
## <a name="rsreportserver-configuration-settings-for-application-domains"></a>Paramètres de configuration de RSReportServer pour les domaines d'application  
 Les paramètres de configuration sont spécifiés dans le [fichier RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md). L'exemple suivant illustre les paramètres de configuration par défaut pour le comportement du recyclage de domaine d'application planifié.  
  
 `<RecycleTime>720</RecycleTime>`  
  
 `<MaxAppDomainUnloadTime>30</MaxAppDomainUnloadTime>`  
  
 Le tableau suivant décrit ces éléments.  
  
|Élément|S'applique à|Définition|  
|-------------|----------------|----------------|  
|**RecycleTime**|Tous les trois domaines d'application de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|Spécifie la fréquence à laquelle les domaines d'application sont recyclés. La planification du recyclage par défaut est conforme au modèle de 12 heures adopté généralement pour le recyclage de domaine d’application [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Au moment prévu, l'ensemble des nouvelles requêtes est transmis à une nouvelle instance du domaine d'application. Les requêtes actives dans l'instance d'origine sont autorisées à terminer leur exécution. Une fois tous les processus terminés, l'instance d'origine est supprimée et la nouvelle instance devient la seule et unique instance active du domaine d'application.<br /><br /> La valeur par défaut est 720 minutes.|  
|**MaxAppDomainUnloadTime**|Un domaine d'application de traitement en arrière-plan uniquement|Par défaut, un serveur de rapports alloue un délai d'attente de 30 minutes, durant lequel un domaine d'application est autorisé à s'arrêter au cours d'une opération de recyclage. Si les travaux en cours de traitement ne peuvent pas s'accomplir dans le délai imparti (ou si un travail prend plus de temps que ce qui a été accordé), l'instance du domaine d'application est redémarrée immédiatement. Tous les travaux inachevés prennent fin.<br /><br /> Pour plus d’informations sur l’état ou l’annulation des travaux qui s’exécutent sur le serveur de rapports, consultez [Annuler les travaux du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/cancel-report-server-jobs-management-studio.md).|  
  
> [!NOTE]  
>  Bien que le service Web Report Server et le Gestionnaire de rapports soient des applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] , aucune d'entre elles ne répond au recyclage de domaine d'application planifié qui peut être spécifié dans machine.config pour les applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] hébergées dans les services IIS (Internet Information Services).  
  
## <a name="see-also"></a> Voir aussi  
 [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)  
  
  
