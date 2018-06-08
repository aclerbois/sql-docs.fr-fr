---
title: Utiliser l’instance de cluster de basculement - SQL Server sur Linux | Documents Microsoft
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
ms.openlocfilehash: e48f0e7150fa24361c8b854ced6f90b22448a68b
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="operate-failover-cluster-instance---sql-server-on-linux"></a>Fonctionnement de SQL Server sur Linux - instance de cluster de basculement

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article explique comment utiliser une instance de cluster de basculement (FCI) SQL Server sur Linux. Si vous n’avez pas créé une instance de cluster SQL Server sur Linux, consultez[instance de cluster de basculement configurer - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md)(sql-server-linux-shared-disk-cluster-configure.md).  

## <a name="failover"></a>Basculement

Le basculement d'une instance fci est similaire à un cluster de basculement Windows Server (WSFC). Si le nœud de cluster qui héberge l’instance FCI rencontre quelque sorte de défaillance, l’instance de cluster doit basculer automatiquement vers un autre nœud. Contrairement à un cluster WSFC, il est impossible de définir des propriétaires favoris. Pacemaker sélectionne le nœud qui sera le nouvel hôte de l’instance FCI.

Vous pouvez être amené à procéder manuellement au basculement de l’instance FCI vers un autre nœud. Le processus n’est pas le même que sur les instances fci d'un cluster WSFC. Sur un cluster WSFC, vous basculez les ressources au niveau du rôle. Dans Pacemaker, vous choisissez une ressource à déplacer, et en supposant que toutes les contraintes sont correctes, tout le reste sera également déplacé. 

La manière de basculer dépend de la distribution de Linux. Suivez les instructions de votre distribution linux.

- [RHEL ou Ubuntu](#rhelFailover)
- [SLES](#slesFailover)

## <a name = "#rhelFailover"></a> Basculement manuel (RHEL ou Ubuntu)

Pour effectuer un basculement manuel, sur des serveurs Red Hat Enterprise Linux (RHEL) ou Ubuntu, exécutez les étapes suivantes.
1.  Exécutez la commande suivante : 

   ```bash
   sudo pcs resource move <FCIResourceName> <NewHostNode> 
   ```

   \<FCIResourceName > est le nom de ressource Pacemaker pour l’instance de cluster FCI SQL Server.

   \<NewHostNode > est le nom du nœud de cluster sur lequel vous souhaitez héberger l’instance FCI. 

   Vous n’obtiendrez aucune confirmation.

2.  Lors d’un basculement manuel, Pacemaker crée une contrainte d’emplacement sur la ressource choisie pour le déplacement manuel. Pour voir cette contrainte, exécutez `sudo pcs constraint`.

3.  Une fois le basculement terminé, supprimez la contrainte en exécutant `sudo pcs resource clear <FCIResourceName>`. 

\<FCIResourceName > est le nom de ressource Pacemaker pour l’instance FCI. 

## <a name = "#slesFailover"></a> Basculement manuel (SLES)


Dans Suse Linux Enterprise Server (SLES), utilisez la commande `migrate` pour basculer manuellement une instance FCI SQL Server. Par exemple :

```bash
crm resource migrate <FCIResourceName> <NewHostNode>
```

\<FCIResourceName > est le nom de ressource pour l’instance de cluster de basculement. 

\<NewHostNode > est le nom du nouvel hôte de destination. 


<!---
|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)
--->

## <a name="next-steps"></a>Étapes suivantes

- [Configurer l’instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
