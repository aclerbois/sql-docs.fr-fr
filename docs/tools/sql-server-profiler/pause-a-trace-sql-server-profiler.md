---
title: Suspendre une Trace (SQL Server Profiler) | Documents Microsoft
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.suite: sql
ms.technology: profiler
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- pausing traces
- temporarily stopping traces
- traces [SQL Server], pausing
- stopping traces
ms.assetid: 432b9b0c-b5e7-47f3-a71b-310fb3bf2445
caps.latest.revision: 24
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: c58d67783fa415936c74c26167517ca7b1933788
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pause-a-trace-sql-server-profiler"></a>Suspendre une trace (SQL Server Profiler)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le fait de marquer une pause dans une trace empêche la capture de nouvelles données d’événement jusqu'au redémarrage de la trace.  
  
 Lorsque vous suspendez une trace, vous empêchez  la capture des données d’événements jusqu'à ce que vous redémarriez la trace. Le redémarrage d'une trace permet de reprendre les opérations de trace. Aucune donnée de trace précédente n'est perdue après le redémarrage. Lors du redémarrage de la trace, la capture des données reprend à partir du point où elle s'était arrêtée. Lorsqu'une trace est suspendue, vous pouvez modifier le nom, les événements, les colonnes et les filtres. Toutefois, vous ne pouvez pas modifier les destinations des données de la trace, ni la connexion au serveur.  
  
### <a name="to-pause-a-trace"></a>Pour interrompre une trace  
  
1.  Sélectionnez une fenêtre contenant une trace en cours d'exécution.  
  
2.  Dans le menu **Fichier** , cliquez sur **Suspendre la trace**.  
  
## <a name="see-also"></a> Voir aussi  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
