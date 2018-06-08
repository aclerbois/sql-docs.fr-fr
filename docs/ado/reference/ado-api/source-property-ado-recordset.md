---
title: Source, propriété (jeu d’enregistrements ADO) | Documents Microsoft
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
- Recordset15::putref_Source
- Recordset15::Source
- Recordset15::PutSource
- Recordset15::get_Source
- Recordset15::GetSource
- Recordset15::PutRefSource
- Recordset15::put_Source
helpviewer_keywords:
- Source property [ADO Recordset]
ms.assetid: a05ba2c9-2821-4343-8607-4de9b764ec91
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fff874c34b527adc976b608e6b1594427245de24
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="source-property-ado-recordset"></a>Source, propriété (jeu d’enregistrements ADO)
Indique la source de données pour un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit un **chaîne** valeur ou [commande](../../../ado/reference/ado-api/command-object-ado.md) objet référence ; retourne uniquement un **chaîne** valeur qui indique la source de la **Recordset**.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **Source** propriété pour spécifier une source de données pour un **Recordset** en utilisant l’une des opérations suivantes : une **commande** de l’objet variable, une instruction SQL, une procédure stockée, ou un nom de table.  
  
 Si vous définissez la **Source** propriété un **commande** objet, le [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) propriété de la **Recordset** objet hérite les valeur de la **ActiveConnection** propriété spécifié **commande** objet. Toutefois, lors de la lecture le **Source** propriété ne retourne pas une **commande** de l’objet ; au lieu de cela, elle retourne le [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) propriété de la **commande** de l’objet à laquelle vous avez défini le **Source** propriété.  
  
 Si le **Source** propriété est une instruction SQL, une procédure stockée ou un nom de table, vous pouvez optimiser les performances en passant approprié *Options* argument avec le [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md)appel de méthode.  
  
 Le **Source** propriété est en lecture/écriture pour fermé **Recordset** objets et en lecture seule pour ouvrir **Recordset** objets.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété source (VB)](../../../ado/reference/ado-api/source-property-example-vb.md)   
 [Source, propriété (erreur ADO)](../../../ado/reference/ado-api/source-property-ado-error.md)   
 [Source, propriété (objet Record ADO)](../../../ado/reference/ado-api/source-property-ado-record.md)
