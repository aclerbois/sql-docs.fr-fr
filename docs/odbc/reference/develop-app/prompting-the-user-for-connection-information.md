---
title: Demander à l’utilisateur des informations de connexion | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- connecting to data source [ODBC], SqlConnect
- connecting to driver [ODBC], prompting user for information
- connecting to driver [ODBC], SQLConnect
- connecting to driver [ODBC], SQLDriverConnect
- connecting to data source [ODBC], SqlDriverConnect
- SQLConnect function [ODBC], prompting user for connection information
- connecting to data source [ODBC], prompting user for information
- prompting user for connection information [ODBC]
- SQLDriverConnect function [ODBC], prompting user for connection information
ms.assetid: da98e9b9-a4ac-4a9d-bae6-e9252b1fe1e5
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 305d8bf4c3e18fdb610c6b67b7c678c43e83c6f0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prompting-the-user-for-connection-information"></a>Demander à l’utilisateur des informations de connexion
Si l’application utilise **SQLConnect** et doit demander à l’utilisateur pour toutes les informations de connexion, telles qu’un nom d’utilisateur et un mot de passe, il doit le faire lui-même. Cela permet l’application de contrôler son « apparence », il peut forcer l’application pour contenir le code spécifique au pilote. Cela se produit lorsque l’application doit demander à l’utilisateur pour les informations de connexion spécifiques au pilote. Cela présente une situation impossible pour les applications génériques qui sont conçues pour fonctionner avec tous les pilotes, y compris les pilotes qui n’existent pas lorsque l’application est écrite.  
  
 **SQLDriverConnect** peut inviter l’utilisateur pour les informations de connexion. Par exemple, le programme personnalisé mentionné précédemment Impossible passer la chaîne de connexion à **SQLDriverConnect**:  
  
```  
DSN=XYZ Corp;  
```  
  
 Le pilote peut ensuite afficher une boîte de dialogue qui vous invite à entrer pour l’ID utilisateur et mots de passe, comme dans l’illustration suivante.  
  
 ![Boîte de dialogue qui vous invite à entrer l’ID d’utilisateur et mots de passe](../../../odbc/reference/develop-app/media/pr18.gif "pr18")  
  
 Que le pilote peut demander des informations de connexion est particulièrement utile pour les applications génériques et verticales. Ces applications ne doivent pas contenir d’informations spécifiques au pilote, et avoir l’invite de pilote pour obtenir les informations dont il a besoin conserve ces informations hors de l’application. Cela est illustré par les deux exemples précédents. Lorsque l’application a transmis uniquement le nom de source de données pour le pilote, l’application ne contenait pas toutes les informations spécifiques au pilote et a par conséquent pas directement liée à un pilote spécifique. Lorsque l’application a transmis une chaîne de connexion complète pour le pilote, il a été lié au pilote qui peut interpréter cette chaîne.  
  
 Une application générique peut prendre une étape supplémentaire et sans spécifier une source de données. Lorsque **SQLDriverConnect** reçoit une chaîne de connexion vide, le Gestionnaire de pilotes affiche la boîte de dialogue suivante.  
  
 ![Boîte de dialogue Source de données Sélectionnez](../../../odbc/reference/develop-app/media/ch06a.gif "CH06A")  
  
 Une fois que l’utilisateur sélectionne une source de données, le Gestionnaire de pilotes construit une chaîne de connexion en spécifiant cette source de données et les transmet au pilote. Le pilote peut inviter l’utilisateur pour des informations supplémentaires dont il a besoin.  
  
 Les conditions dans lesquelles le pilote invite l’utilisateur sont contrôlées par le *DriverCompletion* indicateur ; il existe des options toujours demander, invite si nécessaire ou pour ne jamais demander. Pour obtenir une description complète de cet indicateur, consultez la [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) description de fonction.
