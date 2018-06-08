---
title: Fonction de SQLExtendedFetch | Documents Microsoft
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
- SQLExtendedFetch
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLExtendedFetch
helpviewer_keywords:
- SQLExtendedFetch function [ODBC]
ms.assetid: 940b5cf7-581c-4ede-8533-c67d5e9ef488
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1a02b1c2e050b6fc7a0724286c7f023cb376e7bb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlextendedfetch-function"></a>SQLExtendedFetch (fonction)
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : déconseillé  
  
 **Résumé**  
 **SQLExtendedFetch** extrait l’ensemble spécifié de lignes de données du jeu de résultats et retourne les données pour toutes les colonnes liées. Ensembles de lignes peut être spécifié à une position relative ou absolue ou par signet.  
  
> [!NOTE]  
>  Dans ODBC 3 *.x*, **SQLExtendedFetch** a été remplacé par **SQLFetchScroll**. ODBC 3 *.x* applications ne doivent pas appeler **SQLExtendedFetch**; au lieu de cela, ils doivent appeler **SQLFetchScroll**. Le Gestionnaire de pilotes mappe **SQLFetchScroll** à **SQLExtendedFetch** lorsque vous travaillez avec une API ODBC 2 *.x* pilote. ODBC 3 *.x* pilotes doivent prendre en charge **SQLExtendedFetch** s’ils veulent travailler avec ODBC 2 *.x* les applications qui l’appellent. Pour plus d’informations, consultez « Commentaires » et [curseurs de bloc, les curseurs permettant le défilement et la compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) dans l’annexe g : pilote recommandations pour la compatibilité descendante.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLExtendedFetch(  
      SQLHSTMT         StatementHandle,  
      SQLUSMALLINT     FetchOrientation,  
      SQLLEN           FetchOffset,  
      SQLULEN *        RowCountPtr,  
      SQLUSMALLINT *   RowStatusArray);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *FetchOrientation*  
 [Entrée] Type d’extraction. Cela revient à *FetchOrientation* dans **SQLFetchScroll**.  
  
 *FetchOffset*  
 [Entrée] Numéro de la ligne à récupérer. Cela revient à *FetchOffset* dans **SQLFetchScroll**, avec une exception. Lorsque *FetchOrientation* est SQL_FETCH_BOOKMARK, *FetchOffset* est un signet de longueur fixe, pas un décalage à partir d’un signet. En d’autres termes, **SQLExtendedFetch** récupère le signet à partir de cet argument, et pas l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR. Il ne prend pas en charge les signets de longueur variable et ne prend pas en charge l’extraction d’un ensemble de lignes à un offset (autre que 0) à partir d’un signet.  
  
 *RowCountPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre de lignes réellement extraites. Ce tampon est utilisé dans la même manière que la mémoire tampon spécifiée par l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR. Cette mémoire tampon est utilisé uniquement par **SQLExtendedFetch**. Il n’est pas utilisé par **SQLFetch** ou **SQLFetchScroll**.  
  
 *RowStatusArray*  
 [Sortie] Pointeur vers un tableau dans lequel retourner l’état de chaque ligne. Ce tableau est utilisé de la même manière que dans le tableau spécifié par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.  
  
 Toutefois, l’adresse de ce tableau n’est pas stockée dans le champ SQL_DESC_STATUS_ARRAY_PTR l’IRD. En outre, ce tableau est utilisé uniquement par **SQLExtendedFetch** et par **SQLBulkOperations** avec un *opération* de SQL_ADD ou **SQLSetPos** lorsqu’elle est appelée après **SQLExtendedFetch**. Il n’est pas utilisé par **SQLFetch** ou **SQLFetchScroll**, et il n’est pas utilisé par **SQLBulkOperations** ou **SQLSetPos** lorsqu’elles sont appelées après **SQLFetch** ou **SQLFetchScroll**. Il est également utilisé lorsque **SQLBulkOperations** avec un *opération* SQL_ADD est appelé avant l’appel de toute fonction d’extraction. En d’autres termes, il est utilisé uniquement dans l’état de l’instruction S7. Il n’est pas utilisé dans les États d’instruction S5 ou S6. Pour plus d’informations, consultez [Transitions de l’instruction](../../../odbc/reference/appendixes/statement-transitions.md) dans les Tables de Transition d’état annexe b : ODBC.  
  
 Les applications doivent fournir un pointeur valide dans le *RowStatusArray* argument ; sinon, le comportement de **SQLExtendedFetch** et le comportement d’appels à **SQLBulkOperations** ou **SQLSetPos** une fois un curseur a été positionné par **SQLExtendedFetch** ne sont pas définis.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLExtendedFetch** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLError**. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLExtendedFetch** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire. Si une erreur se produit sur une colonne unique, **SQLGetDiagField** peut être appelé avec un *DiagIdentifier* de SQL_DIAG_COLUMN_NUMBER pour déterminer la colonne de l’erreur s’est produite ; et **SQLGetDiagField** peut être appelé avec un *DiagIdentifier* de SQL_DIAG_ROW_NUMBER pour déterminer la ligne qui contient cette colonne.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Retourné pour une colonne de données binary ou String a entraîné la troncation des caractères non vides ou les données binaires non NULL. S’il s’agissait d’une valeur de chaîne, il a été tronqué à la droite. S’il s’agissait d’une valeur numérique, la partie fractionnaire du nombre a été tronquée.  (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01 S 01|Erreur de ligne|Une erreur s’est produite lors de l’extraction d’une ou plusieurs lignes. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01S06|Tentative de récupération avant que le jeu de résultats renvoyé le premier ensemble de lignes|L’ensemble de lignes demandé avec chevauchement le début du jeu de résultats si la position actuelle a été au-delà de la première ligne et soit *FetchOrientation* a été SQL_PRIOR ou *FetchOrientation* a été SQL_RELATIVE avec une valeur négative *FetchOffset* dont la valeur absolue est inférieure ou égale à la SQL_ROWSET_SIZE actuel. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01 S 07|Troncation fractionnelle|Les données retournées pour une colonne a été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Heure, timestamp, intervalle types de données et qui contient un composant d’heure, la partie fractionnaire du temps a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07006|Violation de l’attribut de type de données restreint|Une valeur de données n’a pas pu être convertie au type de données C spécifié par *TargetType* dans **SQLBindCol**.|  
|07009|Index de descripteur non valide|La colonne 0 a été liée avec **SQLBindCol**, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS était définie sur SQL_UB_OFF.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|22002|Variable indicateur requise mais non fournie|Données de type NULL a été lue dans une colonne dont *StrLen_or_IndPtr* définie par **SQLBindCol** était un pointeur null.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|22003|Valeur numérique hors limites|Renvoi d’une valeur numérique (comme numérique ou chaîne) pour une ou plusieurs colonnes aurait entraîné la partie entière (par opposition aux fractions de seconde) le nombre à tronquer.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).<br /><br /> Pour plus d’informations, consultez [des recommandations pour l’intervalle et les Types de données numériques](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md) annexe d : Types de données.|  
|22007|Format datetime non valide|Une colonne de caractères dans le jeu de résultats a été liée à une date, heure ou une structure d’horodateur C, et une valeur dans la colonne a été, respectivement, une date non valide, heure ou timestamp.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|22012|Division par zéro|Une valeur à partir d’une expression arithmétique a été retournée, ce qui a entraîné de division par zéro.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|22015|Dépassement du champ Intervalle|Affectation d’une valeur numérique exacte ou d’intervalle de type SQL à un type d’intervalle C a provoqué une perte de chiffres significatifs dans le champ Début.<br /><br /> Lors de la récupération des données à un type d’intervalle C, il n’a pas de représentation de la valeur du type SQL dans le type d’intervalle C.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|22018|Valeur de caractère non valide pour la spécification de la casse|Le type C a été un numérique exact ou approximatif, une valeur datetime ou un type de données intervalle ; le type SQL de la colonne a un type de données caractère. et la valeur dans la colonne n’est pas un littéral valid du type C dépendant.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|24000|État de curseur non valide|Le *au paramètre StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats a été associé à la *au paramètre StatementHandle*.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLError** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*, et la fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLExtendedFetch** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM) spécifié *au paramètre StatementHandle* n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute**, ou une fonction de catalogue.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.<br /><br /> (DM) **SQLExtendedFetch** a été appelé pour le *au paramètre StatementHandle* après **SQLFetch** ou **SQLFetchScroll** a été appelée et avant **SQLFreeStmt** a été appelé avec l’option SQL_CLOSE.<br /><br /> (DM) **SQLBulkOperations** a été appelée pour une instruction avant **SQLFetch**, **SQLFetchScroll**, ou **SQLExtendedFetch** a été appelé, puis **SQLExtendedFetch** a été appelé avant **SQLFreeStmt** a été appelé avec l’option SQL_CLOSE.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY106|Type de récupération hors plage|(DM) la valeur spécifiée pour l’argument *FetchOrientation* n’était pas valide. (Voir « Commentaires ».)<br /><br /> L’argument *FetchOrientation* a été SQL_FETCH_BOOKMARK et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a pris la valeur SQL_UB_OFF.<br /><br /> La valeur de l’option d’instruction SQL_CURSOR_TYPE a été SQL_CURSOR_FORWARD_ONLY et la valeur de l’argument *FetchOrientation* n’était pas SQL_FETCH_NEXT.<br /><br /> L’argument *FetchOrientation* a été SQL_FETCH_RESUME.|  
|HY107|Valeur de ligne hors limites|La valeur spécifiée avec l’option d’instruction SQL_CURSOR_TYPE SQL_CURSOR_KEYSET_DRIVEN mais la valeur spécifiée avec l’attribut d’instruction SQL_KEYSET_SIZE était supérieur à 0 et inférieur à la valeur spécifiée avec l’attribut d’instruction SQL_ROWSET_SIZE.|  
|HY111|Valeur de signet non valide|L’argument *FetchOrientation* a été SQL_FETCH_BOOKMARK et le signet spécifié dans le *FetchOffset* argument n’était pas valide.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Source de données ou le pilote ne prend pas en charge le type d’extraction spécifié.<br /><br /> La source de données ou le pilote ne prend pas en charge la conversion spécifiée par la combinaison de la *TargetType* dans **SQLBindCol** et le type de données SQL de la colonne correspondante. Cette erreur s’applique uniquement lorsque le type de données SQL de la colonne a été mappé à un type de données SQL spécifique au pilote.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant que la source de données a retourné le jeu de résultats. Le délai d’expiration est défini par le **SQLSetStmtOption**, SQL_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Le comportement de **SQLExtendedFetch** est identique à celle de **SQLFetchScroll**, avec les exceptions suivantes :  
  
