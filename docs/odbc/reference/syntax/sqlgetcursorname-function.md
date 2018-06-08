---
title: Fonction SQLGetCursorName | Documents Microsoft
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
- SQLGetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetCursorName
helpviewer_keywords:
- SQLGetCursorName function [ODBC]
ms.assetid: e6e92199-7bb6-447c-8987-049a4c6ce05d
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 262eee1b795907c7c429443ae85a25485f257ed5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetcursorname-function"></a>SQLGetCursorName Function
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLGetCursorName** retourne le nom de curseur associé à une instruction spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLGetCursorName(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CursorName,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   NameLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *CursorName*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nom du curseur.  
  
 Si *CursorName* est NULL, *NameLengthPtr* retourne toujours le nombre total de caractères (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *CursorName*.  
  
 *BufferLength*  
 [Entrée] Longueur de \* *CursorName*, en caractères. Si la valeur de  *\*CursorName* est une chaîne Unicode (lors de l’appel **SQLGetCursorNameW**), la *BufferLength* l’argument doit être un nombre pair.  
  
 *NameLengthPtr*  
 [Sortie] Pointeur vers la mémoire dans lequel retourner le nombre total de caractères (sans compter le caractère de fin de la valeur null) disponibles à renvoyer dans \* *CursorName*. Si le nombre de caractères à retourner est supérieur ou égal à *BufferLength*, le nom du curseur dans \* *CursorName* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetCursorName** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLGetCursorName** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|La mémoire tampon \* *CursorName* n’est pas suffisamment grande pour retourner le nom de la totalité du curseur, donc le nom de curseur a été tronqué. La longueur du nom du curseur non tronqué est retournée dans **NameLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLGetCursorName** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY015|Aucun nom de curseur disponible|(DM) le pilote a été un ODBC 2 *.x* pilote, il n’y a aucun curseur ouvert sur l’instruction, et aucun nom de curseur n’a été défini avec **SQLSetCursorName**.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur spécifiée dans l’argument *BufferLength* était inférieure à 0.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Les noms de curseurs sont utilisés uniquement dans la mise à jour positionnée et supprimer des instructions (par exemple, **mettre à jour** *-nom de la table* ... **WHERE CURRENT OF** *nom_curseur*). Pour plus d’informations, consultez [positionné instructions Update et Delete](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md). Si l’application n’appelle pas **SQLSetCursorName** pour définir un nom de curseur, le pilote génère un nom. Ce nom commence par les lettres SQL_CUR.  
  
> [!NOTE]  
>  Dans ODBC 2 *.x*, lorsqu’il n’y a aucun curseur ouvert et aucun nom n’a été défini par un appel à **SQLSetCursorName**, un appel à **SQLGetCursorName** retourné SQLSTATE HY015 (aucun nom de curseur disponible). Dans ODBC 3 *.x*, cela n’est plus true ; indépendamment du moment **SQLGetCursorName** est appelée, le pilote retourne le nom du curseur.  
  
 **SQLGetCursorName** retourne le nom d’un curseur ou non le nom a été créé, explicitement ou implicitement. Un nom de curseur est généré implicitement si **SQLSetCursorName** n’est pas appelée. **SQLSetCursorName** peut être appelée pour renommer un curseur sur une instruction tant que le curseur se trouve dans un état alloué ou préparé.  
  
 Un nom de curseur qui est défini explicitement ou implicitement reste jusqu'à ce que le *au paramètre StatementHandle* auquel il est associé est supprimé, à l’aide de **SQLFreeHandle** avec un *HandleType* de SQL_HANDLE_STMT.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Préparation de l’exécution d’une instruction|[SQLPrepare, fonction](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
