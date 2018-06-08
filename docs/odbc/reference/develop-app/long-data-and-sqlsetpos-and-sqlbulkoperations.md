---
title: Données de type long et SQLSetPos et SQLBulkOperations | Documents Microsoft
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
- long data [ODBC]
- SQLSetPos function [ODBC], long data and SQLBulkOperations
- data updates [ODBC], long data
- updating data [ODBC], long data
- SQLBulkOperations function [ODBC], long data
ms.assetid: e2fdf842-5e4c-46ca-bb21-4625c3324f28
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0c5734480db4ac3b8098254a4c99dbf7a361ea3c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="long-data-and-sqlsetpos-and-sqlbulkoperations"></a>Données de type long et SQLSetPos et SQLBulkOperations
Comme c’est le cas avec des paramètres dans les instructions SQL, les données de type long peuvent être envoyées lors de la mise à jour des lignes avec **SQLBulkOperations** ou **SQLSetPos** ou lors de l’insertion de lignes avec **SQLBulkOperations**. Les données sont envoyées dans des parties, avec plusieurs appels à **SQLPutData**. Les colonnes pour lesquelles les données sont envoyées au moment de l’exécution sont appelées *des colonnes de data-at-execution*.  
  
> [!NOTE]  
>  Une application peut envoyer n’importe quel type de données au moment de l’exécution avec **SQLPutData**, bien que les caractères et les données binaires peuvent être envoyées dans les parties. Toutefois, si les données sont assez petites pour tenir dans un tampon unique, il est généralement inutile d’utiliser **SQLPutData**. Il est beaucoup plus facile de lier la mémoire tampon et de laisser le pilote de récupérer les données à partir de la mémoire tampon.  
  
 Étant donné que les colonnes de données de type long ne sont généralement pas liés, l’application doit lier la colonne avant d’appeler **SQLBulkOperations** ou **SQLSetPos** et annuler la liaison après avoir appelé **SQLBulkOperations** ou **SQLSetPos**. La colonne doit être liée, car **SQLBulkOperations** ou **SQLSetPos** fonctionne uniquement sur les colonnes dépendantes et doit être indépendant afin que **SQLGetData** peut être utilisé pour extraire des données de la colonne.  
  
 Pour envoyer des données au moment de l’exécution, l’application effectue les opérations suivantes :  
  
1.  Place une valeur 32 bits dans le tampon de l’ensemble de lignes au lieu d’une valeur de données. Cette valeur s’affichera à l’application à une version ultérieure, l’application doit définir une valeur explicite, comme le numéro de la colonne ou le handle d’un fichier contenant des données.  
  
2.  Définit la valeur de la mémoire tampon de longueur / d’indicateur pour le résultat de la SQL_LEN_DATA_AT_EXEC (*longueur*) (macro). Cette valeur indique au pilote qui seront envoyées avec les données pour le paramètre **SQLPutData**. Le *longueur* valeur est utilisée lors de l’envoi des données de type long à une source de données qui a besoin de connaître le nombre d’octets de données de type long sera envoyé afin que pré-alloue espace. Pour déterminer si une source de données requiert cette valeur, l’application appelle **SQLGetInfo** avec l’option SQL_NEED_LONG_DATA_LEN. Tous les pilotes doivent prendre en charge cette macro ; Si la source de données ne requiert pas la longueur d’octet, le pilote peut l’ignorer.  
  
3.  Appels **SQLBulkOperations** ou **SQLSetPos**. Le pilote détecte qu’une mémoire tampon de longueur / d’indicateur contient le résultat de la SQL_LEN_DATA_AT_EXEC (*longueur*) (macro) et retourne SQL_NEED_DATA comme valeur de retour de la fonction.  
  
4.  Appels **SQLParamData** en réponse à la SQL_NEED_DATA valeur de retour. Si les données de type long doivent être envoyé, **SQLParamData** retourne SQL_NEED_DATA. Dans la mémoire tampon vers laquelle pointée le *ValuePtrPtr* argument, le pilote retourne la valeur unique que l’application est placée dans le tampon de l’ensemble de lignes. S’il existe plusieurs colonnes de données en cours d’exécution, l’application utilise cette valeur pour déterminer la colonne pour envoyer des données le pilote n’est pas nécessaire pour demander des données pour les colonnes de données en cours d’exécution dans un ordre particulier.  
  
5.  Appels **SQLPutData** pour envoyer les données de colonne pour le pilote. Si les données de colonne ne tient pas dans un tampon unique, comme c’est souvent le cas avec les données de type long, l’application appelle **SQLPutData** à plusieurs reprises pour envoyer les données de parties ; c’est à la source de données et le pilote pour rassembler les données. Si l’application transmet les données de chaîne terminée par null, la source de données ou de pilote doit supprimer le caractère de fin de la valeur null dans le cadre du processus de réassemblage.  
  
6.  Appels **SQLParamData** pour indiquer qu’il a envoyé à toutes les données de la colonne. S’il existe des colonnes de data-at-execution pour lequel les données n’ont pas été envoyées, le pilote retourne SQL_NEED_DATA et la valeur unique pour la colonne de data-at-execution suivante ; l’application retourne à l’étape 5. Si les données ont été envoyées pour toutes les colonnes de données en cours d’exécution, les données de la ligne sont envoyées à la source de données. **SQLParamData** retourne SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO et peut retourner tout SQLSTATE qui **SQLBulkOperations** ou **SQLSetPos** peut retourner.  
  
 Après avoir **SQLBulkOperations** ou **SQLSetPos** retourne SQL_NEED_DATA et avant que les données ont été complètement envoyées pour la dernière colonne de données en cours d’exécution, l’instruction est dans un état besoin des données. Dans cet état, l’application peut appeler uniquement **SQLPutData**, **SQLParamData**, **SQLCancel**, **SQLGetDiagField**, ou **SQLGetDiagRec**; toutes les autres fonctions retournent la valeur SQLSTATE HY010 (erreur de séquence de fonction). Appel de **SQLCancel** annule l’exécution de l’instruction et le retourne à son état précédent. Pour plus d’informations, consultez [Tables de Transition d’état annexe b : ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).
