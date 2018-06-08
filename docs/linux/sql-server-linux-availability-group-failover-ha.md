---
title: Gérer le basculement du groupe de disponibilité - SQL Server sur Linux | Documents Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 03/01/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 26868cfd136f3d06366a47ec7d52fa17e3c8fe39
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="always-on-availability-group-failover-on-linux"></a>Basculement du groupe de disponibilité AlwaysOn sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Dans le contexte d’un groupe de disponibilité (AG), le rôle principal et le rôle secondaire des réplicas de disponibilité sont généralement interchangeables lors d’un processus appelé basculement. Trois formes de basculement existent : basculement automatique (sans perte de données), basculement manuel planifié (sans perte de données) et basculement manuel forcé (avec perte de données possible), ce dernier étant généralement appelé *basculement forcé*. Les basculements manuels automatiques et planifiés conservent toutes vos données. Un groupe de disponibilité bascule au niveau du réplica de disponibilité. Autrement dit, un groupe de disponibilité bascule vers un de ses réplicas secondaires (la cible de basculement en cours). 

Pour des informations générales sur le basculement, consultez [les modes de basculement et](../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).

## <a name="failover"></a>basculement manuel

Utilisez les outils de gestion de cluster pour basculer un groupe de disponibilité gérée par un gestionnaire de cluster externe. Par exemple, si une solution utilise STIMULATEUR pour gérer un cluster Linux, utilisez `pcs` pour effectuer des basculements manuels sur RHEL ou Ubuntu. SLES utiliser `crm`. 

