---
title: Le paramétrage automatique | Documents Microsoft
description: En savoir plus sur le réglage automatique dans SQL Server et la base de données SQL Azure
ms.custom: ''
ms.date: 08/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: automatic-tuning
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- performance tuning [SQL Server]
ms.assetid: ''
caps.latest.revision: ''
author: jovanpop-msft
ms.author: jovanpop
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2017 || = sqlallproducts-allversions
ms.openlocfilehash: 0e77a1d7e24fa2635b3e699672338e588c1f5c1c
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707767"
---
# <a name="automatic-tuning"></a>Paramétrage automatique
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

  L’optimisation automatique est une fonctionnalité de base de données qui fournit des insights sur les éventuels problèmes de performances des requêtes, recommande des solutions et corrige automatiquement les problèmes identifiés.

Le paramétrage automatique [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] vous avertit chaque fois qu’un problème de performance potentiel est détecté et vous permet d’appliquer des actions correctives, ou vous permet du [!INCLUDE[ssde_md](../../includes/ssde_md.md)] résoudre automatiquement les problèmes de performances.
Le paramétrage automatique [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] vous permet d’identifier et résoudre les problèmes de performances causés par **régressions de choix de plan SQL**. Le paramétrage automatique [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] crée des index nécessaires et supprime les index non utilisés.

[!INCLUDE[ssde_md](../../includes/ssde_md.md)] analyse les requêtes qui sont exécutées sur la base de données et automatiquement améliore les performances de la charge de travail. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] présente un mécanisme d’intelligence intégrée qui peut automatiquement optimiser et améliorer les performances de vos requêtes en adaptation dynamique de la base de données à votre charge de travail. Il existe deux fonctionnalités de paramétrage automatique qui sont disponibles :

 -  **Correction du plan automatique** (disponible dans [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] et [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]) qui identifie les plans d’exécution des requêtes problématiques et correctifs SQL planifier des problèmes de performances.
 -  **Gestion automatique des index** (disponible uniquement dans [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]) qui identifie les index qui doivent être ajoutés dans votre base de données et des index qui doivent être supprimés.

## <a name="why-automatic-tuning"></a>Pourquoi le paramétrage automatique ?

Une des principales tâches que dans l’administration de la base de données classique analyse la charge de travail, en identifiant critique [!INCLUDE[tsql_md](../../includes/tsql_md.md)] des requêtes, les index qui doivent être ajoutés pour améliorer les performances et rarement utilisée index. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] fournit un aperçu détaillé dans les requêtes et les index que vous souhaitez analyser. Toutefois, le contrôle en permanence et la base de données est une tâche difficile et fastidieuse, en particulier lors du traitement de plusieurs bases de données. Gérer un très grand nombre de bases de données peut être difficile d’effectuer efficacement. Au lieu de surveillance et de paramétrage de votre base de données manuellement, vous pouvez envisager de délégation de partie de la surveillance et de paramétrage des actions à [!INCLUDE[ssde_md](../../includes/ssde_md.md)] à l’aide de la fonctionnalité de réglage automatique.

### <a name="how-does-automatic-tuning-works"></a>En quoi le fonctionnement de réglage automatique ?

Le paramétrage automatique est un processus d’analyse en permanence apprend plus sur les caractéristiques de votre charge de travail et de surveillance continue et identifier des améliorations et des problèmes potentiels.

![Processus de réglage automatique](./media/tuning-process.png)

Ce processus permet de s’adapter dynamiquement à votre charge de travail en recherchant les plans et les index peuvent améliorer les performances de vos charges de travail et les index affectent vos charges de travail de base de données. En fonction de ces résultats, le paramétrage automatique applique des actions de paramétrage qui améliorent les performances de votre charge de travail. En outre, base de données surveille en permanence les performances après toute modification apportée par le paramétrage automatique pour vous assurer qu’elle améliore les performances de votre charge de travail. Toute action qui n’a pas améliorer les performances est automatiquement restaurée. Ce processus de vérification est une fonctionnalité clé qui permet de s’assurer que toute modification apportée par le paramétrage automatique ne diminue pas les performances de votre charge de travail.

## <a name="automatic-plan-correction"></a>Correction de la planification automatique

Correction du plan automatique est une fonctionnalité de réglage automatique qui identifie **régression de choix de plans SQL** et résoudre automatiquement le problème en forçant le dernier plan correct connu.

### <a name="what-is-sql-plan-choice-regression"></a>Quelle est la régression de choix de plan SQL ?

