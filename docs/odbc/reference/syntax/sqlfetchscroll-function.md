---
title: Fonction SQLFetchScroll | Documents Microsoft
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
- SQLFetchScroll
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLFetchScroll
helpviewer_keywords:
- SQLFetchScroll function [ODBC]
ms.assetid: c0243667-428c-4dda-ae91-3c307616a1ac
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 439639255cc41fc22f94c5a1605dee8fc68a06ad
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlfetchscroll-function"></a>Fonction SQLFetchScroll
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLFetchScroll** extrait l’ensemble spécifié de lignes de données du jeu de résultats et retourne les données pour toutes les colonnes liées. Ensembles de lignes peut être spécifié à une position relative ou absolue ou par signet.  
  
 Lorsque vous travaillez avec du pilote ODBC 2.x, le Gestionnaire de pilotes est mappé à cette fonction pour **SQLExtendedFetch**. Pour plus d’informations, consultez [mappage des fonctions de remplacement pour la compatibilité descendante des Applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLFetchScroll(  
      SQLHSTMT      StatementHandle,  
      SQLSMALLINT   FetchOrientation,  
      SQLLEN        FetchOffset);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *FetchOrientation*  
 [Entrée]  
  
 Type d’extraction :  
  
 SQL_FETCH_NEXT  
  
 SQL_FETCH_PRIOR  
  
 SQL_FETCH_FIRST  
  
 SQL_FETCH_LAST  
  
 SQL_FETCH_ABSOLUTE  
  
 SQL_FETCH_RELATIVE  
  
 SQL_FETCH_BOOKMARK  
  
 Pour plus d’informations, consultez « Positionnement du curseur » dans la section « Commentaires ».  
  
 *FetchOffset*  
 [Entrée]  
  
 Numéro de la ligne à récupérer. L’interprétation de cet argument dépend de la valeur de la *FetchOrientation* argument. Pour plus d’informations, consultez « Positionnement du curseur » dans la section « Commentaires ».  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLFetchScroll** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un HandleType égal à SQL_HANDLE_STMT et un gérer au paramètre StatementHandle. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLFetchScroll** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire. Si une erreur se produit sur une colonne unique, **SQLGetDiagField** peut être appelé avec un DiagIdentifier de SQL_DIAG_COLUMN_NUMBER pour déterminer la colonne de l’erreur s’est produite ; et **SQLGetDiagField** peut être appelé avec un DiagIdentifier de SQL_DIAG_ROW_NUMBER pour déterminer la ligne qui contient cette colonne.  
  
 Pour toutes ces SQLSTATE qui peut retourner SQL_SUCCESS_WITH_INFO ou SQL_ERROR (sauf 01xxx SQLSTATE), SQL_SUCCESS_WITH_INFO est retourné si une erreur se produit sur un ou plusieurs, mais pas toutes, les lignes d’une opération de plusieurs ligne, SQL_ERROR est retourné si une erreur se produit lors d’une opération de ligne unique.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Retourné pour une colonne de données binary ou String a entraîné la troncation des caractères non vides ou les données binaires non NULL. S’il s’agissait d’une valeur de chaîne, il a été tronqué à la droite.|  
|01 S 01|Erreur de ligne|Une erreur s’est produite lors de l’extraction d’une ou plusieurs lignes.<br /><br /> (Si cet valeur SQLSTATE est retourné lorsqu’une ODBC 3 *.x* application fonctionne avec une API ODBC 2 *.x* pilote, il peut être ignoré.)|  
|01S06|Tentative de récupération avant que le jeu de résultats renvoyé le premier ensemble de lignes|L’ensemble de lignes demandé avec chevauchement le début du jeu de résultats si FetchOrientation était SQL_FETCH_PRIOR, la position actuelle a été au-delà de la première ligne, et le numéro de la ligne actuelle est inférieure ou égale à la taille de l’ensemble de lignes.<br /><br /> L’ensemble de lignes demandé avec chevauchement le début du jeu de résultats si FetchOrientation SQL_FETCH_PRIOR, la position actuelle a été au-delà de la fin du jeu de résultats, et la taille de l’ensemble de lignes était supérieure à la taille du jeu de résultats.<br /><br /> L’ensemble de lignes demandé avec chevauchement le début du jeu de résultats si FetchOrientation était SQL_FETCH_RELATIVE, FetchOffset était négative, et la valeur absolue de FetchOffset était inférieur ou égal à la taille de l’ensemble de lignes.<br /><br /> L’ensemble de lignes demandé avec chevauchement le début du jeu de résultats si FetchOrientation était SQL_FETCH_ABSOLUTE, FetchOffset était négative, et la valeur absolue de FetchOffset était supérieure à la taille du jeu de résultats, mais inférieur ou égal à la taille de l’ensemble de lignes.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01 S 07|Troncation fractionnelle|Les données retournées pour une colonne a été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Heure, timestamp, intervalle types de données et qui contient un composant d’heure, la partie fractionnaire du temps a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07006|Violation de l’attribut de type de données restreint|La valeur des données d’une colonne dans le jeu de résultats n’a pas pu être convertie au type de données spécifié par *TargetType* dans **SQLBindCol**.<br /><br /> La colonne 0 a été liée avec un type de données de SQL_C_BOOKMARK, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a pris la valeur SQL_UB_VARIABLE.<br /><br /> La colonne 0 a été liée avec un type de données de SQL_C_VARBOOKMARK, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS n’a pas été définie à SQL_UB_VARIABLE.|  
|07009|Index de descripteur non valide|Le pilote a été un ODBC 2 *.x* pilote qui ne prend pas en charge **SQLExtendedFetch**, et un numéro de colonne spécifié dans la liaison d’une colonne a été 0.<br /><br /> La colonne 0 a été liée, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été définie sur SQL_UB_OFF.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|22001|Données de type chaîne, droite tronquées|Un signet de longueur variable retourné pour une colonne a été tronqué.|  
|22002|Variable indicateur requise mais non fournie|Données de type NULL a été lue dans une colonne dont *StrLen_or_IndPtr* définie par **SQLBindCol** (ou SQL_DESC_INDICATOR_PTR définie par **SQLSetDescField** ou **SQLSetDescRec**) était un pointeur null.|  
|22003|Valeur numérique hors limites|Renvoi d’une valeur numérique (comme numérique ou chaîne) pour une ou plusieurs colonnes dépendantes aurait entraîné la partie entière (par opposition aux fractions de seconde) le nombre à tronquer.<br /><br /> Pour plus d’informations, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) dans [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22007|Format datetime non valide|Une colonne de caractères dans le jeu de résultats a été liée à une date, heure ou une structure d’horodateur C, et une valeur dans la colonne a été, respectivement, une date non valide, heure ou timestamp.|  
|22012|Division par zéro|Une valeur à partir d’une expression arithmétique a été retournée, ce qui a entraîné de division par zéro.|  
|22015|Dépassement du champ Intervalle|Affectation d’une valeur numérique exacte ou d’intervalle de type SQL à un type d’intervalle C a provoqué une perte de chiffres significatifs dans le champ Début.<br /><br /> Lors de la récupération des données à un type d’intervalle C, il n’a pas de représentation de la valeur du type SQL dans le type d’intervalle C.|  
|22018|Valeur de caractère non valide pour la spécification de la casse|Une colonne de caractères dans le jeu de résultats a été liée à une mémoire tampon de caractères C, et la colonne contenue un caractère pour lequel il n’existe aucune représentation dans le jeu de caractères de la mémoire tampon.<br /><br /> Le type C a été un numérique exact ou approximatif, une valeur datetime ou un type de données intervalle ; le type SQL de la colonne a un type de données caractère. et la valeur dans la colonne n’est pas un littéral valid du type C dépendant.|  
|24000|État de curseur non valide|Le *au paramètre StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats a été associé à la *au paramètre StatementHandle*.|  
|40001|Échec de la sérialisation|La transaction dans laquelle l’extraction a été exécutée a été arrêtée pour empêcher tout interblocage.|  
|40003|Saisie semi-automatique des instructions inconnue|Échec de la connexion associée lors de l’exécution de cette fonction, et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLFetchScroll** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM) spécifié *au paramètre StatementHandle* n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.<br /><br /> (DM) **SQLFetch** a été appelé pour le *au paramètre StatementHandle* après **SQLExtendedFetch** a été appelée et avant **SQLFreeStmt** avec la SQL_CLOSE option a été appelée.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|L’attribut d’instruction SQL_ATTR_USE_BOOKMARK a été défini sur SQL_UB_VARIABLE, et la colonne 0 a été liée à une mémoire tampon dont la longueur n’était pas égale à la longueur maximale du signet pour ce jeu de résultats. (Cette longueur est disponible dans le champ SQL_DESC_OCTET_LENGTH de l’IRD et peut être obtenue en appelant **SQLDescribeCol**, **SQLColAttribute**, ou **SQLGetDescField**.)|  
|HY106|Type de récupération hors plage|DM) la valeur spécifiée pour l’argument FetchOrientation n’était pas valide.<br /><br /> (DM) l’argument FetchOrientation a été SQL_FETCH_BOOKMARK et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a pris la valeur SQL_UB_OFF.<br /><br /> La valeur de l’attribut d’instruction SQL_ATTR_CURSOR_TYPE était SQL_CURSOR_FORWARD_ONLY et la valeur de l’argument que FetchOrientation n’a pas été SQL_FETCH_NEXT.<br /><br /> La valeur de l’attribut d’instruction SQL_ATTR_CURSOR_SCROLLABLE était SQL_NONSCROLLABLE et la valeur de l’argument que FetchOrientation n’a pas été SQL_FETCH_NEXT.|  
|HY107|Valeur de ligne hors limites|La valeur spécifiée avec l’attribut d’instruction SQL_ATTR_CURSOR_TYPE SQL_CURSOR_KEYSET_DRIVEN mais la valeur spécifiée avec l’attribut d’instruction SQL_ATTR_KEYSET_SIZE était supérieur à 0 et inférieur à la valeur spécifiée avec l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.|  
|HY111|Valeur de signet non valide|L’argument FetchOrientation a été SQL_FETCH_BOOKMARK, et le signet vers lequel pointé la valeur de l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR n’était pas valide ou était un pointeur null.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La source de données ou le pilote ne prend pas en charge la conversion spécifiée par la combinaison de la *TargetType* dans **SQLBindCol** et le type de données SQL de la colonne correspondante.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant la source de données a retourné le jeu de résultat demandé. Le délai d’expiration est défini via SQLSetStmtAttr, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLFetchScroll** renvoie un ensemble de lignes spécifié dans le jeu de résultats. Ensembles de lignes peut être spécifié par la position relative ou absolue ou par signet. **SQLFetchScroll** peut être appelée uniquement pendant un jeu de résultats existe, autrement dit, après un appel qui crée un jeu de résultats et avant le curseur sur ensemble de résultats est fermé. Si toutes les colonnes sont liés, il retourne les données dans ces colonnes. Si l’application a spécifié un pointeur vers un tableau d’état de ligne ou une mémoire tampon dans lequel retourner le nombre de lignes extraites, **SQLFetchScroll** retourne également ces informations. Les appels à **SQLFetchScroll** peuvent être mélangés avec les appels de **SQLFetch** mais ne peuvent pas être mélangés avec les appels de **SQLExtendedFetch**.  
  
 Pour plus d’informations, consultez [à l’aide de curseurs de bloc](../../../odbc/reference/develop-app/using-block-cursors.md) et [curseurs de défilement à l’aide de](../../../odbc/reference/develop-app/using-scrollable-cursors.md).  
  
