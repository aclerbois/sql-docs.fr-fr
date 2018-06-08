---
title: Embedded SQL exemple | Documents Microsoft
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
- SQL [ODBC], embedded SQL
- SQL statements [ODBC], embedded SQL
- embedded SQL [ODBC]
ms.assetid: b8a26e05-3c82-4c5f-8f01-9de0edb645e9
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8d8983bcabb791c99974055fa718bdd89057e2d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="embedded-sql-example"></a>Exemple SQL incorporé
Le code suivant est un simple programme SQL incorporé, écrit en C. Le programme illustre des techniques SQL de nombreuses, mais pas la totalité, d’incorporé. Le programme vous invite à entrer un numéro de commande de l’utilisateur, récupère le numéro de client, le vendeur et le statut de la commande et affiche les informations récupérées à l’écran.  
  
```  
int main() {  
   EXEC SQL INCLUDE SQLCA;  
   EXEC SQL BEGIN DECLARE SECTION;  
      int OrderID;         /* Employee ID (from user)         */  
      int CustID;            /* Retrieved customer ID         */  
      char SalesPerson[10]   /* Retrieved salesperson name      */  
      char Status[6]         /* Retrieved order status        */  
   EXEC SQL END DECLARE SECTION;  
  
   /* Set up error processing */  
   EXEC SQL WHENEVER SQLERROR GOTO query_error;  
   EXEC SQL WHENEVER NOT FOUND GOTO bad_number;  
  
   /* Prompt the user for order number */  
   printf ("Enter order number: ");  
   scanf_s("%d", &OrderID);  
  
   /* Execute the SQL query */  
   EXEC SQL SELECT CustID, SalesPerson, Status  
      FROM Orders  
      WHERE OrderID = :OrderID  
      INTO :CustID, :SalesPerson, :Status;  
  
   /* Display the results */  
   printf ("Customer number:  %d\n", CustID);  
   printf ("Salesperson: %s\n", SalesPerson);  
   printf ("Status: %s\n", Status);  
   exit();  
  
query_error:  
   printf ("SQL error: %ld\n", sqlca->sqlcode);  
   exit();  
  
bad_number:  
   printf ("Invalid order number.\n");  
   exit();  
}  
```  
  
 Notez les éléments suivants concernant ce programme :  
  
-   **Variables hôte** les variables de l’hôte sont déclarées dans une section délimitée par le **BEGIN DECLARE SECTION** et **END DECLARE SECTION** mots clés. Chaque nom de variable d’hôte est préfixé par un signe deux-points ( :)) lorsqu’il apparaît dans une instruction SQL incorporée. Le signe deux-points permet la précompilés faire la distinction entre les variables de l’hôte et des objets de base de données, tels que des tables et des colonnes qui ont le même nom.  
  
-   **Types de données** les types de données pris en charge par un SGBD et un langage hôte peuvent être différents. Cela affecte les variables de l’hôte, car ils jouent un rôle de double. D’une part, les variables hôtes sont des variables de programme, déclaré et manipulées par les instructions de langage hôte. En revanche, ils sont utilisés dans les instructions SQL incorporées pour récupérer des données de la base de données. S’il n’existe aucun type de langage hôte qui correspond à un type de données SGBD, le SGBD convertit automatiquement les données. Toutefois, étant donné que chaque SGBD possède ses propres règles et les particularités du processus de conversion, les types de variables d’hôte doivent être choisis avec soin.  
  
-   **Gestion des erreurs** le SGBD signale des erreurs d’exécution du programme d’applications via une zone de communication SQL ou SQLCA. Dans l’exemple de code précédent, la première instruction SQL incorporée est INCLUDE SQLCA. Cela indique la précompilés pour inclure la structure SQLCA dans le programme. Cela est nécessaire chaque fois que le programme traite les erreurs retournées par le SGBD. Le WHENEVER... GOTO (instruction) indique le précompilés pour générer le code de gestion des erreurs qui effectue un branchement vers une étiquette spécifique lorsqu’une erreur se produit.  
  
-   **SELECT unique** l’instruction utilisée pour renvoyer les données est une instruction SELECT singleton ; autrement dit, il retourne une seule ligne de données. Par conséquent, l’exemple de code ne déclarez pas ou utiliser des curseurs.
