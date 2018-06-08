---
title: Fonction SQLFetch | Documents Microsoft
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
- SQLFetch
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLFetch
helpviewer_keywords:
- SQLFetch function [ODBC]
ms.assetid: 6c6611d2-bc6a-4390-87c9-1c5dd9cfe07c
caps.latest.revision: 27
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2a340283066558215e5534e327026350cdcab589
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlfetch-function"></a>Fonction SQLFetch
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLFetch** extrait le jeu suivant de lignes de données du jeu de résultats et retourne les données pour toutes les colonnes liées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLFetch(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLFetch** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant [fonction SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLFetch** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire. Si une erreur se produit sur une colonne unique, [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) peut être appelé avec un *DiagIdentifier* de SQL_DIAG_COLUMN_NUMBER pour déterminer la colonne de l’erreur s’est produite ; et **SQLGetDiagField** peut être appelé avec un *DiagIdentifier* de SQL_DIAG_ROW_NUMBER pour déterminer la ligne qui contient cette colonne.  
  
 Pour toutes ces SQLSTATE qui peut retourner SQL_SUCCESS_WITH_INFO ou SQL_ERROR (sauf 01xxx SQLSTATE), SQL_SUCCESS_WITH_INFO est retourné si une erreur se produit sur un ou plusieurs, mais pas toutes, les lignes d’une opération de plusieurs ligne, SQL_ERROR est retourné si une erreur se produit lors d’une opération de ligne unique.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Retourné pour une colonne de données binary ou String a entraîné la troncation des caractères non vides ou les données binaires non NULL. S’il s’agissait d’une valeur de chaîne, il a été tronqué à la droite.|  
|01 S 01|Erreur de ligne|Une erreur s’est produite lors de l’extraction d’une ou plusieurs lignes.<br /><br /> (Si cet valeur SQLSTATE est retourné lorsqu’une ODBC 3 *.x* application fonctionne avec une API ODBC 2 *.x* pilote, il peut être ignoré.)|  
|01 S 07|Troncation fractionnelle|Les données retournées pour une colonne a été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Heure, timestamp, intervalle types de données et qui contiennent un composant d’heure, la partie fractionnaire du temps a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07006|Violation de l’attribut de type de données restreint|La valeur des données d’une colonne dans le jeu de résultats n’a pas pu être convertie au type de données spécifié par *TargetType* dans **SQLBindCol**.<br /><br /> La colonne 0 a été liée avec un type de données de SQL_C_BOOKMARK, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a pris la valeur SQL_UB_VARIABLE.<br /><br /> La colonne 0 a été liée avec un type de données de SQL_C_VARBOOKMARK, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS n’a pas été définie à SQL_UB_VARIABLE.|  
|07009|Index de descripteur non valide|Le pilote a été un ODBC 2 *.x* pilote qui ne prend pas en charge **SQLExtendedFetch**, et un numéro de colonne spécifié dans la liaison d’une colonne a été 0.<br /><br /> La colonne 0 a été liée, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été définie sur SQL_UB_OFF.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|22001|Données de type chaîne, droite tronquées|Un signet de longueur variable retourné pour une colonne a été tronqué.|  
|22002|Variable indicateur requise mais non fournie|Données de type NULL a été lue dans une colonne dont *StrLen_or_IndPtr* définie par **SQLBindCol** (ou SQL_DESC_INDICATOR_PTR définie par **SQLSetDescField** ou **SQLSetDescRec**) était un pointeur null.|  
|22003|Valeur numérique hors limites|Retourner la valeur numérique que numérique ou chaîne pour une ou plusieurs colonnes dépendantes aurait entraîné la partie entière (par opposition aux fractions de seconde) le nombre à tronquer.<br /><br /> Pour plus d’informations, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) annexe d : Types de données.|  
|22007|Format datetime non valide|Une colonne de caractères dans le jeu de résultats a été liée à une date, heure ou une structure d’horodateur C, et une valeur dans la colonne a été, respectivement, une date non valide, heure ou timestamp.|  
|22012|Division par zéro|Une valeur à partir d’une expression arithmétique a été retournée, ce qui a entraîné de division par zéro.|  
|22015|Dépassement du champ Intervalle|Affectation d’une valeur numérique exacte ou d’intervalle de type SQL à un type d’intervalle C a provoqué une perte de chiffres significatifs dans le champ Début.<br /><br /> Lors de la récupération des données à un type d’intervalle C, il n’a pas de représentation de la valeur du type SQL dans le type d’intervalle C.|  
|22018|Valeur de caractère non valide pour la spécification de la casse|Une colonne de caractères dans le jeu de résultats a été liée à une mémoire tampon de caractères C, et la colonne contenue un caractère pour lequel il n’existe aucune représentation dans le jeu de caractères de la mémoire tampon.<br /><br /> Le type C a été un numérique exact ou approximatif, une valeur datetime ou un type de données intervalle ; le type SQL de la colonne a un type de données caractère. et la valeur dans la colonne n’est pas un littéral valid du type C dépendant.|  
|24000|État de curseur non valide|Le *au paramètre StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats a été associé à la *au paramètre StatementHandle*.|  
|40001|Échec de la sérialisation|La transaction dans laquelle l’extraction a été exécutée a été arrêtée pour empêcher tout interblocage.|  
|40003|Saisie semi-automatique des instructions inconnue|Échec de la connexion associée lors de l’exécution de cette fonction, et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire qui est requis pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. Le **SQLFetch** fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. Le **SQLFetch** fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> Sinon, le **SQLFetch** fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLFetch** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM) spécifié *au paramètre StatementHandle* n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.<br /><br /> (DM) **SQLFetch** a été appelé pour le *au paramètre StatementHandle* après **SQLExtendedFetch** a été appelée et avant **SQLFreeStmt** avec la SQL_CLOSE option a été appelée.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|L’attribut d’instruction SQL_ATTR_USE_BOOKMARK a été défini sur SQL_UB_VARIABLE, et la colonne 0 a été liée à une mémoire tampon dont la longueur n’était pas égale à la longueur maximale du signet pour ce jeu de résultats. (Cette longueur est disponible dans le champ SQL_DESC_OCTET_LENGTH de l’IRD et peut être obtenue en appelant **SQLDescribeCol**, **SQLColAttribute**, ou **SQLGetDescField**.)|  
|HY107|Valeur de ligne hors limites|La valeur spécifiée avec l’attribut d’instruction SQL_ATTR_CURSOR_TYPE SQL_CURSOR_KEYSET_DRIVEN mais la valeur spécifiée avec l’attribut d’instruction SQL_ATTR_KEYSET_SIZE était supérieur à 0 et inférieur à la valeur spécifiée avec l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La source de données ou le pilote ne prend pas en charge la conversion spécifiée par la combinaison de la *TargetType* dans **SQLBindCol** et le type de données SQL de la colonne correspondante.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant la source de données a retourné le jeu de résultat demandé. Le délai d’expiration est défini via SQLSetStmtAttr, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLFetch** retourne l’ensemble de lignes suivant dans le jeu de résultats. Il peut être appelé uniquement pendant un jeu de résultats existe : autrement dit, après un appel qui crée un jeu de résultats et avant le curseur sur ensemble de résultats est fermé. Si toutes les colonnes sont liés, il retourne les données dans ces colonnes. Si l’application a spécifié un pointeur vers un tableau d’état de ligne ou une mémoire tampon dans lequel retourner le nombre de lignes extraites, **SQLFetch** retourne également ces informations. Les appels à **SQLFetch** peuvent être mélangés avec les appels de **SQLFetchScroll** mais ne peuvent pas être mélangés avec les appels de **SQLExtendedFetch**. Pour plus d’informations, consultez [l’extraction d’une ligne de données](../../../odbc/reference/develop-app/fetching-a-row-of-data.md).  
  
 Si un ODBC 3 *.x* application fonctionne avec une API ODBC 2 *.x* pilote, le Gestionnaire de pilotes mappe **SQLFetch** appelle à **SQLExtendedFetch** pour une API ODBC 2 *.x* pilote qui prend en charge **SQLExtendedFetch**. Si l’API ODBC 2 *.x* pilote ne prend pas en charge **SQLExtendedFetch**, le Gestionnaire de pilotes est mappé **SQLFetch** appelle à **SQLFetch** dans ODBC 2 *.x* pilote, ce qui peut récupérer qu’une seule ligne.  
  
 Pour plus d’informations, consultez [curseurs de bloc, les curseurs permettant le défilement et la compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) dans l’annexe g : pilote recommandations pour la compatibilité descendante.  
  
