---
title: Principes fondamentaux de disponibilité de SQL Server pour les déploiements de Linux | Documents Microsoft
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
ms.openlocfilehash: f311d9c3116083120b97fa78486242939a438de9
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-availability-basics-for-linux-deployments"></a>Principes fondamentaux de disponibilité de SQL Server pour les déploiements de Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

En commençant par [!INCLUDE[sssql17-md](../includes/sssql17-md.md)], [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] est pris en charge sous Linux et Windows. Comme basés sur Windows [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] déploiements, [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] bases de données et les instances doivent être hautement disponible sous Linux. Cet article traite des aspects techniques de planification et de déploiement à haute disponibilité basés sur Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] bases de données et instances, ainsi que certaines des différences dans les installations basées sur Windows. Étant donné que [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] peuvent ne pas pour les professionnels de Linux et Linux peuvent être nouvelles pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] professionnels, l’article à des moments introduit des concepts qui peuvent être familiarisé à certaines et peu d’autres.

## <a name="includessnoversion-mdincludesssnoversion-mdmd-availability-options-for-linux-deployments"></a>[!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] options de disponibilité pour les déploiements de Linux
En dehors de la sauvegarde et de restauration, les mêmes fonctionnalités de disponibilité de trois sont disponibles sur Linux que pour les déploiements basés sur Windows :
-   Groupes de disponibilité AlwaysOn (groupes de disponibilité)
-   Instances de Cluster de basculement (fci) Always On
-   [Copie des journaux de transaction](sql-server-linux-use-log-shipping.md)

