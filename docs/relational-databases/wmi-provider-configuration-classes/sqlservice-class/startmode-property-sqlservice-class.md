---
title: Propriété StartMode (classe SqlService) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: wmi
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- StartMode Property (SqlService Class)
apilocation:
- sqlmgmproviderxpsp2up.mof
apitype: MOFDef
helpviewer_keywords:
- StartMode property
ms.assetid: c0c2c7f8-d4ae-44f2-ad8e-aecfcb7c2878
caps.latest.revision: 36
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: d5f9ba82b7ef75fa9b2a6fff80dab8e0180d59f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="startmode-property-sqlservice-class"></a>Propriété StartMode (classe SqlService)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Obtient le mode de démarrage du service.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.StartMode [= value]  
```  
  
## <a name="parts"></a>Éléments  
 *objet*  
 Objet de [classe SqlService](../../../relational-databases/wmi-provider-configuration-classes/sqlservice-class/sqlservice-class.md) qui représente le service.  
  
## <a name="property-valuereturn-value"></a>Valeur de propriété/valeur de retour  
 Valeur uint32 qui spécifie le mode du service.  
  
 Il peut s'agir de l'une des valeurs suivantes.  
  
 Boot  
 Valeur = 0. Le service est démarré par le chargeur du système d'exploitation. Cette option est uniquement valide pour les services de pilote.  
  
 Système  
 Valeur = 1. Service démarré par le **IoInitSystem** (méthode). Cette option est uniquement valide pour les services de pilote.  
  
 Automatique  
 Valeur = 2 Le service doit être démarré automatiquement par le Gestionnaire de contrôle des services lors du démarrage du système.  
  
 Manuel  
 Valeur = 3. Le service doit être démarré par Computer Manager lorsqu’un processus appelle la **StartService** (méthode).  
  
 Désactivé  
 Valeur = 4 Le service ne peut pas être démarré.  
  
## <a name="remarks"></a>Notes  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrage et arrêt des Services](http://technet.microsoft.com/library/ms174886\(v=sql.105\).aspx)  
  
  
