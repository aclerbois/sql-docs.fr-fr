---
title: Utiliser l’option BINARY BASE64 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- AUTO FOR XML mode, BINARY BASE64 option
ms.assetid: 86a7bb85-7f83-412a-b775-d2c379702fe9
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0a1ca037d0e79105d150332195bdbda4defc3f24
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-the-binary-base64-option"></a>Utiliser l'option BINARY BASE64
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Si l'option BINARY BASE64 est spécifiée dans la requête, les données binaires sont renvoyées dans un format encodé en base 64. Par défaut, si l'option BINARY BASE64 n'est pas spécifiée, le mode AUTO prend en charge l'encodage URL des données binaires. Dans ce cas, au lieu de données binaires, une référence à une URL relative vers la racine virtuelle de la base de données dans laquelle la requête est exécutée est renvoyée. Cette référence permet d'accéder aux données binaires réelles lors les opérations ultérieures à l'aide de la requête dbobject SQLXML ISAPI. La requête doit fournir suffisamment d'informations, telles que des colonnes clés primaires, pour identifier l'image.  
  
 Lors de la spécification d'une requête, si un alias est utilisé pour la colonne binaire de la vue, il est renvoyé dans l'encodage URL des données binaires. Dans les opérations suivantes, l'alias ne signifie rien et l'encodage URL ne peut pas être utilisé pour extraire l'image. Par conséquent, n'utilisez pas d'alias lors de l'interrogation d'une vue à l'aide du mode FOR XML AUTO.  
  
 Par exemple, dans une requête SELECT, la conversion d'une colonne en type de données d'objet blob en fait une entité temporaire puisqu'elle perd les noms de table et de colonne qui lui sont associés. De ce fait, les requêtes exprimées en mode AUTO génèrent une erreur car elles ne savent pas où placer cette valeur dans la hiérarchie XML. Exemple :  
  
```  
CREATE TABLE MyTable (Col1 int PRIMARY KEY, Col2 binary)  
INSERT INTO MyTable VALUES (1, 0x7);  
```  
  
 Cette requête produit une erreur en raison de la conversion en type de données d'objet blob.  
  
```  
SELECT Col1,  
CAST(Col2 as image) as Col2  
FROM MyTable  
FOR XML AUTO;  
```  
  
 La solution consiste à ajouter l'option BINARY BASE64 dans la clause FOR XML. Si vous supprimez la conversion, la requête produit les résultats escomptés :  
  
```  
SELECT Col1,  
CAST(Col2 as image) as Col2  
FROM MyTable  
FOR XML AUTO, BINARY BASE64;  
```  
  
 Voici le résultat obtenu :  
  
```  
<MyTable Col1="1" Col2="Bw==" />  
```  
  
## <a name="see-also"></a> Voir aussi  
 [UTiliser le mode AUTO avec FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)  
  
  
