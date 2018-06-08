---
title: sp_help_spatial_geometry_histogram (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_help_spatial_geometry_histogram
- sp_help_spatial_geometry_histogram_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_histogram
ms.assetid: 036aaf61-df3e-40f7-aa4e-62983c5a37bd
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d12665106633cbbdf46284089ef5f7eb79d7ccbf
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpspatialgeometryhistogram-transact-sql"></a>sp_help_spatial_geometry_histogram (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Facilite le codage du rectangle englobant et des paramètres de grille pour un index spatial.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_spatial_geometry_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @xmin = ] 'minx' ]   
     [ , [ @ymin = ] 'miny' ]   
     [ ,.[ @xmax = ] 'maxx' ]  
     [ , [ @ymax = ] 'maxy' ]  
     [ , [ @sample = ] 'sample' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@tabname =**] **'***tabname***'**  
 Spécifie le nom qualifié ou non qualifié de la table pour laquelle l'index spatial a été défini.  
  
 Les guillemets ne sont nécessaires que si une une table qualifiée est spécifiée. Si un nom qualifié complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données active. *tabname* est **sysname**, sans valeur par défaut.  
  
 [  **@colname =** ] **'***colname***'**  
 Nom de la colonne spatiale spécifiée. *nom de colonne* est un **sysname**, sans valeur par défaut.  
  
 [  **@resolution =** ] **'***résolution***'**  
 Résolution du rectangle englobant. Les valeurs possibles sont comprises entre 10 et 5000. *résolution* est un **tinyint**, sans valeur par défaut.  
  
 [  **@xmin =** ] **'***xmin***'**  
 Propriété de rectangle englobant X-minimum. *XMIN* est un **float**, sans valeur par défaut.  
  
 [  **@ymin =** ] **'***ymin***'**  
 Propriété de rectangle englobant Y-minimum. *YMIN* est un **float**, sans valeur par défaut.  
  
 [  **@xmax =** ] **'***xmax***'**  
 Propriété de rectangle englobant X-maximum. *XMAX* est un **float**, sans valeur par défaut.  
  
 [  **@ymax =** ] **'***ymax***'**  
 Propriété de rectangle englobant Y-maximum. *YMAX* est un **float**, sans valeur par défaut.  
  
 [  **@sample =** ] **'***exemple***'**  
 Pourcentage de la table utilisée. Les valeurs valides sont comprises entre 0 et 100. *exemple* est un **float**. Valeur par défaut est 100.  
  
## <a name="property-valuereturn-value"></a>Valeur de propriété/valeur de retour  
 Une valeur de table est retournée. La grille suivante décrit le contenu de colonne de la table.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|Représente l'ID unique de chaque cellule ; le comptage démarre à 1.|  
|**Cellule**|**geometry**|Polygone rectangulaire qui représente chaque cellule. La forme de cellule est identique à la forme de cellule utilisée pour l'indexation spatiale.|  
|**row_count**|**bigint**|Indique le nombre d'objets spatiaux qui touchent ou contiennent la cellule.|  
  
## <a name="permissions"></a>Autorisations  
 Utilisateur doit être un membre de la **public** rôle. Nécessite une autorisation READ ACCESS sur le serveur et l'objet.  
  
## <a name="remarks"></a>Notes  
 L'onglet spatial SSMS affiche une représentation graphique des résultats. Vous pouvez interroger les résultats dans la fenêtre spatiale afin d'obtenir un nombre approximatif d'éléments de résultat. Les objets dans la table peuvent couvrir plusieurs cellules ; par conséquent, la somme de cellules peut être supérieure au nombre d'objets réels.  
  
 Une ligne supplémentaire peut être ajoutée au jeu de résultats qui conserve le nombre d'objets qui se trouvent en dehors du rectangle englobant ou qui touchent la bordure du rectangle englobant. Le **cellid** de cette ligne est 0 et la **cellule** de cette ligne contient un **LineString** qui représente la zone englobante. Cette ligne représente l'espace entier à l'extérieur du rectangle englobant.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée un exemple de table et appelle ensuite **sp_help_spatial_geometry_histogram** sur la table.  
  
 `USE AdventureWorksDW2012`  
  
 `GO`  
  
 `-- Set database compatibility for circular arc segments`  
  
 `ALTER DATABASE AdventureWorksDW2012`  
  
 `SET COMPATIBILITY_LEVEL = 110;`  
  
 `GO`  
  
 `-- Create table to execute sp_help_spatial_geometry_histogram on`  
  
 `CREATE TABLE TownSites`  
  
 `(`  
  
 `Location geometry NULL,`  
  
 `SiteName nvarchar(50) NULL`  
  
 `)`  
  
 `GO`  
  
 `-- Insert site data into table`  
  
 `DECLARE @g geometry;`  
  
 `SET @g = geometry::Parse('POINT(0 0)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Booth Map';`  
  
 `SET @g = geometry::Parse('POLYGON((1 1, 1 2, 2 2, 2 1, 1 1))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Town Hall';`  
  
 `SET @g = geometry::Parse('CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-1 0, 0 -1, 1 0),(1 0, 1 2, -1 0)))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Park';`  
  
 `SET @g = geometry::Parse('CIRCULARSTRING(1 5, 2 2, 5 1)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Road';`  
  
 `-- Call proc to see data within bounding box`  
  
 `EXEC sp_help_spatial_geometry_histogram @tabname = TownSites, @colname = Location, @resolution = 64, @xmin = -2, @ymin = -2, @xmax = 3, @ymax = 3, @sample = 100;`  
  
 `GO`  
  
 `DROP TABLE TownSites;`  
  
 `GO`  
  
## <a name="see-also"></a>Voir aussi  
 [Les procédures stockées d’Index spatial &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/1be0f34e-3d5a-4a1f-9299-bd482362ec7a)  
  
  
