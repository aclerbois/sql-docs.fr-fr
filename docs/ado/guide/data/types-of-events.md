---
title: Types d’événements | Documents Microsoft
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
- EventComplete event [ADO]
- events [ADO], types
- Will events [ADO]
- complete events [ADO]
- WillEvent event [ADO]
ms.assetid: f3327ea0-635a-43d4-bd78-c1674f62f1a2
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3d093fbea3d4c4c6410f19b842ba8907aaa2229e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="types-of-events"></a>Types d’événements
Il existe deux types d’événements. « Événements will », appelés avant le début d’une opération, comportent généralement « Est » dans leur nom, par exemple, **WillChangeRecordset** ou **WillConnect**. Les événements qui sont appelées après un événement a été effectué généralement incluent « Complète » dans leur nom, par exemple, **RecordChangeComplete** ou **ConnectComplete**. Existe des exceptions, telles que **InfoMessage** , mais ces modifications se produisent après l’opération associée est terminée.  
  
## <a name="will-events"></a>Événements Will  
 Gestionnaires d’événements appelés avant le démarrage de l’opération vous offre la possibilité d’examiner ou modifier les paramètres d’opération, puis annuler l’opération ou autoriser qu’elle se termine. Ces routines de gestionnaires d’événements ont généralement des noms au format **sera*événement ***.  
  
## <a name="complete-events"></a>Événements de fin  
 Gestionnaires d’événements appelés au terme d’une opération peuvent avertir votre application qu’une opération est terminée. Ce type de gestionnaire d’événements est également informé lorsqu’un gestionnaire d’événements Will annule une opération en attente. Ces routines de gestionnaires d’événements ont généralement des noms au format ***événement * terminé**.  
  
 Événements Will et Complete sont généralement utilisées par paires.  
  
## <a name="other-events"></a>Autres événements  
 Les autres gestionnaires d’événements, autrement dit, les événements dont les noms ne sont pas sous la forme **sera * événement*** ou ***événement * terminé** — sont appelées uniquement après une opération se termine. Ces événements sont **déconnexion**, **EndOfRecordset**, et **InfoMessage**.  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du Gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)   
 [Instanciation des événements ADO par langage](../../../ado/guide/data/ado-event-instantiation-by-language.md)   
 [Paramètres d’événement](../../../ado/guide/data/event-parameters.md)   
 [Fonctionnement conjoint des gestionnaires d’événements](../../../ado/guide/data/how-event-handlers-work-together.md)
