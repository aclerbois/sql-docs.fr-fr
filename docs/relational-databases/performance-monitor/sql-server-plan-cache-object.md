---
title: SQL Server, objet Plan Cache | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Plan Cache object
- SQLServer:Plan Cache
ms.assetid: 225e2b02-8d2f-4f29-9eba-f5847c36ea99
caps.latest.revision: 25
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 5e46cebc6bfb2443f78d15a54b3ec59da5169d59
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-plan-cache-object"></a>SQL Server - Objet Plan Cache
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L’objet **Plan Cache** fournit des compteurs qui permettent de surveiller l’utilisation de la mémoire par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker des objets tels que des procédures stockées, des instructions et des déclencheurs [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc et préparés. Vous pouvez surveiller simultanément plusieurs instances de l’objet **Plan Cache** , chacune représentant un type différent de plan de requête à surveiller.  
  
 Ce tableau décrit les compteurs **SQLServer:Plan Cache**.  
  
|Compteurs Plan Cache SQL Server|Description|  
|------------------------------------|-----------------|  
|**Taux d'accès au cache**|Rapport entre les présences dans le cache et les recherches.|  
|**Base du taux d’accès au cache**|À usage interne uniquement| 
|**Nombre d'objets cache**|Nombre d'objets cache dans le cache.|  
|**Pages du cache**|Nombre de pages de 8 Ko utilisées par des objets cache.|  
|**Objets cache en cours d'utilisation**|Nombre d'objets cache en cours d'utilisation.|  
  
 Chaque compteur de l'objet contient les instances suivantes :  
  
|Instance Plan Cache|Description|  
|-------------------------|-----------------|  
|**_Total**|Informations sur tous les types d'instances du cache.|  
|**Plans Sql**|Plans de requêtes créés à partir d’une requête ad hoc [!INCLUDE[tsql](../../includes/tsql-md.md)] , notamment les requêtes paramétrables automatiquement, ou à partir d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] préparées au moyen de **sp_prepare** or **sp_cursorprepare**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] place en mémoire cache les plans des instructions ad hoc [!INCLUDE[tsql](../../includes/tsql-md.md)] pour une réutilisation ultérieure si l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] identique est exécutée par la suite. Les requêtes paramétrables par l'utilisateur (même sans préparation explicite) sont également surveillées en tant que plans SQL préparés.|  
|**Objet Plans**|Plans de requête générés par la création d'une procédure stockée, d'une fonction ou d'un déclencheur.|  
|**Arborescences liées**|Arborescences normalisées pour les vues, les règles, les colonnes calculées et les contraintes de vérification.|  
|**Procédures stockées étendues**|Informations sur les catalogues pour les procédures stockées étendues.|  
|**Tables temporaires & Variables de table**|Informations sur le cache concernant les tables temporaires et les variables de table.|  
  
## <a name="see-also"></a> Voir aussi  
 [Mémoire du serveur (option de configuration de serveur)](../../database-engine/configure-windows/server-memory-server-configuration-options.md)   
 [SQL Server, objet Gestionnaire de tampons](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)   
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
