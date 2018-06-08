---
title: SQLDescribeParam | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLDescribeParam function
ms.assetid: 396e74b1-5d08-46dc-b404-2ef2003e4689
caps.latest.revision: 61
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e3944044d3b00af02f93ce2e8d41f78caf23dd1d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqldescribeparam"></a>SQLDescribeParam
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Pour décrire les paramètres d’une instruction SQL, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client génère et exécute un [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction SELECT lorsque SQLDescribeParam est appelée sur un descripteur d’instruction ODBC préparé. Les métadonnées du jeu de résultats déterminent les caractéristiques des paramètres dans l'instruction préparée. SQLDescribeParam peut retourner tout code d’erreur SQLExecute ou SQLExecDirect peut retourner.  
  
 Améliorations dans le moteur de base de données en commençant par [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] autoriser SQLDescribeParam obtenir des descriptions plus exactes des résultats attendus. Ces résultats plus précis peuvent différer des valeurs retournées par SQLDescribeParam dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Metadata Discovery](../../relational-databases/native-client/features/metadata-discovery.md).  
  
 Nouveauté de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], *ParameterSizePtr* maintenant retourne une valeur qui correspond à la définition de la taille, en caractères, de la colonne ou l’expression du marqueur de paramètre correspondant, tel que défini dans le [spécification ODBC](http://go.microsoft.com/fwlink/?LinkId=207044). Dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, *ParameterSizePtr* peut être la valeur correspondante de **SQL_DESC_OCTET_LENGTH** pour le type, ou une valeur de taille de colonne peu pertinente fournie à SQLBindParameter pour un type, dont la valeur doit être ignorée (**SQL_INTEGER**, par exemple).  
  
 Le pilote ne prend pas en charge l’appel SQLDescribeParam dans les situations suivantes :  
  
-   Après avoir SQLExecDirect pour toute [!INCLUDE[tsql](../../includes/tsql-md.md)] des instructions UPDATE ou DELETE contenant la clause FROM.  
  
-   pour toute instruction ODBC ou [!INCLUDE[tsql](../../includes/tsql-md.md)] contenant un paramètre dans une clause HAVING, ou en comparaison au résultat d'une fonction SUM ;  
  
-   pour toute instruction ODBC ou [!INCLUDE[tsql](../../includes/tsql-md.md)] qui dépend d'une sous-requête contenant des paramètres ;  
  
-   pour les instructions SQL ODBC contenant des marqueurs de paramètre dans les deux expressions d'un prédicat de comparaison, like ou quantifié ;  
  
-   pour les requêtes dans lesquelles l'un des paramètres est un paramètre d'une fonction ;  
  
-   Lorsqu’il existe des commentaires (/ * \*/) dans le [!INCLUDE[tsql](../../includes/tsql-md.md)] commande.  
  
 Lors du traitement d’un lot de [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions, le pilote également ne pas prend en charge l’appel de SQLDescribeParam pour les marqueurs de paramètre dans les instructions après la première instruction du lot.  
  
 Lorsque vous décrivez les paramètres de procédures stockées préparées, SQLDescribeParam utilise la procédure stockée système [sp_sproc_columns](../../relational-databases/system-stored-procedures/sp-sproc-columns-transact-sql.md) pour récupérer les caractéristiques des paramètres. sp_sproc_columns peut signaler des données pour les procédures stockées dans la base de données utilisateur actuel. Préparation d’un nom de procédure stockée complet permet de SQLDescribeParam exécuter sur plusieurs bases de données. Par exemple, la procédure stockée système [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md) peut être préparée et exécutée dans une base de données en tant que :  
  
```  
SQLPrepare(hstmt, "{call sp_who(?)}", SQL_NTS);  
```  
  
 L’exécution de SQLDescribeParam après préparation réussie retourne une ligne vide est définie lorsqu’il est connecté à une base de données mais **master**. Le même appel, préparé comme suit, entraîne SQLDescribeParam réussisse, quelle que soit la base de données utilisateur actuel :  
  
```  
SQLPrepare(hstmt, "{call master..sp_who(?)}", SQL_NTS);  
```  
  
 Pour les types de données de valeur élevée, la valeur retournée dans *DataTypePtr* est SQL_VARCHAR, SQL_VARBINARY ou SQL_NVARCHAR. Pour indiquer que la taille du paramètre de type de données de valeur élevée est « illimitée », le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jeux de pilote ODBC Native Client *ParameterSizePtr* à 0. Les valeurs de taille réelles sont retournées pour standard **varchar** paramètres.  
  
> [!NOTE]  
>  Si le paramètre a déjà été lié à une taille maximale pour les paramètres SQL_VARCHAR, SQL_VARBINARY ou SQL_WVARCHAR, la taille liée du paramètre est retournée, et non « illimitée ».  
  
 Pour lier un paramètre d'entrée de taille « illimitée », des données en cours d'exécution doivent être utilisées. Il n’est pas possible de lier un paramètre de sortie de taille « illimitée » (il n’existe aucune méthode pour la diffusion en continu des données à partir d’un paramètre de sortie, tel que [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) pour les jeux de résultats).  
  
 Pour les paramètres de sortie, une mémoire tampon doit être liée et si la valeur est trop grande, la mémoire tampon est remplie et un message SQL_SUCCESS_WITH_INFO est retourné avec l'avertissement « Troncation à droite de la chaîne de données ». Les données tronquées sont alors ignorées.  
  
## <a name="sqldescribeparam-and-table-valued-parameters"></a>SQLDescribeParam et paramètres table  
 Une application peut récupérer les informations de paramètre table pour une instruction préparée avec SQLDescribeParam. Pour plus d’informations, consultez [des métadonnées de paramètre table pour des instructions préparées](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md).  
  
 Pour plus d’informations sur les paramètres table en général, consultez [paramètres table &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqldescribeparam-support-for-enhanced-date-and-time-features"></a>Prise en charge de SQLDescribeParam pour les fonctionnalités de date et heure améliorées  
 Les valeurs retournées pour les types date/heure sont les suivantes :  
  
||*DataTypePtr*|*ParameterSizePtr*|*DecimalDigitsPtr*|  
|-|-------------------|------------------------|------------------------|  
|datetime|SQL_TYPE_TIMESTAMP|23|3|  
|smalldatetime|SQL_TYPE_TIMESTAMP|16|0|  
|date|SQL_TYPE_DATE|10|0|  
|time|SQL_SS_TIME2|8, 10..16|0..7|  
|datetime2|SQL_TYPE_TIMESTAMP|19, 21..27|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|26, 28..34|0..7|  
  
 Pour plus d’informations, consultez [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqldescribeparam-support-for-large-clr-udts"></a>Prise en charge SQLDescribeParam pour les types CLR volumineux définis par l'utilisateur  
 **SQLDescribeParam** prend en charge les types CLR volumineux définis par l’utilisateur (UDT). Pour plus d’informations, consultez [Large CLR User-Defined Types & #40 ; ODBC & #41 ;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction SQLDescribeParam](http://go.microsoft.com/fwlink/?LinkId=59339)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
