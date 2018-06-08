---
title: SQL Server Always On le modèles de déploiement du groupe de disponibilité | Documents Microsoft
ms.custom: sql-linux
ms.date: 10/16/2017
ms.prod: sql
ms.component: ''
ms.reviewer: ''
ms.suite: sql
ms.technology: linux
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: edd75f68-dc62-4479-a596-57ce8ad632e5
caps.latest.revision: 34
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 36115370063292f3a3302dac4596222bb513fb67
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="high-availability-and-data-protection-for-availability-group-configurations"></a>Haute disponibilité et protection des données pour les configurations de groupe de disponibilité

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article présente les configurations de déploiement pris en charge pour les groupes de disponibilité SQL Server Always On sur des serveurs Linux. Un groupe de disponibilité prend en charge la haute disponibilité et protection des données. Détection de défaillance automatique, le basculement automatique et la reconnexion après un basculement transparente fournissent une haute disponibilité. Réplicas synchronisés fournissent la protection des données. 

Sur un serveur de basculement Windows Cluster (WSFC), une configuration commune pour la haute disponibilité utilise les deux réplicas synchrones et un troisième serveur ou partage de fichiers pour fournir le quorum. Le témoin de partage de fichiers valide la configuration de groupe de disponibilité - état de synchronisation et le rôle du réplica, par exemple. Cette configuration garantit que le réplica secondaire est choisi comme la cible de basculement possède les données les plus récentes et les modifications de configuration de groupe de disponibilité. 

Le cluster WSFC synchronise les métadonnées de configuration pour l’arbitrage de basculement entre les réplicas de groupe de disponibilité et le témoin de partage de fichiers. Lorsqu’un groupe de disponibilité n’est pas sur un cluster WSFC, les instances de SQL Server stockent des métadonnées de configuration dans la base de données master.

Par exemple, un groupe de disponibilité sur un cluster Linux a `CLUSTER_TYPE = EXTERNAL`. Il n’existe aucun WSFC pour décident de basculement. Dans ce cas, les métadonnées de configuration sont gérée et gérée par les instances de SQL Server. Étant donné qu’aucun serveur témoin dans ce cluster, une troisième instance de SQL Server est nécessaire pour stocker les métadonnées d’état de configuration. Les trois instances de SQL Server ces fournissent le stockage des métadonnées distribuée pour le cluster. 

Le Gestionnaire du cluster peut interroger les instances de SQL Server dans le groupe de disponibilité et orchestrer le basculement pour garantir une haute disponibilité. Dans un cluster Linux, STIMULATEUR est le Gestionnaire du cluster. 

SQL Server 2017 CU 1 permet une haute disponibilité pour un groupe de disponibilité avec `CLUSTER_TYPE = EXTERNAL` pour un seul réplica de configuration, ainsi que deux réplicas synchrones. Le seul réplica de configuration peut être hébergé sur n’importe quelle édition de SQL Server 2017 CU1 ou version ultérieure - y compris SQL Server Express edition. Le seul réplica de configuration conserve les informations de configuration sur le groupe de disponibilité dans la base de données master, mais ne contient pas de bases de données utilisateur dans le groupe de disponibilité. 

## <a name="how-the-configuration-affects-default-resource-settings"></a>Impact de la configuration des paramètres de ressource par défaut

SQL Server 2017 introduit le `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` paramètre de ressource de cluster. Ce paramètre garantit le nombre spécifié de l’écriture des réplicas secondaires les données de transaction pour se connecter avant que le réplica principal valide chaque transaction. Lorsque vous utilisez un gestionnaire de cluster externe, ce paramètre affecte la haute disponibilité et protection des données. La valeur par défaut pour le paramètre varie selon l’architecture au moment de que la création de la ressource de cluster. Lorsque vous installez l’agent de la ressource SQL Server - `mssql-server-ha` - et créer une ressource de cluster pour le groupe de disponibilité, le Gestionnaire du cluster détecte la disponibilité du groupe configuration et jeux de `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` en conséquence. 

