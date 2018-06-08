---
title: Sauvegarder et restaurer des catalogues et des index de recherche en texte intégrals | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], backing up
- full-text search [SQL Server], back up and restore
- recovery [full-text search]
- backups [SQL Server], full-text indexes
- full-text indexes [SQL Server], restoring
- restore operations [full-text search]
ms.assetid: 6a4080d9-e43f-4b7b-a1da-bebf654c1194
caps.latest.revision: 62
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 92cfb3b9b67b622c82031dde7d43f497a2714d07
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="back-up-and-restore-full-text-catalogs-and-indexes"></a>Sauvegarder et restaurer des catalogues et des index de recherche en texte intégral
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment sauvegarder et des index de recherche en texte intégral créés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le catalogue de texte intégral est un concept logique qui ne réside pas dans un groupe de fichiers. Par conséquent, pour sauvegarder un catalogue de texte intégral dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez identifier tous les groupes de fichiers contenant un index de recherche en texte intégral qui appartiennent au catalogue. Vous devez ensuite enregistrer ces groupes de fichiers, un par un.  
  
> [!IMPORTANT]  
>  Il est possible d'importer des catalogues de texte intégral lors de la mise à niveau d'une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Chaque catalogue de texte intégral importé est un fichier de base de données dans son propre groupe de fichiers. Pour sauvegarder un catalogue importé, sauvegardez simplement son groupe de fichiers. Pour plus d’informations, consultez [Sauvegarde et restauration de catalogues de texte intégral](http://go.microsoft.com/fwlink/?LinkID=121052)dans la documentation en ligne de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
##  <a name="backingup"></a> Sauvegarde des index de recherche en texte intégral d'un catalogue de texte intégral  
  
###  <a name="Find_FTIs_of_a_Catalog"></a> Recherche des index de recherche en texte intégral d'un catalogue de texte intégral  
 Vous pouvez extraire les propriétés des index de recherche en texte intégral en utilisant l’instruction [SELECT](../../t-sql/queries/select-transact-sql.md) suivante, qui sélectionne des colonnes à partir des affichages catalogue [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) et [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) .  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @TableID int;  
SET @TableID = (SELECT OBJECT_ID('AdventureWorks2012.Production.Product'));  
SELECT object_name(@TableID), i.is_enabled, i.change_tracking_state,   
   i.has_crawl_completed, i.crawl_type, c.name as fulltext_catalog_name   
   FROM sys.fulltext_indexes i, sys.fulltext_catalogs c   
   WHERE i.fulltext_catalog_id = c.fulltext_catalog_id;  
GO  
```  
  
  
###  <a name="Find_FG_of_FTI"></a> Recherche du groupe de fichiers ou du fichier qui contient un index de recherche en texte intégral  
 Lorsqu'un index de recherche en texte intégral est créé, il est placé à l'un des emplacements suivants :  
  
-   Groupe de fichiers spécifié par l'utilisateur.  
  
-   Le même groupe de fichiers que la table de base ou la vue pour une table non partitionnée.  
  
-   Le groupe de fichiers primaire, pour une table partitionnée.  
  
> [!NOTE]  
>  Pour plus d’informations sur la création d’un index de texte intégral, consultez [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md) et [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
 Pour rechercher le groupe de fichiers de l’index de recherche en texte intégral sur une table ou une vue, utilisez la requête ci-dessous, où *nom_objet* est le nom de la table ou de la vue :  
  
```  
SELECT name FROM sys.filegroups f, sys.fulltext_indexes i   
   WHERE f.data_space_id = i.data_space_id   
      and i.object_id = object_id('object_name');  
GO  
  
```  
  
  
###  <a name="Back_up_FTIs_of_FTC"></a> Sauvegarde des groupes de fichiers qui contiennent des index de recherche en texte intégral  
 Après avoir recherché les groupes de fichiers qui contiennent les index d'un catalogue de texte intégral, vous devez sauvegarder chacun des groupes de fichiers. Durant le processus de sauvegarde, il est impossible de supprimer ou d'ajouter des catalogues de texte intégral.  
  
 La première sauvegarde d'un groupe de fichiers doit être une sauvegarde de fichiers complète. Après avoir créé une sauvegarde complète d'un fichier ou d'un groupe de fichiers, vous pouvez créer une série d'une ou de plusieurs sauvegardes de fichiers différentielles basées sur cette sauvegarde de fichiers complète.  
  
 **Pour sauvegarder les fichiers et groupes de fichiers**  
  
-   [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)  
  
  
##  <a name="Restore_FTI"></a> Restauration d'un index de recherche en texte intégral  
 La restauration d'un groupe de fichiers sauvegardés restaure les fichiers de l'index de recherche en texte intégral, ainsi que les autres fichiers dans le groupe de fichiers. Par défaut, le groupe de fichiers est restauré à l'emplacement du disque sur lequel le groupe de fichiers a été sauvegardé.  
  
 Si une table indexée de texte intégral était en ligne et qu'un remplissage s'exécutait lorsque la sauvegarde a été créée, le remplissage reprend après la restauration.  
  
 **Pour restaurer un groupe de fichiers**  
  
-   [Restaurer des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Restaurer des fichiers et groupes de fichiers en remplaçant des fichiers existants &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [Restaurer des fichiers à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer et surveiller la recherche en texte intégral pour une instance de serveur](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md)   
 [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md)  
  
  
