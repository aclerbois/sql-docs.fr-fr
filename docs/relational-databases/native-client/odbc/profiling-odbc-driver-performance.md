---
title: Profilage des performances du pilote ODBC | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|ODBC
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- profiling ODBC driver performance data [SQL Server Native Client]
- performance [ODBC]
- application statistics [ODBC]
- time statistics [ODBC]
- ODBC, performance data
- SQL Server Native Client ODBC driver, profiling performance data
- SQLPERF data structure
- statistical information [ODBC]
ms.assetid: 8f44e194-d556-4119-a759-4c9dec7ecead
caps.latest.revision: 36
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 337209ac91faecab319f66bcb9b61252e3444f5a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="profiling-odbc-driver-performance"></a>Profilage des performances du pilote ODBC
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Le pilote ODBC de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client peut profiler deux types de données de performances :  
  
-   Requêtes longues.  
  
     Le pilote peut écrire dans un fichier journal les requêtes qui n'obtiennent pas de réponse du serveur dans un délai spécifique. Les programmeurs d'applications ou les administrateurs de base de données peuvent ensuite faire des recherches sur chaque instruction SQL enregistrée pour déterminer comment améliorer ses performances.  
  
-   Données de performances du pilote.  
  
     Le pilote peut enregistrer des statistiques de performance et les écrire dans un fichier ou les rendre accessibles à une application via une structure de données spécifique au pilote nommée SQLPERF. Le fichier contenant les statistiques de performance est un fichier délimité par des tabulations qui peut être analysé facilement à l'aide d'un tableur prenant en charge les fichiers délimités par des tabulations, par exemple Microsoft Excel.  
  
 Les deux types de profilages peuvent être activés par :  
  
-   connexion à une source de données qui spécifie l'enregistrement ;  
  
-   Appel de [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) pour définir les attributs spécifiques au pilote qui contrôlent le profilage.  
  
 Chaque processus d'application obtient sa propre copie du pilote ODBC de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ; par ailleurs, le profilage est global pour la combinaison d'une copie du pilote et d'un processus d'application. Lorsque le profilage est activé dans l'application, il enregistre les informations relatives à toutes les connexions actives dans le pilote à partir de cette application. Même les connexions qui n'ont pas demandé spécifiquement de profilage sont incluses.  
  
 Une fois que le pilote a ouvert un journal de profilage (journal des données de performances ou journal des requêtes longues), il ne ferme pas ce journal tant qu'il n'est pas déchargé par le gestionnaire de pilotes ODBC, lorsqu'une application libère tous les handles d'environnement ouverts dans le pilote. Si l'application ouvre un nouveau handle d'environnement, une nouvelle copie du pilote est chargée. Si l'application se connecte ensuite à une source de données qui spécifie le même fichier journal ou définit les attributs spécifiques au pilote à enregistrer dans le même fichier, le pilote remplace l'ancien journal.  
  
 Si une application démarre le profilage dans un fichier journal et si une seconde application essaie de démarrer le profilage dans le même fichier journal, la seconde application n'est pas en mesure d'enregistrer les données de profilage. Si la seconde application démarre le profilage une fois que la première application a déchargé son pilote, la seconde application remplace le fichier journal de la première application.  
  
 Si une application se connecte à une source de données qui le profilage est activé, le pilote retourne SQL_ERROR si l’application appelle **SQLSetConnectOption** pour démarrer l’enregistrement. Un appel à **SQLGetDiagRec** puis retourne les éléments suivants :  
  
