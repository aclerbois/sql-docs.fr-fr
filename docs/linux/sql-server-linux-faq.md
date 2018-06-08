---
title: SQL Server sur Linux FAQ | Documents Microsoft
description: Cet article fournit des réponses aux questions fréquemment posées sur SQL Server en cours d’exécution sur Linux.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 03/22/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: b0df550ee3489ba8c37ded47878096d75909343e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-on-linux-frequently-asked-questions-faq"></a>SQL Server sur Linux Forum aux Questions (FAQ)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Les sections suivantes fournissent des questions et réponses pour SQL Server en cours d’exécution sur Linux.

## <a name="general-questions"></a>Questions générales

1. **Les plateformes Linux sont pris en charge ?**

   SQL Server est actuellement pris en charge sur Red Hat Enterprise Server et Ubuntu SUSE Linux Enterprise Server. Il est également possible d’exécuter dans un conteneur avec Docker. Pour plus d’informations sur les versions prises en charge, consultez [prise en charge de plates-formes](sql-server-linux-setup.md#supportedplatforms).

1. **SQL Server sur Linux fonctionnera sur d’autres plateformes**?

   SQL Server est testé et pris en charge sous Linux pour les distributions précédemment répertoriées. Les autres distributions Linux sont étroitement liées et peut être en mesure d’exécuter SQL Server (par exemple, CentOS est étroitement liée à Red Hat Enterprise Server). Mais si vous choisissez d’installer SQL Server sur un système d’exploitation non pris en charge, passez en revue les **prennent en charge de la stratégie** section de la [politique de support technique pour Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) pour comprendre la prise en charge implications. Notez également que certains communautéent gérée par les distributions Linux n’ont pas d’un moyen formel de bénéficier du support si le système d’exploitation sous-jacent est le problème.

1. **Les fonctionnalités de SQL Server sont pris en charge sous Linux ?**

   Pour obtenir une liste complète des fonctionnalités prises en charge et les problèmes connus, consultez le [notes de publication](sql-server-linux-release-notes.md).

1. **Nouveautés de la stratégie de prise en charge pour SQL Server ?**

   Pour comprendre la stratégie de prise en charge, consultez la [politique d’assistance pour SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

1. **Je suis proviennent d’un arrière-plan de Windows SQL Server. Existe-t-il des ressources pour aider à apprendre à utiliser SQL Server sur Linux ?**

   Le [Démarrages rapides](sql-server-linux-setup.md#platforms) fournissent des instructions détaillées sur la façon d’installer SQL Server sur Linux et exécuter des requêtes Transact-SQL. Autres didacticiels fournissent des instructions supplémentaires sur l’utilisation de SQL Server sur Linux. Pour obtenir la liste des tiers des conseils, consultez le [liste MSSQLTIPS de SQL Server sur Linux conseils](https://www.mssqltips.com/sql-server-tip-category/226/sql-server-on-linux/).

## <a name="installation"></a>Installation

1. **Comment obtenir le SQL Server installée sur mes serveurs Linux ?**

   Microsoft gère les référentiels de package d’installation de SQL Server et prend en charge l’installation via les gestionnaires de package natif comme yum, zypper et numéro d’appartement. Pour rapidement installer, consultez une de la [Démarrages rapides](sql-server-linux-setup.md#platforms).

1. **Puis-je installer SQL Server sur le sous-système de Linux pour Windows 10 ?**

   Non. Linux en cours d’exécution sur Windows 10 n’est pas une plateforme prise en charge pour SQL Server et les outils connexes.

1. **Les systèmes de fichiers Linux 2017 du serveur SQL peut utiliser des fichiers de données ?**

   SQL Server sur Linux prend en charge ext4 et XFS. Prise en charge d’autres systèmes de fichiers sera ajouté en fonction des besoins à l’avenir.

1. **Puis-je télécharger les packages d’installation pour installer SQL Server en mode hors connexion ?**

   Oui. Pour plus d’informations, voir le package télécharger les liens dans le [notes de publication](sql-server-linux-release-notes.md). En outre, passez en revue les [des instructions pour les installations en mode hors connexion](sql-server-linux-setup.md#offline).

1. **Puis-je effectuer une installation sans assistance de SQL Server sur Linux ?**

   Oui. Pour en savoir plus sur une installation sans assistance, consultez [aide à l’Installation de SQL Server sur Linux](sql-server-linux-setup.md#unattended). Consultez les exemples de scripts pour [Red Hat](sample-unattended-install-redhat.md), [SUSE Linux Enterprise Server](sample-unattended-install-suse.md), et [Ubuntu](sample-unattended-install-ubuntu.md). Vous pouvez également consulter [cet exemple de script](https://blogs.msdn.microsoft.com/sqlcat/2017/10/03/unattended-install-and-configuration-for-sql-server-2017-on-linux/) créé par l’équipe de consultants clients SQL Server.

## <a name="tools"></a>Outils

1. **Puis-je utiliser le client de SQL Server Management Studio sur Windows pour accéder à SQL Server sur Linux ?**

   Oui, vous pouvez utiliser vos outils existants s’exécutant sur Windows pour accéder à SQL Server sur Linux. Celles-ci incluent les outils à partir de Microsoft tels que SQL Server Management Studio (SSMS), SQL Server Data Tools (SSDT) et les systèmes d’exploitation et des outils tiers.

1. **Existe-t-il un outil tel que SSMS qui s’exécute sur Linux ?**

   Nouveau Microsoft SQL Operations Studio (preview) est un outil d’inter-plateformes de gestion SQL Server. Pour plus d’informations, consultez [Nouveautés opérations Studio (version préliminaire) de Microsoft SQL](../sql-operations-studio/what-is.md).

1. **Commandes sqlcmd et bcp sont disponibles sur Linux ?**

   Oui, [sqlcmd et bcp](sql-server-linux-setup-tools.md) sont disponibles en mode natif sur Windows, Linux et macOS. En outre, utilisez la nouvelle [mssql-scripter](https://github.com/Microsoft/mssql-scripter) outil de ligne de commande sous Linux, Mac OS ou Windows pour générer des scripts T-SQL pour votre base de données SQL en cours d’exécution en tout lieu. En outre, l’aperçu de la version de [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/).

1. **Il est possible d’afficher le moniteur d’activité lors d’une connexion via SSMS sur Windows pour une instance s’exécute sur Linux ?**

   Oui, vous pouvez utiliser SSMS sur Windows pour se connecter à distance, et utilisez outils / fonctionnalités telles que les commandes du moniteur d’activité sur une instance de Linux.

1. **Quels sont les outils disponibles pour surveiller les performances de SQL Server sur Linux ?**

   Vous pouvez utiliser [vues de gestion dynamique (DMV) système](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) pour collecter différents types d’informations sur SQL Server, y compris les informations de processus Linux. Vous pouvez utiliser [magasin de requêtes](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md) pour améliorer les performances des requêtes. Autres outils, tels que la fonction intégrée [tableau de bord performances](https://blogs.msdn.microsoft.com/sql_server_team/new-in-ssms-performance-dashboard-built-in/), demandez à distance dans SQL Server Management Studio (SSMS) à partir de Windows.

## <a name="administration"></a>Administration

1. **Microsoft a créé d’une application, comme le Gestionnaire de Configuration SQL Server sur Linux ?**

   Oui, est un outil de configuration de SQL Server sur Linux : [mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. **SQL Server sur Linux prend en charge les instances multiples sur le même hôte ?**

   Nous vous recommandons d’exécuter plusieurs conteneurs sur un ordinateur hôte d’avoir plusieurs instances distinctes. Chaque conteneur doit écouter sur un port différent. Pour plus d’informations, consultez [exécuter plusieurs conteneurs de SQL Server](sql-server-linux-configure-docker.md#run-multiple-sql-server-containers).

1. **Authentification Active Directory est prise en charge sous Linux ?**

   Oui. Pour plus d’informations, consultez [l’authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md).

1. **Sont Always On et clustering pris en charge sous Linux ?**

   Clustering de basculement et de haute disponibilité sur Linux sont réalisées avec STIMULATEUR sur Linux. Pour plus d’informations, consultez [entreprise la continuité et la base de données de récupération : SQL Server sur Linux](sql-server-linux-business-continuity-dr.md).

1. **Il est possible de configurer la réplication à partir de Linux, Windows et vice versa ?**

   Les réplicas en lecture à l’échelle peuvent servir pour la réplication des données à sens unique entre Windows et Linux.

1. **Il est possible de migrer des bases de données existantes dans les versions antérieures de SQL Server à partir de Windows et Linux ?**

   Oui, il existe [plusieurs méthodes](sql-server-linux-migrate-overview.md) pour ce.

1. **Puis-je migrer mes données d’Oracle et d’autres moteurs de base de données vers SQL Server sur Linux ?**

   Oui. SSMA prend en charge la migration à partir de plusieurs types de moteurs de base de données : Microsoft Access, DB2, MySQL, Oracle et SAP ASE (anciennement SAP Sybase ASE). Pour obtenir un exemple d’utilisation de SSMA, consultez [migrer un schéma Oracle pour 2017 du serveur SQL sur Linux avec l’Assistant Migration SQL Server](../ssma/oracle/sql-server-linux-convert-from-oracle.md?toc=%2fsql%2flinux%2ftoc.json).

1. **Quelles autorisations sont nécessaires pour les fichiers SQL Server ?**

   Tous les fichiers dans le `/var/opt/mssql` dossier de fichiers doit être possédé par le **mssql** utilisateur appartient à la **mssql** groupe. Les deux le **mssql** utilisateur et groupe doivent avoir des autorisations en lecture-écriture de tous les fichiers et répertoires. Notez les scénarios spéciaux suivants qui impliquent des autorisations de fichiers et de répertoires :

   * Autorisations de mssql propriétaire et le groupe sont requises pour les partages réseau qui sont utilisés pour stocker les fichiers de SQL Server.
   * Si vous trouvez des sauvegardes ou des fichiers de base de données dans un répertoire par défaut, vous devez également définir des autorisations pour ce répertoire.
   * Si vous remplacez 0022 l’umask racine par défaut, la configuration de SQL Server échoue après l’installation. Vous devez ensuite appliquer manuellement les autorisations requises au compte de démarrage de SQL Server.

1. **Puis-je modifier la propriété des fichiers de SQL Server et des répertoires dans le compte de mssql installés et le groupe ?**

   Nous ne gèrent pas la modification de la propriété du répertoire de SQL Server et les fichiers à partir de l’installation par défaut. Le compte de mssql et le groupe est utilisé spécifiquement pour SQL Server et n’a pas accès à une connexion interactive.

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]