## <a name="positioning-the-cursor"></a>Positionnement du curseur  
 Lorsque le jeu de résultats est créé, le curseur est positionné avant le début du jeu de résultats. **SQLFetchScroll** positionne le curseur de bloc basé sur les valeurs de la *FetchOrientation* et *FetchOffset* arguments, comme indiqué dans le tableau suivant. Les règles exactes pour déterminer le début de l’ensemble de nouvelles lignes sont affichées dans la section suivante.  
  
|FetchOrientation|Signification|  
|----------------------|-------------|  
|SQL_FETCH_NEXT|Retourne l’ensemble de lignes suivant. Cela équivaut à appeler la méthode **SQLFetch**.<br /><br /> **SQLFetchScroll** ignore la valeur de *FetchOffset*.|  
|SQL_FETCH_PRIOR|Retourne l’ensemble de lignes précédente.<br /><br /> **SQLFetchScroll** ignore la valeur de *FetchOffset*.|  
|SQL_FETCH_RELATIVE|Retourner l’ensemble de lignes *FetchOffset* à partir du début de l’ensemble de lignes en cours.|  
|SQL_FETCH_ABSOLUTE|Retourner l’ensemble de lignes en commençant à la ligne *FetchOffset*.|  
|SQL_FETCH_FIRST|Retourne le premier ensemble de lignes du jeu de résultats.<br /><br /> **SQLFetchScroll** ignore la valeur de *FetchOffset*.|  
|SQL_FETCH_LAST|Retourne le dernier ensemble de lignes du jeu de résultats.<br /><br /> **SQLFetchScroll** ignore la valeur de *FetchOffset*.|  
|SQL_FETCH_BOOKMARK|Retourne l’ensemble de lignes FetchOffset lignes à partir du signet spécifié par l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR.|  
  
 Pilotes ne sont pas requis pour prendre en charge toutes les orientations d’extraction ; une application appelle **SQLGetInfo** avec un type d’information de SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 ou SQL_STATIC_CURSOR_ATTRIBUTES1 (selon le type du curseur) pour déterminer les orientations d’extraction sont prises en charge par le pilote. L’application doit examiner les masques de bits SQL_CA1_NEXT, SQL_CA1_RELATIVE, SQL_CA1_ABSOLUTE et WQL_CA1_BOOKMARK dans ces types d’informations. En outre, si le curseur est avant uniquement et FetchOrientation n’est pas SQL_FETCH_NEXT, **SQLFetchScroll** retourne SQLSTATE HY106 (type de récupération hors plage).  
  
 L’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE Spécifie le nombre de lignes dans l’ensemble de lignes. Si l’ensemble de lignes à extraire par **SQLFetchScroll** chevauche la fin du jeu de résultats, **SQLFetchScroll** renvoie un ensemble de lignes partiel. Autrement dit, si S + R – 1 est supérieure à L, où S est la ligne de début de l’ensemble de lignes à extraire, R est la taille de l’ensemble de lignes et L est la dernière ligne du jeu de résultats, puis uniquement le premier L – S + 1 lignes de l’ensemble de lignes sont valides. Les lignes restantes sont vides et auront un état SQL_ROW_NOROW.  
  
 Après avoir **SQLFetchScroll** renvoie, la ligne actuelle est la première ligne de l’ensemble de lignes.  
  
