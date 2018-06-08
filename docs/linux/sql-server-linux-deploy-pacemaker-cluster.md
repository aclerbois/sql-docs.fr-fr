---
title: Déployer un cluster STIMULATEUR pour SQL Server sur Linux | Documents Microsoft
description: Ce didacticiel montre comment déployer un cluster STIMULATEUR pour SQL Server sur Linux.
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 12/11/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: 239d4418c5e7d59a980d9028e2533dd9d7a2c566
ms.sourcegitcommit: fc3cd23685c6b9b6972d6a7bab2cc2fc5ebab5f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
# <a name="deploy-a-pacemaker-cluster-for-sql-server-on-linux"></a>Déployer un cluster STIMULATEUR pour SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce didacticiel décrit les tâches requises pour déployer un cluster Linux STIMULATEUR pour un [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] toujours sur le groupe de disponibilité (AG) ou de l’instance de cluster de basculement (FCI). Contrairement à Windows Server étroitement couplé /[!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] pile, la création du cluster STIMULATEUR comme configuration de groupe (AG) de disponibilité sur Linux peut être effectuée avant ou après l’installation de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. L’intégration et la configuration des ressources pour la partie STIMULATEUR du déploiement d’un groupe de disponibilité ou ICF est effectuée une fois que le cluster est configuré.
> [!IMPORTANT]
> Un groupe de disponibilité avec un type de cluster None est *pas* nécessitent un cluster STIMULATEUR, ni être géré par STIMULATEUR. 

> [!div class="checklist"]
> * Installer le module complémentaire de haute disponibilité et STIMULATEUR.
> * Préparer les nœuds pour STIMULATEUR (RHEL et Ubuntu uniquement).
> * Créez le cluster STIMULATEUR.
> * Installer les packages SQL Server à haute disponibilité et l’Agent SQL Server.
 
## <a name="prerequisite"></a>Condition préalable
[Installer SQL Server 2017](sql-server-linux-setup.md).

## <a name="install-the-high-availability-add-on"></a>Installer le module complémentaire de haute disponibilité
Utilisez la syntaxe suivante pour installer les packages qui composent le module complémentaire de haute disponibilité (HA) pour chaque point de distribution de Linux. 

**Red Hat Enterprise Linux (RHEL)**
1.  Inscrire le serveur à l’aide de la syntaxe suivante. Vous êtes invité à un nom d’utilisateur valide et un mot de passe.
    
    ```bash
    sudo subscription-manager register
    ```
    
2.  Répertorier les pools disponibles pour l’inscription.
    
    ```bash
    sudo subscription-manager list --available
    ```

3.  Exécutez la commande suivante pour associer une disponibilité élevée RHEL à l’abonnement
    
    ```bash
    sudo subscription-manager attach --pool=<PoolID>
    ```
    
    où *PoolId* est l’ID du pool pour l’abonnement de haute disponibilité à partir de l’étape précédente.
    
4.  Activer le référentiel pour être en mesure d’utiliser le module complémentaire de haute disponibilité.
    
    ```bash
    sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
    ```
    
5.  Installez STIMULATEUR.
    
    ```bash
    sudo yum install pacemaker pcs fence-agents-all resource-agents
    ```

**Ubuntu**

```bash
sudo apt-get install pacemaker pcs fence-agents resource-agents
```

**SUSE Linux Enterprise Server (SLES)**

Installer le modèle de haute disponibilité dans YaST ou de le faire dans le cadre de l’installation principale du serveur. L’installation est possible avec une norme ISO/DVD-ROM en tant que source ou par mise en route en ligne.
> [!NOTE]
> SLES, le module complémentaire à haute disponibilité est initialisé lorsque le cluster est créé.

