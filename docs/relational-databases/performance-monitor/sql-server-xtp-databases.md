---
title: Bases de données SQL Server XTP | Microsoft Docs
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
- SQL Server 2016 XTP Databases
ms.assetid: 488ff55e-173f-43f6-9bdb-67b35e7cebfe
caps.latest.revision: 3
author: dagiro
ms.author: v-dagir
manager: craigg
ms.openlocfilehash: a5550c9a6df1ae646d4960a0d9eca8bd5a43bec4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-xtp-databases"></a>Bases de données SQL Server XTP
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

l’objet de performances **Bases de données SQL Server XTP** fournit des compteurs spécifiques de base de données de performance fournit des compteurs spécifiques de base de données OLTP en mémoire.

> [!NOTE]
>  Les compteurs de bases de données SQL Server XTP ne sont pas actuellement visibles à partir de sys.dm_os_performance_counters.  Les compteurs sont visibles depuis le [Moniteur système](../../relational-databases/performance/start-system-monitor-windows.md).

Le tableau suivant décrit les compteurs **Bases de données SQL Server XTP** .

|Compteur|Description| 
|-------------|-----------------|  
|**Taille moyenne des données volumineuses des segments de transaction**|Taille moyenne de la charge utile des données volumineuses des segments de transaction. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Taille moyenne des segments de transaction**|Taille moyenne de la charge utile des segments de transaction. Si cette valeur tombe à zéro, un nombre plus important de pages est alloué à partir de l’allocateur principal. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Profondeur de la file d’attente des threads de vidage de 256 Ko**|Profondeur de la file d’attente des threads de vidage pour les demandes d’E/S de 256 Ko.|
|**Profondeur de la file d’attente des threads de vidage de 4 Ko**|Profondeur de la file d’attente des threads de vidage pour les demandes d’E/S de 4 Ko.|
|**Profondeur de la file d’attente des threads de vidage de 64 Ko**|Profondeur de la file d’attente des threads de vidage pour les demandes d’E/S de 64 Ko.|
|**E/S de vidage des threads figés/s (256 Ko)**|Nombre de demandes d’E/S de 256 Ko rencontrées lors du traitement de la page de vidage situées au-dessus du seuil de blocage et qui ne peuvent par conséquent pas être émises.|
|**E/S de vidage des threads figés/s (4 Ko)**|Nombre de demandes d’E/S de 4 Ko rencontrées lors du traitement de la page de vidage situées au-dessus du seuil de blocage et qui ne peuvent par conséquent pas être émises.|
|**E/S de vidage des threads figés/s (64 Ko)**|Nombre de demandes d’E/S de 64 Ko rencontrées lors du traitement de la page de vidage situées au-dessus du seuil de blocage et qui ne peuvent par conséquent pas être émises.|
|**Nombre de listes libres IoPagePool256K**|Nombre de pages dans la liste libre du pool de pages d’E/S de 256 Ko. Si cette valeur tombe à zéro, un nombre plus important de pages est alloué à partir de l’allocateur principal. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**IoPagePool256K alloués au total**|Nombre total de pages allouées et détenues par le pool de page d’E/S de 256 Ko à partir de l’allocateur principal. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre de listes libres IoPagePool4K**|Nombre de pages dans la liste libre du pool de pages d’E/S de 4 Ko. Si cette valeur tombe à zéro, un nombre plus important de pages est alloué à partir de l’allocateur principal. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**IoPagePool4K alloués au total**|Nombre total de pages allouées et détenues par le pool de pages d’E/S de 4 Ko à partir de l’allocateur principal. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre de listes libres IoPagePool64K**|Nombre de pages dans la liste libre du pool de pages d’E/S de 64 Ko. Si cette valeur tombe à zéro, un nombre plus important de pages est alloué à partir de l’allocateur principal. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**IoPagePool64K alloués au total**|Nombre total de pages allouées et détenues par le pool de pages d’E/S de 64 Ko à partir de l’allocateur principal. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre de développements 256 Ko MtLog**|Nombre de fois qu’un 256 Ko MtLog a été développé. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**E/S 256 Ko MtLog en suspens**|Nombre de demandes d’E/S de 256 Ko en suspens émises par MtLog.|
|**Pourcentage de remplissage des pages de 256 Ko MtLog/Page vidée**|Pourcentage de remplissage moyen de chaque page MtLog de 256 Ko vidée. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre d’octets écrits par 256 Ko MtLog/s**|Nombre d’octets en écriture par seconde sur les objets 256 Ko MtLog. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre de développements 4 Ko MtLog**|Nombre de fois qu’un 4 Ko MtLog a été développé. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**E/S 4 Ko MtLog en suspens**|Nombre de demandes d’E/S de 4 Ko en suspens émises par MtLog.|
|**Pourcentage de remplissage des pages de 4 Ko MtLog/Page vidée**|Pourcentage de remplissage moyen de chaque page MtLog de 4 Ko vidée. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre d’octets écrits par 4 Ko MtLog/s**|Nombre d’octets en écriture par seconde sur les objets 4 Ko MtLog. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre de développements 64 Ko MtLog**|Nombre de fois qu’un 64 Ko MtLog a été développé. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**E/S 64 Ko MtLog en suspens**|Nombre de demandes d’E/S de 64 Ko en suspens émises par MtLog.|
|**Pourcentage de remplissage des pages de 64 Ko MtLog/Page vidée**|Pourcentage de remplissage moyen de chaque page MtLog de 64 Ko vidée. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre d’octets écrits par 64 Ko MtLog/s**|Nombre d’octets en écriture par seconde sur les objets 64 Ko MtLog. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre de fusions**|Nombre de fusions en cours d’exécution.|
|**Nombre de fusions/s**|Nombre de fusions créées par seconde (en moyenne).|
|**Nombre de sérialisations**|Nombre de sérialisations en cours d’exécution.|
|**Nombre de sérialisations/s**|Nombre de sérialisations créées par seconde (en moyenne).|
|**Nombre de pages de fin du cache**|Nombre de pages allouées en fin de cache. Il s'agit d'un compteur de très bas niveau, non destiné au client.|
|**Nombre maximal de pages de fin du cache**|Nombre maximal de pages allouées en fin de cache. Il s'agit d'un compteur de très bas niveau, non destiné au client.|


## <a name="see-also"></a> Voir aussi  
[SQL Server XTP &#40;OLTP en mémoire&#41;, compteurs de performances](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)