```  
SQLState: 01000, pfNative = 0  
ErrorMsg: [Microsoft][SQL Server Native Client]  
   An error has occurred during the attempt to access  
   the log file, logging disabled.  
```  
  
 Le pilote cesse de rassembler les données de performances lorsqu'un handle d'environnement est fermé. Si une application [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client a plusieurs connexions, chacune avec son propre handle d'environnement, le pilote cesse de rassembler les données de performances lorsque les handles d'environnement associés sont fermés.  
  
 Les données de performances du pilote peuvent être stockées dans la structure de données SQLPERF ou être enregistrées dans un fichier délimité par des tabulations. Les données incluent les catégories suivantes de statistiques :  
  
-   Profil de l'application  
  
-   Connexion  
  
-   Réseau  
  
-   Time  
  
 Dans le tableau suivant, les descriptions des champs de la structure de données SQLPERF s'appliquent également aux statistiques enregistrées dans le fichier journal de performance.  
  
### <a name="application-profile-statistics"></a>Statistiques relatives au profil de l'application  
  
|Champ SQLPERF| Description|  
|-------------------|-----------------|  
|TimerResolution|Résolution minimale de l'heure du serveur en millisecondes. Elle est signalée habituellement sous la forme 0 (zéro) et doit être prise en considération uniquement si le nombre indiqué est de grande taille. Si la résolution minimale de l'heure du serveur est supérieure à l'intervalle probable de certaines statistiques basées sur l'horloge, il est possible qu'il y ait augmentation de ces statistiques.|  
|SQLidu|Nombre d'instructions INSERT, DELETE ou UPDATE après SQL_PERF_START.|  
|SQLiduRows|Nombre d'instructions INSERT, DELETE ou UPDATE après SQL_PERF_START.|  
|SQLSelects|Nombre d'instructions SELECT traitées après SQL_PERF_START.|  
|SQLSelectRows|Nombre de lignes sélectionnées après SQL_PERF_START.|  
|Transactions|Nombre de transactions utilisateur après SQL_PERF_START, y compris les restaurations. Lorsqu'une application ODBC s'exécute avec SQL_AUTOCOMMIT_ON, chaque commande est considérée comme une transaction.|  
|SQLPrepares|Nombre de [fonction SQLPrepare](http://go.microsoft.com/fwlink/?LinkId=59360) appels après SQL_PERF_START.|  
|ExecDirects|Nombre de **SQLExecDirect** appels après SQL_PERF_START.|  
|SQLExecutes|Nombre de **SQLExecute** appels après SQL_PERF_START.|  
|CursorOpens|Nombre de fois où le pilote a ouvert un curseur côté serveur après SQL_PERF_START.|  
|CursorSize|Nombre de lignes dans les jeux de résultats ouverts par les curseurs après SQL_PERF_START.|  
|CursorUsed|Nombre de lignes réellement récupérées par le pilote à partir des curseurs après SQL_PERF_START.|  
|PercentCursorUsed|Est égal à CursorUsed/CursorSize. Par exemple, si une application oblige le pilote à ouvrir un curseur côté serveur pour exécuter SELECT COUNT(*) FROM Authors, 23 lignes figurent dans le jeu de résultats de l'instruction SELECT. Si l'application extrait ensuite uniquement trois de ces lignes, CursorUsed/CursorSize est égal à 3/23 ; par conséquent, PercentCursorUsed est égal à 13,043478.|  
|AvgFetchTime|Est égal à SQLFetchTime/SQLFetchCount.|  
|AvgCursorSize|Est égal à CursorSize/CursorOpens.|  
|AvgCursorUsed|Est égal à CursorUsed/CursorOpens.|  
|SQLFetchTime|Durée cumulative nécessaire à l'exécution des extractions par rapport aux curseurs côté serveur.|  
|SQLFetchCount|Nombre d'extractions effectuées par rapport aux curseurs côté serveur après SQL_PERF_START.|  
|CurrentStmtCount|Nombre de descripteurs d'instruction ouverts actuellement sur toutes les connexions ouvertes dans le pilote.|  
|MaxOpenStmt|Nombre maximal de descripteurs d'instruction ouverts simultanément après SQL_PERF_START.|  
|SumOpenStmt|Nombre de descripteurs d'instruction ouverts après SQL_PERF_START.|  
|**Statistiques de connexion :**||  
|CurrentConnectionCount|Nombre actuel de handles de connexion actifs ouverts par l'application sur le serveur.|  
|MaxConnectionsOpened|Nombre maximal de handles de connexion simultanés ouverts après SQL_PERF_START.|  
|SumConnectionsOpened|Somme correspondant au nombre de handles de connexion ouverts après SQL_PERF_START.|  
|SumConnectionTime|Somme correspondant à la durée d'ouverture de toutes les connexions après SQL_PERF_START. Par exemple, si une application a ouvert 10 connexions et a conservé chaque connexion pendant 5 secondes, SumConnectionTime est égal à 50 secondes.|  
|AvgTimeOpened|Est égal à SumConnectionsOpened/ SumConnectionTime.|  
|**Statistiques réseau :**||  
|ServerRndTrips|Nombre de fois où le pilote a envoyé des commandes au serveur et a obtenu une réponse.|  
|BuffersSent|Nombre de paquets TDS (Tabular Data Stream) envoyés à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] par le pilote après SQL_PERF_START. Les commandes de taille importante peuvent nécessiter plusieurs mémoires tampons ; par conséquent, si une commande de taille importante est envoyée au serveur et si elle occupe six paquets, ServerRndTrips est incrémenté d'une unité et BuffersSent de six unités.|  
|BuffersRec|Nombre de paquets TDS reçus par le pilote à partir de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] une fois que l'application a commencé à utiliser le pilote.|  
|BytesSent|Nombre d'octets de données envoyés à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans des paquets TDS une fois que l'application a commencé à utiliser le pilote.|  
|BytesRec|Nombre d'octets de données des paquets TDS reçus par le pilote à partir de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] une fois que l'application a commencé à utiliser le pilote.|  
  
### <a name="time-statistics"></a>Statistiques relatives au temps  
  
|Champ SQLPERF| Description|  
|-------------------|-----------------|  
|msExecutionTime|Durée cumulative de traitement du pilote après SQL_PERF_START, incluant la durée d'attente des réponses du serveur.|  
|msNetworkServerTime|Durée cumulative d'attente des réponses du serveur par le pilote.|  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40;ODBC&#41;](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Profilage des rubriques de procédures Performance pilote ODBC & #40 ; ODBC & #41 ;](../../../relational-databases/native-client-odbc-how-to/profiling-odbc-driver-performance-odbc.md)  
  
  
