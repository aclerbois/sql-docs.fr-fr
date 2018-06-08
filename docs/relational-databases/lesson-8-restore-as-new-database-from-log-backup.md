---
title: Leçon 8. Restaurer une base de données sous la forme d’une nouvelle base de données à partir de la sauvegarde de journal | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
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
ms.assetid: ebba12c7-3d13-4c9d-8540-ad410a08356d
caps.latest.revision: 12
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 14d3ac3a941f9d0bd23931673255083b1e113979
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-8-restore-as-new-database-from-log-backup"></a>Leçon 8. Restaurer une base de données en tant que nouvelle base de données à partir de la sauvegarde de journal
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette leçon, vous allez restaurer la base de données AdventureWorks2014 en tant que nouvelle base de données à partir d’une sauvegarde de journal des transactions d’instantanés de fichiers.  
  
Dans ce scénario, vous effectuez une restauration vers une instance de SQL Server sur une machine virtuelle différente à des fins d’analyse des activités et de création de rapports. La restauration vers une autre instance sur une autre machine virtuelle permet de déplacer la charge de travail vers une machine virtuelle dédiée et dimensionnée à cet effet, et dont les besoins en ressources n’affectent pas le système transactionnel.  
  
Effectuer une restauration à partir d’une sauvegarde de journal des transactions avec une sauvegarde d’instantanés de fichiers est très rapide, notamment par rapport aux sauvegardes en continu traditionnelles. Dans le cas des sauvegardes en continu traditionnelles, vous devez utiliser la sauvegarde complète de la base de données, éventuellement une sauvegarde différentielle, et tout ou partie des sauvegardes du journal des transactions (ou une nouvelle sauvegarde complète de la base de données). Toutefois, dans le cas des sauvegardes de journaux d’instantanés de fichiers, vous avez uniquement besoin de la dernière sauvegarde de journal (ou toute autre sauvegarde de journal ou deux sauvegardes de journaux voisines pour une restauration à un point dans le temps situé entre deux heures de sauvegarde de journal). En clair, vous avez besoin d’un seul jeu de sauvegarde d’instantanés de fichiers journaux, car chaque sauvegarde de journal d’instantanés de fichiers crée un instantané de fichier de chaque fichier de base de données (chaque fichier de données et le fichier journal).  
  
Pour restaurer une base de données dans une nouvelle base de données à partir d’une sauvegarde du journal des transactions à l’aide de la sauvegarde d’instantanés de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données dans une machine virtuelle Azure.  
  
    > [!NOTE]  
    > S’il s’agit d’une machine virtuelle Azure différente de celle que vous avez utilisée pour les leçons précédentes, assurez-vous d’avoir suivi les étapes de la [Leçon 2 : Créer des informations d’identification SQL Server à l’aide d’une signature d’accès partagé](../relational-databases/lesson-2-create-a-sql-server-credential-using-a-shared-access-signature.md). Si vous souhaitez effectuer une restauration vers un autre conteneur, suivez les étapes de la [Leçon 1 : Créer une stratégie d’accès stockée et une signature d’accès partagé sur un conteneur Azure](../relational-databases/lesson-1-create-stored-access-policy-and-shared-access-signature.md) et de la [Leçon 2 : Créer des informations d’identification SQL Server à l’aide d’une signature d’accès partagé](../relational-databases/lesson-2-create-a-sql-server-credential-using-a-shared-access-signature.md) pour le nouveau conteneur.  
  
3.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Sélectionnez le fichier de sauvegarde de journal à utiliser. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés à la leçon 1, fournissez le nom du fichier de sauvegarde du journal, puis exécutez ce script.  
  
    ```  
  
    -- restore as a new database from a transaction log backup file  
    RESTORE DATABASE AdventureWorks2014_EOM   
        FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<logbackupfile.bak'    
        WITH MOVE 'AdventureWorks2014_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Data.mdf'  
       , MOVE 'AdventureWorks2014_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Log.ldf'  
       , RECOVERY  
    --, REPLACE  
  
    ```  
  
4.  Examinez la sortie pour vérifier que la restauration a réussi.  
  
5.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.  
  
6.  Développez Conteneurs, développez le conteneur que vous avez créé à la leçon 1 (actualisez si nécessaire) et vérifiez que les nouveaux fichiers journaux et de données apparaissent dans le conteneur, ainsi que les objets blob issus des leçons précédentes.  
  
    ![Conteneur Azure affichant les données et les fichiers journaux de la nouvelle base de données](../relational-databases/media/e9705083-86bc-4309-a0bf-92c15f174c0a.JPG "Conteneur Azure affichant les fichiers de données et les fichiers journaux de la nouvelle base de données")  
  
[Leçon 9 : Gérer des jeux de sauvegarde et des sauvegardes d’instantanés de fichiers](../relational-databases/lesson-9-manage-backup-sets-and-file-snapshot-backups.md)  
  
  
  
