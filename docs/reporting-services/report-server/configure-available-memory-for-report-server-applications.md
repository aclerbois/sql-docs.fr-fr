---
title: Configurer la mémoire disponible pour les applications du serveur de rapports | Microsoft Docs
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
- memory [Reporting Services]
- memory thresholds [Reporting Services]
ms.assetid: ac7ab037-300c-499d-89d4-756f8d8e99f6
caps.latest.revision: 49
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: cd3c8b2a1d803610d0e1e6f086097d56aead9f77
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-available-memory-for-report-server-applications"></a>Configurer la mémoire disponible pour les applications du serveur de rapports
  Bien que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] puisse utiliser toute la mémoire disponible, vous pouvez substituer le comportement par défaut en configurant une limite supérieure pour la quantité totale des ressources de mémoire allouées aux applications serveur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Vous pouvez également définir des seuils qui obligent le serveur de rapports à modifier son mode de traitement des requêtes et leur classement par ordre de priorité, selon que la sollicitation de la mémoire est faible, moyenne ou élevée. Face à de faibles niveaux de sollicitation de la mémoire, le serveur de rapports répond en donnant une priorité légèrement supérieure au traitement de rapport interactif ou à la demande. Face à des niveaux élevés de sollicitation de la mémoire, le serveur de rapports utilise plusieurs techniques pour rester opérationnel en utilisant les ressources limitées dont il dispose.  
  
 Cette rubrique décrit les paramètres de configuration que vous pouvez spécifier, ainsi que la façon dont le serveur répond quand la sollicitation de la mémoire devient un facteur de traitement des requêtes.  
  
## <a name="memory-management-policies"></a>Stratégies de gestion de la mémoire  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] répond aux limitations des ressources système en ajustant la quantité de mémoire allouée en fonction des applications et des types de requêtes de traitement. Les applications qui s'exécutent dans le service Report Server et qui sont soumises à la gestion de la mémoire sont les suivantes :  
  
-   Gestionnaire de rapports, une application Web frontale pour le serveur de rapports ;  
  
-   service Web Report Server, pour les requêtes de traitement de rapport interactif et à la demande ;  
  
-   application de traitement en arrière-plan, pour le traitement de rapport planifié, la remise d'abonnement et la maintenance de base de données.  
  
 Les stratégies de gestion de la mémoire s'appliquent à l'ensemble du service Report Server, et non aux applications individuelles qui s'exécutent au sein du processus.  
  
 S'il n'y a aucune sollicitation de la mémoire sur le système, chaque application serveur demande de la mémoire au démarrage, avant de recevoir des requêtes, afin d'offrir des performances optimales lorsque les requêtes sont finalement reçues. Face à l'accroissement de la sollicitation de la mémoire, le serveur de rapports ajuste son modèle de processus, comme indiqué dans le tableau suivant.  
  
|Sollicitation de la mémoire|Réponse du serveur|  
|---------------------|---------------------|  
|Faible|Les requêtes actives continuent d'être traitées. Les nouvelles requêtes sont presque toujours acceptées. Les requêtes adressées à l'application de traitement en arrière-plan reçoivent une priorité plus faible que les requêtes adressées au service Web Report Server.|  
|Moyenne|Les requêtes actives continuent d'être traitées. Les nouvelles requêtes peuvent éventuellement être acceptées. Les requêtes adressées à l'application de traitement en arrière-plan reçoivent une priorité plus faible que les requêtes adressées au service Web Report Server. Les allocations de mémoire pour les trois applications serveur sont réduites ; la réduction est relativement plus importante pour le traitement en arrière-plan, ce qui permet d'offrir davantage de mémoire aux requêtes du service Web.|  
|Élevée|L'allocation de mémoire est davantage réduite. Les applications serveur qui requièrent plus de mémoire sont refusées. Les requêtes actuelles sont ralenties et mettent plus de temps à s'exécuter. Les nouvelles requêtes ne sont pas acceptées. Le serveur de rapports place les fichiers de données en mémoire sur disque.<br /><br /> Si les contraintes de mémoire deviennent très importantes et s'il n'y a pas de mémoire disponible pour gérer les nouvelles requêtes, le serveur de rapports retourne l'erreur HTTP 503 relative à l'indisponibilité du serveur pendant que les requêtes actives finissent de s'exécuter. Dans certains cas, les domaines d'application peuvent être recyclés pour réduire immédiatement la sollicitation de la mémoire.|  
  
 Bien que vous ne puissiez pas personnaliser les réponses du serveur de rapports aux différents scénarios de sollicitation de la mémoire, vous pouvez spécifier des paramètres de configuration qui définissent les limites séparant les réponses aux sollicitations élevée, moyenne et faible de la mémoire.  
  