## <a name="positioning-the-cursor"></a>Positionnement du curseur  
 Lorsque le jeu de résultats est créé, le curseur est positionné avant le début du jeu de résultats. **SQLFetch** extrait l’ensemble de lignes suivant. Il est équivalent à l’appel **SQLFetchScroll** avec *FetchOrientation* SQL_FETCH_NEXT la valeur. Pour plus d’informations sur les curseurs, consultez [curseurs](../../../odbc/reference/develop-app/cursors.md) et [curseurs de bloc](../../../odbc/reference/develop-app/block-cursors.md).  
  
 L’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE Spécifie le nombre de lignes dans l’ensemble de lignes. Si l’ensemble de lignes à extraire par **SQLFetch** chevauche la fin du jeu de résultats, **SQLFetch** renvoie un ensemble de lignes partiel. Autrement dit, si S + R – 1 est supérieure à L, où S est la ligne de début de l’ensemble de lignes à extraire, R est la taille de l’ensemble de lignes et L est la dernière ligne du jeu de résultats, puis uniquement le premier L – S + 1 lignes de l’ensemble de lignes sont valides. Les lignes restantes sont vides et auront un état SQL_ROW_NOROW.  
  
 Après avoir **SQLFetch** renvoie, la ligne actuelle est la première ligne de l’ensemble de lignes.  
  
 Les règles répertoriées dans le tableau suivant décrivent le positionnement du curseur après un appel à **SQLFetch**, selon les conditions répertoriées dans la seconde table dans cette section.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|Avant de démarrer|1|  