## <a name="cursor-positioning-rules"></a>Règles de positionnement du curseur  
 Les sections suivantes décrivent les règles exactes pour chaque valeur du paramètre FetchOrientation. Ces règles utilisent la notation suivante.  
  
|Notation|Signification|  
|--------------|-------------|  
|*Avant de démarrer*|Le curseur de bloc est positionné avant le début du jeu de résultats. Si la première ligne de l’ensemble de lignes nouvelles est avant le début du jeu de résultats, **SQLFetchScroll** retourne SQL_NO_DATA.|  
|*Après la fin*|Le curseur de bloc est positionné après que la fin du résultat défini. Si la première ligne de l’ensemble de lignes nouvelles est après la fin du jeu de résultats, **SQLFetchScroll** retourne SQL_NO_DATA.|  
|*CurrRowsetStart*|Le numéro de la première ligne dans l’ensemble de lignes en cours.|  
|*LastResultRow*|Le numéro de la dernière ligne du jeu de résultats.|  
|*RowsetSize*|La taille de l’ensemble de lignes.|  
|*FetchOffset*|La valeur de la *FetchOffset* argument.|  
|*BookmarkRow*|La ligne correspondant au signet spécifié par l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR.|  
  
## <a name="sqlfetchnext"></a>SQL_FETCH_NEXT  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*Avant de démarrer*|1|  
|*CurrRowsetStart + la RowsetSize*[1]  *\<= LastResultRow*|*CurrRowsetStart + la RowsetSize*[1]|  
|*CurrRowsetStart + la RowsetSize*[1]*> LastResultRow*|*Après la fin*|  
|*Après la fin*|*Après la fin*|  
  
 [1] si la taille de l’ensemble de lignes a été modifiée depuis le précédent appel pour extraire les lignes, il s’agit de la taille de l’ensemble de lignes qui a été utilisée avec l’appel précédent.  
  
