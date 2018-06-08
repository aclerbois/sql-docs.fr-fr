---
title: Propriétés de la fonctionnalité | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bec4d89fd1135ecc7bfdbc563547b9d3dbbdfa47
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="feature-properties"></a>Propriétés de fonctionnalité
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les propriétés de fonctionnalité se rapportent à des fonctionnalités de produit, le plus souvent de caractère avancé. Il s'agit notamment de propriétés qui contrôlent les liaisons entre les instances de serveur.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge les propriétés de serveur répertoriées dans le tableau suivant. Pour plus d’informations sur les autres propriétés de serveur et sur la façon de les configurer, consultez [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md).  
  
 **S'applique à :** Mode serveur multidimensionnel uniquement  
  
## <a name="properties"></a>Propriétés  
  
|Propriété|Par défaut| Description|  
|--------------|-------------|-----------------|  
|**ManagedCodeEnabled**|1|Propriété booléenne qui indique si les procédures de stockage du Common Language Runtime sont activées.|  
|**LinkInsideInstanceEnabled**|1|Propriété booléenne qui indique si un objet lié peut être créé dans la même instance du serveur.|  
|**LinkToOtherInstanceEnabled**|0|Propriété booléenne qui indique s'il est possible d'établir des liens à des objets résidant sur des serveurs distants.|  
|**LinkFromOtherInstanceEnabled**|0|Propriété booléenne qui indique s'il est possible d'établir des liens à des objets à partir d'autres instances du serveur.|  
|**ConnStringEncryptionEnabled**|1|Propriété booléenne qui indique si la chaîne de connexion est chiffrée dans le fichier de configuration du serveur.|  
|**UseCachedPageAllocators**|0|Propriété booléenne qui indique si les allocateurs de pages en cache sont activés.|  
|**ComUdfEnabled**|0|Propriété booléenne qui indique si les fonctions définies comme objets COM par l'utilisateur sont activées.|  
|**SQMSupportEnabled**|1|Propriété booléenne qui indique si des rapports d'erreurs et d'utilisation de fonctionnalités sont envoyés automatiquement à [!INCLUDE[msCoName](../../includes/msconame-md.md)] .|  
|**ResourceMonitoringEnabled**|1|Propriété booléenne qui indique si des compteurs d'analyse des ressources internes sont activés. Cette propriété est activée par défaut. Lorsqu'elle est activée, cette propriété permet aux compteurs de recueillir des données d'utilisation relatives au processeur, à la mémoire et aux activités d'E/S.<br /><br /> Des compteurs d'analyse des ressources internes sont utilisés par les vues de gestion dynamique (DMV) qui s'intéressent à l'utilisation des ressources. Si vous désactivez cette propriété, les requêtes DMV continuent à s'exécuter, mais le jeu de résultats ne sera pas valide. Les vues DMV qui dépendent de cette propriété sont notamment :<br /><br /> **DISCOVER_OBJECT_ACTIVITY**<br /><br /> **DISCOVER_COMMAND_OBJECTS**<br /><br /> **DISCOVER_SESSIONS** (pour SESSION_READS, SESSION_WRITES, SESSION_CPU_TIME_MS)<br /><br /> <br /><br /> Remarque : sur un système multicœur qui présente une architecture NUMA, la désactivation de cette propriété peut améliorer les performances de requête, notamment pour les charges de travail multi-utilisateur élevées. Vous devrez effectuer des tests de comparaison afin de déterminer si les performances de requête sont améliorées lorsque vous modifiez cette propriété. Pour connaître les meilleures pratiques concernant l'exécution de tests de comparaison, y compris l'effacement du cache et les erreurs courantes à éviter, consultez le document [SQL Server 2008 R2 Analysis Services Operations Guide](http://go.microsoft.com/fwlink/?LinkID=225539).|  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md)   
 [Déterminer le mode serveur d'une instance Analysis Services](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md)   
 [Utiliser dynamique vues de gestion & #40 ; vues de gestion dynamique & #41 ; pour surveiller Analysis Services](../../analysis-services/instances/use-dynamic-management-views-dmvs-to-monitor-analysis-services.md)  
  
  
