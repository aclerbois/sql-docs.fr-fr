---
title: Champ (ADO - syntaxe WFC) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- Field collection [ADO], ADO/WFC syntax
ms.assetid: 7e01cb24-2338-4f92-ad46-8d97248e1a4d
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7b355a11d2916403c7574c547aac02c61d7b0d71
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="field-ado---wfc-syntax"></a>Champ (ADO - syntaxe WFC)
## <a name="package-commswfcdata"></a>package com.ms.wfc.data  
  
### <a name="methods"></a>Méthodes  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
public byte[] getByteChunk(int len)  
public char[] getCharChunk(int len)  
public String getStringChunk(int len)  
```  
  
### <a name="properties"></a>Propriétés  
  
```  
public int getActualSize()  
public int getAttributes()  
public void setAttributes(int pl)  
public com.ms.com.IUnknown getDataFormat()  
public void setDataFormat(com.ms.com.IUnknown format)  
```  
  
 (Pour plus d’informations, consultez la documentation de l’interface com.ms.wfc.data.IDataFormat.)  
  
```  
public int getDefinedSize()  
public void setDefinedSize(int pl)  
public String getName()  
public int getNumericScale()  
public void setNumericScale(byte pbNumericScale)  
public Variant getOriginalValue()  
public int getPrecision()  
public void setPrecision(byte pbPrecision)  
public int getType()  
public void setType(int pDataType)  
public Variant getUnderlyingValue()  
public Variant getValue()  
public void setValue(Variant value)  
public AdoProperties getProperties()  
```  
  
### <a name="field-accessor-methods"></a>Méthodes d’accesseur de champ  
 Le [valeur](../../../ado/reference/ado-api/value-property-ado.md) propriété d’un [champ](../../../ado/reference/ado-api/field-object.md) objet Obtient ou définit le contenu de cet objet. Le contenu est représenté en tant que VARIANT, un type d’objet qui peut avoir une valeur et de plusieurs types de données.  
  
 ADO/WFC implémente la **valeur** propriété avec le **getValue** (méthode), qui retourne un objet de type VARIANT ; et le **setValue** méthode qui prend une variante en tant qu’argument. Les variantes sont très efficaces dans certains langages, tels que Microsoft Visual Basic.  
  
 Outre la **valeur** ADO/WFC fournit des propriétés, *accesseur* les méthodes qui utilisent des types de données Java pour obtenir et définir le contenu de **champ** objets. La plupart de ces méthodes ont des noms de la forme **obtenir *** DataType* ou **Définir *** DataType*.  
  
 Il existe deux exceptions à cela : parmi les **getObject** méthodes retourne un objet converti en une classe spécifiée. Il est sans **getNull** propriété ; au lieu de cela, il existe un **isNull** propriété qui retourne une valeur booléenne qui indique si le champ est null.  
  
```  
public native boolean getBoolean();  
public void setBoolean(boolean v)  
public native byte getByte();  
public void setByte(byte v)  
public native byte[] getBytes();  
public void setBytes(byte[] v)  
public native double getDouble();  
public void setDouble(double v)  
public native float getFloat();  
public void setFloat(float v)  
public native int getInt();  
public void setInt(int v)  
public native long getLong();  
public void setLong(long v)  
public native short getShort();  
public void setShort(short v)  
public native String getString();  
public void setString(String v)  
public native boolean isNull();  
public void setNull()  
public Object getObject()  
public Object getObject(Class c)  
public void setObject(Object value)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Field, objet](../../../ado/reference/ado-api/field-object.md)