-   **SQLExtendedFetch** et **SQLFetchScroll** utiliser différentes méthodes pour retourner le nombre de lignes extraites. **SQLExtendedFetch** retourne le nombre de lignes extraites dans  *\*RowCountPtr*; **SQLFetchScroll** retourne le nombre de lignes extraites directement à la mémoire tampon vers laquelle pointée SQL_ATTR_ROWS_FETCHED_PTR. Pour plus d’informations, consultez la *RowCountPtr* argument.  
  
-   **SQLExtendedFetch** et **SQLFetchScroll** retourner l’état de chaque ligne dans plusieurs groupes différents. Pour plus d’informations, consultez la *RowStatusArray* argument.  
  
-   **SQLExtendedFetch** et **SQLFetchScroll** utiliser différentes méthodes pour récupérer le signet lorsque *FetchOrientation* est SQL_FETCH_BOOKMARK. **SQLExtendedFetch** ne prend pas en charge les ensembles de lignes de l’extraction à un offset différent de 0 à partir d’un signet ou les signets de longueur variable. Pour plus d’informations, consultez la *FetchOffset* argument.  
  
-   **SQLExtendedFetch** et **SQLFetchScroll** utiliser l’ensemble de lignes de différentes tailles. **SQLExtendedFetch** utilise la valeur de l’attribut d’instruction SQL_ROWSET_SIZE, et **SQLFetchScroll** utilise la valeur de l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.  
  
