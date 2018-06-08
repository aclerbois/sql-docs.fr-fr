---
title: Optimiser les performances des filtres paramétrés avec des partitions précalculées | Microsoft Docs
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
- precomputed partitions [SQL Server replication]
- merge replication precomputed partitions [SQL Server replication]
- merge replication precomputed partitions [SQL Server replication], about precomputed partitions
ms.assetid: 85654bf4-e25f-4f04-8e34-bbbd738d60fa
caps.latest.revision: 45
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: f094b8701172ece034c53ebad5f07b45fa3c207c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parameterized-filters---optimize-for-precomputed-partitions"></a>Filtres paramétrés - Optimiser pour les partitions précalculées
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les partitions précalculées représentent une optimisation des performances pouvant être utilisée avec les publications de fusion filtrées. Les partitions précalculées sont également une condition requise pour l'utilisation d'enregistrements logiques sur les publications filtrées. Pour plus d’informations sur les enregistrements logiques, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 Lorsqu'un abonné synchronise avec un serveur de publication, ce dernier doit évaluer les filtres de l'Abonné pour déterminer quelles lignes appartiennent à la partition, ou à l'ensemble de données de cet abonné. Le processus permettant de déterminer l'appartenance d'une partition aux modifications sur le serveur de publication pour chaque abonné recevant un dataset filtré est dénommé *évaluation de partition*. Sans partitions précalculées, l'évaluation de partition doit être effectuée pour chaque modification apportée à une colonne filtrée sur le serveur de publication depuis la dernière exécution de l'Agent de fusion pour un abonné spécifique, et ce processus doit être répété pour chaque abonné synchronisant avec le serveur de publication.  
  
 Cependant, si le serveur de publication et l'Abonné s'exécutent sur [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ou version ultérieure et que vous utilisez des partitions précalculées, l'appartenance d'une partition pour toutes les modifications sur le serveur de publication est précalculée et persistante au moment où sont apportées les modifications. Il en résulte que lorsque l'Abonné synchronise avec le serveur de publication, il peut commencer immédiatement à télécharger les modifications concernant sa partition, sans avoir à passer par le processus d'évaluation de partition. Cela peut entraîner d'importantes améliorations en terme de performance, lorsqu'une publication comporte beaucoup de modifications, d'abonnés ou d'articles dans la publication.  
  
 En plus d'utiliser des partitions précalculées, prégénérez des instantanés et/ou permettez aux Abonnés de demander la génération et l'application d'un instantané la première fois qu'ils se synchronisent. Utilisez l'une ou l'autre de ces options ou les deux pour fournir des instantanés pour les publications qui utilisent des filtres paramétrés. Si vous ne spécifiez pas une de ces options, les abonnements sont initialisés à l'aide d'une série d'instructions SELECT et INSERT au lieu d'utiliser l'utilitaire **bcp** , ce processus étant beaucoup plus lent. Pour plus d’informations, consultez [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/snapshots-for-merge-publications-with-parameterized-filters.md).  
  
 **Pour utiliser les partitions précalculées**  
  
 Les partitions précalculées sont activées par défaut sur toutes les publications, nouvelles ou existantes, répondant aux conditions décrites ci-dessous. Le paramétrage peut être modifié via [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou par programme. Pour plus d’informations, consultez [Optimize Parameterized Row Filters](../../../relational-databases/replication/publish/optimize-parameterized-row-filters.md).  
  
## <a name="requirements-for-using-precomputed-partitions"></a>Conditions requises à l'utilisation des partitions précalculées  
 Si les conditions requises sont atteintes, les nouvelles publications sont créées par défaut avec les partitions précalculées activées, et les publications existantes sont mises à niveau automatiquement afin d'utiliser cette fonctionnalité. Si une publication ne répond pas aux conditions requises, elle peut être modifiée pour que les partitions précalculées puissent ensuite être activées. Si quelques articles répondent aux conditions requises et d'autres non, envisagez de créer deux publications dont l'une d'entre elles est activée pour les partitions précalculées.  
  
### <a name="requirements-for-filter-clauses"></a>Conditions requises pour les clauses de filtres  
  
-   Toutes les fonctions utilisées dans le filtre de lignes paramétrable, telles que HOST_NAME() et SUSER_SNAME(), doivent apparaître directement dans la clause du filtre paramétrable et ne pas être imbriquées dans une vue ou une fonction dynamique. Pour plus d’informations sur ces fonctions, consultez [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md), [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md) et [Filtres de lignes paramétrés](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
-   Les valeurs renvoyées pour chaque abonné ne doivent pas être modifiées une fois que la partition a été créée. Par exemple, si vous utilisez HOST_NAME() dans un filtre (sans remplacer la valeur HOST_NAME()), ne modifiez pas le nom de l'ordinateur sur l'Abonné.  
  
-   Les filtres de jointure ne doivent pas contenir de fonctions dynamiques (telles que des fonctions HOST_NAME() et SUSER_SNAME() qui évaluent à une valeur différente selon l'Abonné qui se synchronise). Seuls les filtres de lignes paramétrables doivent contenir des fonctions dynamiques.  
  
-   Les fonctions non déterministes ne peuvent pas s'utiliser dans une clause de filtre. Pour plus d'informations sur les fonctions non déterministes, consultez [Deterministic and Nondeterministic Functions](../../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
-   Les vues référencées dans les clauses de filtres de jointure ou de filtre paramétré ne doivent pas contenir de fonctions dynamiques.  
  
-   Aucune relation de filtre de jointure circulaire ne doit exister dans la publication.  
  
### <a name="database-collation"></a>Classement de base de données  
  
-   Lorsque les partitions précalculées sont utilisées, le classement de base de données est toujours utilisé lorsque des comparaisons sont effectuées, plutôt que le classement de la table ou de la colonne. Examinez le cas suivant :  
  
    -   Une base de données avec un classement respectant les accents contient une table avec un classement ne respectant pas les accents.  
  
    -   La table contient une colonne **Nom de l'ordinateur**qui est comparée avec le nom d'hôte de l'Abonné dans un filtre paramétré.  
  
    -   La table contient une ligne avec la valeur « POSTE DE TRAVAIL » et une ligne avec la valeur « poste de travail » dans cette colonne.  
  
     Si l'Abonné synchronise avec un nom d'hôte « poste de travail », il ne reçoit qu'une ligne car la comparaison respecte les accents (le classement de la base de données). Si les partitions précalculées ne sont pas utilisées, l'Abonné reçoit les deux lignes, car la table dispose d'un classement qui ne respecte pas les accents.  
  
## <a name="performance-of-precomputed-partitions"></a>Performances et partitions précalculées  
 Les partitions précalculées engendrent une légère perte de performance lorsque les modifications sont chargées de l'Abonné au serveur de publication, mais comme la majorité du temps de traitement de fusion est passé à évaluer les partitions et à charger les modifications du serveur de publication sur l'Abonné, le gain en terme de performance peut quand même s'avérer important. L'amélioration des performances peut varier en fonction du nombre d'abonnés synchronisant simultanément, et le nombre de mises à jour par synchronisation déplaçant les lignes d'une partition à l'autre.  
  
## <a name="see-also"></a> Voir aussi  
 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
