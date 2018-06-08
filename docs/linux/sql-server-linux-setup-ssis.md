---
title: Installer SQL Server Integration Services sur Linux | Documents Microsoft
description: Cet article décrit la procédure d’installation de SQL Server Integration Services (SSIS) sur Linux.
author: leolimsft
ms.author: lle
ms.reviewer: douglasl
manager: craigg
ms.date: 01/09/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: d2e72c77ad5f200c07a6e71025a3461d6397032a
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="install-sql-server-integration-services-ssis-on-linux"></a>Installer SQL Server Integration Services (SSIS) sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Suivez les étapes de cet article pour installer SQL Server Integration Services (`mssql-server-is`) sur Linux. Pour plus d’informations sur les fonctionnalités prises en charge dans cette version des Services d’intégration pour Linux, consultez les [notes de publication](sql-server-linux-release-notes.md).

Installer SQL Server Integration Services pour votre plateforme :

- [Ubuntu](#ubuntu)
- [Red Hat Enterprise Linux](#RHEL)

## <a name="ubuntu"></a> Installer SSIS sur Ubuntu
Pour installer le package `mssql-server-is` sur Ubuntu, procédez comme suit :

1. Importez les clés publiques GPG de référentiel.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Inscrire le référentiel Microsoft SQL Server pour Ubuntu.

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```

3. Exécutez les commandes suivantes pour installer SQL Server Integration Services.

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

4. Après l’installation d’Integration Services, exécutez `ssis-conf`. Pour plus d’informations, consultez [configurer de SSIS sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

5. Une fois la configuration terminée, définissez le chemin d’accès.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

### <a name="update-ssis"></a>Mise à jour SSIS
Si vous avez déjà `mssql-server-is` installé, vous pouvez mettre à jour vers la dernière version avec la commande suivante :

```bash
sudo apt-get install mssql-server-is
```

### <a name="remove-ssis"></a>Supprimer SSIS
Pour supprimer `mssql-server-is`, vous pouvez exécuter la commande suivante :
```bash
sudo apt-get remove mssql-server-is
```

## <a name="RHEL"></a> Installer SSIS sur RHEL
Pour installer le package `mssql-server-is` sur RHEL, procédez comme suit :

1. Téléchargez le fichier de configuration du référentiel Microsoft SQL Server Red Hat.

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```

1. Exécutez les commandes suivantes pour installer SQL Server Integration Services.

   ```bash
   sudo yum install -y mssql-server-is
   ```


1. Après l’installation, exécutez `ssis-conf`. Pour plus d’informations, consultez [configurer de SSIS sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Une fois la configuration terminée, définissez le chemin d’accès.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

### <a name="update-ssis"></a>Mise à jour SSIS
Si vous avez déjà `mssql-server-is` installé, vous pouvez mettre à jour vers la dernière version avec la commande suivante :

```bash
sudo yum update mssql-server-is
```

### <a name="remove-ssis"></a>Supprimer SSIS
Pour supprimer `mssql-server-is`, vous pouvez exécuter la commande suivante :
```bash
sudo yum remove mssql-server-is
```

## <a name="unattended-installation"></a>Installation sans assistance
Pour exécuter une installation sans assistance lorsque vous exécutez `ssis-conf setup`, procédez comme suit :
1.  Spécifiez le `-n` (sans invite) option.
2.  Fournir les valeurs requises en définissant les variables d’environnement.

L’exemple suivant effectue les opérations suivantes :
-   Installe SSIS.
-   Spécifie l’Édition Developer en fournissant une valeur pour le `SSIS_PID` variable d’environnement.
-   Accepte le CLUF en fournissant une valeur pour le `ACCEPT_EULA` variable d’environnement.
-   Exécute une installation sans assistance en spécifiant le `-n` (sans invite) option.

```
sudo SSIS_PID=Developer ACCEPT_EULA=Y /opt/ssis/bin/ssis-conf -n setup 
```

### <a name="environment-variables-for-unattended-installation"></a>Variables d’environnement pour l’installation sans assistance

| Variable d'environnement |  Description |
|---|---|
| **ACCEPT_EULA** | Accepte le contrat de licence de SQL Server lorsque la valeur n’importe quelle valeur (par exemple, `Y`).|
| **SSIS_PID** | Définit la clé de produit ou d’édition de SQL Server. Les valeurs possibles sont :<br/>Evaluation<br/>Développeur<br/>Express <br/>Web <br/>Standard<br/>Enterprise <br/>Une clé de produit<br/><br/>Si vous spécifiez une clé de produit, la clé de produit doit être sous la forme `#####-#####-#####-#####-#####`, où `#` est une lettre ou un chiffre.  |
| | |

## <a name="next-steps"></a>Étapes suivantes

Pour exécuter des packages SSIS sur Linux, consultez [extraction, de transformation et de charger les données pour SQL Server sur Linux avec SSIS](sql-server-linux-migrate-ssis.md).

Pour configurer des paramètres supplémentaires SSIS sur Linux, consultez [configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="related-content-about-ssis-on-linux"></a>Contenu associé sur SSIS sur Linux
-   [Extraire, transformer et charger des données sur Linux avec SSIS](sql-server-linux-migrate-ssis.md)
-   [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md)
-   [L’exécution sur Linux avec cron du package de planification SQL Server Integration Services](sql-server-linux-schedule-ssis-packages.md)