## <a name="sqlfetchprior"></a>SQL_FETCH_PRIOR  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*Avant de démarrer*|*Avant de démarrer*|  
|*CurrRowsetStart = 1*|*Avant de démarrer*|  
|*1 < CurrRowsetStart < = la RowsetSize* <sup>[2].</sup>|*1* <sup>[1]</sup>|  
|*CurrRowsetStart > La RowsetSize* <sup>[2]</sup>|*CurrRowsetStart – la RowsetSize* <sup>[2]</sup>|  
|*Après la fin et LastResultRow < la RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*Après la fin et LastResultRow > = La RowsetSize* <sup>[2]</sup>|*LastResultRow – la RowsetSize + 1* <sup>[2].</sup>|  
  
 [1] **SQLFetchScroll** retourne SQLSTATE 01S06 (tentative de récupération avant que le jeu de résultats renvoyé le premier ensemble de lignes) et SQL_SUCCESS_WITH_INFO.  
  
 [2] si la taille de l’ensemble de lignes a été modifiée depuis le précédent appel pour extraire les lignes, il s’agit de la nouvelle taille de l’ensemble de lignes.  
  
## <a name="sqlfetchrelative"></a>SQL_FETCH_RELATIVE  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*(Avant de démarrer et FetchOffset > 0) OU (après la fin et FetchOffset < 0)*|*--* <sup>[1]</sup>|  
|*BeforeStart et FetchOffset < = 0*|*Avant de démarrer*|  
|*CurrRowsetStart = 1 et FetchOffset < 0*|*Avant de démarrer*|  
|*CurrRowsetStart > 1 AND CurrRowsetStart + FetchOffset < 1 AND &#124; FetchOffset &#124; > La RowsetSize* <sup>[3]</sup>|*Avant de démarrer*|  
|*CurrRowsetStart > 1 AND CurrRowsetStart + FetchOffset < 1 AND &#124; FetchOffset &#124; < = la RowsetSize* <sup>[3]</sup>|*1* <sup>[2]</sup>|  
|*1 < = CurrRowsetStart + FetchOffset \<= LastResultRow*|*CurrRowsetStart + FetchOffset*|  
|*CurrRowsetStart + FetchOffset > LastResultRow*|*Après la fin*|  
|*Après la fin et FetchOffset > = 0*|*Après la fin*|  
  
 [1] ***SQLFetchScroll*** retourne le même ensemble de lignes comme si elle a été appelée avec FetchOrientation SQL_FETCH_ABSOLUTE la valeur. Pour plus d’informations, consultez la section « SQL_FETCH_ABSOLUTE ».  
  
 [2] **SQLFetchScroll** retourne SQLSTATE 01S06 (tentative de récupération avant que le jeu de résultats renvoyé le premier ensemble de lignes) et SQL_SUCCESS_WITH_INFO.  
  
 [3] si la taille de l’ensemble de lignes a été modifiée depuis le précédent appel pour extraire les lignes, il s’agit de la nouvelle taille de l’ensemble de lignes.  
  
