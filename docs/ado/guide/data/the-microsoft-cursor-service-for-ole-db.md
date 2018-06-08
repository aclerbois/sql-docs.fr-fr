---
title: Le Service de curseur Microsoft pour OLE DB | Documents Microsoft
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
- cursor service for ole db [ADO]
- cursors [ADO], cursor service for OLE DB
ms.assetid: 1ac3bd9b-2d45-4cc8-88ec-bd8a218cfb49
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e01e5d06c798d0418f72ce02004cfeb83baedd05
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-microsoft-cursor-service-for-ole-db"></a>Le Service de curseur Microsoft pour OLE DB
Lorsque vous sélectionnez un curseur côté client ou le **CursorLocation** propriété **adUseClient**, vous appelez le Service de curseur Microsoft pour OLE DB. Vous pouvez également voir des références à « Moteur du curseur Client », qui est essentiellement la même chose dans le contexte de l’objet ADO. Ce service complète les fonctions de curseur-prise en charge des fournisseurs de données. Par conséquent, vous pouvez percevoir relativement uniforme des fonctionnalités à partir de tous les fournisseurs de données.  
  
 Le Service de curseur pour OLE DB rend disponibles les propriétés dynamiques et améliore le comportement de certaines méthodes. Par exemple, le **optimiser** propriété dynamique permet la création d’index temporaires qui facilitent certaines opérations, telles que la **trouver** (méthode).  
  
 Le Service de curseur permet la prise en charge de la mise à jour par lot dans tous les cas. Il simule également des types de curseurs plus performante, tels que les curseurs dynamiques, lorsqu’un fournisseur de données peut fournir uniquement les curseurs d’une capacité inférieure, tels que les curseurs statiques.  
  
## <a name="see-also"></a>Voir aussi  
 [Service de curseur Microsoft pour OLE DB (composant du Service ADO)](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)
