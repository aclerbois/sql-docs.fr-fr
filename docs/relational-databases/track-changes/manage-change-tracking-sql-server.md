---
title: Gérer le suivi des modifications (SQL Server) | Microsoft Docs
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
- tracking data changes [SQL Server]
- change tracking [SQL Server], overhead
- change tracking [SQL Server]
- change tracking [SQL Server], managing
ms.assetid: 94a8d361-e897-4d6d-9a8f-1bb652e7a850
caps.latest.revision: 9
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4eb909657ff729f6893a2ae072935dee5ef5f6f6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-change-tracking-sql-server"></a>Gérer le suivi des modifications (SQL Server)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Cette rubrique décrit comment gérer le suivi des modifications. Elle explique également comment configurer la sécurité et déterminer l'impact de l'utilisation du suivi des modifications sur le stockage et les performances.  
  
## <a name="managing-change-tracking"></a>Gestion du suivi des modifications  
 Les sections suivantes répertorient les affichages catalogue, les autorisations et les paramètres qui relèvent de la gestion du suivi des modifications.  
  
### <a name="catalog-views"></a>Affichages catalogue  
 Pour déterminer quelles tables et bases de données font l'objet d'un suivi des modifications, vous pouvez utiliser les affichages catalogue suivants :  
  
-   [sys.change_tracking_databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)  
  
