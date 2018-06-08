---
title: Préparer et exécuter une instruction (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-how-to
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- statement execution
- statement preparation
ms.assetid: 0adecc63-4da5-486c-bc48-09a004a2fae6
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: cfd8780e6d4fe804a6a124870c06e43bec752463
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prepare-and-execute-a-statement-odbc"></a>Préparer et exécuter une instruction (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

    
### <a name="to-prepare-a-statement-once-and-then-execute-it-multiple-times"></a>Pour préparer une instruction une fois, puis l'exécuter plusieurs fois  
  
1.  Appelez la fonction [SQLPrepare](http://go.microsoft.com/fwlink/?LinkId=59360) pour préparer l'instruction.  
  
2.  Éventuellement, appelez [SQLNumParams](http://go.microsoft.com/fwlink/?LinkId=58404) pour déterminer le nombre de paramètres dans l'instruction préparée.  
  
3.  Éventuellement, pour chaque paramètre dans l'instruction préparée :  
  
    -   Appelez [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) pour obtenir des informations sur les paramètres.  
  
    -   Liez chaque paramètre à une variable de programme à l'aide de [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md). Configurez tous les paramètres de données en cours d'exécution.  
  
4.  Pour chaque exécution d'une instruction préparée :  
  
    -   Si l'instruction a des marqueurs de paramètres, mettez les valeurs de données dans la mémoire tampon de paramètre lié.  
  
    -   Appelez [SQLExecute](http://go.microsoft.com/fwlink/?LinkId=58400) pour exécuter l'instruction préparée.  
  
    -   Si des paramètres d'entrée de données en cours d'exécution sont utilisés, [SQLExecute](http://go.microsoft.com/fwlink/?LinkId=58400) retourne SQL_NEED_DATA. Envoyez les données par segments à l'aide de [SQLParamData](http://go.microsoft.com/fwlink/?LinkId=58405) et [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md).  
  
### <a name="to-prepare-a-statement-with-column-wise-parameter-binding"></a>Pour préparer une instruction avec la liaison de paramètre selon les colonnes  
  
1.  Appelez [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) pour définir les attributs suivants :  
  
    -   Définissez SQL_ATTR_PARAMSET_SIZE au nombre de jeux (S) de paramètres.  
  
    -   Définissez SQL_ATTR_PARAM_BIND_TYPE à SQL_PARAMETER_BIND_BY_COLUMN.  
  
    -   Définissez l'attribut SQL_ATTR_PARAMS_PROCESSED_PTR de sorte qu'il pointe vers une variable SQLUINTEGER contenant le nombre de paramètres traités.  
  
    -   Définissez SQL_ATTR_PARAMS_STATUS_PTR de sorte qu'il pointe vers un tableau [S] de variables SQLUSSMALLINT contenant les indicateurs d'état de paramètre.  
  
2.  Appelez SQLPrepare pour préparer l’instruction.  
  
3.  Éventuellement, appelez [SQLNumParams](http://go.microsoft.com/fwlink/?LinkId=58404) pour déterminer le nombre de paramètres dans l'instruction préparée.  
  
4.  Éventuellement, pour chaque paramètre dans l’instruction préparée, appelez SQLDescribeParam pour obtenir des informations sur les paramètres.  
  
5.  Pour chaque marqueur de paramètre :  
  
    -   Allouez un tableau de S mémoires tampons de paramètres pour stocker les valeurs de données.  
  
    -   Allouez un tableau de S mémoires tampons de paramètres pour stocker les longueurs de données.  
  
    -   Appelez SQLBindParameter pour lier les données et la valeur de longueur tableaux de données au paramètre d’instruction.  
  
    -   Si le paramètre est un paramètre d'image ou de texte de données en cours d'exécution, installez-le.  
  
    -   Si des paramètres de données en cours d'exécution sont utilisés, installez-les.  
  
6.  Pour chaque exécution d'une instruction préparée :  
  
    -   Mettez les valeurs de données S et les longueurs de données S dans les tableaux de paramètres liés.  
  
    -   Appelez SQLExecute pour exécuter l’instruction préparée.  
  
    -   Si les paramètres d’entrée à l’exécution sont utilisés, SQLExecDirect renvoie SQL_NEED_DATA. Envoyer les données dans des segments à l’aide de SQLParamData et SQLPutData.  
  
### <a name="to-prepare-a-statement-with-row-wise-bound-parameters"></a>Pour préparer une instruction avec des paramètres liés selon les lignes  
  
1.  Allouez un tableau [S] de structures, où S est le nombre de jeux de paramètres. La structure dispose d'un élément pour chaque paramètre et chaque élément contient deux parties :  
  
    -   La première partie est une variable du type de données approprié destinée à contenir les données de paramètres.  
  
    -   La deuxième partie est une variable SQLINTEGER destinée à contenir l'indicateur d'état.  
  
2.  Appelez [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) pour définir les attributs suivants :  
  
    -   Définissez SQL_ATTR_PARAMSET_SIZE au nombre de jeux (S) de paramètres.  
  
    -   Définissez SQL_ATTR_PARAM_BIND_TYPE à la taille de la structure allouée à l'étape 1.  
  
    -   Définissez l'attribut SQL_ATTR_PARAMS_PROCESSED_PTR de sorte qu'il pointe vers une variable SQLUINTEGER contenant le nombre de paramètres traités.  
  
    -   Définissez SQL_ATTR_PARAMS_STATUS_PTR de sorte qu'il pointe vers un tableau [S] de variables SQLUSSMALLINT contenant les indicateurs d'état de paramètre.  
  
3.  Appelez SQLPrepare pour préparer l’instruction.  
  
4.  Pour chaque marqueur de paramètre, appelez SQLBindParameter pour diriger le paramètre de données valeur et les données de longueur de pointeur vers leurs variables dans le premier élément du tableau de structures alloué à l’étape 1. Si le paramètre est un paramètre de données en cours d'exécution, installez-le.  
  
5.  Pour chaque exécution d'une instruction préparée :  
  
    -   Remplissez le tableau de tampons de paramètres liés avec les valeurs de données.  
  
    -   Appelez SQLExecute pour exécuter l’instruction préparée. Le pilote exécute efficacement l'instruction SQL S fois, une fois pour chaque jeu de paramètres.  
  
    -   Si les paramètres d’entrée à l’exécution sont utilisés, SQLExecDirect renvoie SQL_NEED_DATA. Envoyer les données dans des segments à l’aide de SQLParamData et SQLPutData.  
  
## <a name="see-also"></a>Voir aussi  
 [L’exécution de rubriques de procédures de requêtes & #40 ; ODBC & #41 ;](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
  
