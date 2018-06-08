---
title: Extraire, transformer et charger des données sur Linux avec SSIS | Documents Microsoft
description: Cet article décrit de SQL Server Integration Services (SSIS) pour les ordinateurs Linux
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
ms.openlocfilehash: 0fc4e01f4bdd5eb56c41b1e85b7e96ba869f5454
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="extract-transform-and-load-data-on-linux-with-ssis"></a>Extraire, transformer et charger des données sur Linux avec SSIS

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article décrit comment exécuter les packages SQL Server Integration Services (SSIS) sur Linux. SSIS permet de résoudre des problèmes d’intégration des données complexes en extrayant les données à partir de plusieurs sources et de formats, transformation et de nettoyage des données et de charger les données dans plusieurs destinations. 

Les packages SSIS en cours d’exécution sur Linux peuvent se connecter à Microsoft SQL Server est en cours d’exécution sur Windows local ou dans le cloud, sur Linux, ou dans Docker. Ils peuvent également se connecter à la base de données SQL Azure, Azure SQL Data Warehouse, les sources de données ODBC, fichiers plats et autres sources de données, y compris les sources de ADO.NET, les fichiers XML et les services OData.

Pour plus d’informations sur les fonctionnalités de SSIS, consultez [SQL Server Integration Services](../integration-services/sql-server-integration-services.md).

## <a name="prerequisites"></a>Configuration requise

Pour exécuter des packages SSIS sur un ordinateur Linux, vous devez d’abord installer SQL Server Integration Services. SSIS n’est pas inclus dans l’installation de SQL Server pour les ordinateurs Linux. Pour obtenir des instructions d’installation, consultez [installer SQL Server Integration Services](sql-server-linux-setup-ssis.md).

Vous devez également disposer d’un ordinateur Windows pour créer et gérer des packages. Les outils de conception et de gestion de SSIS sont des applications Windows qui ne sont pas actuellement disponibles pour les ordinateurs Linux. 

## <a name="run-an-ssis-package"></a>Exécutez un package SSIS

Pour exécuter un package SSIS sur un ordinateur Linux, procédez comme suit :

1.  Copiez le package SSIS sur l’ordinateur Linux.
2.  Exécutez la commande suivante :
    ```
    $ dtexec /F \<package name \> /DE <protection password>
    ```

## <a name="run-an-encrypted-password-protected-package"></a>Exécuter un package chiffré (protégé par mot de passe)
Il existe trois manières d’exécuter un package SSIS qui est chiffré avec un mot de passe :

1.  Définir la valeur de la variable d’environnement `SSIS_PACKAGE_DECRYPT`, comme illustré dans l’exemple suivant :

    ```
    SSIS_PACKAGE_DECRYPT=test /opt/ssis/bin/dtexec /f package.dtsx
    ```

2.  Spécifiez le `/de[crypt]` option pour entrer le mot de passe interactivement, comme indiqué dans l’exemple suivant :

    ```
    /opt/ssis/bin/dtexec /f package.dtsx /de
    
    Enter decryption password:
    ```

3.  Spécifiez le `/de` permet de fournir le mot de passe sur la ligne de commande, comme indiqué dans l’exemple suivant. Cette méthode n’est pas recommandée, car elle stocke le mot de passe de déchiffrement avec la commande dans l’historique des commandes.

    ```
    opt/ssis/bin/dtexec /f package.dtsx /de test
    
    Warning: Using /De[crypt] <password> may store decryption password in command history.
    
    You can use /De[crypt] instead to enter interactive mode,
    or use environment variable SSIS_PACKAGE_DECRYPT to set decryption password.
    ```

## <a name="design-packages"></a>Concevoir des packages

