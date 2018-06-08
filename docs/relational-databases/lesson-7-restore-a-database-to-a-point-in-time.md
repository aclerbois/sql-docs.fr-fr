---
title: 'Leçon 7 : Restaurer une base de données à un point dans le temps | Microsoft Docs'
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
ms.assetid: a9f99670-e1de-441e-972c-69faffcac17a
caps.latest.revision: 13
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 3659e3be185c6148b7688a070fa162a877e56692
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-7-restore-a-database-to-a-point-in-time"></a>Leçon 7 : Restaurer une base de données à un point dans le temps
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette leçon, vous restaurez la base de données AdventureWorks2014 à un point dans le temps entre deux sauvegardes de journal des transactions.  
  
Avec les sauvegardes traditionnelles, pour obtenir une restauration à un point dans le temps, vous deviez utiliser la sauvegarde complète de la base de données, peut-être une sauvegarde différentielle, et tous les fichiers journaux des transactions jusqu’au point dans le temps auquel vous vouliez effectuer la restauration et juste après. Avec les sauvegardes d’instantanés de fichiers, vous n’avez besoin que des deux fichiers adjacents de sauvegarde du journal qui fournissent les limites ciblées encadrant le point dans le temps auquel vous voulez effectuer la restauration. Vous n’avez besoin que de deux jeux de sauvegarde d’instantanés de fichiers journaux, car chaque sauvegarde de journal crée un instantané de chaque fichier de base de données (chaque fichier de données et le fichier journal).  
  
Pour restaurer une base de données à un point spécifié dans le temps à partir de jeux de sauvegarde d’instantanés de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure.  
  
3.  Copiez, collez et exécutez le script Transact-SQL suivant dans la fenêtre de requête. Vérifiez que la table Production.Location a 29 939 lignes avant de la restaurer à un point dans le temps (il y a moins de lignes à l’étape 5).  
  
    ```  
  
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location  
  
    ```  
  
    ![Un nombre de lignes de 29 939 est affiché](../relational-databases/media/5e2f4229-1970-49c9-89b3-e96b6f7fde83.JPG "Un nombre de lignes de 29 939 est affiché")  
  
4.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Sélectionnez deux fichiers de sauvegarde de journal adjacents et remplacez le nom de fichier par la date et l’heure dont vous avez besoin pour ce script. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés à la Leçon 1, fournissez les noms des premier et deuxième fichiers de sauvegarde du journal, fournissez l’heure STOPAT au format « June 26, 2015 01:48 PM », puis exécutez ce script. Le script s’exécute pendant quelques minutes  
  
    ```  
  
    -- restore and recover to a point in time between the times of two transaction log backups, and then verify the row count  
    ALTER DATABASE AdventureWorks2014 SET SINGLE_USER WITH ROLLBACK IMMEDIATE;  
    RESTORE DATABASE AdventureWorks2014   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<firstbackupfile>.bak'   
       WITH NORECOVERY,REPLACE;  
    RESTORE LOG AdventureWorks2014   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<secondbackupfile>.bak'    
       WITH RECOVERY, STOPAT = 'June 26, 2015 01:48 PM';  
    ALTER DATABASE AdventureWorks2014 set multi_user;  
    -- get new count  
    SELECT COUNT (*) FROM AdventureWorks2014.Production.Location ;  
  
    ```  
  
5.  Examinez la sortie. Notez qu’après la restauration, le nombre de lignes est 18 389, nombre qui se situe entre les sauvegardes de journal 5 et 6 (le nombre de lignes peut varier).  
  
    ![Volet de résultats affichant le nombre de lignes après le point de restauration dans le temps](../relational-databases/media/4a0c6d8b-e2ed-4e93-bd7a-ade22a4aecc6.JPG "Volet de résultats affichant le nombre de lignes après le point de restauration dans le temps")  
  
**Leçon suivante :**  
  
[Leçon 8. Restaurer une base de données en tant que nouvelle base de données à partir de la sauvegarde de journal](../relational-databases/lesson-8-restore-as-new-database-from-log-backup.md)  
  
  
  
