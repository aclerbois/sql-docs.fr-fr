---
title: Meilleures pratiques de performances pour SQL Server sur Linux | Documents Microsoft
description: Cet article fournit des performances meilleures pratiques et des instructions pour l’exécution de SQL Server 2017 sur Linux.
author: rgward
ms.author: bobward
manager: craigg
ms.date: 09/14/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: 4725a8043be3aad99f3432f99d00f6279a9d814d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-2017-on-linux"></a>Meilleures pratiques de performance et des instructions de configuration pour 2017 du serveur SQL sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article fournit les meilleures pratiques et recommandations pour optimiser les performances pour les applications de base de données qui se connectent à SQL Server sur Linux. Ces recommandations sont spécifiques à l’exécution de la plate-forme Linux. Toutes les recommandations de SQL Server standard, telles que la création d’index, s’appliquent toujours.

Les instructions suivantes contiennent des recommandations pour la configuration de SQL Server et le système d’exploitation Linux.

## <a name="sql-server-configuration"></a>Configuration de SQL Server

Il est recommandé d’effectuer les tâches de configuration suivantes après avoir installé SQL Server sur Linux pour obtenir de meilleures performances pour votre application.

### <a name="best-practices"></a>Meilleures pratiques

- **Utiliser l’affinité de processus pour le nœud et/ou des unités centrales**

   Il est recommandé d’utiliser `ALTER SERVER CONFIGURATION` pour définir `PROCESS AFFINITY` pour toutes les **NUMANODEs** et/ou de l’UC vous utilisez pour SQL Server (qui est en général pour tous les nœuds et les unités centrales) sur un système d’exploitation de Linux. Affinité du processeur permet de conserver le comportement de Linux et de planification de SQL efficace. À l’aide de la **NUMANODE** option est la méthode la plus simple. Notez que vous devez utiliser **l’affinité de processus** même si vous avez un seul nœud NUMA sur votre ordinateur.  Consultez le [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md) documentation pour plus d’informations sur la façon de définir **l’affinité de processus**.

