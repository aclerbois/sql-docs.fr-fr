---
title: 'Leçon 4 : Restaurer une base de données sur une machine virtuelle à partir d’une URL | Microsoft Docs'
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
ms.assetid: ba793c8f-665a-4c46-b68d-f558a37906b2
caps.latest.revision: 23
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 452839bea162d97482724acee3584481737cc53a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-4-restore-database-to-virtual-machine-from-url"></a>Leçon 4 : Restaurer une base de données sur une machine virtuelle à partir d’une URL
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette leçon, vous restaurez la base de données AdventureWorks2014 sur votre instance de SQL Server 2016 sur votre machine virtuelle Azure.
  
> [!NOTE]  
> Pour simplifier ce didacticiel, nous utilisons le même conteneur pour les fichiers journaux et les données que celui que nous avons utilisé pour la sauvegarde de base de données. Dans un environnement de production, vous utilisez probablement plusieurs conteneurs et souvent plusieurs fichiers de données. Avec SQL Server 2016, vous pouvez également envisager de répartir votre sauvegarde sur plusieurs objets blob pour augmenter les performances de sauvegarde quand vous sauvegardez une base de données volumineuse.  
  
Pour restaurer la base de données SQL Server 2014 à partir d’un stockage Blob Azure sur votre instance de SQL Server 2016 sur votre machine virtuelle, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure.  
  
3.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés à la Leçon 1, puis exécutez ce script.  
  
    ```  
  
    -- Restore AdventureWorks2014 from URL to SQL Server instance using Azure blob storage for database files  
    RESTORE DATABASE AdventureWorks2014   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_onprem.bak'   
       WITH  
          MOVE 'AdventureWorks2014_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_Data.mdf'  
         ,MOVE 'AdventureWorks2014_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_Log.ldf'  
    --, REPLACE  
  
    ```  
  
4.  Ouvrez l’Explorateur d’objets et connectez-vous à votre instance Azure SQL Server 2016.  
  
5.  Dans l’Explorateur d’objets, développez le nœud Bases de données et vérifiez que la base de données AdventureWorks2014 a été restaurée (actualisez le nœud si nécessaire).  
  
    ![Base de données Adventure Works 2014 restaurée dans SQL Server 2016 sur la machine virtuelle](../relational-databases/media/311f69a6-8443-4df5-8f30-3103c2472300.JPG "Base de données Adventure Works 2014 restaurée dans SQL Server 2016 sur la machine virtuelle")  
  
6.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur AdventureWorks2014, puis cliquez sur Propriétés (cliquez sur Annuler une fois terminé).  
  
7.  Cliquez sur Fichiers et vérifiez que le chemin des deux fichiers de base de données sont des URL qui pointent sur des objets blob dans votre conteneur d’objets blob Azure.  
  
    ![Propriétés affichant le chemin d’accès des fichiers de données logique en tant qu’URL de base de données](../relational-databases/media/cfeee576-6319-460e-9fa2-f0922e02ee23.JPG "propriétés affichant le chemin d’accès des fichiers de données logique en tant qu’URL de base de données")  
  
8.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.  
  
9. Développez Conteneurs, développez le conteneur que vous avez créé à la Leçon 1 et vérifiez que les fichiers AdventureWorks2014_Data.mdf et AdventureWorks2014_Log.ldf de l’étape 3 ci-dessus s’affichent dans ce conteneur, ainsi que le fichier de sauvegarde de la Leçon 3 (actualisez le nœud si nécessaire).  
  
    ![Les données et le fichier journal Adventure Works 2014 s’affichent sous la forme d’objets BLOB dans le conteneur Azure](../relational-databases/media/156c7d73-44be-4754-9653-04cccb6c3066.JPG "Les données et le fichier journal Adventure Works 2014 s’affichent sous la forme d’objets BLOB dans le conteneur Azure")  
  
**Leçon suivante :**  
  
[Leçon 5 : Sauvegarder une base de données à l’aide d’une sauvegarde d’instantanés de fichiers](../relational-databases/lesson-5-backup-database-using-file-snapshot-backup.md)  
  
  
  
