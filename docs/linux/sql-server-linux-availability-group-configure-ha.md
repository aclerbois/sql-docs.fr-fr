---
title: Configurer SQL Server groupe de disponibilité AlwaysOn pour une haute disponibilité sur Linux | Documents Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 02/14/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 495646f4394d9639493a6e15846c80116a8bcee8
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-sql-server-always-on-availability-group-for-high-availability-on-linux"></a>Configurer SQL Server groupe de disponibilité AlwaysOn pour une haute disponibilité sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article décrit comment créer un SQL Server toujours sur disponibilité Group (AG) pour la haute disponibilité sur Linux. Il existe deux types de configuration pour les groupes de disponibilité. A *haute disponibilité* configuration utilise un gestionnaire de cluster pour assurer la continuité. Cette configuration peut également inclure les réplicas en lecture à l’échelle. Ce document explique comment créer le groupe de disponibilité pour la haute disponibilité.

Vous pouvez également créer un groupe de disponibilité sans un cluster responsable *en lecture à l’échelle*. Le groupe de disponibilité pour la mise à l’échelle en lecture fournit uniquement des réplicas en lecture seule pour les performances montée en puissance parallèle. Il ne fournit pas de haute disponibilité. Pour créer un groupe de disponibilité pour la lecture à l’échelle, consultez [configurer un groupe de disponibilité de SQL Server pour la lecture à l’échelle sur Linux](sql-server-linux-availability-group-configure-rs.md).

Configurations de garantissent la haute disponibilité et protection des données nécessitent deux ou trois synchrone de réplicas de validation. Avec trois réplicas synchrones, le groupe de disponibilité peut automatiquement récupérer même si un serveur n’est pas disponible. Pour plus d’informations, consultez [haute disponibilité et protection des données pour les configurations de groupe de disponibilité](sql-server-linux-availability-group-ha.md). 

