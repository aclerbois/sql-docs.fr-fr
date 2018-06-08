---
title: Prise en charge de la haute disponibilité | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 2e0f6d3f-0536-46d9-8630-835e199515bf
caps.latest.revision: 5
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e356315c1ccf42baaf9e283549d459599faabd79
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="high-availability-support"></a>Prise en charge de la haute disponibilité
  Le service de capture de données modifiées pour Oracle est conçu pour une haute disponibilité. Les fonctionnalités suivantes fournissent une partie de la prise en charge de la haute disponibilité :  
  
-   Le service de capture de données modifiées pour Oracle n'utilise aucune ressource de fichier (locale ou autre). L'intégralité de son état est stocké dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible. Il est ainsi facile de démarrer le service sur un autre ordinateur qui utilise la même instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si l'ordinateur sur lequel le service s'exécute est défaillant. Pour réduire le temps de récupération, les transactions Oracle longues sont conservées dans une table de mise en lots dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]cible, ce qui évite la nécessité d'une nouvelle analyse de nombreux journaux des transactions Oracle après un échec (ou le redémarrage du service).  
  
-   Le service de capture de données modifiées pour Oracle peut utiliser des instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cluster, de façon à récupérer après le basculement de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers un autre nœud de cluster. L'administrateur de l'ordinateur de service de capture de données modifiées Oracle doit uniquement spécifier les informations de connexion à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cluster lorsqu'il crée un service de capture de données modifiées Oracle.  
  
-   Le service de capture de données modifiées pour Oracle peut utiliser la fonctionnalité de mise en miroir de bases de données [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]**AlwaysOn** . Cette prise en charge requiert que les bases de données MSXDBCDC et toutes bases de données CDC soient membres du même groupe de disponibilité. Elle exige également que l’administrateur de l’ordinateur de service de capture de données modifiées Oracle spécifie les informations de connexion **AlwaysOn** appropriées au groupe de disponibilité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, les propriétés de connexion `Failover_Partner and Network=dbmssocn`). Cela permet au service de capture de données modifiées de reprendre automatiquement le traitement sur une réplication secondaire des bases de données après un basculement.  
  
-   Le service de capture de données modifiées pour Oracle peut être configuré en tant que ressource de service générique sur un cluster de basculement Windows (conjointement avec, ou séparément de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]), ce qui simplifie le basculement du traitement CDC avec le cluster. Pour configurer le service de capture de données modifiées pour Oracle en tant que ressource dans un cluster de basculement, l'administrateur système doit définir le service de capture de données modifiées pour Oracle comme ressource de service générique sur chaque nœud du cluster de basculement.  
  
-   Le service de capture de données modifiées pour Oracle prend en charge Oracle RAC, ce qui lui permet de communiquer avec la base de données Oracle et de traiter les journaux même lorsqu'un des nœuds Oracle RAC est arrêté.  
  
  
