---
title: Événements ADO | Documents Microsoft
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
- events [ADO]
- ADO, events
ms.assetid: 0ded5ad9-8f83-4224-95af-38512783b972
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1573eaca943c1bb018e279f5360f72480610a5d1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-events"></a>Événements ADO
|||  
|-|-|  
|[BeginTransComplete](../../../ado/reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Appelée après le **BeginTrans** opération.|  
|[CommitTransComplete](../../../ado/reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Appelée après le **CommitTrans** opération.|  
|[ConnectComplete](../../../ado/reference/ado-api/connectcomplete-and-disconnect-events-ado.md)|Appelé après une connexion.|  
|[Déconnecter](../../../ado/reference/ado-api/connectcomplete-and-disconnect-events-ado.md)|Appelé après la fin d’une connexion.|  
|[EndOfRecordset](../../../ado/reference/ado-api/endofrecordset-event-ado.md)|Appelé lorsqu’une tentative de déplacement vers une ligne après la fin de la **Recordset**.|  
|[ExecuteComplete](../../../ado/reference/ado-api/executecomplete-event-ado.md)|Appelé après que l’exécution d’une commande est terminée.|  
|[FetchComplete](../../../ado/reference/ado-api/fetchcomplete-event-ado.md)|Appelé après que tous les enregistrements dans une opération asynchrone de longue durée ont été récupérés dans le **Recordset**.|  
|[FetchProgress](../../../ado/reference/ado-api/fetchprogress-event-ado.md)|Appelé régulièrement pendant une longue opération asynchrone pour signaler le nombre de lignes actuellement ont été récupéré dans le **Recordset**.|  
|[FieldChangeComplete](../../../ado/reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|Appelé après que la valeur d’un ou plusieurs **champ** objets a changé.|  
|[InfoMessage](../../../ado/reference/ado-api/infomessage-event-ado.md)|Appelé chaque fois qu’un avertissement se produit pendant une **ConnectionEvent** opération.|  
|[MoveComplete](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)|Appelé après la position actuelle dans le **Recordset** modifications.|  
|[RecordChangeComplete](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|Appelé après la modification d’un ou plusieurs enregistrements.|  
|[RecordsetChangeComplete](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Appelée après le **Recordset** a changé.|  
|[RollbackTransComplete](../../../ado/reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Appelée après le **RollbackTrans** opération.|  
|[WillChangeField](../../../ado/reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|Appelé avant qu’une opération en attente modifie la valeur d’un ou plusieurs **champ** des objets dans le **Recordset**.|  
|[WillChangeRecord](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|Appelé avant qu’un ou plusieurs enregistrements (lignes) dans le **Recordset** modifier.|  
|[WillChangeRecordset](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Appelé avant qu’une opération en attente modifie la **Recordset**.|  
|[WillConnect](../../../ado/reference/ado-api/willconnect-event-ado.md)|Appelé avant le début d’une connexion.|  
|[WillExecute](../../../ado/reference/ado-api/willexecute-event-ado.md)|Appelé juste avant une commande en attente s’exécute sur cette connexion et permet de l’utilisateur pour examiner et modifier les paramètres d’exécution en attente.|  
|[WillMove](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)|Le **WillMove** événement est appelé *avant* une opération en attente modifie la position actuelle dans le **Recordset**.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des API ADO](../../../ado/reference/ado-api/ado-api-reference.md)   
 [Collections ADO](../../../ado/reference/ado-api/ado-collections.md)   
 [Propriétés dynamiques ADO](../../../ado/reference/ado-api/ado-dynamic-properties.md)   
 [Constantes énumérées ADO](../../../ado/reference/ado-api/ado-enumerated-constants.md)   
 [Annexe b : erreurs ADO](../../../ado/guide/appendixes/appendix-b-ado-errors.md)   
 [Gestion des événements ADO](../../../ado/guide/data/handling-ado-events.md)   
 [Méthodes ADO](../../../ado/reference/ado-api/ado-methods.md)   
 [Modèle objet ADO](../../../ado/reference/ado-api/ado-object-model.md)   
 [Objets et interfaces ADO](../../../ado/reference/ado-api/ado-objects-and-interfaces.md)   
 [Propriétés ADO](../../../ado/reference/ado-api/ado-properties.md)
