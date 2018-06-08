---
title: Commentaires client pour SQL Server sur Linux | Documents Microsoft
description: Décrit comment les commentaires des clients de SQL Server sont collectées et configuré sur Linux.
author: annashres
ms.author: anshrest
manager: craigg
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: 66275b164e1d6514d04e0c8a6f1a666de0a02425
ms.sourcegitcommit: df382099ef1562b5f2d1cd506c1170d1db64de41
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="customer-feedback-for-sql-server-on-linux"></a>Commentaires client pour SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Par défaut, Microsoft SQL Server collecte des informations sur la façon dont ses clients utilisent l’application. Plus précisément, SQL Server recueille des données sur l’expérience d’installation, l’utilisation et les performances. Elles aident Microsoft à améliorer le produit pour mieux répondre aux besoins des clients. Par exemple, Microsoft collecte des informations sur les types de codes d’erreur rencontrés par les utilisateurs afin que nous puissions corriger les bogues associés, améliorer notre documentation sur l’utilisation de SQL Server et déterminer s’il faudrait ajouter des fonctionnalités au produit pour mieux servir des clients.

Ce document fournit des détails sur les types d’informations sont collectées et explique comment configurer Microsoft SQL Server sur Linux pour envoyer ce collectées des informations à Microsoft. SQL Server 2017 inclut une déclaration de confidentialité qui explique les informations que nous faire et ne collectent pas d’utilisateurs. Veuillez lire la déclaration de confidentialité.

En particulier, Microsoft n’envoie par ce mécanisme aucune information de ces types :

- valeurs des tables utilisateur ;
- identifiants d’ouverture de session ou autres informations d’authentification ;
- informations d’identification personnelle (PII).

SQL Server 2017 collecte et envoie toujours des informations sur l’expérience d’installation à partir du processus d’installation afin de nous permettre de trouver et de résoudre rapidement les problèmes d’installation que rencontre le client. SQL Server 2017 peut être configuré pour ne pas pour envoyer des informations (sur la base d’une instance par serveur) à Microsoft via **mssql-conf**. MSSQL-conf est un script de configuration qui est installé avec SQL Server 2017 pour Red Hat Enterprise Linux, SUSE Linux Enterprise Server et Ubuntu.

> [!NOTE]
> Vous ne pouvez désactiver l’envoi d’informations à Microsoft que dans les versions payantes de SQL Server.

## <a name="disable-customer-feedback"></a>Désactiver les commentaires client

Cette option vous permet de modifier si SQL Server envoie des commentaires à Microsoft ou non. Par défaut, cette valeur est définie sur true. Pour modifier la valeur, exécutez les commandes suivantes :

### <a name="on-red-hat-suse-and-ubuntu"></a>Sur Ubuntu, SUSE et Red Hat

1. Exécutez le script mssql-conf en tant que racine avec le **définir** commande **telemetry.customerfeedback**. L’exemple suivant désactive les commentaires des clients en spécifiant **false**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>Sur Docker
Pour désactiver les commentaires des clients sur docker, vous devez disposer Docker [conserver vos données](sql-server-linux-configure-docker.md). 

1. Ajouter un `mssql.conf` fichier avec les lignes `[telemetry]` et `customerfeedback = false` dans le répertoire de l’hôte :
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```
2. Exécuter l’image de conteneur
   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d microsoft/mssql-server-linux:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d microsoft/mssql-server-linux:2017-latest
   ```
   
## <a name="local-audit-for-sql-server-on-linux-usage-feedback-collection"></a>Audit local pour SQL Server sur la Collection de commentaires de l’utilisation de Linux

Microsoft SQL Server 2017 contient des fonctionnalités Internet qui peuvent recueillir et envoyer des informations sur votre ordinateur ou appareil (« informations standard de l’ordinateur ») à Microsoft. Le composant d’Audit Local de la collection de commentaires sur l’utilisation de SQL Server peut écrire des données collectées par le service vers un dossier désigné, qui représente les données (journaux) qui peuvent être envoyées à Microsoft. L’objectif de l’audit local est d’autoriser les clients à visualiser toutes les données collectées par Microsoft avec cette fonctionnalité, pour des raisons de conformité, de réglementation ou de validation de la confidentialité.

Dans SQL Server sur Linux, d’Audit Local est configurable au niveau de l’instance du moteur de base de données SQL Server. Autres composants SQL Server et les outils SQL Server ne possèdent pas de fonctionnalité de Local d’Audit pour la collecte de commentaires d’utilisation.

### <a name="enable-local-audit"></a>Activer l’Audit Local

Cette option permet d’Audit Local et vous permet de définir le répertoire dans lequel les journaux d’Audit Local sont créés.

1. Créez un répertoire cible pour les nouveaux journaux d’Audit Local. L’exemple suivant crée un nouveau **/tmp/audit** active :

   ```bash
   sudo mkdir /tmp/audit
   ```

1. Modifier le propriétaire et le groupe du répertoire à la **mssql** utilisateur :

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

1. Exécutez le script mssql-conf en tant que racine avec le **définir** commande **telemetry.userrequestedlocalauditdirectory**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>Sur Docker
Pour activer l’Audit Local sur docker, vous devez disposer Docker [conserver vos données](sql-server-linux-configure-docker.md). 

1. Le répertoire cible pour les nouveaux journaux d’Audit Local se trouve dans le conteneur. Créer un répertoire cible pour les nouveaux journaux d’Audit Local dans le répertoire de l’hôte sur votre ordinateur. L’exemple suivant crée un nouveau **/audit** active :

   ```bash
   sudo mkdir <host directory>/audit
   ```

   
1. Ajouter un `mssql.conf` fichier avec les lignes `[telemetry]` et `userrequestedlocalauditdirectory = <host directory>/audit` dans le répertoire de l’hôte :
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```
2. Exécuter l’image de conteneur
   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d microsoft/mssql-server-linux:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d microsoft/mssql-server-linux:2017-latest
   ```
   
## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur SQL Server sur Linux, consultez le [vue d’ensemble de SQL Server sur Linux](sql-server-linux-overview.md).
