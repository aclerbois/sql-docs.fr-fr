---
title: Toujours sur les groupes de disponibilité pour SQL Server sur Linux | Documents Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 11/27/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: e37742d4-541c-4d43-9ec7-a5f9b2c0e5d1
ms.openlocfilehash: 7de4097fdc843097cbd2865e4a4f3986c392ac04
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="always-on-availability-groups-on-linux"></a>Toujours sur les groupes de disponibilité sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article décrit les caractéristiques des groupes de disponibilité AlwaysOn (groupes de disponibilité) sous basés sur Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] installations. Elle traite également des différences entre Linux et le cluster de basculement Windows Server (WSFC)-en fonction des groupes de disponibilité. Consultez le [documentation sur Windows](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) pour les principes fondamentaux de groupes de disponibilité, comme ils fonctionnent de la même sur Windows et Linux à l’exception du cluster WSFC.

À partir d’un point de vue de niveau supérieur, les groupes de disponibilité sous [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux sont identiques lorsqu’ils sont sur des implémentations WSFC. Cela signifie que les limitations et les fonctionnalités sont identiques, à quelques exceptions. Les principales différences sont les suivantes :

-   Microsoft Distributed Transaction Coordinator (DTC) n’est pas pris en charge sous Linux dans [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. Si vos applications nécessitent l’utilisation de transactions distribuées et avez besoin d’un groupe de disponibilité, déployez [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Windows.
-   Déploiements basés sur Linux utilisent STIMULATEUR au lieu d’un cluster WSFC.
-   Contrairement à la plupart des configurations pour les groupes de disponibilité sur Windows à l’exception de ce scénario de Cluster du groupe de travail, STIMULATEUR ne nécessite jamais les Services de domaine Active Directory (AD DS).
-   L’échec d’un groupe de disponibilité d’un nœud à un autre est différente entre Linux et Windows.
-   Certains paramètres tels que `required_synchronized_secondaries_to_commit` ne peut être modifié via STIMULATEUR sur Linux, tandis qu’une installation basée sur un WSFC utilise Transact-SQL.

## <a name="number-of-replicas-and-cluster-nodes"></a>Nombre de réplicas et les nœuds de cluster

Un groupe de disponibilité dans [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] peut avoir deux réplicas total : un serveur principal et un secondaire qui peut uniquement être utilisé à des fins de disponibilité. Il ne peut pas être utilisé à d’autres fins, telles que les requêtes accessible en lecture. Un groupe de disponibilité dans [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] peut avoir jusqu'à neuf réplicas total : un principal et jusqu'à huit bases de données secondaires, dont trois (y compris le réplica principal) peut être synchrone. Si vous utilisez un cluster sous-jacent, il peut y être un maximum de 16 nœuds total lorsque Corosync est impliqué. Un groupe de disponibilité peut englober au maximum neuf 16 nœuds avec [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)]et deux avec [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)].

Une configuration de deux réplicas qui nécessite la possibilité d’effectuer un basculement automatique vers un autre réplica nécessite l’utilisation d’un réplica de configuration uniquement, comme décrit dans [réplica Configuration uniquement et quorum](#configuration-only-replica-and-quorum). Configuration seule réplicas ont été introduits dans [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] Cumulative Update 1 (CU1), qui doit être la version minimale est déployée pour cette configuration.

Si STIMULATEUR est utilisé, il doit être correctement configuré afin qu’il reste en cours d’exécution. Cela signifie que quorum et STONITH doivent être implémenté correctement à partir d’une perspective STIMULATEUR, outre les [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] spécifications comme un réplica de configuration.

Réplicas secondaires lisibles sont uniquement pris en charge avec [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)].

## <a name="cluster-type-and-failover-mode"></a>Mode de type et le basculement de cluster

Vous débutez avec [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] est l’introduction d’un type de cluster pour les groupes de disponibilité. Pour Linux, il existe deux valeurs valides : externe et None. Un type de cluster d’externes signifie que STIMULATEUR est utilisé sous le groupe de disponibilité. À l’aide externe pour le type de cluster nécessite que le mode de basculement est également définie externe (nouveauté de [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]). Le basculement automatique est pris en charge, mais contrairement à un cluster WSFC, le mode de basculement est défini externe, non automatique, lorsque STIMULATEUR est utilisé. Contrairement à un cluster WSFC, la partie STIMULATEUR du groupe de disponibilité est créée après avoir configuré le groupe de disponibilité.

Un type de cluster None signifie qu’il n’est pas obligatoire pour, ni le groupe de disponibilité utilisera, STIMULATEUR. Même sur les serveurs qui ont STIMULATEUR configuré, si un groupe de disponibilité est configuré avec un type de cluster None, STIMULATEUR ne voient pas ou gérer ce groupe de disponibilité. Un type de cluster None prend uniquement en charge le basculement manuel d’un serveur principal vers un réplica secondaire. Un groupe de disponibilité créé aucune concerne principalement pour la lecture évolutif scénario ainsi que les mises à niveau. Il peut fonctionner dans des scénarios tels que la récupération d’urgence ou de disponibilité local lorsqu’aucun basculement automatique n’est nécessaire, il n’est pas recommandé. L’article de l’écouteur est également plus complexe sans STIMULATEUR.

Type de cluster est stocké dans le [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] vue de gestion dynamique (DMV) `sys.availability_groups`, dans les colonnes `cluster_type` et `cluster_type_desc`.

## <a name="requiredsynchronizedsecondariestocommit"></a>requis\_synchronisé\_secondaires\_à\_validation

Vous débutez avec [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] est un paramètre qui est utilisé par les groupes de disponibilité appelés `required_synchronized_secondaries_to_commit`. Le groupe de disponibilité vous indique le nombre de réplicas secondaires qui doivent être en parallèle avec le réplica principal. Cela permet des éléments tels que le basculement automatique (uniquement lors d’intégrer STIMULATEUR avec un type de cluster d’externes) et contrôle le comportement des éléments comme la disponibilité des principales si le nombre approprié de réplicas secondaires est en ligne ou hors connexion. Pour en savoir plus sur cette procédure, consultez [haute disponibilité et protection des données pour les configurations de groupe de disponibilité](sql-server-linux-availability-group-ha.md). Le `required_synchronized_secondaries_to_commit` valeur est la valeur par défaut et gérée par STIMULATEUR /[!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Vous pouvez substituer manuellement cette valeur.

La combinaison de `required_synchronized_secondaries_to_commit` et le nouveau numéro de séquence (qui est stockée dans `sys.availability_groups`) informe STIMULATEUR et [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] que, par exemple, le basculement automatique peut se produire. Dans ce cas, un réplica secondaire aurait le même numéro de séquence en tant que le serveur principal, ce qui signifie qu’il est à jour avec les dernières informations de configuration.

Il existe trois valeurs qui peuvent être définies pour `required_synchronized_secondaries_to_commit`: 0, 1 ou 2. Ils contrôlent le comportement de ce qui se produit lorsqu’un réplica devient indisponible. Les numéros correspondent au nombre de réplicas secondaires qui doivent être synchronisés avec le réplica principal. Le comportement est comme suit sous Linux :

-   0 – aucun basculement automatique n’est possible car aucun réplica secondaire n’est requise à synchroniser. La base de données principale est disponible en permanence.
-   1 – un réplica secondaire doit être dans un état synchronisé avec le réplica principal ; le basculement automatique est possible. La base de données primaire n’est pas disponible jusqu'à ce qu’un réplica synchrone secondaire n’est disponible.
-   2 – les deux réplicas secondaires dans une configuration de groupe de disponibilité au moins trois nœuds doivent être synchronisés avec le serveur principal ; le basculement automatique est possible.

`required_synchronized_secondaries_to_commit` contrôle non seulement le comportement de basculement avec des réplicas synchrones, mais une perte de données. Avec la valeur 1 ou 2, un réplica secondaire est toujours requis pour être synchronisé, il y aura toujours la redondance des données. Cela signifie aucune perte de données.

Pour modifier la valeur de `required_synchronized_secondaries_to_commit`, utilisez la syntaxe suivante :

>[!NOTE]
>Modification de la valeur provoque à la ressource à redémarrer, ce qui signifie une brève interruption. La seule façon d’éviter ce problème consiste à définir des ne pas être géré par le cluster temporairement la ressource.

**Red Hat Enterprise Linux (RHEL) et Ubuntu**

```bash
sudo pcs resource update <AGResourceName> required_synchronized_secondaries_to_commit=<Value>
```

**SUSE Linux Enterprise Server (SLES)**

```bash
sudo crm resource param ms-<AGResourceName> set required_synchronized_secondaries_to_commit <value>
```

où *AGResourceName* est le nom de la ressource configurée pour le groupe de disponibilité, et *valeur* est 0, 1 ou 2. Pour la définir à la valeur par défaut de STIMULATEUR gérer le paramètre, exécutez la même instruction sans valeur.

Le basculement automatique d’un groupe de disponibilité est possible lorsque les conditions suivantes sont remplies :

-   Le serveur principal et le réplica secondaire sont définies sur le déplacement des données synchrone.
-   La base de données secondaire a un état de synchronisation (sans synchronisation), ce qui signifie que les deux sont sur le même point de données.
-   Le type de cluster est défini en mode externe. Le basculement automatique n’est pas possible avec un type de cluster None.
-   Le `sequence_number` le réplica secondaire qui deviendra le réplica principal a le numéro de séquence plus élevé : en d’autres termes, le réplica secondaire `sequence_number` correspond à celui du réplica principal d’origine.

Si ces conditions sont réunies et que le serveur qui héberge le réplica principal échoue, le groupe de disponibilité changent de propriété vers un réplica synchrone. Le comportement de réplicas synchrones (de laquelle il peut y avoir trois total : un serveur principal et deux réplicas secondaires) peuvent également être gérées par `required_synchronized_secondaries_to_commit`. Cela fonctionne avec les groupes de disponibilité de Windows et Linux, mais il est configuré complètement différemment. Sur Linux, la valeur est configurée automatiquement par le cluster sur la ressource de groupe de disponibilité lui-même.

## <a name="configuration-only-replica-and-quorum"></a>Quorum et le réplica de configuration uniquement

Nouveauté de [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] depuis CU1 est un réplica de configuration. STIMULATEUR étant différent de celui d’un cluster WSFC, en particulier lorsqu’il s’agit de quorum et en demandant STONITH, seulement une configuration de deux nœuds ne fonctionnera pas lorsqu’il s’agit d’un groupe de disponibilité. Pour une instance de cluster, les mécanismes de quorum fournis par STIMULATEUR peuvent être précis, car tous les arbitrage de basculement FCI se produit au niveau du cluster. Pour un groupe de disponibilité, arbitrage sous Linux se produit dans [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]et toutes les métadonnées sont stockées. Il s’agit là le réplica de configuration entre en jeu.

Sans quoi, un troisième nœud et au moins un réplica synchronisé est requis. Cela ne fonctionne pas pour [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)], car il ne peut comporter deux réplicas qui participent à un groupe de disponibilité. Le réplica de configuration stocke la configuration du groupe de disponibilité dans la base de données master, identique à celui des autres réplicas dans la configuration du groupe de disponibilité. Le réplica de configuration n’a pas de bases de données utilisateur participant dans le groupe de disponibilité. Les données de configuration sont envoyées de façon synchrone à partir du principal. Ces données de configuration sont ensuite utilisées pendant les basculements, qu’ils soient automatique ou manuelle.

Pour un groupe de disponibilité gérer le quorum et activer des basculements automatiques avec un type de cluster d’externes, il soit doit :

-   Trois réplicas synchrones ([!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] uniquement) ; ou
-   Avoir deux réplicas (principales et secondaire) ainsi que d’un seul réplica de configuration.

Basculements manuels peuvent se produire si l’aide externe ou aucun cluster types pour les configurations de groupe de disponibilité. Lors d’un réplica de configuration peut être configuré avec un groupe de disponibilité qui a un type de cluster None, il est déconseillé, car cela complique le déploiement. Pour ces configurations, modifiez manuellement `required_synchronized_secondaries_to_commit` doit avoir la valeur d’au moins 1, pour qu’il y a au moins un réplica synchronisé.

Un réplica de configuration peut être hébergé sur n’importe quelle édition de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], y compris [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)]. Cela réduit les coûts des licences et permet de s’assurer qu’il fonctionne avec les groupes de disponibilité dans [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)]. Cela signifie que le troisième requis serveur doit simplement répondre aux spécifications minimales pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], car il ne reçoit pas de trafic de transaction utilisateur pour le groupe de disponibilité.