-   **SQLExtendedFetch** a erreur légèrement différent une sémantique de la gestion des **SQLFetchScroll**. Pour plus d’informations, consultez « Gestion des erreurs » dans la section « Commentaires » de [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
-   **SQLExtendedFetch** ne prend pas en charge les décalages de liaison (l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR).  
  
-   Les appels à **SQLExtendedFetch** ne peut pas être mélangés avec les appels à **SQLFetch** ou **SQLFetchScroll**et si **SQLBulkOperations** est appelée avant toute fonction d’extraction, **SQLExtendedFetch** ne peut pas être appelée tant que le curseur est fermé et rouvert. Autrement dit, **SQLExtendedFetch** peut être appelée uniquement dans l’état de l’instruction S7. Pour plus d’informations, consultez [Transitions de l’instruction](../../../odbc/reference/appendixes/statement-transitions.md) dans les Tables de Transition d’état annexe b : ODBC.  
  
 Lorsqu’une application appelle **SQLFetchScroll** lors de l’utilisation d’une API ODBC 2 *.x* pilote, le Gestionnaire de pilotes est mappé à cet appel à **SQLExtendedFetch**. Pour plus d’informations, consultez « SQLFetchScroll et ODBC 2 *.x* pilotes » dans [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
 Dans ODBC 2 *.x*, **SQLExtendedFetch** a été appelé pour extraire plusieurs lignes et **SQLFetch** a été appelé pour extraire une seule ligne. Dans ODBC 3 *.x*, quant à eux, **SQLFetch** peut être appelé pour extraire plusieurs lignes.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Exécution d’opérations de suppression, mise à jour ou insertion en bloc|[SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[SQLDescribeCol, fonction](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Définie les colonnes de retour du nombre de résultats|[SQLNumResultCols, fonction](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Positionnement du curseur, l’actualisation des données dans l’ensemble de lignes ou de mise à jour ou de suppression de données dans le jeu de résultats|[SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Définition d’un attribut d’instruction|[SQLSetStmtAttr, fonction](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