## <a name="sqlfetchabsolute"></a>SQL_FETCH_ABSOLUTE  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*FetchOffset < 0 AND &#124; FetchOffset &#124; < = LastResultRow*|*LastResultRow + FetchOffset + 1*|  
|*FetchOffset < 0 AND &#124; FetchOffset &#124; > LastResultRow AND &#124; FetchOffset &#124; > La RowsetSize* <sup>[2]</sup>|*Avant de démarrer*|  
|*FetchOffset < 0 AND &#124; FetchOffset &#124; > LastResultRow AND &#124; FetchOffset &#124; < = la RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*FetchOffset = 0*|*Avant de démarrer*|  
|*1 < = FetchOffset \<= LastResultRow*|*FetchOffset*|  
|*FetchOffset > LastResultRow*|*Après la fin*|  
  
 [1] **SQLFetchScroll** retourne SQLSTATE 01S06 (tentative de récupération avant que le jeu de résultats renvoyé le premier ensemble de lignes) et SQL_SUCCESS_WITH_INFO.  
  
 [2] si la taille de l’ensemble de lignes a été modifiée depuis le précédent appel pour extraire les lignes, il s’agit de la nouvelle taille de l’ensemble de lignes.  
  
 Une extraction absolue effectuée sur un curseur dynamique ne peut pas fournir le résultat requis, car les positions de ligne dans un curseur dynamique ne sont pas déterminées. Une telle opération est équivalente à une extraction tout d’abord suivie par une instruction fetch relative ; Il n’est pas une opération atomique, en l’état d’une extraction absolue sur un curseur statique.  
  
## <a name="sqlfetchfirst"></a>SQL_FETCH_FIRST  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*N’importe quel*|*1*|  
  
## <a name="sqlfetchlast"></a>SQL_FETCH_LAST  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*La RowsetSize* <sup>[1]</sup> < = LastResultRow|*LastResultRow – la RowsetSize + 1* <sup>[1]</sup>|  
|*La RowsetSize* <sup>[1]</sup> > LastResultRow|*1*|  
  
 [1] si la taille de l’ensemble de lignes a été modifiée depuis le précédent appel pour extraire les lignes, il s’agit de la nouvelle taille de l’ensemble de lignes.  
  
## <a name="sqlfetchbookmark"></a>SQL_FETCH_BOOKMARK  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*BookmarkRow + FetchOffset < 1*|*Avant de démarrer*|  
|*1 < = BookmarkRow + FetchOffset \<= LastResultRow*|*BookmarkRow + FetchOffset*|  
|*BookmarkRow + FetchOffset > LastResultRow*|*Après la fin*|  
  
 Pour plus d’informations sur les signets, consultez [signets (ODBC)](../../../odbc/reference/develop-app/bookmarks-odbc.md).  
  
