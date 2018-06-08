---
title: Fonctionner Red Hat Enterprise Linux partagé du cluster pour SQL Server | Documents Microsoft
description: Implémenter la haute disponibilité en configurant des clusters de disques partagés Red Hat Enterprise Linux pour SQL Server.
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 03/17/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 075ab7d8-8b68-43f3-9303-bbdf00b54db1
ms.openlocfilehash: 71fef5396f5be6fa615de190a9374c646f467e7e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="operate-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>Fonctionnement des clusters de disques partagés Red Hat Enterprise Linux pour SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce document décrit comment effectuer les tâches suivantes pour SQL Server sur un cluster de basculement de disque partagé avec Red Hat Enterprise Linux.

- Basculez manuellement le cluster
- Surveiller un service SQL Server du cluster de basculement
- Ajouter un nœud de cluster
- Supprimer un nœud de cluster
- Modifier la fréquence d’analyse de la ressource SQL Server

## <a name="architecture-description"></a>Description de l’architecture

La couche de clustering basée sur Red Hat Enterprise Linux (RHEL) [module complémentaire de haute disponibilité](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) construit sur [STIMULATEUR](http://clusterlabs.org/). Corosync et STIMULATEUR coordonnent les communications de cluster et la gestion des ressources. L’instance de SQL Server est active sur un nœud ou l’autre.

Le diagramme suivant illustre les composants de cluster Linux avec SQL Server. 

![Red Hat Enterprise Linux 7 partagé de Cluster de disque SQL](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

Pour plus d’informations sur la configuration du cluster, options d’agents de ressources et la gestion, visitez [documentation de référence RHEL](http://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

## <a name = "failManual"></a>Basculement de cluster manuellement

Le `resource move` commande crée une contrainte de forcer la ressource à démarrer sur le nœud cible.  Après l’exécution de la `move` commande, exécute une ressource `clear` supprimera la contrainte, il est possible de le déplacer de nouveau la ressource ou que la ressource d’effectuer un basculement automatique. 

```bash
sudo pcs resource move <sqlResourceName> <targetNodeName>  
sudo pcs resource clear <sqlResourceName> 
```

L’exemple suivant déplace le **mssqlha** ressource à un nœud nommé **sqlfcivm2**, puis supprime la contrainte afin que la ressource peut déplacer ultérieurement vers un autre nœud.  

```bash
sudo pcs resource move mssqlha sqlfcivm2 
sudo pcs resource clear mssqlha 
```

## <a name="monitor-a-failover-cluster-sql-server-service"></a>Surveiller un service SQL Server du cluster de basculement

Afficher l’état actuel du cluster :

```bash
sudo pcs status  
```

Afficher l’état dynamique de cluster et de ressources :

```bash
sudo crm_mon 
```

Afficher les journaux de l’agent de ressources dans `/var/log/cluster/corosync.log`

## <a name="add-a-node-to-a-cluster"></a>Ajouter un nœud à un cluster

1. Vérifiez l’adresse IP pour chaque nœud. Le script suivant montre l’adresse IP du nœud actuel. 

   ```bash
   ip addr show
   ```

3. Le nouveau nœud doit avoir un nom unique qui est de 15 caractères ou moins. Par défaut dans Red Hat Linux, le nom d’ordinateur est `localhost.localdomain`. Ce nom par défaut ne peut pas être unique et est trop long. Définir le nom d’ordinateur du nouveau nœud. Définir le nom d’ordinateur en l’ajoutant à `/etc/hosts`. Le script suivant vous permet de modifier `/etc/hosts` avec `vi`. 

   ```bash
   sudo vi /etc/hosts
   ```

   L’exemple suivant `/etc/hosts` avec les ajouts de trois nœuds nommés `sqlfcivm1`, `sqlfcivm2`, et`sqlfcivm3`.

   ```
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1         localhost localhost6 localhost6.localdomain6
   10.128.18.128 fcivm1
   10.128.16.77 fcivm2
   10.128.14.26 fcivm3
    ```
    
   Le fichier doit être identique sur chaque nœud. 

1. Arrêtez le service SQL Server sur le nouveau nœud.

1. Suivez les instructions pour monter le répertoire du fichier de base de données à l’emplacement partagé :

   À partir du serveur NFS, installer `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils 
   ``` 

   Ouvrez le pare-feu sur les clients et le serveur NFS 

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

   Modifiez le fichier/etc/fstab pour inclure la commande de montage : 

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr
   ```

   Exécutez `mount -a` pour que les modifications prennent effet.
   
1. Sur le nouveau nœud, créez un fichier pour stocker le nom d’utilisateur SQL Server et le mot de passe pour la connexion STIMULATEUR. La commande suivante a pour effet de créer et remplir ce fichier :

   ```bash
   sudo touch /var/opt/mssql/passwd
   sudo echo "<loginName>" >> /var/opt/mssql/secrets/passwd
   sudo echo "<loginPassword>" >> /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/passwd
   sudo chmod 600 /var/opt/mssql/passwd
   ```

3. Sur le nouveau nœud, ouvrez les ports du pare-feu STIMULATEUR. Pour ouvrir ces ports avec `firewalld`, exécutez la commande suivante :

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > [!NOTE]
   > Si vous utilisez un autre pare-feu qui n’intègre pas de configuration à haute disponibilité, les ports suivants doivent être ouverts pour permettre à Pacemaker de communiquer avec les autres nœuds du cluster.
   >
   > * TCP : ports 2224, 3121, 21064
   > * UDP : port 5405

1. Installer des packages de STIMULATEUR sur le nouveau nœud.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
 
2. Définissez le mot de passe pour l’utilisateur par défaut qui est créé pendant l’installation des packages Pacemaker et Corosync. Utilisez le même mot de passe en tant que les nœuds existants. 

   ```bash
   sudo passwd hacluster
   ```
 
3. Activez et démarrez le service `pcsd` et Pacemaker. Ainsi, le nouveau nœud à rejoindre le cluster après le redémarrage. Exécutez la commande suivante sur le nouveau nœud.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. Installez l’agent de ressources FCI pour SQL Server. Exécutez les commandes suivantes sur le nouveau nœud. 

   ```bash
   sudo yum install mssql-server-ha
   ```

1. Sur un nœud existant à partir du cluster, authentifier le nouveau nœud et l’ajouter au cluster :

    ```bash
    sudo pcs    cluster auth <nodeName3> -u hacluster 
    sudo pcs    cluster node add <nodeName3> 
    ```

    L’exemple suivant ajoute un nœud nommé **vm3** au cluster.

    ```bash
    sudo pcs    cluster auth  
    sudo pcs    cluster start 
    ```

## <a name="remove-nodes-from-a-cluster"></a>Supprimer des nœuds d’un cluster

Pour supprimer un nœud d’un cluster, exécutez la commande suivante :

```bash
sudo pcs    cluster node remove <nodeName>  
```

## <a name="change-the-frequency-of-sqlservr-resource-monitoring-interval"></a>Modifier la fréquence de l’intervalle d’analyse de la ressource sqlservr

```bash
sudo pcs    resource op monitor interval=<interval>s <sqlResourceName> 
```

L’exemple suivant définit l’intervalle de surveillance de 2 secondes pour la ressource mssql :

```bash
sudo pcs    resource op monitor interval=2s mssqlha 
```
## <a name="troubleshoot-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>Résoudre les problèmes de cluster de disque partagé Red Hat Enterprise Linux pour SQL Server

Dans la résolution des problèmes de cluster, il peut vous aider à comprendre comment les trois processus fonctionnent ensemble pour gérer les ressources de cluster. 

| Daemon |  Description 
| ----- | -----
| Corosync | Fournit l’appartenance de quorum et de la messagerie entre les nœuds de cluster.
| STIMULATEUR | Réside au-dessus Corosync et fournit les machines d’état pour les ressources. 
| PCSD | Gère STIMULATEUR et Corosync via la `pcs` outils

PCSD doit être en cours d’exécution pour pouvoir utiliser `pcs` outils. 

### <a name="current-cluster-status"></a>État actuel de cluster 

`sudo pcs status` Retourne des informations de base sur l’état pour chaque nœud de cluster, les quorum, les nœuds, les ressources et le démon. 

Un exemple d’une sortie de quorum sain STIMULATEUR serait :

```
Cluster name: MyAppSQL 
Last updated: Wed Oct 31 12:00:00 2016  Last change: Wed Oct 31 11:00:00 2016 by root via crm_resource on sqlvmnode1 
Stack: corosync 
Current DC: sqlvmnode1  (version 1.1.13-10.el7_2.4-44eb2dd) - partition with quorum 
3 nodes and 1 resource configured 

Online: [ sqlvmnode1 sqlvmnode2 sqlvmnode3] 

Full list of resources: 

mssqlha (ocf::sql:fci): Started sqlvmnode1 

PCSD Status: 
sqlvmnode1: Online 
sqlvmnode2: Online 
sqlvmnode3: Online 

Daemon Status: 
corosync: active/disabled 
pacemaker: active/enabled 
```

Dans l’exemple, `partition with quorum` signifie qu’un quorum de la majorité des nœuds est en ligne. Si le cluster perd le quorum majoritaire de nœuds, `pcs status` retournera `partition WITHOUT quorum` et toutes les ressources va être arrêtés. 

`online: [sqlvmnode1 sqlvmnode2 sqlvmnode3]` Retourne le nom de tous les nœuds participant dans le cluster. Si tous les nœuds ne font pas partie, `pcs status` retourne `OFFLINE: [<nodename>]`.

`PCSD Status` Affiche l’état du cluster pour chaque nœud.

### <a name="reasons-why-a-node-may-be-offline"></a>Raisons pour lesquelles un nœud peut être en mode hors connexion

Vérifiez les éléments suivants lorsqu’un nœud est hors connexion.

- **Pare-feu**

    Les ports suivants doivent être ouverts sur tous les nœuds pour STIMULATEUR être en mesure de communiquer.
    
    - ** TCP : 2224, 3121, 21064

- **STIMULATEUR ou services Corosync en cours d’exécution**

- **Communication entre les nœuds**

- **Mappages de noms de nœud**

## <a name="additional-resources"></a>Ressources supplémentaires

* [Cluster de toutes pièces](http://clusterlabs.org/doc/Cluster_from_Scratch.pdf) guide de STIMULATEUR

## <a name="next-steps"></a>Étapes suivantes

[Configurer des clusters de disques partagés Red Hat Enterprise Linux pour SQL Server](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)

