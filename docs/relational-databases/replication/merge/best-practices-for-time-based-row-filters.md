---
title: Bonnes pratiques en matière de filtres de lignes basés sur le temps | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- best practices
ms.assetid: 773c5c62-fd44-44ab-9c6b-4257dbf8ffdb
caps.latest.revision: 15
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: bd7bfdf2bb9e1971dd6557f562f18fbbff5c3a20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="best-practices-for-time-based-row-filters"></a>Bonnes pratiques en matière de filtres de lignes basés sur le temps
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les utilisateurs d'applications ont souvent besoin d'un sous-ensemble de données d'une table basé sur le temps. Par exemple, un vendeur peut avoir besoin des données sur les commandes passées au cours de la dernière semaine tandis qu'un planificateur d'événements peut avoir besoin des données sur les événements qui auront lieu au cours de la semaine à venir. Dans de nombreux cas, pour accomplir cette tâche, les applications utilisent des requêtes qui contiennent la fonction **GETDATE()** . Considérons l'instruction de filtre de lignes suivante :  
  
```  
WHERE SalesPersonID = CONVERT(INT,HOST_NAME()) AND OrderDate >= (GETDATE()-6)  
```  
  
 Avec un filtre de ce type, il est généralement admis que deux événements se produisent systématiquement à l'exécution de l'Agent de fusion : les lignes qui satisfont aux critères de ce filtre sont répliquées vers les Abonnés, tandis que celles qui n'y satisfont plus sont nettoyées sur ceux-ci. (Pour plus d’informations sur les options de filtrage avec **HOST_NAME()**, consultez [Filtres de lignes paramétrés](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).) Toutefois, la réplication de fusion ne fait que répliquer et nettoyer les données qui ont changé depuis la dernière synchronisation, quelle que soit la façon dont vous définissez un filtre de lignes pour ces données.  
  
 Pour que la réplication de fusion traite une ligne, les données contenues dans celle-ci doivent satisfaire aux critères du filtre de lignes et avoir changé depuis la dernière synchronisation. Dans le cas de la table **SalesOrderHeader** , **OrderDate** est entré lorsqu'une ligne est insérée. Les lignes sont répliquées vers l'Abonné comme prévu car l'insertion constitue une modification de données. Toutefois, s'il existe des lignes sur l'Abonné qui ne satisfont plus aux critères de filtre (relatives en l'occurrence aux commandes qui datent de plus de sept jours), elles ne sont supprimées de l'Abonné que si elles ont été mises à jour pour une raison quelconque.  
  
 Le cas du planificateur d'événements met davantage en lumière le problème lié à ce type de filtrage. Considérons le filtre suivant pour une table **Events** :  
  
```  
WHERE EventCoordID = CONVERT(INT,HOST_NAME()) AND EventDate <= (GETDATE()+6)  
```  
  
 Dans le cas d'une table qui contient des événements, les insertions peuvent être réalisées largement à l'avance par rapport à la date de l'événement. Si l'insertion d'un événement qui doit avoir lieu au cours de la semaine à venir a été réalisée avec un mois d'avance et que la ligne n'a pas été mise à jour pour une raison quelconque, la ligne n'est pas répliquée vers l'Abonné même si elle satisfait aux critères du filtre de lignes.  
  
 En outre, suivant la configuration de la publication, la réplication de fusion évalue les filtres à différents moments :  
  
-   Si une publication utilise des partitions précalculées (configuration par défaut), les filtres sont évalués lorsqu'une ligne est insérée ou mise à jour.  
  
-   Si la publication n'utilise pas de partitions précalculées, les filtres sont évalués à l'exécution de l'Agent de fusion.  
  
 Pour plus d’informations sur les partitions précalculées, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md). Le moment auquel le filtre est évalué détermine les données qui satisfont aux critères de ce filtre. Par exemple, si une publication utilise des partitions précalculées et que vous synchronisez les données tous les deux jours, le sous-ensemble de données pour le vendeur peut comprendre des lignes dont l'ancienneté dépasse les deux jours prévus.  
  
## <a name="recommendations-for-using-time-based-row-filters"></a>Recommandations pour l'utilisation de filtres de lignes basés sur le temps  
 La méthode suivante propose une approche fiable et directe du filtrage en fonction du temps :  
  
-   Ajoutez une colonne à la table de type de données **bit**. Cette colonne permet d'indiquer si une ligne doit être répliquée.  
  
-   Utilisez un filtre de lignes qui référence la nouvelle colonne plutôt qu'une colonne basée sur le temps.  
  
-   Créez un travail de l'Agent SQL Server (ou un travail planifié par le biais d'un autre mécanisme) qui met à jour la colonne avant que ne soit planifiée l'exécution de l'Agent de fusion.  
  
 Cette approche pallie les points faibles de l'utilisation de **GETDATE()** ou d'une autre méthode basée sur le temps et évite d'avoir à déterminer à quel moment les filtres sont évalués pour les partitions. Considérons l'exemple suivant d'une table **Events** :  
  
|**EventID**|**EventName**|**EventCoordID**|**EventDate**|**Répliquer**|  
|-----------------|-------------------|----------------------|-------------------|-------------------|  
| 1|Réception|112|2006-10-04| 1|  
|2|Dîner|112|2006-10-10|0|  
|3|Soirée|112|2006-10-11|0|  
|4|Mariage|112|2006-10-12|0|  
  
 Le filtre de lignes de cette table ressemblerait à ceci :  
  
```  
WHERE EventCoordID = CONVERT(INT,HOST_NAME()) AND Replicate = 1  
```  
  
 Le travail de l'Agent SQL Server pourrait exécuter des instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] similaires à la suivante avant chaque exécution de l'Agent de fusion :  
  
```  
UPDATE Events SET Replicate = 0 WHERE Replicate = 1  
GO  
UPDATE Events SET Replicate = 1 WHERE EventDate <= GETDATE()+6  
GO  
```  
  
 La première ligne réinitialise la colonne **Replicate** à **0**, tandis que la seconde lui attribue la valeur **1** pour les événements qui se produiront au cours des sept prochains jours. Si cette instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] s'exécute le 07/10/2006, la table, une fois mise à jour, présentera l'aspect suivant :  
  
|**EventID**|**EventName**|**EventCoordID**|**EventDate**|**Répliquer**|  
|-----------------|-------------------|----------------------|-------------------|-------------------|  
| 1|Réception|112|2006-10-04|0|  
|2|Dîner|112|2006-10-10| 1|  
|3|Soirée|112|2006-10-11| 1|  
|4|Mariage|112|2006-10-12| 1|  
  
 Les événements relatifs à la semaine à venir sont désormais signalés comme étant prêts à être répliqués. La prochaine fois que l'Agent de fusion s'exécutera pour l'abonnement utilisé par le coordinateur d'événements 112, les lignes 2, 3 et 4 seront téléchargées vers l'Abonné tandis que la ligne 1 sera supprimée de celui-ci.  
  
## <a name="see-also"></a> Voir aussi  
 [GETDATE &#40;Transact-SQL&#41;](../../../t-sql/functions/getdate-transact-sql.md)   
 [Implémenter des travaux](http://msdn.microsoft.com/library/69e06724-25c7-4fb3-8a5b-3d4596f21756)   
 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
