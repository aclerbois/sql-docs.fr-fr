---
title: Notification de fin de la fonction asynchrone | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 336565da-4203-4745-bce2-4f011c08e357
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b86f906385341b5b67a51cc60ef702f61dff13ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="notification-of-asynchronous-function-completion"></a>Notification de fin de la fonction asynchrone
Dans le SDK Windows 8, ODBC ajouté un mécanisme pour informer les applications lors de l’opération asynchrone terminée, nous appelons « notification d’achèvement ». (Consultez [exécution asynchrone (méthode de Notification)](../../../odbc/reference/develop-app/asynchronous-execution-notification-method.md) pour plus d’informations.) Cette rubrique décrit certains des problèmes pour les développeurs de pilote.  
  
## <a name="the-interface-between-the-driver-manager-and-driver"></a>L’Interface entre le Gestionnaire de pilotes et le pilote  
 Le Gestionnaire de pilotes fournit en interne une fonction de rappel [SQLAsyncNotificationCallback fonction](../../../odbc/reference/develop-driver/sqlasyncnotificationcallback-function.md). **SQLAsyncNotificationCallback** peut être appelé uniquement par le pilote--une application ne peut pas l’appeler directement. Le pilote appelle **SQLAsyncNotificationCallback** chaque fois que de nouvelles données reçues à partir du serveur après la dernière SQL_STILL_EXECUTING le retour.  
  
 Le Gestionnaire de pilotes fournit un mécanisme de rappel pour un pilote peut informer le Gestionnaire de pilotes lorsque des progrès a été effectuée lors de l’exécution d’une opération asynchrone après que la fonction correspondante retourne SQL_STILL_EXECUTING  
  
 Les jeux de gestionnaire de pilotes que l’attribut SQL_ATTR_ASYNC_DBC_NOTIFICATION_CALLBACK sur une connexion au pilote gérer avec un pointeur de fonction non-NULL, ce qui est de type SQL_ASYNC_NOTIFICATION_CALLBACK, pour le pilote en mode de notification pour toutes les opérations asynchrones sur ce handle. De même, la définit de gestionnaire de pilotes l’attribut SQL_ATTR_ASYNC_STMT_NOTIFICATION_CALLBACK sur une instruction gérer avec un pointeur de fonction non-NULL, ce qui est également de type SQL_ASYNC_NOTIFICATION_CALLBACK, pour le pilote en mode de notification pour toutes les opérations asynchrones sur ce handle.  
  
 Si une opération asynchrone est effectuée sur un descripteur de pilote, les fonctions de pilote asynchrone doivent fonctionner dans un style non bloquant. Si l’opération ne peut pas se termine immédiatement, la fonction de pilote doit retourner SQL_STILL_EXECUTING. Cela est vrai pour le mode d’interrogation et le mode de notification.  
  
 Si un handle est en mode asynchrone de notification, le pilote doit appeler la fonction de rappel de notification, dont l’adresse est la valeur de l’attribut SQL_ATTR_ASYNC_DBC_NOTIFICATION_CALLBACK ou SQL_ATTR_ASYNC_STMT_NOTIFICATION_CALLBACK, une seule fois après le retour de SQL_STILL_EXECUTING. En d’autres termes, un SQL_STILL_EXECUTING de retour doivent être associés à un appel de la fonction de rappel de notification. Le pilote doit utiliser la valeur actuelle de l’attribut de handle SQL_ATTR_ASYNC_DBC_NOTIFICATION_CONTEXT ou SQL_ATTR_ASYNC_STMT_NOTIFICATION_CONTEXT comme valeur pour le paramètre d’appel de fonction de rappel *pContext*.  
  
 Le pilote ne doit pas appeler de nouveau dans le thread qui appelle la fonction de pilote ; Il n’existe aucune raison de notification de progression avant le retour de la fonction. Le pilote doit utiliser son propre thread au rappel. Le Gestionnaire de pilotes n’utilise pas de thread de rappel du pilote pour exécuter la logique de traitement complet.  
  
 Le Gestionnaire de pilotes appelle la fonction d’origine à nouveau une fois que le pilote rappelle. Le Gestionnaire de pilotes peut utiliser un thread qui n’est ni un thread d’application, ni un thread de pilote. Si le pilote utilise des informations associées au thread (par exemple, identificateur utilisateur ou le jeton de sécurité), le pilote doit enregistrer les informations requises dans l’appel asynchrone initial et utiliser la valeur enregistrée avant la fin de l’opération asynchrone entière. En règle générale, uniquement **SQLDriverConnect**, **SQLConnect**, ou **SQLBrowseConnect** devez utiliser ce genre d’informations.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)
