---
title: Instances de Cluster de basculement, SQL Server sur Linux | Documents Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 08/28/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 0275d0004bc02f1e32e7da3630c8a0bd15532d18
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="failover-cluster-instances---sql-server-on-linux"></a>Instances de Cluster de basculement, SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article explique les concepts liés aux instances de cluster de basculement SQL Server (ICF) sur Linux. 

Pour créer une instance de cluster SQL Server sur Linux, consultez [configurer SQL Server ICF sur Linux](sql-server-linux-shared-disk-cluster-configure.md)

## <a name="the-clustering-layer"></a>La couche de Clustering

* Dans RHEL, la couche de gestion de clusters est basée sur Red Hat Enterprise Linux (RHEL) [module complémentaire de haute disponibilité](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf). 

    > [!NOTE] 
    > Accès à un module complémentaire Red Hat à haute disponibilité et de la documentation nécessite un abonnement. 

* Dans SLES, la couche de gestion de clusters est basée sur SUSE Linux Enterprise [haute disponibilité Extension (HAÉ)](https://www.suse.com/products/highavailability).

    Pour plus d’informations sur la configuration du cluster, les options de l’agent de ressources, la gestion, meilleures pratiques et recommandations, consultez [SUSE Linux Enterprise haute disponibilité Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

Le complément RHEL à haute disponibilité et la HAÉ SUSE reposent sur [STIMULATEUR](http://clusterlabs.org/).

Comme le montre le diagramme suivant, le stockage est présenté à deux serveurs. Les composants clusters - Corosync et STIMULATEUR - coordonnent les communications et gestion des ressources. Un des serveurs a la connexion active pour les ressources de stockage et le serveur SQL Server. Lorsque STIMULATEUR détecte une défaillance les composants clusters gèrent le déplacement des ressources vers un autre nœud.  

![Red Hat Enterprise Linux 7 partagé de Cluster de disque SQL](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 


> [!NOTE]
> À ce stade, l’intégration de SQL Server avec STIMULATEUR sur Linux n’est pas comme exhaustivement comme WSFC sur Windows. Dans SQL, il n’est pas connaissance de la présence du cluster, tous les l’orchestration est en dehors d’et le service est contrôlé sous la forme d’une instance autonome par STIMULATEUR. En outre, nom de réseau virtuel est spécifique à WSFC, il n’existe aucun équivalent de la même dans STIMULATEUR. Il est prévu que @@servername et sys.servers pour renvoyer le nom du nœud, tandis que le cluster DMV sys.dm_os_cluster_nodes et sys.dm_os_cluster_properties ne seront aucun enregistrement. Pour utiliser une chaîne de connexion qui pointe vers un nom de serveur de chaîne et n’utilisez pas l’adresse IP, ils doivent inscrire dans leur serveur DNS l’adresse IP utilisée pour créer la ressource IP virtuelle (comme expliqué dans les sections suivantes) avec le nom de serveur choisi.

## <a name="number-of-instances-and-nodes"></a>Nombre d’Instances et de nœuds

Une des différences clés avec SQL Server sur Linux sont qu’il ne peut exister une seule installation de SQL Server par le serveur Linux. Cette installation est appelée une instance. Cela signifie que, contrairement à Windows Server qui prend en charge jusqu'à 25 fci par cluster de basculement Windows Server (WSFC), une instance FCI basés sur Linux ont qu’une seule instance. Cette instance d’un seul est également une instance par défaut ; Il n’existe pas de concept d’une instance nommée sur Linux. 

Un cluster STIMULATEUR ne peut avoir jusqu'à 16 nœuds lorsque Corosync est impliqué, pour une instance de cluster unique peut s’étendre sur jusqu'à 16 serveurs. Une instance de cluster mis en œuvre avec l’Édition Standard de SQL Server prend en charge jusqu'à deux nœuds d’un cluster même si le cluster STIMULATEUR a 16 nœuds maximum.

Dans une instance de cluster SQL Server, l’instance de SQL Server est active sur un nœud ou l’autre.

## <a name="ip-address-and-name"></a>Nom et adresse IP
Sur un cluster Linux STIMULATEUR, chaque instance de cluster SQL Server a besoin de sa propre adresse IP unique et un nom. Si la configuration ICF s’étend sur plusieurs sous-réseaux, une adresse IP sera requise par sous-réseau. Le nom unique et les adresses IP sont utilisés pour accéder à l’instance FCI afin que les applications et les utilisateurs finaux n’avez pas besoin de savoir quel serveur sous-jacent du cluster STIMULATEUR.

Le nom de l’instance de cluster dans le système DNS doit être le même que le nom de la ressource FCI qui est créé dans le cluster STIMULATEUR.
Le nom et l’adresse IP doivent être inscrit dans DNS.

## <a name="shared-storage"></a>Stockage partagé
Toutes les instances de fci, qu’ils soient sur Linux ou Windows Server, nécessitent une certaine forme de stockage partagé. Ce stockage est présenté à tous les serveurs qui peuvent héberger éventuellement l’instance FCI, mais un seul serveur peut utiliser le stockage de l’instance FCI à tout moment donné. Les options disponibles pour le stockage partagé sous Linux sont :

- iSCSI
- Network File System (NFS)
- Server Message Block (SMB) sous Windows Server, il existe des options légèrement différentes. Une des options non prises en charge pour les instances de fci basés sur Linux sont la possibilité d’utiliser un disque local sur le nœud pour TempDB, qui est l’espace de travail temporaire de SQL Server.

Dans une configuration qui s’étend sur plusieurs emplacements, ce qui est stocké à un centre de données doit être synchronisé avec les autres. En cas de basculement, l’instance FCI sera mise en ligne et le stockage est visible pour être identiques. Pour cela nécessitera une méthode externe pour la réplication de stockage, si elle est effectuée via le matériel de stockage sous-jacent ou un utilitaire basé sur le logiciel. 

>[!NOTE]
>Pour SQL Server 2017, les déploiements basés sur Linux à l’aide de disques présentées directement sur un serveur doivent être formatés avec XFS ou EXT4. Autres systèmes de fichiers ne sont pas actuellement pris en charge. Les modifications apparaîtront ici.

Le processus permettant de présenter le stockage partagé est identique pour les différentes méthodes prises en charge :

- Configurer le stockage partagé
- Monter le stockage en tant que dossier pour les serveurs qui servira de nœuds du cluster STIMULATEUR pour l’instance FCI
- Si nécessaire, déplacez les bases de données système SQL Server à un stockage partagé
- Test SQL Server fonctionne à partir de chaque serveur connecté à un stockage partagé

Une différence majeure avec SQL Server sur Linux est que vous pouvez configurer l’emplacement de fichier par défaut utilisateur données et les journaux, les bases de données système doivent toujours exister à `/var/opt/mssql/data`. Sur Windows Server, il est la possibilité de déplacer des bases de données système, y compris TempDB. Ce fait est lu dans le stockage partagé est configuré pour une instance de cluster.

Les chemins d’accès par défaut pour les bases de données non système peuvent être modifiés à l’aide de la `mssql-conf` utilitaire. Pour plus d’informations sur la façon de modifier les valeurs par défaut, [modifier l’emplacement de répertoire par défaut des données ou de journal](sql-server-linux-configure-mssql-conf.md#datadir). Vous pouvez également stocker les données de SQL Server et de transaction dans d’autres emplacements tant qu’ils aient une sécurité même si elle n’est pas un emplacement par défaut ; l’emplacement doit être indiqué.

Les rubriques suivantes expliquent comment configurer les types de stockage pris en charge pour une Linux basée sur l’instance de cluster de SQL Server :

- [Configurer l’instance de cluster de basculement - iSCSI - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [Configuration d’instance de cluster de basculement - NFS - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [Configuration d’instance de cluster de basculement - SMB - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-smb.md)
