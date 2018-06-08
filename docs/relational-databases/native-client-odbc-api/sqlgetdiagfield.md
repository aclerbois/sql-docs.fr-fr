---
title: SQLGetDiagField | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
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
- SQLGetDiagField function
ms.assetid: 395245ba-0372-43ec-b9a4-a29410d85a6d
caps.latest.revision: 36
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ee119689f7bf66cde7f2276372185cb42907274e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetdiagfield"></a>SQLGetDiagField
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client spécifie les champs de diagnostic supplémentaires suivants pour **SQLGetDiagField**. Ces champs prennent en charge la création de rapports d'erreurs riches pour les applications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et sont disponibles dans tous les enregistrements de diagnostic générés sur les handles de connexion ODBC et les handles d'instructions ODBC connectés. Les champs sont définis dans sqlncli.h.  
  
|Champ d'enregistrement de diagnostic| Description|  
|------------------------------|-----------------|  
|SQL_DIAG_SS_LINE|Signale le numéro de ligne d'une procédure stockée qui génère une erreur. La valeur de SQL_DIAG_SS_LINE est explicite uniquement si SQL_DIAG_SS_PROCNAME retourne une valeur. La valeur est retournée en tant qu'entier 16 bits non signé.|  
|SQL_DIAG_SS_MSGSTATE|État d'un message d'erreur. Pour plus d’informations sur l’état de message d’erreur, consultez [RAISERROR](../../t-sql/language-elements/raiserror-transact-sql.md). La valeur est retournée en tant qu'entier 32 bits signé.|  
|SQL_DIAG_SS_PROCNAME|Nom de la procédure stockée qui génère une erreur, le cas échéant. La valeur est retournée en tant que chaîne de caractères. La longueur de la chaîne, en caractères, dépend de la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il peut être déterminé en appelant [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md) qui demande la valeur pour SQL_MAX_PROCEDURE_NAME_LEN.|  
|SQL_DIAG_SS_SEVERITY|Niveau de gravité du message d'erreur associé. La valeur est retournée en tant qu'entier 32 bits signé.|  
|SQL_DIAG_SS_SRVNAME|Nom du serveur sur lequel l'erreur s'est produite. La valeur est retournée en tant que chaîne de caractères. La longueur de la chaîne (en caractères) est définie par la macro SQL_MAX_SQLSERVERNAME dans sqlncli.h.|  
  
 Les champs de diagnostic spécifiques à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contiennent des données caractères, SQL_DIAG_SS_PROCNAME et SQL_DIAG_SS_SRVNAME, retournent ces données au client en tant que chaînes terminées par le caractère NULL, ANSI ou Unicode. Si nécessaire, le nombre de caractères doit être ajusté par la largeur des caractères. Vous pouvez aussi utiliser un type de données C portable tel que TCHAR ou SQLTCHAR pour garantir une longueur variable de programme correcte.  
  
 Le pilote ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client signale les codes de fonction dynamique supplémentaires suivants qui identifient la dernière instruction [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tentée. Le code de fonction dynamique est retourné dans l'en-tête (enregistrement 0) du jeu d'enregistrements de diagnostic. Il est par conséquent disponible sur chaque exécution (réussi ou pas).  
  
|Code de fonction dynamique|Source|  
|---------------------------|------------|  
|SQL_DIAG_DFC_SS_ALTER_DATABASE|ALTER DATABASE, instruction|  
|SQL_DIAG_DFC_SS_CHECKPOINT|CHECKPOINT (instruction)|  
|SQL_DIAG_DFC_SS_CONDITION|Une erreur est survenue dans les clauses WHERE ou HAVING d'une instruction.|  
|SQL_DIAG_DFC_SS_CREATE_DATABASE|CREATE DATABASE, instruction|  
|SQL_DIAG_DFC_SS_CREATE_DEFAULT|CREATE DEFAULT (instruction)|  
|SQL_DIAG_DFC_SS_CREATE_PROCEDURE|CREATE PROCEDURE (instruction)|  
|SQL_DIAG_DFC_SS_CREATE_RULE|CREATE RULE (instruction)|  
|SQL_DIAG_DFC_SS_CREATE_TRIGGER|CREATE TRIGGER (instruction)|  
|SQL_DIAG_DFC_SS_CURSOR_DECLARE|DECLARE CURSOR (instruction)|  
|SQL_DIAG_DFC_SS_CURSOR_OPEN|OPEN (instruction)|  
|SQL_DIAG_DFC_SS_CURSOR_FETCH|FETCH (instruction)|  
|SQL_DIAG_DFC_SS_CURSOR_CLOSE|CLOSE (instruction)|  
|SQL_DIAG_DFC_SS_DEALLOCATE_CURSOR|DEALLOCATE (instruction)|  
|SQL_DIAG_DFC_SS_DBCC|DBCC (instruction)|  
|SQL_DIAG_DFC_SS_DENY|DENY (instruction)|  
|SQL_DIAG_DFC_SS_DROP_DATABASE|DROP DATABASE (instruction)|  
|SQL_DIAG_DFC_SS_DROP_DEFAULT|DROP DEFAULT (instruction)|  
|SQL_DIAG_DFC_SS_DROP_PROCEDURE|DROP PROCEDURE (instruction)|  
|SQL_DIAG_DFC_SS_DROP_RULE|DROP RULE (instruction)|  
|SQL_DIAG_DFC_SS_DROP_TRIGGER|DROP TRIGGER (instruction)|  
|SQL_DIAG_DFC_SS_DUMP_DATABASE|BACKUP ou DUMP DATABASE (instruction)|  
|SQL_DIAG_DFC_SS_DUMP_TABLE|DUMP TABLE (instruction)|  
|SQL_DIAG_DFC_SS_DUMP_TRANSACTION|BACKUP ou DUMP TRANSACTION (instruction) Également retourné pour une instruction de point de contrôle si le **trunc. log sur chkpt.** option de base de données est activée.|  
|SQL_DIAG_DFC_SS_GOTO|Instruction de contrôle de flux GOTO|  
|SQL_DIAG_DFC_SS_INSERT_BULK|INSERT BULK (instruction)|  
|SQL_DIAG_DFC_SS_KILL|KILL (instruction)|  
|SQL_DIAG_DFC_SS_LOAD_DATABASE|LOAD ou RESTORE DATABASE (instruction)|  
|SQL_DIAG_DFC_SS_LOAD_HEADERONLY|LOAD ou RESTORE HEADERONLY (instruction)|  
|SQL_DIAG_DFC_SS_LOAD_TABLE|LOAD TABLE (instruction)|  
|SQL_DIAG_DFC_SS_LOAD_TRANSACTION|LOAD ou RESTORE TRANSACTION (instruction)|  
|SQL_DIAG_DFC_SS_PRINT|PRINT (instruction)|  
|SQL_DIAG_DFC_SS_RAISERROR|RAISERROR (instruction)|  
|SQL_DIAG_DFC_SS_READTEXT|READTEXT (instruction)|  
|SQL_DIAG_DFC_SS_RECONFIGURE|RECONFIGURE (instruction)|  
|SQL_DIAG_DFC_SS_RETURN|Instruction de contrôle de flux RETURN|  
|SQL_DIAG_DFC_SS_SELECT_INTO|SELECT INTO, instruction|  
|SQL_DIAG_DFC_SS_SET|SET (instruction) (générique, toutes les options)|  
|SQL_DIAG_DFC_SS_SET_IDENTITY_INSERT|SET IDENTITY_INSERT (instruction)|  
|SQL_DIAG_DFC_SS_SET_ROW_COUNT|SET ROWCOUNT (instruction)|  
|SQL_DIAG_DFC_SS_SET_STATISTICS|SET STATISTICS IO ou SET STATISTICS TIME (instructions)|  
|SQL_DIAG_DFC_SS_SET_TEXTSIZE|SET TEXTSIZE (instruction)|  
|SQL_DIAG_DFC_SS_SETUSER|SETUSER (instruction)|  
|SQL_DIAG_DFC_SS_SET_XCTLVL|SET TRANSACTION ISOLATION LEVEL (instruction)|  
|SQL_DIAG_DFC_SS_SHUTDOWN|SHUTDOWN (instruction)|  
|SQL_DIAG_DFC_SS_TRANS_BEGIN|BEGIN TRAN (instruction)|  
|SQL_DIAG_DFC_SS_TRANS_COMMIT|COMMIT TRAN (instruction)|  
|SQL_DIAG_DFC_SS_TRANS_PREPARE|Préparation de la validation d'une transaction distribuée|  
|SQL_DIAG_DFC_SS_TRANS_ROLLBACK|ROLLBACK TRAN (instruction)|  
|SQL_DIAG_DFC_SS_TRANS_SAVE|SAVE TRAN (instruction)|  
|SQL_DIAG_DFC_SS_TRUNCATE_TABLE|TRUNCATE TABLE (instruction)|  
|SQL_DIAG_DFC_SS_UPDATE_STATISTICS|UPDATE STATISTICS (instruction)|  
|SQL_DIAG_DFC_SS_UPDATETEXT|UPDATETEXT (instruction)|  
|SQL_DIAG_DFC_SS_USE|USE (instruction)|  
|SQL_DIAG_DFC_SS_WAITFOR|Instruction de contrôle de flux WAITFOR|  
|SQL_DIAG_DFC_SS_WRITETEXT|WRITETEXT (instruction)|  
  
## <a name="sqlgetdiagfield-and-table-valued-parameters"></a>SQLGetDiagField et paramètres table  
 SQLGetDiagField peut être utilisé pour récupérer les deux champs de diagnostic : SQL_DIAG_SS_TABLE_COLUMN_NUMBER et SQL_DIAG_SS_TABLE_ROW_NUMBER. Ces champs vous aident à déterminer la valeur ayant provoqué l'erreur ou l'avertissement associé à l'enregistrement de diagnostic.  
  
 Pour plus d’informations sur les paramètres table, consultez [paramètres table & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLGetDiagField, fonction](http://go.microsoft.com/fwlink/?LinkId=59352)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
