---
title: À l’aide de fichiers de données et les fichiers de Format | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-bulk-copy-operations
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], file formats
- ODBC, functions
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [SQL Server Native Client]
- ODBC, bulk copy operations
- bulk copy [ODBC], data files
ms.assetid: c01b7155-3f0a-473d-90b7-87a97bc56ca5
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4473473b97545a522ed0a051a69104d2b3d69af8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-data-files-and-format-files"></a>Utilisation de fichiers de données et de format
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le programme de copie en bloc le plus simple permet d'effectuer les opérations suivantes :  
  
1.  Appels [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) pour spécifier la copie en bloc (définir BCP_OUT) à partir d’une table ou une vue dans un fichier de données.  
  
2.  Appels [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md) pour exécuter l’opération de copie en bloc.  
  
 Le fichier de données est créé en mode natif ; les données de toutes les colonnes dans une table ou une vue sont stockées dans le fichier de données sous le même format que celui de la base de données. Le fichier peut ensuite être copié en bloc vers un serveur en suivant la même procédure et en définissant DB_IN au lieu de DB_OUT. Cela ne fonctionne que si les tables source et cible ont exactement la même structure. Le fichier de données qui en résulte peut également être entrée dans le **bcp** utilitaire à l’aide de la **/n** commutateur de (mode natif).  
  
 Pour copier en bloc le jeu de résultats d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] plutôt que directement depuis une table ou une vue :  
  
1.  Appelez **bcp_init** pour spécifier la copie en bloc du délai d’attente, mais spécifiez NULL pour le nom de table.  
  
2.  Appelez [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) avec *eOption* défini sur BCPHINTS et *iValue* défini sur un pointeur vers une chaîne SQLTCHAR contenant l’instruction Transact-SQL.  
  
3.  Appelez **bcp_exec** pour exécuter l’opération de copie en bloc.  
  
 L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] peut être n'importe quelle instruction qui génère un jeu de résultats. Le fichier de données contenant le premier jeu de résultats de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] est créé. La copie en bloc ignore tous les jeux de résultats après le premier si l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] génère plusieurs jeux de résultats.  
  
 Pour créer un fichier de données dans la colonne données sont stockées dans un format différent de celui de la table, appelez [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) pour spécifier le nombre de colonnes est modifié, puis appelez [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md) pour chaque colonne dont vous souhaitez modifier le format. Cette opération est effectuée après l’appel **bcp_init** mais avant d’appeler **bcp_exec**. **bcp_colfmt** Spécifie le format dans lequel les données de la colonne sont stockées dans le fichier de données. Il peut être utilisé lors de la copie en bloc vers ou à partir d'éléments. Vous pouvez également utiliser **bcp_colfmt** pour définir les indicateurs de fin de ligne et de colonne. Par exemple, si vos données ne contient aucun caractère de l’onglet, vous pouvez créer un fichier délimité par des tabulations à l’aide de **bcp_colfmt** pour définir le caractère de tabulation comme indicateur de fin pour chaque colonne.  
  
 Lorsque copie en bloc et à l’aide de **bcp_colfmt**, vous pouvez facilement créer un fichier de format décrivant le fichier de données que vous avez créé en appelant [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md) après le dernier appel à **bcp_colfmt**.  
  
 Lors de la copie en bloc dans un fichier de données décrit par un fichier de format, lire le fichier de format en appelant [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md) après **bcp_init** mais avant **bcp_exec**.  
  
 Le **bcp_control** fonction contrôle plusieurs options de copie en bloc dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d’un fichier de données. **bcp_control** définit les options, telles que le nombre maximal d’erreurs avant l’arrêt, la ligne dans le fichier auquel démarrer la copie en bloc et la ligne d’arrêt sur la taille de lot.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution d’opérations de copie en bloc &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  
