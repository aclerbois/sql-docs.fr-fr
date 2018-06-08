---
title: Extraction de données de résultat | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-results
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC], fetching
- SQLBindCol function
- result sets [ODBC], fetching
- fetching [ODBC]
- ODBC data types, fetching
- SQLFetch function
- SQL Server Native Client ODBC driver, data types
- SQLGetData function
ms.assetid: b289c7fb-5017-4d7e-a2d3-19401e9fc4cd
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 2909c4472a4ab8dee56c37da3cdf23db1ee3e512
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fetching-result-data"></a>Extraction des données de résultat
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Une application ODBC offre trois options pour l'extraction des données de résultat.  
  
 La première option est basée sur [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md). Avant l’extraction du résultat défini, l’application utilise **SQLBindCol** pour lier chaque colonne dans le jeu de résultats à une variable de programme. Une fois les colonnes ont été liés, le pilote transfère les données de la ligne actuelle dans les variables liées pour les colonnes de résultats chaque fois que l’application appelle **SQLFetch** ou [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md). Le pilote gère les conversions de données si la colonne du jeu de résultats et la variable de programme possèdent des types de données différents. Si l’application a une valeur SQL_ATTR_ROW_ARRAY_SIZE supérieure à 1, elle peut lier des colonnes de résultats aux tableaux de variables qui seront tous remplis sur chaque appel à **SQLFetchScroll**.  
  
 La deuxième option est basée sur [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md). L’application n’utilise pas **SQLBindCol** pour lier le résultat de définie les colonnes aux variables de programme. Après chaque appel à **SQLFetch**, l’application appelle **SQLGetData** une fois pour chaque colonne dans le résultat défini. **SQLGetData** fait en sorte que le pilote pour transférer des données d’une colonne de jeu de résultat spécifique à une variable de programme spécifique et spécifie les types de données de la colonne et de la variable. Le pilote peut ainsi convertir des données si la colonne de résultat et la variable de programme possèdent des types de données différents. **Texte**, **ntext**, et **image** les colonnes sont généralement trop volumineuses pour tenir dans une variable de programme mais peuvent néanmoins être extraites à l’aide de **SQLGetData**. Si le **texte**, **ntext**, ou **image** données dans la colonne de résultats est supérieure à la variable de programme, **SQLGetData** retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01004 (chaîne de données tronquées à droite). Les appels successifs à **SQLGetData** retournent des segments successifs de la **texte** ou **image** données. Lorsque la fin des données est atteinte, **SQLGetData** retourne SQL_SUCCESS. Chaque extraction retourne un jeu de lignes, ou « ensemble de lignes », si SQL_ATTR_ROW_ARRAY_SIZE est supérieur à 1. Avant d’utiliser **SQLGetData**, vous devez d’abord utiliser **SQLSetPos** pour spécifier une ligne spécifique dans l’ensemble de lignes que la ligne actuelle.  
  
 La troisième option consiste à utiliser un mélange de **SQLBindCol** et **SQLGetData**. Une application peut, par exemple, lier les dix premières colonnes d’un jeu de résultats et à chaque extraction, appelez ensuite **SQLGetData** trois fois pour récupérer les données à partir de trois colonnes indépendantes. Cela doit généralement être utilisé lorsqu’un jeu de résultats contient un ou plusieurs **texte** ou **image** colonnes.  
  
 Selon les options de curseur définies pour le jeu de résultats, une application peut également utiliser les options de défilement de **SQLFetchScroll** pour faire défiler le jeu de résultats.  
  
 L’utilisation excessive de **SQLBindCol** pour lier un résultat de jeu de colonnes à une variable de programme est coûteuse, car **SQLBindCol** provoque un pilote ODBC allouer de la mémoire. Lorsque vous liez une colonne de résultats à une variable, cette liaison demeure en vigueur jusqu'à ce que vous appeliez soit [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) pour libérer le descripteur d’instruction ou d’un appel [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) avec *fOption* défini sur SQL_UNBIND. Les liaisons ne sont pas automatiquement annulées à la fin de l'instruction.  
  
 Cette logique vous permet de gérer avec efficacité l'exécution à maintes reprises de la même instruction avec des paramètres différents. Étant donné que le jeu de résultats conserve la même structure, vous pouvez lier le jeu de résultats qu’une seule fois, traiter les instructions SELECT, puis appelez **SQLFreeStmt** avec *fOption* défini sur SQL_UNBIND après la dernière exécution. Vous ne devez pas appeler **SQLBindCol** pour lier les colonnes dans un jeu de résultats sans appeler d’abord **SQLFreeStmt** avec *fOption* défini sur SQL_UNBIND pour libérer toutes les liaisons précédentes.  
  
 Lorsque vous utilisez **SQLBindCol**, vous pouvez procéder à la liaison selon les lignes ou colonnes. La liaison selon les lignes est un tantinet plus rapide que la liaison selon les colonnes.  
  
 Vous pouvez utiliser **SQLGetData** pour récupérer des données sur une base par colonne au lieu de lier le résultat défini à l’aide de colonnes **SQLBindCol**. Si un jeu de résultats contient uniquement quelques lignes, à l’aide **SQLGetData** au lieu de **SQLBindCol** est plus rapide ; sinon, **SQLBindCol** offre de meilleures performances. Si vous n’insérez pas toujours les données dans le même ensemble de variables, vous devez utiliser **SQLGetData** au lieu de la reliaison en permanence. Vous ne pouvez utiliser **SQLGetData** sur les colonnes qui figurent dans la liste de sélection, une fois que toutes les colonnes sont liées par **SQLBindCol**. La colonne doit également apparaître après toutes les colonnes sur lequel vous avez déjà utilisé **SQLGetData**.  
  
 Les fonctions ODBC qui gèrent le déplacement des données dans ou en dehors des variables de programme, tel que **SQLGetData**, **SQLBindCol**, et [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md), prennent en charge la conversion de type implicite de données. Par exemple, si une application lie une colonne d'entiers à une variable de programme de chaîne de caractères, le pilote convertit automatiquement les données d'entier en caractère avant de les insérer dans la variable de programme.  
  
 La conversion de données dans les applications doit être réduite. À moins que la conversion de données ne soit nécessaire pour le traitement pris en charge par l'application, les applications doivent lier les colonnes et les paramètres à des variables de programme du même type de données. En revanche, si les données doivent être converties d'un type à un autre, il est plus efficace de confier la conversion au pilote plutôt que de l'effectuer dans l'application. Normalement, le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client se contente de transférer directement les données des tampons réseau vers les variables de l'application. En confiant la conversion des données au pilote, vous forcez ce dernier à mettre les données en mémoire tampon et à les convertir au moyen de cycles processeur.  
  
 Variables de programme doivent être suffisamment grandes pour contenir les données transférées à partir d’une colonne, à l’exception de **texte**, **ntext**, et **image** données. Si une application tente d'extraire les données d'un jeu de résultats et de les placer dans une variable trop petite pour les contenir, le pilote émet un avertissement. Cette opération contraint le pilote à allouer de la mémoire pour le message et le pilote et l'application doivent tous les deux utiliser des cycles processeur pour traiter le message et gérer les erreurs. L'application doit soit allouer une variable suffisamment grande pour accueillir les données extraites, soit faire appel à la fonction SUBSTRING dans la liste de sélection pour réduire la taille de la colonne au sein du jeu de résultats.  
  
 Soyez vigilant lorsque vous utilisez SQL_C_DEFAULT pour définir le type de la variable C. SQL_C_DEFAULT spécifie que le type de la variable C correspond au type de données SQL de la colonne ou du paramètre. Si SQL_C_DEFAULT pour une **ntext**, **nchar**, ou **nvarchar** colonne, données Unicode sont retournées à l’application. Ceci peut entraîner divers problèmes si l'application n'a pas été codée pour gérer des données Unicode. Les mêmes types de problèmes peuvent se produire avec les **uniqueidentifier** type de données (SQL_GUID).  
  
 **texte**, **ntext**, et **image** données sont généralement trop volumineuses pour tenir dans une variable de programme unique et est généralement traitées avec **SQLGetData** à la place de **SQLBindCol**. Lors de l’utilisation de curseurs côté serveur, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client est optimisé pour ne pas transmettre les données pour indépendant **texte**, **ntext**, ou **image** colonnes au moment de l’extraction de la ligne. Le **texte**, **ntext**, ou **image** données ne sont pas réellement récupérées à partir du serveur jusqu'à ce que les problèmes des applications **SQLGetData** pour la colonne.  
  
 Cette optimisation peut être appliquée aux applications afin qu’aucun **texte**, **ntext**, ou **image** données s’affiche lors d’un utilisateur fait défiler haut et bas d’un curseur. Une fois que l’utilisateur sélectionne une ligne, l’application peut appeler **SQLGetData** pour récupérer le **texte**, **ntext**, ou **image** données. Ceci évite de transmettre le **texte**, **ntext**, ou **image** données pour une lignes de l’utilisateur ne sélectionne pas et peut enregistrer la transmission de très grandes quantités de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Le traitement des résultats &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
