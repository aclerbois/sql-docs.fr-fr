---
title: Types CLR volumineux définis par l’utilisateur (OLE DB) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- large CLR user-defined types [OLE DB]
ms.assetid: 4bf12058-0534-42ca-a5ba-b1c23b24d90f
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6959f7e6993a6d9f024a8201056f57c0b85b54f1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="large-clr-user-defined-types-ole-db"></a>Types CLR volumineux définis par l'utilisateur (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Cette rubrique traite des modifications apportées à OLE DB dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client pour prendre en charge les types définis par l'utilisateur (UDT) du CLR (Common Language Runtime) volumineux.  
  
 Pour plus d’informations sur la prise en charge pour les UDT volumineux du CLR dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, consultez [Large CLR User-Defined Types](../../../relational-databases/native-client/features/large-clr-user-defined-types.md). Pour obtenir un exemple, consultez [utilisation grand CLR UDT &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-how-to/use-large-clr-udts-ole-db.md).  
  
## <a name="data-format"></a>Format de données  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client utilise ~0 pour représenter la longueur des valeurs qui sont de taille illimitée pour les types d'objets volumineux (LOB). ~0 représente également la taille des types UDT du CLR supérieurs à 8 000 octets.  
  
 Le tableau suivant montre le mappage des types de données dans les paramètres et les ensembles de lignes :  
  
|Type de données SQL Server|Type de données OLE DB|Disposition en mémoire|Valeur|  
|--------------------------|----------------------|-------------------|-----------|  
|UDT CLR|DBTYPE_UDT|BYTE [] (tableau d’octets\)|132 (oledb.h)|  
  
 Les valeurs UDT sont représentées sous la forme de tableaux d'octets. Les conversions en chaînes hexadécimales de même que les conversions depuis des chaînes hexadécimales sont prises en charge. Les valeurs littérales sont représentées sous forme de chaînes hexadécimales avec le préfixe « 0x ». Une chaîne hexadécimale est la représentation textuelle de données binaires en base 16. Un exemple est une conversion de type de serveur **varbinary(10)** à DBTYPE_STR, ce qui entraîne une représentation hexadécimale de 20 caractères où chaque paire de caractères représente un octet unique.  
  
## <a name="parameter-properties"></a>Propriétés de paramètre  
 Le jeu de propriétés DBPROPSET_SQLSERVERPARAMETER prend en charge les types UDT via OLE DB. Pour plus d’informations, consultez [Defined Types](~/relational-databases/native-client/features/using-user-defined-types.md).  
  
## <a name="column-properties"></a>Propriétés de colonne  
 Le jeu de propriétés DBPROPSET_SQLSERVERCOLUMN prend en charge la création de tables via OLE DB. Pour plus d’informations, consultez [Defined Types](~/relational-databases/native-client/features/using-user-defined-types.md).  
  
## <a name="data-type-mapping-in-itabledefinitioncreatetable"></a>Mappage des types de données dans ITableDefinition::CreateTable  
 Les informations suivantes sont utilisées dans **DBCOLUMNDESC** structures utilisées par ITableDefinition::CreateTable lorsque les colonnes UDT sont requises :  
  
|Type de données OLE DB (*wType*)|*pwszTypeName*|Type de données SQL Server|*rgPropertySets*|  
|----------------------------------|--------------------|--------------------------|----------------------|  
|DBTYPE_UDT|Ignoré|UDT|Doit inclure un jeu de propriétés DBPROPSET_SQLSERVERCOLUMN.|  
  
## <a name="icommandwithparametersgetparameterinfo"></a>ICommandWithParameters::GetParameterInfo  
 Informations retournées dans la structure DBPARAMINFO via **prgParamInfo** est comme suit :  
  
|Type de paramètre|*wType*|*ulParamSize*|*bPrecision*|*bScale*|*dwFlags* DBPARAMFLAGS_ISLONG|  
|--------------------|-------------|-------------------|------------------|--------------|------------------------------------|  
|DBTYPE_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|"DBTYPE_UDT"|*n*|non défini|non défini|clear|  
|DBTYPE_UDT<br /><br /> (longueur supérieure à 8 000 octets)|"DBTYPE_UDT"|~0|non défini|non défini|jeu|  
  
