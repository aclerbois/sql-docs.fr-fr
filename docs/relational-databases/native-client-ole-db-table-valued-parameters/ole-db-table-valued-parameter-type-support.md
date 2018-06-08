---
title: Prise en charge de Type de paramètre table OLE DB | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-table-valued-parameters
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (OLE DB), API support (OLE DB)
ms.assetid: 147036a0-260e-4f81-8b3b-89209e023a32
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: c5e3643eb70e25a9725c439c2375fe4c633aa760
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-table-valued-parameter-type-support"></a>Prise en charge du type de paramètre table OLE DB
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Cette rubrique décrit la prise en charge du type OLE DB pour les paramètres table.  
  
## <a name="table-valued-parameter-rowset-object"></a>Objet d'ensemble de lignes de paramètre table  
 Vous pouvez créer un objet d'ensemble de lignes spécialisé pour des paramètres table. Vous créez l’objet d’ensemble de lignes de paramètre table à l’aide de ITableDefinitionWithConstraints::CreateTableWithConstraints ou IOpenRowset::OpenRowset. Pour ce faire, définissez la *eKind* membre de la *pTableID* paramètre sur DBKIND_GUID_NAME et fournissez le CLSID_ROWSET_INMEMORY comme le *guid* membre. Le nom de type de serveur pour le paramètre table doit être spécifié dans le *pwszName* membre *pTableID* lors de l’utilisation de IOpenRowset::OpenRowset. L'objet d'ensemble de lignes de paramètre table se comporte comme un objet Fournisseur OLE DB SQL Server Native Client normal.  
  
```  
const GUID CLSID_ROWSET_TVP =   
{0xc7ef28d5, 0x7bee, 0x443f, {0x86, 0xda, 0xe3, 0x98, 0x4f, 0xcd, 0x4d, 0xf9}};  
  
CoType RowsetTVP  
{  
[mandatory] interface IAccessor;  
[mandatory] interface IColumnsInfo;  
[mandatory] interface IConvertType;  
[mandatory] interface IRowset;  
[mandatory] interface IRowsetInfo;  
[optional]  interface IColumnsRowset;  
[optional]  interface IRowsetChange;  
[optional]  interface ISupportErrorInfo;  
};  
```  
  
## <a name="dbtypetable"></a>DBTYPE_TABLE  
 DBTYPE_TABLE est un nouveau type qui représente un type de table. Ce type spécifie des paramètres table dans différentes interfaces OLE DB où un DBTYPE est requis.  
  
```  
#define DBTYPE_TABLE (143)  
```  
  
 DBTYPE_TABLE a le même format que DBTYPE_IUNKNOWN. Il s'agit d'un pointeur vers un objet dans le tampon de données. Pour une spécification complète dans les liaisons, le consommateur remplit la mémoire tampon DBOBJECT, avec *iid* défini sur l’une des interfaces objet ensemble de lignes (IID_IRowset). Si aucun DBOBJECT n’est spécifié dans les liaisons, IID_IRowset est supposé.  
  
 Les conversions vers et depuis DBTYPE_TABLE pour tout autre type ne sont pas prises en charge. IConvertType::CanConvert retourne S_FALSE pour les conversions non prises en charge pour toute demande autre qu’à une conversion de DBTYPE_TABLE. Cela suppose DBCONVERTFLAGS_PARAMETER sur l’objet de commande.  
  
## <a name="methods"></a>Méthodes  
 Pour plus d’informations sur les méthodes OLE DB qui prennent en charge les paramètres table, consultez [prennent en charge de Type de paramètre OLE DB Table-Valued &#40;méthodes&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-methods.md).  
  
## <a name="properties"></a>Propriétés  
 Pour plus d’informations sur les propriétés OLE DB qui prennent en charge les paramètres table, consultez [prennent en charge de Type de paramètre OLE DB Table-Valued &#40;propriétés&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres table &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Utilisez la Table-Valued paramètres & #40 ; OLE DB & #41 ;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