## <a name="effect-of-deleted-added-and-error-rows-on-cursor-movement"></a>Effet de supprimés, ajoutés et les lignes d’erreur sur le déplacement du curseur  
 Parfois, les curseurs statiques et pilotés par détectent lignes ajoutées au résultat et de supprimer les lignes supprimées du jeu de résultats. En appelant **SQLGetInfo** avec les options SQL_STATIC_CURSOR_ATTRIBUTES2 et SQL_KEYSET_CURSOR_ATTRIBUTES2 et en examinant les masques de bits SQL_CA2_SENSITIVITY_ADDITIONS, SQL_CA2_SENSITIVITY_DELETIONS et SQL_CA2_SENSITIVITY_UPDATES, une application détermine si les curseurs implémentés par un pilote spécifique pour cela. Pour les pilotes qui peuvent détecter les lignes supprimées et les supprimer, les paragraphes suivants décrivent les effets de ce comportement. Pour les pilotes qui peuvent détecter les lignes supprimées, mais ne peut pas les supprimer, suppressions n’ont aucun effet sur les mouvements de curseur, et les paragraphes suivants ne s’appliquent pas.  
  
 Si le curseur détecte les lignes ajoutées au jeu de résultats ou supprime des lignes supprimées du jeu de résultats, il apparaît comme s’il détecte ces modifications uniquement lorsqu’il extrait les données. Cela inclut le cas lorsque **SQLFetchScroll** est appelée avec FetchOrientation définie sur SQL_FETCH_RELATIVE et FetchOffset d’extraire le même ensemble de lignes à la valeur 0, mais n’inclut pas le cas lorsque SQLSetPos est appelée avec fOption défini à SQL_REFRESH. Dans ce cas, les données dans les tampons de l’ensemble de lignes sont actualisées, mais pas de nouveau extraits et supprimés les lignes ne sont pas supprimés du jeu de résultats. Par conséquent, lorsqu’une ligne est supprimée à partir d’ou insérée dans l’ensemble de lignes en cours, le curseur ne modifie pas les tampons de l’ensemble de lignes. Au lieu de cela, il détecte la modification lorsqu’il lit tout ensemble de lignes précédemment inclus la ligne supprimée ou inclut désormais la ligne insérée.  
  
 Par exemple :  
  
```  
// Fetch the next rowset.  
SQLFetchScroll(hstmt, SQL_FETCH_NEXT, 0);  
// Delete third row of the rowset. Does not modify the rowset buffers.  
SQLSetPos(hstmt, 3, SQL_DELETE, SQL_LOCK_NO_CHANGE);  
// The third row has a status of SQL_ROW_DELETED after this call.  
SQLSetPos(hstmt, 3, SQL_REFRESH, SQL_LOCK_NO_CHANGE);  
// Refetch the same rowset. The third row is removed, replaced by what  
// was previously the fourth row.  
SQLFetchScroll(hstmt, SQL_FETCH_RELATIVE, 0);  
```  
  
 Lorsque **SQLFetchScroll** retourne un nouvel ensemble de lignes qui a une position par rapport à l’ensemble de lignes en cours : FetchOrientation est SQL_FETCH_NEXT, SQL_FETCH_PRIOR ou SQL_FETCH_RELATIVE : il n’inclut pas les modifications apportées à l’ensemble de lignes en cours lors du calcul de la position de départ de l’ensemble de lignes de nouveau. Toutefois, il inclut des modifications à l’extérieur de l’ensemble de lignes en cours si elle est capable de détecter les. En outre, lorsque **SQLFetchScroll** retourne un nouvel ensemble de lignes qui a une position indépendante de l’ensemble de lignes en cours : FetchOrientation est SQL_FETCH_FIRST, SQL_FETCH_LAST, SQL_FETCH_ABSOLUTE ou SQL_FETCH_BOOKMARK, elle inclut toutes les modifications, il est capable de détecter, même s’ils se trouvent dans l’ensemble de lignes en cours.  
  
 Pour déterminer si les nouvelles lignes ajoutées sont à l’intérieur ou à l’extérieur de l’ensemble de lignes en cours, un ensemble de lignes partiel est considéré comme se terminer à la dernière ligne valide ; Autrement dit, la dernière ligne pour laquelle l’état de la ligne n’est pas SQL_ROW_NOROW. Par exemple, supposons que le curseur est capable de détecter les nouvelles lignes ajoutées, l’ensemble de lignes en cours est un ensemble de lignes partiel, l’application ajoute de nouvelles lignes et le curseur ajoute ces lignes à la fin du jeu de résultats. Si l’application appelle **SQLFetchScroll** avec FetchOrientation SQL_FETCH_NEXT, la valeur **SQLFetchScroll** retourne l’ensemble de lignes en commençant par la première ligne qui vient d’être ajoutée.  
  
 Par exemple, supposons que l’ensemble de lignes en cours comporte des lignes 21 à 30, la taille de l’ensemble de lignes est 10, le curseur supprime les lignes supprimées du jeu de résultats et le curseur détecte les lignes ajoutées au jeu de résultats. Le tableau suivant présente les lignes **SQLFetchScroll** retourne dans différentes situations.  
  