[!INCLUDE[ssdenoversion_md](../../includes/ssdenoversion_md.md)] peut utiliser différents plans SQL pour exécuter le [!INCLUDE[tsql_md](../../includes/tsql_md.md)] requêtes. Plans de requête dépendent les statistiques, les index et les autres facteurs. Le plan optimal qui doit être utilisé pour exécuter certaines [!INCLUDE[tsql_md](../../includes/tsql_md.md)] requête peut-être être modifiée au fil du temps. Dans certains cas, le nouveau plan peut ne pas être une meilleure que la précédente, et le nouveau plan peut provoquer une régression des performances.

 ![Régression de choix de plan SQL](media/plan-choice-regression.png "régression de choix de plan SQL") 

Chaque fois que vous remarquez la régression de choix de plan, vous devez rechercher précédente bien planifier et force au lieu d’utiliser un cours `sp_query_store_force_plan` procédure.
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] dans [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] fournit des informations sur le point ont régressé plans et les actions correctives recommandées.
En outre, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] vous permet d’entièrement automatiser ce processus et laissez [!INCLUDE[ssde_md](../../includes/ssde_md.md)] résoudre tout problème rencontré liés aux changements de plan.

### <a name="automatic-plan-choice-correction"></a>Correction de choix de plan automatique

[!INCLUDE[ssde_md](../../includes/ssde_md.md)] peut automatiquement basculer vers le dernier plan correct connu chaque fois que la régression de choix de plan est détectée.

![Correction de choix de plan SQL](media/force-last-good-plan.png "correction de choix de plan SQL") 

[!INCLUDE[ssde_md](../../includes/ssde_md.md)] détecte automatiquement une régression de choix de plan potentiels, y compris le plan qui doit être utilisé au lieu du plan incorrect.
Lorsque le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] applique la dernière connu bon plan, il analyse automatiquement les performances du plan forcé. Si le plan forcé n’est pas meilleur que le plan de régression, le nouveau plan sera unforced et le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] compile un plan. Si [!INCLUDE[ssde_md](../../includes/ssde_md.md)] vérifie que le plan forcé est préférable à une régression, le plan forcé est conservé jusqu'à une recompilation (par exemple, sur la prochaine modification de schéma ou de statistiques) s’il est préférable que le plan de régression.

Remarque : N’importe quel automatique des plans forcé ne pas persit sur un redémarrage de l’instance de SQL Server.

### <a name="enabling-automatic-plan-choice-correction"></a>L’activation de correction des choix de plan automatique

Vous pouvez activer le réglage automatique pour chaque base de données et spécifier que le dernier bon plan connu doit être forcé quand une régression de changement de plan est détectée. Pour cela, utilisez la commande suivante :

```sql   
ALTER DATABASE current
SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON ); 
```
Une fois que vous activer cette option, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] automatiquement forcer toute recommandation où le gain d’UC estimé est supérieur à 10 secondes, ou le nombre d’erreurs dans le nouveau plan est supérieur au nombre d’erreurs dans le plan recommandé et vérifiez que le plan forcé est meilleur que l’objet actuel.

### <a name="alternative---manual-plan-choice-correction"></a>Alternative - correction de choix de planification manuelle

Quand le réglage automatique n’est pas activé, les utilisateurs doivent régulièrement surveiller le système et rechercher les requêtes régressées. Si n’importe quel plan ont régressé, utilisateur doit rechercher précédente bien planifier et force au lieu d’utiliser un cours `sp_query_store_force_plan` procédure. La meilleure pratique serait pour forcer le dernier plan correct connu, car les anciens plans est peut-être non valides en raison de modifications des statistiques ou un index. L’utilisateur qui force le dernier plan correct connu doit surveiller les performances de la requête est exécutée à l’aide du plan forcé et vérifiez que le plan forcé fonctionne comme prévu. Selon les résultats de la surveillance et d’analyse, plan doit être forcé ou utilisateur doit trouver une autre façon d’optimiser la requête.
Plans forcés manuellement ne doivent pas être forcés à forever, parce que le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] doit être en mesure d’appliquer des plans non optimaux. L’utilisateur ou l’administrateur doit finalement annuler son application forcée du plan à l’aide de `sp_query_store_unforce_plan` procédure et laisser le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] trouver le plan optimal.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit toutes les vues nécessaires et les procédures requises pour surveiller les performances et résoudre les problèmes dans le magasin de requêtes.

Dans [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)], vous pouvez rechercher les régressions de choix de plan à l’aide de vues système de magasin de requêtes. Dans [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] détecte et présente des régressions de choix de plan potentiels et les actions recommandées qui doivent être appliquées dans le [sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) vue. La vue affiche des informations sur le problème, l’importance du problème et des détails tels que la requête identifiée, l’ID du plan de régression, l’ID du plan qui a été utilisé en tant que ligne de base pour la comparaison et le [!INCLUDE[tsql_md](../../includes/tsql_md.md)] instruction qui peut être exécutée pour résoudre le problème.

