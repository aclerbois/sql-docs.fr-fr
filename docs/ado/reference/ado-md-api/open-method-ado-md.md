---
title: Open (méthode) (ADO MD) | Documents Microsoft
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
- Open
- Cellset::Open
helpviewer_keywords:
- Open method [ADO MD]
ms.assetid: a87d8080-a238-45e5-bc80-9a8625b3810f
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c6fb508318c54e4b82a4efd301b889baadad6edc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="open-method-ado-md"></a>Open (méthode) (ADO MD)
Récupère les résultats d’une requête multidimensionnelle et renvoie les résultats vers un [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Cellset.Open Source, ActiveConnection  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 Ce paramètre est facultatif. A **Variant** qui correspond à une requête multidimensionnelle valide, par exemple une requête MDX (Multidimensional Expression). Le *Source* argument correspond à la [Source](../../../ado/reference/ado-md-api/source-property-ado-md.md) propriété. Pour plus d’informations sur MDX, consultez le [OLE DB pour OLAP Online Analytical Processing ()](http://msdn.microsoft.com/en-us/8a7673c6-3ca1-4411-9f1e-adf1e47df4f3) documentation dans le Microsoft Data Access Components SDK.  
  
 *ActiveConnection*  
 Ce paramètre est facultatif. A **Variant** qui correspond à une chaîne spécifiant un ADO valide [connexion](../../../ado/reference/ado-api/connection-object-ado.md) de l’objet nom de variable ou une définition d’une connexion. Le *ActiveConnection* argument spécifie la connexion dans lequel ouvrir le [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md) objet. Si vous passez une définition de connexion pour cet argument, ADO ouvre une nouvelle connexion à l’aide des paramètres spécifiés. Le *ActiveConnection* argument correspond à la [ActiveConnection](../../../ado/reference/ado-md-api/activeconnection-property-ado-md.md) propriété.  
  
## <a name="remarks"></a>Notes  
 Le **ouvrir** méthode génère une erreur si un de ses paramètres est omis et que sa valeur de propriété correspondante n’a pas été définie avant d’essayer d’ouvrir le **ensemble de cellules**.  
  
## <a name="applies-to"></a>S'applique à  
 [Cellset, objet (ADO MD)](../../../ado/reference/ado-md-api/cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple d’ensemble de cellules (VB)](../../../ado/reference/ado-md-api/cellset-example-vb.md)   
 [ActiveConnection, propriété (ADO MD)](../../../ado/reference/ado-md-api/activeconnection-property-ado-md.md)   
 [Close (méthode) (ADO MD)](../../../ado/reference/ado-md-api/close-method-ado-md.md)   
 [Objet de connexion (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Source, propriété (ADO MD)](../../../ado/reference/ado-md-api/source-property-ado-md.md)   
 [State, propriété (ADO MD)](../../../ado/reference/ado-md-api/state-property-ado-md.md)
