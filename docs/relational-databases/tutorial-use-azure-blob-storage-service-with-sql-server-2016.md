---
title: 'Didacticiel : Utiliser le service Stockage Blob Azure avec SQL Server 2016 | Microsoft Docs'
ms.custom: ''
ms.date: 01/07/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: e69be67d-da1c-41ae-8c9a-6b12c8c2fb61
caps.latest.revision: 23
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: c2aef9476c254267156c5bbde4d777a2ed5ab570
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-use-azure-blob-storage-service-with-sql-server-2016"></a>Didacticiel : Utiliser le service Stockage Blob Azure avec SQL Server 2016
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Bienvenue dans le didacticiel sur l’utilisation de SQL Server 2016 dans le service Stockage Blob Microsoft Azure. Ce didacticiel explique comment utiliser le service Stockage Blob Microsoft Azure pour les fichiers de données SQL Server et les sauvegardes SQL Server.  
  
La prise en charge de l’intégration SQL Server pour le service Stockage Blob Microsoft Azure a commencé sous la forme d’une amélioration dans SQL Server 2012 Service Pack 1 CU2 et a été affinée avec SQL Server 2014 et SQL Server 2016. Pour obtenir une vue d’ensemble des fonctionnalités et des avantages offerts par cette fonctionnalité, consultez [Fichiers de données SQL Server dans Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). Pour une démonstration en situation réelle, consultez [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(Démonstration de la restauration à un point dans le temps).  
  
  
**Télécharger**<br /><br />**>>**  Pour télécharger [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], accédez au  **[Centre d’évaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016)**.<br /><br />**>>**  Vous avez un compte Azure ?  Cliquez **[ici](https://azure.microsoft.com/en-us/services/virtual-machines/sql-server/)** pour lancer une machine virtuelle déjà équipée de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] .  
  
## <a name="what-you-will-learn"></a>Contenu du didacticiel  
Ce didacticiel vous montre comment utiliser des fichiers de données SQL Server dans le service Stockage Blob Microsoft Azure en plusieurs leçons. Chaque leçon porte sur une tâche spécifique et les leçons doivent être effectuées dans l’ordre. Tout d’abord, vous allez apprendre à créer un conteneur de stockage d’objets Blob avec une stratégie d’accès stockée et une signature d’accès partagé. Ensuite, vous découvrirez comment créer des informations d’identification SQL Server pour intégrer SQL Server au stockage Azure. Ensuite, vous sauvegarderez une base de données dans l’espace de stockage d’objets Blob et la restaurerez dans une machine virtuelle Azure. Vous utiliserez ensuite la sauvegarde du journal des transactions d’instantanés de fichiers SQL Server 2016 pour effectuer une restauration à un point dans le temps et dans une base de données. Enfin, pour illustrer les sauvegardes d’instantanés de fichiers et leur utilisation, le didacticiel vous montrera comment utiliser des fonctions et procédures stockées système de métadonnées.  
  
Cet article suppose que les conditions suivantes sont remplies :  
  
-   Vous avez une instance locale de SQL Server 2016 avec une copie d’AdventureWorks2014 installée.  
  
-   Vous disposez d’un compte de stockage Azure.  
  
-   Vous avez au moins une machine virtuelle Azure dotée de SQL Server 2016 et configurée comme indiqué dans [Approvisionnement d’une machine virtuelle SQL Server dans le portail Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-provision-sql-server/). Si vous le souhaitez, vous pouvez utiliser une seconde machine virtuelle pour le scénario de la [Leçon 8. Restaurer une base de données en tant que nouvelle base de données à partir de la sauvegarde de journal](../relational-databases/lesson-8-restore-as-new-database-from-log-backup.md).  
  
Ce didacticiel est divisé en neuf leçons, que vous devez effectuer dans l’ordre :  
  
**[Leçon 1 : Créer une stratégie d’accès stockée et une signature d’accès partagé sur un conteneur Azure](../relational-databases/lesson-1-create-stored-access-policy-and-shared-access-signature.md)**  
Dans cette leçon, vous créez une stratégie sur un conteneur d’objets blob et générez une signature d’accès partagé à utiliser pour créer des informations d’identification SQL Server.  
  
**[Leçon 2 : Créer des informations d’identification SQL Server à l’aide d’une signature d’accès partagé](../relational-databases/lesson-2-create-a-sql-server-credential-using-a-shared-access-signature.md)**  
Dans cette leçon, vous créez des informations d’identification à l’aide d’une clé SAP afin de stocker les informations de sécurité utilisées pour accéder au compte de stockage Microsoft Azure.  
  
**[Leçon 3 : Sauvegarder une base de données vers une URL](../relational-databases/lesson-3-database-backup-to-url.md)**  
Dans cette leçon, vous sauvegardez une base de données locale vers un espace de stockage d’objets Blob Microsoft Azure.  
  
**[Leçon 4 : Restaurer une base de données sur une machine virtuelle à partir d’une URL](../relational-databases/lesson-4-restore-database-to-virtual-machine-from-url.md)**  
Dans cette leçon, vous restaurez une base de données sur une machine virtuelle Azure à partir d’un espace de stockage d’objets Microsoft Azure.  
  
**[Leçon 5 : Sauvegarder une base de données à l’aide d’une sauvegarde d’instantanés de fichiers](../relational-databases/lesson-5-backup-database-using-file-snapshot-backup.md)**  
Dans cette leçon, vous sauvegardez une base de données à l’aide d’une sauvegarde d’instantanés de fichiers et de métadonnées d’instantané de l’affichage des fichiers.  
  
**[Leçon 6 : Générer un journal d’activité et de sauvegarde à l’aide d’une sauvegarde d’instantanés de fichiers](../relational-databases/lesson-6-generate-activity-and-backup-log-using-file-snapshot-backup.md)**  
Dans cette leçon, vous générez une activité dans la base de données et effectuez plusieurs sauvegardes de journal à l’aide de la sauvegarde d’instantanés de fichiers et des métadonnées d’instantané de l’affichage des fichiers.  
  
**[Leçon 7 : Restaurer une base de données à un point dans le temps](../relational-databases/lesson-7-restore-a-database-to-a-point-in-time.md)**  
Dans cette leçon, vous restaurez une base de données à un point dans le temps à l’aide de deux sauvegardes de journaux d’instantanés de fichiers.  
  
**[Leçon 8. Restaurer une base de données en tant que nouvelle base de données à partir de la sauvegarde de journal](../relational-databases/lesson-8-restore-as-new-database-from-log-backup.md)**  
Dans cette leçon, vous effectuez une restauration à partir d’une sauvegarde de journal d’instantanés de fichiers dans une nouvelle base de données sur une autre machine virtuelle.  
  
**[Leçon 9 : Gérer des jeux de sauvegarde et des sauvegardes d’instantanés de fichiers](../relational-databases/lesson-9-manage-backup-sets-and-file-snapshot-backups.md)**  
Dans cette leçon, vous supprimez les jeux de sauvegarde inutiles et découvrez comment supprimer les sauvegardes d’instantanés de fichiers orphelines (le cas échéant).  
  
## <a name="see-also"></a> Voir aussi  
[Fichiers de données SQL Server dans Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
[Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
[Sauvegarde SQL Server vers une URL](../relational-databases/backup-restore/sql-server-backup-to-url.md)  
  
  
  

