---
title: Mappage de Type de données dans les ensembles de lignes et les paramètres | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- DBTYPE_SQLVARIANT data type
- SQL Server Native Client OLE DB provider, data types
- rowsets [OLE DB], data type mapping
- data types [OLE DB]
- GetColumnInfo function
- parameters [OLE DB]
- SSPROP_ALLOWNATIVEVARIANT property
- GetParameterInfo function
- OLE DB, data types
ms.assetid: 3d831ff8-3b79-4698-b2c1-2b5dd2f8235c
caps.latest.revision: 41
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e81a584d2af05050483a68e05f164b920f0ad764
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-mapping-in-rowsets-and-parameters"></a>Mappage de type de données dans les ensembles de lignes et les paramètres
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Dans les ensembles de lignes et en tant que valeurs de paramètre, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB fournisseur représente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] données à l’aide suivante OLE DB définie par les types de données, signalés dans les fonctions **IColumnsInfo::GetColumnInfo** et **ICommandWithParameters::GetParameterInfo**.  
  
|Type de données SQL Server|Type de données OLE DB|  
|--------------------------|----------------------|  
|**bigint**|DBTYPE_I8|  
|**binaire**|DBTYPE_BYTES|  
|**bit**|DBTYPE_BOOL|  
|**char**|DBTYPE_STR|  
|**datetime**|DBTYPE_DBTIMESTAMP|  
|**datetime2**|DBTYPE_DBTIME2|  
|**decimal**|DBTYPE_NUMERIC|  
|**float**|DBTYPE_R8|  
|**image**|DBTYPE_BYTES|  
|**int**|DBTYPE_I4|  
|**money**|DBTYPE_CY|  
|**nchar**|DBTYPE_WSTR|  
|**ntext**|DBTYPE_WSTR|  
|**numeric**|DBTYPE_NUMERIC|  
|**nvarchar**|DBTYPE_WSTR|  
|**real**|DBTYPE_R4|  
|**smalldatetime**|DBTYPE_DBTIMESTAMP|  
|**smallint**|DBTYPE_I2|  
|**smallmoney**|DBTYPE_CY|  
|**sql_variant**|DBTYPE_VARIANT, DBTYPE_SQLVARIANT|  
|**sysname**|DBTYPE_WSTR|  
|**texte**|DBTYPE_STR|  
|**timestamp**|DBTYPE_BYTES|  
|**tinyint**|DBTYPE_UI1|  
|**UDT**|DBTYPE_UDT|  
|**uniqueidentifier**|DBTYPE_GUID|  
|**varbinary**|DBTYPE_BYTES|  
|**varchar**|DBTYPE_STR|  
|**XML**|DBTYPE_XML|  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif prend en charge les conversions de données demandées par le consommateur comme indiqué dans l’illustration.  
  
 Le **sql_variant** objets peuvent contenir des données de n’importe quel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données à l’exception de texte, ntext, image, varchar (max), nvarchar (max), varbinary (max), xml, timestamp et les types définis par l’utilisateur de Microsoft .NET Framework common language runtime (CLR). sql_variant ne peut pas être le type de données de base sous-jacent d'une instance de données sql_variant. Par exemple, la colonne peut contenir **smallint** les valeurs de certaines lignes, **float** valeurs pour les autres lignes, et **char**/**nchar** valeurs dans le reste.  
  
> [!NOTE]  
>  Le **sql_variant** type de données est semblable au type de données Variant dans Microsoft Visual Basic® et à DBTYPE_VARIANT, DBTYPE_SQLVARIANT dans OLEDB.  
  
 Lorsque **sql_variant** données sont extraites en tant que DBTYPE_VARIANT, il est placé dans une structure VARIANT dans la mémoire tampon. Mais les sous-types dans la structure VARIANT peut ne pas correspondent aux sous-types définis dans le **sql_variant** type de données. Le **sql_variant** données doivent ensuite être extraites en tant que DBTYPE_SQLVARIANT, afin que tous les sous-types correspondent.  
  
## <a name="dbtypesqlvariant-data-type"></a>Type de données DBTYPE_SQLVARIANT  
 Pour prendre en charge la **sql_variant** type de données, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client expose un type de données spécifique au fournisseur appelé DBTYPE_SQLVARIANT. Lorsque **sql_variant** données sont extraites en tant que DBTYPE_SQLVARIANT, il est stocké dans une structure SSVARIANT spécifique au fournisseur. La structure SSVARIANT contient tous les sous-types qui correspondent aux sous-types de le **sql_variant** type de données.  
  
 La propriété de session SSPROP_ALLOWNATIVEVARIANT doit également avoir la valeur TRUE.  
  
## <a name="provider-specific-property-sspropallownativevariant"></a>Propriété SSPROP_ALLOWNATIVEVARIANT spécifique au fournisseur  
 Pour extraire des données, vous pouvez spécifier explicitement le type de données à retourner pour une colonne ou un paramètre. **IColumnsInfo** peut également être utilisé pour obtenir les informations de colonne et l’utiliser pour effectuer la liaison. Lorsque **IColumnsInfo** est utilisée pour obtenir des informations sur les colonnes à des fins de liaison, si la propriété a la valeur FALSE (valeur par défaut), de session SSPROP_ALLOWNATIVEVARIANT DBTYPE_VARIANT est retourné pour **sql_variant** colonnes. Si la propriété SSPROP_ALLOWNATIVEVARIANT a la valeur FALSE, DBTYPE_SQLVARIANT n'est pas pris en charge. Si la propriété SSPROP_ALLOWNATIVEVARIANT a la valeur TRUE, le type de colonne est retourné en tant que DBTYPE_SQLVARIANT, auquel cas la mémoire tampon contiendra la structure SSVARIANT. Dans une extraction **sql_variant** données en tant que DBTYPE_SQLVARIANT, la propriété de session SSPROP_ALLOWNATIVEVARIANT doit être définie sur TRUE.  
  
 La propriété SSPROP_ALLOWNATIVEVARIANT est une propriété de session et fait partie du jeu de propriétés DBPROPSET_SQLSERVERSESSION spécifique au fournisseur.  
  
 DBTYPE_VARIANT s'applique à tous les autres fournisseurs OLE DB.  
  
## <a name="sspropallownativevariant"></a>SSPROP_ALLOWNATIVEVARIANT  
 SSPROP_ALLOWNATIVEVARIANT est une propriété de session et fait partie du jeu de propriétés DBPROPSET_SQLSERVERSESSION.  
  
|||  
|-|-|  
|SSPROP_ALLOWNATIVEVARIANT|Type : VT_BOOL<br /><br /> Lecture/écriture : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE<br /><br /> Description : détermine si les données sont extraites en tant que DBTYPE_VARIANT ou DBTYPE_SQLVARIANT.<br /><br /> VARIANT_TRUE : le type de colonne est retourné en tant que DBTYPE_SQLVARIANT, auquel cas la mémoire tampon contient la structure SSVARIANT.<br /><br /> VARIANT_FALSE : le type de colonne est retourné en tant que DBTYPE_VARIANT et la mémoire tampon a la structure VARIANT.|  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données & #40 ; OLE DB & #41 ;](../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
  
