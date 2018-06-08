---
title: Les champs et les enregistrements de diagnostic | Documents Microsoft
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
- header records [ODBC]
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], diagnostic records
- ODBC error handling, diagnostic records
- SQLGetDiagField function
- diagnostic records [ODBC]
- errors [ODBC], diagnostic records
- fields [ODBC]
- status information [ODBC]
ms.assetid: 4949530c-62d1-4f1a-b592-144244444ce0
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d9dfbebe695b3c85631ba8e3c44d9a8fe404da37
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="diagnostic-records-and-fields"></a>Enregistrements et champs de diagnostic
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Les enregistrements de diagnostic sont associés aux handles d'environnement, de connexion, d'instruction ou de descripteur ODBC. Lorsqu'une fonction ODBC déclenche un code de retour autre que SQL_SUCCESS ou SQL_INVALID_HANDLE, le handle appelé par la fonction est associé à des enregistrements de diagnostic qui contiennent des messages informationnels ou d'erreur. Ces enregistrements sont conservés jusqu'à ce qu'une autre fonction soit appelée à l'aide de ce handle, après quoi les enregistrements sont ignorés. Le nombre d'enregistrements de diagnostic pouvant être associés à un handle à un instant donné n'est pas limité.  
  
 Les enregistrements de diagnostic sont de deux types : en-tête et état. L'enregistrement d'en-tête est l'enregistrement 0 ; lorsque des enregistrements d'état sont présents, ils prennent les valeurs 1 et supérieures. Les enregistrements de diagnostic contiennent des champs différents pour l'enregistrement d'en-tête et les enregistrements d'état. Des composants ODBC peuvent également définir leurs propres champs d'enregistrement de diagnostic.  
  
 Les champs dans l'enregistrement d'en-tête contiennent des informations générales sur l'exécution d'une fonction, y compris le code de retour, le nombre de lignes, le nombre d'enregistrements d'état et le type d'instruction exécutée. L'enregistrement d'en-tête est toujours créé à moins qu'une fonction ODBC ne retourne SQL_INVALID_HANDLE. Pour obtenir une liste complète des champs de l’enregistrement d’en-tête, consultez [SQLGetDiagField](../../relational-databases/native-client-odbc-api/sqlgetdiagfield.md).  
  
 Les champs dans les enregistrements d'état contiennent des informations sur des erreurs ou des avertissements spécifiques retournés par le gestionnaire de pilotes, le pilote ou la source de données ODBC, y compris la valeur SQLSTATE, le numéro d'erreur native, le message de diagnostic, le numéro de colonne et le numéro de ligne. Les enregistrements d'état sont créés uniquement si la fonction retourne SQL_ERROR, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_NEED_DATA ou SQL_STILL_EXECUTING. Pour obtenir une liste complète des champs dans les enregistrements d’état, consultez **SQLGetDiagField**.  
  
 **SQLGetDiagRec** récupère un seul enregistrement de diagnostic, ainsi que ses ODBC SQLSTATE, numéro d’erreur natif et des champs de message de diagnostic. Cette fonctionnalité est similaire à l’API ODBC 2. *x *** SQLError** fonction. La fonction de gestion des erreurs la plus simple dans ODBC 3. *x* consiste à appeler à plusieurs reprises **SQLGetDiagRec** en commençant par le *RecNumber* paramètre défini sur 1 et en incrémentant *RecNumber* de 1 jusqu'à ce que **SQLGetDiagRec** retourne SQL_NO_DATA. Cela équivaut à une API ODBC 2. *x* application appelant **SQLError** jusqu'à ce qu’elle retourne SQL_NO_DATA_FOUND.  
  
 ODBC 3. *x* prend en charge beaucoup plus d’informations de diagnostic qu’ODBC 2. *x*. Ces informations sont stockées dans des champs supplémentaires dans les enregistrements de diagnostic récupérés à l’aide de **SQLGetDiagField**.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client a des champs de diagnostic spécifiques au pilote qui peuvent être récupérés avec **SQLGetDiagField**. Les étiquettes pour ces champs spécifiques au pilote sont définies dans sqlncli.h. Utilisez ces étiquettes pour récupérer l'état [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le niveau de gravité, le nom du serveur, le nom de la procédure et le numéro de ligne associés à chaque enregistrement de diagnostic. En outre, sqlncli.h contient les définitions des codes que le pilote utilise pour identifier les instructions Transact-SQL si une application appelle **SQLGetDiagField** avec *DiagIdentifier* ayant pour valeur SQL_DIAG_DYNAMIC_FUNCTION_CODE.  
  
 **SQLGetDiagField** est traité par le Gestionnaire de pilotes ODBC à l’aide des informations sur l’erreur Il met en cache à partir du pilote sous-jacent. Le gestionnaire de pilotes ODBC ne met pas en cache les champs de diagnostic spécifiques aux pilotes tant qu'une connexion n'a pas été établie avec succès. **SQLGetDiagField** retourne SQL_ERROR si elle est appelée pour obtenir les champs de diagnostic spécifiques au pilote avant une connexion réussie a été effectuée. Si une fonction de connexion ODBC retourne SQL_SUCCESS_WITH_INFO, les champs de diagnostic spécifiques au pilote pour la fonction de connexion ne sont pas encore disponibles. Vous pouvez commencer à appeler **SQLGetDiagField** pour les champs de diagnostic spécifiques au pilote uniquement après avoir apporté ODBC un autre appel de fonction après la fonction de connexion.  
  
 La plupart des erreurs signalées par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client peut être diagnostiqué efficacement à l’aide uniquement les informations retournées par **SQLGetDiagRec**. Toutefois, dans certains cas, les informations retournées par les champs de diagnostic spécifiques au pilote sont importantes pour diagnostiquer une erreur. Lors du codage d’un gestionnaire d’erreurs ODBC pour les applications utilisant le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client, il est judicieux d’utiliser également **SQLGetDiagField** pour récupérer au moins les champs spécifiques au pilote SQL_DIAG_SS_MSGSTATE et SQL_DIAG_SS_SEVERITY. Si une erreur particulière peut être déclenchée à plusieurs endroits dans le code [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], SQL_DIAG_SS_MSGSTATE indique à un ingénieur du support technique Microsoft l'emplacement précis où une erreur a été déclenchée, ce qui peut parfois s'avérer utile pour diagnostiquer un problème.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des erreurs et des messages](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  
