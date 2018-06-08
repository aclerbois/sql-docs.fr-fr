---
title: Groupes de disponibilité avec échelle lecture | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: 9
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: dca3919c6ec8b74342122a750da6d4b77e37d93c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="read-scale-availability-groups"></a>Groupes de disponibilité avec échelle lecture
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Un groupe de disponibilité est une solution complète qui fournit des fonctionnalités de haute disponibilité à SQL Server, ainsi que des solutions de mise à l’échelle intégrées. Dans une application de base de données classique, plusieurs clients exécutent divers types de charge de travail. Des goulots d’étranglement peuvent parfois apparaître à cause d’une insuffisance de ressources. Vous pouvez libérer des ressources et obtenir un débit plus élevé pour la charge de travail OLTP. Vous pouvez également fournir une mise à l’échelle et des performances plus élevées sur les charges de travail en lecture seule. Tirez parti de la technologie de réplication la plus rapide pour SQL Server et créez un groupe de bases de données répliquées pour décharger les charges de travail de reporting et d’analytique sur des réplicas en lecture seule. 

Avec les groupes de disponibilité, un ou plusieurs réplicas secondaires peuvent être configurés pour prendre en charge un accès en lecture seule aux bases de données secondaires.

Les applications clientes qui exécutent des charges de travail de reporting et d’analytique peuvent se connecter directement aux bases de données secondaires. Vous pouvez également définir une liste de routage en lecture seule et vous connecter à la base de données primaire. Elle transfère alors la demande de connexion à chacun des réplicas secondaires de la liste de routage en mode tourniquet (round-robin).

## <a name="read-scale-availability-groups-without-cluster"></a>Groupes de disponibilité avec échelle lecture sans cluster

Dans [!INCLUDE[sssql15-md](..\..\..\includes\sssql15-md.md)] et antérieur, tous les groupes de disponibilité avaient besoin d’un cluster. Ce cluster assurait la continuité de l’activité avec la haute disponibilité et la récupération d’urgence (HADR). De plus, les réplicas secondaires étaient configurés pour des opérations de lecture. Même si la haute disponibilité n’était pas la finalité, une charge opérationnelle considérable était utilisée pour configurer et exécuter un cluster. SQL Server 2017 introduit les groupes de disponibilité avec échelle lecture sans aucun cluster. 

Si l’impératif de votre entreprise est de préserver les ressources pour les charges de travail critiques qui s’exécutent sur le réplica principal, vous pouvez désormais utiliser le routage en lecture seule ou vous connecter directement à des réplicas secondaires lisibles. Vous n’avez pas besoin de dépendre de l’intégration à une technologie de clustering. Ces nouvelles fonctionnalités sont disponibles pour SQL Server 2017 exécuté sur les plateformes Windows et Linux.

>[!IMPORTANT]
>Il ne s’agit pas d’une configuration de haute disponibilité. Il n’existe aucune infrastructure pour surveiller et coordonner la détection d’échec et le basculement automatique. Sans cluster, SQL Server ne peut pas offrir l’objectif de temps de récupération faible qu’offre une solution de haute disponibilité automatisée. Si vous avez besoin de fonctionnalités de haute disponibilité, utilisez un gestionnaire de clusters (clustering de basculement Windows Server sur Windows ou Pacemaker sur Linux). 
>
>Le groupe de disponibilité avec échelle lecture peut fournir la fonctionnalité de récupération d’urgence. Quand les réplicas en lecture seule sont en mode de validation synchrone, ils fournissent un objectif de point de récupération égal à zéro. Pour basculer un groupe de disponibilité avec échelle lecture, consultez [Basculer le réplica principal sur un groupe de disponibilité avec échelle lecture](perform-a-planned-manual-failover-of-an-availability-group-sql-server.md#ReadScaleOutOnly).

## <a name="use-distributed-availability-groups-for-geographic-read-scale"></a>Utiliser des groupes de disponibilité distribués pour l’échelle lecture géographique

Les solutions géographiquement séparées permettent d’implémenter des solutions d’échelle lecture avec des groupes de disponibilité distribués. Vous pouvez les utiliser pour décharger des charges de travail de lecture depuis le réplica principal vers les réplicas secondaires lisibles sur des sites plus proches de la source des charges de travail de lecture. Les groupes de disponibilité distribués réduisent l’utilisation des ressources sur le réplica principal. Ils aident également le débit de lecture en réduisant la latence du réseau et en tirant parti des ressources dédiées.

Un même groupe de disponibilité distribué peut avoir jusqu’à 17 réplicas secondaires lisibles. Pour augmenter la capacité de mise à l’échelle, créez une chaîne en série de plusieurs groupes de disponibilité afin d’augmenter encore plus le nombre de réplicas lisibles. Vous pouvez également déployer deux groupes de disponibilité distribués à partir du même groupe de disponibilité pour atteindre des lectures à faible latence dans les environnements éloignés les uns des autres.




## <a name="next-steps"></a>Étapes suivantes 

[Configurer un groupe de disponibilité avec échelle lecture sur Linux](../../../linux/sql-server-linux-availability-group-configure-rs.md)

## <a name="see-also"></a>Voir aussi 
 [Vue d’ensemble des groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
  
  
