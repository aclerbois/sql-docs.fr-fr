---
title: Résumé de la fonction ODBC | Documents Microsoft
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
- functions [ODBC], listed by task
ms.assetid: 7aa635da-e6b7-439f-8e9b-c3860e24de5e
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bc994a3e3ec3e0e41a7b18958c013f1876f38c86
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-function-summary"></a>Résumé de la fonction ODBC
Le tableau suivant répertorie les fonctions ODBC, regroupées par type de tâche et inclut la désignation de conformité et une brève description de l’objectif de chaque fonction. Pour plus d’informations sur les désignations de conformité, consultez [ODBC et l’interface CLI Standard](../../../odbc/reference/odbc-and-the-standard-cli.md). Pour plus d’informations sur la syntaxe et la sémantique pour chaque fonction, consultez [référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Une application peut appeler le **SQLGetInfo** pour obtenir des informations de conformité sur un pilote de fonction. Pour obtenir plus d’informations sur la prise en charge pour une fonction spécifique dans un pilote, une application peut appeler **SQLGetFunctions**.  
  
|Tâche|Nom de la fonction|Mise en conformité|Fonction|  
|----------|-------------------|-----------------|-------------|  
|Connexion à une source de données|[SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|ISO 92|Obtient un handle d’environnement, connexion, instruction ou descripteur.|  
||[SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|ISO 92|Se connecte à un pilote spécifique par nom de source de données, ID d’utilisateur et mot de passe.|  
||[SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|ODBC|Se connecte à un pilote spécifique les demandes que le Gestionnaire de pilotes et le pilote affichent des boîtes de dialogue de connexion pour l’utilisateur ou de la chaîne de connexion.|  
||[SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|ODBC|Retourne les niveaux successifs des attributs de connexion et des valeurs d’attribut valide. Lorsqu’une valeur a été spécifiée pour chaque attribut de connexion, se connecte à la source de données.|  
|Obtention d’informations sur un pilote et de la source de données|[SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)<br /><br /> [SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|ISO 92<br /><br /> ODBC|Retourne la liste des sources de données disponibles.<br /><br /> Retourne la liste des pilotes installés et leurs attributs.|  
||[SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|ISO 92|Retourne des informations sur une source de données et de pilote spécifique.|  
||[SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md)|ISO 92|Retourne pris en charge les fonctions de pilote.|  
||[SQLGetTypeInfo](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)|ISO 92|Retourne des informations sur les types de données pris en charge.|  
|Définir et récupérer des attributs de pilote|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)<br /><br /> [SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|ISO 92<br /><br /> ISO 92|Définit un attribut de connexion.<br /><br /> Retourne la valeur d’un attribut de connexion.|  
||[SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|ISO 92|Définit un attribut de l’environnement.|  
||[Cas](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|ISO 92|Retourne la valeur d’un attribut de l’environnement.|  
||[SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|ISO 92|Définit un attribut d’instruction.|  
||[SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|ISO 92|Retourne la valeur d’un attribut d’instruction.|  
|Définir et récupérer des champs de descripteur|[SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)<br /><br /> [SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|ISO 92<br /><br /> ISO 92|Retourne la valeur d’un champ de descripteur unique.<br /><br /> Retourne les valeurs de plusieurs champs de descripteur.|  
||[SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|ISO 92|Définit un champ de descripteur unique.|  
||[SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|ISO 92|Définit plusieurs champs de descripteur.|  
||[SQLCopyDesc](../../../odbc/reference/syntax/sqlcopydesc-function.md)|ISO 92|Copie les informations de descripteur à partir d’un descripteur handle vers un autre.|  
|Demande de préparation de SQL|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|ISO 92|Prépare une instruction SQL pour une exécution ultérieure.|  
||[SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|ODBC|Affecte le stockage pour un paramètre dans une instruction SQL.|  
||[SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|ISO 92|Retourne le nom de curseur associé à un descripteur d’instruction.|  
||[SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|ISO 92|Spécifie un nom de curseur.|  
||[SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)|ODBC|Définit les options qui contrôlent le comportement du curseur.|  
|L’envoi de demandes|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)<br /><br /> [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|ISO 92<br /><br /> ISO 92|Exécute une instruction préparée.<br /><br /> Exécute une instruction.|  
||[SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md)|ODBC|Retourne le texte d’une instruction SQL, traduites par le pilote.|  
||[SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|ODBC|Retourne la description pour un paramètre spécifique dans une instruction.|  
||[SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)|ISO 92|Retourne le nombre de paramètres dans une instruction.|  
||[SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|ISO 92|Utilisée conjointement avec **SQLPutData** pour fournir des données de paramètre au moment de l’exécution. (Utile pour les valeurs de données de type long).|  
||[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|ISO 92|Envoie tout ou partie d’une valeur de données pour un paramètre. (Utile pour les valeurs de données de type long).|  
|Lors de la récupération des résultats et des informations sur les résultats|[SQLRowCount](../../../odbc/reference/syntax/sqlrowcount-function.md)<br /><br /> [SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|ISO 92<br /><br /> ISO 92|Retourne le nombre de lignes affectées par une insertion, mise à jour ou demande de suppression.<br /><br /> Retourne le nombre de colonnes dans le jeu de résultats.|  
||[SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|ISO 92|Décrit une colonne dans le jeu de résultats.|  
||[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|ISO 92|Décrit les attributs d’une colonne dans le jeu de résultats.|  
||[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|ISO 92|Affecte le stockage pour une colonne de résultats et spécifie le type de données.|  
||[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|ISO 92|Retourne plusieurs lignes de résultat.|  
||[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|ISO 92|Retourne les lignes de résultats déroulables.|  
||[SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|ISO 92|Retourne ou partie d’une colonne d’une ligne d’un résultat défini. (Utile pour les valeurs de données de type long).|  
||[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|ODBC|Place un curseur dans un bloc d’extraction de données et permet à une application pour actualiser les données dans l’ensemble de lignes ou de mettre à jour ou supprimer des données dans le jeu de résultats.|  
||[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|ODBC|Effectue les insertions en bloc et signet du bloc opérations, y compris la mise à jour, supprimer et à extraire par le signet.|  
||[SQLMoreResults](../../../odbc/reference/syntax/sqlmoreresults-function.md)|ODBC|Détermine s’il existe plusieurs résultats jeux disponibles et, dans ce cas, initialise le traitement pour le jeu de résultats suivant.|  
||[SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|ISO 92|Retourne des informations de diagnostic supplémentaires (un seul champ de la structure de données de diagnostic).|  
||[SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|ISO 92|Retourne des informations de diagnostic supplémentaires (plusieurs champs de la structure de données de diagnostic).|  
|Obtention d’informations sur les tables système de la source de données (fonctions de catalogue)|[SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)<br /><br /> [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|ODBC<br /><br /> Ouvrir le groupe|Retourne une liste de colonnes et les privilèges associés pour une ou plusieurs tables.<br /><br /> Retourne la liste des noms de colonnes dans les tables spécifiées.|  
||[SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|ODBC|Retourne une liste de noms de colonnes qui composent les clés étrangères, s’ils existent pour une table spécifiée.|  
||[SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|ODBC|Retourne la liste des noms de colonnes qui composent la clé primaire pour une table.|  
||[SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)|ODBC|Retourne la liste d’entrée et les paramètres de sortie, ainsi que les colonnes qui composent le jeu de résultats pour les procédures spécifiées.|  
||[SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)|ODBC|Retourne la liste des noms de procédures stockées dans une source de données spécifique.|  
||[SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|Ouvrir le groupe|Retourne des informations sur l’ensemble optimal de colonnes qui identifie de façon unique une ligne dans une table spécifiée ou les colonnes qui sont automatiquement mis à jour lorsqu’une valeur dans la ligne est mise à jour par une transaction.|  
||[SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|ISO 92|Retourne des statistiques sur une seule table et de la liste des index associés à la table.|  
||[SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|ODBC|Retourne une liste de tables et les privilèges associés à chaque table.|  
||[SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|Ouvrir le groupe|Retourne la liste des noms de tables stockées dans une source de données spécifique.|  
|Fin d’une instruction|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|ISO 92|Termine le traitement d’instruction, ignore les résultats en attente et, éventuellement, libère toutes les ressources associées au handle d’instruction.|  
||[SQLCloseCursor](../../../odbc/reference/syntax/sqlclosecursor-function.md)|ISO 92|Ferme un curseur qui a été ouverte sur un descripteur d’instruction.|  
||[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|ISO 92|Annule le traitement sur une instruction.|  
||[SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|ODBC|Annule le traitement sur une instruction ou une connexion.|  
||[SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|ISO 92|Valide ou annule une transaction.|  
|Arrêt d’une connexion|[SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)<br /><br /> [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|ISO 92<br /><br /> ISO 92|Ferme la connexion.<br /><br /> Libère un handle d’environnement, connexion, instruction ou descripteur.|
