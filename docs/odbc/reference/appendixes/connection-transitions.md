---
title: Les Transitions de connexion | Documents Microsoft
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
- transitioning states [ODBC], connection
- connection transitions [ODBC]
- state transitions [ODBC], connection
ms.assetid: 6b6e1a47-4a52-41c8-bb9e-7ddeae09913e
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b59fe86e0fb16dca51d24098c98694e77fa7e82b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connection-transitions"></a>Transitions de connexion
Connexions ODBC ont les états suivants.  
  
|État| Description|  
|-----------|-----------------|  
|C0|Environnement non alloué, non alloué de connexion|  
|C1|Environnement alloué, non alloué de connexion|  
|C2|Allouée environnement, allouée de connexion|  
|C3|Fonction de connexion a besoin de données|  
|C4|Connexion connectée|  
|C5|Connecté connexion, allouée d’instruction|  
|C6|Connexion connectée, la transaction en cours. Il est possible qu’une connexion est en état C6 sans instructions allouées sur la connexion. Par exemple, supposons que la connexion est en mode de validation manuelle et est en état C4. Si une instruction est allouée, exécutée (démarrage d’une transaction) et puis libérée, la transaction reste active, mais il n’existe aucune instruction sur la connexion.|  
  
 Les tableaux suivants indiquent comment chaque fonction ODBC affecte l’état de connexion.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|C0<br /><br /> Aucun Env.|C1 non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|--------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|C1 [1]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|(INCLUENT) [2]|C2|--[5]|--[5]|--[5]|--[5]|--[5]|  
|(INCLUENT) [3]|(INCLUENT)|(08003)|(08003)|C5|--[5]|--[5]|  
|(INCLUENT) [4]|(INCLUENT)|(08003)|(08003)|--[5]|--[5]|--[5]|  
  
 [1] cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_ENV.  
  
 [2] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DBC.  
  
 [3] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_STMT.  
  
 [4] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DESC.  
  
 [5] appel de **SQLAllocHandle** avec *OutputHandlePtr* pointant vers un handle valide remplace ce handle sans tenir compte pour le handle d’ofthat contenu précédent et peut provoquer des problèmes pour les pilotes ODBC. Il s’agit de la programmation d’applications ODBC incorrecte pour appeler **SQLAllocHandle** à deux reprises avec la même variable d’application définie pour  *\*OutputHandlePtr* sans appeler **SQLFreeHandle** pour libérer le handle avant de réallouer. Remplacement ODBC gère de manière peut entraîner un comportement incohérent ou erreur de la part des pilotes ODBC.  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|C4 de C3 [d] [s]|--C2 [d] [e] C4 [s]|(08002)|(08002)|(08002)|  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--|--C5 [1] [2]|  
  
 [1] la connexion était en mode de validation manuelle.  
  
 [2] de la connexion était en mode de validation automatique.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges et SQLTables  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--C6 [1] [2]|--|  
  
 [1] la connexion était en mode de validation automatique, ou la source de données n’a pas commencé une transaction.  
  
 [2] de la connexion était en mode de validation manuelle, et la source de données a commencé une transaction.  
  
## <a name="sqlconnect"></a>SQLConnect  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|C4|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlcopydesc-sqlgetdescfield-sqlgetdescrec-sqlsetdescfield-and-sqlsetdescrec"></a>SQLCopyDesc, SQLGetDescField, SQLGetDescRec, SQLSetDescField et SQLSetDescRec  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--[1]|--|--|  
  
 [1] dans cet état, les descripteurs uniquement accessibles à l’application sont allouées explicitement les descripteurs.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources et SQLDrivers  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|--|--|--|--|--|--|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(08003)|C2|C2|C2|25000|  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|S C4--n [f]|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT) [1]|--[3]|--[3]|--[3]|--|--|--[4] ou ([5], [6] et [8]) C4 [5] et [7] C5 [5], [6] et [9]|  
|(INCLUENT) [2]|(INCLUENT)|(08003)|(08003)|--|--|C5|  
  
 [1] cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_ENV.  
  
 [2] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DBC.  
  
 [3], car la connexion n’est pas dans un état connecté, il n’est pas affecté par la transaction.  
  
 [4] de la validation ou de restauration a échoué sur la connexion. La fonction retourne SQL_ERROR dans ce cas.  
  
 [5] la validation ou restauration a réussi sur la connexion. La fonction retourne SQL_ERROR si la validation ou restauration a échoué sur une autre connexion, ou la fonction retourne SQL_SUCCESS, que si la validation ou restauration a réussi sur toutes les connexions.  
  
 [6] s’est produite au moins une instruction allouée sur la connexion.  
  
 [7] ne comportaient aucuns instructions allouées sur la connexion.  
  
 [8] la connexion a au moins une instruction pour laquelle il a été un curseur ouvert, et la source de données conserve les curseurs lorsque les transactions sont validées ou annulées, selon le cas (selon que *CompletionType* a été SQL_COMMIT ou SQL_ROLLBACK). Pour plus d’informations, voir les attributs SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 [9] si la connexion a toutes les instructions pour lequel ont été les curseurs ouverts, les curseurs non préservées lors de la transaction a été validée ou restaurée.  
  
## <a name="sqlexecdirect-and-sqlexecute"></a>SQLExecDirect et SQLExecute  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--LES C6 C6 [1] [2] [3]|--|  
  
 [1] de la connexion en mode de validation automatique, et l’instruction d’exécution ne était pas un *curseur* *spécification* (par exemple, une instruction SELECT) ; ou la connexion est en mode de validation manuelle et l’instruction d’exécution n’a pas commencé une transaction.  
  
 [2] de la connexion en mode de validation automatique, et l’instruction d’exécution était un *curseur* *spécification* (par exemple, une instruction SELECT).  
  
 [3] de la connexion était en mode de validation manuelle, et la source de données a commencé une transaction.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT) [1]|C0|(HY010)|(HY010)|(HY010)|(HY010)|(HY010)|  
