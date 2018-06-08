---
title: Suppression d’un Index SQL Server | Documents Microsoft
description: Suppression d’un index sql server à l’aide du pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-tables-indexes
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- removing indexes
- deleting indexes
- DropIndex function
- dropping indexes
- OLE DB Driver for SQL Server, indexes
- indexes [OLE DB]
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 6c1ce25341ac4d5f092e61f2d8f23d1ea0baaade
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dropping-a-sql-server-index"></a>Suppression d'un index SQL Server

  Le pilote OLE DB pour SQL Server expose la **IIndexDefinition::DropIndex** (fonction). Cela permet aux consommateurs de supprimer un index à partir d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table.  
  
 Le pilote OLE DB pour SQL Server expose certains [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] des contraintes PRIMARY KEY et UNIQUE en tant qu’index. Propriétaire de la table, le propriétaire de la base de données et certains membres du rôle d’administrateur peuvent modifier un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table, la suppression d’une contrainte. Par défaut, seul le propriétaire de la table peut supprimer un index existant. Par conséquent, **DropIndex** réussite ou l’échec dépend non seulement de droits d’accès de l’utilisateur de l’application, mais également sur le type d’index indiqué.  
  
 Les consommateurs spécifient le nom de la table en tant que chaîne de caractères Unicode dans le *pwszName* membre de la *uName* union dans la *pTableID* paramètre. Le *eKind* membre *pTableID* doit être DBKIND_NAME.  
  
 Les consommateurs spécifient le nom de l’index en tant que chaîne de caractères Unicode dans le *pwszName* membre de la *uName* union dans la *pIndexID* paramètre. Le *eKind* membre *pIndexID* doit être DBKIND_NAME. Le pilote OLE DB pour SQL Server ne prend pas en charge la fonctionnalité OLE DB de la suppression de tous les index sur une table lorsque *pIndexID* a la valeur null. Si *pIndexID* est null, E_INVALIDARG est retourné.  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et des index](../../oledb/ole-db-tables-indexes/tables-and-indexes.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md)   
 [DROP INDEX &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-index-transact-sql.md)  
  
  
