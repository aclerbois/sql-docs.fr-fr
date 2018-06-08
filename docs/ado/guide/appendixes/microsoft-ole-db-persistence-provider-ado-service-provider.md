---
title: Fournisseur de persistance Microsoft OLE DB (fournisseur de services ADO) | Documents Microsoft
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
helpviewer_keywords:
- providers [ADO], OLE DB persistence provider
- persistence provider [ADO]
- OLE DB persistence provider [ADO]
ms.assetid: e75ef0dc-2016-4fcc-8918-23311c0d4e02
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9c47487547153426b2826792c5af0ad3bbeb7371
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-ole-db-persistence-provider-overview"></a>Vue d’ensemble du fournisseur de persistance Microsoft OLE DB
Le fournisseur de persistance Microsoft OLE DB vous permet d’enregistrer un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) de l’objet dans un fichier et les restaurer ultérieurement qui **Recordset** objet à partir du fichier. Informations de schéma, les données et les modifications en attente sont conservées.

 Vous pouvez enregistrer le **Recordset** dans le format de données Table Gram ADTG (Advanced) propriétaire ou le format Extensible Markup Language (XML) ouvert.

## <a name="provider-keyword"></a>Mot clé de fournisseur
 Pour appeler ce fournisseur, spécifiez le mot clé et la valeur suivante dans la chaîne de connexion.

```
"Provider=MSPersist"
```

## <a name="errors"></a>Erreurs
 Les erreurs suivantes émises par ce fournisseur peuvent être détectées dans votre application.

|Constante| Description|
|--------------|-----------------|
|E_BADSTREAM|Le fichier ouvert n’a pas un format valide (autrement dit, le format n’est pas ADTG ou XML).|
|E_CANTPERSISTROWSET|Le **Recordset** objet enregistré présente des caractéristiques qui l’empêchent d’être stocké.|

## <a name="remarks"></a>Notes
 Le fournisseur de persistance Microsoft OLE DB n’expose aucuns propriétés dynamiques.

 Actuellement, ne paramétrables que hiérarchique **Recordset** objets ne peut pas être enregistrés.

 Pour plus d’informations sur le stockage de manière permanente **Recordset** , consultez [persistance Recordset](../../../ado/guide/data/more-about-recordset-persistence.md).

 Lorsqu’un flux est utilisé pour ouvrir un **Recordset,** qu’il ne doit y avoir aucun paramètre spécifié autre que le *Source* paramètre de la **ouvrir** (méthode).

## <a name="see-also"></a>Voir aussi
[Fournisseur de persistance Microsoft OLE DB (fournisseur de services ADO)](../../../ado/guide/appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md)
