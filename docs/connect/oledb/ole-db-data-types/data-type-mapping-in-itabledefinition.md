---
title: Mappage de Type de données dans ITableDefinition | Documents Microsoft
description: Mappage de type de données dans ITableDefinition
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- OLE DB Driver for SQL Server, data types
- ITableDefinition interface
- DBCOLUMNDESC structure
- data types [OLE DB]
- CreateTable function
- OLE DB, data types
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: ade365add5b89069e86f67d82dfa84638bcb71f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-mapping-in-itabledefinition"></a>Mappage de type de données dans ITableDefinition
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Lors de la création de tables à l’aide de la **ITableDefinition::CreateTable** (fonction), le pilote OLE DB pour le consommateur de SQL Server peut spécifier [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] types de données dans le *pwszTypeName* membre de la Tableau DBCOLUMNDESC passé. Si le consommateur Spécifie le type de données d’une colonne par son nom, mappage, représenté par de type de données OLE DB le *wType* membre de la structure DBCOLUMNDESC, est ignoré.  
  
 Lorsque vous spécifiez des nouveaux types de données de colonne avec les types de données OLE DB à l’aide de la structure DBCOLUMNDESC *wType* membre, le pilote OLE DB pour SQL Server mappe les types de données OLE DB comme suit.  
  
|Type de données OLE DB|SQL Server<br /><br /> type de données|Autres informations|  
|----------------------|------------------------------|----------------------------|  
|DBTYPE_BOOL|**bit**||  
|DBTYPE_BYTES|**binaire**, **varbinary**, **image,** ou **varbinary (max)**|Le pilote OLE DB pour SQL Server inspecte le *ulColumnSize* membre de la structure DBCOLUMNDESC. En fonction de la valeur et la version de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de l’instance, le pilote OLE DB pour SQL Server mappe le type à **image**.<br /><br /> Si la valeur de *ulColumnSize* est inférieure à la longueur maximale d’un **binaire** type de données de colonne, le pilote OLE DB pour SQL Server inspecte la structure DBCOLUMNDESC *rgPropertySets*membre. Si DBPROP_COL_FIXEDLENGTH est VARIANT_TRUE, le pilote OLE DB pour SQL Server mappe le type à **binaire**. Si la valeur de la propriété est VARIANT_FALSE, le pilote OLE DB pour SQL Server mappe le type à **varbinary**. Dans les deux cas, la structure DBCOLUMNDESC *ulColumnSize* détermine la largeur de la colonne SQL Server créée.|  
|DBTYPE_CY|**money**||  
|DBTYPE_DBTIMESTAMP|**datetime2**||  
|DBTYPE_GUID|**uniqueidentifier**||  
|DBTYPE_I2|**smallint**||  
|DBTYPE_I4|**int**||  
|DBTYPE_I8|**bigint**||
|DBTYPE_NUMERIC|**numeric**|Le pilote OLE DB pour SQL Server inspecte les membres *bPrecision* et *bScale* membres pour déterminer la précision et l’échelle pour le **numérique** colonne.|  
|DBTYPE_R4|**real**||  
|DBTYPE_R8|**float**||  
|DBTYPE_STR|**char**, **varchar**, **texte,** ou **varchar (max)**|Le pilote OLE DB pour SQL Server inspecte le *ulColumnSize* membre de la structure DBCOLUMNDESC. En fonction de la valeur et la version de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de l’instance, le pilote OLE DB pour SQL Server mappe le type à **texte**.<br /><br /> Si la valeur de *ulColumnSize* est inférieure à la longueur maximale d’une colonne de type de données de caractères multioctets, puis le pilote OLE DB pour SQL Server inspecte la structure DBCOLUMNDESC *rgPropertySets* membre. Si DBPROP_COL_FIXEDLENGTH est VARIANT_TRUE, le pilote OLE DB pour SQL Server mappe le type à **char**. Si la valeur de la propriété est VARIANT_FALSE, le pilote OLE DB pour SQL Server mappe le type à **varchar**. Dans les deux cas, la structure DBCOLUMNDESC *ulColumnSize* détermine la largeur de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] colonne créée.|  
|DBTYPE_UDT|**UDT**|Les informations suivantes sont utilisées dans **DBCOLUMNDESC** par les structures **ITableDefinition::CreateTable** lorsque les colonnes UDT sont requises :<br /><br /> *pwSzTypeName* est ignoré.<br /><br /> *rgPropertySets* doit inclure un **DBPROPSET_SQLSERVERCOLUMN** propriété est définie comme décrit dans la section sur **DBPROPSET_SQLSERVERCOLUMN**, dans [Defined Types ](../../oledb/features/using-user-defined-types.md).|  
|DBTYPE_UI1|**tinyint**||  
|DBTYPE_VARIANT|**sql_variant**||
|DBTYPE_WSTR|**NCHAR**, **nvarchar**, **ntext,** ou **nvarchar (max)**|Le pilote OLE DB pour SQL Server inspecte le *ulColumnSize* membre de la structure DBCOLUMNDESC. En fonction de la valeur, le pilote OLE DB pour SQL Server mappe le type à **ntext**.<br /><br /> Si la valeur de *ulColumnSize* est inférieure à la longueur maximale d’une colonne de type de données de caractères Unicode, puis le pilote OLE DB pour SQL Server inspecte la structure DBCOLUMNDESC *rgPropertySets* membre. Si DBPROP_COL_FIXEDLENGTH est VARIANT_TRUE, le pilote OLE DB pour SQL Server mappe le type à **nchar**. Si la valeur de la propriété est VARIANT_FALSE, le pilote OLE DB pour SQL Server mappe le type à **nvarchar**. Dans les deux cas, la structure DBCOLUMNDESC *ulColumnSize* détermine la largeur de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] colonne créée.|  
|DBTYPE_XML|**XML**||  

> [!NOTE]  
>  Lorsque vous créez une nouvelle table, le pilote OLE DB pour SQL Server mappe uniquement les valeurs OLE DB données type énumération spécifiés dans le tableau précédent. Toute tentative de création d'une table avec une colonne de tout autre type de données OLE DB génère une erreur.  

## <a name="see-also"></a>Voir aussi  
 [Types de données & #40 ; OLE DB & #41 ;](../../oledb/ole-db-data-types/data-types-ole-db.md)  
  
  