## <a name="when-to-customize-memory-management-settings"></a>Quand personnaliser les paramètres de gestion de la mémoire  
 Les paramètres par défaut spécifient des plages inégales pour une sollicitation faible, moyenne et élevée de la mémoire. Par défaut, la zone correspondant à une faible sollicitation de la mémoire est plus importante que les zones correspondant aux sollicitations moyenne et élevée de la mémoire. Cette configuration est optimale pour les charges de traitement qui sont distribuées de manière égale, ou qui s'accroissent ou se réduisent de façon incrémentielle. Dans ce scénario, la transition entre les zones est graduelle ; par ailleurs, le serveur de rapports a le temps d'ajuster sa réponse.  
  
 La modification des paramètres par défaut est utile si le modèle de charge inclut des pics. Lorsqu'il y a des pics soudains dans la charge de traitement, le serveur de rapports peut passer très rapidement de l'absence de sollicitation de la mémoire aux échecs d'allocation de mémoire. Cela peut se produire si plusieurs instances simultanées d'un rapport utilisant beaucoup de mémoire démarrent en même temps. Pour gérer ce type de charge de traitement, le serveur de rapports doit pouvoir répondre le plus rapidement possible à une sollicitation moyenne ou élevée de la mémoire, afin d'entraîner un ralentissement du traitement. Cela permet l'exécution d'un plus grand nombre de requêtes. Pour ce faire, vous devez baisser la valeur de **MemorySafetyMargin** afin que la zone de réponse à une faible sollicitation de la mémoire soit plus petite que les autres zones. Ainsi, les réponses aux sollicitations moyenne et élevée de la mémoire se produiront plus tôt.  
  
## <a name="configuration-settings-for-memory-management"></a>Paramètres de configuration pour la gestion de la mémoire  
 Les paramètres de configuration qui contrôlent l’allocation de mémoire pour le serveur de rapports sont **WorkingSetMaximum**, **WorkingSetMinimum**, **MemorySafetyMargin**et **MemoryThreshold**.  
  
-   **WorkingSetMaximum** et **WorkingSetMinimum** définissent la plage de mémoire disponible. Vous pouvez configurer ces paramètres pour définir une plage de mémoire disponible pour les applications du serveur de rapports. Cela peut être utile si vous hébergez plusieurs applications sur le même ordinateur et si vous réalisez que le serveur de rapports utilise une quantité disproportionnée de ressources système par rapport aux autres applications du même ordinateur.  
  
