---
title: Propriétés de la base de données (page Groupes de fichiers) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.filegroups.f1
ms.assetid: 8d06e859-73dd-4019-b6e8-99c5c5297697
caps.latest.revision: 31
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: d8902da97f015bad1f548fb3aaf3d23b7d108aec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-properties-filegroups-page"></a>Propriétés de la base de données (page Groupes de fichiers)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette page vous permet d'afficher les groupes de fichiers existants ou d'ajouter un nouveau groupe de fichiers à la base de données sélectionnée. Il existe trois types de groupes de fichiers : les groupes de fichiers de *ligne* , les groupes de fichiers de données FILESTREAM et les groupes de fichiers optimisés en mémoire.  
  
 Les groupes de fichiers de ligne contiennent des données régulières et des fichiers journaux. Les groupes de fichiers de données FILESTREAM contiennent des fichiers de données FILESTREAM. Ces fichiers de données comportent des informations sur la manière dont les données des objets BLOB (Binary Large Objects) sont stockées sur le système de fichiers lorsque vous utilisez le stockage FILESTREAM. Les deux types de groupes de fichiers disposent des mêmes options.  
  
 Si FILESTREAM n'est pas activé, la section **Filestream** ne sera pas disponible. Vous pouvez activer le stockage FILESTREAM dans la boîte de dialogue [Propriétés du serveur (page Avancé)](../../database-engine/configure-windows/server-properties-advanced-page.md).  
  
 Pour plus d’informations sur la manière dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les groupes de fichiers de ligne, consultez [Groupes de fichiers et fichiers de base de données](../../relational-databases/databases/database-files-and-filegroups.md). Pour plus d’informations sur les données et groupes de fichiers FILESTREAM, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
 Les groupes de fichiers optimisés en mémoire sont nécessaires pour toute base de données devant contenir une ou plusieurs tables optimisées en mémoire.  
  
## <a name="row-and-filestream-data-filegroup-options"></a>Options des groupes de fichiers de ligne et de données FILESTREAM  
 **Nom**  
 Entrez le nom du groupe de fichiers.  
  
 **Fichiers**  
 Indique le nombre de fichiers contenus dans le groupe de fichiers.  
  
 **Lecture seule**  
 Sélectionnez cette option pour attribuer au groupe de fichiers l'état lecture seule.  
  
 **Default**  
 Sélectionnez cette option pour définir ce groupe de fichiers comme groupe de fichiers par défaut. Vous pouvez définir un groupe de fichiers de ligne par défaut et un groupe de fichiers par défaut pour les données FILESTREAM.  
  
 **Ajouter**  
 Ajoute une nouvelle ligne vide dans la grille qui répertorie les groupes de fichiers associés à la base de données.  
  
 **Supprimer**  
 Supprime la ligne sélectionnée des groupes de fichiers de la grille.  
  
## <a name="memory-optimized-data-filegroup-options"></a>Options des groupes de fichiers optimisés en mémoire  
 **Nom**  
 Entrez le nom du groupe de fichiers optimisé en mémoire.  
  
 **Fichiers FILESTREAM**  
 Affiche le nombre de fichiers (conteneurs) figurant dans le groupe de fichiers de données optimisé en mémoire. Vous pouvez ajouter des conteneurs sur la page **Fichiers** .  
  
 **Ajouter**  
 Ajoute une nouvelle ligne vide dans la grille qui répertorie les groupes de fichiers associés à la base de données.  
  
 **Supprimer**  
 Supprime la ligne sélectionnée des groupes de fichiers de la grille.  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
  
