---
title: Fonctionnement conjoint des gestionnaires d’événements | Documents Microsoft
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
- events [ADO], about event handlers
- unpaired event handlers [ADO]
- paired event handlers [ADO]
- single event handlers [ADO]
- event handlers [ADO]
- multiple object event handlers [ADO]
ms.assetid: a86c8a02-dd69-420d-8a47-0188b339858d
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4ef9af3c4ba076048e0d04d31601b20e9d9ca321
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-event-handlers-work-together"></a>Fonctionnement conjoint des gestionnaires d’événements
Sauf si vous programmez en Visual Basic, tous les gestionnaires d’événements pour **connexion** et **Recordset** événements doivent être implémentés, que vous décidiez de traiter si tous les événements. La quantité de travail de mise en œuvre que vous avez à faire dépend de votre langage de programmation. Pour plus d’informations, consultez [instanciation des événements ADO par langage](../../../ado/guide/data/ado-event-instantiation-by-language.md).  
  
## <a name="paired-event-handlers"></a>Gestionnaires d’événements associés  
 Chaque gestionnaire d’événements Will est associé à un **Complete** Gestionnaire d’événements. Par exemple, lorsque votre application modifie la valeur d’un champ, la **WillChangeField** Gestionnaire d’événements est appelé. Si la modification est acceptable, l’application conserve la **ne** paramètre inchangée et l’opération est effectuée. Lorsque l’opération est terminée, un **FieldChangeComplete** événements informe l’application que l’opération est terminée. Si elle a réussi, **ne** contient **adStatusOK**; sinon, **ne** contient **contraire** et Vous devez vérifier le **erreur** objet afin de déterminer la cause de l’erreur.  
  
 Lorsque **WillChangeField** est appelée, vous pouvez déterminer que la modification ne doit pas être apportée. Dans ce cas, définissez **ne** à **adStatusCancel.** L’opération est annulée et la **FieldChangeComplete** événement reçoit un **ne** valeur **contraire**. Le **erreur** objet contient **adErrOperationCancelled** afin que votre **FieldChangeComplete** Gestionnaire sait que l’opération a été annulée. Toutefois, vous devez vérifier la valeur de la **ne** paramètre avant de le modifier, car la définition **ne** à **adStatusCancel** n’a aucun effet si le paramètre a été défini. pour **adStatusCantDeny** sur entrée dans la procédure.  
  
 Parfois, une opération peut déclencher plusieurs événements. Par exemple, le **Recordset** objet a associé pour les événements **champ** modifications et **enregistrement** modifications. Lorsque votre application modifie la valeur d’un **champ**, le **WillChangeField** Gestionnaire d’événements est appelé. S’il détermine que l’opération puisse continuer, la **WillChangeRecord** Gestionnaire d’événements est également déclenché. Si ce gestionnaire permet également l’événement à se poursuivre, la modification est apportée et la **FieldChangeComplete** et **RecordChangeComplete** gestionnaires d’événements sont appelés. L’ordre dans lequel les gestionnaires d’événements est une opération particulière sont appelées n’est pas défini, vous devez éviter d’écrire du code qui dépend de l’appel de gestionnaires dans une séquence particulière.  
  
 Dans les instances lorsque plusieurs événements Will sont déclenchés, l’un des événements peut annuler l’opération en attente. Par exemple, lorsque votre application modifie la valeur d’un **champ**, à la fois **WillChangeField** et **WillChangeRecord** devrait être appelés gestionnaires d’événements. Toutefois, si l’opération est annulée dans le premier gestionnaire d’événements, qui lui est associé **Complete** gestionnaire est appelé immédiatement avec **adStatusOperationCancelled**. Le second gestionnaire n’est jamais appelé. Si, toutefois, le premier gestionnaire d’événements autorise l’événement continuer, l’autre gestionnaire d’événements sera appelée. S’il annule ensuite l’opération, les deux **Complete** événements sont appelés comme dans les exemples précédents.  
  
## <a name="unpaired-event-handlers"></a>Gestionnaires d’événements non couplés  
 Tant que l’état passé à l’événement n’est pas **adStatusCantDeny**, vous pouvez désactiver les notifications d’événements pour tout événement en retournant **adStatusUnwantedEvent** dans les *état*paramètre. Par exemple, lorsque votre **Complete** Gestionnaire d’événements est appelé la première fois, vous pouvez retourner **adStatusUnwantedEvent**. Vous recevrez ensuite uniquement **sera** événements. Toutefois, certains événements peuvent être déclenchés pour plusieurs raisons. Dans ce cas, l’événement aura un *raison* paramètre. Lorsque vous retournez **adStatusUnwantedEvent**, vous recevrez plus de notifications pour cet événement s’ils se produisent pour une raison particulière. En d’autres termes, vous recevrez potentiellement notification pour chaque raison possible est que l’événement peut être déclenchée.  
  
 Seul **sera** gestionnaires d’événements peuvent être utiles lorsque vous souhaitez examiner les paramètres qui seront utilisées dans une opération. Vous pouvez modifier ces paramètres d’opération ou annuler l’opération.  
  
 Vous pouvez également laisser **Complete** notification d’événement activée. Lorsque le premier gestionnaire d’événements Will est appelé, retournez **adStatusUnwantedEvent**. Vous recevrez ensuite uniquement **Complete** événements.  
  
 Seul **Complete** gestionnaires d’événements peuvent être utiles pour la gestion des opérations asynchrones. Chaque opération asynchrone a appropriées **Complete** événement.  
  
 Par exemple, il peut prendre beaucoup de temps pour remplir une grande [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet. Si votre application est correctement écrit, vous pouvez démarrer une `Recordset.Open(...,adAsyncExecute)` opération et poursuivre le traitement des autres. Vous serez éventuellement recevoir une notification lorsque le **Recordset** est remplie par une **ExecuteComplete** événement.  
  
## <a name="single-event-handlers-and-multiple-objects"></a>Gestionnaires d’événements unique et plusieurs objets  
 La souplesse d’un langage de programmation comme Microsoft Visual C++® vous permet de disposer d’un gestionnaire d’événements processus événements à partir de plusieurs objets. Par exemple, vous pourriez avoir un **déconnexion** événement Gestionnaire traiter les événements de plusieurs **connexion** objets. Si l’une des connexions s’est terminée, le **déconnexion** Gestionnaire d’événements est appelé. Vous pouvez déterminer quelle connexion a provoqué l’événement, car le paramètre de l’objet gestionnaire d’événements est défini correspondant **connexion** objet.  
  
> [!NOTE]
>  Cette technique ne peut pas être utilisée en Visual Basic, car ce langage ne peut associer qu’un seul objet à un gestionnaire d’événements.  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du Gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)   
 [Instanciation des événements ADO par langage](../../../ado/guide/data/ado-event-instantiation-by-language.md)   
 [Paramètres d’événement](../../../ado/guide/data/event-parameters.md)   
 [Types d’événements](../../../ado/guide/data/types-of-events.md)
