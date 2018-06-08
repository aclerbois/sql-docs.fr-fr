---
title: Configurer un cluster partagé de Red Hat Enterprise Linux pour SQL Server | Documents Microsoft
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
ms.assetid: dcc0a8d3-9d25-4208-8507-a5e65d2a9a15
ms.openlocfilehash: 7ddd34e56d8f8499715c535de21ae6f23bd282b1
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>Configurer des clusters de disques partagés Red Hat Enterprise Linux pour SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce guide fournit des instructions pour créer un cluster de disque partagé de deux nœuds pour SQL Server sur Red Hat Enterprise Linux. La couche de clustering basée sur Red Hat Enterprise Linux (RHEL) [module complémentaire de haute disponibilité](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) construit sur [STIMULATEUR](http://clusterlabs.org/). L’instance de SQL Server est active sur un nœud ou l’autre.

> [!NOTE] 
> Accès à un module complémentaire Red Hat à haute disponibilité et de la documentation nécessite un abonnement. 

Comme le montre le diagramme suivant, le stockage est présenté à deux serveurs. Les composants clusters - Corosync et STIMULATEUR - coordonnent les communications et gestion des ressources. Un des serveurs a la connexion active pour les ressources de stockage et le serveur SQL Server. Lorsque STIMULATEUR détecte une défaillance les composants clusters gèrent le déplacement des ressources vers un autre nœud.  

![Red Hat Enterprise Linux 7 partagé de Cluster de disque SQL](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

Pour plus d’informations sur la configuration du cluster, options d’agents de ressources et la gestion, visitez [documentation de référence RHEL](http://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).


> [!NOTE] 
> À ce stade, l’intégration de SQL Server avec STIMULATEUR n’est pas comme exhaustivement comme WSFC sur Windows. Dans SQL, il n’est pas connaissance de la présence du cluster, tous les l’orchestration est en dehors d’et le service est contrôlé sous la forme d’une instance autonome par STIMULATEUR. Par exemple, sys.dm_os_cluster_properties et sys.dm_os_cluster_nodes de vues de gestion dynamique de cluster ne peut aucun enregistrement.
Pour utiliser une chaîne de connexion qui pointe vers un nom de serveur de chaîne et n’utilisez pas l’adresse IP, ils doivent inscrire dans leur serveur DNS l’adresse IP utilisée pour créer la ressource IP virtuelle (comme expliqué dans les sections suivantes) avec le nom de serveur choisi.

Les sections suivantes les différentes étapes pour configurer une solution de cluster de basculement. 

## <a name="prerequisites"></a>Configuration requise

Pour terminer le scénario de bout en bout suivant, vous avez besoin de deux ordinateurs pour déployer le cluster à deux nœuds et un autre serveur pour configurer le serveur NFS. Les étapes ci-dessous décrivent la configuration de ces serveurs.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Installer et configurer le système d’exploitation sur chaque nœud de cluster

La première étape consiste à configurer le système d’exploitation sur les nœuds de cluster. Pour cette procédure pas à pas, utilisez RHEL avec un abonnement valide pour le module complémentaire à haute disponibilité. 

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>Installer et configurer SQL Server sur chaque nœud de cluster

1. Installer et configurer SQL Server sur les deux nœuds.  Pour obtenir des instructions détaillées, consultez [installer SQL Server sur Linux](sql-server-linux-setup.md).

1. Désigner un nœud en tant que principal et l’autre comme secondaire, à des fins de configuration. Utiliser ces termes pour les éléments suivants ce guide.  

1. Sur le nœud secondaire, arrêter et désactiver SQL Server.

   L’exemple suivant arrête et désactive les SQL Server : 

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl disable mssql-server
   ```
> [!NOTE] 
> Au moment de l’installation, une clé principale du serveur est généré pour l’instance de SQL Server et placées à `/var/opt/mssql/secrets/machine-key`. Sur Linux, SQL Server s’exécute toujours comme un compte local nommé mssql. S’agissant d’un compte local, son identité n’est pas partagée entre les nœuds. Par conséquent, vous devez copier la clé de chiffrement à partir du nœud principal à chaque nœud secondaire pour chaque compte mssql local puisse accéder pour déchiffrer la clé principale du serveur. 

1. Sur le nœud principal, créez une connexion SQL server pour Pacemaker et accorder l’autorisation de connexion pour exécuter `sp_server_diagnostics`. STIMULATEUR utilise ce compte pour vérifier le nœud qui exécute SQL Server. 

   ```bash
   sudo systemctl start mssql-server
   ```

   Se connecter à la base de données `master` de SQL Server de avec le compte sa et exécutez la commande suivante :

   ```bashsql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'

   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```
   Vous pouvez aussi définir les autorisations à un niveau plus granulaire. La connexion STIMULATEUR nécessite `VIEW SERVER STATE` demander l’état d’intégrité avec sp_server_diagnostics, `setupadmin` et `ALTER ANY LINKED SERVER` pour mettre à jour le nom de l’instance FCI avec le nom de ressource en exécutant sp_dropserver et sp_addserver. 

1. Sur le nœud principal, arrêtez et désactivez SQL Server. 

1. Configurer le fichier hosts de chaque nœud du cluster. Le fichier d’hôte doit inclure l’adresse IP et le nom de chaque nœud de cluster. 

    Vérifiez l’adresse IP pour chaque nœud. Le script suivant montre l’adresse IP du nœud actuel. 

   ```bash
   sudo ip addr show
   ```

   Définir le nom d’ordinateur sur chaque nœud. Donnez à chaque nœud un nom unique de 15 caractères au maximum. Définir le nom d’ordinateur en l’ajoutant à `/etc/hosts`. Le script suivant vous permet de modifier `/etc/hosts` avec `vi`. 

   ```bash
   sudo vi /etc/hosts
   ```
   L’exemple suivant montre `/etc/hosts` avec les ajouts de deux nœuds nommés `sqlfcivm1` et `sqlfcivm2`.

   ```bash
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1       localhost localhost6 localhost6.localdomain6
   10.128.18.128 sqlfcivm1
   10.128.16.77 sqlfcivm2
   ```

Dans la section suivante vous configurerez un stockage partagé et déplacer vos fichiers de base de données pour que le stockage. 

## <a name="configure-shared-storage-and-move-database-files"></a>Configurer le stockage partagé et déplacer les fichiers de base de données 

Il existe un large éventail de solutions pour fournir un stockage partagé. Cette procédure pas à pas illustre la configuration du stockage partagé avec NFS. Nous vous recommandons de suivre les meilleures pratiques et d’utiliser Kerberos pour sécuriser NFS (vous trouverez un exemple ici : https://www.certdepot.net/rhel7-use-kerberos-control-access-nfs-network-shares/). 

>[!Warning]
>Si vous ne sécurisez pas NFS, toute personne qui peut accéder à votre réseau et usurper l’identité de l’adresse IP d’un nœud SQL seront en mesure d’accéder à vos fichiers de données. Comme toujours, assurez-vous que votre système de modèle de menaces avant de l’utiliser en production. Une autre option de stockage consiste à utiliser le partage de fichiers SMB.

### <a name="configure-shared-storage-with-nfs"></a>Configurer le stockage partagé avec NFS

> [!IMPORTANT] 
> Qui héberge les fichiers de base de données sur un serveur NFS version < 4 n’est pas pris en charge dans cette version. Cela inclut l’utilisation de NFS pour le disque partagé clustering de basculement, ainsi que des bases de données sur les instances non cluster. Nous travaillons sur l’activation d’autres versions de serveur NFS dans les versions à venir. 

Sur le serveur NFS, procédez comme suit :

1. Installation de `nfs-utils`.

   ```bash
   sudo yum -y install nfs-utils
   ```

1. Activer et démarrer `rpcbind`

   ```bash
   sudo systemctl enable rpcbind && sudo systemctl start rpcbind
   ```

1. Activer et démarrer `nfs-server`
 
   ```bash
   sudo systemctl enable nfs-server && sudo systemctl start nfs-server
   ```
 
1.  Modifier `/etc/exports` pour exporter le répertoire que vous souhaitez partager. Vous devez 1 ligne pour chaque action que vous souhaitez. Par exemple : 

   ```bash
   /mnt/nfs  10.8.8.0/24(rw,sync,no_subtree_check,no_root_squash)
   ```

1. Exporter les partages

   ```bash
   sudo exportfs -rav
   ```

1. Vérifiez que les chemins d’accès sont partagés/exporté, exécutez à partir du serveur NFS

   ```bash
   sudo showmount -e
   ```

1. Ajouter l’exception dans SELinux

   ```bash
   sudo setsebool -P nfs_export_all_rw 1
   ```
   
1. Ouvrez le pare-feu du serveur.

   ```bash 
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>Configurez tous les nœuds de cluster pour vous connecter au stockage partagées NFS

Procédez comme suit sur tous les nœuds de cluster.

1.  Installation de `nfs-utils`.

   ```bash
   sudo yum -y install nfs-utils
   ```

1. Ouvrez le pare-feu sur les clients et le serveur NFS

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

1. Vérifiez que vous pouvez voir les partages NFS sur les ordinateurs clients

   ```bash
   sudo showmount -e <IP OF NFS SERVER>
   ```

1. Répétez ces étapes sur tous les nœuds de cluster.

Pour plus d’informations sur l’utilisation de NFS, voir les ressources suivantes :

* [NFS serveurs et firewalld | Pile Exchange](http://unix.stackexchange.com/questions/243756/nfs-servers-and-firewalld)
* [Montage d’un Volume NFS | Guide de l’administrateur réseau Linux](http://www.tldp.org/LDP/nag2/x-087-2-nfs.mountd.html)
* [Configuration du serveur NFS](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/Reference_Guide/s1-nfs-server-export.html)

### <a name="mount-database-files-directory-to-point-to-the-shared-storage"></a>Monter le répertoire des fichiers de base de données pour pointer vers le stockage partagé

1.  **Sur le nœud principal uniquement**, enregistrer les fichiers de base de données dans un emplacement temporaire. Le script suivant crée un répertoire temporaire, copie les fichiers de base de données vers le nouveau répertoire et supprime les anciens fichiers de base de données. Lorsque SQL Server s’exécute en tant qu’utilisateur local mssql, vous devez vous assurer qu’après le transfert de données pour le partage monté, utilisateur local a accès en lecture-écriture au partage. 

   ``` 
   $ sudo su mssql
   $ mkdir /var/opt/mssql/tmp
   $ cp /var/opt/mssql/data/* /var/opt/mssql/tmp
   $ rm /var/opt/mssql/data/*
   $ exit
   ``` 

1.  Sur tous les nœuds de cluster modifier `/etc/fstab` fichier à inclure la commande de montage.  

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr 
   ```
   
   Le script suivant montre un exemple de la modification.  

   ``` 
   10.8.8.0:/mnt/nfs /var/opt/mssql/data nfs timeo=14,intr 
   ``` 
> [!NOTE] 
>Si vous utilisez une ressource de système de fichiers (FS) comme indiqué ici, il n’est pas nécessaire de conserver la commande de montage dans/etc/fstab. STIMULATEUR s’occupe de montage du dossier quand il démarre la ressource FS en cluster. À l’aide de délimitation, elle garantit que le système de fichiers n’est jamais monté à deux reprises. 

1.  Exécutez `mount -a` commande pour le système mettre à jour les chemins d’accès montés.  

1.  Copiez les fichiers journaux et de base de données que vous avez enregistré à `/var/opt/mssql/tmp` au partage qui vient d’être monté `/var/opt/mssql/data`. Cette opération ne doit être effectué **sur le nœud principal**. Assurez-vous que vous accordez des autorisations de lecture/écriture à l’utilisateur local de « mssql ».

   ``` 
   $ sudo chown mssql /var/opt/mssql/data
   $ sudo chgrp mssql /var/opt/mssql/data
   $ sudo su mssql
   $ cp /var/opt/mssql/tmp/* /var/opt/mssql/data/
   $ rm /var/opt/mssql/tmp/*
   $ exit
   ``` 
 
1.  Vérifiez que SQL Server démarre correctement avec le nouveau chemin de fichier. Cela sur chaque nœud. À ce stade qu’un seul nœud doit exécuter SQL Server à la fois. Ils ne peuvent pas les deux s’exécuter en même temps, car ils essaieront à la fois d’accéder aux fichiers de données simultanément (pour éviter le démarrage accidentellement de SQL Server sur les deux nœuds, une ressource de cluster du système de fichiers pour vous assurer que le partage n’est pas monté deux fois par différents nœuds). Les commandes suivantes démarrer SQL Server, vérifient l’état, puis arrêtez SQL Server.
 
   ```bash
   sudo systemctl start mssql-server
   sudo systemctl status mssql-server
   sudo systemctl stop mssql-server
   ```
 
À ce stade, les deux instances de SQL Server sont configurés pour s’exécuter avec les fichiers de base de données sur le stockage partagé. L’étape suivante consiste à configurer SQL Server pour STIMULATEUR. 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installer et configurer Pacemaker sur chaque nœud de cluster


2. Sur les deux nœuds du cluster, créez un fichier pour stocker le nom d’utilisateur et le mot de passe SQL Server du compte de connexion Pacemaker. La commande suivante a pour effet de créer et remplir ce fichier :

   ```bash
   sudo touch /var/opt/mssql/secrets/passwd
   echo '<loginName>' | sudo tee -a /var/opt/mssql/secrets/passwd
   echo '<loginPassword>' | sudo tee -a /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/secrets/passwd 
   sudo chmod 600 /var/opt/mssql/secrets/passwd    
   ```

3. Sur les deux nœuds de cluster, ouvrez les ports de pare-feu pour Pacemaker. Pour ouvrir ces ports avec `firewalld`, exécutez la commande suivante :

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > Si vous utilisez un autre pare-feu qui n’intègre pas de configuration à haute disponibilité, les ports suivants doivent être ouverts pour permettre à Pacemaker de communiquer avec les autres nœuds du cluster.
   >
   > * TCP : ports 2224, 3121, 21064
   > * UDP : port 5405

1. Installez les packages Pacemaker sur chaque nœud.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```

   

2. Définissez le mot de passe pour l’utilisateur par défaut qui est créé pendant l’installation des packages Pacemaker et Corosync. Utilisez le même mot de passe sur les deux nœuds. 

   ```bash
   sudo passwd hacluster
   ```

   

3. Activez et démarrez le service `pcsd` et Pacemaker. Cela permettra aux nœuds de rejoindre le cluster après le redémarrage. Exécutez la commande suivante sur les deux nœuds.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. Installez l’agent de ressources FCI pour SQL Server. Exécutez les commandes suivantes sur les deux nœuds. 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="create-the-cluster"></a>Créer le cluster 

1. Sur l’un des nœuds, créez le cluster.

   ```bash
   sudo pcs cluster auth <nodeName1 nodeName2 …> -u hacluster
   sudo pcs cluster setup --name <clusterName> <nodeName1 nodeName2 …>
   sudo pcs cluster start --all
   ```

   > RHEL HA possède clôtures agents pour VMWare et KVM. Délimitation doit être désactivée sur tous les autres hyperviseurs. La désactivation d’agents de délimitation n’est pas recommandée dans les environnements de production. À compter de la plage de temps, il n’existe aucun agent de délimitation pour les environnements Hyper-v ou le cloud. Si vous exécutez une de ces configurations, vous devez désactiver la délimitation. \**Cela n’est pas recommandée dans un système de production.**

   La commande suivante désactive les agents de délimitation.

   ```bash
   sudo pcs property set stonith-enabled=false
   sudo pcs property set start-failure-is-fatal=false
   ```

2. Configurer les ressources de cluster de SQL Server, système de fichiers et les ressources IP virtuels et de distribuer la configuration pour le cluster. Vous avez besoin des informations suivantes :

   - **Nom de la ressource SQL Server**: un nom pour la ressource SQL Server en cluster. 
   - **Variable de nom de la ressource IP**: un nom pour la ressource d’adresse IP virtuelle.
   - **Adresse IP**: l’adresse IP que les clients utiliseront pour se connecter à l’instance en cluster de SQL Server. 
   - **Nom de la ressource système de fichiers**: un nom pour la ressource du système de fichiers.
   - **APPAREIL**: chemin d’accès de partage de la NFS
   - **APPAREIL**: le chemin d’accès local, qu’il est monté sur le partage
   - **(fsType)**: type de partage de fichier (par exemple, nfs)

   Mettre à jour les valeurs dans le script suivant pour votre environnement. Exécutez sur un nœud pour configurer et démarrer le service en cluster.  

   ```bash
   sudo pcs cluster cib cfg 
   sudo pcs -f cfg resource create <sqlServerResourceName> ocf:mssql:fci
   sudo pcs -f cfg resource create <floatingIPResourceName> ocf:heartbeat:IPaddr2 ip=<ip Address>
   sudo pcs -f cfg resource create <fileShareResourceName> Filesystem device=<networkPath> directory=<localPath>         fstype=<fileShareType>
   sudo pcs -f cfg constraint colocation add <virtualIPResourceName> <sqlResourceName>
   sudo pcs -f cfg constraint colocation add <fileShareResourceName> <sqlResourceName> 
   sudo pcs cluster cib-push cfg
   ```

   Par exemple, le script suivant crée une ressource de cluster SQL Server nommée `mssqlha`et une ressource IP flottante avec l’adresse IP `10.0.0.99`. Il crée une ressource de système de fichiers et ajoute des contraintes pour toutes les ressources sont trouvent sur le même nœud en tant que ressource SQL. 

   ```bash
   sudo pcs cluster cib cfg
   sudo pcs -f cfg resource create mssqlha ocf:mssql:fci
   sudo pcs -f cfg resource create virtualip ocf:heartbeat:IPaddr2 ip=10.0.0.99
   sudo pcs -f cfg resource create fs Filesystem device="10.8.8.0:/mnt/nfs" directory="/var/opt/mssql/data" fstype="nfs"
   sudo pcs -f cfg constraint colocation add virtualip mssqlha
   sudo pcs -f cfg constraint colocation add fs mssqlha
   sudo pcs cluster cib-push cfg
   ```

   Une fois que la configuration est transmise, SQL Server démarre sur un nœud. 

3. Vérifiez que SQL Server est démarré. 

   ```bash
   sudo pcs status 
   ```

   Les exemples suivants illustrent les résultats lorsque STIMULATEUR a correctement démarré une instance en cluster de SQL Server. 

   ```
   fs     (ocf::heartbeat:Filesystem):    Started sqlfcivm1
   virtualip     (ocf::heartbeat:IPaddr2):      Started sqlfcivm1
   mssqlha  (ocf::mssql:fci): Started sqlfcivm1
   
   PCSD Status:
    slqfcivm1: Online
    sqlfcivm2: Online
   
   Daemon Status:
    corosync: active/disabled
    pacemaker: active/enabled
    pcsd: active/enabled
   ```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Cluster de toutes pièces](http://clusterlabs.org/doc/Cluster_from_Scratch.pdf) guide de STIMULATEUR

## <a name="next-steps"></a>Étapes suivantes

[Exploiter SQL Server sur le cluster de disque partagé Red Hat Enterprise Linux](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
