---
title: Configurer des clusters de disques partagés SLES pour SQL Server | Documents Microsoft
description: Implémenter la haute disponibilité en configurant des clusters de disques partagés SUSE Linux Enterprise Server (SLES) pour SQL Server.
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
ms.assetid: e5ad1bdd-c054-4999-a5aa-00e74770b481
ms.openlocfilehash: 5c394dbfc613803a8f7eb0cf906b4b7733777919
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-sles-shared-disk-cluster-for-sql-server"></a>Configurer des clusters de disques partagés SLES pour SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce guide fournit des instructions pour créer un cluster de disque partagé de deux nœuds pour SQL Server sur SUSE Linux Enterprise Server (SLES). La couche de gestion de clusters est basée sur SUSE [haute disponibilité Extension (HAÉ)](https://www.suse.com/products/highavailability) construit sur [STIMULATEUR](http://clusterlabs.org/). 

Pour plus d’informations sur la configuration du cluster, les options de l’agent de ressources, la gestion, meilleures pratiques et recommandations, consultez [SUSE Linux Enterprise haute disponibilité Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

## <a name="prerequisites"></a>Configuration requise

Pour terminer le scénario de bout en bout suivant, vous avez besoin de deux ordinateurs pour déployer le cluster à deux nœuds et un autre serveur pour configurer le partage NFS. Les étapes ci-dessous décrivent la configuration de ces serveurs.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Installer et configurer le système d’exploitation sur chaque nœud de cluster

La première étape consiste à configurer le système d’exploitation sur les nœuds de cluster. Pour cette procédure pas à pas, utilisez SLES avec un abonnement valide pour le module complémentaire à haute disponibilité.

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>Installer et configurer SQL Server sur chaque nœud de cluster

1. Installer et configurer SQL Server sur les deux nœuds. Pour obtenir des instructions détaillées, consultez [installer SQL Server sur Linux](sql-server-linux-setup.md).
2. Désigner un nœud en tant que principal et l’autre comme secondaire, à des fins de configuration. Utiliser ces termes pour les éléments suivants ce guide. 
3. Sur le nœud secondaire, arrêter et désactiver SQL Server. L’exemple suivant arrête et désactive les SQL Server :

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE]
    > Au moment de l’installation, une clé principale du serveur est généré pour l’instance de SQL Server et placées à `/var/opt/mssql/secrets/machine-key`. Sur Linux, SQL Server s’exécute toujours comme un compte local nommé mssql. S’agissant d’un compte local, son identité n’est pas partagée entre les nœuds. Par conséquent, vous devez copier la clé de chiffrement à partir du nœud principal à chaque nœud secondaire pour chaque compte mssql local puisse accéder pour déchiffrer la clé principale du serveur.
4. Sur le nœud principal, créez une connexion SQL server pour Pacemaker et accorder l’autorisation de connexion pour exécuter `sp_server_diagnostics`. STIMULATEUR utilise ce compte pour vérifier le nœud qui exécute SQL Server.

    ```bash
    sudo systemctl start mssql-server
    ```
    Se connecter à la base de données master de SQL Server avec le compte 'sa' et exécutez la commande suivante :

    ```tsql
    USE [master]
    GO
    CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
    GRANT VIEW SERVER STATE TO <loginName>
    ```