|(INCLUENT) [2]|(INCLUENT)|(C1)|(HY010)|(HY010)|(HY010)|(HY010)|  
|(INCLUENT) [3]|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|C4 [5], [6]|--C4 [7] [5] et [8] C5 [6] et [8]|  
|(INCLUENT) [4]|(INCLUENT)|(INCLUENT)|(INCLUENT)|--|--|--|  
  
 [1] cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_ENV.  
  
 [2] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DBC.  
  
 [3] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_STMT.  
  
 [4] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DESC.  
  
 [5] s’est produite qu’une seule instruction allouée sur la connexion.  
  
 [6] ont été allouées sur la connexion de plusieurs instructions.  
  
 [7] la connexion était en mode de validation manuelle.  
  
 [8] la connexion était en mode de validation automatique.  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT) [1]|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--|C5 [3] : [4]|  
|(INCLUENT) [2]|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--|--|  
  
 [1] cette ligne affiche les transactions lorsque la *Option* argument est SQL_CLOSE.  
  
 [2] de cette ligne affiche les transactions lorsque la *Option* argument est SQL_UNBIND ou SQL_RESET_PARAMS.  
  
 [3] de la connexion est en mode de validation automatique, et aucun curseur ont été ouverts sur toutes les instructions à l’exception de celui-ci.  
  
 [4] de la connexion est en mode de validation manuelle, ou il était en mode de validation automatique et un curseur a été ouvert au moins une autre instruction.  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|INCLUENT|INCLUENT|--[1] 08003[2]|HY010|--|--|--|  
  
 [1] le *attribut* argument a été SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, SQL_ATTR_LOGIN_TIMEOUT, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE ou SQL_ATTR_TRACEFILE, ou une valeur a été définie pour l’attribut de connexion.  
  
 [2] le *attribut* argument n’était pas SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, SQL_ATTR_LOGIN_TIMEOUT, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE ou SQL_ATTR_TRACEFILE, et une valeur n’a pas été définie pour l’attribut de connexion.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField et SQLGetDiagRec  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT) [1]|--|--|--|--|--|--|  
|(INCLUENT) [2]|(INCLUENT)|--|--|--|--|--|  
|(INCLUENT) [3]|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--|--|  
|(INCLUENT) [4]|(INCLUENT)|(INCLUENT)|(INCLUENT)|--|--|--|  
  
 [1] cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_ENV.  
  
 [2] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DBC.  
  
 [3] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_STMT.  
  
 [4] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DESC.  
  
## <a name="sqlgetenvattr"></a>Cas  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|INCLUENT|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|INCLUENT|INCLUENT|HY010|HY010|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|INCLUENT|INCLUENT|--[1] 08003[2]|08003|--|--|--|  
  
 [1] le *InfoType* argument a été SQL_ODBC_VER.  
  
 [2] le *InfoType* argument n’a pas été SQL_ODBC_VER.  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--C6 [1] [2]|--C5 [3] [1]|  
  
 [1] la connexion était en mode de validation automatique et l’appel à **SQLMoreResults** n’a pas initialisé le traitement d’un jeu de résultats d’une spécification de curseur.  
  
 [2] de la connexion est en mode de validation automatique, l’appel à **SQLMoreResults** a initialisé le traitement d’un jeu de résultats d’une spécification de curseur.  
  
 [3] de la connexion était en mode de validation manuelle.  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(08003)|(08003)|--|--|--|  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--C6 [1] [2]|--|  
  
 [1] la connexion était en mode de validation automatique, ou la source de données n’a pas commencé une transaction.  
  
 [2] de la connexion était en mode manuel – commit, et la source de données a commencé une transaction.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|INCLUENT|INCLUENT|--[1] 08003[2]|HY010|--08002 [3] [4] HY011 [5]|--08002 [3] [4] HY011 [5]|: [3] et [6] C5 [8] [4] HY011 de 08002 [5] ou [7]|  
  
 [1] le *attribut* argument n’était pas SQL_ATTR_TRANSLATE_LIB ou SQL_ATTR_TRANSLATE_OPTION.  
  
 [2] le *attribut* argument a été SQL_ATTR_TRANSLATE_LIB ou SQL_ATTR_TRANSLATE_OPTION.  
  
 [3] le *attribut* argument n’était pas SQL_ATTR_ODBC_CURSORS ou SQL_ATTR_PACKET_SIZE.  
  
 [4] le *attribut* SQL_ATTR_ODBC_CURSORS a été l’argument.  
  
 [5] le *attribut* argument a été SQL_ATTR_PACKET_SIZE.  
  
 [6] le *attribut* argument n’a pas été SQL_ATTR_AUTOCOMMIT, ou le *attribut* argument était SQL_ATTR_AUTOCOMMIT et si cet attribut n’a pas de validé la transaction.  
  
 [7] le *attribut* argument était SQL_ATTR_TXN_ISOLATION.  
  
 [8] le *attribut* argument était SQL_ATTR_AUTOCOMMIT, et si cet attribut a validé la transaction.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|--|--|(HY010)|--|--|--|  
  
## <a name="all-other-odbc-functions"></a>Toutes les autres fonctions ODBC  
  
|C0<br /><br /> Aucun Env.|C1<br /><br /> Non alloué|C2<br /><br /> Alloué|C3<br /><br /> Besoin de données|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|(INCLUENT)|--|--|