|Modifier|Type d’extraction|FetchOffset|Nouvel ensemble de lignes [1]|  
|------------|----------------|-----------------|---------------------|  
|Supprimer une ligne 21|NEXT|0|31 à 40|  
|Supprimer une ligne 31|NEXT|0|32 à 41|  
|Insérer une ligne entre les lignes 21 et 22|NEXT|0|31 à 40|  
|Insérer une ligne entre les lignes 30 et 31|NEXT|0|Ligne insérée, 31 à 39|  
|Supprimer une ligne 21|PRIOR|0|11 à 20|  
|Supprimer une ligne 20|PRIOR|0|10 à 19.|  
|Insérer une ligne entre les lignes 21 et 22|PRIOR|0|11 à 20|  
|Insérer une ligne entre les lignes 20 et 21|PRIOR|0|ligne insérée de 12 à 20,|  
|Supprimer une ligne 21|RELATIVE|0|22 à 31<sup>[2]</sup>|  
|Supprimer une ligne 21|RELATIVE|1|22 à 31.|  
|Insérer une ligne entre les lignes 21 et 22|RELATIVE|0|21, ligne insérée, 22 à 29|  
|Insérer une ligne entre les lignes 21 et 22|RELATIVE|1|22 à 31.|  
|Supprimer une ligne 21|ABSOLUTE|21|22 à 31<sup>[2]</sup>|  
|Supprimer une ligne 22|ABSOLUTE|21|21, 23 et 31.|  
|Insérer une ligne entre les lignes 21 et 22|ABSOLUTE|22|Ligne insérée, 22 à 29|  
  
 [1] cette colonne utilise les numéros de ligne avant que toutes les lignes ont été insérées ou supprimées.  
  
 [2] dans ce cas, le curseur tente de retourner les lignes commençant par 21. Étant donné que la ligne 21 a été supprimée, la première ligne, qu'elle retourne est ligne 22.  
  
 Lignes d’erreur (autrement dit, lignes avec un état de SQL_ROW_ERROR) n’affectent pas le déplacement du curseur. Par exemple, si l’ensemble de lignes en cours commence par la ligne 11 et l’état de la ligne 11 est SQL_ROW_ERROR, l’appel **SQLFetchScroll** avec FetchOrientation SQL_FETCH_RELATIVE et FetchOffset la valeur définie à 5 retourne l’ensemble de lignes en commençant par la ligne 16, comme il le ferait si l’état de la ligne 11 SQL_SUCCESS.  
  
