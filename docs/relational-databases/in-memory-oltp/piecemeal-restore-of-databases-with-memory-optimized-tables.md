---
title: Restauration fragmentaire de bases de données avec des tables mémoire optimisées | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 732c9721-8dd4-481d-8ff9-1feaaa63f84f
caps.latest.revision: 16
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b18a1c4ffebbfd0080c2be671dc50494878ac3d7
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="piecemeal-restore-of-databases-with-memory-optimized-tables"></a>Restauration fragmentaire de bases de données avec des tables mémoire optimisées
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La restauration fragmentaire est prise en charge sur les bases de données avec des tables mémoire optimisées, à l'exception d'une restriction décrite ci-dessous. Pour plus d’informations sur la sauvegarde et restauration fragmentaires, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md) et [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
 Un groupe de fichiers mémoire optimisé doit être sauvegardé et restauré avec le groupe de fichiers principal.  
  
-   Si vous sauvegardez (ou restaurez) le groupe de fichiers principal, vous devez spécifier le groupe de fichiers mémoire optimisé.  
  
-   Si vous sauvegardez (ou restaurez) le groupe de fichiers mémoire optimisé, vous devez spécifier le groupe de fichiers principal.  
  
 Principaux scénarios de sauvegarde et de restauration fragmentaires  
  
-   La sauvegarde fragmentaire vous permet de réduire la taille de la sauvegarde. Exemples :  
  
    -   Configurez la sauvegarde de base de données de façon à ce qu'elle se produise à différentes heures ou différents jours pour réduire l'impact sur la charge de travail. Par exemple, une base de données très volumineuse (de plus de 1 To) où une sauvegarde complète de la base de données ne peut pas être effectuée dans le temps alloué pour la maintenance de la base de données. Dans ce cas, utilisez une sauvegarde fragmentaire pour sauvegarder la base de données complète en plusieurs sauvegardes fragmentaires.  
  
    -   Si un groupe de fichiers est indiqué comme étant en lecture seule, il ne nécessite pas de sauvegarde du journal des transactions après qu'il a été marqué en lecture seule. Vous pouvez choisir de sauvegarder le groupe de fichiers une seule fois après son marquage en lecture seule.  
  
-   Restauration fragmentaire.  
  
    -   Le but d'une restauration fragmentaire consiste à mettre en ligne les parties critiques de base de données sans attendre toutes les données. Par exemple, si une base de données contient des données partitionnées, les partitions les plus anciennes sont rarement utilisées. Vous pouvez les restaurer uniquement si nécessaire. Il en va de même pour les groupes de fichiers qui contiennent, par exemple, des données d'historique.  
  
    -   Utilisez la réparation de page pour résoudre l'altération de page lors de la restauration spécifique de la page. Pour plus d’informations, consultez [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).  
  
## <a name="samples"></a>Exemples  
 Les exemples utilisent le schéma suivant :  
  
```  
CREATE DATABASE imoltp  
ON PRIMARY (name = imoltp_primary1, filename = 'c:\data\imoltp_data1.mdf')  
LOG ON (name = imoltp_log, filename = 'c:\data\imoltp_log.ldf')  
GO  
  
ALTER DATABASE imoltp ADD FILE (name = imoltp_primary2, filename = 'c:\data\imoltp_data2.ndf')  
GO  
  
ALTER DATABASE imoltp ADD FILEGROUP imoltp_secondary  
ALTER DATABASE imoltp ADD FILE (name = imoltp_secondary, filename = 'c:\data\imoltp_secondary.ndf') TO FILEGROUP imoltp_secondary  
GO  
  
ALTER DATABASE imoltp ADD FILEGROUP imoltp_mod CONTAINS MEMORY_OPTIMIZED_DATA   
ALTER DATABASE imoltp ADD FILE (name='imoltp_mod1', filename='c:\data\imoltp_mod1') TO FILEGROUP imoltp_mod   
ALTER DATABASE imoltp ADD FILE (name='imoltp_mod2', filename='c:\data\imoltp_mod2') TO FILEGROUP imoltp_mod   
GO  
```  
  
### <a name="backup"></a>Sauvegarde  
 Cet exemple montre comment sauvegarder le groupe de fichiers principal et le groupe de fichiers mémoire optimisé. Vous devez spécifier le groupe de fichiers principal et le groupe de fichiers mémoire optimisé.  
  
```  
backup database imoltp filegroup='primary', filegroup='imoltp_mod' to disk='c:\data\imoltp.dmp' with init  
```  
  
 L'exemple suivant montre qu'une sauvegarde d'un ou plusieurs groupes de fichiers autres que le groupe de fichiers principal et le groupe de fichiers mémoire optimisé est similaire dans les bases de données sans tables mémoire optimisées. La commande suivante permet de sauvegarder le groupe de fichiers secondaire  
  
```  
backup database imoltp filegroup='imoltp_secondary' to disk='c:\data\imoltp_secondary.dmp' with init  
```  
  
### <a name="restore"></a>Restaurer  
 Cet exemple montre comment restaurer le groupe de fichiers principal et le groupe de fichiers mémoire optimisé ensemble.  
  
```  
restore database imoltp filegroup = 'primary', filegroup = 'imoltp_mod'   
from disk='c:\data\imoltp.dmp' with partial, norecovery  
  
--restore the transaction log  
 RESTORE LOG [imoltp] FROM DISK = N'c:\data\imoltp_log.dmp' WITH  FILE = 1,  NOUNLOAD,  STATS = 10  
GO  
```  
  
 L'exemple suivant montre que la restauration d'un ou plusieurs groupes de fichiers autres que le groupe de fichiers principal et le groupe de fichiers mémoire optimisé est similaire dans les bases de données sans tables mémoire optimisées.  
  
```  
RESTORE DATABASE [imoltp] FILE = N'imoltp_secondary'   
FROM  DISK = N'c:\data\imoltp_secondary.dmp' WITH  FILE = 1,  RECOVERY,  NOUNLOAD,  STATS = 10  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarder, restaurer et récupérer des tables optimisées en mémoire](http://msdn.microsoft.com/library/3f083347-0fbb-4b19-a6fb-1818d545e281)  
  
  
