---
title: Créer des graphiques, des alertes, des journaux et des rapports | Microsoft Docs
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
- System Monitor [SQL Server], charts and reports
- charts [SQL Server]
- reports [SQL Server]
- reports [SQL Server], creating
- Windows System Monitor [SQL Server], charts and reports
- logs [SQL Server], System Monitor
- System Monitor [SQL Server], logs
- Windows System Monitor [SQL Server], logs
ms.assetid: c9162b37-e5dc-43d1-a3aa-1e9ebc69fecc
caps.latest.revision: 21
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 02a4ab9af70761821edb91113e53df6502ea4b19
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-charts-alerts-logs-and-reports"></a>Créer des graphiques, des alertes, des journaux et des rapports
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le Moniteur système vous permet de créer des graphiques, des alertes, des journaux et des rapports pour surveiller une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="charts"></a>Graphiques  
 Les graphiques permettent de surveiller les performances en cours des objets et des compteurs sélectionnés (par exemple l'utilisation de l'UC ou les E/S du disque). Il est possible d'ajouter à un graphique diverses combinaisons d'objets et de compteurs du Moniteur système. Vous pouvez également ajouter des objets et compteurs [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows à un graphique.  
  
 Chaque graphique représente un sous-ensemble des informations à surveiller. Par exemple, un premier graphique peut assurer le suivi des statistiques d'utilisation de la mémoire et un deuxième celui des E/S du disque.  
  
 L'utilisation d'un graphique peut s'avérer utile pour les tâches suivantes :  
  
-   rechercher l'explication de la lenteur ou de l'inefficacité d'un ordinateur ou d'une application ;  
  
-   surveiller les systèmes en continu pour détecter des problèmes de performances intermittents ;  
  
-   comprendre pourquoi la capacité doit être accrue ;  
  
-   afficher une tendance sous forme de courbe ;  
  
-   afficher une comparaison sous forme d'histogramme.  
  
 Les graphiques sont utiles pour une surveillance à cours terme et en temps réel d'un ordinateur local ou distant (par exemple pour surveiller un événement lorsqu'il se produit).  
  
## <a name="alerts"></a>Alertes  
 En utilisant des alertes, le Moniteur système peut suivre des événements spécifiques et vous les signaler si vous le lui avez demandé. Un journal d'alerte peut surveiller les performances en cours des compteurs sélectionnés et les instances des objets de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quand un compteur dépasse une valeur donnée, le journal enregistre la date et l'heure de l'événement. Un événement peut également générer une alerte sur le réseau. Vous pouvez spécifier le programme qui sera exécuté la première fois, ou à chaque fois, qu'un événement se produit. Par exemple, une alerte peut envoyer un message sur le réseau à tous les administrateurs du système pour les avertir que l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est bientôt à court d'espace disque.  
  
## <a name="logs"></a>Journaux  
 Les journaux vous permettent d'enregistrer les informations relatives à l'activité en cours des objets et des ordinateurs sélectionnés afin de les afficher et de les analyser ultérieurement. Vous pouvez regrouper des données de plusieurs systèmes dans un seul fichier journal. Par exemple, vous pouvez créer divers journaux afin de réunir les informations relatives aux performances des objets sélectionnés sur divers ordinateurs en vue d'une analyse future. Vous pouvez sauvegarder ces sélections sous un nom de fichier et les réutiliser pour créer, à des fins de comparaison, un autre journal contenant des informations similaires.  
  
 Les fichiers journaux sont riches en informations utiles pour le dépannage ou la planification. Alors que les graphiques, les alertes, et les rapports relatifs à l'activité en cours offrent une réponse instantanée, les fichiers journaux vous permettent de suivre les compteurs sur une longue durée. Ainsi, vous pouvez étudier les informations plus en détail, afin de documenter les performances du système.  
  
## <a name="reports"></a>Rapports  
 Les rapports vous permettent d'afficher les valeurs de compteurs et d'instances qui varient constamment pour les objets sélectionnés. Les valeurs apparaissent en colonnes pour chaque instance. Vous pouvez définir l'intervalle séparant chaque rapport, imprimer des instantanés et exporter des données. Utilisez les rapports pour afficher les nombres bruts.  
  
 Pour plus d'informations sur la création des graphiques, alertes, journaux et rapports, ou encore sur les objets et compteurs de Windows, consultez la documentation de Windows.  
  
## <a name="see-also"></a> Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