|*CurrRowsetStart* \< =  *LastResultRow – la RowsetSize*[1]|*CurrRowsetStart* + *la RowsetSize*[2]|  
|*CurrRowsetStart* > *LastResultRow – la RowsetSize*[1]|Après la fin|  
|Après la fin|Après la fin|  
  
 [1] si la taille de l’ensemble de lignes est modifiée entre les extractions, il s’agit de la taille de l’ensemble de lignes qui a été utilisée avec l’extraction précédente.  
  
 [2] si la taille de l’ensemble de lignes est modifiée entre les extractions, il s’agit de la taille de l’ensemble de lignes qui a été utilisée avec la nouvelle extraction.  
  
|Notation|Signification|  
|--------------|-------------|  
|Avant de démarrer|Le curseur de bloc est positionné avant le début du jeu de résultats. Si la première ligne de l’ensemble de lignes nouvelles est avant le début du jeu de résultats, **SQLFetch** retourne SQL_NO_DATA.|  
|Après la fin|Le curseur de bloc est positionné après que la fin du résultat défini. Si la première ligne de l’ensemble de lignes nouvelles est après la fin du jeu de résultats, **SQLFetch** retourne SQL_NO_DATA.|  
|*CurrRowsetStart*|Le numéro de la première ligne dans l’ensemble de lignes en cours.|  
|*LastResultRow*|Le numéro de la dernière ligne du jeu de résultats.|  
|*RowsetSize*|La taille de l’ensemble de lignes.|  
  
 Par exemple, un jeu de résultats comporte 100 lignes, et la taille de l’ensemble de lignes est 5. Le tableau suivant présente le code de l’ensemble de lignes et de retour renvoyé par **SQLFetch** pour les positions de départ différentes.  
  