## <a name="icommandwithparameterssetparameterinfo"></a>ICommandWithParameters::SetParameterInfo  
 Les informations fournies dans la structure DBPARAMBINDINFO doivent respecter les conditions suivantes :  
  
|Type de paramètre|*pwszDataSourceType*|*ulParamSize*|*bPrecision*|*bScale*|*dwFlags* DBPARAMFLAGS_ISLONG|  
|--------------------|--------------------------|-------------------|------------------|--------------|------------------------------------|  
|DBTYPE_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|DBTYPE_UDT|*n*|ignoré|ignoré|Doit être défini si le paramètre doit être passé à l'aide de DBTYPE_IUNKNOWN.|  
|DBTYPE_UDT<br /><br /> (longueur supérieure à 8 000 octets)|DBTYPE_UDT|~0|ignoré|ignoré|ignoré|  
  
## <a name="isscommandwithparameters"></a>ISSCommandWithParameters  
 Les applications utilisent **ISSCommandWithParameters** pour obtenir et définir les propriétés des paramètres définies dans la section des propriétés de paramètre.  
  
## <a name="icolumnsrowsetgetcolumnsrowset"></a>IColumnsRowset::GetColumnsRowset  
 Les colonnes retournées sont les suivantes :  
  
|Type de colonne|DBCOLUMN_TYPE|DBCOLUMN_COLUMNSIZE|DBCOLUMN_PRECISION|DBCOLUMN_SCALE|DBCOLUMN_FLAGS_ISLONG|DBCOLUMNS_ISSEARCHABLE|DBCOLUMN_OCTETLENGTH|  
|-----------------|--------------------|--------------------------|-------------------------|---------------------|-----------------------------|-----------------------------|---------------------------|  
|DBTYPE_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|DBTYPE_UDT|*n*|NULL|NULL|Désactiver|DB_ALL_EXCEPT_LIKE|n|  
|DBTYPE_UDT<br /><br /> (longueur supérieure à 8 000 octets)|DBTYPE_UDT|~0|NULL|NULL|Définissez|DB_ALL_EXCEPT_LIKE|0|  
  
 Les colonnes suivantes sont également définies pour les types UDT :  
  
|Identificateur de colonne|Type| Description|  
|-----------------------|----------|-----------------|  
|DBCOLUMN_UDT_CATALOGNAME|DBTYPE_WSTR|Pour les colonnes UDT, il s'agit du nom du catalogue où le type UDT est défini.|  
|DBCOLUMN_UDT_SCHEMANAME|DBTYPE_WSTR|Pour les colonnes UDT, il s'agit du nom du schéma où le type UDT est défini.|  
|DBCOLUMN_UDT_NAME|DBTYPE_WSTR|Pour les colonnes UDT, il s'agit du nom en une partie du type UDT.|  
|DBCOLUMN_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Pour les colonnes UDT, le nom de type complet de l’UDT. Le nom complet du type d'assembly vous permet d'instancier un objet de ce type à l'aide de la méthode Type.GetType.|  
  
## <a name="icolumnsinfogetcolumninfo"></a>IColumnsInfo::GetColumnInfo  
 Les informations retournées dans la structure DBCOLUMNINFO sont les suivantes :  
  
|Type de paramètre|*wType*|*ulColumnSize*|*bPrecision*|*bScale*|*dwFlags*<br /><br /> DBCOLUMNFLAGS_ISLONG|  
|--------------------|-------------|--------------------|------------------|--------------|-----------------------------------------|  
|DBTYPE_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|DBTYPE_UDT|*n*|~0|~0|Désactiver|  
|DBTYPE_UDT<br /><br /> (longueur supérieure à 8 000 octets)|DBTYPE_UDT|~0|~0|~0|Définissez|  
  
## <a name="columns-rowset-schema-rowsets"></a>Ensemble de lignes COLUMNS (ensembles de lignes de schéma)  
 Les valeurs de colonnes suivantes sont retournées pour les types UDT :  
  
