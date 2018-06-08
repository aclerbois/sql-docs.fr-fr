---
title: Configurer SLES Cluster pour le groupe de disponibilité de SQL Server | Documents Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 04/30/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 85180155-6726-4f42-ba57-200bf1e15f4d
ms.openlocfilehash: dc6298c55104aeabcf2da799be4ed1977ea39620
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-sles-cluster-for-sql-server-availability-group"></a>Configurer SLES Cluster pour le groupe de disponibilité de SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce guide fournit des instructions pour créer un cluster à trois nœuds pour SQL Server sur SUSE Linux Enterprise Server (SLES) 12 SP2. Pour la haute disponibilité, un groupe de disponibilité sur Linux nécessite trois nœuds, voir [haute disponibilité et protection des données pour les configurations de groupe de disponibilité](sql-server-linux-availability-group-ha.md). La couche de gestion de clusters est basée sur SUSE [haute disponibilité Extension (HAÉ)](https://www.suse.com/products/highavailability) construit sur [STIMULATEUR](http://clusterlabs.org/). 

Pour plus d’informations sur la configuration du cluster, les options de l’agent de ressources, la gestion, meilleures pratiques et recommandations, consultez [SUSE Linux Enterprise haute disponibilité Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

>[!NOTE]
>À ce stade, l’intégration de SQL Server avec STIMULATEUR sur Linux n’est pas comme exhaustivement comme WSFC sur Windows. Service SQL Server sur Linux n’est pas compatible avec les clusters. STIMULATEUR gère l’ensemble de l’orchestration de ressources de cluster, y compris la ressource de groupe de disponibilité. Sur Linux, ne vous fiez pas à toujours sur Disponibilité groupe vues de gestion dynamique (DMV) qui fournissent des informations de cluster comme sys.dm_hadr_cluster. En outre, nom de réseau virtuel est spécifique à WSFC, il n’existe aucun équivalent de la même dans STIMULATEUR. Vous pouvez toujours créer un écouteur pour l’utiliser pour la reconnexion après un basculement transparente, mais vous devez inscrire manuellement le nom de l’écouteur sur le serveur DNS avec l’adresse IP utilisée pour créer la ressource IP virtuelle (comme expliqué dans les sections suivantes).


## <a name="roadmap"></a>Feuille de route

La procédure de création d’un groupe de disponibilité pour la haute disponibilité diffère entre les serveurs Linux et d’un cluster de basculement Windows Server. La liste suivante décrit les étapes principales : 

1. [Configurer SQL Server sur les nœuds de cluster](sql-server-linux-setup.md).

2. [Créer le groupe de disponibilité](sql-server-linux-availability-group-failover-ha.md). 

3. Configurer un gestionnaire de ressources de cluster, comme STIMULATEUR. Ces instructions sont fournies dans ce document.
   
   La façon de configurer un gestionnaire de ressources du cluster dépend de la distribution Linux spécifique. 

   >[!IMPORTANT]
   >Les environnements de production requièrent un agent de délimitation, tels que STONITH pour la haute disponibilité. Les exemples de cet article n’utilisent pas les agents de délimitation. Ils servent de test et de validation uniquement. 
   
   >Un cluster STIMULATEUR utilise délimitation pour retourner le cluster à un état connu. La façon de configurer la délimitation dépend de la distribution et l’environnement. À ce stade, la délimitation n’est pas disponible dans certains environnements de cloud. Consultez [SUSE Linux Enterprise haute disponibilité Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. [Ajouter le groupe de disponibilité en tant que ressource dans le cluster](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server). 

## <a name="prerequisites"></a>Configuration requise

Pour terminer le scénario de bout en bout suivant, vous avez besoin de trois ordinateurs pour déployer le trois nœuds cluster. Les étapes suivantes expliquent comment configurer ces serveurs.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Installer et configurer le système d’exploitation sur chaque nœud de cluster 

La première étape consiste à configurer le système d’exploitation sur les nœuds de cluster. Pour cette procédure pas à pas, utilisez SLES 12 SP2 avec un abonnement valide pour le module complémentaire à haute disponibilité.

### <a name="install-and-configure-sql-server-service-on-each-cluster-node"></a>Installer et configurer le service SQL Server sur chaque nœud de cluster

1. Installer et configurer un service SQL Server sur tous les nœuds. Pour obtenir des instructions détaillées, consultez [installer SQL Server sur Linux](sql-server-linux-setup.md).

1. Désigner un nœud en tant que nœuds principaux et d’autres en tant que bases de données secondaires. Utilisez ces termes tout au long de ce guide.

1. Assurez-vous que les nœuds devant faire partie du cluster peuvent communiquer entre eux.

   L’exemple suivant `/etc/hosts` avec les ajouts de trois nœuds nommés SLES1, SLES2 et SLES3.

   ```
   127.0.0.1   localhost
   10.128.16.33 SLES1
   10.128.16.77 SLES2
   10.128.16.22 SLES3
   ```

   Tous les nœuds de cluster doivent être en mesure des uns aux autres via une connexion SSH. Outils tels que `hb_report` ou `crm_report` (pour la résolution des problèmes) et l’Explorateur de l’historique de Hawk nécessitent un passwordless accès SSH entre les nœuds, sinon ils peuvent uniquement collecter des données à partir du nœud actuel. En cas d’utilisation d’un port SSH non standard, utilisez l’option -X (consultez `man` page). Par exemple, si votre port SSH est 3479, appeler un `crm_report` avec :

   ```bash
   sudo crm_report -X "-p 3479" [...]
   ```

   Pour plus d’informations, consultez la [Guide d’Administration SLES - section divers](http://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc).


## <a name="create-a-sql-server-login-for-pacemaker"></a>Créer une connexion SQL Server pour STIMULATEUR

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="configure-an-always-on-availability-group"></a>Configurer un groupe de disponibilité Always On

Sur des serveurs Linux, configurer le groupe de disponibilité, puis configurez les ressources de cluster. Pour configurer le groupe de disponibilité, consultez [configurer groupe de disponibilité AlwaysOn pour SQL Server sur Linux](sql-server-linux-availability-group-configure-ha.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installer et configurer Pacemaker sur chaque nœud de cluster

1. Installer l’extension de la haute disponibilité

   Pour référence, consultez [installation SUSE Linux Enterprise Server et l’Extension de haute disponibilité](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.installation)

1. Installer un package de l’agent de ressource SQL Server sur les deux nœuds.

   ```bash
   sudo zypper install mssql-server-ha
   ```

## <a name="set-up-the-first-node"></a>Configurer le premier nœud

   Reportez-vous à [des instructions d’installation SLES](http://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node)

1. Connectez-vous en tant que `root` à l’ordinateur physique ou virtuel à utiliser en tant que nœud de cluster.
2. Démarrez le script de démarrage en exécutant :
   ```bash
   sudo ha-cluster-init
   ```

   NTP n’a pas été configuré pour démarrer au démarrage, un message s’affiche. 

   Si vous décidez de continuer, le script génère des clés pour l’accès SSH et l’outil de synchronisation Csync2 automatiquement et démarre les services nécessaires pour les deux. 

3. Pour configurer la couche de communication du cluster (Corosync) : 

   A. Entrez une adresse réseau à lier. Par défaut, le script propose l’adresse réseau d’eth0. Vous pouvez également entrer une adresse réseau différente, par exemple l’adresse de bond0. 

   B. Entrez une adresse de multidiffusion. Le script propose une adresse aléatoire que vous pouvez utiliser comme valeur par défaut. 

   c. Entrez un port de multidiffusion. Le script propose 5405 comme valeur par défaut. 

   d. Pour configurer `SBD ()`, entrez un chemin persistant pour la partition de votre appareil de bloc que vous souhaitez utiliser pour le même jour ouvrable. Le chemin d’accès doit être cohérente sur tous les nœuds du cluster. 
   Enfin, le script démarre le service STIMULATEUR pour mettre le cluster d’un nœud en ligne et d’activer l’interface de gestion Web Hawk2. L’URL à utiliser pour Hawk2 s’affiche sur l’écran. 

4. Pour les détails du processus de configuration, consultez `/var/log/sleha-bootstrap.log`. Vous avez maintenant un cluster à un nœud en cours d’exécution. Vérifiez l’état du cluster avec l’état de crm :

   ```bash
   sudo crm status
   ```

   Vous pouvez également consulter la configuration de cluster avec `crm configure show xml` ou `crm configure show`.

5. La procédure de démarrage crée un utilisateur Linux nommé hacluster avec le mot de passe linux. Remplacez le mot de passe par défaut sûre dès que possible : 

   ```bash
   sudo passwd hacluster
   ```

## <a name="add-nodes-to-the-existing-cluster"></a>Ajouter des nœuds au cluster existant

Si vous avez un cluster exécutant avec un ou plusieurs nœuds, ajouter des nœuds de cluster avec le script de démarrage à haute disponibilité cluster-jointure. Le script requiert seulement l’accès à un nœud de cluster existant et terminera automatiquement la configuration de base sur l’ordinateur actuel. Utilisez les étapes suivantes :

Si vous avez configuré les nœuds de cluster existants avec les `YaST` module de cluster, assurez-vous que les conditions préalables suivantes sont remplies avant d’exécuter `ha-cluster-join`:
- L’utilisateur racine sur les nœuds existants a des clés SSH en place pour la connexion passwordless. 
- `Csync2` est configuré sur les nœuds existants. Pour plus d’informations, consultez Configuration de Csync2 avec YaST. 

1. Ouvrez une session en tant que racine de l’ordinateur physique ou virtuel censé rejoindre le cluster. 
2. Démarrez le script de démarrage en exécutant : 

   ```bash
   sudo ha-cluster-join
   ```

   NTP n’a pas été configuré pour démarrer au démarrage, un message s’affiche. 

3. Si vous décidez de continuer, le système vous demandera l’adresse IP d’un nœud existant. Entrez l’adresse IP. 

4. Si vous n’avez pas déjà configuré un accès SSH passwordless entre les deux ordinateurs, vous serez également être invité au mot de passe racine du nœud existant. 

   Une fois connecté au nœud spécifié, le script copie la configuration Corosync, configure SSH et `Csync2`et met en ligne l’ordinateur actuel en tant que nouveau nœud du cluster. En outre, il démarre le service requis pour Hawk. Si vous avez configuré le stockage partagé avec `OCFS2`, il crée également automatiquement le répertoire de point de montage pour le `OCFS2` système de fichiers. 

5. Répétez les étapes précédentes pour tous les ordinateurs que vous souhaitez ajouter au cluster. 

6. Pour obtenir les détails du processus, consultez `/var/log/ha-cluster-bootstrap.log`. 

1. Vérifiez l’état du cluster avec `sudo crm status`. Si vous avez correctement ajouté le deuxième nœud, voici ce que vous obtenez en sortie :

   ```bash
   sudo crm status
   
   3 nodes configured
   1 resource configured
   Online: [ SLES1 SLES2 SLES3]
   Full list of resources:   
   admin_addr     (ocf::heartbeat:IPaddr2):       Started node1
   ```

   >[!NOTE]
   >`admin_addr` est la ressource de cluster IP virtuelle qui est configurée pendant l’installation de cluster à un nœud initial.

Après avoir ajouté tous les nœuds, vérifiez si vous devez ajuster la stratégie quorum non dans les options de cluster global. Cela est particulièrement important pour les clusters à deux nœuds. Pour plus d’informations, consultez la Section 4.1.2, Option non-quorum-policy. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Définir la propriété cluster cluster-revérification-intervalle

`cluster-recheck-interval` Indique l’intervalle d’interrogation à laquelle le cluster vérifie les modifications dans les paramètres des ressources, des contraintes ou des autres options de cluster. Si un réplica tombe en panne, le cluster tente de redémarrer le réplica à un intervalle est lié par le `failure-timeout` valeur et le `cluster-recheck-interval` valeur. Par exemple, si `failure-timeout` est défini à 60 secondes et `cluster-recheck-interval` est définie à 120 secondes, le redémarrage sera tenté à un intervalle est supérieur à 60 secondes, mais moins de 120 secondes. Nous vous recommandons de définir d’expiration de l’échec à 60 s et de cluster-revérification-intervalle à une valeur qui est supérieure à 60 secondes. Paramètre d’intervalle de revérification de cluster sur une petite valeur n’est pas recommandé.

Pour mettre à jour la valeur de propriété à `2 minutes` exécuter :

```bash
crm configure property cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Si vous disposez déjà d’une ressource de groupe de disponibilité gérée par un cluster STIMULATEUR, notez que toutes les distributions qui utilisent le dernier package 1.1.18-11.el7 STIMULATEUR disponible introduisent un changement de comportement pour le cluster start-échec-est-irrécupérable paramètre lorsque son la valeur est false. Cette modification affecte le flux de travail de basculement. Si un réplica principal connaît une panne, le cluster est prévu pour basculer vers un des réplicas secondaires disponibles. Au lieu de cela, les utilisateurs Remarquez que le cluster conserve la tentative de démarrage du réplica principal a échoué. Si ce principal est jamais en ligne (en raison d’une panne permanente), le cluster ne bascule vers un autre réplica secondaire disponible. Grâce à cette modification, une configuration précédemment recommandée pour définir le début-échec-est-irrécupérable n’est plus valide et que le paramètre doit être restauré à sa valeur par défaut de `true`. En outre, la ressource de groupe de disponibilité doit être mis à jour pour inclure le `failover-timeout` propriété. 
>
>Pour mettre à jour la valeur de propriété à `true` exécuter :
>
>```bash
>crm configure property start-failure-is-fatal=true
>```
>
>Mettre à jour de votre propriété de ressource de groupe de disponibilité existante `failure-timeout` à `60s` exécuter (remplacez `ag1` par le nom de votre ressource de groupe de disponibilité) : 
>
>```bash
>crm configure edit ag1
># In the text editor, add `meta failure-timeout=60s` after any `param`s and before any `op`s
>```

Pour plus d’informations sur les propriétés du cluster STIMULATEUR, consultez [configuration des ressources de Cluster](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_config_crm_resources.html).

# <a name="configure-fencing-stonith"></a>Configurer la délimitation (STONITH)
Fournisseurs de cluster STIMULATEUR nécessitent STONITH doit être activée et un appareil de délimitation configuré pour une installation de clusters prises en charge. Lorsque le Gestionnaire de ressources de cluster ne peut pas déterminer l’état d’un nœud ou d’une ressource sur un nœud, délimitation est utilisée pour remettre le cluster à un état connu.

Délimitation de niveau de ressources permet de garantir principalement aucune altération des données pendant une panne en configurant une ressource. Vous pouvez utiliser la délimitation au niveau de la ressource, par exemple, avec DRBD (Distributed répliquées bloc Device) pour marquer le disque sur un nœud comme obsolètes lorsque la liaison de communication s’arrête.

Délimitation de niveau de nœud garantit qu’un nœud ne s’exécute pas toutes les ressources. Pour ce faire, vous devez réinitialiser le nœud et l’implémentation de stimulateur de celle-ci est appelée STONITH (ce qui signifie « dépanner l’autre nœud dans l’en-tête »). STIMULATEUR prend en charge une grande variété d’isolement des périphériques, tels qu’un onduleur approvisionnement ou gestion des cartes d’interface pour les serveurs.

Pour plus d’informations, consultez [Clusters STIMULATEUR partant](http://clusterlabs.org/doc/en-US/Pacemaker/1.1-plugin/html/Clusters_from_Scratch/ch05.html), [délimitation et Stonith](http://clusterlabs.org/doc/crm_fencing.html) et [SUSE HA documentation : délimitation et STONITH](https://www.suse.com/documentation/sle_ha/book_sleha/data/cha_ha_fencing.html).

Au moment de l’initialisation du cluster, STONITH est désactivée si aucune configuration n’est détectée. Il peut être activé ultérieurement en exécutant la commande suivante :

```bash
sudo crm configure property stonith-enabled=true
```
  
>[!IMPORTANT]
>La désactivation de STONITH est uniquement pour des tests. Si vous envisagez d’utiliser STIMULATEUR dans un environnement de production, vous devez planifier une implémentation STONITH en fonction de votre environnement et qu’il soit activé. SUSE ne fournit pas de délimitation agents Hyper-V ou des environnements de cloud (y compris Azure). Par conséquent, le fournisseur de cluster n’offre pas de prise en charge des clusters de production en cours d’exécution dans ces environnements. Nous travaillons sur une solution pour cet intervalle qui est disponible dans les versions futures.


## <a name="configure-the-cluster-resources-for-sql-server"></a>Configurer les ressources de cluster pour SQL Server

Reportez-vous à [SLES Administration Guid](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config)

### <a name="create-availability-group-resource"></a>Créer la ressource du groupe de disponibilité

La commande suivante crée et configure la ressource du groupe de disponibilité pour les trois réplicas de groupe de disponibilité [ag1]. Les opérations d’analyse et les délais d’attente doivent être spécifiés explicitement dans SLES basée sur le fait que des délais d’attente sont fortement dépendants de la charge de travail et doivent être ajustés avec soin pour chaque déploiement.
Exécutez la commande sur l’un des nœuds du cluster :

1. Exécutez `crm configure` pour ouvrir l’invite de crm :

   ```bash
   sudo crm configure 
   ```

1. Dans l’invite de crm, exécutez la commande suivante pour configurer les propriétés de ressource.

   ```bash
   primitive ag_cluster \
      ocf:mssql:ag \
      params ag_name="ag1" \
      meta failure-timeout=60s \
      op start timeout=60s \
      op stop timeout=60s \
      op promote timeout=60s \
      op demote timeout=10s \
      op monitor timeout=60s interval=10s \
      op monitor timeout=60s interval=11s role="Master" \
      op monitor timeout=60s interval=12s role="Slave" \
      op notify timeout=60s
   ms ms-ag_cluster ag_cluster \
      meta master-max="1" master-node-max="1" clone-max="3" \
     clone-node-max="1" notify="true" \
   commit
      ```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

### <a name="create-virtual-ip-resource"></a>Créer la ressource IP virtuelle

Si vous n’avez pas créé la ressource IP virtuelle lors de l’exécution `ha-cluster-init` vous pouvez créer maintenant cette ressource. La commande suivante crée une ressource IP virtuelle. Remplacez `<**0.0.0.0**>` avec une adresse disponible à partir de votre réseau et `<**24**>` avec le nombre de bits dans le masque de sous-réseau CIDR. Exécutez sur un nœud.

```bash
crm configure \
primitive admin_addr \
   ocf:heartbeat:IPaddr2 \
   params ip=<**0.0.0.0**> \
      cidr_netmask=<**24**>
```

### <a name="add-colocation-constraint"></a>Ajouter une contrainte de colocalisation
Presque chaque décision dans un cluster STIMULATEUR, comme choisir où une ressource doit s’exécuter, s’effectue en comparant les scores. Les scores sont calculés par la ressource et le Gestionnaire de ressources de cluster choisit le nœud avec le score le plus élevé pour une ressource particulière. (Si un nœud possède un score négatif pour une ressource, la ressource ne peut pas s’exécuter sur ce nœud.) Nous pouvons manipuler les décisions du cluster avec des contraintes. Les contraintes ont un score. Si une contrainte a un score inférieur à l’infini, il est uniquement une recommandation. Un score d’infini signifie qu’il est indispensable. Vous souhaitez vous assurer que principal du groupe de disponibilité et virtuel ressource ip sont exécutés sur le même hôte, donc nous allons définir une contrainte de colocalisation avec un score d’infini. 

Pour définir la contrainte de la colocalisation pour l’adresse IP virtuelle pour s’exécuter sur le même nœud que le maître, exécutez la commande suivante sur un nœud :

```bash
crm configure
colocation vip_on_master inf: \
    admin_addr ms-ag_cluster:Master
commit
```

### <a name="add-ordering-constraint"></a>Ajouter la contrainte de classement
La contrainte de colocalisation a une contrainte de classement implicite. Il déplace la ressource IP virtuelle avant de passer la ressource de groupe de disponibilité. Par défaut, la séquence d’événements est : 

1. Ressource de problèmes utilisateur migrer de Nœud1 vers Nœud2 vers le maître du groupe de disponibilité.
2. La ressource IP virtuelle s’arrête sur le nœud 1.
3. La ressource IP virtuelle démarre sur le nœud 2. À ce stade, l’adresse IP temporairement pointe vers le nœud 2 pendant que le nœud 2 est toujours un pre-basculement secondaire. 
4. Le maître du groupe de disponibilité sur le nœud 1 est rétrogradé pour esclave.
5. L’esclave du groupe de disponibilité sur le nœud 2 est promue au masque. 

Pour éviter que l’adresse IP temporairement en pointant sur le nœud avec le serveur secondaire avant le basculement, ajoutez une contrainte de classement. Pour ajouter une contrainte de classement, exécutez la commande suivante sur un nœud : 

```bash
crm crm configure \
   order ag_first inf: ms-ag_cluster:promote admin_addr:start
```


>[!IMPORTANT]
>Une fois que vous configurez le cluster et ajoutez le groupe de disponibilité en tant que ressource de cluster, vous ne pouvez pas utiliser Transact-SQL pour basculer les ressources de groupe de disponibilité. Ressources de cluster de SQL Server sur Linux non couplées aussi étroitement avec le système d’exploitation qu’ils sont sur un Cluster de basculement du serveur Windows (WSFC). Service SQL Server n’est pas informé de la présence du cluster. Orchestration toutes s’effectue via les outils de gestion de cluster. Dans SLES utiliser `crm`. 

Basculer manuellement le groupe de disponibilité avec `crm`. Ne pas lancer le basculement avec Transact-SQL. Pour plus d’informations, consultez [basculement](sql-server-linux-availability-group-failover-ha.md#failover).


Pour plus d'informations, consultez :
- [La gestion des ressources de cluster](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm).   
- [Haute disponibilité Concepts](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.concepts)
- [Référence rapide de STIMULATEUR](https://github.com/ClusterLabs/pacemaker/blob/master/doc/pcs-crmsh-quick-ref.md) 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Étapes suivantes

[Utiliser le groupe de disponibilité haute disponibilité](sql-server-linux-availability-group-failover-ha.md)