Si la prise en charge par la configuration, le paramètre d’agent ressource `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est définie sur la valeur qui fournit une haute disponibilité et protection des données. Pour plus d’informations, consultez [agent de ressource comprendre SQL Server pour STIMULATEUR](#pacemakerNotify).

Les sections suivantes expliquent le comportement par défaut pour la ressource de cluster. 

Choisissez une conception de groupe de disponibilité pour répondre aux besoins spécifiques pour la haute disponibilité, la protection des données et en lecture à l’échelle.

Les configurations suivantes décrivent les modèles de conception de groupe de disponibilité et les fonctionnalités de chaque modèle. Ces modèles de conception s’appliquent aux groupes de disponibilité avec `CLUSTER_TYPE = EXTERNAL` pour les solutions de haute disponibilité. 

- **Trois réplicas synchrones**
- **Deux réplicas synchrones**
- **Un seul réplica de configuration et de deux réplicas synchrones**

<a name="threeSynch"></a>

## <a name="three-synchronous-replicas"></a>Trois réplicas synchrones

Cette configuration se compose de trois réplicas synchrones. Par défaut, il fournit une haute disponibilité et protection des données. Elle peut également fournir à l’échelle en lecture.

![Trois réplicas][3]

Un groupe de disponibilité avec trois réplicas synchrones peut fournir à l’échelle en lecture, haute disponibilité et la protection des données. Le tableau suivant décrit le comportement de la disponibilité. 

| |lecture à l’échelle|Haute disponibilité & </br> protection de données | Protection des données
|:---|---|---|---
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 |1<sup>*</sup>|2
|Indisponibilité du réplica principal | Basculement manuel. Perte de données possible. Nouveau réplica principal est R / w. |Basculement automatique. Nouveau réplica principal est R / w. |Basculement automatique. Nouveau réplica principal n’est pas disponible pour les transactions utilisateur jusqu'à ce que le réplica principal précédent récupère et joint le groupe de disponibilité secondaire. 
|Une indisponibilité du réplica secondaire  | Principal est R / w. Aucun basculement automatique si principal n’échoue. |Principal est R / w. Aucun basculement automatique si principal n’échoue également. | Principal n’est pas disponible pour les transactions utilisateur. 
<sup>*</sup> Par défaut

<a name="twoSynch"></a>

## <a name="two-synchronous-replicas"></a>Deux réplicas synchrones

Cette configuration permet la protection des données. Comme les autres configurations de groupe de disponibilité, il permet à l’échelle en lecture. La configuration de deux réplicas synchrones ne fournit pas automatique haute disponibilité. 

![Deux réplicas synchrones][1]

Un groupe de disponibilité avec deux réplicas synchrones offre une protection à l’échelle en lecture et les données. Le tableau suivant décrit le comportement de la disponibilité. 

| |lecture à l’échelle |Protection des données
|:---|---|---
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>*</sup>|1
|Indisponibilité du réplica principal | Basculement manuel. Perte de données possible. Nouveau réplica principal est R / w.| Basculement automatique. Nouveau réplica principal n’est pas disponible pour les transactions utilisateur jusqu'à ce que le réplica principal précédent récupère et joint le groupe de disponibilité secondaire.
|Une indisponibilité du réplica secondaire  |Principal est en lecture/écriture, exécution exposée à des pertes de données. |Principal n’est pas disponible pour les transactions utilisateur jusqu'à ce que celle-ci est résolue secondaire.
<sup>*</sup> Par défaut

>[!NOTE]
>Ceci est le comportement avant SQL Server 2017 CU 1. 

<a name = "configOnly"></a>

## <a name="two-synchronous-replicas-and-a-configuration-only-replica"></a>Un seul réplica de configuration et de deux réplicas synchrones

Un groupe de disponibilité avec réplicas synchrones de deux (ou plus) et un seul réplica de configuration fournit une protection des données et peut également fournir une haute disponibilité. Le diagramme suivant représente cette architecture :

![Groupe de disponibilité uniquement de configuration][2]

1. Réplication synchrone des données utilisateur vers le réplica secondaire. Il inclut également des métadonnées de configuration de groupe de disponibilité.
2. Réplication synchrone des métadonnées de configuration de groupe de disponibilité. Il n’inclut pas de données utilisateur.

Dans le schéma de groupe de disponibilité, un réplica principal envoie les données de configuration pour le réplica secondaire et le seul réplica de configuration. Le réplica secondaire reçoit également des données de l’utilisateur. Le seul réplica de configuration ne reçoit pas de données utilisateur. Le réplica secondaire est en mode de disponibilité synchrone. Le seul réplica de configuration ne contient pas de bases de données dans le groupe de disponibilité - seules les métadonnées sur le groupe de disponibilité. Données de configuration sur le seul réplica de configuration sont validées synchrone.

>[!NOTE]
>Un groupe availabilility avec un seul réplica de configuration est une nouveauté de SQL Server 2017 CU1. Toutes les instances de SQL Server dans le groupe de disponibilité doivent être SQL Server 2017 CU1 ou version ultérieure. 

La valeur par défaut `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est 0. Le tableau suivant décrit le comportement de la disponibilité. 

