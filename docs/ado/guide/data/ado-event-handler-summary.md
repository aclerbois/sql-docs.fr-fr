---
title: Résumé du Gestionnaire d’événements ADO | Documents Microsoft
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
- event handlers [ADO]
ms.assetid: b34f4472-5e04-4a2c-ab64-38d6eca31a69
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e3e52fe70e497ab8c5b715861e16f3f67266f9f8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-connection-and-recordset-events"></a>Connexion ADO et les événements de jeu d’enregistrements
Deux objets ADO peuvent déclencher des événements : le [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet et la [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet. Le **ConnectionEvent** famille relative aux opérations sur le **connexion** objet et le **RecordsetEvent** famille relative aux opérations sur le  **Jeu d’enregistrements** objet.

-   **Événements de connexion**: événements émis quand une transaction sur une connexion commence, est validée ou est restaurée nouveau ; lorsque un [commande](../../../ado/reference/ado-api/command-object-ado.md) exécute ; lorsqu’un avertissement se produit pendant une **l’événement de connexion**opération ; ou quand un **connexion** commence ou se termine.

-   **Événements Recordset**: événements sont émis dans les opérations d’extraction asynchrone, ainsi que lorsque vous parcourez les lignes d’un **Recordset** d’objet, de modifier un champ dans une ligne d’un **Recordset**, modifier une ligne dans un **Recordset**, ouvrez un **Recordset** avec un curseur côté serveur, vous devez fermer un **Recordset**, ou assurez-vous que toute modification que ce soit dans le  **Jeu d’enregistrements**.

 Les tableaux suivants répertorient les événements et leurs descriptions.

|ConnectionEvent| Description|
|---------------------|-----------------|
|[BeginTransComplete, CommitTransComplete, RollbackTransComplete](../../../ado/reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|**Gestion des transactions** : indique que la transaction actuelle sur la connexion a démarré, validée ou restaurée.|
|[WillConnect](../../../ado/reference/ado-api/willconnect-event-ado.md), [ConnectComplete, Disconnect](../../../ado/reference/ado-api/connectcomplete-and-disconnect-events-ado.md)|**Gestion des connexions** : Notification qui démarre la connexion actuelle, a démarré ou est terminée.|
|[WillExecute](../../../ado/reference/ado-api/willexecute-event-ado.md), [ExecuteComplete](../../../ado/reference/ado-api/executecomplete-event-ado.md)|**Gestion de l’exécution de la commande** : notification signalant que l’exécution de la commande en cours sur la connexion démarre ou s’est terminée.|
|[InfoMessage](../../../ado/reference/ado-api/infomessage-event-ado.md)|**Information** : Notification qu’il existe des informations supplémentaires sur l’opération en cours.|

|RecordsetEvent| Description|
|--------------------|-----------------|
|[FetchProgress](../../../ado/reference/ado-api/fetchprogress-event-ado.md), [FetchComplete](../../../ado/reference/ado-api/fetchcomplete-event-ado.md)|**État d’extraction** : Notification de la progression d’une opération de récupération de données, ou que l’opération de récupération est terminée. Ces événements sont disponibles uniquement si la **Recordset** a été ouvert à l’aide d’un curseur côté client.|
|[WillChangeField, FieldChangeComplete](../../../ado/reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|**Gestion des modifications de champ** : indique que la valeur du champ actuel va changer ou a changé.|
|[WillMove, MoveComplete](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md), [EndOfRecordset](../../../ado/reference/ado-api/endofrecordset-event-ado.md)|**Gestion de la navigation** : Notification de la position de la ligne actuelle dans un **Recordset** change, a été modifié ou a atteint la fin de la **Recordset**.|
|[WillChangeRecord, RecordChangeComplete](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|**Gestion des modifications de ligne** : Notification qu’un élément de la ligne actuelle de la **Recordset** va changer ou a changé.|
|[WillChangeRecordset, RecordsetChangeComplete](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|**Gestion des modifications de jeu d’enregistrements** : Notification que quelque chose en cours **Recordset** va changer ou a changé.|

## <a name="see-also"></a>Voir aussi
 [Instanciation des événements ADO par langage](../../../ado/guide/data/ado-event-instantiation-by-language.md) [événements ADO](../../../ado/reference/ado-api/ado-events.md) [paramètres d’événement](../../../ado/guide/data/event-parameters.md) [fonctionnement conjoint des gestionnaires d’événements](../../../ado/guide/data/how-event-handlers-work-together.md) [Types d’événements](../../../ado/guide/data/types-of-events.md)
