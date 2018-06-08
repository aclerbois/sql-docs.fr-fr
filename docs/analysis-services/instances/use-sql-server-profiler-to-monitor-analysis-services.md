---
title: Utiliser SQL Server Profiler pour surveiller Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e9e4407abe5565931994d9dcca4cd0326577dbf4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="use-sql-server-profiler-to-monitor-analysis-services"></a>Utiliser SQL Server Profiler pour contrôler Analysis Services
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] Le assure le suivi des événements de processus du moteur, notamment le début d’un lot ou d’une transaction, et capture des données à propos de ces événements, ce qui vous permet de surveiller l’activité des serveurs et des bases de données (par exemple, les requêtes des utilisateurs ou les connexions). Vous pouvez capturer les données du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] dans une table ou un fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , en vue d'une analyse ultérieure, et relire les événements capturés sur la même instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou sur une autre instance pour voir exactement ce qui s'est passé. Vous pouvez relire les événements en temps réel ou pas à pas. Il est également très utile d'exécuter sur la même machine les événements de trace et les compteurs de performances. SQL Profiler peut corréler les deux en fonction de l'heure et les afficher ensemble sur une même chronologie. Les événements de trace vous donneront plus de détails, tandis que les compteurs de performances vous fourniront une vue agrégée. Pour plus d’informations sur la création et l’exécution de traces, consultez [Créer des traces de SQL Server Profiler pour la relecture &#40;Analysis Services&#41;](../../analysis-services/instances/create-profiler-traces-for-replay-analysis-services.md).  
  
 Le tableau suivant décrit les rubriques de cette section.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Introduction à la surveillance de Analysis Services à l'aide de SQL Server Profiler](../../analysis-services/instances/introduction-to-monitoring-analysis-services-with-sql-server-profiler.md)|Explique comment utiliser le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour contrôler une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|[Créer des traces de SQL Server Profiler pour la relecture &#40;Analysis Services&#41;](../../analysis-services/instances/create-profiler-traces-for-replay-analysis-services.md)|Indique les conditions requises pour créer une trace afin d'effectuer une relecture en utilisant le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|[Événements de Trace Analysis Services](../../analysis-services/trace-events/analysis-services-trace-events.md)|Décrit les classes d'événements [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Ces classes d'événements sont associées aux actions générées par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et utilisées pour les relectures de traces.|  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller une Instance Analysis Services](../../analysis-services/instances/monitor-an-analysis-services-instance.md)  
  
  