> [!IMPORTANT]
> Dans des conditions normales, ne basculent pas avec des outils de gestion de Transact-SQL ou SQL Server comme SSMS ou PowerShell. Lorsque `CLUSTER_TYPE = EXTERNAL`, la seule valeur acceptable pour `FAILOVER_MODE` est `EXTERNAL`. Avec ces paramètres, toutes les actions de basculement manuel ou automatique sont exécutées par le Gestionnaire de cluster externe. Pour obtenir des instructions forcer le basculement avec perte de données potentielle, consultez [forcer le basculement](#forceFailover).

### <a name="a-namemanualfailovermanual-failover-steps"></a><a name="manualFailover">Étapes de basculement manuel

Pour effectuer un basculement, le réplica secondaire qui deviendra le réplica principal doit être synchrone. Si un réplica secondaire est asynchrone, [modifier le mode de disponibilité](../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md).

Basculer manuellement en deux étapes.

   Tout d’abord,[ basculer manuellement en déplaçant des ressources du groupe de disponibilité](#manualMove) à partir du nœud de cluster qui possède les ressources vers un nouveau nœud.

   Le cluster de bascule de la ressource de groupe de disponibilité et ajoute une contrainte d’emplacement. Cette contrainte configure la ressource à exécuter sur le nouveau nœud. Supprimer cette contrainte afin de basculer correctement dans le futur.

   Ensuite, [supprimer la contrainte d’emplacement](#removeLocConstraint).

#### <a name="a-namemanualmovestep-1-manually-fail-over-by-moving-availability-group-resource"></a><a name="manualMove">Étape 1. Basculer manuellement en déplaçant des ressources de groupe de disponibilité

Pour basculer manuellement une ressource de groupe de disponibilité nommée *ag_cluster* au nœud de cluster nommé *Nom_nœud2*, exécutez la commande appropriée pour votre distribution :

- **Exemple RHEL/Ubuntu**

   ```bash
   sudo pcs resource move ag_cluster-master nodeName2 --master
   ```

- **Exemple SLES**

   ```bash
   crm resource migrate ag_cluster nodeName2
   ```

>[!IMPORTANT]
>Après avoir basculé manuellement une ressource, vous devez supprimer une contrainte d’emplacement qui est automatiquement ajoutée.

#### <a name="a-nameremovelocconstraint-step-2-remove-the-location-constraint"></a><a name="removeLocConstraint"> Étape 2. Supprimer la contrainte d’emplacement

Lors d’un basculement manuel, le `pcs` commande `move` ou `crm` commande `migrate` ajoute une contrainte d’emplacement de la ressource doit être placé sur le nouveau nœud cible. Pour afficher la nouvelle contrainte, exécutez la commande suivante après avoir déplacé manuellement la ressource :

- **Exemple RHEL/Ubuntu**

   ```bash
   sudo pcs constraint --full
   ```

- **Exemple SLES**

   ```bash
   crm config show
   ```

Supprimez la contrainte d’emplacement des basculements futures, y compris le basculement automatique - fonctionner. 

Pour supprimer la contrainte, exécutez la commande suivante : 

- **Exemple RHEL/Ubuntu**

   Dans cet exemple `ag_cluster-master` est le nom de la ressource qui a basculé. 

   ```bash
   sudo pcs resource clear ag_cluster-master 
   ```

- **Exemple SLES**

   Dans cet exemple `ag_cluster` est le nom de la ressource qui a basculé. 

   ```bash
   crm resource clear ag_cluster
   ```

Vous pouvez aussi supprimer la contrainte d’emplacement en exécutant la commande ci-dessous.  

- **Exemple RHEL/Ubuntu**

   Dans la commande `cli-prefer-ag_cluster-master` ci-dessous figure l’ID de la contrainte à supprimer. `sudo pcs constraint --full` retourne cet ID. 

   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```
- **Exemple SLES**

   Dans la commande suivante `cli-prefer-ms-ag_cluster` est l’ID de la contrainte. `crm config show` retourne cet ID. 
   
   ```bash
   crm configure
   delete cli-prefer-ms-ag_cluster 
   commit
   ```

>[!NOTE]
>Comme l’opération de basculement automatique n’ajoute pas de contrainte d’emplacement, aucun nettoyage n’est nécessaire. 

Pour plus d'informations, consultez :
- [Red Hat - Managing Cluster Resources](http://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-manageresource-HAAR.html) (Red Hat - Gestion des ressources de cluster)
- [STIMULATEUR - déplacer manuellement les ressources](http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Clusters_from_Scratch/_move_resources_manually.html)
 [SLES ressources - Guide d’Administration](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.resource) 
 
## <a name="forceFailover"></a> Forcer le basculement 

Un basculement forcé est destiné exclusivement pour la récupération d’urgence. Dans ce cas, vous ne peuvent pas basculer des outils de gestion de cluster, car le centre de données principal est arrêté. Si vous forcez le basculement vers un réplica secondaire qui n'est pas synchronisé, une perte de données est possible. Forcer le basculement uniquement si vous devez restaurer le service pour le groupe de disponibilité immédiatement et prendre le risque de perdre des données.

Si vous ne pouvez pas utiliser les outils de gestion de cluster pour l’interaction avec le cluster - par exemple, si le cluster ne répond pas en raison d’un incident dans le centre de données principal, vous devrez peut-être forcer le basculement d’ignorer le Gestionnaire de cluster externe. Cette procédure n’est pas recommandée pour les opérations courantes, car il présente un risque de perte de données. Utilisez-le lorsque les outils de gestion de cluster ne parviennent pas à exécuter l’action de basculement. Fonctionnellement, cette procédure est similaire à [effectuer un basculement manuel forcé](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) sur un groupe de disponibilité dans Windows.
 
Ce processus pour forcer le basculement est spécifique à SQL Server sur Linux.

1. Vérifiez que la ressource de groupe de disponibilité n’est pas plus gérée par le cluster. 

      - Définissez les ressources non managées en mode sur le nœud de cluster cible. Cette commande signale que l’agent de ressource pour la surveillance des ressources d’arrêt et la gestion. Par exemple : 
      
      ```bash
      sudo pcs resource unmanage <resourceName>
      ```

      - Si la tentative de définir le mode de la ressource en mode non managé échoue, supprimez la ressource. Par exemple :

      ```bash
      sudo pcs resource delete <resourceName>
      ```

      >[!NOTE]
      >Lorsque vous supprimez une ressource, elle supprime également toutes les contraintes associées. 

1. Sur l’instance de SQL Server qui héberge le réplica secondaire, définissez la variable de contexte de session `external_cluster`.

   ```Transact-SQL
   EXEC sp_set_session_context @key = N'external_cluster', @value = N'yes';
   ```

1. Basculer le groupe de disponibilité avec Transact-SQL. Dans l’exemple suivant, remplacez `<MyAg>` par le nom de votre groupe de disponibilité. Connectez-vous à l’instance de SQL Server qui héberge le réplica secondaire cible et exécutez la commande suivante :

   ```Transact-SQL
   ALTER AVAILABILITY GROUP <MyAg> FORCE_FAILOVER_ALLOW_DATA_LOSS;
   ```

1.  Après un basculement forcé, mettre le groupe de disponibilité dans un état sain avant le redémarrage de l’analyse de ressource de cluster et la gestion ou de recréation de la ressource de groupe de disponibilité. Examinez le [tâches essentielles après un basculement forcé](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md#FollowUp).

1.  Soit redémarrer gestion et analyse de ressource de cluster :

   Pour redémarrer la surveillance des ressources de cluster et la gestion, exécutez la commande suivante :

   ```bash
   sudo pcs resource manage <resourceName>
   sudo pcs resource cleanup <resourceName>
   ```

   Si vous avez supprimé la ressource de cluster, le recréer. Pour recréer la ressource de cluster, suivez les instructions à [de créer la ressource de groupe de disponibilité](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).

>[!Important]
>N’utilisez pas les étapes précédentes pour les exercices de récupération d’urgence, car elle risque de perte de données. À la place de modifier le réplica asynchrone à synchrone et les instructions pour [le basculement manuel normal](#manualFailover).

## <a name="database-level-monitoring-and-failover-trigger"></a>Déclencheur de surveillance et de basculement au niveau de base de données

Pour `CLUSTER_TYPE=EXTERNAL`, la sémantique de déclencheur de basculement est différente par rapport à WSFC. Lorsque le groupe de disponibilité se trouve sur une instance de SQL Server dans un cluster WSFC, en cours de transition de `ONLINE` pour la base de données provoque l’intégrité du groupe de disponibilité signaler une erreur d’état. En réponse, le Gestionnaire du cluster déclenche une action de basculement. Sur Linux, l’instance de SQL Server ne peut pas communiquer avec le cluster. Analyse pour le contrôle d’intégrité de la base de données est effectuée *extérieur dans*. Si utilisateur a été sélectionné pour la surveillance de basculement au niveau base de données et de basculement (en définissant l’option `DB_FAILOVER=ON` lorsque vous créez le groupe de disponibilité), le cluster vérifie si l’état de la base de données `ONLINE` chaque fois qu’il s’exécute une action d’analyse. Le cluster interroge l’état dans `sys.databases`. Pour tout état autre que `ONLINE`, cela déclenche un basculement automatique (si les conditions de basculement automatique sont remplies). L’heure réelle du basculement dépend de la fréquence de l’action d’analyse, ainsi que l’état de la base de données en cours de mise à jour dans sys.databases.

Le basculement automatique nécessite au moins un réplica synchrone.

## <a name="next-steps"></a>Étapes suivantes

[Configurer Red Hat Enterprise Linux Cluster pour les ressources de Cluster de groupe de disponibilité de SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configurer SUSE Linux Enterprise Server Cluster pour les ressources de Cluster de groupe de disponibilité de SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurer le Cluster Ubuntu pour les ressources de Cluster de groupe de disponibilité de SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)
