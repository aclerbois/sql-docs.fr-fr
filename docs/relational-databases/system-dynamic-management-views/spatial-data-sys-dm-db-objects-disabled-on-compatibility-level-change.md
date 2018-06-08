---
title: Sys.dm_db_objects_disabled_on_compatibility_level_change (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_db_objects_disabled_on_compatibility_level_change
- dm_db_objects_disabled_on_compatibility_level_change_TSQL
- sys.dm_db_objects_disabled_on_compatibility_level_change
- sys.dm_db_objects_disabled_on_compatibility_level_change_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_objects_disabled_on_compatibility_level_change catalog view
ms.assetid: a5d70064-0330-48b9-b853-01eba50755d0
caps.latest.revision: 16
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 083af55f2629a14f2ad28b293bb84ea9184a0345
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="spatial-data---sysdmdbobjectsdisabledoncompatibilitylevelchange"></a>Données spatiales - sys.dm_db_objects_disabled_on_compatibility_level_change
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Répertorie les index et les contraintes qui seront désactivés suite à la modification du niveau de compatibilité dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les index et les contraintes qui contiennent des colonnes calculées persistantes dont les expressions utilisent des types définis par l'utilisateur spatiaux sont désactivés après une mise à niveau ou une modification du niveau de compatibilité. Utilisez cette fonction de gestion dynamique pour déterminer l'impact d'un changement de niveau de compatibilité.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
sys.dm_db_objects_disabled_on_compatibility_level_change ( compatibility_level )   
```  
  
##  <a name="Arguments"></a> Arguments  
 *compatibility_level*  
 **int** qui identifie le niveau de compatibilité que vous souhaitez définir.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**class**|**int**|1 = contraintes<br /><br /> 7 = index et segments|  
|**class_desc**|**nvarchar(60)**|OBJECT ou COLUMN pour les contraintes<br /><br /> INDEX pour les index et les segments|  
|**major_id**|**int**|OBJECT ID des contraintes<br /><br /> OBJECT ID de la table qui contient des index et des segments|  
|**minor_id**|**int**|NULL pour les contraintes<br /><br /> Index_id pour les index et les segments|  
|**Dépendance**|**nvarchar(60)**|Description de la dépendance qui provoque la désactivation de la contrainte ou de l'index. Les mêmes valeurs sont également utilisées dans les avertissements générés pendant la mise à niveau. En voici quelques exemples :<br /><br /> « space » pour un type intrinsèque<br /><br /> « geometry » pour un type défini par l'utilisateur système<br /><br /> « geography::Parse » pour une méthode d'un type défini par l'utilisateur système|  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Les colonnes calculées persistantes qui utilisent des fonctions intrinsèques sont désactivées lorsque le niveau de compatibilité est modifié. En outre, les colonnes calculées persistantes qui utilisent une méthode de type geometry ou geography sont désactivées lorsqu'une base de données est mise à niveau.  
  
### <a name="which-functions-cause-persisted-computed-columns-to-be-disabled"></a>Quelles sont les fonctions qui provoquent la désactivation des colonnes calculées persistantes ?  
 Lorsque les fonctions suivantes sont utilisées dans l'expression d'une colonne calculée persistante, elles entraînent la désactivation des index et des contraintes qui font référence à ces colonnes lorsque le niveau de compatibilité est modifié de 80 à 90 :  
  
-   **IsNumeric**  
  
 Lorsque les fonctions suivantes sont utilisées dans l'expression d'une colonne calculée persistante, elles entraînent la désactivation des index et des contraintes qui font référence à ces colonnes lorsque le niveau de compatibilité est modifié de 100 à 110 ou plus :  
  
-   **Soundex**  
  
-   **Geography :: GeomFromGML**  
  
-   **Geography :: STGeomFromText**  
  
-   **Geography :: STLineFromText**  
  
-   **Geography :: STPolyFromText**  
  
-   **Geography :: STMPointFromText**  
  
-   **Geography :: STMLineFromText**  
  
-   **Geography :: STMPolyFromText**  
  
-   **Geography :: STGeomCollFromText**  
  
-   **Geography :: STGeomFromWKB**  
  
-   **Geography :: STLineFromWKB**  
  
-   **Geography :: STPolyFromWKB**  
  
-   **Geography :: STMPointFromWKB**  
  
-   **Geography :: STMLineFromWKB**  
  
-   **Geography :: STMPolyFromWKB**  
  
-   **Geography :: STUnion**  
  
-   **Geography :: STIntersection**  
  
-   **Geography :: STDifference**  
  
-   **Geography :: STSymDifference**  
  
-   **Geography :: STBuffer**  
  
-   **Geography :: BufferWithTolerance**  
  
-   **Geography :: analyser**  
  
-   **Geography :: réduire**  
  
### <a name="behavior-of-the-disabled-objects"></a>Comportement des objets désactivés  
 **Index**  
  
 Si l’index cluster est désactivé, ou si un index non cluster est forcé, l’erreur suivante est générée : « le processeur de requêtes ne peut pas créer de plan car l’index ' %. \*%.*ls sur la table ou la vue ' %. \*%.*ls est désactivé. » Pour réactiver ces objets, reconstruisez les index après la mise à niveau en appelant **ALTER INDEX ON... RECONSTRUIRE**.  
  
 **Segments de mémoire**  
  
 Si une table avec un segment désactivé est utilisée, l'erreur suivante est levée. Pour réactiver ces objets, reconstruisez après mise à niveau en appelant **ALTER INDEX ON tous les... RECONSTRUIRE**.  
  
```  
// ErrorNumber: 8674  
// ErrorSeverity: EX_USER  
// ErrorFormat: The query processor is unable to produce a plan because the table or view '%.*ls' is disabled.  
// ErrorCause: The table has a disabled heap.   
// ErrorCorrectiveAction: Rebuild the disabled heap to enable it.   
// ErrorInserts: table or view name   
// ErrorOwner: mtintor   
// ErrorFirstProduct: SQL11  
```  
  
 Si vous essayez de reconstruire le segment lors d’une opération en ligne, une erreur est générée.  
  
 **Contraintes de validation et les clés étrangères**  
  
 Les contraintes de validation et les clés étrangères désactivées ne déclenchent pas d'erreur. Toutefois, les contraintes ne sont pas appliquées lorsque des lignes sont modifiées. Pour réactiver ces objets, vérifiez les contraintes après la mise à niveau en appelant **ALTER TABLE... CONTRAINTE de validation**.  
  
 **Colonnes calculées persistantes**  
  
 Comme il est impossible de désactiver une colonne unique, la table entière est désactivée grâce à la désactivation de l'index cluster ou du segment.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DATABASE STATE.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant illustre une requête sur **sys.dm_db_objects_disabled_on_compatibility_level_change** pour trouver les objets affectés par la modification du niveau de compatibilité à 120.  
  
```sql  
SELECT * FROM sys.dm_db_objects_disabled_on_compatibility_level_change(120);  
GO  
  
```  
  
  
