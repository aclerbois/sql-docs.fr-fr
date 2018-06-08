---
title: L’exécution d’une instruction | Documents Microsoft
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
- SQL statements [ODBC], executing
ms.assetid: e5f0d2ee-0453-4faf-b007-12978dd300a1
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c111620b2f06e7b2eacb159d7cf1c8817bd27c59
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="executing-a-statement"></a>L’exécution d’une instruction
Il existe quatre méthodes pour exécuter une instruction, en fonction de quand elles sont compilées (préparé) par le moteur de base de données et qui les définit :  
  
-   **Dirigent l’exécution** l’application définit l’instruction SQL. Il est préparée et exécutée au moment de l’exécution en une seule étape.  
  
-   **Préparation de l’exécution** l’application définit l’instruction SQL. Il est préparée et exécutée au moment de l’exécution dans des étapes distinctes. L’instruction peut être une seule fois et exécutée plusieurs fois.  
  
-   **Procédures** l’application de définir et de compiler une ou plusieurs instructions SQL au développement de temps et stocker ces instructions sur la source de données en tant que procédure. La procédure est exécutée une ou plusieurs fois au moment de l’exécution. L’application peut énumérer les procédures stockées disponibles à l’aide des fonctions de catalogue.  
  
-   **Fonctions de catalogue** le créateur du pilote crée une fonction qui retourne un jeu de résultats prédéfinis. En règle générale, cette fonction envoie une instruction SQL prédéfinie ou appelle une procédure créée à cet effet. La fonction est exécutée une ou plusieurs fois au moment de l’exécution.  
  
 Une instruction spécifique (tel qu’identifié par son handle d’instruction) peut être exécutée un nombre de fois. L’instruction peut être exécutée avec une variété d’instructions SQL différentes, ou elle peut être exécutée à plusieurs reprises avec la même instruction SQL. Par exemple, le code suivant utilise le même descripteur d’instruction (*hstmt1*) pour récupérer et afficher les tables dans la base de données de ventes. Puis, il réutilise ce handle pour récupérer les colonnes dans une table sélectionnée par l’utilisateur.  
  
```  
SQLHSTMT    hstmt1;  
SQLCHAR *   Table;  
  
// Create a result set of all tables in the Sales database.  
SQLTables(hstmt1, "Sales", SQL_NTS, "sysadmin", SQL_NTS, NULL, 0, NULL, 0);  
  
// Fetch and display the table names; then close the cursor.  
// Code not shown.  
  
// Have the user select a particular table.  
SelectTable(Table);  
  
// Reuse hstmt1 to create a result set of all columns in Table.  
SQLColumns(hstmt1, "Sales", SQL_NTS, "sysadmin", SQL_NTS, Table, SQL_NTS, NULL, 0);  
  
// Fetch and display the column names in Table; then close the cursor.  
// Code not shown.  
```  
  
 Et le code suivant montre comment un handle unique est utilisé pour exécuter à plusieurs reprises la même instruction pour supprimer des lignes d’une table.  
  
```  
SQLHSTMT      hstmt1;  
SQLUINTEGER   OrderID;  
SQLINTEGER    OrderIDInd = 0;  
  
// Prepare a statement to delete orders from the Orders table.  
SQLPrepare(hstmt1, "DELETE FROM Orders WHERE OrderID = ?", SQL_NTS);  
  
// Bind OrderID to the parameter for the OrderID column.  
SQLBindParameter(hstmt1, 1, SQL_PARAM_INPUT, SQL_C_ULONG, SQL_INTEGER, 5, 0,  
                  &OrderID, 0, &OrderIDInd);  
  
// Repeatedly execute hstmt1 with different values of OrderID.  
while ((OrderID = GetOrderID()) != 0) {  
   SQLExecute(hstmt1);  
}  
```  
  
 Pour de nombreux pilotes, allouer des instructions est une tâche coûteuse, afin de réutiliser la même instruction de cette manière est généralement plus efficace que la libération des instructions existantes et nouvelles allocation. Applications qui créent des jeux de résultats sur une instruction doivent être attentif fermer le curseur sur les résultats avant de réexécuter l’instruction ; Pour plus d’informations, consultez [fermeture du curseur](../../../odbc/reference/develop-app/closing-the-cursor.md).  
  
 Réutilisation d’instructions force également l’application afin d’éviter une limitation de certains pilotes du nombre d’instructions qui peuvent être actives en même temps. La définition exacte de « actif » est spécifique au pilote, mais il fait souvent référence à une instruction qui a été préparée ou exécutée et a toujours les résultats disponibles. Par exemple, après un **insérer** instruction a été préparée, il est généralement considéré comme actif ; après un **sélectionnez** instruction a été exécutée et le curseur est toujours ouvert, il est généralement considéré comme actif ; après un **CREATE TABLE** instruction a été exécutée, il n’est pas généralement considéré comme étant actif.  
  
 Une application détermine le nombre d’instructions peut être actif sur une seule connexion à la fois en appelant **SQLGetInfo** avec l’option SQL_MAX_CONCURRENT_ACTIVITIES. Une application peut utiliser des instructions plus actives que cette limite en ouvrant plusieurs connexions à la source de données ; Étant donné que les connexions peuvent être coûteuses, toutefois, l’effet sur les performances doit être considérée.  
  
 Les applications peuvent limiter la quantité de temps alloué pour une instruction à exécuter avec l’attribut d’instruction SQL_ATTR_QUERY_TIMEOUT. Si le délai d’attente expire avant que la source de données retourne le jeu de résultats, la fonction de l’exécution de l’instruction SQL retourne SQLSTATE HYT00 (délai d’attente expiré). Par défaut, il n’existe aucun délai d’expiration.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Exécution directe](../../../odbc/reference/develop-app/direct-execution-odbc.md)  
  
-   [Exécution préparée](../../../odbc/reference/develop-app/prepared-execution-odbc.md)  
  
-   [Procédures](../../../odbc/reference/develop-app/procedures-odbc.md)  
  
-   [Lots d’instructions SQL](../../../odbc/reference/develop-app/batches-of-sql-statements.md)  
  
-   [Exécution de fonctions de catalogue](../../../odbc/reference/develop-app/executing-catalog-functions.md)
