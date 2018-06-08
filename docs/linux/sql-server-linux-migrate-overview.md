---
title: Migrer des bases de données vers SQL Server sur Linux | Documents Microsoft
description: Cet article décrit les différentes options de migration de bases de données et données vers SQL Server sur Linux.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 03/17/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.technology: linux
ms.assetid: 1619489d-377a-4f32-8930-d4f536539689
ms.custom: sql-linux
ms.openlocfilehash: c7b7225ae4e981244ee06194ea9e4ef595ba283d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="migrate-databases-and-structured-data-to-sql-server-on-linux"></a>Migrer des bases de données et les données structurées vers SQL Server sur Linux 

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Vous pouvez migrer vos bases de données et à SQL Server 2017 est en cours d’exécution sur Linux. La méthode que vous choisissez d’utiliser dépend de la source de données et de votre scénario spécifique. Les sections suivantes fournissent les meilleures pratiques pour différents scénarios de migration.

## <a name="migrate-from-sql-server-on-windows"></a>Migrer à partir de SQL Server sur Windows
Si vous souhaitez migrer les bases de données SQL Server sur Windows à 2017 du serveur SQL sur Linux, la recommandée consiste à utiliser la sauvegarde de SQL Server et de restauration.

1. Créer une sauvegarde de la base de données sur l’ordinateur Windows.
2. Transférer le fichier de sauvegarde sur l’ordinateur SQL Server Linux cible.
3. Restaurez la sauvegarde sur l’ordinateur Linux. 

Pour obtenir un didacticiel sur la migration d’une base de données avec la sauvegarde et de restauration, consultez la rubrique suivante :

- [Restaurer une base de données SQL Server à partir de Windows et Linux](sql-server-linux-migrate-restore-database.md).

Il est également possible d’exporter votre base de données dans un fichier BACPAC (un fichier compressé qui contient votre schéma de base de données et les données). Si vous avez un fichier BACPAC, vous pouvez transférer ce fichier sur votre ordinateur Linux et puis l’importer vers SQL Server. Pour plus d'informations, consultez les rubriques suivantes :

- [Exporter et importer une base de données avec SSMS ou SqlPackage.exe](sql-server-linux-migrate-ssms.md)

## <a name="migrate-from-other-database-servers"></a>Migrer à partir d’autres serveurs de base de données
Vous pouvez migrer des bases de données sur d’autres systèmes de base de données vers 2017 du serveur SQL sur Linux. Cela inclut les bases de données Microsoft Access, DB2, MySQL, Oracle et Sybase. Dans ce scénario, utilisez le SQL Server Management Assistant (SSMA) pour automatiser la migration vers SQL Server sur Linux. Pour plus d’informations, consultez [SSMA utilisé pour migrer des bases de données vers SQL Server sur Linux](sql-server-linux-migrate-ssma.md).  

## <a name="migrate-structured-data"></a>Migrer des données structurées
Il existe également des techniques pour l’importation des données brutes. Peut avoir structuré les fichiers de données qui ont été exportées à partir d’autres bases de données ou les sources de données. Dans ce cas, vous pouvez utiliser l’outil de bcp pour l’insertion en bloc les données. Ou vous pouvez exécuter SQL Server Integration Services sur Windows pour importer les données dans une base de données SQL Server sur Linux. SQL Server Integration Services permet d’exécuter des transformations plus complexes sur les données pendant l’importation. 

Pour plus d’informations sur ces techniques, consultez les rubriques suivantes :

- [Données de copie en bloc avec bcp](sql-server-linux-migrate-bcp.md)
- [Extraire, transformer et charger les données pour SQL Server sur Linux avec SSIS](sql-server-linux-migrate-ssis.md) 
