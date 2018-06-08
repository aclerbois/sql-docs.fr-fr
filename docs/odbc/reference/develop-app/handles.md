---
title: Gère les | Documents Microsoft
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
- handles [ODBC]
- driver handles [ODBC]
- driver manager [ODBC], handles
- handles [ODBC], about handles
ms.assetid: f663101e-a4cc-402b-b9d7-84d5e975be71
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 830c4b653af74097c59c9aff9e073267792a84b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="handles"></a>Poignées
Poignées sont opaques, 32 bits des valeurs qui identifient un élément particulier ; dans ODBC, cet élément peut être un environnement, une connexion, une instruction ou une descripteur. Lorsque l’application appelle **SQLAllocHandle**, le Gestionnaire de pilote ou le pilote crée un nouvel élément du type spécifié et retourne son handle à l’application. L’application plus tard utilise le handle pour identifier cet élément lors de l’appel de fonctions ODBC. Le Gestionnaire de pilotes et le pilote utilisent le handle pour rechercher des informations sur l’élément.  
  
 Par exemple, le code suivant utilise deux pointeurs d’instruction (*hstmtOrder* et *hstmtLine*) pour identifier les instructions sur lesquelles créer des jeux de résultats des ventes de commandes des commandes client et les numéros de ligne. Il utilise ensuite ces poignées pour identifier le jeu de résultats pour extraire des données à partir de.  
  
```  
SQLHSTMT      hstmtOrder, hstmtLine; // Statement handles.  
SQLUINTEGER   OrderID;  
SQLINTEGER    OrderIDInd = 0;  
SQLRETURN     rc;  
  
// Prepare the statement that retrieves line number information.  
SQLPrepare(hstmtLine, "SELECT * FROM Lines WHERE OrderID = ?", SQL_NTS);  
  
// Bind OrderID to the parameter in the preceding statement.  
SQLBindParameter(hstmtLine, 1, SQL_PARAM_INPUT, SQL_C_ULONG, SQL_INTEGER, 5, 0,  
               &OrderID, 0, &OrderIDInd);  
  
// Bind the result sets for the Order table and the Lines table. Bind  
// OrderID to the OrderID column in the Orders table. When each row is  
// fetched, OrderID will contain the current order ID, which will then be  
// passed as a parameter to the statement tofetch line number  
// information. Code not shown.  
  
// Create a result set of sales orders.  
SQLExecDirect(hstmtOrder, "SELECT * FROM Orders", SQL_NTS);  
  
// Fetch and display the sales order data. Code to check if rc equals  
// SQL_ERROR or SQL_SUCCESS_WITH_INFO not shown.  
while ((rc = SQLFetch(hstmtOrder)) != SQL_NO_DATA) {  
   // Display the sales order data. Code not shown.  
  
   // Create a result set of line numbers for the current sales order.  
   SQLExecute(hstmtLine);  
  
   // Fetch and display the sales order line number data. Code to check  
   // if rc equals SQL_ERROR or SQL_SUCCESS_WITH_INFO not shown.  
   while ((rc = SQLFetch(hstmtLine)) != SQL_NO_DATA) {  
      // Display the sales order line number data. Code not shown.  
   }  
  
   // Close the sales order line number result set.  
   SQLCloseCursor(hstmtLine);  
}  
  
// Close the sales order result set.  
SQLCloseCursor(hstmtOrder);  
```  
  
 Poignées sont uniquement explicites pour le composant ODBC qui les a créés ; Autrement dit, seulement le Gestionnaire de pilotes peut interpréter les poignées du Gestionnaire de pilotes et uniquement un pilote peut interpréter de ses propres poignées.  
  
 Par exemple, supposons que le pilote dans l’exemple précédent alloue une structure pour stocker des informations sur une instruction et retourne le pointeur vers cette structure en tant que le handle d’instruction. Lorsque l’application appelle **SQLPrepare**, il transmet une instruction SQL et la gestion de l’instruction utilisée pour les numéros de ligne de commande client. Le pilote envoie l’instruction SQL à la source de données, qui prépare et renvoie un identificateur de plan d’accès. Le pilote utilise le handle pour rechercher la structure dans lequel stocker cet identificateur.  
  
 Ensuite, lorsque l’application appelle **SQLExecute** pour générer le jeu de résultats des numéros de ligne pour une commande spécifique, il transmet le même handle. Le pilote utilise le handle pour récupérer l’identificateur de plan d’accès de la structure. Il envoie l’identificateur de la source de données pour lui indiquer le plan d’exécution.  
  
 ODBC comporte deux niveaux de handles : handles du Gestionnaire de pilotes et pilote. L’application utilise des descripteurs de gestionnaire de pilotes lors de l’appel de fonctions ODBC parce qu’elle appelle ces fonctions dans le Gestionnaire de pilotes. Le Gestionnaire de pilotes utilise ce handle pour rechercher le descripteur du pilote correspondant et le descripteur du pilote lors de l’appel de la fonction dans le pilote. Pour obtenir un exemple d’utilisation de pilote et les handles de gestionnaire de pilotes, consultez [rôle du Gestionnaire de pilotes dans le processus de connexion](../../../odbc/reference/develop-app/driver-manager-s-role-in-the-connection-process.md).  
  
 Qu’il n’y a deux niveaux de handles est un artefact de l’architecture ODBC ; dans la plupart des cas, il ne s’applique pas à l’application ou le pilote. Bien qu’il n’existe généralement aucune raison de cela, il est possible de l’application déterminer les poignées de pilote en appelant **SQLGetInfo**.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Handles d’environnement](../../../odbc/reference/develop-app/environment-handles.md)  
  
-   [Handles de connexion](../../../odbc/reference/develop-app/connection-handles.md)  
  
-   [Handles d’instruction](../../../odbc/reference/develop-app/statement-handles.md)  
  
-   [Handles de descripteur](../../../odbc/reference/develop-app/descriptor-handles.md)  
  
-   [Transitions d’état](../../../odbc/reference/develop-app/state-transitions.md)
