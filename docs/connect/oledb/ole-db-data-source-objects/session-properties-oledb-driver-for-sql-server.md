---
title: Propriétés de la session - pilote OLE DB SQL Server | Documents Microsoft
description: Propriétés de la session - pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-data-source-objects
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- sessions [OLE DB]
- OLE DB Driver for SQL Server, sessions
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 3a6c4816c57ea0cac07f17a34e1dfafa1d4b2e93
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="session-properties---ole-db-driver-for-sql-server"></a>Propriétés de la session - pilote OLE DB SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Le pilote OLE DB pour SQL Server interprète les propriétés de la session OLE DB comme suit.  
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|Le pilote OLE DB pour SQL Server prend en charge tous les niveaux d’isolation de transactions validées automatiquement à l’exception du niveau de chaos DBPROPVAL_TI_CHAOS.|  
  
 Dans l’ensemble de la propriété spécifique au fournisseur DBPROPSET_SQLSERVERSESSION, le pilote OLE DB pour SQL Server définit la propriété de session supplémentaire suivante.  
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|SSPROP_QUOTEDCATALOGNAMES|Type : VT_BOOL<br /><br /> R/W : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE<br /><br /> Description : identificateurs entre guillemets autorisés dans la restriction CATALOG.<br /><br /> VARIANT_TRUE : les identificateurs entre guillemets sont reconnus pour une restriction de catalogue pour les ensembles de lignes de schéma qui fournissent la prise en charge des requêtes distribuées.<br /><br /> VARIANT_FALSE : les identificateurs entre guillemets ne sont pas reconnus pour une restriction de catalogue pour les ensembles de lignes de schéma qui fournissent la prise en charge des requêtes distribuées.<br /><br /> Pour plus d’informations sur les ensembles de lignes de schéma qui fournissent la prise en charge des requêtes distribuées, consultez [distribuée requête prend en charge dans les ensembles de lignes de schéma](../../oledb/ole-db/schema-rowsets-distributed-query-support.md).|  
|SSPROP_ALLOWNATIVEVARIANT|Type : VT_BOOL<br /><br /> Lecture/écriture : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE<br /><br /> Description : détermine si les données sont extraites en tant que DBTYPE_VARIANT ou DBTYPE_SQLVARIANT.<br /><br /> VARIANT_TRUE : le type de colonne est retourné en tant que DBTYPE_SQLVARIANT, auquel cas la mémoire tampon contient la structure SSVARIANT.<br /><br /> VARIANT_FALSE : le type de colonne est retourné en tant que DBTYPE_VARIANT et la mémoire tampon a la structure VARIANT.|  
|SSPROP_ASYNCH_BULKCOPY|Pour utiliser le mode asynchrone, affectez la valeur VARIANT_TRUE à la propriété de session spécifique au fournisseur SSPROP_ASYNCH_BULKCOPY avant d'appeler la méthode BCPExec. Cette propriété est disponible dans le jeu de propriétés DBPROPSET_SQLSERVERSESSION.|  
  
## <a name="see-also"></a>Voir aussi  
 [Objets Source de données & #40 ; OLE DB & #41 ;](../../oledb/ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