| Type | description | DATETIME | score | détails | … |
| --- | --- | --- | --- | --- | --- |
| `FORCE_LAST_GOOD_PLAN` | Temps processeur passé de 4 ms à 14 ms | 3/17/2017 | 83 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL`) |   |
| `FORCE_LAST_GOOD_PLAN` | Temps processeur passé de ms 37 à 84 ms | 3/16/2017 | 26 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL`) |   |

Certaines colonnes de cette vue sont décrits dans la liste suivante :
 - Type de l’action recommandée - `FORCE_LAST_GOOD_PLAN`.
 - Description qui contient des informations pourquoi [!INCLUDE[ssde_md](../../includes/ssde_md.md)] pense que ce changement de plan est une régression des performances potentielles.
 - Date et heure de la régression potentielle est détectée.
 - Score de cette recommandation. 
 - Pour plus d’informations sur les problèmes tels que les ID du plan détecté, l’ID du plan de régression, ID du plan qui doit être forcé pour résoudre le problème, [!INCLUDE[tsql_md](../../includes/tsql_md.md)]
 script qui peut-être être appliquée pour résoudre le problème, etc. Sont stockés les détails [format JSON](../../relational-databases/json/index.md).

Utilisez la requête suivante pour obtenir un script qui résout le problème et des informations supplémentaires sur l’estimation bénéficiez :

```sql   
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount+recommendedPlanExecutionCount)
                  *(regressedPlanCpuTimeAverage-recommendedPlanCpuTimeAverage)/1000000,
      error_prone = IIF(regressedPlanErrorCount>recommendedPlanErrorCount, 'YES','NO')
FROM sys.dm_db_tuning_recommendations
  CROSS APPLY OPENJSON (Details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            [current plan_id] int '$.regressedPlanId',
            [recommended plan_id] int '$.recommendedPlanId',

            regressedPlanErrorCount int,
            recommendedPlanErrorCount int,

            regressedPlanExecutionCount int,
            regressedPlanCpuTimeAverage float,
            recommendedPlanExecutionCount int,
            recommendedPlanCpuTimeAverage float

          ) as planForceDetails;
```

[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]     

| reason | score | script | requête\_id | plan actuel\_id | recommandé plan\_id | estimé\_obtenir | erreur\_sujets
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Temps processeur passé de 3 ms à 46 ms | 36 | EXEC sp\_requête\_stocker\_forcer\_plan 12, 17 ; | 12 | 28 | 17 | 11.59 | 0

`estimated_gain` représente le nombre estimé de secondes qui seraient enregistrés si le plan recommandé est exécuté à la place le plan actuel. Si le gain est supérieur à 10 secondes, le plan recommandé doit être forcé à la place le plan actuel. S’il existe des erreurs plus (par exemple, les délais d’attente ou abandonnées exécutions) dans le plan actuel que dans la planification, la colonne `error_prone` serait défini sur la valeur `YES`. Plan susceptible d’engendrer des erreurs est une autre raison pour lesquelles le plan recommandé doit être forcé au lieu de l’objet actuel.

Bien que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] fournit toutes les informations requises pour identifier les régressions de choix de plan ; continue d’analyse et de résolution des problèmes de performances peuvent être un processus fastidieux. Le paramétrage automatique facilite ce processus.

Remarque : Les données dans cette vue DMV ne persistent pas après un redémarrage de l’instance de SQL Server.

## <a name="automatic-index-management"></a>Gestion automatique des index

Dans [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], la gestion d’index est facile car [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] a eu connaissance de votre charge de travail et garantit que vos données sont toujours de façon optimale indexées. Conception d’index approprié est essentielle pour optimiser les performances de votre charge de travail et la gestion d’index automatique peut vous aider à optimiser votre index. Gestion automatique des index peuvent résoudre les problèmes de performances dans les bases de données indexées de manière incorrecte, ou mettre à jour et améliorez les index sur le schéma de base de données existante. Le paramétrage automatique [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] effectue les actions suivantes :

 - Identifie les index qui peut améliorer les performances de vos requêtes T-SQL qui lisent des données à partir des tables.
 - Identifie l’index redondants ou les index qui n’ont pas été utilisés dans une longue période de temps qui a pu être supprimé. Suppression d’index inutiles améliore les performances des requêtes qui mettent à jour des données dans des tables.

### <a name="why-do-you-need-index-management"></a>Pourquoi devez-vous la gestion d’index ?