|Type de colonne|DATA_TYPE|COLUMN_FLAGS, DBCOLUMFLAGS_ISLONG|CHARACTER_OCTET_LENGTH|  
|-----------------|----------------|-----------------------------------------|------------------------------|  
|DBTYPE_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|DBTYPE_UDT|Désactiver|*n*|  
|DBTYPE_UDT<br /><br /> (longueur supérieure à 8 000 octets)|DBTYPE_UDT|Définissez|0|  
  
 Les colonnes supplémentaires suivantes sont définies pour les types UDT :  
  
|Identificateur de la colonne|Type| Description|  
|-----------------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|Pour les colonnes UDT, il s'agit du nom du catalogue où le type UDT est défini.|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|Pour les colonnes UDT, il s'agit du nom du schéma où le type UDT est défini.|  
|SS_UDT_NAME|DBTYPE_WSTR|Pour les colonnes UDT, il s'agit du nom en une partie du type UDT.|  
|SS_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Pour les colonnes UDT, il s'agit du nom de type complet du type UDT. Le nom complet du type d'assembly vous permet d'instancier un objet de ce type à l'aide de la méthode Type.GetType.|  
  
 En ce qui concerne l'ensemble de lignes PROCEDURE_PARAMETERS, DATA_TYPE contient les mêmes valeurs que l'ensemble de lignes de schéma COLUMNS et TYPE_NAME contient UDT. Les mêmes colonnes supplémentaires sont également définies.  
  
 Les types définis par l'utilisateur n'apparaîtront pas dans l'ensemble de lignes de schéma PROVIDER_TYPES.  
  
## <a name="bindings-and-conversions"></a>Liaisons et conversions  
  
|Type de données de liaison|UDT vers serveur|Non-UDT vers serveur|UDT à partir du serveur|Non-UDT à partir du serveur|  
|----------------------|-------------------|------------------------|---------------------|--------------------------|  
|DBTYPE_UDT|Prise en charge (5)|Erreur (1)|Prise en charge (5)|Erreur (4)|  
|DBTYPE_BYTES|Prise en charge (5)|Néant|Prise en charge (5)|Néant|  
|DBTYPE_WSTR|Pris en charge (2), (5)|Néant|Pris en charge (3), (5), (6)|Néant|  
|DBTYPE_BSTR|Pris en charge (2), (5)|Néant|Prise en charge (3), (5)|Néant|  
|DBTYPE_STR|Pris en charge (2), (5)|Néant|Prise en charge (3), (5)|Néant|  
|DBTYPE_IUNKNOWN|Prise en charge (6)|Néant|Prise en charge (6)|Néant|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|Pris en charge (5)|Néant|Prise en charge (3), (5)|Néant|  
|DBTYPE_VARIANT (VT_BSTR)|Pris en charge (2), (5)|Néant|Néant|Néant|  
  
### <a name="key-to-symbols"></a>Liste des symboles  
  
