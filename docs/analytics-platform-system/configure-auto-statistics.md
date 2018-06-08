---
title: Automatique des statistiques (système de plateforme Analytique)
description: Décrit la fonctionnalité de statistiques d’automatique introduite dans AU7 de système de plateforme Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 1c0f4623adad35ab874330b42aa54f6e1b91d961
ms.sourcegitcommit: fc3cd23685c6b9b6972d6a7bab2cc2fc5ebab5f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
# <a name="configure-auto-statistics"></a>Configurer automatiquement les statistiques

Découvrez comment configurer Parallel Data Warehouse pour utiliser automatique des statistiques pour la création et la mise à jour automatique des statistiques.  Cette fonctionnalité permet d’améliorer les plans de requête et par conséquent, améliorer les performances des requêtes.

**S’applique à :** APS (à partir de AU7)

## <a name="what-are-statistics"></a>Quelles sont les statistiques ?
Statistiques pour l’optimisation des requêtes sont des objets qui contiennent des informations statistiques sur la distribution des valeurs dans une ou plusieurs colonnes d’une table. L’optimiseur de requête utilise ces statistiques pour estimer la cardinalité, ou entraîne de nombre de lignes, dans la requête. Ces estimations de cardinalité permettent à l’optimiseur de requête créer un plan de requête de haute qualité. Par exemple, dans les points d’accès, les utilisations d’optimiseur de requête MPP estimations de cardinalité pour choisir de lecture aléatoire ou de répliquer le plus petit de deux tables sont utilisées dans une clause join et ainsi améliorent les performances.  Pour plus d’informations, consultez [statistiques](../relational-databases/statistics/statistics.md) et [DBCC SHOW_STATISTICS](../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)

## <a name="what-are-auto-statistics"></a>Quelles sont les statistiques automatique ?
Automatique des statistiques des statistiques que l’optimiseur de requête crée et met à jour automatiquement pour améliorer le plan de requête. Statistiques deviennent obsolètes après charges, insère, met à jour et supprime les opérations. Sans automatique des statistiques, vous devez effectuer votre propre analyse afin de comprendre les colonnes qui ont besoin de statistiques, et lorsque les statistiques doivent être mis à jour.

Automatique des statistiques inclut les trois paramètres suivants : 

### <a name="autocreatestatistics"></a>AUTO_CREATE_STATISTICS
Lorsque l’option de statistiques de création automatique AUTO_CREATE_STATISTICS est activée, l’optimiseur de requête crée des statistiques sur des colonnes individuelles du prédicat de requête, si nécessaire améliorer les estimations de cardinalité pour le plan de requête. Ces statistiques de colonne unique sont créées sur les colonnes où ne figure pas déjà un histogramme au niveau d'un objet de statistiques existant.

### <a name="autoupdatestatistics"></a>AUTO_UPDATE_STATISTICS 
Lorsque l'option de mise à jour automatique des statistiques AUTO_UPDATE_STATISTICS est activée, l'optimiseur de requête détermine si les statistiques sont obsolètes et les met éventuellement à jour lorsqu'elles sont utilisées par une requête. Les statistiques deviennent obsolètes après que les opérations Insérer, mettre à jour, suppriment ou de fusion modifient la distribution des données dans la table ou vue indexée. L'optimiseur de requête détermine si les statistiques sont obsolètes en comptant le nombre de modifications de données depuis la dernière mise à jour des statistiques et en comparant le nombre de modifications à un seuil. Ce seuil est basé sur le nombre de lignes contenues dans la table ou la vue indexée.

### <a name="autoupdatestatisticsasync"></a>AUTO_UPDATE_STATISTICS_ASYNC
L'option de mise à jour asynchrone des statistiques AUTO_UPDATE_STATISTICS_ASYNC détermine si l'optimiseur de requête utilise des mises à jour de statistiques synchrones ou asynchrones. Pour les points d’accès, l’option de mise à jour asynchrone des statistiques est activé par défaut, et l’optimiseur de requête met à jour les statistiques de façon asynchrone. L’option AUTO_UPDATE_STATISTICS_ASYNC s’applique aux objets de statistiques créés pour les index, les colonnes uniques contenues dans les prédicats de requête et les statistiques créées avec l’instruction CREATE STATISTICS.

## <a name="configuration-settings-for-system-administrators"></a>Paramètres de configuration pour les administrateurs système
Après la mise à niveau pour les points d’accès AU7, automatique des statistiques sont activée par défaut. L’administrateur système peut activer ou désactiver les automatique des statistiques avec le [fonctionnalité commutateur](appliance-feature-switch.md) option dans le Gestionnaire de Configuration de matériel.  Une fois activée, les utilisateurs peuvent modifier les paramètres des statistiques par base de données.
Changer des valeurs de commutateur de fonctionnalité nécessite un redémarrage du service sur les points d’accès.

## <a name="change-auto-statistics-settings-on-a-database"></a>Modifier les paramètres de statistiques automatique sur une base de données
Lorsque automatique des statistiques sont activée par l’administrateur système, vous pouvez utiliser [ALTER DATABASE (Parallel Data Warehouse)](/sql/t-sql/statements/alter-database-parallel-data-warehouse) pour modifier les paramètres des statistiques sur une base de données. Si le commutateur de la fonctionnalité automatique des statistiques est activée par l’administrateur système, toutes les bases de données créées après la mise à niveau AU7 aura automatique des statistiques activés. Toutes les bases de données qui existaient avant la mise à niveau AU7 ont automatique des statistiques désactivés. L’exemple suivant active automatique des statistiques sur le myPDW de base de données existante.

```sql
ALTER DATABASE myPDW SET AUTO_CREATE_STATISTICS ON
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS ON 
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS_ASYNC ON
```
 
Option de AUTO_UPDATE STATISTICS_ASYNC fonctionne uniquement si l’option AUTO_UPDATE_STATISTICS est activée.  Par conséquent, les statistiques ne sont pas actualisées lorsque l’option AUTO_UPDATE_STATISTICS est désactivée et AUTO_UPDATE_STATISTICS_ASYNC est activé. 

### <a name="error-messages"></a>Messages d’erreur
Vous pouvez recevoir le message d’erreur « cette option n’est pas prise en charge dans PDW ».  Cette erreur se produit lorsque l’administrateur système n’a pas activé automatiquement les statistiques, et que vous tentez de définir les automatique des options de statistiques dans ALTER DATABASE. 

### <a name="limitations-and-restrictions"></a>Limitations et restrictions
Automatique des statistiques ne fonctionnent pas sur des tables externes. 

### <a name="check-the-current-values"></a>Vérifiez les valeurs en cours
La requête suivante retourne les valeurs actuelles des paramètres automatique des statistiques pour toutes les bases de données.

```sql
SELECT NAME
    , IS_AUTO_CREATE_STATS_ON 
    , IS_AUTO_UPDATE_STATS_ON
    , IS_AUTO_UPDATE_STATS_ASYNC_ON
FROM
    sys.databases;
```

Une valeur de retour de 1 signifie que le paramètre est de 0 signifie que le paramètre est désactivé 

## <a name="next-steps"></a>Étapes suivantes
Pour voir les performances de vos requêtes, consultez [analyse les requêtes actives](monitoring-active-queries.md)