## <a name="prepare-the-nodes-for-pacemaker-rhel-and-ubuntu-only"></a>Préparer les nœuds pour STIMULATEUR (RHEL et Ubuntu uniquement)
STIMULATEUR lui-même utilise un utilisateur créé sur la distribution nommée *hacluster*. L’utilisateur est créé lorsque le module complémentaire à haute disponibilité est installé sur RHEL et Ubuntu.
1. Sur chaque serveur qui servira à un nœud du cluster STIMULATEUR, créez le mot de passe pour un utilisateur à utiliser par le cluster. Le nom utilisé dans les exemples est *hacluster*, mais n’importe quel nom peut être utilisé. Le nom et le mot de passe doivent être le même sur tous les nœuds participant au cluster STIMULATEUR.
   
    ```bash
    sudo passwd hacluster
    ```
    
2. Sur chaque nœud qui fera partie du cluster STIMULATEUR, activer et démarrer le `pcsd` service avec les commandes suivantes (RHEL et Ubuntu) :

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   ```
   
   Puis exécutez
   
   ```bash
   sudo systemctl status pcsd
   ```
   
   Pour vous assurer que `pcsd` est démarré.
3. Activer le service STIMULATEUR sur chaque nœud du cluster STIMULATEUR de possible.
   
   ```bash
   sudo systemctl start pacemaker
   ```

   Sur Ubuntu, vous voyez une erreur :
   
   *STIMULATEUR de démarrage par défaut ne contient aucun runlevels, abandon.*
   
   Cette erreur est un problème connu. En dépit de l’erreur, l’activation du service STIMULATEUR a réussi, et ce bogue à un moment donné dans le futur.
   
4. Ensuite, créez et démarrez le cluster STIMULATEUR. Il existe une différence entre RHEL et Ubuntu à cette étape. Tandis que sur les deux distributions, installez `pcs` configure un fichier de configuration par défaut pour le cluster STIMULATEUR, sur RHEL, l’exécution de cette commande détruit toute configuration existante et crée un nouveau cluster.

<a id="create"></a>
## <a name="create-the-pacemaker-cluster"></a>Créer le cluster STIMULATEUR 
Cette section décrit comment créer et configurer le cluster pour chaque point de distribution de Linux.

**RHEL**

1. Autoriser les nœuds
   
   ```bash
   sudo pcs cluster auth <Node1 Node2 … NodeN> -u hacluster
   ```
   
   où *NodeX* est le nom du nœud.
2. Créer le cluster
   
   ```bash
   sudo pcs cluster setup --name <PMClusterName Nodelist> --start --all --enable
   ```
   
   où *PMClusterName* est le nom affecté au cluster STIMULATEUR et *Nodelist* est la liste des noms des nœuds séparés par un espace.

**Ubuntu**

Configuration d’Ubuntu est similaire à RHEL. Toutefois, il existe une différence majeure : installer les packages STIMULATEUR crée une configuration de base pour le cluster et active démarre `pcsd`. Si vous essayez de configurer le cluster STIMULATEUR en suivant les instructions de RHEL exactement, vous obtenez une erreur. Pour résoudre ce problème, procédez comme suit : 
1. Supprimez la configuration de STIMULATEUR par défaut de chaque nœud.
   
   ```bash
   sudo pcs cluster destroy
   ```
   
2. Suivez les étapes décrites dans la section RHEL pour créer le cluster STIMULATEUR.

**SLES**

Le processus de création d’un cluster STIMULATEUR est complètement différent sur SLES sur RHEL et Ubuntu. Les étapes suivantes de la création d’un cluster avec SLES document.
1. Démarrer le processus de configuration de cluster en exécutant 
   ```bash
   sudo ha-cluster-init
   ``` 
   
   sur l’un des nœuds. Vous pouvez être invité NTP n’est pas configuré et qu’aucun périphérique de surveillance n’est trouvé. Qui est tout indiquée pour la route les choses. Surveillance est lié à STONITH si vous utilisez délimitation intégrée de SLES qui est basé sur le stockage. NTP et surveillance peuvent être configurés plus tard.
   
2. Vous êtes invité à configurer Corosync. Vous êtes invité à entrer l’adresse réseau à lier, ainsi que l’adresse de multidiffusion et le port. L’adresse réseau est le sous-réseau que vous utilisez ; par exemple, 192.191.190.0. Vous pouvez accepter les valeurs par défaut à chaque invite de commandes ou les modifier si nécessaire.
   
3. Ensuite, vous êtes invité si vous souhaitez configurer SBD, qui est la délimitation basée sur disque. Cette configuration peut être effectuée ultérieurement si vous le souhaitez. Si le même jour ouvrable n’est pas configuré, contrairement sur RHEL et Ubuntu, `stonith-enabled` sera par défaut défini sur false.
   
4. Enfin, vous êtes invité si vous souhaitez configurer une adresse IP pour l’administration. Cette adresse IP est facultative, mais des fonctions similaire à l’adresse IP pour un cluster de basculement Windows Server (WSFC) dans le sens où il crée une adresse IP du cluster à utiliser pour se connecter à celui-ci via HA Web Konsole (HAWK). Cette configuration, est également facultative.
   
5. Vérifiez que le cluster est en cours d’exécution en émettant 
   ```bash
   sudo crm status
   ```
   
6. Modifier la *hacluster* un mot de passe 
   ```bash
   sudo passwd hacluster
   ```
   
7. Si vous avez configuré une adresse IP pour l’administration, vous pouvez le tester dans un navigateur, qui teste également la modification du mot de passe pour *hacluster*.
   ![](./media/sql-server-linux-deploy-pacemaker-cluster/image2.png)
   
8. Sur un autre serveur SLES destiné à être un nœud du cluster, exécutez 
   ```bash
   sudo ha-cluster-join
   ```
   
9. Lorsque vous y êtes invité, entrez le nom ou l’adresse IP du serveur qui a été configuré comme le premier nœud du cluster dans les étapes précédentes. Le serveur est ajouté en tant que nœud au cluster existant.
   
10. Vérifiez que le nœud a été ajouté par émission 
   ```bash
   sudo crm status
   ```
   
11. Modifier la *hacluster* un mot de passe 
   ```bash
   sudo passwd hacluster
   ```
   
12. Répétez les étapes 8 à 11 pour tous les autres serveurs à ajouter au cluster.

## <a name="install-the-sql-server-ha-and-sql-server-agent-packages"></a>Installer les packages SQL Server à haute disponibilité et l’Agent SQL Server
Utilisez les commandes suivantes pour installer le package SQL Server à haute disponibilité et [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Agent, s’ils ne sont pas déjà installés. Installation du package de haute disponibilité après l’installation [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] nécessite un redémarrage de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] pour pouvoir être utilisé. Ces instructions supposent que les référentiels pour les packages de Microsoft ont déjà été configurés, étant donné que [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] doit être installé à ce stade.
> [!NOTE]
> - Si vous ne souhaitez pas utiliser [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Agent pour l’envoi de journaux ou de toute autre utilisation, il n’a pas à installer, par conséquent, le package *mssql-server-agent* peut être ignorée.
> - Les autres packages facultatifs pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux, [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] recherche en texte intégral (*FTP du serveur mssql*) et [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Integration Services (*mssql server est*), ne sont pas requis pour la haute disponibilité, soit pour une instance de cluster ou un groupe de disponibilité.

**RHEL**

```bash
sudo yum install mssql-server-ha mssql-server-agent
sudo systemctl restart mssql-server
```

**Ubuntu**

```bash
sudo apt-get install mssql-server-ha mssql-server-agent
sudo systemctl restart mssql-server
```

**SLES**

```bash
sudo zypper install mssql-server-ha mssql-server-agent
sudo systemctl restart mssql-server
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris comment déployer un cluster STIMULATEUR pour SQL Server sur Linux. Vous avez appris à : 
> [!div class="checklist"]
> * Installer le module complémentaire de haute disponibilité et STIMULATEUR.
> * Préparer les nœuds pour STIMULATEUR (RHEL et Ubuntu uniquement).
> * Créez le cluster STIMULATEUR.
> * Installer les packages SQL Server à haute disponibilité et l’Agent SQL Server.

Pour créer et configurer un groupe de disponibilité pour SQL Server sur Linux, consultez :

> [!div class="nextstepaction"]
> [Créer et configurer un groupe de disponibilité pour SQL Server sur Linux](sql-server-linux-create-availability-group.md).