- **Configurer plusieurs fichiers de données tempdb**

   Un serveur SQL Server sur Linux installation n’offre pas la possibilité de configurer plusieurs fichiers de tempdb, nous vous recommandons que vous envisagez de créer des fichiers de données tempdb plusieurs après l’installation. Pour plus d’informations, consultez les instructions de l’article, [recommandations pour réduire les contentions d’allocation dans la base de données tempdb de SQL Server](https://support.microsoft.com/en-us/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d).

### <a name="advanced-configuration"></a>Configuration avancée

Les recommandations suivantes sont des paramètres de configuration facultatives que vous pouvez choisir à effectuer après l’installation de SQL Server sur Linux. Ces choix est basées sur les exigences de votre charge de travail et la configuration de votre système d’exploitation de Linux.

- **Définir une limite de mémoire avec mssql-conf**

   Afin de garantir la mémoire physique disponible pour le système d’exploitation Linux, le processus SQL Server utilise seulement 80 % de la RAM physique par défaut. Pour certains systèmes la grande quantité de RAM physique, 20 % peut être un nombre significatif. Par exemple, sur un système de 1 To de RAM, le paramètre par défaut continuent environ 200 Go de RAM inutilisé. Dans ce cas, vous souhaiterez configurer la limite de mémoire à une valeur plus élevée. Consultez la documentation sur le **mssql-conf** outil et la [memory.memorylimitmb](sql-server-linux-configure-mssql-conf.md#memorylimit) qui contrôle la mémoire visible pour SQL Server (en unités de Mo).

   Lorsque vous modifiez ce paramètre, veillez à ne pas définir une valeur trop élevée. Si vous ne laissez pas suffisamment de mémoire, vous pouvez constater des problèmes avec le système d’exploitation Linux et d’autres applications de Linux.

## <a name="linux-os-configuration"></a>Configuration du système d’exploitation de Linux

Envisagez d’utiliser les paramètres de configuration de système d’exploitation Linux suivants pour bénéficier des performances optimales pour une Installation de SQL Server.

### <a name="kernel-settings-for-high-performance"></a>Paramètres de noyau pour des performances élevées
Voici le débit pour une installation de SQL Server et les performances connexes élevé de paramètres de système d’exploitation Linux recommandée. Consultez la documentation de votre système d’exploitation Linux pour le processus configurer ces paramètres.



> [!Note]
> Pour les utilisateurs de Red Hat Enterprise Linux (RHEL), le profil de performance de débit configurerez ces paramètres automatiquement (à l’exception des États-C).

Le tableau suivant fournit des recommandations pour les paramètres de l’UC :

| Paramètre | Valeur | Informations complémentaires |
|---|---|---|
| Gouverneur de fréquence du processeur | performances | Consultez le **cpupower** commande |
| ENERGY_PERF_BIAS | performances | Consultez le **x86_energy_perf_policy** commande |
| min_perf_pct | 100 | Consultez la documentation sur intel p-état |
| États C | C1 uniquement | Consultez la documentation de Linux ou système et vous assurer que les États C a la valeur C1 uniquement |

Le tableau suivant fournit des recommandations pour les paramètres de disque :

| Paramètre | Valeur | Informations complémentaires |
|---|---|---|
| lues par anticipation de disque | 4096 | Consultez le **blockdev** commande |
| paramètres de sysctl | Kernel.sched_min_granularity_ns = 10000000<br/>Kernel.sched_wakeup_granularity_ns = 15 000 000<br/>VM.dirty_ratio = 40<br/>VM.dirty_background_ratio = 10<br/>vm.swappiness=10 | Consultez le **sysctl** commande |

### <a name="kernel-setting-auto-numa-balancing-for-multi-node-numa-systems"></a>Noyau paramètre auto numa équilibrage pour les systèmes à plusieurs nœuds NUMA

Si vous installez SQL Server sur un nœud multi **NUMA** systèmes, les éléments suivants **kernel.numa_balancing** paramètre du noyau est activé par défaut. Pour permettre à SQL Server fonctionner à une efficacité maximale sur une **NUMA** système numa automatique de désactiver l’équilibrage sur un système NUMA de plusieurs nœud :

```bash
sysctl -w kernel.numa_balancing=0
```

### <a name="kernel-settings-for-virtual-address-space"></a>Paramètres de noyau pour l’espace d’adressage virtuel

Le paramètre par défaut de **vm.max_map_count** (qui est de 65 536) peut ne pas être suffisamment élevée pour une installation de SQL Server. Modifiez cette valeur (qui est une limite supérieure) à 256 Ko.

```bash
sysctl -w vm.max_map_count=262144
```

### <a name="disable-last-accessed-datetime-on-file-systems-for-sql-server-data-and-log-files"></a>Désactiver les date/date du dernier accès sur les systèmes de fichiers pour les fichiers journaux et de données SQL Server

Utilisez le **noatime** attribut avec n’importe quel système de fichiers qui est utilisé pour stocker les données de SQL Server et les fichiers journaux. Consultez votre documentation Linux sur la façon de définir cet attribut.

### <a name="leave-transparent-huge-pages-thp-enabled"></a>Laissez Transparent énorme Pages (THP) activé

La plupart des installations de Linux doit disposer de cette option sur par défaut. Nous vous recommandons de l’expérience de performances plus cohérent de laisser cette option de configuration activée.

### <a name="swapfile"></a>fichier d’échange

Assurez-vous de qu'avoir un fichier d’échange configuré correctement pour éviter les problèmes de mémoire insuffisante. Consultez votre documentation Linux pour savoir comment créer et dimensionner correctement un fichier d’échange.

### <a name="virtual-machines-and-dynamic-memory"></a>Machines virtuelles et mémoire dynamique

Si vous exécutez SQL Server sur Linux sur une machine virtuelle, veillez à que sélectionner les options pour corriger la quantité de mémoire réservée pour l’ordinateur virtuel. N’utilisez pas de fonctionnalités telles que la mémoire dynamique Hyper-V.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les fonctionnalités de SQL Server qui améliorent les performances, consultez [prise en main des fonctionnalités de performances](sql-server-linux-performance-get-started.md).

Pour plus d’informations sur SQL Server sur Linux, consultez [vue d’ensemble de SQL Server sur Linux](sql-server-linux-overview.md).
