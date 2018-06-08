---
title: 'Leçon 3 : Sauvegarder une base de données vers une URL | Microsoft Docs'
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
ms.assetid: a9ae1501-b614-49d3-b975-6569da8350b2
caps.latest.revision: 16
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: c04ca530ed9e01a7a5ec01e5fe7bcd31af78b41c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-3-database-backup-to-url"></a>Leçon 3 : Sauvegarder une base de données dans une URL
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Dans cette leçon, vous sauvegardez la base de données AdventureWorks2014 de votre instance SQL Server 2016 locale dans le conteneur Azure que vous avez créé à la [Leçon 1 : Créer une stratégie d’accès stockée et une signature d’accès partagé sur un conteneur Azure](../relational-databases/lesson-1-create-stored-access-policy-and-shared-access-signature.md).  
  
> [!NOTE]  
> Si vous souhaitez effectuer une sauvegarde d’une base de données SQL Server 2012 SP1 CU2 ou version ultérieure ou d’une base de données SQL Server 2014 dans ce conteneur Azure, vous pouvez utiliser la syntaxe déconseillée décrite [ici](https://technet.microsoft.com/en-US/library/dn435916(v=sql.120).aspx) pour sauvegarder dans une URL à l’aide de la syntaxe WITH CREDENTIALS.  
  
Pour sauvegarder une base de données dans un stockage Blob, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure.  
  
3.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés à la Leçon 1, puis exécutez ce script.  
  
    ```  
  
    -- To permit log backups, before the full database backup, modify the database to use the full recovery model.  
    USE master;  
    ALTER DATABASE AdventureWorks2014  
       SET RECOVERY FULL;  
  
    -- Back up the full AdventureWorks2014 database to the container that you created in Lesson 1  
    BACKUP DATABASE AdventureWorks2014   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_onprem.bak'  
  
    ```  
  
4.  Ouvrez l’Explorateur d’objets et connectez-vous au stockage Azure à l’aide de votre compte de stockage et de la clé de compte.  
  
5.  Développez Conteneurs, développez le conteneur que vous avez créé à la Leçon 1 et vérifiez que la sauvegarde de l’étape 3 ci-dessus s’affiche dans ce conteneur.  
  
    ![Le fichier de sauvegarde local s’affiche sous la forme d’un objet blob dans le conteneur Azure local](../relational-databases/media/0d060e51-012f-4c61-ab8d-16d461d0ffad.JPG "Le fichier de sauvegarde local s’affiche sous la forme d’un objet blob dans le conteneur Azure local")  
  
**Leçon suivante :**  
  
[Leçon 4 : Restaurer une base de données sur une machine virtuelle à partir d’une URL](../relational-databases/lesson-4-restore-database-to-virtual-machine-from-url.md)  
  