5. Sur le nœud principal, arrêtez et désactivez SQL Server.
6. Suivez les instructions [dans la documentation SUSE](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) pour configurer et mettre à jour le fichier hosts de chaque nœud du cluster. Le fichier « hosts » doit inclure l’adresse IP et le nom de chaque nœud de cluster.

    Pour vérifier l’adresse IP du nœud actuel s’exécuter :

    ```bash
    sudo ip addr show
    ```

    Définir le nom d’ordinateur sur chaque nœud. Donnez à chaque nœud un nom unique de 15 caractères au maximum. Définir le nom d’ordinateur en l’ajoutant à `/etc/hostname` à l’aide de [yast](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) ou [manuellement](https://www.suse.com/documentation/sled11/book_sle_admin/data/sec_basicnet_manconf.html).

    L’exemple suivant montre `/etc/hosts` avec les ajouts de deux nœuds nommés `SLES1` et `SLES2`.

    ```
    127.0.0.1   localhost
    10.128.18.128 SLES1
    10.128.16.77 SLES2
    ```

    > [!NOTE]
    > Tous les nœuds de cluster doivent être en mesure des uns aux autres via une connexion SSH. Certains outils, comme hb_report ou crm_report (pour le dépannage) et l’Explorateur d’historique de Hawk, exigent un accès SSH sans mot de passe entre les nœuds, sinon ils ne peuvent collecter que les données du nœud actif. En cas d’utilisation d’un port SSH non standard, utilisez l’option -X ([consultez la page de manuel](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html)). Par exemple, si votre port SSH est 3479, appelez une crm_report avec :
    >
    >```bash
    >crm_report -X "-p 3479" [...]
    >```
    >Pour plus d’informations, consultez [le Guide d’Administration]. (https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc)

Dans la section suivante vous configurerez un stockage partagé et déplacer vos fichiers de base de données pour que le stockage.  

## <a name="configure-shared-storage-and-move-database-files"></a>Configurer le stockage partagé et déplacer les fichiers de base de données

Il existe un large éventail de solutions pour fournir un stockage partagé. Cette procédure pas à pas illustre la configuration du stockage partagé avec NFS. Nous vous recommandons de suivre les meilleures pratiques et d’utiliser Kerberos pour sécuriser NFS : 

- [Partage des systèmes de fichiers avec NFS](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.nfs)

Si vous ne suivez pas cette recommandation, toute personne qui peut accéder à votre réseau et usurper l’identité de l’adresse IP d’un nœud SQL seront en mesure d’accéder à vos fichiers de données. Comme toujours, assurez-vous que votre système de modèle de menaces avant de l’utiliser en production. 

Une autre option de stockage consiste à utiliser le partage de fichiers SMB :

- [Section Samba de documentation de SUSE](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.samba)

### <a name="configure-an-nfs-server"></a>Configurer un serveur NFS

Pour configurer un serveur NFS, voir les étapes suivantes dans la documentation SUSE : [configuration du serveur NFS](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-server).

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>Configurez tous les nœuds de cluster pour vous connecter au stockage partagées NFS

Avant de configurer le client NFS pour monter le chemin d’accès des fichiers de base de données SQL Server pour pointer vers l’emplacement de stockage partagé, veillez à qu'enregistrer les fichiers de base de données vers un emplacement temporaire pour être en mesure de les copier ultérieurement sur le partage :

1. **Sur le nœud principal uniquement**, enregistrer les fichiers de base de données dans un emplacement temporaire. Le script suivant crée un répertoire temporaire, copie les fichiers de base de données vers le nouveau répertoire et supprime les anciens fichiers de base de données. Lorsque SQL Server s’exécute en tant qu’utilisateur local mssql, vous devez vous assurer qu’après le transfert de données pour le partage monté, utilisateur local a accès en lecture-écriture au partage. 

    ```bash
    su mssql
    mkdir /var/opt/mssql/tmp
    cp /var/opt/mssql/data/* /var/opt/mssql/tmp
    rm /var/opt/mssql/data/*
    exit
    ```

    Configurer le client NFS sur tous les nœuds de cluster :

    - [Configuration des Clients](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-clients)

    > [!NOTE]
    > Il est recommandé de suivre les meilleures pratiques et les recommandations en matière de stockage de hautement disponible NFS de SUSE : [hautement disponible stockage NFS avec DRBD et STIMULATEUR](https://www.suse.com/documentation/sle-ha-12/book_sleha_techguides/data/art_ha_quick_nfs.html).

2. Vérifiez que SQL Server démarre correctement avec le nouveau chemin de fichier. Cela sur chaque nœud. À ce stade qu’un seul nœud doit exécuter SQL Server à la fois. Ils ne peuvent pas les deux s’exécuter en même temps, car ils essaieront à la fois d’accéder aux fichiers de données simultanément (pour éviter le démarrage accidentellement de SQL Server sur les deux nœuds, une ressource de cluster du système de fichiers pour vous assurer que le partage n’est pas monté deux fois par différents nœuds). Les commandes suivantes démarrer SQL Server, vérifient l’état, puis arrêtez SQL Server.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    sudo systemctl stop mssql-server
    ```

À ce stade, les deux instances de SQL Server sont configurés pour s’exécuter avec les fichiers de base de données sur le stockage partagé. L’étape suivante consiste à configurer SQL Server pour STIMULATEUR. 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installer et configurer Pacemaker sur chaque nœud de cluster

1. **Sur les deux nœuds du cluster, créez un fichier pour stocker le nom d’utilisateur et le mot de passe SQL Server du compte de connexion Pacemaker**. La commande suivante a pour effet de créer et remplir ce fichier :

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd
    ```
2. **Tous les nœuds de cluster doivent pouvoir accéder aux uns et aux autres via SSH**. Certains outils, comme hb_report ou crm_report (pour le dépannage) et l’Explorateur d’historique de Hawk, exigent un accès SSH sans mot de passe entre les nœuds, sinon ils ne peuvent collecter que les données du nœud actif. Si vous utilisez un port SSH non standard, utilisez l’option -X (consultez le manuel). Par exemple, si votre port SSH est le 3479, appelez un hb_report avec :

    ```bash
    crm_report -X "-p 3479" [...]
    ```

    Pour plus d’informations, consultez la [configuration système requise et les recommandations dans la documentation de SUSE](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html).

3. **Installez l’extension High Availability**. Pour installer cette extension, suivez les étapes décrites dans la rubrique SUSE suivante :
    
    [Installation and Setup Quick Start](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html) (Guide rapide d’installation et de configuration)

4. **Installez l’agent de ressources FCI pour SQL Server**. Exécutez les commandes suivantes sur les deux nœuds :

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
    sudo zypper --gpg-auto-import-keys refresh
    sudo zypper install mssql-server-ha
    ```

5. **Configurez automatiquement le premier nœud**. L’étape suivante consiste à configurer un cluster à un nœud en cours d’exécution en configurant le premier nœud, SLES1. Suivez les instructions de la rubrique SUSE, [Setting Up the First Node](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node) (Configuration du premier nœud).

    À la fin de la procédure, vérifiez l’état du cluster à l’aide de `crm status` :
    ```bash
    crm status
    ```

    Le résultat doit indiquer qu’un seul nœud est configuré : SLES1.

6. **Ajoutez des nœuds à un cluster existant**. Joignez ensuite le nœud SLES2 au cluster. Suivez les instructions de la rubrique SUSE, [Adding the Second Node](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.2nd-node) (Ajout du deuxième nœud).
    
    À la fin de la procédure, vérifiez l’état du cluster à l’aide de **crm status**. Si vous avez correctement ajouté le deuxième nœud, voici ce que vous obtenez en sortie :
        
    ```
    2 nodes configured
    1 resource configured
    Online: [ SLES1 SLES2 ]
    Full list of resources:
    admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
    ```

    > [!NOTE]
    > **admin_addr** est la ressource de cluster IP virtuelle qui est configurée pendant l’installation initiale du cluster à un nœud.

7.  **Procédures de suppression**. Si vous devez supprimer un nœud du cluster, utilisez le script d’amorçage **ha-cluster-remove**. Pour plus d’informations, consultez [Overview of the Bootstrap Scripts](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.bootstrap) (Vue d’ensemble des scripts d’amorçage).  

## <a name="configure-the-cluster-resources-for-sql-server"></a>Configurer les ressources de cluster pour SQL Server

Les étapes suivantes expliquent comment configurer la ressource de cluster pour SQL Server. Il existe deux paramètres que vous souhaitez personnaliser.

- **Nom de la ressource SQL Server**: un nom pour la ressource SQL Server en cluster. 
- **Valeur de délai d’attente**: la valeur de délai d’attente est la durée pendant laquelle le cluster attend pendant une ressource est mise en ligne. Pour SQL Server, cela indique l’heure que vous prévoyez de SQL Server à suivre pour mettre le `master` en ligne de base de données. 

Mettre à jour les valeurs dans le script suivant pour votre environnement. Exécutez sur un nœud pour configurer et démarrer le service en cluster.

```bash
sudo crm configure
primitive <sqlServerResourceName> ocf:mssql:fci op start timeout=<timeout_in_seconds>
colocation <constraintName> inf: <virtualIPResourceName> <sqlServerResourceName>
show
commit
exit
```

Par exemple, le script suivant crée une ressource de cluster SQL Server nommée mssqlha. 

```bash
sudo crm configure
primitive mssqlha ocf:mssql:fci op start timeout=60s
colocation admin_addr_mssqlha inf: admin_addr mssqlha
show
commit
exit
```

Une fois validée, la configuration de SQL Server démarre sur le même nœud en tant que la ressource IP virtuelle. 

Pour plus d’informations, consultez [configuration et la gestion des ressources de Cluster (ligne de commande)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config). 

### <a name="verify-that-sql-server-is-started"></a>Vérifiez que SQL Server est démarré. 

Pour vérifier que SQL Server est démarré, exécutez le **crm état** commande :

```bash
crm status
```

Les exemples suivants présentent les résultats lorsque STIMULATEUR a démarré avec succès en tant que ressource en cluster. 
```
2 nodes configured
2 resources configured

Online: [ SLES1 SLES2 ]

Full list of resources:

 admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
 mssqlha        (ocf::mssql:fci):       Started SLES1
```

## <a name="managing-cluster-resources"></a>La gestion des ressources de cluster

Pour gérer vos ressources de cluster, consultez la rubrique suivante de SUSE : [la gestion des ressources de Cluster](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm )

### <a name="manual-failover"></a>basculement manuel

Bien que les ressources sont configurées pour effectuer un basculement automatique (ou migrer) vers d’autres nœuds du cluster en cas de défaillance matérielle ou logicielle, vous pouvez également déplacer une ressource vers un autre nœud du cluster à l’aide de l’interface utilisateur graphique de STIMULATEUR ou de la ligne de commande. 

Utilisez la commande de migration pour cette tâche. Par exemple, pour migrer la ressource SQL pour un nom de nœud de cluster SLES2 exécuter : 

```bash
crm resource
migrate mssqlha SLES2
```

## <a name="additional-resources"></a>Ressources supplémentaires

[Extension de haute disponibilité SUSE Linux Enterprise - Guide d’Administration](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html) 
