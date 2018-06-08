---
title: Sauvegarder des bases de données Stretch (Stretch Database) | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, backing up
- backups (Stretch Database)
ms.assetid: 18f0dff0-d8ce-4bee-a935-76ed6dfb3208
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 512d4908213084555019c20628307309229d2b76
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772755"
---
# <a name="backup-stretch-enabled-databases-stretch-database"></a>Sauvegarder des bases de données Stretch (Stretch Database)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md)]


 Les sauvegardes de base de données vous aident à récupérer suite à de nombreux types d’échecs, d’erreurs et de sinistres.  
  
 -   Vous devez sauvegarder vos bases de données Stretch SQL Server.  
      
 -   Microsoft Azure sauvegarde automatiquement les données distantes que Stretch Database a migrées de SQL Server vers Azure.  

> [!TIP]
> La sauvegarde ne constitue qu’une partie d’une solution complète de haute disponibilité et de continuité d’activité. Pour plus d’informations sur la haute disponibilité, consultez [Solutions haute disponibilité](../../sql-server/failover-clusters/high-availability-solutions-sql-server.md).
   
## <a name="back-up-your-sql-server-data"></a>Sauvegarder vos données SQL Server  
  
Pour sauvegarder vos bases de données Stretch SQL Server, vous pouvez continuer à utiliser les méthodes de sauvegarde SQL Server que vous utilisez actuellement. Pour plus d’informations, consultez [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).
  
 Les sauvegardes d’une base de données Stretch SQL Server contiennent uniquement des données locales et des données éligibles à la migration à la limite dans le temps quand la sauvegarde s’exécute. (Les données éligibles sont des données qui n’ont pas encore été migrées, mais qui seront migrées vers Azure d’après les paramètres de migration des tables.) Il s’agit d’une sauvegarde **partielle**. Elle ne comprend pas les données déjà migrées vers Azure.  
  
## <a name="back-up-your-remote-azure-data"></a>Sauvegarder vos données Azure distantes   
  
Microsoft Azure sauvegarde automatiquement les données distantes que Stretch Database a migrées de SQL Server vers Azure.    
### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure réduit le risque de perte de données avec la sauvegarde automatique  
Le service SQL Server Stretch Database sur Azure protège vos bases de données distantes avec des instantanés de stockage automatiques au moins toutes les huit heures. Il conserve chaque instantané pendant sept jours pour vous fournir une gamme de points de restauration.  
  
### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure réduit le risque de perte de données avec la géoredondance  
Les sauvegardes de base de données Azure sont stockées sur du stockage Azure géoredondant (RA-GRS) et sont donc géoredondantes par défaut. Le stockage géoredondant réplique vos données dans une région secondaire située à des centaines de kilomètres de la région principale. Dans les régions principale et secondaire, vos données sont répliquées trois fois chacune, sur des domaines d’erreur et de mise à niveau distincts. Cela garantit la durabilité de vos données même en cas de panne ou de sinistre régional total rendant l’une des régions Azure indisponible.

### <a name="stretchRPO"></a>Stretch Database réduit le risque de perte de données pour vos données Azure en conservant temporairement les lignes migrées
Une fois que Stretch Database a migré les lignes éligibles de SQL Server vers Azure, il conserve ces lignes dans la table intermédiaire pendant au minimum huit heures. Si vous restaurez une sauvegarde de votre base de données Azure, Stretch Database utilise les lignes enregistrées dans la table intermédiaire pour rapprocher le serveur SQL Server et les bases de données Azure.

Après avoir restauré une sauvegarde de vos données Azure, vous devez exécuter la procédure stockée [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) pour reconnecter la base de données Stretch SQL Server à la base de données Azure distante. Quand vous exécutez **sys.sp_rda_reauthorize_db**, Stretch Database rapproche automatiquement le serveur SQL Server et les bases de données Azure.

Pour augmenter le nombre d’heures de données migrées que Stretch Database conserve temporairement dans la table intermédiaire, exécutez la procédure stockée [sys.sp_rda_set_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md) et spécifiez une durée supérieure à huit heures. Pour déterminer la quantité de données à conserver, tenez compte des facteurs suivants :
-   La fréquence des sauvegardes Azure automatiques (au moins huit heures)
-   Le temps nécessaire après un problème pour identifier ce problème et décider de restaurer une sauvegarde
-   La durée de l’opération de restauration Azure

> [!NOTE]
> Le faut d’augmenter la quantité de données que Stretch Database conserve temporairement dans la table intermédiaire augmente la quantité d’espace nécessaire sur le serveur SQL Server.

Pour vérifier le nombre d’heures de données que Stretch Database conserve actuellement temporairement dans la table intermédiaire, exécutez la procédure stockée [sys.sp_rda_get_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md).

## <a name="see-also"></a> Voir aussi  
[Restaurer des bases de données Stretch](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
 [Gérer Stretch Database et résoudre ses problèmes](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)   
   
  
  