| |Haute disponibilité & </br> protection de données | Protection des données
|:---|---|---
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>*</sup>|1
|Indisponibilité du réplica principal | Basculement automatique. Nouveau réplica principal est R / w. | Basculement automatique. Nouveau réplica principal n’est pas disponible pour les transactions utilisateur. 
|Panne du réplica secondaire | Primaire est en lecture/écriture, exécution exposée à des pertes de données (si principal échoue et ne peut pas être récupérée). Aucun basculement automatique si principal n’échoue également. | Principal n’est pas disponible pour les transactions utilisateur. Aucun réplica de basculer vers si principal n’échoue également. 
|Panne de réplica configuration uniquement | Principal est R / w. Aucun basculement automatique si principal n’échoue également. | Principal est R / w. Aucun basculement automatique si principal n’échoue également. 
|Base de données secondaire synchrone + configuration uniquement panne de réplica| Principal n’est pas disponible pour les transactions utilisateur. Aucun basculement automatique. | Principal n’est pas disponible pour les transactions utilisateur. Aucun réplica pour le basculement se principal échoue également. 
<sup>*</sup> Par défaut

>[!NOTE]
>L’instance de SQL Server qui héberge le réplica uniquement configuration peut également héberger d’autres bases de données. Il peut également être inclus en tant qu’une base de données uniquement de configuration pour plus d’un groupe de disponibilité. 

## <a name="requirements"></a>Spécifications

* Tous les réplicas dans un groupe de disponibilité avec un seul réplica de configuration doivent être SQL Server 2017 CU 1 ou version ultérieure.
* N’importe quelle édition de SQL Server peut héberger un réplica seule configuration, y compris SQL Server Express. 
* Le groupe de disponibilité a besoin d’au moins un réplica secondaire - outre le réplica principal.
* Réplicas uniquement de configuration ne sont pas dans le nombre maximal de réplicas de chaque instance de SQL Server. Édition standard de SQL Server permet de configurer jusqu'à trois réplicas, SQL Server Enterprise Edition permet jusqu'à 9.

## <a name="considerations"></a>Observations

* Configuration pas plus d’un seul réplica par groupe de disponibilité. 
* Un seul réplica de configuration ne peut pas être un réplica principal.
* Vous ne pouvez pas modifier le mode de disponibilité d’un seul réplica de configuration. Pour passer d’un seul réplica de configuration à un réplica secondaire synchrone ou asynchrone, supprimez le seul réplica de configuration et ajouter un réplica secondaire avec le mode de disponibilité requis. 
* Un seul réplica de configuration est synchrone avec les métadonnées de groupe de disponibilité. Il n’existe aucune donnée de l’utilisateur. 
* Un groupe de disponibilité avec un réplica principal et les réplicas uniquement une configuration, mais aucun réplica secondaire n’est pas valide. 
* Impossible de créer un groupe de disponibilité sur une instance de SQL Server Express edition. 

<a name="pacemakerNotify"></a>

## <a name="understand-sql-server-resource-agent-for-pacemaker"></a>Comprendre l’agent de ressources de SQL Server pour STIMULATEUR

SQL Server 2017 CTP 1.4 ajouté `sequence_number` à `sys.availability_groups` pour autoriser STIMULATEUR identifier la base de données secondaire des réplicas sont avec le réplica principal. `sequence_number` est une valeur BIGINT monolithique qui représente la mise à jour le réplica de groupe de disponibilité local. Mises à jour STIMULATEUR le `sequence_number` avec chaque modification de configuration de groupe de disponibilité. Les exemples de modifications de configuration de basculement, ajout de réplica ou la suppression. Le nombre est mis à jour sur le serveur principal, puis répliqué vers les réplicas secondaires. Par conséquent, un réplica secondaire qui a une configuration à jour a le même numéro de séquence en tant que le serveur principal. 

