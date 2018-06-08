---
title: Installer l’Agent SQL Server sur Linux | Documents Microsoft
description: Cet article décrit comment installer l’Agent SQL Server sur Linux.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 02/20/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 77f16adc-e6cb-4a57-82f3-7b9780369868
ms.openlocfilehash: eae6d3389405f6c70e486d0569a65ca5d0a0d15d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="install-sql-server-agent-on-linux"></a>Installer l’Agent SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

 [L’Agent SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) exécute les travaux planifiés de SQL Server. À partir de SQL Server 2017 CU4, l’Agent SQL Server est inclus avec le **mssql-serveur** du package et est désactivée par défaut. Pour plus d’informations sur les fonctionnalités prises en charge pour cette version de l’Agent SQL Server, ainsi que des informations de version, consultez le [Release Notes](sql-server-linux-release-notes.md).

 Installer/activer l’Agent SQL Server :
- [Pour les Versions 2017 CU4 et versions ultérieures, activez l’Agent SQL Server](#EnableAgentAfterCU4)
- [Pour les Versions 2017 CU3 et versions antérieures, installez l’Agent SQL Server](#InstallAgentBelowCU4)


## <a name="EnableAgentAfterCU4">Pour les Versions 2017 CU4 et versions ultérieures, activez l’Agent SQL Server</a>

 Pour activer l’Agent SQL Server, suivez les étapes ci-dessous.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
sudo systemctl restart mssql-server
```

> [!NOTE]
> Si vous mettez à niveau à partir de 2017 CU3 ou ci-dessous avec l’Agent installé, l’Agent SQL Server sera automatiquement activé et précédent seront désinstallés packages d’Agent.  

## <a name="InstallAgentBelowCU4">Pour les Versions 2017 CU3 et versions antérieures, installez l’Agent SQL Server</a>

> [!NOTE]
> Les instructions d’installation ci-dessous s’appliquent à SQL Server Versions 2017 CU3 et versions antérieures. Avant d’installer l’Agent SQL Server, tout d'abord [installez SQL Server 2017](sql-server-linux-setup.md#platforms). Cela configurera les clés et les référentiels que vous utiliserez lorsque vous installerez le package **mssql-server-agent**.

Installez l’Agent SQL Server pour votre plateforme :
- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

### <a name="RHEL">Installer sur RHEL</a>

Utilisez les étapes suivantes pour installer le package **mssql-server-agent** sur Red Hat Enterprise Linux. 

```bash
sudo yum install mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez déjà **mssql-server-agent** installé, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo yum check-update
sudo yum update mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package de l’Agent SQL Server dans les [notes de publication](sql-server-linux-release-notes.md).  Puis utilisez les étapes d’installation hors connexion qui sont décrites dans la rubrique [installer SQL Server](sql-server-linux-setup.md#offline).

### <a name="ubuntu">Installer sur Ubuntu</a>

Utilisez les étapes suivantes pour installer le package **mssql-server-agent** sur Ubuntu.  

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez déjà **mssql-server-agent** installé, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package de l’Agent SQL Server dans les [notes de publication](sql-server-linux-release-notes.md).  Puis utilisez les étapes d’installation hors connexion qui sont décrites dans la rubrique [installer SQL Server](sql-server-linux-setup.md#offline).

### <a name="SLES">Installez SLES</a>

Utilisez les étapes suivantes pour installer le package **mssql-server-agent** sur SUSE Linux Enterprise Server. 

Installer **mssql-server-agent** 

```bash
sudo zypper install mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez déjà **mssql-server-agent** installé, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo zypper refresh
sudo zypper update mssql-server-agent
sudo systemctl restart mssql-server
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package de l’Agent SQL Server dans les [notes de publication](sql-server-linux-release-notes.md).  Puis utilisez les étapes d’installation hors connexion qui sont décrites dans la rubrique [installer SQL Server](sql-server-linux-setup.md#offline).

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur l’utilisation de l’Agent SQL Server pour créer, planifier et exécuter des travaux, consultez [exécuter un travail de l’Agent SQL Server sur Linux](sql-server-linux-run-sql-server-agent-job.md).