|Ensemble de lignes actuel|Code de retour|Nouvel ensemble de lignes|nombre de lignes extraites|  
|--------------------|-----------------|----------------|------------------------|  
|Avant de démarrer|SQL_SUCCESS|1 à 5|5|  
|1 à 5|SQL_SUCCESS|6 à 10|5|  
|52 à 56|SQL_SUCCESS|57 à 61|5|  
|91 à 95|SQL_SUCCESS|96-100|5|  
|93 à 97|SQL_SUCCESS|98 et 100. Les lignes 4 et 5 du tableau d’état de ligne sont définies sur SQL_ROW_NOROW.|3|  
|96-100|SQL_NO_DATA|Aucun.|0|  
|99 à 100.|SQL_NO_DATA|Aucun.|0|  
|Après la fin|SQL_NO_DATA|Aucun.|0|  
  
## <a name="returning-data-in-bound-columns"></a>Renvoi de données dans les colonnes dépendantes  
 En tant que **SQLFetch** retourne chaque ligne, il place les données pour chaque colonne dépendante dans la mémoire tampon liée à cette colonne. Si aucune colonne n’est liée, **SQLFetch** ne retourne aucune donnée, mais avance le curseur de bloc. Les données peuvent toujours être récupérées à l’aide de **SQLGetData**. Si le curseur est en ligne (autrement dit, le paramètre SQL_ATTR_ROW_ARRAY_SIZE est supérieur à 1), **SQLGetData** peut être appelée uniquement si SQL_GD_BLOCK est retourné lorsque **SQLGetInfo** est appelée avec un *InfoType* de SQL_GETDATA_EXTENSIONS. (Pour plus d’informations, consultez [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).)  
  
 Pour chaque colonne dépendante dans une ligne, **SQLFetch** effectue les opérations suivantes :  
  
