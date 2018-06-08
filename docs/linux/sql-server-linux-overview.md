---
title: Vue d’ensemble de SQL Server sur Linux | Documents Microsoft
description: Cet article décrit comment SQL Server s’exécute sur Linux et fournit des informations sur la façon d’en savoir plus.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 05/24/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 9dcc6a90-0add-42c2-815b-862e4e2a21ac
ms.openlocfilehash: 2c67e5d1563ef2420f45ec4b1d0093de888dd157
ms.sourcegitcommit: a9da0abd3e17fbcd6339980d7331d0418cdada53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2018
---
# <a name="sql-server-on-linux"></a>SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

SQL Server 2017 s’exécute désormais également sur Linux. C'est le même moteur de base de données que MSSQL Server, avec de nombreuses fonctionnalités et des services équivalents, quel que soit votre système d’exploitation.

## <a name="install"></a>Installation

Pour démarrer, installez SQL Server sur Linux en suivant l'une des sections de démarrage rapide suivantes :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md)
- [Exécuter sur Docker](quickstart-install-connect-docker.md)
- [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%2fsql%2flinux%2ftoc.json)

> [!NOTE]
> Docker lui-même s’exécute sur plusieurs plateformes, ce qui signifie que vous pouvez exécuter l’image Docker sur Windows, Mac ou Linux.

## <a name="connect"></a>Connection

Après l’installation, connectez-vous à l’instance de SQL Server installée sur un ordinateur Linux. Vous pouvez vous connecter localement ou à distance via un large éventail d’outils et de pilotes. Les didacticiels de démarrage rapide montrent comment utiliser l'outil en ligne de commande [sqlcmd](sql-server-linux-setup-tools.md). Les autres outils possibles sont les suivants :

| Outil | Didacticiel |
|-----|-----|
| Visual Studio Code (Code de Visual Studio) | [Utilisation de Code de Visual Studio avec SQL Server sur Linux](sql-server-linux-develop-use-vscode.md) |
| SQL Server Management Studio (SSMS) | [Utilisez SSMS sur Windows pour se connecter à SQL Server sur Linux](sql-server-linux-manage-ssms.md) |
| Outils de données SQL Server (SSDT) | [Utiliser SSDT avec SQL Server sur Linux](sql-server-linux-develop-use-ssdt.md) |

## <a name="explore"></a>Explorer

SQL Server 2017 a le même moteur de base de données sous-jacent sur toutes les plateformes prises en charge, y compris Linux. Beaucoup de fonctionnalités et de possibilités existent et fonctionnent de la même manière sur Linux ou Windows. Cette partie de la documentation expose certaines de ces fonctionnalités du point de vue Linux. Elle indique également les parties qui ont des exigences spécifiques à Linux.

Si vous êtes déjà familiarisé avec SQL Server, passez en revue les [notes de publication](sql-server-linux-release-notes.md) pour des recommandations générales et les problèmes connus pour cette version. Observez [quelles sont les nouveautés de SQL Server sur Linux](sql-server-linux-whats-new.md) ainsi que les [Nouveautés globales à SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md). 

> [!TIP]
> Pour obtenir des réponses aux questions fréquemment posées, consultez le [SQL Server sur le Forum aux questions sur Linux](sql-server-linux-faq.md).

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]