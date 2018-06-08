---
title: Sauvegarde d’une base de données avec des tables mémoire optimisées | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 83d47694-e56d-4dae-b54e-14945bf8ba31
caps.latest.revision: 18
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 35c700c5f9ada3fbe9426a1e7b99f6a791737e25
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="backing-up-a-database-with-memory-optimized-tables"></a>Sauvegarde d'une base de données avec des tables mémoire optimisées
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Les tables mémoire optimisées sont sauvegardées dans le cadre des sauvegardes de base de données normales. Pour les tables sur disque, la valeur CHECKSUM des paires de fichiers de données et delta est validée dans le cadre de la sauvegarde de base de données pour détecter la corruption du stockage.  
  
> [!NOTE]  
>  Lors d’une sauvegarde, si vous détectez une erreur CHECKSUM dans un ou plusieurs fichiers d’un groupe de fichiers à mémoire optimisée, la sauvegarde échoue. Dans ce cas, vous devez restaurer votre base de données à partir de la dernière sauvegarde connue et fiable.  
>   
>  Si vous ne disposez pas d'une sauvegarde, exportez des données des tables mémoire optimisées et des tables sur disque et rechargez-les après avoir supprimé et recréé la base de données.  
  
 Une sauvegarde complète d'une base de données avec une ou plusieurs tables mémoire optimisées comprend le stockage alloué pour les tables sur disque (le cas échéant), le journal des transactions actives, et les paires de fichiers de données et delta (également appelées des « paires de fichiers de point de contrôle ») pour les tables mémoire optimisées. Cependant, comme décrit dans [Durabilité pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/durability-for-memory-optimized-tables.md), le stockage utilisé par les tables optimisées en mémoire peut être bien supérieur à sa taille en mémoire, et cela a une incidence sur la taille de la sauvegarde de base de données.  
  
## <a name="full-database-backup"></a>Sauvegarde de base de données complète  
 Cette section aborde les sauvegardes de bases de données qui ne contiennent que des tables mémoire optimisées durables, car la sauvegarde de tables sur disque est identique. Les paires de fichiers de point de contrôle dans le groupe de fichiers mémoire optimisé peuvent avoir différents états. Le tableau ci-dessous indique la partie des fichiers qui est sauvegardée.  
  
|État de la paire de fichiers de point de contrôle|Backup|  
|--------------------------------|------------|  
|PRECREATED|Métadonnées du fichier uniquement|  
|UNDER CONSTRUCTION|Métadonnées du fichier uniquement|  
|ACTIVE|Métadonnées du fichier plus octets utilisés|  
|MERGE TARGET|Métadonnées du fichier uniquement|  
|WAITING FOR LOG TRUNCATION|Métadonnées du fichier plus octets utilisés|  
  
 Pour obtenir la description des états des paires de fichiers de point de contrôle, consultez [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) et sa colonne state_desc.  
  
 La taille des sauvegardes de base de données avec une ou plusieurs tables mémoire optimisées est généralement supérieure à leur taille en mémoire, mais inférieure à leur taille de stockage sur disque. Le supplément de taille varie en fonction de différents facteurs, notamment le nombre de lignes supprimées.  
  
### <a name="estimating-size-of-full-database-backup"></a>Estimation de la taille d'une sauvegarde complète de base de données  
  
> [!IMPORTANT]  
>  Il est recommandé de ne pas utiliser la valeur BackupSizeInBytes pour estimer la taille de sauvegarde de l’OLTP en mémoire.  
  
 Le premier scénario de charge de travail concerne l'insertion (pour la plupart). Dans ce scénario, la plupart des fichiers de données présentent l’état Actif, sont totalement chargés et comportent très peu de lignes supprimées. La taille de la sauvegarde de base de données est proche de celle des données en mémoire.  
  
 Le second scénario de charge de travail correspond à des opérations fréquentes d’insertion, de suppression et de mise à jour. Dans le pire des cas, chacune des paires de fichiers de point de contrôle sont chargées, à 50 % après prise en compte les lignes supprimées. La taille de la sauvegarde de base de données sera égale à au moins 2 fois la taille des données en mémoire.  
  
## <a name="differential-backups-of-databases-with-memory-optimized-tables"></a>Sauvegardes différentielles de bases de données avec des tables mémoire optimisées  
 Le stockage des tables optimisées en mémoire comprend des données et des fichiers delta, comme décrit dans [Durabilité pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/durability-for-memory-optimized-tables.md). La sauvegarde différentielle d'une base de données avec des tables mémoire optimisées contient les données suivantes :  
  
-   La sauvegarde différentielle pour les groupes de fichiers qui stockent des tables sur disque n'est pas affectée par la présence de tables mémoire optimisées.  
  
-   Le journal des transactions actif est le même que dans une sauvegarde de base de données complète.  
  
-   Pour un groupe de fichiers de données mémoire optimisé, la sauvegarde différentielle utilise le même algorithme que la sauvegarde de base de données complète pour identifier les fichiers de données et delta, mais filtre ensuite le sous-ensemble de fichiers comme suit :  
  
    -   Un fichier de données contient les nouvelles lignes insérées et, une fois plein, est fermé et marqué en lecture seule. Un fichier de données est sauvegardé uniquement s'il est fermé après la dernière sauvegarde complète de la base de données. La sauvegarde différentielle sauvegarde uniquement les fichiers de données contenant les lignes insérées depuis la dernière sauvegarde complète de la base de données. Une exception à cette règle existe dans le cadre d’un scénario de mise à jour et de suppression où certaines des lignes insérées peuvent déjà avoir été marquées pour le processus de garbage collection ou déjà avoir été soumises à cette opération.  
  
    -   Un fichier delta stocke les référence aux lignes de données supprimées. Étant donné qu'une transaction ultérieure peut supprimer une ligne, un fichier delta peut être modifié à tout moment au cours de sa durée de vie, et n'est jamais fermé. Un fichier delta est toujours sauvegardé. Les fichiers delta utilisent généralement moins de 10 % du stockage, par conséquent, ils ont un impact minime sur la taille de la sauvegarde différentielle.  
  
 Si les tables mémoire optimisées représentent une partie significative de la taille de votre base de données, la sauvegarde différentielle peut considérablement réduire la taille de votre sauvegarde. Pour les charges de travail OLTP typiques, les sauvegardes différentielles seront considérablement inférieures aux sauvegardes de base de données complètes.  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarder, restaurer et récupérer des tables optimisées en mémoire](http://msdn.microsoft.com/library/3f083347-0fbb-4b19-a6fb-1818d545e281)  
  
  
