---
title: L’exécution d’ODBC préparée | Documents Microsoft
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
- prepared execution [ODBC]
- SQL statements [ODBC], prepared execution
- SQL statements [ODBC], executing
ms.assetid: f08c8a98-31ee-48b2-9dbf-6f31c2166dbb
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1e03123e34834033b4c53fb1eb5818f6c78def82
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prepared-execution-odbc"></a>Exécution préparée ODBC
L’exécution préparée est un moyen efficace pour exécuter une instruction plusieurs fois. L’instruction est compilée tout d’abord, ou *préparée,* dans un plan d’accès. Le plan d’accès est alors exécutée une ou plusieurs fois à une date ultérieure. Pour plus d’informations sur les plans d’accès, consultez [le traitement d’une instruction SQL](../../../odbc/reference/processing-a-sql-statement.md).  
  
 Exécution préparée est couramment utilisée par les applications verticales et personnalisées pour exécuter à plusieurs reprises la même instruction SQL paramétrable. Par exemple, le code suivant prépare une instruction pour mettre à jour les prix des différentes parties. Il exécute ensuite l’instruction plusieurs fois avec différentes valeurs de paramètre chaque fois.  
  
```  
SQLREAL       Price;  
SQLUINTEGER   PartID;  
SQLINTEGER    PartIDInd = 0, PriceInd = 0;  
  
// Prepare a statement to update salaries in the Employees table.  
SQLPrepare(hstmt, "UPDATE Parts SET Price = ? WHERE PartID = ?", SQL_NTS);  
  
// Bind Price to the parameter for the Price column and PartID to  
// the parameter for the PartID column.  
SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_FLOAT, SQL_REAL, 7, 0,  
                  &Price, 0, &PriceInd);  
SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_ULONG, SQL_INTEGER, 10, 0,  
                  &PartID, 0, &PartIDInd);  
  
// Repeatedly execute the statement.  
while (GetPrice(&PartID, &Price)) {  
   SQLExecute(hstmt);  
}  
```  
  
 Exécution préparée est plus rapide que l’exécution directe pour les instructions exécutées plusieurs fois, principalement du fait que l’instruction est compilée une seule fois ; instructions exécutées directement sont compilées chaque fois qu’ils sont exécutés. Exécution préparée peut également fournir qu'une réduction du trafic réseau, car le pilote peut envoyer un identificateur de plan d’accès à la source de données chaque fois que l’instruction est exécutée, au lieu d’une instruction SQL entière, si le prend en charge l’accès aux sources de données envisagez d’identificateurs.  
  
 L’application peut récupérer les métadonnées pour le jeu de résultats après que l’instruction est préparée et avant son exécution. Toutefois, les instructions non exécutées retour de métadonnées pour préparé, est coûteuses pour certains pilotes et doit être évité si possible par les applications interopérables. Pour plus d’informations, consultez [métadonnées de valeur de résultat](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 L'exécution préparée ne doit pas être utilisée pour les instructions exécutées une seule fois. Pour ces instructions, il est légèrement plus lente que l’exécution directe, car elle requiert un appel de fonction ODBC supplémentaire.  
  
> [!IMPORTANT]  
>  Validation ou la restauration d’une transaction en appelant explicitement **SQLEndTran** ou en travaillant dans le mode de validation automatique, certaines sources de données supprimer les plans d’accès pour toutes les instructions sur une connexion. Pour plus d’informations, consultez les options SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR dans les [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) description de fonction.  
  
 Pour préparer et exécuter une instruction, l’application :  
  
1.  Appels **SQLPrepare** et lui passe une chaîne contenant l’instruction SQL.  
  
2.  Définit les valeurs des paramètres. Paramètres réellement peuvent être définis avant ou après la préparation de l’instruction. Pour plus d’informations, consultez [paramètres de l’instruction](../../../odbc/reference/develop-app/statement-parameters.md), plus loin dans cette section.  
  
3.  Appels **SQLExecute** et effectue un traitement supplémentaire est nécessaire, par exemple, l’extraction de données.  
  
4.  Répétez les étapes 2 et 3 si nécessaire.  
  
5.  Lorsque **SQLPrepare** est appelée, le pilote :  
  
    -   Modifie l’instruction SQL pour utiliser la grammaire SQL de la source de données sans analyse de l’instruction. Cela inclut en remplaçant les séquences d’échappement présentés dans [les séquences d’échappement dans ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md). L’application peut récupérer le formulaire de modification d’une instruction SQL en appelant **SQLNativeSql**. Les séquences d’échappement ne sont pas remplacés si la valeur de l’attribut d’instruction SQL_ATTR_NOSCAN.  
  
    -   Envoie l’instruction à la source de données de préparation.  
  
    -   Stocke l’identificateur de plan d’accès renvoyé pour une exécution ultérieure (si la préparation a réussi) ou retourne toutes les erreurs (si la préparation de l’échec). Erreurs incluent les erreurs syntaxiques comme SQLSTATE 42000 (syntaxe ou violation d’accès) et les erreurs sémantiques telles que 42 s 02 SQLSTATE (Base de table ou vue introuvable).  
  
        > [!NOTE]  
        >  Certains pilotes ne renvoie pas les erreurs à ce stade, mais les retourner à la place lorsque l’instruction est exécutée ou lorsque les fonctions de catalogue sont appelées. Par conséquent, **SQLPrepare** semblera avoir réussi lorsqu’en fait, il a échoué.  
  
6.  Lorsque **SQLExecute** est appelée, le pilote :  
  
    -   Récupère les valeurs de paramètre en cours et les convertit si nécessaire. Pour plus d’informations, consultez [paramètres de l’instruction](../../../odbc/reference/develop-app/statement-parameters.md), plus loin dans cette section.  
  
    -   Envoie l’identificateur de plan d’accès et les valeurs de paramètre converti à la source de données.  
  
    -   Retourne toutes les erreurs. Il s’agit des erreurs d’exécution généralement comme SQLSTATE 24000 (état de curseur non valide). Toutefois, certains pilotes de renvoient des erreurs syntaxiques et sémantiques à ce stade.  
  
 Si la source de données ne prend pas en charge la préparation de l’instruction, le pilote doit l’émuler dans la mesure du possible. Par exemple, le pilote peut ne rien faire lorsque **SQLPrepare** est appelée, puis effectuez l’exécution directe de l’instruction lorsque **SQLExecute** est appelée.  
  
 Si la source de données prend en charge la vérification sans exécution de la syntaxe, le pilote peut soumettre l’instruction de vérification lorsque **SQLPrepare** est appelée et envoyer l’instruction pour l’exécution lorsque **SQLExecute** est appelée.  
  
 Si le pilote ne peut pas émuler la préparation de l’instruction, elle stocke l’instruction lorsque **SQLPrepare** est appelée et la soumet pour exécution lorsque **SQLExecute** est appelée.  
  
 Car la préparation de l’instruction émulée n’est pas parfaite, **SQLExecute** peut retourner les erreurs retournées normalement par **SQLPrepare**.
