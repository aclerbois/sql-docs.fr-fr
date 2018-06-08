---
title: Afficher la taille du fichier partiellement alloué d’un instantané de base de données (Transact-SQL) | Microsoft Docs
ms.date: 07/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server database snapshots], sparse files
- space [SQL Server], sparse files
- sparse files [SQL Server]
- size [SQL Server], sparse files
- maximum sparse file size
- database snapshots [SQL Server], sparse files
- space [SQL Server], database snapshots
ms.assetid: 1867c5f8-d57c-46d3-933d-3642ab0a8e24
caps.latest.revision: 41
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 580026cb944a7b63d1970af34c6218b0d70147f1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql"></a>Afficher la taille du fichier partiellement alloué d'un instantané de base de données (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment utiliser [!INCLUDE[tsql](../../includes/tsql-md.md)] pour vérifier qu'un fichier de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un fichier partiellement alloué et pour déterminer ses tailles réelle et maximale. Les fichiers partiellement alloués, qui sont une fonctionnalité du système de fichiers NTFS, sont utilisés par les instantanés de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Pendant la création d'un instantané de base de données, ces fichiers partiellement alloués sont créés et nommés par l'instruction CREATE DATABASE. Ces noms de fichiers sont stockés dans **sys.master_files** dans la colonne **physical_name** . Dans **sys.database_files** (dans la base de données source ou dans l’instantané), la colonne **physical_name** contient toujours les noms des fichiers de la base de données source.  
  
## <a name="verify-that-a-database-file-is-a-sparse-file"></a>Vérifier qu'un fichier de base de données est un fichier partiellement alloué  
  
1.  Sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
     Sélectionnez la colonne **is_sparse** soit dans **sys.database_files** dans l’instantané de la base de données, soit dans **sys.master_files**. La valeur indique si le fichier est un fichier partiellement alloué, comme suit :  
  
     1 = le fichier est un fichier partiellement alloué.  
  
     0 = le fichier n'est pas un fichier partiellement alloué.  
  
## <a name="find-out-the-actual-size-of-a-sparse-file"></a>Pour connaître la taille réelle d'un fichier partiellement alloué  
  
> [!NOTE]  
>  La taille d’un fichier partiellement alloué augmente par incréments de 64 kilo-octets (Ko). Il s’agit donc toujours d’un multiple de 64 Ko.  
  
 Pour afficher le nombre d’octets utilisés actuellement sur le disque par chaque fichier partiellement alloué d’un instantané, interrogez la colonne **size_on_disk_bytes** de la vue de gestion dynamique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) .  
  
 Pour afficher l’espace disque occupé par un fichier partiellement alloué, cliquez avec le bouton droit sur le fichier dans Microsoft Windows, cliquez sur **Propriétés**et notez la valeur indiquée dans **Taille sur le disque** .  
  
## <a name="find-out-the-maximum-size-of-a-sparse-file"></a>Pour connaître la taille maximale d'un fichier partiellement alloué  
 La taille maximale d'un fichier partiellement alloué correspond à la taille du fichier de la base de données source au moment de la création de l'instantané. Pour connaître la taille de ce fichier, utilisez une des méthodes suivantes :  
  
-   Utilisation de l'invite de commandes Windows :  
  
    1.  Utilisez les commandes **dir** de Windows.  
  
    2.  Sélectionnez le fichier partiellement alloué, ouvrez la boîte de dialogue **Propriétés** dans Windows et relevez la valeur du champ **Taille** .  
  
-   Sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
     Sélectionnez la colonne **size** soit dans **sys.database_files** dans l’instantané de la base de données, soit dans **sys.master_files**. La valeur de la colonne **size** reflète l'espace maximal que l'instantané peut utiliser en pages SQL. Cette valeur correspond à celle du champ **Taille** de Windows, sauf qu'elle est exprimée en termes de nombre de pages SQL dans le fichier ; la taille en octets étant :  
  
     ( *nombre_de_pages* * 8192)  

## <a name="example"></a> Exemple
Le script suivant indique la taille du disque en kilooctets, pour chaque fichier partiellement alloué.  Le script affiche également la taille maximale en mégaoctets que peut atteindre un fichier partiellement alloué.  Exécutez le script Transact-SQL en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

```sql
SELECT  DB_NAME(sd.source_database_id) AS [SourceDatabase], 
        sd.name AS [Snapshot],
        mf.name AS [Filename], 
        size_on_disk_bytes/1024 AS [size_on_disk (KB)],
        mf2.size/128 AS [MaximumSize (MB)]
FROM sys.master_files mf
JOIN sys.databases sd
    ON mf.database_id = sd.database_id
JOIN sys.master_files mf2
    ON sd.source_database_id = mf2.database_id
    AND mf.file_id = mf2.file_id
CROSS APPLY sys.dm_io_virtual_file_stats(sd.database_id, mf.file_id)
WHERE mf.is_sparse = 1
AND mf2.is_sparse = 0
ORDER BY 1;
```
  
## <a name="see-also"></a> Voir aussi  
 [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)   
 [sys.fn_virtualfilestats &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-virtualfilestats-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
