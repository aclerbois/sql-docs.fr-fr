---
title: 'Instanciation des événements ADO : ADO et WFC | Documents Microsoft'
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 9ee4be21-657b-407a-afa4-0b27a6b096ce
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 89fefc8d733f515ef6f085e39b69268a808cefd4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-event-instantiation-ado-and-wfc"></a>Instanciation des événements ADO : ADO et WFC
ADO pour Windows Foundation Classes (ADO/WFC) repose sur le modèle d’événement ADO et présente une interface de programmation d’application simplifiée. En général, ADO/WFC intercepte les événements ADO, consolide les paramètres d’événement dans une classe d’événement unique, puis appelle le Gestionnaire d’événements.  
  
### <a name="to-use-ado-events-in-adowfc"></a>Pour utiliser les événements ADO dans ADO/WFC  
  
1.  Définir votre propre gestionnaire d’événements pour traiter un événement. Par exemple, si vous voulez traiter les **ConnectComplete** événement dans le **ConnectionEvent** famille, vous pouvez utiliser ce code :  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    {  
        System.out.println("onConnectComplete:" + e);  
    }  
    ```  
  
2.  Définissez un objet gestionnaire pour représenter votre gestionnaire d’événements. L’objet gestionnaire doit être de type de données **ConnectEventHandler** pour un événement de type **ConnectionEvent**, ou le type de données **RecordsetEventHandler** pour un événement de type  **RecordsetEvent**. Par exemple, le code suivant pour votre **ConnectComplete** Gestionnaire d’événements :  
  
    ```  
    ConnectionEventHandler handler =   
        new ConnectionEventHandler(this, "onConnectComplete");  
    ```  
  
     Le premier argument de la **ConnectionEventHandler** constructeur est une référence à la classe qui contient le Gestionnaire d’événements nommé dans le deuxième argument.  
  
3.  Ajouter votre gestionnaire d’événements à une liste de gestionnaires chargés de traiter un type particulier d’événement. Utilisez la méthode avec un nom tel que **addOn *** EventName*(*gestionnaire*).  
  
4.  ADO/WFC implémente en interne tous les gestionnaires d’événements ADO. Par conséquent, un événement déclenché par un **connexion** ou **Recordset** opération est interceptée par un gestionnaire d’événements ADO/WFC.  
  
     Le Gestionnaire d’événements ADO/WFC transmet ADO **ConnectionEvent** paramètres dans une instance de la ADO/WFC **ConnectionEvent** classe ou ADO **RecordsetEvent** paramètres dans un instance de la ADO/WFC **RecordsetEvent** classe. Ces classes ADO/WFC consolident les paramètres d’événement ADO ; Autrement dit, chaque classe ADO/WFC contient un membre de données pour chaque paramètre de toutes les ADO **ConnectionEvent** ou **RecordsetEvent** méthodes.  
  
5.  ADO/WFC appelle ensuite le Gestionnaire d’événements avec l’objet d’événement ADO/WFC. Par exemple, votre **onConnectComplete** gestionnaire a une signature comme suit :  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    ```  
  
     Le premier argument est le type d’objet qui a envoyé l’événement ([connexion](../../../ado/reference/ado-api/connection-object-ado.md) ou [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)), et le deuxième argument est l’objet d’événement ADO/WFC (**ConnectionEvent** ou **RecordsetEvent**).  
  
     La signature de votre gestionnaire d’événements est plus simple que d’un événement ADO. Toutefois, vous devez toujours comprendre le modèle d’événement ADO pour savoir quels paramètres s’appliquent à un événement et comment y répondre.  
  
6.  Retourner à partir de votre gestionnaire d’événements au gestionnaire ADO/WFC pour l’événement ADO. ADO/WFC copie pertinentes membres de données d’événement ADO/WFC dans les paramètres d’événement ADO et renvoie le Gestionnaire d’événements ADO.  
  
7.  Lorsque vous avez terminé le traitement, supprimez votre gestionnaire dans la liste des gestionnaires d’événements ADO/WFC. Utilisez la méthode avec un nom tel que **removeOn *** EventName*(*gestionnaire*).  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du Gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)   
 [ADO - Index de la syntaxe WFC](../../../ado/reference/ado-api/ado-wfc-syntax-index.md)   
 [Paramètres d’événement](../../../ado/guide/data/event-parameters.md)   
 [Fonctionnement conjoint des gestionnaires d’événements](../../../ado/guide/data/how-event-handlers-work-together.md)   
 [Types d’événements](../../../ado/guide/data/types-of-events.md)
