---
title: DataTypeEnum | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- DataTypeEnum
helpviewer_keywords:
- DataTypeEnum enumeration [ADO]
ms.assetid: 2c57eca6-9336-4b06-ba10-9fef5926b1d0
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5ca60e4fda2319b9b63d7c0b7c0162cfd8027063
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datatypeenum"></a>DataTypeEnum
Spécifie le type de données d’une [champ](../../../ado/reference/ado-api/field-object.md), [paramètre](../../../ado/reference/ado-api/parameter-object.md), ou [propriété](../../../ado/reference/ado-api/property-object-ado.md). L’indicateur de type OLE DB correspondant est affichée entre parenthèses dans la colonne description du tableau suivant.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**AdArray**|0x2000|Une valeur d’indicateur, toujours combinée avec une autre constante de type de données, qui indique un tableau de l’autre type de données. Ne s’applique pas à ADOX.|  
|**adBigInt**|20|Indique un entier signé de 8 octets (DBTYPE_I8).|  
|**adBinary**|128|Indique une valeur binaire (DBTYPE_BYTES).|  
|**adBoolean**|11|Indique un **booléenne** valeur (DBTYPE_BOOL).|  
|**adBSTR**|8|Indique une chaîne de caractères terminée par null (Unicode) (DBTYPE_BSTR).|  
|**adChapter**|136|Indique une valeur de chapitre de 4 octets qui identifie les lignes dans un ensemble de lignes enfant (DBTYPE_HCHAPTER).|  
|**adChar**|129|Indique une valeur de chaîne (DBTYPE_STR).|  
|**adCurrency**|6|Indique une valeur monétaire (DBTYPE_CY). Devise est un nombre à virgule fixe avec quatre chiffres à droite de la virgule décimale. Il est stocké dans un entier signé de 8 octets multiplié par 10 000.|  
|**adDate**|7|Indique une valeur de date (DBTYPE_DATE). Une date est stockée comme un double, la partie entière est le nombre de jours depuis le 30 décembre 1899, et dont la partie fractionnaire est la fraction d’un jour.|  
|**adDBDate**|133|Indique une valeur de date (AAAAMMJJ) (DBTYPE_DBDATE).|  
|**adDBTime**|134|Indique une valeur d’heure (hhmmss) (DBTYPE_DBTIME).|  
|**adDBTimeStamp**|135|Indique un cachet de date/heure (aaaammjjhhmmss plus une fraction en milliardièmes) (DBTYPE_DBTIMESTAMP).|  
|**adDecimal**|14|Indique une valeur numérique exacte avec une précision et échelle fixes (DBTYPE_DECIMAL).|  
|**adDouble**|5|Indique la valeur à virgule flottante double précision (DBTYPE_R8).|  
|**adEmpty**|0|Ne spécifie aucune valeur (DBTYPE_EMPTY).|  
|**adError**|10|Indique un code d’erreur 32 bits (DBTYPE_ERROR).|  
|**adFileTime**|64|Indique une valeur 64 bits représentant le nombre d’intervalles de 100 nanosecondes depuis le 1er janvier 1601 (DBTYPE_FILETIME).|  
|**adGUID**|72|Indique un identificateur global unique (GUID) (DBTYPE_GUID).|  
|**adIDispatch**|9|Indique un pointeur vers un **IDispatch** interface sur un objet COM (DBTYPE_IDISPATCH).<br /><br /> **Remarque** ce type de données n’est actuellement pas pris en charge par ADO. L’utilisation peut entraîner des résultats imprévisibles.|  
|**adInteger**|3|Indique un entier signé de 4 octets (DBTYPE_I4).|  
|**adIUnknown**|13|Indique un pointeur vers un **IUnknown** interface sur un objet COM (DBTYPE_IUNKNOWN).<br /><br /> **Remarque** ce type de données n’est actuellement pas pris en charge par ADO. L’utilisation peut entraîner des résultats imprévisibles.|  
|**adLongVarBinary**|205|Indique une valeur binaire.|  
|**adLongVarChar**|201|Indique une valeur de chaîne longue.|  
|**adLongVarWChar**|203|Indique une valeur de chaîne Unicode longue se terminant par null.|  
|**adNumeric**|131|Indique une valeur numérique exacte avec une précision et échelle fixes (DBTYPE_NUMERIC).|  
|**adPropVariant**|138|Indique une automatisation PROPVARIANT (DBTYPE_PROP_VARIANT).|  
|**adSingle**|4|Indique la valeur à virgule flottante simple précision (DBTYPE_R4).|  
|**adSmallInt**|2|Indique un entier signé de deux octets (DBTYPE_I2).|  
|**adTinyInt**|16|Indique un entier signé de 1 octet (DBTYPE_I1).|  
|**adUnsignedBigInt**|21|Indique un entier non signé de 8 octets (DBTYPE_UI8).|  
|**adUnsignedInt**|19|Indique un entier non signé de 4 octets (DBTYPE_UI4).|  
|**adUnsignedSmallInt**|18|Indique un entier non signé de 2 octets (DBTYPE_UI2).|  
|**adUnsignedTinyInt**|17|Indique un entier non signé de 1 octet (DBTYPE_UI1).|  
|**adUserDefined**|132|Indique une variable définie par l’utilisateur (DBTYPE_UDT).|  
|**adVarBinary**|204|Indique une valeur binaire.|  
|**adVarChar**|200|Indique une valeur de chaîne.|  
|**adVariant**|12|Indique un objet Automation **Variant** (DBTYPE_VARIANT).<br /><br /> **Remarque** ce type de données n’est actuellement pas pris en charge par ADO. L’utilisation peut entraîner des résultats imprévisibles.|  
|**adVarNumeric**|139|Indique une valeur numérique.|  
|**adVarWChar**|202|Indique une chaîne de caractères Unicode se terminant par null.|  
|**adWChar**|130|Indique une chaîne terminée par le caractère null de caractères Unicode (DBTYPE_WSTR).|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Package : **com.ms.wfc.data**  
  
