---
title: 'Leçon 6 : Générer un journal d’activité et de sauvegarde à l’aide d’une sauvegarde d’instantanés de fichiers | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
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
ms.assetid: 26aa534a-afe7-4a14-b99f-a9184fc699bd
caps.latest.revision: 15
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 282f88a0b41749a4e38d73285a794e357155d9d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-6-generate-activity-and-backup-log-using-file-snapshot-backup"></a>Leçon 6 : Générer un journal d’activité et de sauvegarde à l’aide d’une sauvegarde d’instantanés de fichiers
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette leçon, vous allez générer une activité dans la base de données AdventureWorks2014 et créer régulièrement des sauvegardes du journal des transactions à l’aide de sauvegardes d’instantanés de fichiers. Pour plus d’informations sur l’utilisation de sauvegardes d’instantanés de fichiers, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Pour générer une activité dans la base de données AdventureWorks2014 et créer régulièrement des sauvegardes du journal des transactions à l’aide de sauvegardes d’instantanés de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
  
2.  Ouvrez deux nouvelles fenêtres de requête et connectez chacune d’elles à l’instance SQL Server 2016 du moteur de base de données dans votre machine virtuelle Azure.  
  
3.  Copiez, collez et exécutez le script Transact-SQL suivant dans l’une des fenêtres de requête. Notez que la table Production.Location contient 14 lignes avant que nous ajoutions de nouvelles lignes à l’étape 4.  
  
    ```  
  
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location;  
  
    ```  
  
4.  Copiez et collez les deux scripts Transact-SQL suivants dans les deux fenêtres de requête. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifié à la leçon 1, puis exécutez ces scripts simultanément dans les fenêtres de requête. L’exécution de ces scripts prendra environ sept minutes.  
  
    ```  
    -- Insert 30,000 new rows into the Production.Location table in the AdventureWorks2014 database in batches of 75  
    DECLARE @count INT=1, @inner INT;  
    WHILE @count < 400  
       BEGIN  
          BEGIN TRAN;  
             SET @inner =1;  
                WHILE @inner <= 75  
                   BEGIN;  
                      INSERT INTO AdventureWorks2014.Production.Location    
                         (Name, CostRate, Availability, ModifiedDate)   
                            VALUES (NEWID(), .5, 5.2, GETDATE());  
                      SET @inner = @inner + 1;  
                   END;  
          COMMIT;  
       WAITFOR DELAY '00:00:01';   
       SET @count = @count + 1;  
       END;  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location;  
  
    ```  
  
    ```  
    --take 7 transaction log backups with FILE_SNAPSHOT, one per minute, and include the row count and the execution time in the backup file name   
    DECLARE @count INT=1, @device NVARCHAR(120), @numrows INT;  
    WHILE @count <= 7  
       BEGIN  
             SET @numrows = (SELECT COUNT (*) FROM AdventureWorks2014.Production.Location);  
             SET @device = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-' + CONVERT (varchar(10),@numrows) + '-' + FORMAT(GETDATE(), 'yyyyMMddHHmmss') + '.bak';  
             BACKUP LOG AdventureWorks2014 TO URL = @device WITH FILE_SNAPSHOT;  
             SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2014');  
          WAITFOR DELAY '00:1:00';   
             SET @count = @count + 1;  
       END;  
    ```  
  
5.  Examinez la sortie du premier script ; le nombre de lignes final est désormais 29 939.  
  
    ![Un nombre de lignes de 29 939 est affiché](../relational-databases/media/5e2f4229-1970-49c9-89b3-e96b6f7fde83.JPG "Un nombre de lignes de 29 939 est affiché")  
  
6.  Examinez la sortie du deuxième script ; chaque fois que l’instruction BACKUP LOG est exécutée, deux instantanés de fichiers sont créés, à savoir un instantané de fichier du fichier journal et un instantané de fichier du fichier de données, soit un total de deux instantanés de fichiers par fichier de base de données. Une fois le deuxième script terminé, il existe un total de 16 instantanés de fichiers, à raison de 8 par fichier de base de données (un issu de l’instruction BACKUP DATABASE et un pour chaque exécution de l’instruction BACKUP LOG).  
  
    ![volet de résultats affichant les captures instantanées de fichier des fichiers de données et journal lors de la sauvegarde du journal](../relational-databases/media/acd213b8-895e-425c-bd72-2bf10e65a5ba.JPG "volet de résultats affichant les captures instantanées de fichier des fichiers de données et journal lors de la sauvegarde du journal")  
  
    ![quatre captures instantanées de fichier sont affichées](../relational-databases/media/e7eff77d-85b9-4e52-abd8-e49952c8118a.JPG "quatre captures instantanées de fichier sont affichées")  
  
    ![volet de résultats montrant un total de 16 captures instantanées de fichier](../relational-databases/media/c3ddff17-a83c-4bf0-a670-a38834f9c922.JPG "volet de résultats montrant un total de 16 captures instantanées de fichier")  
  
7.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.  
  
8.  Développez Conteneurs, développez le conteneur que vous avez créé à la leçon 1 et vérifiez que 7 nouveaux fichiers de sauvegarde apparaissent, ainsi que les objets blob issus des leçons précédentes (actualisez le nœud si nécessaire).  
  
    ![Conteneur Azure montrant 7 objets blob de sauvegarde de journal](../relational-databases/media/cfa5a326-87a2-4202-9a04-38bf577d2d0b.JPG "Conteneur Azure montrant 7 objets blob de sauvegarde de journal")  
  
**Leçon suivante :**  
  
[Leçon 7 : Restaurer une base de données à un point dans le temps](../relational-databases/lesson-7-restore-a-database-to-a-point-in-time.md)  
  
  
  
