---
title: Mode, propriété (ADO) | Documents Microsoft
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
- Connection15::Mode
- _Stream::Mode
- _Record::Mode
helpviewer_keywords:
- Mode property [ADO]
ms.assetid: 808661eb-0d7c-4e6d-8e40-9dc3bef3d77a
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e1aa3910a07fd1e24aeab7429234c1d7211eb6f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mode-property-ado"></a>Mode, propriété (ADO)
Indique les autorisations disponibles pour la modification des données dans un [connexion](../../../ado/reference/ado-api/connection-object-ado.md), [enregistrement](../../../ado/reference/ado-api/record-object-ado.md), ou [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un [ConnectModeEnum](../../../ado/reference/ado-api/connectmodeenum.md) valeur. La valeur par défaut pour un **connexion** est **adModeUnknown**. La valeur par défaut pour un **enregistrement** objet est **adModeRead**. La valeur par défaut pour un **flux** associé à une source sous-jacente (ouverte avec une URL comme source, ou en tant que la valeur par défaut **flux** d’un **enregistrement**) est  **adModeRead**. La valeur par défaut pour un **flux** non associé à un sous-jacent source (instanciée en mémoire) est **adModeUnknown**.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **Mode** propriété pour définir ou retourner les autorisations d’accès en cours d’utilisation par le fournisseur sur la connexion actuelle. Vous pouvez définir le **Mode** propriété uniquement lorsque la **connexion** objet est fermé.  
  
 Pour un **flux** de l’objet, si le mode d’accès n’est pas spécifié, il est hérité de la source utilisée pour ouvrir le **flux** objet. Par exemple, si un **flux** est ouvert à partir d’un **enregistrement** objet, par défaut, il est ouvert dans le même mode que le **enregistrement**.  
  
 Cette propriété est en lecture/écriture lorsque l’objet est fermé et en lecture seule alors que l’objet est ouvert.  
  
> [!NOTE]
>  **Utilisation du Service de données à distance** lorsqu’il est utilisé sur un côté client **connexion** objet, le **Mode** propriété peut uniquement être définie sur **adModeUnknown**.  
  
## <a name="applies-to"></a>S'applique à  
  
||||  
|-|-|-|  
|[Connection, objet (ADO MD)](../../../ado/reference/ado-api/connection-object-ado.md)|[Record, objet (ADO)](../../../ado/reference/ado-api/record-object-ado.md)|[Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [IsolationLevel et Mode, propriétés-exemple (VB)](../../../ado/reference/ado-api/isolationlevel-and-mode-properties-example-vb.md)   
 [IsolationLevel et Mode, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/isolationlevel-and-mode-properties-example-vc.md)   