Lorsqu’un réplica de configuration est utilisé, il a le comportement suivant :

-   Par défaut, `required_synchronized_secondaries_to_commit` est définie sur 0. Cela peut être modifié manuellement à 1 si vous le souhaitez.
-   Si le serveur principal échoue et `required_synchronized_secondaries_to_commit` est 0, le réplica secondaire deviennent le nouveau réplica principal et être disponible pour la lecture et l’écriture. Si la valeur est 1, le basculement automatique se produit, mais n’accepte pas les nouvelles transactions jusqu'à ce que l’autre réplica est en ligne.
-   Si un réplica secondaire échoue et `required_synchronized_secondaries_to_commit` est 0, le réplica principal continue d’accepter les transactions, mais si le serveur principal ne parvient pas à ce stade, il n’existe aucune protection pour les données ni de basculement possible (manuel ou automatique), car un réplica secondaire n’est pas disponible.
-   Si les réplicas de configuration échoue, le groupe de disponibilité fonctionne normalement, mais aucun basculement automatique n’est possible.
-   Si un réplica secondaire synchrone et le réplica de configuration échouent, le serveur principal ne peut pas accepter les transactions et ne peut avoir pour le serveur principal à échouer.

Dans CU1 il existe un bogue connu dans la journalisation dans le fichier corosync.log qui est généré par `mssql-server-ha`. Si un réplica secondaire n’est pas en mesure de devenir le réplica principal en raison du nombre de réplicas requis disponibles, le message en cours indique que « prévus pour recevoir les numéros de séquence de 1, mais uniquement reçu 2. N’est pas suffisant réplicas sont en ligne en toute sécurité promouvoir le réplica local ». Les numéros doivent être annulées, et il doit indiquer « prévus pour recevoir les numéros de séquence de 2, mais uniquement a reçu 1. N’est pas suffisant réplicas sont en ligne en toute sécurité promouvoir le réplica local ». 

