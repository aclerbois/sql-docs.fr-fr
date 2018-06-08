---
title: Gestion des erreurs et Messages | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-error-messages
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- ODBC error handling, about error handling
- errors [ODBC]
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], about messages
- ODBC error handling
- SQL_INVALID_HANDLE return code
- errors [ODBC], about error handling
- messages [ODBC]
ms.assetid: 74ea9630-e482-4a46-bb45-f5234f079b48
caps.latest.revision: 34
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 640e21cdf83375d603fca4d40d1f0fb6fd7e01ce
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708207"
---
# <a name="handling-errors-and-messages"></a>Gestion des erreurs et des messages
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Lorsqu'une application appelle une fonction ODBC, le pilote exécute la fonction et retourne les informations de diagnostic de deux façons : un code de retour indique le succès ou l'échec total d'une fonction ODBC et les enregistrements de diagnostic fournissent des informations détaillées sur la fonction. Les enregistrements de diagnostic comportent un enregistrement d'en-tête et des enregistrements d'état. Au moins un enregistrement de diagnostic, l'enregistrement d'en-tête, est retourné même si la fonction réussit.  
  
 Les informations de diagnostic sont utilisées au moment du développement pour intercepter les erreurs de programmation, telles que les handles non valides et les erreurs de syntaxe dans les instructions SQL codées de manière irréversible. Elles sont également utilisées au moment de l'exécution pour intercepter les avertissements et les erreurs d'exécution, tels que la troncation de données, les violations de règle et les erreurs de syntaxe des instructions SQL saisies par l'utilisateur. La logique du programme repose en général sur des codes de retour.  
  
 Par exemple, une fois une application appelle **SQLFetch** pour extraire les lignes dans un jeu de résultats, le code de retour indique si la fin du jeu de résultats a été atteinte (SQL_NO_DATA), si des messages d’information ont été retournés (SQL_SUCCESS_WITH_INFO) ou si une erreur s’est produite (SQL_ERROR).  
  
 Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client retourne la valeur autre que SQL_SUCCESS, l’application peut appeler **SQLGetDiagRec** permet de récupérer d’information ou de messages d’erreur. Utilisez **SQLGetDiagRec** défilement haut et bas du message défini s’il existe plusieurs messages.  
  
 Le code de retour SQL_INVALID_HANDLE indique toujours une erreur de programmation et ne doit jamais se rencontrer au moment de l'exécution. Tous les autres codes de retour fournissent des informations d'exécution, bien que SQL_ERROR puisse indiquer une erreur de programmation.  
  
 La version d’origine [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] API native, DB-Library for C, permet à une application installer la gestion des erreurs de rappel et les fonctions de gestion des messages que retournent des erreurs ou les messages. Certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], telles que PRINT, RAISERROR, DBCC et SET, retournent leurs résultats à la fonction gestionnaire des messages de DB-Library et non dans un jeu de résultats. Toutefois, l'API ODBC n'a aucune fonction de rappel similaire. Lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client détecte les messages provenant de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il définit le code de retour ODBC SQL_SUCCESS_WITH_INFO ou SQL_ERROR et retourne le message à une ou plusieurs enregistrements de diagnostic. Par conséquent, une application ODBC doit tester avec soin ces codes de retour et appel **SQLGetDiagRec** pour récupérer des données de message.  
  
 Pour plus d’informations sur le suivi des erreurs, consultez [suivi d’accès aux données](http://go.microsoft.com/fwlink/?LinkId=125805). Pour plus d’informations sur les améliorations apportées au suivi d’erreur ajouté dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], consultez [l’accès à des informations de Diagnostic dans le journal des événements étendus](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Traitement des instructions qui génèrent des messages](../../relational-databases/native-client-odbc-error-messages/processing-statements-that-generate-messages.md)  
  
-   [Enregistrements et champs de diagnostic](../../relational-databases/native-client-odbc-error-messages/diagnostic-records-and-fields.md)  
  
-   [Numéros des erreurs natives](../../relational-databases/native-client-odbc-error-messages/native-error-numbers.md)  
  
-   [SQLSTATE &#40;Codes d’erreur ODBC&#41;](../../relational-databases/native-client-odbc-error-messages/sqlstate-odbc-error-codes.md)  
  
-   [Messages d’erreur](../../relational-databases/native-client-odbc-error-messages/error-messages.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  