|Constante|  
|--------------|  
|AdoEnums.DataType.ARRAY|  
|AdoEnums.DataType.BIGINT|  
|AdoEnums.DataType.BINARY|  
|AdoEnums.DataType.BOOLEAN|  
|AdoEnums.DataType.BSTR|  
|AdoEnums.DataType.CHAPTER|  
|AdoEnums.DataType.CHAR|  
|AdoEnums.DataType.CURRENCY|  
|AdoEnums.DataType.DATE|  
|AdoEnums.DataType.DBDATE|  
|AdoEnums.DataType.DBTIME|  
|AdoEnums.DataType.DBTIMESTAMP|  
|AdoEnums.DataType.DECIMAL|  
|AdoEnums.DataType.DOUBLE|  
|AdoEnums.DataType.EMPTY|  
|AdoEnums.DataType.ERROR|  
|AdoEnums.DataType.FILETIME|  
|AdoEnums.DataType.GUID|  
|AdoEnums.DataType.IDISPATCH|  
|AdoEnums.DataType.INTEGER|  
|AdoEnums.DataType.IUNKNOWN|  
|AdoEnums.DataType.LONGVARBINARY|  
|AdoEnums.DataType.LONGVARCHAR|  
|AdoEnums.DataType.LONGVARWCHAR|  
|AdoEnums.DataType.NUMERIC|  
|AdoEnums.DataType.PROPVARIANT|  
|AdoEnums.DataType.SINGLE|  
|AdoEnums.DataType.SMALLINT|  
|AdoEnums.DataType.TINYINT|  
|AdoEnums.DataType.UNSIGNEDBIGINT|  
|AdoEnums.DataType.UNSIGNEDINT|  
|AdoEnums.DataType.UNSIGNEDSMALLINT|  
|AdoEnums.DataType.UNSIGNEDTINYINT|  
|AdoEnums.DataType.USERDEFINED|  
|AdoEnums.DataType.VARBINARY|  
|AdoEnums.DataType.VARCHAR|  
|AdoEnums.DataType.VARIANT|  
|AdoEnums.DataType.VARNUMERIC|  
|AdoEnums.DataType.VARWCHAR|  
|AdoEnums.DataType.WCHAR|  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Append, méthode (ADO)](../../../ado/reference/ado-api/append-method-ado.md)|[CreateParameter, méthode (ADO)](../../../ado/reference/ado-api/createparameter-method-ado.md)|  
|[CreateRecordset, méthode (RDS)](../../../ado/reference/rds-api/createrecordset-method-rds.md)|[Type, propriété (ADO)](../../../ado/reference/ado-api/type-property-ado.md)|