-   [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
 En outre, l’affichage catalogue [sys.internal_tables](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md) répertorie les tables internes créées lorsque le suivi des modifications est activé pour une table utilisateur.  
  
### <a name="security"></a>Sécurité  
 Pour accéder aux informations de suivi des modifications à l'aide des [fonctions de suivi des modifications](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md), le principal doit posséder les autorisations suivantes :  
  
-   Autorisation SELECT sur au moins les colonnes clés primaire de la table faisant l'objet d'un suivi des modifications pour la table interrogée.  
  
-   Autorisation VIEW CHANGE TRACKING sur la table pour laquelle les modifications sont obtenues. L'autorisation VIEW CHANGE TRACKING est obligatoire pour les raisons suivantes :  
  
    -   Les enregistrements de suivi des modifications incluent des informations sur les lignes supprimées et plus particulièrement sur les valeurs de clés primaires des lignes qui ont été supprimées. Un principal pourrait avoir reçu, après la suppression de certaines données sensibles, une autorisation SELECT pour une table faisant l'objet d'un suivi des modifications. Dans ce cas, vous ne souhaiteriez pas que ce principal soit en mesure d'accéder aux informations supprimées à l'aide du suivi des modifications.  
  
    -   Les informations de suivi des modifications peuvent stocker des informations sur les colonnes qui ont été modifiées par des opérations de mise à jour. Un principal pourrait se voir refuser l'autorisation d'accéder à une colonne qui contient des informations sensibles. Toutefois, étant donné que les informations de suivi des modifications sont disponibles, un principal peut déterminer qu'une valeur de colonne a été mise à jour, mais le principal ne peut pas déterminer la valeur de la colonne.  
  
## <a name="understanding-change-tracking-overhead"></a>Présentation de la charge de traitement liée au suivi des modifications  
 Lorsque le suivi des modifications est activé pour une table, certaines opérations d'administration sont affectées. Le tableau suivant répertorie les opérations et les effets à considérer.  
  
|Opération|Lorsque le suivi des modifications est activé|  
|---------------|-------------------------------------|  
|DROP TABLE|Toutes les informations de suivi des modifications pour la table supprimée sont supprimées.|  
|ALTER TABLE DROP CONSTRAINT|Toute tentative de supprimer la contrainte PRIMARY KEY échoue. Le suivi des modifications doit être désactivé avant de supprimer une contrainte PRIMARY KEY.|  
|ALTER TABLE DROP COLUMN|Si une colonne supprimée fait partie de la clé primaire, la suppression de la colonne n'est pas autorisée, indépendamment du suivi des modifications.<br /><br /> Si la colonne supprimée ne fait pas partie de la clé primaire, la suppression de la colonne réussit. Toutefois, il est préférable de bien comprendre au préalable l'effet sur toutes les applications qui synchronisent ces données. Si le suivi des modifications de colonnes est activé pour la table, la colonne supprimée peut quand même être retournée dans le cadre des informations de suivi des modifications. L'application est chargée de gérer la colonne supprimée.|  
|ALTER TABLE ADD COLUMN|Si une nouvelle colonne est ajoutée à la table faisant l'objet d'un suivi des modifications, cet ajout ne fait pas l'objet d'un suivi des modifications. Seules sont suivies les mises à jour et les modifications apportées à la nouvelle colonne.|  
|ALTER TABLE ALTER COLUMN|Les modifications des types de données dans les colonnes clés non primaires ne font pas l'objet d'un suivi.|  
|ALTER TABLE SWITCH|Le basculement de partition échoue si le suivi des modifications est activé pour l'une des tables ou les deux.|  
|DROP INDEX ou ALTER INDEX DISABLE|L'index qui applique la clé primaire ne peut pas être supprimé ni désactivé.|  
|TRUNCATE TABLE|La troncature d'une table peut être effectuée sur une table pour laquelle le suivi des modifications est activé. Toutefois, les lignes supprimées par l'opération ne sont pas suivies, et la version valide minimale est mise à jour. Lorsqu'une application vérifie sa version, le contrôle indique que la version est trop ancienne et qu'une réinitialisation est requise. Cela revient au même qu'une désactivation du suivi des modifications, suivie d'une nouvelle activation pour la table.|  
  
 L'utilisation du suivi des modifications ajoute à la charge de traitement des opérations DML en raison des informations stockées dans le cadre de l'opération.  
  
### <a name="effects-on-dml"></a>Effets sur les opérations DML  
 Le suivi des modifications a été optimisé afin de réduire la sollicitation des ressources système que les opérations DML engendrent. Les diminutions de performances incrémentielles associées à l'utilisation du suivi des modifications sur une table sont semblables aux charges de traitement générées lorsqu'un index est créé pour une table et doit être géré.  
  
 Pour chaque ligne modifiée par une opération DML, une ligne est ajoutée à la table de suivi des modifications interne. L'effet produit en fonction de l'opération DML dépend de différents facteurs, tels que les suivants :  
  
-   le nombre de colonnes clés primaire ;  
  
-   la quantité de données modifiées dans la ligne de la table utilisateur ;  
  
-   le nombre d'opérations effectuées dans une transaction.  
  
 L'isolement d'instantané, s'il est utilisé, produit également un effet sur les performances de toutes les opérations DML, que le suivi des modifications soit activé ou non.  
  
### <a name="effects-on-storage"></a>Effets sur le stockage  
 Les données de suivi des modifications sont stockées dans les types suivants de tables internes :  
  
-   Table des modifications interne  
  
     Il existe une seule table des modifications interne pour chaque table utilisateur où le suivi des modifications est activé.  
  
-   Table des transactions interne  
  
     Il existe une seule table des transactions interne pour la base de données.  
  
 Ces tables internes affectent les besoins de stockage des manières suivantes :  
  
-   Pour chaque modification apportée à chaque ligne dans la table utilisateur, une ligne est ajoutée à la table des modifications interne. Cette ligne a une faible charge fixe, plus une charge variable égale à la taille des colonnes clés primaire. La ligne peut contenir des informations de contexte facultatives définies par une application. En outre, si le suivi des colonnes est activé, chaque colonne modifiée requiert 4 octets dans la table de suivi.  
  
-   Pour chaque transaction validée, une ligne est ajoutée à une table des transactions interne.  
  
 Comme avec d’autres tables internes, vous pouvez déterminer l’espace utilisé pour les tables de suivi des modifications en utilisant la procédure stockée [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) . Vous pouvez obtenir les noms des tables internes en utilisant l’affichage catalogue [sys.internal_tables](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md) , comme l’illustre l’exemple suivant.  
  
```sql  
sp_spaceused 'sys.change_tracking_309576141'  
sp_spaceused 'sys.syscommittab'  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [Propriétés de la base de données &#40;page Suivi des modifications&#41;](../../relational-databases/databases/database-properties-changetracking-page.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.change_tracking_databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)   
 [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [À propos du suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [Utiliser les données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
  
  