-   **MemorySafetyMargin** et **MemoryThreshold** définissent les limites des niveaux faible, moyen et élevé de sollicitation de la mémoire. Pour chaque état, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] entreprend une action corrective visant à garantir une gestion appropriée du traitement des rapports et des autres requêtes en fonction de la quantité de mémoire disponible sur l’ordinateur. Vous pouvez spécifier les paramètres de configuration qui déterminent la limite entre les niveaux faible, moyen et élevé de sollicitation de la mémoire.  
  
     Bien que vous puissiez modifier les paramètres de configuration, cela n'améliorera pas les performances de traitement des rapports. La modification des paramètres de configuration n'est utile que si les requêtes sont supprimées avant la fin de leur exécution. La meilleure méthode pour améliorer les performances du serveur est de déployer le serveur de rapports ou des applications individuelles du serveur de rapports sur des ordinateurs dédiés.  
  
 L'illustration suivante montre comment les paramètres sont utilisés conjointement pour distinguer les niveaux faible, moyen et élevé de sollicitation de la mémoire :  
  
 ![Paramètres de configuration de l’état de la mémoire](../../reporting-services/report-server/media/rs-memoryconfigurationzones.gif "Paramètres de configuration de l’état de la mémoire")  
  
 Le tableau suivant décrit les paramètres **WorkingSetMaximum**, **WorkingSetMinimum**, **MemorySafetyMargin**et **MemoryThreshold** . Les paramètres de configuration sont spécifiés dans le [fichier RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
|Élément|Description|  
|-------------|-----------------|  
|**WorkingSetMaximum**|Spécifie un seuil de mémoire au-delà duquel aucune nouvelle requête d'allocation de mémoire n'est accordée aux applications du serveur de rapports.<br /><br /> Par défaut, le serveur de rapports affecte à **WorkingSetMaximum** la quantité de mémoire disponible sur l’ordinateur. Cette valeur est détectée lorsque le service démarre.<br /><br /> Ce paramètre n'apparaît pas dans le fichier RSReportServer.config à moins que vous ne l'ajoutiez manuellement. Si vous souhaitez que le serveur de rapports utilise moins de mémoire, vous pouvez modifier le fichier RSReportServer.config en ajoutant l'élément et la valeur. La plage de valeurs valides s'étend de 0 à un entier maximal. Cette valeur est exprimée en kilo-octets.<br /><br /> Quand la valeur de **WorkingSetMaximum** est atteinte, le serveur de rapports n’accepte plus de nouvelle requête. La fin de l'exécution des requêtes en cours de traitement est autorisée. Les nouvelles requêtes sont acceptées uniquement si l’utilisation de la mémoire affiche une valeur inférieure à celle spécifiée pour l’option **WorkingSetMaximum**.<br /><br /> Si les requêtes existantes continuent de consommer de la mémoire supplémentaire une fois la valeur de **WorkingSetMaximum** atteinte, tous les domaines d’application du serveur de rapports sont recyclés. Pour plus d'informations, consultez [Application Domains for Report Server Applications](../../reporting-services/report-server/application-domains-for-report-server-applications.md).|  
|**WorkingSetMinimum**|Spécifie une limite inférieure pour l'utilisation des ressources ; le serveur de rapports ne libère pas de mémoire si la quantité totale de mémoire utilisée est inférieure à cette limite.<br /><br /> Par défaut, la valeur est calculée au démarrage du service. La requête d’allocation de mémoire initiale doit représenter 60 pour cent de **WorkingSetMaximum**.<br /><br /> Ce paramètre n'apparaît pas dans le fichier RSReportServer.config à moins que vous ne l'ajoutiez manuellement. Si vous souhaitez personnaliser cette valeur, vous devez ajouter l’élément **WorkingSetMinimum** au fichier RSReportServer.config. La plage de valeurs valides s'étend de 0 à un entier maximal. Cette valeur est exprimée en kilo-octets.|  
|**MemoryThreshold**|Spécifie un pourcentage de **WorkingSetMaximum** qui définit la limite entre les scénarios correspondant à une sollicitation élevée et moyenne. Si l'utilisation de la mémoire du serveur de rapports atteint cette valeur, ce dernier ralentit le traitement des requêtes et modifie la quantité de mémoire allouée aux différentes applications serveur. La valeur par défaut est 90. Elle doit être supérieure à la valeur définie pour **MemorySafetyMargin**.|  
|**MemorySafetyMargin**|Spécifie un pourcentage de **WorkingSetMaximum** qui définit la limite entre les scénarios correspondant à une sollicitation moyenne et basse. Cette valeur est le pourcentage de mémoire disponible réservée au système et ne peut pas être utilisée pour les opérations du serveur de rapports. La valeur par défaut est 80.|  
  
> [!NOTE]  
>  Les paramètres**MemoryLimit** et **MaximumMemoryLimit** sont obsolètes dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures. Si vous avez mis à niveau une installation existante ou si vous utilisez un fichier RSReportServer.config qui comprend ces paramètres, le serveur de rapports ne lit plus ces valeurs.  
  
#### <a name="example-of-memory-configuration-settings"></a>Exemple de paramètres de configuration de la mémoire  
 L'exemple suivant illustre les paramètres de configuration d'un serveur de rapports qui utilise des valeurs de configuration de la mémoire personnalisées. Pour ajouter **WorkingSetMaximum** ou **WorkingSetMinimum**, vous devez taper les éléments et les valeurs dans le fichier RSReportServer.config. Les deux valeurs sont des entiers qui représentent les kilo-octets de mémoire vive (RAM) que vous allouez aux applications serveur. L'exemple suivant spécifie que l'allocation de mémoire totale pour les applications du serveur de rapports ne peut pas dépasser 4 gigaoctets. Si la valeur par défaut **WorkingSetMinimum** (60 % de **WorkingSetMaximum**) est acceptable, vous pouvez l’omettre et spécifier simplement **WorkingSetMaximum** dans le fichier RSReportServer.config. Cet exemple inclut **WorkingSetMinimum** afin que vous puissiez voir à quoi il ressemble, si vous décidez de l’ajouter :  
  
```  
      <MemorySafetyMargin>80</MemorySafetyMargin>  
      <MemoryThreshold>90</MemoryThreshold>  
      <WorkingSetMaximum>4000000</WorkingSetMaximum>  
      <WorkingSetMinimum>2400000</WorkingSetMinimum>  
```  
  
#### <a name="about-aspnet-memory-configuration-settings"></a>À propos des paramètres de configuration de la mémoire ASP.NET  
 Bien que le service web Report Server et le Gestionnaire de rapports soient des applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] , aucune d’entre elles ne répond aux paramètres de configuration de la mémoire spécifiés dans la section **processModel** de machine.config pour les applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] qui s’exécutent en mode de compatibilité IIS 5.0. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] lit les paramètres de configuration de la mémoire du fichier RSReportServer.config uniquement.  
  
## <a name="see-also"></a> Voir aussi  
 [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [Domaines d’application pour des applications du serveur de rapports](../../reporting-services/report-server/application-domains-for-report-server-applications.md)  
  
  
