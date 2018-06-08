---
title: Ensembles de lignes de schéma modifiés pour les paramètres table OLE DB | Documents Microsoft
description: Ensembles de lignes de schéma modifiés pour les paramètres de OLE DB Table-Valued
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-table-valued-parameters
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- schema rowsets [OLE DB]
- table-valued parameters (OLE DB), schema rowsets changed for (OLE DB)
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 458c5df3860b03a94a2d2c0e3693494f3f0aa3ba
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="schema-rowsets-changed-for-ole-db-table-valued-parameters"></a>Ensembles de lignes de schéma modifiés pour les paramètres table OLE DB
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Le tableau suivant répertorie les ensembles de lignes de schéma qui ont été modifiés ou ajoutés pour prendre en charge des paramètres table.  
  
|Ensemble de lignes de schéma| Description|  
|-------------------|-----------------|  
|DBSCHEMA_PROCEDURE_PARAMETERS|Deux nouvelles colonnes ont été ajoutées à la fin de l'ensemble de lignes nommé SS_TYPE_CATALOG_NAME et SS_TYPE_SCHEMANAME. Ces colonnes peuvent être réutilisées pour des types futurs. Les colonnes TYPE_NAME et LOCAL_TYPE_NAME contiendront le nom du type TABLE de paramètre table. La colonne DATA_TYPE aura la valeur DBTYPE_TABLE = 143 pour les paramètres table.|  
|DBSCHEMA_TABLE_TYPES|Cet ensemble de lignes a été ajouté pour prendre en charge des paramètres table. Il est identique à DBSCHEMA_TABLES, à la différence près qu'il retourne uniquement des métadonnées pour des types de table plutôt que pour des tables, des vues ou des synonymes. La colonne TABLE_TYPE aura la valeur « TABLE TYPE ».|  
|DBSCHEMA_TABLE_TYPE_PRIMARY_KEYS|Cet ensemble de lignes a été ajouté pour prendre en charge des paramètres table. Il est identique à DBSCHEMA_PRIMARY_KEYS, à la différence près qu'il retourne uniquement des métadonnées de clés primaires pour des types de table plutôt que pour des tables.|  
|DBSCHEMA_TABLE_TYPE_COLUMNS|Cet ensemble de lignes a été ajouté pour prendre en charge des paramètres table. Il est identique à DBSCHEMA_COLUMNS, à la différence près qu'il retourne uniquement des métadonnées de colonne pour des types de table plutôt que pour des tables, des vues ou des synonymes.|  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres table &#40;OLE DB&#41;](../../oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Utilisez la Table-Valued paramètres & #40 ; OLE DB & #41 ;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