Sur Windows, les instances fci nécessite toujours un cluster de basculement Windows Server (WSFC) sous-jacent. Selon le scénario de déploiement, un groupe de disponibilité nécessite généralement un cluster WSFC sous-jacent, avec l’exception en cours de la nouvelle variante dans aucun [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. Un cluster WSFC n’existe pas dans Linux. Implémentation dans Linux est décrite dans la section [STIMULATEUR pour le cluster de basculement et de groupes de disponibilité AlwaysOn des instances sur Linux](#pacemaker-for-always-on-availability-groups-and-failover-cluster-instances-on-linux).

## <a name="a-quick-linux-primer"></a>Une présentation rapide
Alors que certaines installations Linux peuvent être installées avec une interface, la plupart ne sont pas, ce qui signifie que presque tous les éléments au niveau du système d’exploitation sont effectué via la ligne de commande. Le terme courantes pour cette ligne de commande dans le monde Linux est un *bash shell*.

Sous Linux, plusieurs commandes doivent être exécutées avec des privilèges élevés, comme plusieurs opérations doivent être effectuées dans Windows Server en tant qu’administrateur. Il existe deux méthodes principales pour s’exécuter avec des privilèges élevés :
1. Exécuter dans le contexte de l’utilisateur approprié. Pour passer à un autre utilisateur, utilisez la commande `su`. Si `su` est exécutée sur son propre sans un nom d’utilisateur, tant que vous connaissez le mot de passe, vous serez dans un interpréteur de commandes en tant que *racine*.
   
2. Les plus courants et sécurité consciente pour exécuter des opérations consiste à utiliser `sudo` avant l’exécution de quoi que ce soit. La plupart des exemples dans cet article utilisent `sudo`.

Certaines commandes courantes, chacun d'entre eux ont différents commutateurs et options que vous pouvant rechercher en ligne :
-   `cd` – Accédez au répertoire
-   `chmod` – modifier les autorisations d’un fichier ou un répertoire
-   `chown` : modifier la propriété d’un fichier ou un répertoire
-   `ls` : afficher le contenu d’un répertoire
-   `mkdir` : créez un dossier (répertoire) sur un lecteur
-   `mv` : déplacer un fichier d’un emplacement vers un autre
-   `ps` – afficher tous les processus de travail
-   `rm` : supprimer un fichier localement sur un serveur
-   `rmdir` : supprimer un dossier (répertoire)
-   `systemctl` – Démarrer, arrêter ou activer les services
-   Commandes de l’éditeur de texte. Sur Linux, il existe différentes options de l’éditeur de texte, telles que vi et emacs.

## <a name="common-tasks-for-availability-configurations-of-includessnoversion-mdincludesssnoversion-mdmd-on-linux"></a>Tâches courantes pour les configurations de la disponibilité de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux
Cette section présente les tâches qui sont communes à tous les basés sur Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] déploiements.

### <a name="ensure-that-files-can-be-copied"></a>Assurez-vous que les fichiers peuvent être copiés.
Copie des fichiers d’un serveur vers un autre est une tâche que vous soyez à l’aide de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux doit être en mesure d’effectuer. Cette tâche est très importante pour les configurations de groupe de disponibilité.

Des éléments tels que des problèmes d’autorisation peuvent exister sur Linux, ainsi que sur les installations basées sur Windows. Toutefois, ceux qui connaissent la copie de serveur à serveur sur Windows peut-être pas familiarisés avec la façon dont elle est effectuée sur Linux. Une méthode courante consiste à utiliser l’utilitaire de ligne de commande `scp`, ce qui signifie copie sécurisée. Les coulisses, `scp` utilise OpenSSH. SSH est l’acronyme shell sécurisé. Selon la distribution de Linux, OpenSSH lui-même ne peut-être pas être installé. Si elle n’est pas le cas, OpenSSH doit être installé en premier. Pour plus d’informations sur la configuration OpenSSH, consultez les informations sur les liens suivants pour chaque point de distribution :
-   [Red Hat Enterprise Linux (RHEL)](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-OpenSSH.html)
-   [SUSE Linux Enterprise Server (SLES)](https://en.opensuse.org/SDB:Configure_openSSH)
-   [Ubuntu](https://help.ubuntu.com/community/SSH/OpenSSH/Configuring)

Lorsque vous utilisez `scp`, vous devez fournir les informations d’identification du serveur si elle n’est pas la source ou la destination. Par exemple, à l’aide de

```bash
scp MyAGCert.cer username@servername:/folder/subfolder
```

copie le fichier MyAGCert.cer dans le dossier spécifié sur l’autre serveur. Notez que vous devez disposer des autorisations et éventuellement la propriété – du fichier à copier, par conséquent, `chown` serez peut-être amené à employer avant la copie. De même, à la réception, l’utilisateur a besoin d’accéder pour manipuler les fichiers. Par exemple, pour restaurer ce fichier de certificat, le `mssql` utilisateur doit pouvoir y accéder.

Samba, qui est la variante de Linux du bloc de message serveur (SMB), peut également servir à créer des partages accédés par les chemins d’accès UNC tel que `\\SERVERNAME\SHARE`. Pour plus d’informations sur la configuration Samba, consultez les informations sur les liens suivants pour chaque point de distribution :
-   [RHEL](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Confined_Services/chap-Managing_Confined_Services-Samba.html)
-   [SLES](https://www.suse.com/documentation/sles11/book_sle_admin/data/cha_samba.html)
-   [Ubuntu](https://help.ubuntu.com/community/Samba)

Les partages SMB basé sur Windows peuvent également être utilisées ; Partages SMB n’avez pas besoin d’être basés sur Linux, tant que la partie cliente de Samba est correctement configurée sur le serveur Linux hébergeant [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] et le partage a l’accès approprié. Pour ceux dans un environnement mixte, il s’agit d’une façon de tirer parti de l’infrastructure existante pour basés sur Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] déploiements.

Une chose importante est que la version de Samba déployé doit être conforme à SMB 3.0. Lorsque la prise en charge SMB a été ajoutée dans [!INCLUDE[sssql11-md](../includes/sssql11-md.md)], il requis tous les partages de la prise en charge SMB 3.0. Si vous utilisez Samba pour le partage et le pas à Windows Server, le partage de Samba doit utiliser Samba 4.0 ou version ultérieure et dans l’idéal, 4.3 ou version ultérieure, qui prend en charge SMB 3.1.1. Une bonne source d’informations sur le protocole SMB et Linux est [SMB3 dans Samba](http://events.linuxfoundation.org/sites/events/files/slides/smb3-in-samba.pr__0.pdf).

Enfin, à l’aide d’un partage NFS (système) de fichiers réseau est une option. À l’aide de NFS n’est pas une option sur des déploiements basés sur Windows [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]et peut être utilisé uniquement pour les déploiements basés sur Linux.

### <a name="configure-the-firewall"></a>Configurer le pare-feu
Comme pour Windows, les distributions Linux ont un pare-feu intégré. Si votre entreprise utilise un pare-feu externe pour les serveurs, la désactivation du pare-feu dans Linux peut être acceptable. Toutefois, quel que soit l’endroit où le pare-feu est activé, les ports doivent être ouverts. Le tableau suivant répertorie les ports communes nécessaires pour haut [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] déploiements sur Linux.

| Numéro de port | Type     |  Description                                                                                                                 |
|-------------|----------|-----------------------------------------------------------------------------------------------------------------------------|
| 111         | TCP/UDP  | NFS : `rpcbind/sunrpc`                                                                                                    |
| 135         | TCP      | Samba (si utilisé) – le mappeur de Point de terminaison                                                                                          |
| 137         | UDP      | Samba (si utilisé) – Service de noms NetBIOS                                                                                      |
| 138         | UDP      | Samba (si utilisé) – datagramme NetBIOS                                                                                          |
| 139         | TCP      | Samba (si utilisé) – NetBIOS Session                                                                                           |
| 445         | TCP      | Samba (si utilisé) – SMB sur TCP                                                                                              |
| 1433        | TCP      | [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] – par défaut le port ; Si vous le souhaitez, peut changer à `mssql-conf set network.tcpport <portnumber>`                       |
| 2049        | TCP, UDP | NFS (si utilisé)                                                                                                               |
| 2224        | TCP      | STIMULATEUR – utilisé par `pcsd`                                                                                                |
| 3121        | TCP      | STIMULATEUR – requis s’il existe des nœuds distants STIMULATEUR                                                                    |
| 3260        | TCP      | l’initiateur (si utilisé) – iSCSI peut être modifiée dans `/etc/iscsi/iscsid.config` (RHEL), mais doit correspondre au port de cible iSCSI |
| 5022        | TCP      | [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] -par défaut le port utilisé pour un point de terminaison du groupe de disponibilité ; peut être modifié lors de la création du point de terminaison                                |
| 5403        | TCP      | STIMULATEUR                                                                                                                   |
| 5404        | UDP      | STIMULATEUR – requis par Corosync si vous utilisez la multidiffusion UDP                                                                     |
| 5405        | UDP      | STIMULATEUR – requis par Corosync                                                                                            |
| 21064       | TCP      | STIMULATEUR – requis par les ressources à l’aide de DLM                                                                                 |
| Variable    | TCP      | Port du point de terminaison de groupe de disponibilité ; valeur par défaut est 5022                                                                                           |
| Variable    | TCP      | NFS – port pour `LOCKD_TCPPORT` (trouvé dans `/etc/sysconfig/nfs` sur RHEL)                                              |
| Variable    | UDP      | NFS – port pour `LOCKD_UDPPORT` (trouvé dans `/etc/sysconfig/nfs` sur RHEL)                                              |
| Variable    | TCP/UDP  | NFS – port pour `MOUNTD_PORT` (trouvé dans `/etc/sysconfig/nfs` sur RHEL)                                                |
| Variable    | TCP/UDP  | NFS – port pour `STATD_PORT` (trouvé dans `/etc/sysconfig/nfs` sur RHEL)                                                 |

Pour des ports supplémentaires qui peuvent être utilisées par Samba, consultez [l’utilisation du Port Samba](https://wiki.samba.org/index.php/Samba_Port_Usage).

À l’inverse, le nom du service sous Linux peut également être ajouté en tant qu’exception au lieu du port ; par exemple, `high-availability` pour STIMULATEUR. Reportez-vous à la distribution pour les noms si la direction que vous souhaitez poursuivre. Par exemple, la commande à ajouter dans STIMULATEUR est sur RHEL

```bash
sudo firewall-cmd --permanent --add-service=high-availability
```

**Documentation de pare-feu :**
-   [RHEL](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/s1-firewalls-haar)
-   [SLES](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html)

### <a name="install-includessnoversion-mdincludesssnoversion-mdmd-packages-for-availability"></a>Installer [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] packages pour la disponibilité
Sur un Windows [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] installation, certains composants sont installés même dans une installation du moteur de base, tandis que d’autres ne le sont pas. Sous Linux, uniquement le [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] moteur est installé dans le cadre du processus d’installation. Tout le reste est facultative. Pour haut [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] instances sous Linux, deux packages doivent être installés avec [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]: [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Agent (*mssql-server-agent*) et le package de haute disponibilité (HA) ( *MSSQL-server-ha*). Alors que [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Agent est techniquement facultatif, il est [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]du Planificateur de travaux et est requise par l’envoi de journaux, afin de l’installation est recommandée. Sur les installations basées sur Windows, [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] l’Agent n’est pas facultatif.

>[!NOTE]
>Pour les nouveaux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Agent est [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]du Planificateur de travaux intégrées. Il s’agit d’une méthode courante pour les administrateurs de planifier des éléments tels que les sauvegardes et d’autres [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] maintenance. Contrairement à une installation de Windows [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] où [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Agent est un service complètement différent, sur Linux, [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Agent s’exécute dans le contexte de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] lui-même.

Lorsque les groupes de disponibilité ou fci est configurés sur une configuration basée sur Windows, ils sont adaptée aux clusters. Détection de cluster signifie que [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] a des ressources spécifiques DLL un WSFC connaît (sqagtres.dll et sqsrvres.dll pour fci, hadrres.dll pour les groupes de disponibilité) et sont utilisées par le cluster WSFC pour vous assurer que le [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] est une fonctionnalité en cluster, en cours d’exécution, et fonctionne correctement. Étant donné que le clustering est externe non seulement à [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] mais Linux lui-même, Microsoft a dû l’équivalent d’une DLL de ressource pour les déploiements basés sur Linux le groupe de disponibilité et l’ICF de code. Il s’agit de la *mssql-server-ha* du package, également connu sous le [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] agent de ressource pour STIMULATEUR. Pour installer le *mssql-server-ha* de package, consultez [installer les packages de haute disponibilité et de l’Agent SQL Server](sql-server-linux-deploy-pacemaker-cluster.md#install-the-sql-server-ha-and-sql-server-agent-packages).

Les autres packages facultatifs pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux, [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] recherche en texte intégral (*FTP du serveur mssql*) et [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Integration Services (*mssql server est*), ne sont pas requis pour la haute disponibilité, soit pour une instance de cluster ou un groupe de disponibilité.

## <a name="pacemaker-for-always-on-availability-groups-and-failover-cluster-instances-on-linux"></a>STIMULATEUR pour les groupes de disponibilité AlwaysOn et les instances de cluster de basculement sur Linux
Précédente comme indiqué, le seul mécanisme clustering actuellement pris en charge par Microsoft pour les groupes de disponibilité et de fci est STIMULATEUR avec Corosync. Cette section décrit les informations de base pour comprendre la solution, ainsi que comment planifier et déployer pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] configurations.

### <a name="ha-add-onextension-basics"></a>Notions de base sur complémentaires/extension haute disponibilité
Toutes les distributions actuellement pris en charge sont fournis une haute disponibilité complémentaires-on/extension, qui est basé sur le Pacemaker clustering pile. Cette pile comprend deux composants principaux : STIMULATEUR et Corosync. Tous les composants de la pile sont :
-   STIMULATEUR – le cœur de composant, qui effectue des opérations telles que des coordonnées entre les ordinateurs en cluster de clustering.
-   Corosync – une structure et un ensemble d’API qui fournit des éléments tels que le quorum, la possibilité d’échec du redémarrage des processus et ainsi de suite.
-   libQB – fournit notamment la journalisation.
-   Agent de ressource – fonctionnalité spécifique fournie afin qu’une application peut s’intégrer à STIMULATEUR.
-   Agent – Scripts/fonctionnalité permettre d’isoler les nœuds et les traiter si elles rencontrent des problèmes de la clôture.
    
> [!NOTE]
> La pile de cluster est communément appelé STIMULATEUR dans le monde Linux.

Cette solution est quelque peu similaire à, mais de nombreuses manières différentes de déployer des configurations en cluster à l’aide de Windows. Dans Windows, la forme de la disponibilité de clustering, appelée un cluster de basculement Windows Server (WSFC), est créée dans le système d’exploitation et la fonctionnalité qui permet de créer un cluster WSFC, le clustering de basculement, est désactivée par défaut. Dans Windows, groupes de disponibilité fci s’appuient sur un cluster WSFC et partager une intégration étroite en raison de la ressource spécifique DLL fournie par [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Cette solution étroitement couplée est généralement possible, car il est tout à partir d’un fournisseur.

![](./media/sql-server-linux-ha-basics/image1.png)

Sur Linux, alors que chaque point de distribution pris en charge a STIMULATEUR disponible, chaque point de distribution permettre personnaliser et ont des versions et des implémentations légèrement différentes. Certaines des différences apparaîtront dans les instructions fournies dans cet article. La couche de gestion de clusters est open source, même s’il est fourni avec les distributions, il n’est pas étroitement intégré de la même façon un WSFC sous Windows. C’est pourquoi Microsoft propose *mssql-server-ha*, de sorte que [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] et la pile STIMULATEUR peut fermer pour fournir, mais pas exactement les mêmes fonctionnalités pour les instances fci et les groupes de disponibilité dans le cadre de Windows.

Pour obtenir une documentation complète sur STIMULATEUR, y compris une explication plus approfondie des tous les éléments avec des informations de référence complètes pour RHEL et SLES :
-   [RHEL](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/ch-overview-HAAR.html)
-   [SLES](https://www.suse.com/documentation/sle_ha/book_sleha/data/book_sleha.html)

Ubuntu ne dispose pas d’un guide pour la disponibilité.

Pour plus d’informations sur l’ensemble de la pile, consultez également officielle [page de documentation STIMULATEUR](http://clusterlabs.org/doc/) sur le site Clusterlabs.

### <a name="pacemaker-concepts-and-terminology"></a>STIMULATEUR concepts et terminologie
Cette section décrit les concepts et la terminologie relative à une implémentation STIMULATEUR courantes.

#### <a name="node"></a>Nœud
Un nœud est un serveur faisant partie du cluster. Un cluster STIMULATEUR prend nativement en charge jusqu'à 16 nœuds. Ce nombre peut être dépassé si Corosync n’est pas en cours d’exécution sur les nœuds supplémentaires, mais Corosync est requis pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Par conséquent, le nombre maximal de nœuds d’un cluster peut avoir pour toute [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-en fonction de la configuration est 16 ; il correspond à la limite de STIMULATEUR et n’a rien à voir avec les limitations maximales pour les groupes de disponibilité ou fci imposées par [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. 

#### <a name="resource"></a>Ressource
Un cluster WSFC et un cluster STIMULATEUR présentent le concept d’une ressource. Une ressource est une fonctionnalité spécifique qui s’exécute dans le contexte d’un cluster, comme un disque ou une adresse IP. Par exemple, sous STIMULATEUR ICF et le groupe de disponibilité des ressources peuvent être créés. Ce n’est pas différent aux opérations réalisées dans un cluster WSFC, où vous voyez un [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] une ressource pour une FCI ou groupe de disponibilité lors de la configuration d’un groupe de disponibilité, mais est pas identique au dû aux différences sous-jacentes dans la procédure [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] s’intègre avec STIMULATEUR.

STIMULATEUR dispose des ressources standard et le clone. Ressources de clonage sont ceux qui s’exécutent simultanément sur tous les nœuds. Un exemple serait une adresse IP qui s’exécute sur plusieurs nœuds à des fins d’équilibrage de charge. N’importe quelle ressource qui est créé pour les instances fci utilise une ressource standard, car un seul nœud peut héberger une instance de cluster à un moment donné.

Lorsqu’un groupe de disponibilité est créé, il nécessite un type spécial d’une ressource de clone appelé une ressource plusieurs état. Alors qu’un groupe de disponibilité a uniquement un réplica principal, le groupe de disponibilité est en cours d’exécution sur tous les nœuds qu’il est configuré pour fonctionner sur et permet à des éléments tels que l’accès en lecture seule. Comme il s’agit d’une utilisation « dynamique » du nœud, les ressources présentent le concept de deux états : maître et esclave. Pour plus d’informations, consultez [états multiples ressources : ressources qui ont plusieurs modes](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/s1-multistateresource-HAAR.html).

#### <a name="resource-groupssets"></a>/ Jeux de groupes de ressources
Similaires aux rôles dans un cluster WSFC, un cluster STIMULATEUR a le concept d’un groupe de ressources. Un groupe de ressources (appelé un jeu dans SLES) est une collection de ressources qui fonctionnent ensemble et peut basculer d’un nœud vers un autre comme une unité unique. Groupes de ressources ne peut pas contenir des ressources qui sont configurés en tant que maître/esclave ; Par conséquent, ils ne peuvent pas être utilisés pour les groupes de disponibilité. Si un groupe de ressources peut être utilisé pour les instances fci, il n’est pas généralement une configuration recommandée.

#### <a name="constraints"></a>Contraintes
WSFCs ont des paramètres différents pour les ressources, ainsi que des éléments tels que les dépendances, qui indiquent le cluster WSFC d’une relation parent/enfant entre deux ressources différentes. Une dépendance est simplement une règle indiquant le cluster WSFC à la ressource doit être en ligne tout d’abord.

Un cluster STIMULATEUR n’a pas le concept de dépendances, mais il existe des contraintes. Il existe trois types de contraintes : la colocalisation, l’emplacement et classement.
-   Une contrainte de colocalisation applique ou non de deux ressources doivent être en cours d’exécution sur le même nœud.
-   Une contrainte d’emplacement indique le cluster STIMULATEUR où une ressource (ou non) s’exécuter.
-   Une contrainte de classement indique le cluster STIMULATEUR l’ordre dans lequel les ressources doivent commencer.

> [!NOTE]
> Les contraintes de colocalisation ne sont pas requis pour les ressources dans un groupe de ressources, étant donné que tous ces sont considérés comme une unité unique.

#### <a name="quorum-fence-agents-and-stonith"></a>Quorum, les agents de délimitation et STONITH
Quorum sous STIMULATEUR est quelque peu similaire à un cluster WSFC dans le concept. Le but de mécanisme de quorum d’un cluster est pour vous assurer que le cluster reste en cours d’exécution. Un cluster WSFC et les modules complémentaires de haute disponibilité pour les distributions Linux ont le concept du vote, où chaque nœud compte vers le quorum. Vous souhaitez que la majorité des voix, dans le cas contraire, dans le pire des cas, le cluster va être arrêté.

Contrairement à un cluster WSFC, il n’existe aucune ressource de témoin pour travailler avec le quorum. Comme un cluster WSFC, l’objectif est de conserver le nombre d’électeurs impairs. Configuration de quorum a différentes considérations pour les groupes de disponibilité à la fci.

WSFCs surveiller l’état des nœuds participant et les gérer en cas de problème. Les versions ultérieures de WSFCs offrent des fonctionnalités telles que la mise en quarantaine d’un nœud qui est un dysfonctionnement ou non disponible (nœud n’est pas sur, la communication réseau est vers le bas, etc.). Sur le côté de Linux, ce type de fonctionnalité est fourni par un agent de délimitation. Le concept est parfois appelée délimitation. Toutefois, ces agents de délimitation sont généralement spécifiques au déploiement et souvent fournie par les fournisseurs de matériel et des éditeurs de logiciels, tels que ceux qui fournissent des hyperviseurs. Par exemple, VMware fournit un agent de délimitation qui peut être utilisé pour les machines virtuelles Linux virtualisés à l’aide de vSphere.

Quorum clôtures ties dans un autre concept appelé STONITH et dépanner l’autre nœud dans l’en-tête. STONITH est nécessaire d’avoir un cluster STIMULATEUR pris en charge sur toutes les distributions de Linux. Pour plus d’informations, consultez [clôtures dans un Cluster de disponibilité élevé Red Hat](https://access.redhat.com/solutions/15575) (RHEL).

#### <a name="corosyncconf"></a>corosync.conf
Le `corosync.conf` fichier contient la configuration du cluster. Il se trouve dans `/etc/corosync`. Au cours des opérations quotidiennes normales, ce fichier devront jamais être modifiée si le cluster est correctement configuré.

#### <a name="cluster-log-location"></a>Emplacement des journaux de cluster
Emplacement des journaux pour les clusters de STIMULATEUR diffère en fonction de la distribution.
-   RHEL et SLES- `/var/log/cluster/corosync.log`
-   Ubuntu : `/var/log/corosync/corosync.log`

Pour modifier l’emplacement du fichier journal par défaut, modifiez `corosync.conf`.

## <a name="plan-pacemaker-clusters-for-includessnoversion-mdincludesssnoversion-mdmd"></a>Planifier les clusters STIMULATEUR pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
Cette section décrit les points importants de la planification pour un cluster STIMULATEUR.

### <a name="virtualizing-linux-based-pacemaker-clusters-for-includessnoversion-mdincludesssnoversion-mdmd"></a>Pour les clusters virtualisation STIMULATEUR basés sur Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
À l’aide de machines virtuelles pour déployer basés sur Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] déploiements pour les groupes de disponibilité et de fci est couverte par les mêmes règles que pour leurs équivalents Windows. Il existe un ensemble de règles pour la prise en charge de base virtualisé [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] fournis par Microsoft dans les déploiements [956893 de Ko de prise en charge de Microsoft](https://support.microsoft.com/en-us/help/956893/support-policy-for-microsoft-sql-server-products-that-are-running-in-a-hardware-virtualization-environment). Hyperviseurs différents tels que Microsoft Hyper-V et VMware ESXi peuvent avoir des variances différentes en plus de cela, en raison de différences dans les plateformes eux-mêmes.

Lorsqu’il s’agit de groupes de disponibilité et les instances fci sous la virtualisation, assurez-vous qu’anti-d’affinité est définie pour les nœuds d’un cluster STIMULATEUR donné. Lorsque configuré pour la haute disponibilité dans une configuration de groupe de disponibilité ou instance de cluster, les machines virtuelles hébergeant [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] ne doit jamais s’exécuter sur le même hyperviseur hôte. Par exemple, si une instance FCI à deux nœuds est déployée, il doit être *au moins* trois hôtes hyperviseur qui est donc quelque part pour l’une des machines virtuelles hébergeant un nœud pour accéder en cas de défaillance de l’hôte, en particulier si l’utilisation des fonctionnalités telles que Live La migration ou vMotion.

Pour plus d’informations, consultez :
-   Documentation d’Hyper-V – [à l’aide du Clustering invité pour la haute disponibilité](https://technet.microsoft.com/library/dn440540(v=ws.11).aspx)
-   Livre blanc (écrit pour les déploiements basés sur Windows, mais la plupart des concepts toujours appliquer) – [planification hautement disponible, les déploiements cruciaux en SQL Server avec VMware vSphere](http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/solutions/vmware-vsphere-highly-available-mission-critical-sql-server-deployments.pdf)

>[!NOTE]
>RHEL avec un cluster STIMULATEUR avec STONITH n’est pas encore pris en charge par Hyper-V. Jusqu'à ce que qui est pris en charge, pour plus d’informations et les mises à jour, consultez [des stratégies de prise en charge pour les Clusters de disponibilité élevée RHEL](https://access.redhat.com/articles/29440#3physical_host_mixing).

### <a name="networking"></a>Réseau
Contrairement à un cluster WSFC, STIMULATEUR ne nécessite pas un nom dédié ou au moins une adresse IP dédiée pour le cluster STIMULATEUR lui-même. Groupes de disponibilité et les instances fci nécessite des adresses IP (consultez la documentation pour chacun d’eux pour plus d’informations), mais pas les noms, car il n’existe aucune ressource de nom réseau. SLES autorise la configuration d’une adresse IP pour des raisons d’administration, mais il n’est pas requis, comme indiqué dans [créer le cluster STIMULATEUR](sql-server-linux-deploy-pacemaker-cluster.md#create).

Comme un cluster WSFC, STIMULATEUR préférez redondance réseau, ce qui signifie que les cartes réseau distinctes (cartes réseau ou pNICs pour physique) ayant des adresses IP individuelles. En termes de la configuration du cluster, chaque adresse IP aurait ce que l'on appelle sa propre anneau. Toutefois, comme avec WSFCs aujourd'hui, de nombreuses implémentations sont virtualisées ou dans le cloud public dans lequel il existe réellement une seule virtualisés NIC (vNIC) présenté au serveur. Si tous les pNICs et les cartes réseau est connectés au même commutateur physique ou virtuel, il n’étant aucune redondance au niveau de la couche réseau, la configuration de plusieurs cartes réseau est un peu d’une illusion à l’ordinateur virtuel. Redondance réseau est généralement générée dans l’hyperviseur pour les déploiements virtualisés et est définitivement générée dans le cloud public.

Une différence avec plusieurs cartes réseau et STIMULATEUR par rapport à un cluster WSFC est que STIMULATEUR autorise plusieurs adresses IP sur le même sous-réseau, contrairement à un cluster WSFC. Pour plus d’informations sur plusieurs sous-réseaux et des clusters Linux, consultez l’article [configurer plusieurs sous-réseaux](sql-server-linux-configure-multiple-subnet.md).

### <a name="quorum-and-stonith"></a>Quorum et STONITH
Configuration de quorum et les spécifications sont liées aux déploiements de groupe de disponibilité ou spécifiques de l’ICF de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

STONITH est requis pour un cluster STIMULATEUR pris en charge. Utilisez la documentation de distribution pour configurer STONITH. Par exemple, à [basée sur le stockage de délimitation](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_storage_protect_fencing.html) pour SLES. Il existe également un agent STONITH pour VMware vCenter pour les solutions ESXI. Pour plus d’informations, consultez [Stonith l’Agent de plug-in pour l’isolement de VMWare VM VCenter SOAP (Unofficial)](https://github.com/olafrv/fence_vmware_soap).

> [!NOTE]
> Au moment de la rédaction de cet article, Hyper-V n’a pas d’une solution pour STONITH. Cela est vrai pour les déploiements locaux et affecte également des déploiements de stimulateur de basé sur Azure à l’aide de certaines distributions comme RHEL.

### <a name="interoperability"></a>Interopérabilité
Cette section décrit comment un cluster basé sur Linux peut interagir avec un cluster WSFC ou à d’autres distributions de Linux.

#### <a name="wsfc"></a>WSFC
Actuellement, il n’existe aucun moyen direct pour un cluster WSFC et un cluster STIMULATEUR fonctionner ensemble. Cela signifie qu’il n’existe aucun moyen pour créer un groupe de disponibilité ou l’ICF qui fonctionne sur un WSFC et un stimulateur. Toutefois, il existe deux solutions d’interopérabilité, qui sont conçues pour les groupes de disponibilité. Une instance FCI peut participer à une configuration inter-plateformes impérativement si elle participe en tant qu’instance dans un des deux scénarios suivants :
-   Un groupe de disponibilité avec un type de cluster None. Pour plus d’informations, consultez les fenêtres [documentation de groupes de disponibilité](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).
-   Un groupe de disponibilité distribué, qui est un type spécial de groupe de disponibilité qui permet à deux groupes de disponibilité différentes à être configuré en tant que leur propre groupe de disponibilité. Pour plus d’informations sur les groupes de disponibilité distribués, consultez la documentation [groupes de disponibilité distribués](../database-engine/availability-groups/windows/distributed-availability-groups.md).

#### <a name="other-linux-distributions"></a>Autres distributions Linux
Sur Linux, tous les nœuds d’un cluster STIMULATEUR doivent être sur la même distribution. Par exemple, cela signifie qu’un nœud RHEL ne peut pas être la partie d’un cluster STIMULATEUR qui possède un nœud SLES. La principale raison de cela a été indiquée précédemment : les distributions peuvent avoir différentes versions et les fonctionnalités, afin de choses ne peuvent pas fonctionner correctement. Mélange des distributions a le même article que mélange WSFCs et Linux : aucune ou distribué des groupes de disponibilité.

## <a name="next-steps"></a>Étapes suivantes
[Déployer un cluster STIMULATEUR pour SQL Server sur Linux](sql-server-linux-deploy-pacemaker-cluster.md)
