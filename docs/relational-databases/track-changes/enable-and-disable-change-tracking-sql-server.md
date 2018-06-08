---
title: Activer et désactiver le suivi des modifications (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: track-changes
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server], disabling
- data changes [SQL Server]
- change tracking [SQL Server], enabling
- tracking data changes [SQL Server]
- change tracking [SQL Server], configuring
- data [SQL Server], changing
ms.assetid: 1c92ec7e-ae53-4498-8bfd-c66a42a24d54
caps.latest.revision: 34
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 49bbb4a42468f359490c6536841af70c02bcb318
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enable-and-disable-change-tracking-sql-server"></a>Activer et désactiver le suivi des modifications (SQL Server)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Cette rubrique décrit l'activation et la désactivation du suivi des modifications pour une base de données et une table.  
  
## <a name="enable-change-tracking-for-a-database"></a>Activer le suivi des modifications pour une base de données  
 Avant de pouvoir utiliser le suivi des modifications, vous devez l'activer au niveau de la base de données. L’exemple suivant indique comment activer le suivi des modifications en utilisant [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
```sql  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING = ON  
(CHANGE_RETENTION = 2 DAYS, AUTO_CLEANUP = ON)  
```  
  
 Vous pouvez également activer le suivi des modifications dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en utilisant la boîte de dialogue [Propriétés de la base de données &#40;page Suivi des modifications&#41;](../../relational-databases/databases/database-properties-changetracking-page.md) .  
  
 Vous pouvez spécifier les options CHANGE_RETENTION et AUTO_CLEANUP lorsque vous activez le suivi des modifications et vous pouvez modifier leurs valeurs à tout moment une fois que le suivi des modifications a été activé.  
  
 La valeur de la rétention des modifications indique la période pendant laquelle les informations de suivi des modifications sont conservées. À l'issue de cette période, les informations de suivi des modifications sont supprimées. Lorsque vous affectez cette valeur, vous devez considérer la fréquence à laquelle les applications sont synchronisées avec les tables incluses dans la base de données. En effet, la période de rétention spécifiée doit être supérieure ou égale à la période maximale entre deux synchronisations. Si une application obtient des modifications sur des intervalles plus longs, les résultats retournés risquent d'être incorrects, parce qu'une partie des informations de modification aura probablement été supprimée. Pour éviter d'obtenir des résultats incorrects, une application peut utiliser la fonction système CHANGE_TRACKING_MIN_VALID_VERSION pour déterminer si l'intervalle entre synchronisations a été trop long.  
  
 Vous pouvez utiliser l'option AUTO_CLEANUP pour activer ou désactiver la tâche de nettoyage qui permet de supprimer les informations de suivi des modifications anciennes. Ce paramètre peut s'avérer utile lorsqu'il existe un problème temporaire qui empêche les applications de se synchroniser et que le processus de suppression des informations de suivi des modifications antérieures à la période de rétention doit être suspendu jusqu'à ce que le problème soit résolu.  
  
 Pour toute base de données qui utilise le suivi des modifications, ayez conscience de ce qui suit :  
  
-   Pour utiliser le suivi des modifications, le niveau de compatibilité de la base de données doit être défini à 90 ou plus. Si une base de données présente un niveau de compatibilité inférieur à 90, vous pouvez configurer le suivi des modifications. Dans ce cas, toutefois, la fonction CHANGETABLE, utilisée pour obtenir des informations de suivi des modifications, retourne une erreur.  
  
-   L'utilisation de l'isolement d'instantané constitue le moyen le plus simple de garantir la cohérence de toutes les informations de suivi des modifications. C'est pourquoi nous recommandons fortement d'affecter la valeur ON à cet isolement d'instantané pour la base de données. Pour plus d’informations, consultez [Utiliser le suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-tracking-sql-server.md).  
  
## <a name="enable-change-tracking-for-a-table"></a>Activer le suivi des modifications pour une table  
 Le suivi des modifications doit être activé pour chaque table que vous souhaitez suivre. Lorsque le suivi des modifications est activé, les informations correspondantes sont conservées pour toutes les lignes de la table qui sont affectées par une opération DML.  
  
 L’exemple suivant indique comment activer le suivi des modifications pour une table en utilisant [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
```sql  
ALTER TABLE Person.Contact  
ENABLE CHANGE_TRACKING  
WITH (TRACK_COLUMNS_UPDATED = ON)  
```  
  
 Vous pouvez également activer le suivi des modifications pour une table dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en utilisant la boîte de dialogue [Propriétés de la base de données &#40;page Suivi des modifications&#41;](../../relational-databases/databases/database-properties-changetracking-page.md) .  
  
 Quand l’option TRACK_COLUMNS_UPDATED a la valeur ON, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] stocke des informations supplémentaires sur les colonnes qui ont été mises à jour dans la table de suivi des modifications interne. Le suivi des colonnes peut permettre à une application de synchroniser uniquement les colonnes mises à jour. Il permet ainsi d'améliorer l'efficacité et les performances. Toutefois, étant donné que la conservation des informations de suivi des colonnes augmente la charge de stockage, cette option a la valeur OFF par défaut.  
  
## <a name="disable-change-tracking-for-a-database-or-table"></a>Désactiver le suivi des modifications pour une base de données ou une table  
 Vous devez d'abord désactiver le suivi des modifications pour toutes les tables qui en font l'objet avant d'affecter la valeur OFF au suivi des modifications pour la base de données. Pour déterminer les tables dont le suivi des modifications est activé pour une base de données, utilisez l’affichage catalogue [sys.change_tracking_tables](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md) .  
  
 Quand aucune table n'effectue un suivi des modifications dans une base de données, vous pouvez désactiver le suivi des modifications pour cette base de données. L’exemple suivant indique comment désactiver le suivi des modifications pour une base de données en utilisant [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
```sql  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING = OFF  
```  
  
 L’exemple suivant indique comment désactiver le suivi des modifications pour une table en utilisant [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
```sql  
ALTER TABLE Person.Contact  
DISABLE CHANGE_TRACKING;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Propriétés de la base de données &#40;page Suivi des modifications&#41;](../../relational-databases/databases/database-properties-changetracking-page.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.change_tracking_databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)   
 [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [À propos du suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [Utiliser les données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Gérer le suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/manage-change-tracking-sql-server.md)  
  
  
