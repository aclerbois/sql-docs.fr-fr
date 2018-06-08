---
title: Planifier des packages SSIS sur Linux avec cron | Documents Microsoft
description: Cet article décrit comment planifier des packages SQL Server Integration Services (SSIS) sur Linux avec le service cron.
author: leolimsft
ms.author: lle
ms.reviewer: douglasl
manager: craigg
ms.date: 10/02/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: 3dd2c69dae65f073ec7bc34a40ae1f31be2c1a7c
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="schedule-sql-server-integration-services-package-execution-on-linux-with-cron"></a>L’exécution sur Linux avec cron du package de planification SQL Server Integration Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Quand vous exécutez SSIS (SQL Server Integration Services) et SQL Server sur Windows, vous pouvez automatiser l’exécution de packages SSIS à l’aide de SQL Server Agent. Toutefois, quand vous exécutez SQL Server et SSIS sur Linux, l’utilitaire SQL Server Agent n’est pas disponible pour planifier ces travaux. Au lieu de cela, vous utilisez le service cron, qui est largement utilisé sur les plateformes Linux pour automatiser l’exécution de packages.

Cet article fournit des exemples qui montrent comment automatiser l’exécution des packages SSIS. Les exemples sont écrits pour s’exécuter sur Red Hat Enterprise. Le code est identique pour les autres distributions Linux, telles que Ubuntu.

## <a name="prerequisites"></a>Configuration requise

Avant d’utiliser le service cron pour exécuter des travaux, vérifiez s'il est en cours d'exécution sur votre ordinateur.

Pour vérifier l’état du service cron, utilisez la commande suivante : `systemctl status crond.service`.

Si le service n’est pas actif (autrement dit, il ne fonctionne pas), consultez votre administrateur pour installer et configurer le service cron correctement.

## <a name="create-jobs"></a>Créer des travaux

Une tâche cron est une tâche que vous pouvez configurer pour s’exécuter régulièrement à un intervalle spécifié. La tâche peut être aussi simple qu’une commande qui vous permet de taper directement dans la console ou d’exécuter en tant qu’un script shell normalement.

Pour faciliter la gestion et à des fins de maintenance, nous vous recommandons de placer vos commandes de l’exécution du package dans un script qui contient un nom descriptif.

Voici un exemple d’un script shell simple pour l’exécution d’un package. Il contient une commande unique, mais vous pouvez ajouter davantage de commandes en fonction des besoins.

```bash
# A simple shell script that contains a simple package execution command
# Script name: SSISpackageName.daily

/opt/ssis/bin/dtexec /F yourSSISpackageName.dtsx >> $HOME/tmp/out 2>&1
```

## <a name="schedule-jobs-with-the-cron-service"></a>Planifier des tâches avec le service cron

Après avoir défini vos tâches, vous pouvez planifier celles-ci pour qu'elles s’exécutent automatiquement à l’aide du service cron.

Pour ajouter un travail à cron pour qu’il l’exécute, ajoutez-le dans le fichier crontab. Pour ouvrir le fichier crontab dans un éditeur, où vous pouvez ajouter ou mettre à jour les travaux, utilisez la commande suivante : `crontab -e`.

Pour planifier le travail décrit précédemment pour exécuter tous les jours à 2 h 10, ajoutez la ligne suivante au fichier crontab :

```
# run <SSIS package name> at 2:10 AM every day
10 2 \* \* \* $/HOME/SSIS/jobs/SSISpackageName.daily
```

Enregistrez le fichier crontab, puis quittez l’éditeur.

Pour comprendre le format de l’exemple de commande, passez en revue les informations contenues dans la section suivante.
 
## <a name="format-of-a-crontab-file"></a>Format d’un fichier crontab

L’illustration suivante montre la description du format de la ligne de tâche qui est ajoutée au fichier crontab.

![Description du format pour l’entrée dans le fichier crontab](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-job-definition.png)

Pour obtenir une description plus détaillée du format de fichier crontab, utilisez la commande suivante : `man 5 crontab`.

Voici un exemple partiel de la sortie qui permet de comprendre l’exemple contenu dans cet article :

![Description partielle détaillée du format de crontab](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-crontab-format.png)

## <a name="related-content-about-ssis-on-linux"></a>Contenu associé sur SSIS sur Linux
-   [Extraire, transformer et charger des données sur Linux avec SSIS](sql-server-linux-migrate-ssis.md)
-   [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md)
-   [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md)