1.  Définit la mémoire tampon de longueur / d’indicateur avec la valeur SQL_NULL_DATA et passe à la colonne suivante si les données sont NULL. Si les données sont NULL et aucune mémoire tampon de longueur / d’indicateur a été lié, **SQLFetch** retourne SQLSTATE 22002 (variable d’indicateur requise mais non fournie) pour la ligne et le passe à la ligne suivante. Pour plus d’informations sur la façon de déterminer l’adresse de la mémoire tampon de longueur / d’indicateur, consultez « Adresses mémoire tampon » dans [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
     Si les données de la colonne ne sont pas NULL, **SQLFetch** passe à l’étape 2.  
  
2.  Si l’attribut d’instruction SQL_ATTR_MAX_LENGTH est défini sur une valeur différente de zéro et la colonne contient des données binaires ou caractères, les données sont tronquées à octets SQL_ATTR_MAX_LENGTH.  
  
    > [!NOTE]  
    >  L’attribut d’instruction SQL_ATTR_MAX_LENGTH vise à réduire le trafic réseau. Il est généralement implémentée par la source de données, ce qui tronque les données avant de le renvoyer sur le réseau. Pilotes et les sources de données ne sont pas obligés sa prise en charge. Par conséquent, pour garantir que les données sont tronquées à une taille particulière, une application doit allouer un tampon de cette taille et spécifiez la taille de la *cbValueMax* argument dans **SQLBindCol**.  
  
3.  Convertit les données en type spécifié par *TargetType* dans **SQLBindCol**.  
  
4.  Si les données a été converties en un type de données de longueur variable, telles que le caractère ou binaire, **SQLFetch** vérifie si la longueur des données dépasse la longueur du tampon de données. Si la longueur des données de caractères (y compris le caractère de fin de la valeur null) dépasse la longueur du tampon de données, **SQLFetch** tronque les données à la longueur du tampon de données moins la longueur d’un caractère de fin de la valeur null. Il null-termine ensuite les données. Si la longueur des données binaires dépasse la longueur du tampon de données, **SQLFetch** tronque à la longueur du tampon de données. La longueur du tampon de données est spécifiée avec *BufferLength* dans **SQLBindCol**.  
  
     **SQLFetch** jamais tronque les données converties en données de longueur fixe types ; il suppose toujours que la longueur du tampon de données est la taille du type de données.  
  
5.  Place les données converties (et éventuellement tronquées) dans le tampon de données. Pour plus d’informations sur la façon de déterminer l’adresse de la mémoire tampon de données, consultez « Adresses mémoire tampon » dans [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
6.  Place la longueur des données dans la mémoire tampon de longueur / d’indicateur. Si le pointeur de l’indicateur et le pointeur de longueur ont été définies pour la même mémoire tampon (comme un appel à **SQLBindCol** est), la longueur est écrite dans la mémoire tampon des données valides et SQL_NULL_DATA est écrite dans la mémoire tampon pour les données de valeur NULL. Si aucune mémoire tampon de longueur / d’indicateur a été lié, **SQLFetch** ne retourne pas la longueur.  
  
    -   Pour les données binaires ou de caractères, c’est la longueur des données après la conversion et avant la troncature en raison de la mémoire tampon de données est trop petite. Si le pilote ne peut pas déterminer la longueur des données après la conversion, comme cela est parfois le cas avec les données de type long, il définit la longueur à SQL_NO_TOTAL. Si les données ont été tronquées en raison de l’attribut d’instruction SQL_ATTR_MAX_LENGTH, la valeur de cet attribut est placée dans la mémoire tampon de longueur / d’indicateur au lieu de la longueur réelle. Il s’agit, car cet attribut est conçu pour tronquer des données sur le serveur avant la conversion, afin que le pilote n’a aucun moyen de savoir quelle est la longueur réelle.  
  
    -   Pour tous les autres types de données, il s’agit la longueur des données après la conversion ; Autrement dit, elle est la taille du type auquel les données a été converties.  
  
     Pour plus d’informations sur la façon de déterminer l’adresse de la mémoire tampon de longueur / d’indicateur, consultez « Adresses mémoire tampon » dans [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
7.  Si les données sont tronquées lors de la conversion sans perte de chiffres significatifs (par exemple, le nombre réel 1,234 est tronqué à l’entier 1 lorsque converti), **SQLFetch** retourne SQLSTATE 01 s 07 (troncation fractionnelle) et SQL_SUCCESS_WITH_INFO. Si les données sont tronquées, car la longueur du tampon de données est trop petite (par exemple, la chaîne « abcdef » est placée dans une mémoire tampon de 4 octets), **SQLFetch** retourne SQLSTATE 01004 (données tronquées) et SQL_SUCCESS_WITH_INFO. Si les données sont tronquées en raison de l’attribut d’instruction SQL_ATTR_MAX_LENGTH, **SQLFetch** retourne SQL_SUCCESS et ne retourne pas de SQLSTATE 01 s 07 (troncation fractionnelle) ou SQLSTATE 01004 (données tronquées). Si les données sont tronquées lors de la conversion avec une perte de chiffres significatifs (par exemple, si une valeur SQL_INTEGER supérieure à 100 000 ont été convertie en un SQL_C_TINYINT), **SQLFetch** retourne SQLSTATE 22003 (valeur numérique hors limites) et (si la taille de l’ensemble de lignes est 1) de SQL_ERROR ou SQL_SUCCESS_WITH_INFO (si la taille de l’ensemble de lignes est supérieure à 1).  
  
 Le contenu de la mémoire tampon de longueur / d’indicateur et de la mémoire tampon de données liées n’est pas défini si **SQLFetch** ou **SQLFetchScroll** ne renvoie pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO.  
  
## <a name="row-status-array"></a>Tableau d’état de ligne  
 Le tableau d’état de ligne est utilisé pour retourner l’état de chaque ligne dans l’ensemble de lignes. L’adresse de ce tableau est spécifié avec l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR. Le tableau est alloué par l’application et doit avoir autant d’éléments spécifiés par l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE. Ses valeurs sont définies **SQLFetch**, **SQLFetchScroll**, et **SQLBulkOperations** ou **SQLSetPos** (sauf quand ils ont été appelées après que le curseur a été positionné par **SQLExtendedFetch**). Si la valeur de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR est un pointeur null, ces fonctions ne retournent pas l’état de la ligne.  
  
 Le contenu de la mémoire tampon de ligne état tableau n’est pas défini si **SQLFetch** ou **SQLFetchScroll** ne renvoie pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO.  
  
 Les valeurs suivantes sont retournées dans le tableau d’état de ligne.  
  
|Valeur de tableau de statut de ligne| Description|  
|----------------------------|-----------------|  
|SQL_ROW_SUCCESS|La ligne a été extraite avec succès et n’a pas changé depuis sa dernière extraction à partir de ce jeu de résultats.|  
|SQL_ROW_SUCCESS_WITH_INFO|La ligne a été extraite avec succès et n’a pas changé depuis sa dernière extraction à partir de ce jeu de résultats. Toutefois, un avertissement a été retourné à la ligne.|  
|SQL_ROW_ERROR|Une erreur s’est produite lors de l’extraction de la ligne.|  
|SQL_ROW_UPDATED [1], [2] et [3]|La ligne a été extraite avec succès et a été modifié depuis sa dernière extraction à partir de ce jeu de résultats. Si la ligne est extraite à nouveau à partir de ce jeu de résultats ou qu’il est actualisée par **SQLSetPos**, l’état est modifié pour le nouvel état de la ligne.|  
|SQL_ROW_DELETED [3]|La ligne a été supprimée depuis sa dernière extraction à partir de ce jeu de résultats.|  
|SQL_ROW_ADDED [4]|La ligne a été insérée par **SQLBulkOperations**. Si la ligne est extraite à nouveau à partir de ce jeu de résultats ou qu’il est actualisée par **SQLSetPos**, son état est SQL_ROW_SUCCESS.|  
|SQL_ROW_NOROW|L’ensemble de lignes avec chevauchement de la fin du jeu de résultats, et aucune ligne n’a été retourné qui correspondait à cet élément du tableau d’état de ligne.|  
  
 [1] pour le jeu de clés, mixtes et les curseurs dynamiques, si une valeur de clé est mis à jour, la ligne de données est considérée comme ont été supprimées et une nouvelle ligne ajoutée.  
  
 [2], certains pilotes ne peut pas détecter les mises à jour des données et par conséquent impossible de retourner cette valeur. Pour déterminer si un pilote peut détecter les mises à jour à nouveau extraites selon les lignes, une application appelle **SQLGetInfo** avec l’option SQL_ROW_UPDATES.  
  
 [3] **SQLFetch** peut retourner cette valeur uniquement lorsqu’il est combiné avec des appels à **SQLFetchScroll**. C’est parce que **SQLFetch** parcourt le jeu de résultats et lorsqu’il est utilisé exclusivement, ne pas récupérer de nouveau toutes les lignes. Car aucune ligne n’est de nouveau extraits, **SQLFetch** ne détecte pas les modifications apportées aux lignes extraites précédemment. Toutefois, si **SQLFetchScroll** positionne le curseur avant un préalable extrait des lignes et **SQLFetch** est utilisé pour extraire ces lignes, **SQLFetch** peut détecter les modifications apportées aux lignes.  
  
 [4] retournée par SQLBulkOperations uniquement. Non défini par **SQLFetch** ou **SQLFetchScroll**.  
  
### <a name="rows-fetched-buffer"></a>Mémoire tampon d’extraction de lignes  
 Les lignes extraites de mémoire tampon est utilisé pour retourner le nombre de lignes extraites, y compris les lignes pour lesquelles aucune donnée n’a été retournée, car une erreur s’est produite pendant qu’ils ont été lue. En d’autres termes, il est le nombre de lignes pour lesquelles la valeur dans le tableau d’état de ligne n’est pas SQL_ROW_NOROW. L’adresse de cette mémoire tampon est spécifiée avec l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR. La mémoire tampon est allouée par l’application. Il est défini par **SQLFetch** et **SQLFetchScroll**. Si la valeur de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR est un pointeur null, ces fonctions ne retournent pas le nombre de lignes extraites. Pour déterminer le numéro de la ligne actuelle dans le jeu de résultats, une application peut appeler **SQLGetStmtAttr** avec l’attribut SQL_ATTR_ROW_NUMBER.  
  
 Le contenu de la mémoire tampon de lignes lues n’est pas défini si **SQLFetch** ou **SQLFetchScroll** ne renvoie pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, sauf lorsque SQL_NO_DATA soit retourné, auquel cas la valeur dans la mémoire tampon de lignes lues est définie sur 0.  
  
### <a name="error-handling"></a>Gestion des erreurs  
 Erreurs et avertissements peuvent s’appliquent à des lignes individuelles ou à la fonction entière. Pour plus d’informations sur les enregistrements de diagnostic, consultez [Diagnostics](../../../odbc/reference/develop-app/diagnostics.md) et [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
#### <a name="errors-and-warnings-on-the-entire-function"></a>Erreurs et avertissements sur la fonction entière  
 Si une erreur s’applique à la fonction entière, tels que SQLSTATE HYT00 (expiration du délai) ou SQLSTATE 24000 (état de curseur non valide), **SQLFetch** retourne SQL_ERROR et la valeur SQLSTATE applicable. Le contenu des mémoires tampons de lignes n’est pas défini, et la position du curseur est inchangée.  
  
 Si un avertissement s’applique à la fonction entière, **SQLFetch** retourne SQL_SUCCESS_WITH_INFO et SQLSTATE applicable. Les enregistrements d’état pour les avertissements qui s’appliquent à la fonction entière sont retournés avant les enregistrements d’état qui s’appliquent à des lignes individuelles.  
  
#### <a name="errors-and-warnings-in-individual-rows"></a>Erreurs et avertissements dans les lignes individuelles  
 Si une erreur (par exemple, SQLSTATE 22012 (Division par zéro)) ou un avertissement (par exemple, SQLSTATE 01004 (données tronquées)) s’applique à une seule ligne, **SQLFetch**effectue les opérations suivantes :  
  
-   Définit l’élément correspondant du tableau d’état de ligne pour SQL_ROW_ERROR des erreurs ou SQL_ROW_SUCCESS_WITH_INFO pour les avertissements.  
  
-   Ajoute un zéro ou plusieurs enregistrements d’état qui contiennent des SQLSTATE pour l’erreur ou l’avertissement.  
  
-   Définit les champs de numéro de ligne et de colonne dans les enregistrements d’état. Si **SQLFetch** ne peut pas déterminer un nombre de lignes ou de colonnes, il définit ce nombre pour SQL_ROW_NUMBER_UNKNOWN ou SQL_COLUMN_NUMBER_UNKNOWN, respectivement. Si l’enregistrement d’état ne s’applique pas à une colonne particulière, **SQLFetch** définit le numéro de colonne à SQL_NO_COLUMN_NUMBER.  
  
 **SQLFetch** se poursuit jusqu'à ce qu’il a extrait toutes les lignes dans l’ensemble de lignes, l’extraction de lignes. Il retourne SQL_SUCCESS_WITH_INFO, sauf si une erreur se produit dans chaque ligne de l’ensemble de lignes (sans compter les lignes avec l’état SQL_ROW_NOROW), auquel cas il retourne SQL_ERROR. En particulier, si la taille de l’ensemble de lignes est 1 et une erreur se produit dans cette ligne, **SQLFetch** retourne SQL_ERROR.  
  
 **SQLFetch** retourne les enregistrements d’état dans l’ordre de numéro de ligne. Autrement dit, il retourne tous les enregistrements d’état pour les lignes inconnues (le cas échéant) ; Ensuite, il retourne tous les enregistrements d’état pour la première ligne (le cas échéant), et il retourne tous les enregistrements d’état pour la deuxième ligne (le cas échéant) et ainsi de suite. Les enregistrements d’état pour chaque ligne sont classés selon les règles normales pour le classement des enregistrements d’état ; Pour plus d’informations, consultez « Séquence d’enregistrements d’état » dans [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
### <a name="descriptors-and-sqlfetch"></a>Descripteurs et SQLFetch  
 Les sections suivantes décrivent comment **SQLFetch** interagit avec des descripteurs.  
  
#### <a name="argument-mappings"></a>Mappages d’argument  
 Le pilote ne définit pas de tous les champs de descripteur basées sur les arguments de **SQLFetch**.  
  
#### <a name="other-descriptor-fields"></a>Autres champs de descripteur  
 Les champs de descripteur suivants sont utilisés par **SQLFetch**.  
  
|Champ de descripteur|DESC.|Champ de|Défini par le biais|  
|----------------------|-----------|--------------|-----------------|  
|SQL_DESC_ARRAY_SIZE|ARD|En-tête|Attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE|  
|SQL_DESC_ARRAY_STATUS_PTR|IRD|En-tête|Attribut d’instruction SQL_ATTR_ROW_STATUS_PTR|  
|SQL_DESC_BIND_OFFSET_PTR|ARD|En-tête|Attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR|  
|SQL_DESC_BIND_TYPE|ARD|En-tête|Attribut d’instruction SQL_ATTR_ROW_BIND_TYPE|  
|SQL_DESC_COUNT|ARD|En-tête|*ColumnNumber* argument de **SQLBindCol**|  
|SQL_DESC_DATA_PTR|ARD|enregistrements|*TargetValuePtr* argument de **SQLBindCol**|  
|SQL_DESC_INDICATOR_PTR|ARD|enregistrements|*StrLen_or_IndPtr* argument dans **SQLBindCol**|  
|SQL_DESC_OCTET_LENGTH|ARD|enregistrements|*BufferLength* argument dans **SQLBindCol**|  
|SQL_DESC_OCTET_LENGTH_PTR|ARD|enregistrements|*StrLen_or_IndPtr* argument dans **SQLBindCol**|  
|SQL_DESC_ROWS_PROCESSED_PTR|IRD|En-tête|Attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR|  
|SQL_DESC_TYPE|ARD|enregistrements|*TargetType* argument dans **SQLBindCol**|  
  
 Tous les champs de descripteur peuvent également être définies via **SQLSetDescField**.  
  
#### <a name="separate-length-and-indicator-buffers"></a>Séparer la longueur et les mémoires tampons d’indicateur  
 Les applications peuvent lier une seule mémoire tampon ou les deux mémoires tampons distinctes qui peuvent être utilisés pour stocker des valeurs de longueur et l’indicateur. Lorsqu’une application appelle **SQLBindCol**, le pilote définit les champs SQL_DESC_OCTET_LENGTH_PTR et SQL_DESC_INDICATOR_PTR de la ARD à la même adresse, ce qui est passée par le *StrLen_or_IndPtr* argument. Lorsqu’une application appelle **SQLSetDescField** ou **SQLSetDescRec**, il peut définir ces deux champs à des adresses différentes.  
  
 **SQLFetch** détermine si l’application a spécifié les mémoires tampons de longueur et l’indicateur distincts. Dans ce cas, lorsque les données ne sont pas NULL, **SQLFetch** définit la mémoire tampon d’indicateur à 0 et retourne la longueur de la mémoire tampon de longueur. Lorsque les données sont NULL, **SQLFetch** définit la mémoire tampon d’indicateur avec la valeur SQL_NULL_DATA et ne modifie pas la mémoire tampon de longueur.  
  
### <a name="code-example"></a>Exemple de code  
 Consultez [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md), et [SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md).  
  
### <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[SQLDescribeCol, fonction](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Fermeture du curseur sur l’instruction|[SQLFreeStmt, fonction](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|L’extraction de tout ou partie d’une colonne de données|[SQLGetData, fonction](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Définie les colonnes de retour du nombre de résultats|[SQLNumResultCols, fonction](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Préparation de l’exécution d’une instruction|[SQLPrepare, fonction](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
