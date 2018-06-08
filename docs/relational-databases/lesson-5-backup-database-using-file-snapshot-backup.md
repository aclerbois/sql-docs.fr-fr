---
title: 'Leçon 5 : Sauvegarder une base de données à l’aide d’une sauvegarde d’instantanés de fichiers | Microsoft Docs'
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: d9134ade-7b03-4c5c-8ed3-3bc369a61691
caps.latest.revision: 19
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: f56a7cfbf4303ca03d3d6e5cda360643a892c35b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-5-backup-database-using-file-snapshot-backup"></a>Leçon 5 : Sauvegarder une base de données à l’aide d’une sauvegarde d’instantanés de fichiers
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette leçon, vous sauvegardez la base de données AdventureWorks2014 sur votre machine virtuelle Azure à l’aide de la sauvegarde d’instantanés de fichiers pour effectuer une sauvegarde quasi instantanée au moyen d’instantanés Azure. Pour plus d’informations sur les sauvegardes d’instantanés de fichiers, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
  
Pour sauvegarder la base de données AdventureWorks2014 à l’aide de la sauvegarde d’instantanés de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure.  
  
3.  Copiez, collez et exécutez le script Transact-SQL suivant dans la fenêtre de requête (ne fermez pas cette fenêtre de requête, vous réexécutez ce script à l’étape 5. Ce système de procédure stockées vous permet d’afficher les sauvegardes d’instantanés de fichiers existantes pour chaque fichier qui comprend une base de données spécifiée. Notez qu’il n’y a aucune sauvegarde d’instantanés de fichiers pour cette base de données.  
  
    ```  
  
    -- Verify that no file snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2014');  
  
    ```  
  
4.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés à la Leçon 1, puis exécutez ce script. Notez la rapidité d’exécution de cette sauvegarde.  
  
    ```  
  
    -- Backup the AdventureWorks2014 database with FILE_SNAPSHOT  
    BACKUP DATABASE AdventureWorks2014   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_Azure.bak'   
       WITH FILE_SNAPSHOT;  
  
    ```  
  
    ![Volet de résultats affichant des captures instantanées de fichiers pour chaque fichier de base de données](../relational-databases/media/2a9320e0-067a-485a-8e0e-636660005e5c.JPG "Volet de résultats affichant des captures instantanées de fichiers pour chaque fichier de base de données")  
  
5.  Après avoir vérifié que le script de l’étape 4 a été correctement exécuté, réexécutez le script suivant. Notez que l’opération de sauvegarde d’instantanés de fichiers de l’étape 4 a généré des instantanés de fichiers des données et du fichier journal.  
  
    ```  
  
    -- Verify that two file-snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2014');  
  
    ```  
  
    ![Résultats de la fonction sys.fn_db_backup_file_snapshots affichant des 2 instantanés](../relational-databases/media/fca1436e-9607-4432-97ee-f66ac2f2108d.JPG "Résultats de la fonction sys.fn_db_backup_file_snapshots affichant des 2 instantanés")  
  
6.  Dans l’Explorateur d’objets, dans l’instance SQL Server 2016 sur votre machine virtuelle Azure, développez le nœud Bases de données et vérifiez que la base de données AdventureWorks2014 a été restaurée dans cette instance (actualisez le nœud si nécessaire).  
  
7.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.  
  
8.  Développez Conteneurs, développez le conteneur que vous avez créé dans la Leçon 1 et vérifiez que le fichier AdventureWorks2014_Azure.bak de l’étape 4 ci-dessus s’affiche dans ce conteneur, ainsi que le fichier de sauvegarde de la leçon 3 et les fichiers de base de données de la leçon 4 (actualisez le nœud si nécessaire).  
  
    ![La sauvegarde d’instantanés de fichiers s’affiche dans le conteneur Azure](../relational-databases/media/181bc970-4af7-4272-a9ae-4bef674f2e02.JPG "La sauvegarde d’instantanés de fichiers s’affiche dans le conteneur Azure")  
  
**Leçon suivante :**  
  
[Leçon 6 : Générer un journal d’activité et de sauvegarde à l’aide d’une sauvegarde d’instantanés de fichiers](../relational-databases/lesson-6-generate-activity-and-backup-log-using-file-snapshot-backup.md)  
  
## <a name="see-also"></a> Voir aussi  
[Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
[sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)  
  
  
  