Tous les serveurs doivent être physiques ou virtuels et serveurs virtuels doivent se trouver sur la même plateforme de virtualisation. Cette exigence est, car les agents de délimitation sont spécifiques à la plateforme. Consultez [des stratégies pour les Clusters invités](https://access.redhat.com/articles/29440#guest_policies).

## <a name="roadmap"></a>Feuille de route

Les étapes pour créer un groupe de disponibilité sur des serveurs Linux pour une haute disponibilité sont différentes des étapes sur un cluster de basculement Windows Server. La liste suivante décrit les étapes principales : 

1. [Configurer SQL Server sur les trois serveurs de cluster](sql-server-linux-setup.md).

   >[!IMPORTANT]
   >Les trois serveurs dans le groupe de disponibilité doivent être sur la même plateforme - physique ou virtuelle - étant donné que Linux haute disponibilité utilise des agents de délimitation pour isoler les ressources sur les serveurs. Les agents de délimitation sont spécifiques pour chaque plateforme.

2. Créer le groupe de disponibilité. Cette étape est abordée dans cet article en cours. 

3. Configurer un gestionnaire de ressources de cluster, comme STIMULATEUR.
   
   La façon de configurer un gestionnaire de ressources du cluster dépend de la distribution Linux spécifique. Consultez les liens suivants pour obtenir des instructions spécifiques de distribution : 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md)
   * [SUSE](sql-server-linux-availability-group-cluster-sles.md)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)

   >[!IMPORTANT]
   >Les environnements de production requièrent un agent de délimitation, tels que STONITH pour la haute disponibilité. Les démonstrations dans cette documentation n’utilisent pas les agents de délimitation. Les démonstrations sont pour le test et de validation uniquement. 
   
   >Un cluster Linux utilise délimitation pour retourner le cluster à un état connu. La façon de configurer la délimitation dépend de la distribution et l’environnement. Actuellement, la délimitation n’est pas disponible dans certains environnements de cloud. Pour plus d’informations, consultez [des stratégies de prise en charge de RHEL haute disponibilité Clusters - plateformes de virtualisation](https://access.redhat.com/articles/29440).
   
   >Pour SLES, consultez [SUSE Linux Enterprise haute disponibilité Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. Ajoutez le groupe de disponibilité en tant que ressource dans le cluster.  

   La façon d’ajouter le groupe de disponibilité en tant que ressource dans le cluster dépend de la distribution de Linux. Consultez les liens suivants pour obtenir des instructions spécifiques de distribution : 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource)
   * [SLES](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource)

[!INCLUDE [Create Prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Créer le groupe de disponibilité

Pour une configuration de haute disponibilité qui garantit que le basculement automatique, le groupe de disponibilité requiert au moins trois réplicas. Une des configurations suivantes peut prendre en charge une haute disponibilité :

- [Trois réplicas synchrones](sql-server-linux-availability-group-ha.md#threeSynch)

- [Un réplica de la configuration ainsi que deux réplicas synchrones](sql-server-linux-availability-group-ha.md#twoSynch)

Pour plus d’informations, consultez [haute disponibilité et protection des données pour les configurations de groupe de disponibilité](sql-server-linux-availability-group-ha.md).

>[!NOTE]
>Les groupes de disponibilité peuvent inclure des réplicas synchrones ou asynchrones. 

Créez le groupe de disponibilité pour la haute disponibilité sur Linux. Utilisez le [créer un groupe de disponibilité](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-availability-group-transact-sql) avec `CLUSTER_TYPE = EXTERNAL`. 

* Groupe de disponibilité - `CLUSTER_TYPE = EXTERNAL` Spécifie qu’une entité externe cluster gère le groupe de disponibilité. STIMULATEUR est un exemple d’une entité externe. Lorsque le type de cluster du groupe de disponibilité est externe, 

* Définir les réplicas principal et secondaire `FAILOVER_MODE = EXTERNAL`. 
   Spécifie que le réplica interagit avec un gestionnaire de cluster externe, comme STIMULATEUR. 

Les scripts Transact-SQL suivants créent un groupe de disponibilité pour la haute disponibilité nommée `ag1`. Le script configure les réplicas de groupe de disponibilité avec `SEEDING_MODE = AUTOMATIC`. Ce paramètre permet à SQL Server créer automatiquement la base de données sur chaque serveur secondaire. Mettre à jour le script suivant pour votre environnement. Remplacez le `<node1>`, `<node2>`, ou `<node3>` valeurs avec les noms des instances de SQL Server qui hébergent les réplicas. Remplacez le `<5022>` avec le port que vous définissez pour les données de point de terminaison de mise en miroir. Pour créer le groupe de disponibilité, exécutez l’instruction Transact-SQL suivant sur l’instance de SQL Server qui héberge le réplica principal.

Exécutez **qu’une seule** des scripts suivants : 

- [Création de groupe de disponibilité avec trois réplicas synchrones](#threeSynch).
- [Création de groupe de disponibilité avec deux réplicas synchrones et un réplica de la configuration](#configOnly)
- [Création de groupe de disponibilité avec deux réplicas synchrones](#readScale).

<a name="threeSynch"></a>

- Créer un groupe de disponibilité avec trois réplicas synchrones

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
       WITH (DB_FAILOVER = ON, CLUSTER_TYPE = EXTERNAL)
       FOR REPLICA ON
           N'<node1>' 
            WITH (
               ENDPOINT_URL = N'tcp://<node1>:<5022>',
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node2>' 
            WITH ( 
               ENDPOINT_URL = N'tcp://<node2>:<5022>', 
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node3>'
           WITH( 
              ENDPOINT_URL = N'tcp://<node3>:<5022>', 
              AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
              FAILOVER_MODE = EXTERNAL,
              SEEDING_MODE = AUTOMATIC
              );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```

   >[!IMPORTANT]
   >Après avoir exécuté le script précédent pour créer un groupe de disponibilité avec trois réplicas synchrones, ne pas exécuter le script suivant :

- Créer le groupe de disponibilité avec deux réplicas synchrones et un réplica de la configuration :

   >[!IMPORTANT]
   >Cette architecture permet à n’importe quelle édition de SQL Server pour héberger le réplica de tiers. Par exemple, le troisième réplica peut être hébergé sur SQL Server Enterprise Edition. Sur l’édition entreprise, le type de point de terminaison valide uniquement est `WITNESS`. 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1] 
      WITH (CLUSTER_TYPE = EXTERNAL) 
      FOR REPLICA ON 
       N'<node1>' WITH ( 
          ENDPOINT_URL = N'tcp://<node1>:<5022>', 
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = EXTERNAL, 
          SEEDING_MODE = AUTOMATIC 
          ), 
       N'<node2>' WITH (  
          ENDPOINT_URL = N'tcp://<node2>:<5022>',  
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = EXTERNAL, 
          SEEDING_MODE = AUTOMATIC 
          ), 
       N'<node3>' WITH ( 
          ENDPOINT_URL = N'tcp://<node3>:<5022>', 
          AVAILABILITY_MODE = CONFIGURATION_ONLY  
          );
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```
<a name="readScale"></a>

- Créer un groupe de disponibilité avec deux réplicas synchrones

   Inclure les deux réplicas avec le mode de disponibilité synchrone. Par exemple, le script suivant crée un groupe de disponibilité appelé `ag1`. `node1` et `node2` hébergent des réplicas en mode synchrone, avec l’amorçage automatique et le basculement automatique.

   >[!IMPORTANT]
   >Uniquement, exécutez le script suivant pour créer un groupe de disponibilité avec deux réplicas synchrones. N’exécutez pas le script suivant si vous avez exécuté le script précédent. 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
      WITH (CLUSTER_TYPE = EXTERNAL)
      FOR REPLICA ON
      N'node1' WITH (
         ENDPOINT_URL = N'tcp://node1:5022',
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      ),
      N'node2' WITH ( 
         ENDPOINT_URL = N'tcp://node2:5022', 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```


Vous pouvez également configurer un groupe de disponibilité avec `CLUSTER_TYPE=EXTERNAL` à l’aide de SQL Server Management Studio ou PowerShell. 

### <a name="join-secondary-replicas-to-the-ag"></a>Joindre les réplicas secondaires pour le groupe de disponibilité

Le script Transact-SQL suivant joint une instance de SQL Server à un groupe de disponibilité nommé `ag1`. Mettre à jour le script pour votre environnement. Sur chaque instance de SQL Server qui héberge un réplica secondaire, exécutez Transact-SQL suivant pour rejoindre le groupe de disponibilité.

```Transact-SQL
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create Post](../includes/ss-linux-cluster-availability-group-create-post.md)]

>[!IMPORTANT]
>Après avoir créé le groupe de disponibilité, vous devez configurer l’intégration avec une technologie de cluster comme STIMULATEUR pour la haute disponibilité. Pour une configuration en lecture à l’échelle à l’aide de groupes de disponibilité, en commençant par [!INCLUDE [SQL Server version](..\includes\sssqlv14-md.md)], configuration d’un cluster n’est pas requis.

Si vous avez suivi les étapes décrites dans ce document, vous avez un groupe de disponibilité n’est pas mis en cluster. L’étape suivante consiste à ajouter le cluster. Cette configuration est valide pour les scénarios de l’équilibrage de charge/mise à l’échelle en lecture, il n’est pas terminée pour la haute disponibilité. Pour la haute disponibilité, vous devez ajouter le groupe de disponibilité en tant que ressource de cluster. Consultez [étapes](#next-steps) pour obtenir des instructions. 

## <a name="notes"></a>Remarques

>[!IMPORTANT]
>Une fois que vous configurez le cluster et ajoutez le groupe de disponibilité en tant que ressource de cluster, vous ne pouvez pas utiliser Transact-SQL pour basculer les ressources du groupe de disponibilité. Ressources de cluster de SQL Server sur Linux non couplées aussi étroitement avec le système d’exploitation qu’ils sont sur un Cluster de basculement du serveur Windows (WSFC). Service SQL Server n’est pas informé de la présence du cluster. Orchestration toutes s’effectue via les outils de gestion de cluster. Dans RHEL ou Ubuntu utiliser `pcs`. Dans SLES utiliser `crm`. 

>[!IMPORTANT]
>Si le groupe de disponibilité est une ressource de cluster, existe un problème connu dans la version actuelle, le basculement forcé avec une perte de données vers un réplica asynchrone ne fonctionne pas. Ce problème sera corrigé dans la version à venir. Basculement manuel ou automatique pour un réplica synchrone réussit. 


## <a name="next-steps"></a>Étapes suivantes

[Configurer Red Hat Enterprise Linux Cluster pour les ressources de Cluster de groupe de disponibilité de SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configurer SUSE Linux Enterprise Server Cluster pour les ressources de Cluster de groupe de disponibilité de SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurer le Cluster Ubuntu pour les ressources de Cluster de groupe de disponibilité de SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)
