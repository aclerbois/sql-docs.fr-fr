---
title: Fonction SQLGetData | Documents Microsoft
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
- SQLGetData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetData
helpviewer_keywords:
- SQLGetData function [ODBC]
ms.assetid: e3c1356a-5db7-4186-85fd-8b74633317e8
caps.latest.revision: 46
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ab603b24536afcbe7304dae907c10ee911d3cfd9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetdata-function"></a>Fonction SQLGetData
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLGetData** récupère les données pour une seule colonne du jeu de résultats ou pour un seul paramètre après **SQLParamData** retourne SQL_PARAM_DATA_AVAILABLE. Il peut être appelée plusieurs fois pour récupérer des données de longueur variable dans les parties.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLGetData(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   Col_or_Param_Num,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *Col_or_Param_Num*  
 [Entrée] Pour récupérer des données de la colonne, il est le numéro de la colonne pour laquelle retourner des données. Colonnes de jeu de résultats sont numérotées dans l’ordre croissant en commençant à 1 colonne. Le signet de colonne est le numéro de colonne 0 ; Cela peut être spécifié uniquement si les signets sont activés.  
  
 Pour récupérer des données de paramètre, il s’agit de l’ordinal du paramètre, qui commence à 1.  
  
 *TargetType*  
 [Entrée] L’identificateur de type du type de données C de la **TargetValuePtr* mémoire tampon. Pour obtenir la liste des types de données C valides et les identificateurs de type, consultez la [les Types de données C](../../../odbc/reference/appendixes/c-data-types.md) section annexe d : Types de données.  
  
 Si *TargetType* est SQL_ARD_TYPE, les pilotes utilise l’identificateur de type spécifié dans le champ SQL_DESC_CONCISE_TYPE de la ARD. Si *TargetType* est SQL_APD_TYPE, **SQLGetData** utilisera le même type de données C qui a été spécifié dans **SQLBindParameter**. Dans le cas contraire, le type de données C est spécifié dans **SQLGetData** remplace le type de données C spécifié dans **SQLBindParameter**. S’il s’agit SQL_C_DEFAULT, le pilote sélectionne le type de données C par défaut en fonction du type de données SQL de la source.  
  
 Vous pouvez également spécifier un type de données C étendu. Pour plus d’informations, consultez [des Types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 *TargetValuePtr*  
 [Sortie] Pointeur vers la mémoire tampon dans lequel retourner les données.  
  
 *TargetValuePtr* ne peut pas être NULL.  
  
 *BufferLength*  
 [Entrée] Longueur de la **TargetValuePtr* mémoire tampon en octets.  
  
 Le pilote utilise *BufferLength* pour éviter d’écrire au-delà de la fin de la \* *TargetValuePtr* lors du retour des données de longueur variable, telles que les données binaires ou de caractères de la mémoire tampon. Notez que le pilote compte le caractère de fin de la valeur null lors du retour des données caractères \* *TargetValuePtr*. **TargetValuePtr* doit comporter un espace pour le caractère null, ou le pilote tronque les données.  
  
 Lorsque le pilote retourne les données de longueur fixe, par exemple un entier ou une structure de date, le pilote ignore *BufferLength* et suppose que la mémoire tampon est suffisamment grande pour contenir les données. Il est donc important de l’application pour allouer une mémoire tampon suffisamment grand pour les données de longueur fixe ou le pilote écrira au-delà de la fin de la mémoire tampon.  
  
 **SQLGetData** retourne SQLSTATE HY090 (longueur de chaîne ou une mémoire tampon non valide) lorsque *BufferLength* est inférieure à 0 mais pas lorsque *BufferLength* est 0.  
  
 *StrLen_or_IndPtr*  
 [Sortie] Pointeur vers la mémoire tampon dans lequel retourner la valeur de longueur ou d’indicateur. S’il s’agit d’un pointeur null, aucune valeur de longueur ou indicateur n’est retourné. Cela retourne une erreur lorsque les données en cours d’extraction seront NULL.  
  
 **SQLGetData** peut renvoyer les valeurs suivantes dans la mémoire tampon de longueur / d’indicateur :  
  
-   La longueur des données available retourner  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 Pour plus d’informations, consultez [à l’aide des valeurs de longueur / d’indicateur](../../../odbc/reference/develop-app/using-length-and-indicator-values.md) et « Commentaires » dans cette rubrique.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetData** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLGetData** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Toutes les données pour la colonne spécifiée, *Col_or_Param_Num*, pu être récupérées dans un seul appel à la fonction. SQL_NO_TOTAL ou la longueur des données restantes dans la colonne spécifiée avant l’appel à **SQLGetData** est retourné dans \* *StrLen_or_IndPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).<br /><br /> Pour plus d’informations sur l’utilisation de plusieurs appels à **SQLGetData** pour une colonne unique, consultez « Commentaires ».|  
|01 S 07|Troncation fractionnelle|Les données retournées pour une ou plusieurs colonnes ont été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Heure, timestamp, intervalle types de données et qui contient un composant d’heure, la partie fractionnaire du temps a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07006|Violation de l’attribut de type de données restreint|La valeur des données d’une colonne dans le jeu de résultats ne peut pas être convertie au type de données C spécifié par l’argument *TargetType*.|  
|07009|Index de descripteur non valide|La valeur spécifiée pour l’argument *Col_or_Param_Num* était 0, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS était définie sur SQL_UB_OFF.<br /><br /> La valeur spécifiée pour l’argument *Col_or_Param_Num* était supérieur au nombre de colonnes dans le jeu de résultats.<br /><br /> Le *Col_or_Param_Num* valeur n’est pas égal à l’ordinal du paramètre qui n’est disponible.<br /><br /> (DM) la colonne spécifiée a été liée. Cette description ne s’applique pas aux pilotes qui retournent le masque de bits SQL_GD_BOUND pour l’option SQL_GETDATA_EXTENSIONS **SQLGetInfo**.<br /><br /> (DM) le numéro de la colonne spécifiée est inférieur ou égal au nombre de la colonne liée de la plus élevée. Cette description ne s’applique pas aux pilotes qui retournent le masque de bits SQL_GD_ANY_COLUMN pour l’option SQL_GETDATA_EXTENSIONS **SQLGetInfo**.<br /><br /> (DM) l’application a déjà appelé **SQLGetData** pour la ligne actuelle ; le nombre de la colonne spécifiée dans l’appel actuel est inférieur au nombre de la colonne spécifiée dans l’appel précédent ; et le pilote ne retourne pas le masque de bits SQL_GD_ANY_ORDER pour l’option SQL_GETDATA_EXTENSIONS **SQLGetInfo**.<br /><br /> (DM) le *TargetType* argument était SQL_ARD_TYPE et le *Col_or_Param_Num* Échec de l’enregistrement de descripteur dans le ARD la vérification de cohérence.<br /><br /> (DM) le *TargetType* argument SQL_ARD_TYPE, et la valeur du champ SQL_DESC_COUNT de la ARD était inférieure à la *Col_or_Param_Num* argument.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|22002|Variable indicateur requise mais non fournie|*StrLen_or_IndPtr* était un pointeur null et les données de valeur NULL a été récupérées.|  
|22003|Valeur numérique hors limites|Renvoi d’une valeur numérique (comme numérique ou chaîne) pour la colonne aurait entraîné la partie entière (par opposition aux fractions de seconde) le nombre à tronquer.<br /><br /> Pour plus d’informations, consultez [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22007|Format datetime non valide|La colonne de type caractère dans le jeu de résultats a été liée à une date, une heure ou une structure d’horodateur C, et la valeur dans la colonne a une date non valide, heure ou timestamp, respectivement. Pour plus d’informations, consultez [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22012|Division par zéro|Une valeur à partir d’une expression arithmétique a entraîné de division par zéro a été retournée.|  
|22015|Dépassement du champ Intervalle|Affectation d’une valeur numérique exacte ou d’intervalle de type SQL à un type d’intervalle C a provoqué une perte de chiffres significatifs dans le champ Début.<br /><br /> Lors du renvoi des données à un type d’intervalle C, il n’a pas de représentation de la valeur du type SQL dans le type d’intervalle C.|  
|22018|Valeur de caractère non valide pour la spécification de la casse|Une colonne de caractères dans le jeu de résultats a été retournée dans une mémoire tampon de caractères C, et la colonne contenue un caractère pour lequel il n’existe aucune représentation dans le jeu de caractères de la mémoire tampon.<br /><br /> Le type C a été un numérique exact ou approximatif, une valeur datetime ou un type de données intervalle ; le type SQL de la colonne a un type de données caractère. et la valeur dans la colonne n’est pas un littéral valid du type C dépendant.|  
|24000|État de curseur non valide|(DM), la fonction a été appelée sans appeler d’abord **SQLFetch** ou **SQLFetchScroll** pour positionner le curseur sur la ligne de données requis.<br /><br /> (DM) le *au paramètre StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats a été associé à la *au paramètre StatementHandle*.<br /><br /> Un curseur a été ouvert sur le *au paramètre StatementHandle* et **SQLFetch** ou **SQLFetchScroll** avait été appelée, mais le curseur est positionné avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les *MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY003|Type de programme hors limites|(DM) l’argument *TargetType* n’était pas un type de données valide, SQL_C_DEFAULT, SQL_ARD_TYPE (en cas de la récupération des données de la colonne) ou SQL_APD_TYPE (en cas de la récupération des données de paramètre).<br /><br /> (DM) l’argument *Col_or_Param_Num* était 0 et l’argument *TargetType* n’était pas SQL_C_BOOKMARK pour un signet de longueur fixe ou un SQL_C_VARBOOKMARK un signet de longueur variable.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*, et la fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un thread différent dans une application multithread, puis la fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) l’argument *TargetValuePtr* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) spécifié *au paramètre StatementHandle* n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLGetData** fonction a été appelée.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.<br /><br /> (DM) le *au paramètre StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats a été associé à la *au paramètre StatementHandle*.<br /><br /> Un appel à **SQLExeceute**, **SQLExecDirect**, ou **SQLMoreResults** retourné SQL_PARAM_DATA_AVAILABLE, mais **SQLGetData** a été appelé, au lieu de **SQLParamData**.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength* était inférieure à 0.<br /><br /> La valeur spécifiée pour l’argument *BufferLength* était inférieure à 4, le *Col_or_Param_Num* argument a la valeur 0, et le pilote a été un ODBC 2 *.x* pilote.|  
|HY109|Position du curseur non valide|Le curseur a été positionné (par **SQLSetPos**, **SQLFetch**, **SQLFetchScroll**, ou **SQLBulkOperations**) sur une ligne qui a été supprimée ou ne peut pas être extraites.<br /><br /> Le curseur a été un curseur avant uniquement, et la taille de l’ensemble de lignes est supérieure à un.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La source de données ou le pilote ne prend pas en charge l’utilisation de **SQLGetData** avec plusieurs lignes dans **SQLFetchScroll**. Cette description ne s’applique pas aux pilotes qui retournent le masque de bits SQL_GD_BLOCK pour l’option SQL_GETDATA_EXTENSIONS **SQLGetInfo**.<br /><br /> La source de données ou le pilote ne prend pas en charge la conversion spécifiée par la combinaison de la *TargetType* argument et le type de données SQL de la colonne correspondante. Cette erreur s’applique uniquement lorsque le type de données SQL de la colonne a été mappé à un type de données SQL spécifique au pilote.<br /><br /> Le pilote prend en charge uniquement ODBC 2 *.x*et l’argument *TargetType* est une des opérations suivantes :<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> et les types de données d’intervalle C répertoriés dans [les Types de données C](../../../odbc/reference/appendixes/c-data-types.md) annexe d : Types de données.<br /><br /> Le pilote prend uniquement en charge les versions ODBC avant 3.50 et l’argument *TargetType* a été SQL_C_GUID.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLGetData** renvoie les données dans une colonne spécifiée. **SQLGetData** peut être appelée uniquement après une ou plusieurs lignes ont été extraites à partir du jeu de résultats en **SQLFetch**, **SQLFetchScroll**, ou **SQLExtendedFetch**. Si les données de longueur variable sont trop grandes pour être retourné dans un seul appel à **SQLGetData** (en raison d’une limitation de l’application), **SQLGetData** peut les récupérer en parties. Il est possible de lier des colonnes dans une ligne et l’appel **SQLGetData** pour d’autres, bien que ce soit soumis à certaines restrictions. Pour plus d’informations, consultez [obtention de données de type Long](../../../odbc/reference/develop-app/getting-long-data.md).  
  
 Pour plus d’informations sur l’utilisation de **SQLGetData** avec des paramètres de sortie diffusée en continu, consultez [la récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="using-sqlgetdata"></a>À l’aide de SQLGetData  
 Si le pilote ne prend pas en charge les extensions de **SQLGetData**, la fonction peut retourner uniquement les données des colonnes indépendantes avec un nombre supérieur à celui de la dernière colonne dépendante. En outre, au sein d’une ligne de données, la valeur de la *Col_or_Param_Num* argument dans chaque appel à **SQLGetData** doit être supérieure ou égale à la valeur de *Col_or_Param_Num* dans l’appel précédent ; autrement dit, celles-ci doivent être extraites en augmentant l’ordre de numéro de colonne. Enfin, si aucune extension n’est prises en charge, **SQLGetData** ne peut pas être appelée si la taille de l’ensemble de lignes est supérieure à 1.  
  
 Les pilotes peuvent assouplir ces restrictions. Pour déterminer quelles restrictions assouplit d’un pilote, une application appelle **SQLGetInfo** avec les options SQL_GETDATA_EXTENSIONS suivantes :  
  
-   SQL_GD_OUTPUT_PARAMS = **SQLGetData** peut être appelée pour renvoyer les valeurs de paramètre de sortie. Pour plus d’informations, consultez [la récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
-   SQL_GD_ANY_COLUMN. Si cette option est retournée, **SQLGetData** peut être appelée pour toute colonne indépendante, y compris celles avant la dernière colonne dépendante.  
  
-   SQL_GD_ANY_ORDER. Si cette option est retournée, **SQLGetData** peut être appelée pour les colonnes indépendantes dans n’importe quel ordre.  
  
-   SQL_GD_BLOCK. Si cette option est retournée par **SQLGetInfo** l’InfoType SQL_GETDATA_EXTENSIONS, le pilote prend en charge les appels à **SQLGetData** lorsque la taille de l’ensemble de lignes est supérieure à 1 et que l’application peut appeler **SQLSetPos** avec l’option SQL_POSITION pour positionner le curseur sur la ligne appropriée avant d’appeler **SQLGetData.**  
  
-   SQL_GD_BOUND. Si cette option est retournée, **SQLGetData** peut être appelée pour les colonnes liées, ainsi que pour les colonnes indépendantes.  
  
 Il existe deux exceptions à ces restrictions et la capacité d’un pilote d’assouplir les. Tout d’abord, **SQLGetData** ne doit jamais être appelée pour un curseur avant uniquement lorsque la taille de l’ensemble de lignes est supérieure à 1. En second lieu, si un pilote prend en charge les signets, il doit toujours prendre en charge la possibilité d’appeler **SQLGetData** pour la colonne 0, même si elle n’autorise pas les applications d’appeler **SQLGetData** pour d’autres colonnes avant la dernière colonne dépendante. (Lorsqu’une application ne fonctionne pas avec une API ODBC 2 *.x* pilote, **SQLGetData** retournent avec succès un signet lorsqu’elle est appelée avec *Col_or_Param_Num* égale à 0 après un appel à **SQLFetch**, car **SQLFetch** est mappé par le ODBC 3 *.x* du Gestionnaire de pilotes à **SQLExtendedFetch** avec un *FetchOrientation* de SQL_FETCH_NEXT, et **SQLGetData** avec un *Col_or_Param_Num* 0 est mappé par le ODBC 3 *.x* du Gestionnaire de pilotes à **SQLGetStmtOption** avec un *fOption* de SQL_GET_BOOKMARK.)  
  
 **SQLGetData** ne peut pas être utilisé pour récupérer le signet pour une ligne qui vient d’être inséré en appelant **SQLBulkOperations** avec l’option SQL_ADD, étant donné que le curseur n’est pas positionné sur la ligne. Une application peut récupérer le signet pour une ligne en liant la colonne 0 avant d’appeler **SQLBulkOperations** avec SQL_ADD, auquel cas **SQLBulkOperations** renvoie le signet dans la mémoire tampon liée. **SQLFetchScroll** peut ensuite être appelée avec SQL_FETCH_BOOKMARK pour repositionner le curseur sur cette ligne.  
  
 Si le *TargetType* argument est un type de données d’intervalle, l’intervalle par défaut de début précision (2) et la précision de secondes de l’intervalle par défaut (6), comme défini dans les champs SQL_DESC_DATETIME_INTERVAL_PRECISION et SQL_DESC_PRECISION de la ARD, respectivement, sont utilisés pour les données. Si le *TargetType* argument est un type de données SQL_C_NUMERIC, la précision par défaut (définies par le pilote) et par défaut de l’échelle (0), comme défini dans les champs SQL_DESC_PRECISION et SQL_DESC_SCALE de la ARD, sont utilisé pour les données. Si l’échelle ni précision par défaut ne c'est-à-dire pas, l’application doit définir explicitement le champ de descripteur approprié par un appel à **SQLSetDescField** ou **SQLSetDescRec**. Il peut définir le champ SQL_DESC_CONCISE_TYPE à SQL_C_NUMERIC et appelez **SQLGetData** avec un *TargetType* argument de SQL_ARD_TYPE, ce qui provoque les valeurs de précision et l’échelle dans les champs de descripteur à utiliser.  
  
> [!NOTE]  
>  Dans ODBC 2 *.x*, ensemble d’applications *TargetType* SQL_C_DATE, SQL_C_TIME ou SQL_C_TIMESTAMP pour indiquer que \* *TargetValuePtr* est une structure de date, heure ou timestamp. Dans ODBC 3 *.x*, ensemble d’applications *TargetType* SQL_C_TYPE_DATE, SQL_C_TYPE_TIME ou SQL_C_TYPE_TIMESTAMP. Le Gestionnaire de pilotes rend les mappages appropriés si nécessaire, en fonction de la version de l’application et le pilote.  
  
## <a name="retrieving-variable-length-data-in-parts"></a>La récupération des données de longueur Variable dans les parties  
 **SQLGetData** peut être utilisé pour récupérer des données d’une colonne qui contient les données de longueur variable dans les parties, autrement dit, lorsque l’identificateur du type de données SQL de la colonne est SQL_CHAR, SQL_VARCHAR, SQL_LONGVARCHAR, SQL_WCHAR, SQL_WVARCHAR, SQL_ WLONGVARCHAR, SQL_BINARY, SQL_VARBINARY, SQL_LONGVARBINARY ou un identificateur spécifique du pilote pour un type de longueur variable.  
  
 Pour récupérer des données à partir d’une colonne dans les parties, l’application appelle **SQLGetData** plusieurs fois de suite pour la même colonne. À chaque appel, **SQLGetData** retourne la partie suivante de données. C’est à l’application pour réassembler les parties, en prenant soin de supprimer le caractère de fin de la valeur null à partir des parties intermédiaires des données de caractères. S’il existe plus de données à retourner ou non de mémoire tampon insuffisante a été alloué pour le caractère de fin, **SQLGetData** retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01004 (données tronquées). Lorsqu’elle retourne la dernière partie des données, **SQLGetData** retourne SQL_SUCCESS. Ni SQL_NO_TOTAL ni zéro peut être renvoyé sur le dernier appel valid pour récupérer des données à partir d’une colonne, parce que l’application puis n’aurait aucun moyen de connaître la quantité des données dans la mémoire tampon d’application est valide. Si **SQLGetData** est appelée après cela, il retourne SQL_NO_DATA. Pour plus d’informations, consultez la section suivante, « Récupération des données avec SQLGetData ».  
  
 Les signets de longueur variable peuvent être retournées dans les parties par **SQLGetData**. Comme avec d’autres données, un appel à **SQLGetData** pour renvoyer des signets de longueur variable dans les parties retourne SQLSTATE 01004 (chaîne de données tronquées à droite) et SQL_SUCCESS_WITH_INFO lorsqu’il existe plus de données à retourner. Cela est différent dans le cas lorsqu’un signet de longueur variable est tronqué par un appel à **SQLFetch** ou **SQLFetchScroll**, qui retourne SQL_ERROR et SQLSTATE 22001 (données de chaîne tronquées à droite).  
  
 **SQLGetData** ne peut pas être utilisée pour retourner des données de longueur fixe dans les parties. Si **SQLGetData** est appelée plusieurs fois dans une ligne d’une colonne contenant des données de longueur fixe, il retourne SQL_NO_DATA pour tous les appels après la première.  
  
## <a name="retrieving-streamed-output-parameters"></a>La récupération des paramètres de sortie diffusée en continu  
 Si un pilote prend en charge les paramètres de sortie en continu, une application peut appeler **SQLGetData** avec une petite mémoire tampon autant de fois pour récupérer une valeur de paramètre de grande taille. Pour plus d’informations sur le paramètre de sortie en continu, consultez [la récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="retrieving-data-with-sqlgetdata"></a>La récupération des données avec SQLGetData  
 Pour retourner des données pour la colonne spécifiée, **SQLGetData** effectue la séquence d’étapes suivante :  
  
1.  Retourne SQL_NO_DATA si elle a déjà retourné toutes les données de la colonne.  
  
2.  Jeux de \* *StrLen_or_IndPtr* avec la valeur SQL_NULL_DATA si les données sont NULL. Si les données sont NULL et *StrLen_or_IndPtr* était un pointeur null, **SQLGetData** retourne SQLSTATE 22002 (variable d’indicateur requise mais non fournie).  
  
     Si les données de la colonne ne sont pas NULL, **SQLGetData** passe à l’étape 3.  
  
3.  Si l’attribut d’instruction SQL_ATTR_MAX_LENGTH est définie sur une valeur différente de zéro, si la colonne contient des données binaires ou caractères et si **SQLGetData** n’a pas été précédemment appelé pour la colonne, les données sont tronquées à octets SQL_ATTR_MAX_LENGTH.  
  
    > [!NOTE]  
    >  L’attribut d’instruction SQL_ATTR_MAX_LENGTH vise à réduire le trafic réseau. Il est généralement implémentée par la source de données, ce qui tronque les données avant de le renvoyer sur le réseau. Pilotes et les sources de données ne sont pas obligés sa prise en charge. Par conséquent, pour garantir que les données sont tronquées à une taille particulière, une application doit allouer un tampon de cette taille et spécifiez la taille de la *BufferLength* argument.  
  
4.  Convertit les données dans le type spécifié dans *TargetType*. Les données étant données la précision par défaut et l’échelle pour ce type de données. Si *TargetType* est SQL_ARD_TYPE, les données de type dans le champ SQL_DESC_CONCISE_TYPE de la ARD est utilisé. Si *TargetType* est SQL_ARD_TYPE, les données dans le SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_PRECISION, reçoit la précision et l’échelle et champs SQL_DESC_SCALE de la ARD, en fonction des données de type dans le champ SQL_DESC_CONCISE_TYPE. Si l’échelle ni précision par défaut ne c'est-à-dire pas, l’application doit définir explicitement le champ de descripteur approprié par un appel à **SQLSetDescField** ou **SQLSetDescRec**.  
  
5.  Si les données a été converties en un type de données de longueur variable, telles que le caractère ou binaire, **SQLGetData** vérifie si la longueur des données dépasse *BufferLength*. Si la longueur des données de caractères (y compris le caractère de fin de la valeur null) dépasse *BufferLength*, **SQLGetData** tronque les données à *BufferLength* moins la longueur d’un caractère de fin de la valeur null. Il null-termine ensuite les données. Si la longueur des données binaires dépasse la longueur du tampon de données, **SQLGetData** tronque à *BufferLength* octets.  
  
     Si la mémoire tampon de données fourni est trop petit pour contenir le caractère null, **SQLGetData** retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01004.  
  
     **SQLGetData** jamais tronque les données converties en données de longueur fixe types ; il suppose toujours que la longueur de **TargetValuePtr* est la taille du type de données.  
  
6.  Place les données converties (et éventuellement tronquées) dans \* *TargetValuePtr*. Notez que **SQLGetData** ne peut pas retourner des données hors ligne.  
  
7.  Place la longueur des données dans \* *StrLen_or_IndPtr*. Si *StrLen_or_IndPtr* était un pointeur null, **SQLGetData** ne retourne pas la longueur.  
  
    -   Pour les données binaires ou de caractères, il s’agit la longueur des données après la conversion et avant la troncature en raison *BufferLength*. Si le pilote ne peut pas déterminer la longueur des données après la conversion, comme cela est parfois le cas avec les données de type long, il retourne SQL_SUCCESS_WITH_INFO et définit la longueur à SQL_NO_TOTAL. (Le dernier appel à **SQLGetData** doivent toujours retourner la longueur des données, pas zéro ou un SQL_NO_TOTAL.) Si les données ont été tronquées en raison de l’instruction SQL_ATTR_MAX_LENGTH d’attribut, la valeur de cet attribut, par opposition à la longueur réelle : est placé dans \* *StrLen_or_IndPtr*. Il s’agit, car cet attribut est conçu pour tronquer des données sur le serveur avant la conversion, donc le pilote n’a aucun moyen de savoir quelles la longueur réelle est. Lorsque **SQLGetData** est appelée plusieurs fois de suite pour la même colonne, il s’agit de la longueur des données disponibles au début de l’appel en cours ; autrement dit, la longueur diminue avec chaque appel suivant.  
  
    -   Pour tous les autres types de données, il s’agit la longueur des données après la conversion ; Autrement dit, elle est la taille du type auquel les données a été converties.  
  
8.  Si les données sont tronquées sans perte de précision lors de la conversion (par exemple, le nombre réel 1,234 est tronqué lorsque converti à l’entier 1) ou parce que *BufferLength* est trop petite (par exemple, la chaîne « abcdef » est placée dans une mémoire tampon de 4 octets), **SQLGetData** retourne SQLSTATE 01004 (données tronquées) et SQL_SUCCESS_WITH_INFO. Si les données sont tronquées sans perte de crédibilité en raison de l’attribut d’instruction SQL_ATTR_MAX_LENGTH, **SQLGetData** retourne SQL_SUCCESS et ne retourne pas de valeur SQLSTATE 01004 (données tronquées).  
  
 Le contenu de la mémoire tampon de données liées (si **SQLGetData** est appelée sur une colonne dépendante) et la mémoire tampon de longueur / d’indicateur ne sont pas définis si **SQLGetData** ne renvoie pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO.  
  
 Les appels successifs à **SQLGetData** récupère les données de la dernière colonne demandée ; décalages préalables deviennent non valides. Par exemple, lorsque la séquence suivante est exécutée :  
  
```  
SQLGetData(icol=n), SQLGetData(icol=m), SQLGetData(icol=n)  
```  
  
 le deuxième appel à SQLGetData(icol=n) récupère les données à partir du début de la colonne n. Un décalage dans les données en raison d’appels antérieurs à **SQLGetData** pour la colonne n’est plus valide.  
  
## <a name="descriptors-and-sqlgetdata"></a>Descripteurs et SQLGetData  
 **SQLGetData** n’interagit pas directement avec tous les champs de descripteur.  
  
 Si *TargetType* est SQL_ARD_TYPE, les données de type dans le champ SQL_DESC_CONCISE_TYPE de la ARD est utilisé. Si *TargetType* est SQL_ARD_TYPE ou SQL_C_DEFAULT, les données dans le SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_PRECISION, reçoit la précision et l’échelle et champs SQL_DESC_SCALE de la ARD, en fonction des données de type dans le champ SQL_DESC_CONCISE_TYPE.  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application exécute un **sélectionnez** instruction pour retourner un jeu de résultats du client ID, les noms et triés par nom, ID et numéro de téléphone de numéros de téléphone. Pour chaque ligne de données, il appelle **SQLFetch** pour positionner le curseur à la ligne suivante. Il appelle **SQLGetData** pour récupérer les données extraites ; les mémoires tampons pour les données et le nombre d’octets retourné sont spécifiés dans l’appel à **SQLGetData**. Enfin, elle affiche le nom de chaque employé, ID et numéro de téléphone.  
  
```  
#define NAME_LEN 50  
#define PHONE_LEN 50  
  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   sCustID, cbName, cbAge, cbBirthday;  
SQLRETURN    retcode;  
SQLHSTMT     hstmt;  
  
retcode = SQLExecDirect(hstmt,  
   "SELECT CUSTID, NAME, PHONE FROM CUSTOMERS ORDER BY 2, 1, 3",  
   SQL_NTS);  
  
if (retcode == SQL_SUCCESS) {  
   while (TRUE) {  
      retcode = SQLFetch(hstmt);  
      if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO) {  
         show_error();  
      }  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
         /* Get data for columns 1, 2, and 3 */  
  
         SQLGetData(hstmt, 1, SQL_C_ULONG, &sCustID, 0, &cbCustID);  
         SQLGetData(hstmt, 2, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
         SQLGetData(hstmt, 3, SQL_C_CHAR, szPhone, PHONE_LEN,  
            &cbPhone);  
  
         /* Print the row of data */  
  
         fprintf(out, "%-5d %-*s %*s", sCustID, NAME_LEN-1, szName,   
            PHONE_LEN-1, szPhone);  
      } else {  
         break;  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Assignation du stockage pour une colonne dans un jeu de résultats|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Exécution d’opérations en bloc qui ne sont pas liées à la position de curseur de bloc|[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|L’exécution d’une instruction SQL|[SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|L’extraction d’une ligne unique de données ou un bloc de données dans une direction avant uniquement|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Envoi de données de paramètre au moment de l’exécution|[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Positionnement du curseur, l’actualisation des données dans l’ensemble de lignes ou de mise à jour ou de suppression de données dans l’ensemble de lignes|[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Récupération des paramètres de sortie à l’aide de SQLGetData ](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
