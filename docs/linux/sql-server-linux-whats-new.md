---
title: Nouveautés de SQL Server 2017 sur Linux | Documents Microsoft
description: Cet article souligne les nouveautés de 2017 du serveur SQL sur Linux.
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
ms.assetid: 456b6f31-6b97-4e31-80ab-b40151ec4868
ms.openlocfilehash: f0e20a06faed4b2d6cda1b80f5be9d41aa2c14f1
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="whats-new-for-sql-server-2017-on-linux"></a>Nouveautés de 2017 du serveur SQL sous Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article décrit les principales fonctionnalités et les services disponibles pour SQL Server 2017 s’exécutant sur Linux.

> [!NOTE]
> Outre ces fonctionnalités dans cet article, les mises à jour cumulatives sont publiées à intervalles réguliers après la version GA. Ces mises à jour cumulatives fournissent de nombreuses améliorations et correctifs. Pour plus d’informations sur la dernière version CU, consultez [ http://aka.ms/sql2017cu ](http://aka.ms/sql2017cu). Pour les téléchargements de packages et les problèmes connus, consultez le [notes de publication](sql-server-linux-release-notes.md).

## <a name="sql-server-database-engine"></a>Moteur de base de données SQL Server

- Fonctionnalités principales de moteur de base de données SQL Server activées.
- Prise en charge des chemins d’accès de Linux natifs.
- Prise en charge IPv6.
- Prise en charge pour les fichiers de base de données sur NFS.
- Chiffrement [Transparent Layer Security ](sql-server-linux-encrypted-connections.md) (TLS) activé.
- [Authentification Active Directory](sql-server-linux-active-directory-authentication.md) activée.
- [Fonctionnalité de groupes de disponibilité](sql-server-linux-availability-group-overview.md) pour la haute disponibilité.
- Prise en charge de la [Recherche de texte intégral](sql-server-linux-setup-full-text-search.md)

## <a name="sql-server-agent"></a>Agent SQL Server

- [L’Agent SQL Server](sql-server-linux-setup-sql-agent.md) prend en charge les tâches suivantes :
  - [Travaux Transact-SQL](sql-server-linux-run-sql-server-agent-job.md)
  - [Messagerie de base de données](sql-server-linux-db-mail-sql-agent.md)
  - [Envoi des journaux de transaction](sql-server-linux-use-log-shipping.md)

## <a name="sql-server-integration-services-ssis"></a>SQL Server Integration Services (SSIS)

- Possibilité d’exécuter des packages SSIS sur Linux. Pour plus d’informations, consultez [configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="other-improvements"></a>Autres améliorations

- Outil de configuration en ligne de commande, [mssql-conf](sql-server-linux-configure-mssql-conf.md).
- Prise en charge de l’installation sans assistance avec [variables d’environnement](sql-server-linux-configure-environment-variables.md).
- [Extension mssql-server de Visual Studio Code](sql-server-linux-develop-use-vscode.md) multiplateforme.
- Générateur de script multiplateforme, [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli/blob/dev/doc/usage_guide.md).
- Moniteur de vue de gestion dynamique (DMV) multiplateforme, [outil DBFS](https://github.com/Microsoft/dbfs).

## <a name="next-steps"></a>Étapes suivantes

Pour installer SQL Server sur Linux, utilisez un des didacticiels suivants :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md)
- [Exécuter sur Docker](quickstart-install-connect-docker.md)
- [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%2fsql%2flinux%2ftoc.json)

Pour connaître les autres améliorations introduites dans SQL Server 2017, consultez [Nouveautés de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

> [!TIP]
> Pour obtenir des réponses aux questions fréquemment posées, consultez le [SQL Server sur le Forum aux questions sur Linux](sql-server-linux-faq.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
