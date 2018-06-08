---
title: Source, propriété (enregistrement ADO) | Documents Microsoft
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
- _Record::Source
- _Record::PutRefSource
- _Record::GetSource
- _Record::put_Source
- _Record::putref_Source
- _Record::get_Source
helpviewer_keywords:
- Source property [ADO Record]
ms.assetid: 2c18279e-6f35-4af0-b12e-8f1543d9ed20
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bff992c2c2406c7ea6df95ca92f67da3bf52e3fc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="source-property-ado-record"></a>Source, propriété (enregistrement ADO)
Indique la source de données ou l’objet représenté par le [enregistrement](../../../ado/reference/ado-api/record-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **Variant** valeur qui indique l’entité représentée par le **enregistrement**.  
  
## <a name="remarks"></a>Notes  
 Le **Source** propriété retourne le *Source* argument de la **enregistrement** objet [ouvrir](../../../ado/reference/ado-api/open-method-ado-record.md) (méthode). Il peut contenir une chaîne d’URL absolue ou relative. Une URL absolue peut être utilisée sans le [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) propriété pour ouvrir directement le **enregistrement** objet. Implicite **connexion** objet est créé dans ce cas.  
  
 Le **Source** propriété peut également contenir une référence à un déjà ouvert **Recordset**, qui ouvre un **enregistrement** objet représentant la ligne actuelle dans le  **Jeu d’enregistrements**.  
  
 Le **Source** propriété peut également contenir une référence à un [commande](../../../ado/reference/ado-api/command-object-ado.md) objet qui retourne une ligne unique de données à partir du fournisseur.  
  
 Si le **ActiveConnection** est également définie, le **Source** propriété doit pointer vers un objet qui existe dans l’étendue de cette connexion. Par exemple, dans les espaces de noms de la structure d’arborescence, si la **Source** propriété contient une URL absolue, elle doit pointer vers un nœud qui existe à l’intérieur de la portée du nœud identifié par l’URL dans la chaîne de connexion. Si le **Source** propriété contient une URL relative, puis elle est validée dans le contexte défini le **ActiveConnection** propriété.  
  
 Le **Source** propriété est en lecture/écriture lors de la **enregistrement** objet est fermé et est en lecture seule lors de la **enregistrement** objet est ouvert.  
  
> [!NOTE]
>  URL à l’aide du modèle http appellent automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Record, objet (ADO)](../../../ado/reference/ado-api/record-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Source, propriété (erreur ADO)](../../../ado/reference/ado-api/source-property-ado-error.md)   
 [Source, propriété (objet Recordset ADO)](../../../ado/reference/ado-api/source-property-ado-recordset.md)