Index accélérer de certaines de vos requêtes qui lisent des données à partir des tables ; Cependant, ils peuvent ralentir les requêtes qui mettent à jour des données. Vous devez analyser soigneusement les quand créer un index et les colonnes que vous devez inclure dans l’index. Certains index ne peut pas être nécessaire après un certain temps. Par conséquent, vous devez régulièrement identifier et supprimer les index qui ne mettez pas d’avantages. Si vous ignorez les index non utilisés, les performances des requêtes qui mettent à jour des données sont diminuée sans aucun avantage sur les requêtes qui lisent des données. Index inutilisés affectent également les performances globales du système, car les mises à jour supplémentaires besoin d’une journalisation inutile.

Recherche l’ensemble optimal d’index qui améliorent les performances des requêtes qui ont un impact minimal sur les mises à jour et de lire les données de vos tables peut nécessiter l’analyse continue et complexe.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] utilise une intelligence intégrée et des règles avancées qui analysent vos requêtes, identifier les index qui seraient optimales pour vos charges de travail en cours, et les index peuvent être supprimées. Base de données SQL Azure permet de s’assurer que vous disposez d’un ensemble minimal nécessaire d’index qui optimisent les requêtes qui lisent des données, avec un impact réduit sur les autres requêtes.

### <a name="automatic-index-management"></a>Gestion automatique des index

En plus de la détection, [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] peuvent appliquer automatiquement les recommandations identifiées. Si vous trouvez que les règles intégrées améliorent les performances de votre base de données, vous pouvez laisser [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] gérer automatiquement les index.

Pour activer le réglage automatique dans la base de données SQL Azure et permettent de gérer entièrement votre charge de travail de réglage automatique, consultez [activer le réglage automatique dans la base de données SQL Azure à l’aide du portail Azure](https://docs.microsoft.com/azure/sql-database/sql-database-automatic-tuning-enable).

Lorsque le [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] applique une recommandation de CREATE INDEX ou DROP INDEX, il analyse automatiquement les performances des requêtes qui sont affectés par l’index. Nouvel index est conservé uniquement si les performances des requêtes concernées sont améliorées. L’index supprimé sera automatiquement recréé s’il existe des requêtes qui s’exécutent plus lentement en raison de l’absence de l’index.

### <a name="automatic-index-management-considerations"></a>Considérations sur la gestion automatique d’index

Actions requises pour créer des index nécessaires dans [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] peut consommer des ressources et affecter temporairement les performances de la charge de travail. Pour minimiser l’impact de la création d’index sur les performances de la charge de travail, base de données SQL Azure recherche la fenêtre de temps approprié pour toutes les opérations de gestion d’index. Action de paramétrage est reporté à plus tard si la base de données a besoin de ressources pour l’exécution de votre charge de travail et démarré lors de la base de données est suffisant des ressources inutilisées qui peuvent être utilisés pour la tâche de maintenance. Une fonctionnalité importante dans la gestion automatique des index est une vérification des actions. Lors de la base de données SQL Azure crée ou supprime des index, un processus d’analyse analyse des performances de votre charge de travail pour vérifier que l’action amélioré les performances. Si elle n’a pas d’apporter une amélioration significative : l’action est immédiatement annulée. De cette manière, base de données SQL Azure permet de s’assurer que les actions automatiques ne pas nuire aux performances de votre charge de travail. Index créés par le paramétrage automatique sont transparentes pour l’opération de maintenance sur le schéma sous-jacent. Modifications de schéma telles que supprimer ou renommer des colonnes ne sont pas bloquées par la présence d’index créés automatiquement. Les index sont créés automatiquement par la base de données SQL Azure sont supprimés immédiatement lorsque liés de table ou colonnes est supprimée.

### <a name="alternative---manual-index-management"></a>Alternative - gestion d’index manuelles

Sans la gestion automatique des index, l’utilisateur doit interroger manuellement [sys.dm_db_missing_index_details &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) afin de retrouver les index qui peut améliorer les performances, créer des index à l’aide des détails fourni dans cette vue et surveiller les performances de la requête manuellement. Afin de trouver l’index doivent être supprimés, les utilisateurs doivent surveiller opérationnel statistiques des index à l’index de recherche rarement utilisée.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] simplifie ce processus. [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] analyse de votre charge de travail, identifie les requêtes qui peuvent être exécutées plus rapidement avec un nouvel index et identifie les index non utilisés ou en double. Trouver les informations d’identification d’index doit être modifié sur [trouver des recommandations d’index dans le portail Azure](https://docs.microsoft.com/azure/sql-database/sql-database-advisor-portal).

## <a name="see-also"></a>Voir aussi  
 [MODIFICATION de base de données ensemble AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_automatic_tuning_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)  
 [Sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sp_query_store_force_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)     
 [sp_query_store_unforce_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)           
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [Fonctions JSON](../../relational-databases/json/index.md)
