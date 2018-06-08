---
title: Configurer le Cluster RHEL pour le groupe de disponibilité de SQL Server | Documents Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 06/14/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: b7102919-878b-4c08-a8c3-8500b7b42397
ms.openlocfilehash: 784f98acb1e8223e14d3f1e1a74e73cfdc3561bc
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-rhel-cluster-for-sql-server-availability-group"></a>Configurer le Cluster RHEL pour le groupe de disponibilité de SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce document explique comment créer un cluster de groupe de disponibilité de trois nœuds pour SQL Server sur Red Hat Enterprise Linux. Pour la haute disponibilité, un groupe de disponibilité sur Linux nécessite trois nœuds, voir [haute disponibilité et protection des données pour les configurations de groupe de disponibilité](sql-server-linux-availability-group-ha.md). La couche de clustering basée sur Red Hat Enterprise Linux (RHEL) [module complémentaire de haute disponibilité](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) construit sur [STIMULATEUR](http://clusterlabs.org/). 

> [!NOTE] 
> Accès à la documentation complète de Red Hat nécessite un abonnement valid. 

Pour plus d’informations sur la configuration du cluster, options d’agents de ressources et la gestion, visitez [documentation de référence RHEL](http://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

> [!NOTE] 
> SQL Server n’est pas aussi étroitement intégré à STIMULATEUR sur Linux qu’avec le clustering de basculement Windows Server. Une instance de SQL Server n’est pas prenant en charge du cluster. STIMULATEUR fournit l’orchestration de ressource de cluster. En outre, le nom de réseau virtuel est spécifique au clustering de basculement Windows Server - il n’existe aucun équivalent dans STIMULATEUR. Disponibilité groupe vues de gestion dynamique (DMV) qui interrogent des informations de cluster retournent les lignes vides sur les clusters de STIMULATEUR. Pour créer un écouteur pour la reconnexion après un basculement transparent, vous devez enregistrer manuellement le nom de l’écouteur dans DNS avec l’adresse IP utilisée pour créer la ressource IP virtuelle. 

Les sections suivantes les différentes étapes pour configurer un cluster STIMULATEUR et ajouter un groupe de disponibilité en tant que ressource dans le cluster pour la haute disponibilité.

## <a name="roadmap"></a>Feuille de route

Les étapes pour créer un groupe de disponibilité sur des serveurs Linux pour une haute disponibilité sont différentes des étapes sur un cluster de basculement Windows Server. La liste suivante décrit les étapes principales : 

1. [Configurer SQL Server sur les nœuds de cluster](sql-server-linux-setup.md).

2. [Créer le groupe de disponibilité](sql-server-linux-availability-group-configure-ha.md). 

3. Configurer un gestionnaire de ressources de cluster, comme STIMULATEUR. Ces instructions sont fournies dans ce document.
   
   La façon de configurer un gestionnaire de ressources du cluster dépend de la distribution Linux spécifique. 

   >[!IMPORTANT]
   >Les environnements de production requièrent un agent de délimitation, tels que STONITH pour la haute disponibilité. Les démonstrations dans cette documentation n’utilisent pas les agents de délimitation. Les démonstrations sont pour le test et de validation uniquement. 
   
   >Un cluster Linux utilise délimitation pour retourner le cluster à un état connu. La façon de configurer la délimitation dépend de la distribution et l’environnement. Actuellement, la délimitation n’est pas disponible dans certains environnements de cloud. Pour plus d’informations, consultez [des stratégies de prise en charge de RHEL haute disponibilité Clusters - plateformes de virtualisation](https://access.redhat.com/articles/29440).

5. [Ajouter le groupe de disponibilité en tant que ressource dans le cluster](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).  

## <a name="configure-high-availability-for-rhel"></a>Configurer la haute disponibilité pour RHEL

Pour configurer la haute disponibilité pour RHEL, activer l’abonnement de haute disponibilité, puis configurez STIMULATEUR.

### <a name="enable-the-high-availability-subscription-for-rhel"></a>Activer l’abonnement de haute disponibilité pour RHEL

Chaque nœud du cluster doit avoir un abonnement approprié pour RHEL et l’ajout de la haute disponibilité. Passez en revue la configuration requise à [comment installer des packages de cluster de haute disponibilité dans Red Hat Enterprise Linux](http://access.redhat.com/solutions/45930). Suivez ces étapes pour configurer l’abonnement et les dépôts :

1. Enregistrer le système.

   ```bash
   sudo subscription-manager register
   ```

   Fournissez votre nom d’utilisateur et un mot de passe.   

1. Répertorier les pools disponibles pour l’inscription.

   ```bash
   sudo subscription-manager list --available
   ```

   Dans la liste des pools disponibles, notez l’ID du pool pour l’abonnement de haute disponibilité.

1. Mettre à jour le script suivant. Remplacez `<pool id>` avec l’ID du pool pour la haute disponibilité de l’étape précédente. Exécutez le script pour attacher l’abonnement.

   ```bash
   sudo subscription-manager attach --pool=<pool id>
   ```

1. Activer le référentiel.

   ```bash
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   ```

Pour plus d’informations, consultez [clusters STIMULATEUR – Open Source pour la haute disponibilité](http://www.opensourcerers.org/pacemaker-the-open-source-high-availability-cluster/). 

Après avoir configuré l’abonnement, procédez comme suit pour configurer STIMULATEUR :

### <a name="configure-pacemaker"></a>Configurer STIMULATEUR

Après avoir inscrit l’abonnement, procédez comme suit pour configurer STIMULATEUR :
   
[!INCLUDE [RHEL-Configure-Pacemaker](../includes/ss-linux-cluster-pacemaker-configure-rhel.md)]

Après avoir configuré STIMULATEUR, utilisez `pcs` pour interagir avec le cluster. Exécuter toutes les commandes sur un nœud du cluster. 

## <a name="configure-fencing-stonith"></a>Configurer la délimitation (STONITH)

Fournisseurs de cluster STIMULATEUR nécessitent STONITH doit être activée et un appareil de délimitation configuré pour une installation de clusters prises en charge. STONITH est l’acronyme « dépanner l’autre nœud dans l’en-tête. » Lorsque le Gestionnaire de ressources de cluster ne peut pas déterminer l’état d’un nœud ou d’une ressource sur un nœud, délimitation met à nouveau le cluster à un état connu.

Délimitation de niveau de ressources permet de s’assurer qu’il n’existe aucune altération des données en cas de panne en configurant une ressource. Par exemple, vous pouvez utiliser la délimitation de niveau de ressources pour marquer le disque sur un nœud comme obsolètes lorsque la liaison de communication tombe en panne. 

Délimitation de niveau de nœud garantit qu’un nœud ne s’exécute pas toutes les ressources. Pour cela, vous devez réinitialiser le nœud. STIMULATEUR prend en charge une grande variété d’appareils de clôture. Un onduleur approvisionnement ou gestion des cartes d’interface pour les serveurs sont des exemples.

Pour plus d’informations sur les STONITH et délimitation, consultez les articles suivants :

* [STIMULATEUR des Clusters à partir de zéro](http://clusterlabs.org/doc/en-US/Pacemaker/1.1-plugin/html/Clusters_from_Scratch/ch05.html)
* [Délimitation et STONITH](http://clusterlabs.org/doc/crm_fencing.html)
* [Module complémentaire de haute disponibilité de Red Hat avec STIMULATEUR : isolement](http://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-fencing-HAAR.html)

Le niveau de nœud clôtures configuration dépend largement de votre environnement, désactivez-le pour ce didacticiel (il peut être configuré plus tard). Le script suivant désactive la délimitation de niveau de nœud :

```bash
sudo pcs property set stonith-enabled=false
```
  
>[!IMPORTANT]
>La désactivation de STONITH est uniquement pour des tests. Si vous envisagez d’utiliser STIMULATEUR dans un environnement de production, vous devez planifier une implémentation STONITH en fonction de votre environnement et qu’il soit activé. RHEL ne fournit pas de délimitation agents Hyper-V ou des environnements de cloud (y compris Azure). Par conséquent, le fournisseur de cluster n’offre pas de prise en charge des clusters de production en cours d’exécution dans ces environnements. Nous travaillons sur une solution pour cet intervalle qui est disponible dans les versions futures.

## <a name="set-cluster-property-cluster-recheck-interval"></a>Définir la propriété cluster cluster-revérification-intervalle

`cluster-recheck-interval` Indique l’intervalle d’interrogation à laquelle le cluster vérifie les modifications dans les paramètres des ressources, des contraintes ou des autres options de cluster. Si un réplica tombe en panne, le cluster tente de redémarrer le réplica à un intervalle est lié par le `failure-timeout` valeur et le `cluster-recheck-interval` valeur. Par exemple, si `failure-timeout` est défini à 60 secondes et `cluster-recheck-interval` est définie à 120 secondes, le redémarrage sera tenté à un intervalle est supérieur à 60 secondes, mais moins de 120 secondes. Nous vous recommandons de définir d’expiration de l’échec à 60 s et de cluster-revérification-intervalle à une valeur qui est supérieure à 60 secondes. Paramètre d’intervalle de revérification de cluster sur une petite valeur n’est pas recommandé.

Pour mettre à jour la valeur de propriété à `2 minutes` exécuter :

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Toutes les distributions qui utilisent le dernier package 1.1.18-11.el7 STIMULATEUR disponibles (notamment RHEL 7.3 et 7.4) introduisent un changement de comportement pour le paramètre de démarrage-échec-est-irrécupérable cluster lorsque sa valeur est false. Cette modification affecte le flux de travail de basculement. Si un réplica principal connaît une panne, le cluster est prévu pour basculer vers un des réplicas secondaires disponibles. Au lieu de cela, les utilisateurs Remarquez que le cluster conserve la tentative de démarrage du réplica principal a échoué. Si ce principal est jamais en ligne (en raison d’une panne permanente), le cluster ne bascule vers un autre réplica secondaire disponible. Grâce à cette modification, une configuration précédemment recommandée pour définir le début-échec-est-irrécupérable n’est plus valide et que le paramètre doit être restauré à sa valeur par défaut de `true`. En outre, la ressource de groupe de disponibilité doit être mis à jour pour inclure le `failover-timeout` propriété. 

Pour mettre à jour la valeur de propriété à `true` exécuter :

```bash
sudo pcs property set start-failure-is-fatal=true
```

Pour mettre à jour le `ag1` propriété de ressource `failure-timeout` à `60s` exécuter :

```bash
pcs resource update ag1 meta failure-timeout=60s
```


Pour plus d’informations sur les propriétés du cluster STIMULATEUR, consultez [STIMULATEUR Clusters propriétés](http://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/ch-clusteropts-HAAR.html).

## <a name="create-a-sql-server-login-for-pacemaker"></a>Créer une connexion SQL Server pour STIMULATEUR

[!INCLUDE [SQL-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Créer la ressource du groupe de disponibilité

Pour créer la ressource de groupe de disponibilité, utilisez `pcs resource create` de commandes et définissez les propriétés de ressource. La commande suivante crée un `ocf:mssql:ag` maître/esclave pour le groupe de disponibilité avec le nom de ressource de type `ag1`.

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=30s master notify=true
``` 

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

<a name="createIP"></a>

## <a name="create-virtual-ip-resource"></a>Créer la ressource IP virtuelle

Pour créer la ressource d’adresse IP virtuelle, exécutez la commande suivante sur un nœud. Utilisez une adresse IP statique disponible à partir du réseau. Remplacez l’adresse IP entre `<10.128.16.240>` avec une adresse IP valide.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

Aucun nom de serveur virtuel est STIMULATEUR équivalents. Pour utiliser une chaîne de connexion qui pointe vers un nom de serveur de chaîne au lieu d’une adresse IP, inscrire l’adresse de la ressource IP virtuelle et nom du serveur virtuel souhaité dans DNS. Pour les configurations de récupération d’urgence, inscrire le nom du serveur virtuel souhaitée et une adresse IP avec les serveurs DNS sur le serveur principal et le site de récupération d’urgence.

## <a name="add-colocation-constraint"></a>Ajouter une contrainte de colocalisation

Presque chaque décision dans un cluster STIMULATEUR, comme choisir où une ressource doit s’exécuter, s’effectue en comparant les scores. Les scores sont calculés par la ressource. Le Gestionnaire de ressources de cluster choisit le nœud avec le score le plus élevé pour une ressource particulière. Si un nœud possède un score négatif pour une ressource, la ressource ne peut pas s’exécuter sur ce nœud.

Sur un cluster STIMULATEUR, vous pouvez manipuler les décisions du cluster avec des contraintes. Les contraintes ont un score. Si une contrainte a un score inférieur à `INFINITY`, STIMULATEUR il considère comme recommandation. Un score de `INFINITY` est obligatoire.

Pour vous assurer que le réplica principal et les ressources d’adresse ip virtuelle exécutent sur le même hôte, définir une contrainte de colocalisation avec un score d’infini. Pour ajouter la contrainte de colocalisation, exécutez la commande suivante sur un nœud.

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Ajouter la contrainte de classement

La contrainte de colocalisation a une contrainte de classement implicite. Il déplace la ressource IP virtuelle avant de passer la ressource de groupe de disponibilité. Par défaut, la séquence d’événements est :

1. Problèmes de l’utilisateur `pcs resource move` pour le groupe de disponibilité principal de Nœud1 vers Nœud2.
1. La ressource IP virtuelle s’arrête sur le nœud 1.
1. La ressource IP virtuelle démarre sur le nœud 2.
  
   >[!NOTE]
   >À ce stade, l’adresse IP temporairement pointe vers le nœud 2 pendant que le nœud 2 est toujours un pre-basculement secondaire. 
   
1. Le groupe de disponibilité principal sur le nœud 1 est rétrogradé en secondaire.
1. Le groupe de disponibilité secondaire sur le nœud 2 est promu comme cache principal. 

Pour éviter que l’adresse IP temporairement en pointant sur le nœud avec le serveur secondaire avant le basculement, ajoutez une contrainte de classement. 

Pour ajouter une contrainte de classement, exécutez la commande suivante sur un nœud :

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

>[!IMPORTANT]
>Une fois que vous configurez le cluster et ajoutez le groupe de disponibilité en tant que ressource de cluster, vous ne pouvez pas utiliser Transact-SQL pour basculer les ressources de groupe de disponibilité. Ressources de cluster de SQL Server sur Linux non couplées aussi étroitement avec le système d’exploitation qu’ils sont sur un Cluster de basculement du serveur Windows (WSFC). Service SQL Server n’est pas informé de la présence du cluster. Orchestration toutes s’effectue via les outils de gestion de cluster. Dans RHEL ou Ubuntu utiliser `pcs` et en cours d’utilisation SLES `crm` outils. 

Basculer manuellement le groupe de disponibilité avec `pcs`. Ne démarrez pas le basculement avec Transact-SQL. Pour obtenir des instructions, consultez [basculement](sql-server-linux-availability-group-failover-ha.md#failover).

## <a name="next-steps"></a>Étapes suivantes

[Utiliser le groupe de disponibilité haute disponibilité](sql-server-linux-availability-group-failover-ha.md)
