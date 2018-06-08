---
title: Fonction SQLSetCursorName | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLSetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetCursorName
helpviewer_keywords:
- SQLSetCursorName function [ODBC]
ms.assetid: 4e055946-12d4-4589-9891-41617a50f34e
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 64c560f2934d04b098ed26c347d2819d90948e5f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetcursorname-function"></a>Fonction SQLSetCursorName
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLSetCursorName** associe un nom de curseur d’une instruction active. Si une application n’appelle pas **SQLSetCursorName**, le pilote génère des noms de curseur en fonction des besoins de traitement des instructions SQL.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLSetCursorName(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CursorName,  
     SQLSMALLINT   NameLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *CursorName*  
 [Entrée] Nom du curseur. Pour un traitement efficace, le nom du curseur ne doit pas inclure de d’espaces de début ou de fin dans le nom du curseur, et si le nom du curseur inclut un identificateur délimité, le délimiteur doit être positionné en tant que le premier caractère dans le nom du curseur.  
  
 *Longueur de nom*  
 [Entrée] Longueur en caractères de **CursorName*.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLSetCursorName** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLSetCursorName** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Le nom du curseur a dépassé la limite maximale, ainsi seulement le nombre maximal autorisé de caractères a été utilisé.|  
|24000|État de curseur non valide|L’instruction correspondant à *au paramètre StatementHandle* était déjà dans un état exécuté ou curseur positionné.|  
|34000|Nom de curseur non valide|Le nom du curseur spécifié dans **CursorName* n’était pas valide, car il dépasse la longueur maximale, tel que défini par le pilote, ou il a démarré avec « SQLCUR » ou « SQL_CUR ».|  
|3C000|Nom de curseur en double|Le nom du curseur spécifié dans **CursorName* existe déjà.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) l’argument *CursorName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction aynchronous toujours en cours d’exécution lorsque le **SQLSetCursorName** fonction a été appelée.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) l’argument *longueur de nom* était inférieur à 0, mais pas égale à SQL_NTS.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Les noms de curseurs sont utilisés uniquement dans la mise à jour positionnée et supprimer des instructions (par exemple, **mettre à jour** *-nom de la table* ... **WHERE CURRENT OF** *nom_curseur*). Pour plus d’informations, consultez [positionné instructions Update et Delete](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md). Si l’application n’appelle pas **SQLSetCursorName** pour définir un nom de curseur, lors de l’exécution d’une instruction de requête que le pilote génère un nom qui commence par les lettres SQL_CUR et ne dépasse pas de 18 caractères.  
  
 Tous les noms de curseur dans la connexion doivent être uniques. La longueur maximale d’un nom de curseur est définie par le pilote. Pour une interopérabilité maximale, il est recommandé que les applications limitent les noms de curseur pas plus de 18 caractères. Dans ODBC 3 *.x*, si un nom de curseur est un identificateur entre guillemets, il est traité de la casse et il peut contenir des caractères que la syntaxe SQL n’autorise pas la traitait spécialement, tels que des espaces ou des mots clés réservés. Si un nom de curseur doit être traité en respectant la casse, il doit être passée comme un identificateur entre guillemets.  
  
 Un nom de curseur qui est soit défini explicitement ou implicitement reste défini jusqu'à ce que l’instruction auquel il est associé est supprimée, à l’aide de **SQLFreeHandle**. **SQLSetCursorName** peut être appelée pour renommer un curseur sur une instruction tant que le curseur se trouve dans un état alloué ou préparé.  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application utilise **SQLSetCursorName** pour définir un nom de curseur pour une instruction. Il utilise ensuite cette instruction pour récupérer les résultats de la table CUSTOMERS. Enfin, il effectue une mise à jour positionnée pour modifier le numéro de téléphone de John Smith. Notez que l’application utilise des descripteurs d’instruction différent pour le **sélectionnez** et **mise à jour** instructions.  
  
 Pour un autre exemple de code, consultez [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```  
#define NAME_LEN 50  
#define PHONE_LEN 10  
  
SQLHSTMT     hstmtSelect,  
SQLHSTMT     hstmtUpdate;  
SQLRETURN    retcode;  
SQLHDBC      hdbc;  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   cbName, cbPhone;  
  
/* Allocate the statements and set the cursor name. */  
  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtSelect);  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtUpdate);  
SQLSetCursorName(hstmtSelect, "C1", SQL_NTS);  
  
/* SELECT the result set and bind its columns to local buffers. */  
  
SQLExecDirect(hstmtSelect,  
      "SELECT NAME, PHONE FROM CUSTOMERS",  
      SQL_NTS);  
SQLBindCol(hstmtSelect, 1, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
SQLBindCol(hstmtSelect, 2, SQL_C_CHAR, szPhone, PHONE_LEN, &cbPhone);  
  
/* Read through the result set until the cursor is */  
/* positioned on the row for John Smith. */  
  
do  
 retcode = SQLFetch(hstmtSelect);  
while ((retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) &&  
   (strcmp(szName, "Smith, John") != 0));  
  
/* Perform a positioned update of John Smith's name. */  
  
if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
   SQLExecDirect(hstmtUpdate,  
   "UPDATE EMPLOYEE SET PHONE=\"2064890154\" WHERE CURRENT OF C1",  
   SQL_NTS);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Renvoi d’un nom de curseur|[SQLGetCursorName, fonction](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|Définition des options de défilement de curseur|[SQLSetScrollOptions, fonction](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