## <a name="multiple-availability-groups"></a>Plusieurs groupes de disponibilité 

Plus d’un groupe de disponibilité peut être créé par le cluster de STIMULATEUR ou un ensemble de serveurs. La seule limitation est ressources système. La propriété du groupe de disponibilité est affichée par le maître. Différents groupes de disponibilité peuvent être détenus par différents nœuds ; Il faut pas tous être en cours d’exécution sur le même nœud.

## <a name="drive-and-folder-location-for-databases"></a>Emplacement de lecteur et le dossier de bases de données

Comme sur les systèmes basés sur Windows, la structure de dossier pour les bases de données utilisateur participe à un groupe de disponibilité doit être identique. Par exemple, si les bases de données utilisateur sont dans `/var/opt/mssql/userdata` sur le serveur A, ce même dossier doit exister sur le serveur B. La seule exception à cela est indiquée dans la section [l’interopérabilité avec les groupes de disponibilité de basés sur Windows et les réplicas](#interoperability-with-windows-based-availability-groups-and-replicas).

## <a name="the-listener-under-linux"></a>L’écouteur sous Linux

L’écouteur est une fonctionnalité facultative pour un groupe de disponibilité. Il fournit un point d’entrée unique pour toutes les connexions (en lecture/écriture sur le réplica principal et/ou les réplicas en lecture seule sur le site secondaire) afin que les applications et les utilisateurs finaux n’avez pas besoin de connaître le serveur qui héberge les données. Dans un cluster WSFC, ceci est la combinaison d’une ressource nom réseau et une ressource IP, qui est ensuite enregistrée dans les services AD DS (si nécessaire), ainsi que le DNS. En association avec la ressource de groupe de disponibilité elle-même, il fournit cette abstraction. Pour plus d’informations sur un écouteur, consultez [écouteurs, connectivité Client et basculement d’Application](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).

L’écouteur sous Linux est configuré différemment, mais ses fonctionnalités sont identiques. Il n’existe aucun concept de ressource de nom réseau dans STIMULATEUR ni est un objet créé dans AD DS ; Il est simplement une ressource d’adresse IP créée dans STIMULATEUR pouvant s’exécuter sur tous les nœuds. Une entrée associée à la ressource IP de l’écouteur dans le système DNS avec un « nom convivial » doit être créé. La ressource IP de l’écouteur sera active sur le serveur qui héberge le réplica principal pour ce groupe de disponibilité.

Si STIMULATEUR est utilisé et une ressource d’adresse IP est créée qui est associé à l’écouteur, il y aura une brève interruption comme l’adresse IP s’arrête sur un seul serveur et démarre sur l’autre, s’il s’agit de basculement automatique ou manuelle. Si cette fonction abstraction via la combinaison d’un nom unique et une adresse IP, il ne masque pas la panne. Une application doit être en mesure de gérer la déconnexion à un quelconque des fonctionnalités de détecter et de vous reconnecter.

Toutefois, la combinaison du nom DNS et adresse IP est toujours n’est pas suffisant fournir toutes les fonctionnalités fournies par un port d’écoute sur un cluster WSFC, telles que le routage en lecture seule pour les réplicas secondaires. Lorsque vous configurez un groupe de disponibilité, un « écouteur » doit être configuré dans [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Ceci peut être observé dans l’Assistant, ainsi que la syntaxe Transact-SQL. Il existe deux façons que cela peut être configurée pour fonctionner de la même manière que sur Windows :

-   Pour un groupe de disponibilité avec un type de cluster d’externe, l’adresse IP associé à « écouteur » créé dans [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] doit être l’adresse IP de la ressource créée dans STIMULATEUR.
-   Pour un groupe de disponibilité créé avec un type de cluster None, utilisez l’adresse IP associé avec le réplica principal.

L’instance associée à l’adresse IP fournie puis devienne le coordinateur à des éléments comme les demandes de routage en lecture seule à partir d’applications.

## <a name="interoperability-with-windows-based-availability-groups-and-replicas"></a>Interopérabilité avec les réplicas et les groupes de disponibilité de basés sur Windows 

Un groupe de disponibilité qui a un type de cluster d’externe ou un WSFC ne peut pas avoir ses réplicas Cross-plateformes. Cela est vrai si le groupe de disponibilité est [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] ou [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)]. Cela signifie que dans une configuration de groupe de disponibilité traditionnelle avec un cluster sous-jacent, un réplica ne peut pas être sur un cluster WSFC et l’autre sur Linux avec STIMULATEUR.

Un groupe de disponibilité avec un type de cluster None peut avoir ses réplicas de franchir les limites du système d’exploitation, il peut y avoir deux réplicas basés sur Linux et Windows dans le même groupe de disponibilité. Un exemple est illustré ici dans lequel le réplica principal repose sur Windows, alors que la base de données secondaire se trouve sur une des distributions Linux.

![Hybride None](./media/sql-server-linux-availability-group-overview/image1.png)

Un groupe de disponibilité distribué peut également se croiser les limites du système d’exploitation. Les indicateurs de tables sous-jacentes sont liés par les règles de la façon dont ils sont configurés, tel que celui configuré étant externe Linux uniquement, mais le groupe de disponibilité auquel il est joint peut être configuré à l’aide d’un cluster WSFC. Prenons l'exemple suivant :

![Groupe de disponibilité de serveur de distribution hybride](./media/sql-server-linux-availability-group-overview/image2.png)

<!-- Distributed AGs are also supported for upgrades from [!INCLUDE[sssql15-md](../includes/sssql15-md.md)] to [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. For more information on how to achieve this, see [the article “x”].

If using automatic seeding with a distributed availability group that crosses OSes, it can handle the differences in folder structure. How this works is described in [the documentation for automatic seeding].
-->
 
## <a name="next-steps"></a>Étapes suivantes
[Configurer le groupe de disponibilité pour SQL Server sur Linux](sql-server-linux-availability-group-configure-ha.md)

[Configurer le groupe de disponibilité de la lecture à l’échelle pour SQL Server sur Linux](sql-server-linux-availability-group-configure-rs.md)

[Ajouter le groupe de disponibilité des ressources de Cluster sur RHEL](sql-server-linux-availability-group-cluster-rhel.md)

[Ajouter le groupe de disponibilité des ressources de Cluster sur SLES](sql-server-linux-availability-group-cluster-sles.md)

[Ajouter le groupe de disponibilité des ressources de Cluster sur Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)

[Configurer un groupe de disponibilité d’inter-plateformes](sql-server-linux-availability-group-cross-platform.md)