Lorsque STIMULATEUR décide de promouvoir un réplica principal, il envoie d’abord un *préalable promouvoir* notification à tous les réplicas. Les réplicas de retournent le numéro de séquence. Ensuite, quand il tente réellement STIMULATEUR promouvoir un réplica principal, le réplica lui-même promeut uniquement si son numéro de séquence est le plus élevé de tous les numéros de séquence. Si son propre numéro de séquence ne correspond pas le numéro de séquence le plus élevé, le réplica rejette l’opération de promotion. De cette façon, seul le réplica ayant le numéro de séquence le plus élevé peut être promu principal, ce qui évite toute perte de données. 

Ce processus requiert au moins un réplica disponible pour la promotion avec le même numéro de séquence en tant que le réplica principal précédent. Les jeux de l’agent de ressource STIMULATEUR `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` telles qu’au moins un réplica secondaire synchrone est à jour et disponible pour être la cible d’un basculement automatique par défaut. Avec chaque action d’analyse, la valeur de `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est calculée (et mises à jour si nécessaire). Le `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` la valeur est « nombre de réplicas synchrones » de divisé par 2. Au moment du basculement, l’agent de ressource requiert (`total number of replicas`  -  `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` réplicas) pour répondre à la promouvoir la notification préalable. Le réplica avec la plus grande `sequence_number` est devenu le principal. 

Par exemple, un groupe de disponibilité avec trois réplicas synchrones - un réplica principal et deux réplicas secondaires synchrones.

- `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est 1 ; (3 / 2 -> 1).

- Le nombre de réplicas pour répondre aux préalable promouvoir action requis est 2 ; (3 - 1 = 2). 

Dans ce scénario, les deux réplicas ont répondu pour le basculement doit être déclenchée. Pour le basculement automatique réussi après une panne de réplica principal, les deux réplicas secondaires doivent être mis à jour et de répondre à la promouvoir la notification préalable. S’ils sont en ligne et synchrone, ils ont le même numéro de séquence. Le groupe de disponibilité promeut un d’eux. Si seul un des réplicas secondaires répond à l’avant de promouvoir l’action, l’agent de ressource ne peut pas garantir que la base de données secondaire qui a répondu possède les numéros de séquence le plus élevé et un basculement n’est pas déclenché.

>[!IMPORTANT]
>Quand la valeur de `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est 0, il existe un risque de perte de données. Durant une panne de réplica principal, l’agent de ressource ne déclenche pas automatiquement un basculement. Vous pouvez attendre primary afin de restaurer ou basculer manuellement à l’aide de `FORCE_FAILOVER_ALLOW_DATA_LOSS`.

Vous pouvez choisir de remplacer le comportement par défaut et empêcher la ressource de groupe de disponibilité de paramètre `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` automatiquement.

Le script suivant définit `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 0 sur un groupe de disponibilité nommé `<**ag1**>`. Avant de l’exécuter, remplacez `<**ag1**>` par le nom de votre groupe de disponibilité.

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=0
```

Pour rétablir la valeur par défaut, en fonction de la configuration du groupe de disponibilité s’exécuter :

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=
```

>[!NOTE]
>Lorsque vous exécutez les commandes précédentes, le serveur principal est rétrogradé temporairement la sur le site secondaire, puis promu à nouveau. La mise à jour de ressource entraîne tous les réplicas arrêter et redémarrer. La nouvelle valeur de`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est définie uniquement une fois que les réplicas sont redémarrés, pas instantanément.

## <a name="see-also"></a>Voir aussi

[Groupes de disponibilité sur Linux](sql-server-linux-availability-group-overview.md)

<!--Image references-->
[1]: ./media/sql-server-linux-availability-group-ha/1-read-scale-out.png
[2]: ./media/sql-server-linux-availability-group-ha/2-configuration-only.png
[3]: ./media/sql-server-linux-availability-group-ha/3-three-replica.png
[4]: ./media/sql-server-linux-availability-group-ha/configuration-only-example.png
