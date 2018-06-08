---
title: L’accès aux informations de Diagnostic dans le journal des événements étendus | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a79e9468-2257-4536-91f1-73b008c376c3
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 06da8446c04bf656d5ae4dfb13d15f8a173e5bbf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>Accès aux informations de diagnostic dans le journal des événements étendus
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Dans le [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)], le suivi ([fonctionnement du pilote suivi](../../connect/jdbc/tracing-driver-operation.md)) a été mis à jour pour faciliter plus facile à mettre en corrélation des événements clients avec les informations de diagnostic, tels que les échecs de connexion à partir de l’anneau de connectivité du serveur informations de performances mémoire tampon et d’applications dans le journal des événements étendus. Pour plus d’informations sur la lecture du journal des événements étendus, consultez [afficher les données de Session événements](http://msdn.microsoft.com/library/hh710068(SQL.110).aspx).  
  
## <a name="details"></a>Détails  
 Pour les opérations de connexion, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] enverra un client ID de connexion. Si la connexion échoue, vous pouvez accéder à la mémoire tampon en anneau de connectivité ([résolution des problèmes de connectivité dans SQL Server 2008 avec la mémoire tampon en anneau de connectivité](http://go.microsoft.com/fwlink/?LinkId=207752)) et recherchez le **ClientConnectionID** champ et obtenir des informations de diagnostic sur l’échec de connexion. Les ID de connexion du client sont enregistrés dans la mémoire tampon en anneau uniquement en cas d'erreur. (Si une connexion échoue avant d'envoyer le paquet de préconnexion, un ID de connexion client ne sera pas généré.) L'ID de connexion client est un GUID à 16 octets. Vous trouverez également la connexion client ID dans la sortie de la cible des événements étendus, si le **client_connection_id** action est ajoutée aux événements dans une session d’événements étendus. Vous pouvez activer le suivi et réexécutez la commande de connexion et observer les **ClientConnectionID** champ dans la trace, si vous avez besoin d’assistance diagnostic du pilote client supplémentaire.  
  
 Vous pouvez obtenir le client ID de connexion par programmation en utilisant [ISQLServerConnection Interface](../../connect/jdbc/reference/isqlserverconnection-interface.md). L'ID de connexion sera également présent dans toutes les exceptions liées à la connexion.  
  
 En cas d'erreur de connexion, l'ID de connexion client dans les informations de trace BID du serveur et dans la mémoire tampon de l'anneau de connectivité peuvent aider à corréler les connexions du client aux connexions sur le serveur. Pour plus d’informations sur l’enchère effectue le suivi sur le serveur, consultez [Data Access Tracing](http://go.microsoft.com/fwlink/?LinkId=125805). Notez que l’article de suivi d’accès aux données contient également des informations sur l’exécution d’une trace d’accès aux données, qui ne s’applique pas à la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]; consultez [fonctionnement du pilote suivi](../../connect/jdbc/tracing-driver-operation.md) pour plus d’informations sur la création d’une trace d’accès aux données avec le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].  
  
 Le pilote JDBC envoie également un ID d'activité spécifique au thread. L'ID d'activité est capturé dans les sessions d'événements étendus si les sessions sont démarrées alors que l'option TRACK_CAUSAILITY est activée. En cas de problèmes de performance avec une connexion active, vous pouvez obtenir l'ID d'activité de la trace du client (champ ActivityID), puis le localiser dans la sortie d'événements étendus. L'ID d'activité dans les événements étendus est un GUID à 16 octets (différent du GUID de l'ID de connexion du client) accompagné d'un numéro séquentiel de 4 octets. Le numéro séquentiel représente l'ordre d'une demande dans un thread. L'ActivityId est envoyé pour les instructions de lot SQL et les requêtes RPC. Pour activer l'envoi d'ActivityId au serveur, vous devez d'abord spécifier la paire de valeur/clé suivante dans le fichier Logging.Properties :  
  
```  
com.microsoft.sqlserver.jdbc.traceactivity = on  
```  
  
 Toute valeur autre que `on` (respectant la casse) désactive l’envoi de l’ID d’activité.  
  
 Pour plus d’informations, consultez [fonctionnement du pilote de traçage](../../connect/jdbc/tracing-driver-operation.md). L'indicateur de trace est utilisé avec les enregistreurs d'objets JDBC correspondants pour décider s'il faut tracer et envoyer l'ActivityId dans le pilote JDBC. En plus de la mise à jour du fichier Logging.Properties, l'enregistreur com.microsoft.sqlserver.jdbc doit être activé avec le niveau FINER ou plus élevé. Si vous souhaitez envoyer ActivityId au serveur pour des requêtes effectuées par une classe donnée, l'enregistreur de classe correspondant doit être activé avec le niveau FINER ou FINEST. Par exemple, si la classe est SQLServerStatement, activez l'enregistreur com.microsoft.sqlserver.jdbc.SQLServerStatement.  
  
 Voici un exemple qui utilise [!INCLUDE[tsql](../../includes/tsql_md.md)] pour démarrer une session d’événements étendus qui sera stockée dans un mémoire tampon en anneau et enregistrera l’ID d’activité envoyé par un client sur les opérations par lot et RPC :  
  
```  
create event session MySession on server  
add event connectivity_ring_buffer_recorded,  
add event sql_statement_starting (action (client_connection_id)),  
add event sql_statement_completed (action (client_connection_id)),  
add event rpc_starting (action (client_connection_id)),  
add event rpc_completed (action (client_connection_id))  
add target ring_buffer with (track_causality=on)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Diagnostic des problèmes avec le pilote JDBC](../../connect/jdbc/diagnosing-problems-with-the-jdbc-driver.md)  
  
  