**Se connecter aux sources de données ODBC**. SSIS lors de l’actualisation de Linux CTP 2.1 et versions ultérieures, les packages SSIS permet les connexions ODBC sur Linux. Cette fonctionnalité a été testée avec les pilotes ODBC MySQL sur le serveur SQL Server, mais il est également prévue pour fonctionner avec n’importe quel pilote ODBC Unicode qui respecte la spécification ODBC. Au moment du design, vous pouvez fournir une source de données ou une chaîne de connexion pour se connecter aux données ODBC ; Vous pouvez également utiliser l’authentification Windows. Pour plus d’informations, consultez la [blog de l’annonce prise en charge d’ODBC sur Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

**Chemins d’accès**. Spécifiez les chemins d’accès de type Windows dans vos packages SSIS. SSIS sur Linux ne prend pas en charge les chemins d’accès de type Linux, mais mappe les chemins d’accès de type Windows aux chemins d’accès de type Linux en cours d’exécution. Ensuite, par exemple, SSIS sur Linux mappe le chemin d’accès de type Windows `C:\test` pour le chemin d’accès de type Linux `/test`.

## <a name="deploy-packages"></a>Déployer des packages
Vous ne pouvez stocker des packages dans le système de fichiers sur Linux dans cette version. La base de données du catalogue SSIS et le service SSIS hérité ne sont pas disponibles sur Linux pour le stockage et le déploiement du package.

## <a name="schedule-packages"></a>Planifier les packages
Vous pouvez utiliser le système Linux tels que les outils de planification `cron` pour planifier les packages. Vous ne pouvez pas utiliser l’Agent SQL sur Linux pour planifier l’exécution du package dans cette version. Pour plus d’informations, consultez [packages SSIS de planification sur Linux avec cron](sql-server-linux-schedule-ssis-packages.md).

## <a name="limitations-and-known-issues"></a>Limitations et problèmes connus

Pour obtenir des informations détaillées sur les limitations et problèmes connus de SSIS sur Linux, consultez [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md).

## <a name="more-info-about-ssis-on-linux"></a>Plus d’informations sur SSIS sur Linux

Pour plus d’informations sur SSIS sur Linux, consultez les billets de blog suivants :

-   [SSIS sur Linux n’est disponible dans SQL Server 2017 CTP2.1](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/)
-   [ODBC est pris en charge dans SSIS sur Linux (SQL Server 2017 CTP 2.1 Actualiser)](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/)

## <a name="more-info-about-ssis"></a>Plus d’informations sur SSIS

Microsoft SQL Server Integration Services (SSIS) est une plate-forme de création de solutions d’intégration de données hautes performances, y compris l’extraction, transformation et chargement (ETL) de packages pour l’entreposage des données. Pour plus d’informations, consultez [SQL Server Integration Services](/sql/integration-services/sql-server-integration-services).

SSIS inclut les fonctionnalités suivantes :
- outils graphiques et des Assistants pour générer et déboguer des packages sur Windows
- une variété de tâches pour effectuer des fonctions de flux de travail tels que les opérations FTP, exécution d’instructions SQL et envoi de messages électroniques
- une variété de sources de données et des destinations pour l’extraction et chargement des données
- différentes transformations pour le nettoyage, l’agrégation, la fusion et la copie de données
- interfaces de programmation d’application (API) pour l’extension SSIS avec vos propres scripts personnalisés et les composants

Pour commencer avec SSIS, téléchargez la dernière version de [SQL Server Data Tools (SSDT)](../integration-services/ssis-how-to-create-an-etl-package.md).

Pour en savoir plus sur SSIS, consultez les articles suivants :
- [En savoir plus sur SQL Server Integration Services](../integration-services/sql-server-integration-services.md)
- [Développement de SQL Server Integration Services (SSIS) et les outils de gestion](../integration-services/integration-services-ssis-development-and-management-tools.md)
- [Didacticiels sur SQL Server Integration Services](../integration-services/integration-services-tutorials.md)

## <a name="related-content-about-ssis-on-linux"></a>Contenu associé sur SSIS sur Linux
-   [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md)
-   [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md)
-   [L’exécution sur Linux avec cron du package de planification SQL Server Integration Services](sql-server-linux-schedule-ssis-packages.md)