## <a name="returning-data-in-bound-columns"></a>Renvoi de données dans les colonnes dépendantes  
 **SQLFetchScroll** retourne des données dans les colonnes dépendantes de la même façon que **SQLFetch**. Pour plus d’informations, voir « renvoi dans lié colonnes de données » dans [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
 Si aucune colonne n’est liée, **SQLFetchScroll** ne retourne pas de données mais déplace le curseur de bloc à la position spécifiée. Indique si les données peuvent être récupérées à partir d’un curseur de bloc avec des colonnes indépendantes **SQLGetData** dépend du pilote. Cette fonctionnalité est prise en charge si un appel à **SQLGetInfo** retourne le bit pour le type d’informations SQL_GETDATA_EXTENSIONS de SQL_GD_BLOCK.  
  
## <a name="buffer-addresses"></a>Adresses de mémoire tampon  
 **SQLFetchScroll** utilise la même formule pour déterminer l’adresse des mémoires tampons de données et de longueur / d’indicateur en tant que **SQLFetch**. Pour plus d’informations, consultez « Adresses mémoire tampon » dans [fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
## <a name="row-status-array"></a>Tableau d’état de ligne  
 **SQLFetchScroll** définit des valeurs dans le tableau d’état de ligne de la même manière que SQLFetch. Pour plus d’informations, consultez « Tableau d’état de ligne » dans [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="rows-fetched-buffer"></a>Mémoire tampon d’extraction de lignes  
 **SQLFetchScroll** retourne le nombre de lignes lues dans la mémoire tampon de lignes extraites de la même manière que **SQLFetch**. Pour plus d’informations, consultez « Lignes lues la mémoire tampon » dans [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Lorsqu’une application appelle **SQLFetchScroll** dans une version 3.x du pilote ODBC, le Gestionnaire de pilotes appelle **SQLFetchScroll** dans le pilote. Lorsqu’une application appelle **SQLFetchScroll** dans une version 2.x du pilote ODBC, le Gestionnaire de pilotes appelle SQLExtendedFetch dans le pilote. Étant donné que **SQLFetchScroll** et SQLExtendedFetch gérer les erreurs de manière légèrement différente, l’application ne voit le comportement légèrement différentes erreurs lorsqu’il appelle **SQLFetchScroll** dans ODBC 2.x et 3.x ODBC (pilotes).  
  
 **SQLFetchScroll** renvoie les erreurs et avertissements de la même manière que **SQLFetch**; pour plus d’informations, consultez « Gestion des erreurs » dans **SQLFetch**. **SQLExtendedFetch** retourne des erreurs de la même manière que **SQLFetch**, avec les exceptions suivantes :  
  
 Lorsqu’un avertissement se produit qui s’applique à une ligne particulière dans l’ensemble de lignes, SQLExtendedFetch définit l’entrée correspondante dans le tableau d’état de ligne à SQL_ROW_SUCCESS, SQL_ROW_SUCCESS_WITH_INFO pas.  
  
 En cas d’erreurs dans chaque ligne dans l’ensemble de lignes, SQLExtendedFetch retourne SQL_SUCCESS_WITH_INFO, pas SQL_ERROR.  
  
 Dans chaque groupe d’enregistrements d’état qui s’applique à une ligne spécifique, le premier enregistrement d’état retourné par SQLExtendedFetch doit contenir 01 s 01 SQLSTATE (erreur de ligne) ; **SQLFetchScroll** ne retourne pas ce SQLSTATE. Si SQLExtendedFetch ne peut pas retourner de SQLSTATE supplémentaires, elle doit toujours retourner ce SQLSTATE.  
  
## <a name="sqlfetchscroll-and-optimistic-concurrency"></a>SQLFetchScroll et accès concurrentiel optimiste  
 Si un curseur utilise l’accès concurrentiel optimiste, autrement dit, l’attribut d’instruction SQL_ATTR_CONCURRENCY a une valeur SQL_CONCUR_VALUES ou SQL_CONCUR_ROWVER : **SQLFetchScroll** met à jour les valeurs d’accès concurrentiel optimiste utilisés par la source de données pour détecter si une ligne a changé. Cela se produit chaque fois que **SQLFetchScroll** extrait un nouvel ensemble de lignes, y compris lorsqu’il réextrait l’ensemble de lignes en cours. (Elle est appelée avec FetchOrientation définie sur SQL_FETCH_RELATIVE et FetchOffset la valeur 0.)  
  
## <a name="sqlfetchscroll-and-odbc-2x-drivers"></a>Pilotes 2.x SQLFetchScroll et ODBC  
 Lorsqu’une application appelle **SQLFetchScroll** dans une version 2.x du pilote ODBC, le Gestionnaire de pilotes est mappé à cet appel à **SQLExtendedFetch**. Il transmet les valeurs suivantes pour les arguments de **SQLExtendedFetch**.  
  
|Argument de SQLExtendedFetch|Valeur|  
|-------------------------------|-----------|  
|Au paramètre StatementHandle|Au paramètre StatementHandle dans **SQLFetchScroll**.|  
|FetchOrientation|FetchOrientation dans **SQLFetchScroll**.|  
|FetchOffset|Si FetchOrientation n’est pas SQL_FETCH_BOOKMARK, la valeur de l’argument FetchOffset dans **SQLFetchScroll** est utilisé.<br /><br /> Si FetchOrientation est SQL_FETCH_BOOKMARK, la valeur stockée à l’adresse spécifiée par l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR est utilisée.|  
|RowCountPtr|L’adresse spécifiée par l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.|  
|RowStatusArray|L’adresse spécifiée par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.|  
  
 Pour plus d’informations, consultez [curseurs de bloc, les curseurs permettant le défilement et la compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) dans l’annexe g : pilote recommandations pour la compatibilité descendante.  
  
## <a name="descriptors-and-sqlfetchscroll"></a>Descripteurs et SQLFetchScroll  
 **SQLFetchScroll** interagit avec les descripteurs de la même manière que **SQLFetch**. Pour plus d’informations, consultez la section « Descripteurs et SQLFetchScroll » dans [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [liaison selon les colonnes](../../../odbc/reference/develop-app/column-wise-binding.md), [liaison selon les lignes](../../../odbc/reference/develop-app/row-wise-binding.md), [positionné les instructions Update et Delete](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md), et [mise à jour des lignes dans l’ensemble de lignes avec SQLSetPos](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Exécution d’opérations de suppression, mise à jour ou insertion en bloc|[SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[SQLDescribeCol, fonction](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|L’extraction d’une seule ligne ou un bloc de données dans une direction avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Fermeture du curseur sur l’instruction|[SQLFreeStmt, fonction](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Définie les colonnes de retour du nombre de résultats|[SQLNumResultCols, fonction](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Positionnement du curseur, l’actualisation des données dans l’ensemble de lignes ou de mise à jour ou de suppression de données dans le jeu de résultats|[SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Définition d’un attribut d’instruction|[SQLSetStmtAttr, fonction](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