|Symbole|Signification|  
|------------|-------------|  
|1|Si un type de serveur autre que DBTYPE_UDT est spécifié avec **ICommandWithParameters::SetParameterInfo** et le type d’accesseur est DBTYPE_UDT, une erreur se produit lorsque l’instruction est exécutée.  L'erreur sera DB_E_ERRORSOCCURRED et l'état du paramètre sera DBSTATUS_E_BADACCESSOR.<br /><br /> Le fait de spécifier un paramètre de type UDT pour un paramètre serveur qui n'est pas un type UDT constitue une erreur.|  
|2|Les données sont converties d'une chaîne hexadécimale en données binaires.|  
|3|Les données sont converties données binaires en chaîne hexadécimale.|  
|4|La validation peut se produire lorsque vous utilisez **CreateAccessor** ou **GetNextRows**. L'erreur est DB_E_ERRORSOCCURRED. L'état de liaison a la valeur DBBINDSTATUS_UNSUPPORTEDCONVERSION.|  
|5|BY_REF peut être utilisé.|  
|6|Les paramètres UDT peuvent être liés en tant que DBTYPE_IUNKNOWN dans le DBBINDING. Liaison à DBTYPE_IUNKNOWN indique que l’application souhaite traiter les données en tant que flux à l’aide de l’interface ISequentialStream. Lorsqu’un consommateur spécifie *wType* dans une liaison en tant que type DBTYPE_IUNKNOWN et la colonne correspondante ou la sortie, le paramètre de la procédure stockée est un UDT, SQL Server Native Client retourne ISequentialStream. Pour un paramètre d’entrée, SQL Server Native Client recherchera le pour l’interface ISequentialStream.<br /><br /> Vous pouvez choisir de ne pas lier la longueur de données UDT à l'aide de la liaison DBTYPE_IUNKNOWN en cas types UDT volumineux. Toutefois, la longueur doit être liée pour de petits types UDT. Un paramètre DBTYPE_UDT peut être spécifié en tant que type UDT volumineux si une ou plusieurs des conditions suivantes sont réunies :<br />*ulParamParamSize* est ~ 0.<br />DBPARAMFLAGS_ISLONG est défini dans le struct DBPARAMBINDINFO.<br /><br /> Pour les données de ligne, la liaison DBTYPE_IUNKNOWN est uniquement autorisée pour les types UDT volumineux. Vous pouvez déterminer si une colonne est un type UDT volumineux à l’aide de la méthode IColumnsInfo::GetColumnInfo sur un ensemble de lignes ou l’interface IColumnsInfo de l’objet de commande. Une colonne DBTYPE_UDT est une colonne UDT volumineuse si une ou plusieurs des conditions suivantes sont réunies :<br />Indicateur DBCOLUMNFLAGS_ISLONG est défini sur *dwFlags* membre de la structure DBCOLUMNINFO. <br />*ulColumnSize* membre de DBCOLUMNINFO est ~ 0.|  
  
 DBTYPE_NULL et DBTYPE_EMPTY peuvent être liés pour des paramètres d'entrée, mais pas pour des résultats ou des paramètres de sortie. S'ils sont liés pour des paramètres d'entrée, l'état doit avoir la valeur DBSTATUS_S_ISNULL pour DBTYPE_NULL ou DBSTATUS_S_DEFAULT pour DBTYPE_EMPTY. DBTYPE_BYREF ne peut pas être utilisé avec DBTYPE_NULL ou DBTYPE_EMPTY.  
  
 DBTYPE_UDT peut également être converti en DBTYPE_EMPTY et en DBTYPE_NULL. Toutefois, DBTYPE_NULL et DBTYPE_EMPTY ne peuvent pas être convertis en DBTYPE_UDT, ce qui est cohérent avec DBTYPE_BYTES. **ISSCommandWithParameters** est utilisé pour traiter les types UDT en tant que paramètres.  
  
 Conversions de données fournies par les services principaux OLE DB (**IDataConvert**) ne sont pas applicables à DBTYPE_UDT.  
  
 Aucune autre liaison n'est prise en charge.  
  
## <a name="comparability-for-irowsetfind"></a>Comparabilité pour IRowsetFind  
 Pour les types UDT, seules les comparaisons suivantes sont prises en charge :  
  
-   EQ  
  
-   NE  
  
-   IGNORE  
  
 Si une autre comparaison est tentée, DB_E_BADCOMPAREOP est retourné.  
  
## <a name="bcp-support-for-udts"></a>Prise en charge des UDT par BCP  
 Les valeurs UDT peuvent être uniquement importées et exportées en tant que valeurs de caractère ou valeurs binaires.  
  
## <a name="down-level-client-behavior-for-udts"></a>Comportement client de bas niveau pour les types UDT  
 Les UDT sont mappés aux clients de niveau inférieur de la manière suivante :  
  
|Version du client|DBTYPE_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|DBTYPE_UDT<br /><br /> (longueur supérieure à 8 000 octets)|  
|--------------------|------------------------------------------------------------------|---------------------------------------------------------|  
|SQL Server 2005|UDT|varbinary(max)|  
|SQL Server 2008 et versions ultérieur|UDT|UDT|  
  
 Lorsque **DataTypeCompatibility** (SSPROP_INIT_DATATYPECOMPATIBILITY) a la valeur « 80 », les types UDT volumineux apparaissent aux clients de la même façon qu’ils s’affichent pour les clients de bas niveau.  
  
## <a name="see-also"></a>Voir aussi  
 [Types CLR volumineux définis par l’utilisateur](~/relational-databases/native-client/features/large-clr-user-defined-types.md)  
  
  

