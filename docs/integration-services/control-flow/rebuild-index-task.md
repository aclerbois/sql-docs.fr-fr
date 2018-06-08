---
title: Reconstruire l’index, tâche| Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.rebuildindextask.f1
helpviewer_keywords:
- rebuilding indexes
- indexes [Integration Services]
- Rebuild Index task
ms.assetid: 021884dd-e72d-47b2-99e8-b741410509c3
caps.latest.revision: 50
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a20876a0279196f8ec84d9e50218f95251d96495
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rebuild-index-task"></a>tâche Reconstruire l'index
  La tâche Reconstruire l'index reconstruit les index dans les vues et les tables de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur la gestion des index, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 La tâche Reconstruire l'index permet à un package de reconstruire les index dans une ou plusieurs bases de données. Si la tâche reconstruit uniquement les index d'une base de données, vous pouvez choisir les vues et les tables dont la tâche reconstruit les index.  
  
 Cette tâche encapsule une instruction ALTER INDEX REBUILD avec les options de reconstruction d'index suivantes :  
  
-   Spécifiez un pourcentage FILLFACTOR ou utilisez la quantité d'origine de FILLFACTOR.  
  
-   Attribuez au paramètre SORT_IN_TEMPDB la valeur ON afin de stocker dans tempdb le résultat intermédiaire du tri utilisé pour reconstruire l'index. Lorsque le résultat intermédiaire du tri a pour valeur OFF, il est stocké dans la même base de données que l'index.  
  
-   Attribuez au paramètre PAD_INDEX la valeur ON afin d'allouer l'espace disponible spécifié par FILLFACTOR aux pages de niveau intermédiaire de l'index.  
  
-   Attribuez au paramètre IGNORE_DUP_KEY la valeur ON pour permettre une opération d'insertion de plusieurs lignes incluant les enregistrements qui violent des contraintes uniques afin d'insérer les enregistrements qui ne violent pas les contraintes uniques.  
  
-   Attribuez au paramètre ONLINE la valeur ON pour ne pas appliquer de verrous de table, afin que les requêtes ou les mises à jour portant sur la table sous-jacente puissent être exécutées pendant la réindexation.  
  
    > [!NOTE]  
    >  Les opérations d'index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Affectez une valeur à MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plans parallèles.  
  
-   Spécifiez WAIT_AT_LOW_PRIORITY, MAX_DURATION et ABORT_AFTER_WAIT pour contrôler la durée pendant laquelle l’opération d’index attend des verrous de basse priorité.  
  
 Pour plus d’informations sur l’instruction ALTER INDEX et sur les options de reconstruction d’index, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
> [!IMPORTANT]  
>  Le temps que prend la tâche pour créer l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qu'elle exécute est proportionnel au nombre d'index qu'elle reconstruit. Si la tâche est configurée de manière à reconstruire les index dans toutes les tables et vues d'une base de données possédant de nombreux index ou à reconstruire les index de plusieurs bases de données, elle peut prendre un temps considérable pour générer l'instruction Transact-SQL.  
  
## <a name="configuration-of-the-rebuild-index-task"></a>Configuration de la tâche Reconstruire l'index  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Cette tâche se trouve dans la section **Tâches du plan de maintenance** de la **boîte à outils** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
 [Tâche Reconstruire l’index &#40;Plan de maintenance&#41;](../../relational-databases/maintenance-plans/rebuild-index-task-maintenance-plan.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d’informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Définir les propriétés d’une tâche ou d’un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b).  
  
## <a name="see-also"></a> Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
  
