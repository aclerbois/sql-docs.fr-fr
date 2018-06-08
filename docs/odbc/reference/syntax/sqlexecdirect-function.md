---
title: SQLExecDirect, fonction | Documents Microsoft
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
- SQLExecDirect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLExecDirect
helpviewer_keywords:
- SQLExecDirect function [ODBC]
ms.assetid: 985fcee1-f204-425c-bdd1-deb0e7d7bbd9
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e6af2069f900c4844d6d16fad3b7b8b920894098
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlexecdirect-function"></a>SQLExecDirect, fonction
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLExecDirect** exécute une instruction préliminaire, en utilisant les valeurs actuelles des variables de marqueur de paramètre si tous les paramètres existent dans l’instruction. **SQLExecDirect** est le moyen le plus rapide pour envoyer une instruction SQL pour une exécution unique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLExecDirect(  
     SQLHSTMT     StatementHandle,  
     SQLCHAR *    StatementText,  
     SQLINTEGER   TextLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *StatementText*  
 [Entrée] Instruction SQL à exécuter.  
  
 *TextLength*  
 [Entrée] Longueur de **StatementText* en caractères.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR, SQL_NO_DATA, SQL_INVALID_HANDLE ou SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLExecDirect** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLExecDirect** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01001|Conflit d’opération de curseur|\**StatementText* contenus une position de mettre à jour ou de l’instruction delete, et aucune ligne ou plusieurs lignes ont été mis à jour ou supprimés. (Pour plus d’informations sur les mises à jour de plusieurs lignes, consultez la description de la SQL_ATTR_SIMULATE_CURSOR *attribut* dans **SQLSetStmtAttr**.)<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01003|Valeur NULL supprimée dans la fonction définie|L’argument *StatementText* contenait une fonction d’ensemble (tel que **AVG**, **MAX**, **MIN**, et ainsi de suite), mais pas les **nombre** définir une fonction et l’argument NULL valeurs ont été éliminés avant que la fonction a été appliquée. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Chaîne ou des données binaires retournées pour une entrée/sortie ou paramètre de sortie a entraîné la troncation des caractères non vides ou les données binaires non NULL. S’il s’agissait d’une valeur de chaîne, il a été tronqué à la droite. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01006|Privilège non révoqué|\**StatementText* contenait un **RÉVOQUER** instruction et l’utilisateur n’ont pas le privilège spécifié. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01007|Privilège n'accordé pas|*\*StatementText* a été un **GRANT** instruction et l’utilisateur n’ont pas reçu le privilège spécifié.|  
|01S02|Valeur de l’option modifiée|Un attribut d’instruction spécifiée non valide en raison de conditions de travail de mise en œuvre, donc une valeur similaire a été remplacée temporairement. (**SQLGetStmtAttr** peut être appelée pour déterminer quelle est la valeur substituée temporairement.) La valeur de remplacement n’est valide pour le *au paramètre StatementHandle* jusqu'à ce que le curseur est fermé, à quel point l’attribut d’instruction reprend sa valeur précédente. Les attributs d’instruction qui peuvent être modifiés sont :<br /><br /> SQL_ ATTR_CONCURRENCY SQL_ ATTR_CURSOR_TYPE SQL_ ATTR_KEYSET_SIZE SQL_ ATTR_MAX_LENGTH SQL_ ATTR_MAX_ROWS SQL_ ATTR_QUERY_TIMEOUT SQL_ ATTR_SIMULATE_CURSOR<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01 S 07|Troncation fractionnelle|Les données retournées pour une entrée/sortie ou paramètre de sortie a été tronqué de sorte que la partie fractionnaire d’un type de données numérique a été tronquée ou la partie fractionnaire du composant d’un type de données heure, timestamp ou intervalle de temps a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07002|Champ COUNT incorrect|Le nombre de paramètres spécifiés dans **SQLBindParameter** était inférieur au nombre de paramètres dans l’instruction SQL contenue dans \* *StatementText*.<br /><br /> **SQLBindParameter** a été appelée avec *ParameterValuePtr* un pointeur null, la valeur *StrLen_or_IndPtr* ne pas la valeur SQL_NULL_DATA ou SQL_DATA_AT_EXEC, et *InputOutputType*  ne pas la valeur SQL_PARAM_OUTPUT, afin que le nombre de paramètres spécifiés dans **SQLBindParameter** était supérieur au nombre de paramètres dans l’instruction SQL contenue dans **StatementText* .|  
|07006|Violation de l’attribut de type de données restreint|La valeur de données identifiée par le *ValueType* argument dans **SQLBindParameter** pour le paramètre dépendant n’a pas pu être converti au type de données identifié par le *ParameterType* argument dans **SQLBindParameter**.<br /><br /> La valeur des données retournées pour un paramètre en tant que SQL_PARAM_INPUT_OUTPUT ou SQL_PARAM_OUTPUT pas pu être convertie vers le type de données identifié par le *ValueType* argument dans **SQLBindParameter**.<br /><br /> (Si les valeurs de données pour une ou plusieurs lignes n’a pas pu être converties, mais une ou plusieurs lignes ont été retournés avec succès, cette fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07007|Violation de valeur de paramètre réservé|Le type de paramètre SQL_PARAM_INPUT_OUTPUT_STREAM est utilisé uniquement pour un paramètre qui envoie et reçoit des données dans les parties. Une entrée de la mémoire tampon dépendante n’est pas autorisée pour ce type de paramètre.<br /><br /> Cette erreur se produit lorsque le type de paramètre est SQL_PARAM_INPUT_OUTPUT et lorsque la \* *StrLen_or_IndPtr* spécifié dans **SQLBindParameter** n’est pas égal à SQL_NULL_DATA, SQL_DEFAULT_PARAM, SQL_LEN_DATA_AT_EXEC(len) ou SQL_DATA_AT_EXEC.|  
|07 S 01|Utilisation non valide du paramètre par défaut|Un paramètre défini avec **SQLBindParameter**, a été SQL_DEFAULT_PARAM, et le paramètre correspondant n’avait pas de valeur par défaut.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|21S01|Liste de valeurs d’insertion ne correspond pas à la liste des colonnes|\**StatementText* contenait un **insérer** instruction et le nombre de valeurs à insérer ne correspond pas au degré de la table dérivée.|  
|21S02|Degré de la table dérivée ne correspond pas à la liste des colonnes|\**StatementText* contenus un **CREATE VIEW** l’instruction et la liste de colonnes non qualifiés (le nombre de colonnes spécifié pour l’affichage dans le *identificateur de la colonne* arguments de l’instruction SQL instruction) contenait des noms plus que le nombre de colonnes dans la table dérivée définie par le *-spécification de la requête* argument de l’instruction SQL.|  
|22001|Chaîne de données sont tronquées à droite|L’affectation d’un caractère ou d’une valeur binaire à une colonne a entraîné la troncation des données de type caractère non vides ou les données binaires non null.|  
|22002|Variable indicateur requise mais non fournie|Données de type NULL a été liées à un paramètre de sortie dont *StrLen_or_IndPtr* définie par **SQLBindParameter** était un pointeur null.|  
|22003|Valeur numérique hors limites|**StatementText* contenait une instruction SQL qui contenait un littéral ou un paramètre numérique lié, et la valeur a provoqué la partie entière (par opposition aux fractions de seconde) le nombre à tronquer lorsque affecté à la colonne de table associée.<br /><br /> Renvoi d’une valeur numérique (comme numérique ou chaîne) pour un ou plusieurs paramètres d’entrée/sortie ou de sortie aurait entraîné la partie entière (par opposition aux fractions de seconde) le nombre à tronquer.|  
|22007|Format datetime non valide|**StatementText* contenait une instruction SQL contenant une date, heure ou une structure d’horodateur comme un paramètre dépendant, et le paramètre a été, respectivement, une date non valide, heure ou timestamp.<br /><br /> Un paramètre d’entrée/sortie ou de sortie a été lié à une date, heure ou une structure d’horodateur C, et une valeur dans le paramètre retournée était, respectivement, une date non valide, heure ou timestamp. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|22008|Dépassement du champ DateTime|**StatementText* contenu d’une instruction SQL contenant une expression datetime que, lorsque calculées, a entraîné une date, heure ou timestamp de la structure qui n’est pas valide.<br /><br /> Une expression datetime calculée pour une entrée/sortie ou paramètre de sortie a entraîné une date, heure ou une structure d’horodatage C n’est pas valide.|  
|22012|Division par zéro|**StatementText* contient une instruction SQL qui contenue une expression arithmétique ayant provoqué la division par zéro.<br /><br /> Une expression arithmétique calculée pour une entrée/sortie ou paramètre de sortie a entraîné de division par zéro.|  
|22015|Dépassement du champ Intervalle|*\*StatementText* contient un paramètre numérique ou intervalle exact qui, lorsque converti en un type de données SQL de l’intervalle, a entraîné une perte de chiffres significatifs.<br /><br /> *\*StatementText* contient un paramètre d’intervalle avec plus d’un champ que, lorsque converti en un type de données numériques dans une colonne, n’ont aucune représentation dans le type de données numérique.<br /><br /> *\*StatementText* contient des données de paramètre qui a été affectées à un intervalle de type SQL, et il n’a pas de représentation de la valeur de type C dans l’intervalle de type SQL.<br /><br /> Affectation d’un paramètre d’entrée/sortie ou de sortie qui a une valeur numérique exacte ou un intervalle de type SQL à un type d’intervalle C a entraîné une perte de chiffres significatifs.<br /><br /> Lorsqu’un paramètre d’entrée/sortie ou de sortie a été affecté à une structure d’intervalle C, il n’a pas de représentation des données dans la structure de données d’intervalle.|  
|22018|Valeur de caractère non valide pour la spécification de la casse|*\*StatementText* contenait un type C qui était un numérique exact ou approximatif, une valeur datetime ou un type de données d’intervalle ; le type SQL de la colonne a un type de données caractère ; et la valeur dans la colonne n’est pas un littéral valid du type C dépendant.<br /><br /> Lorsqu’un paramètre d’entrée/sortie ou de sortie a été retourné, le type SQL a été un numérique exact ou approximatif, une valeur datetime ou un type de données intervalle ; le type C a été SQL_C_CHAR ; et la valeur dans la colonne n’est pas un littéral valid du type SQL lié.|  
|22019|Caractère d’échappement non valide|\**StatementText* contenait une instruction SQL qui contenait un **comme** prédicat avec une **échappement** dans les **où** clause et la longueur de la séquence d’échappement caractère suivant **d’échappement** n’est pas égale à 1.|  
|22025|Séquence d’échappement non valide|\**StatementText* contenait une instruction SQL contenant «**comme** *valeur de modèle* **échappement** *caractère d’échappement* « dans le **où** clause et le caractère suivant le caractère d’échappement dans la valeur de modèle n’est pas une « % » ou « _ ».|  
|23000|Violation de contrainte d’intégrité|**StatementText* contenait une instruction SQL qui contenait un littéral ou un paramètre. La valeur du paramètre était NULL pour une colonne définie comme NOT NULL dans la colonne de table associée, une valeur en double a été fournie pour une colonne doit contenir uniquement des valeurs uniques ou une autre contrainte d’intégrité a été violée.|  
|24000|État de curseur non valide|Un curseur a été positionné sur le *au paramètre StatementHandle* par **SQLFetch** ou **SQLFetchScroll**. Cette erreur est retournée par le Gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **SQLFetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert, mais pas positionné sur le *au paramètre StatementHandle*.<br /><br /> **StatementText* contenus une position de mettre à jour ou de l’instruction delete, et le curseur est positionné avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|34000|Nom de curseur non valide|**StatementText* contenus une position de mettre à jour ou de l’instruction delete, et le curseur référencé par l’instruction en cours d’exécution n’est pas ouvert.|  
|3D000|Nom de catalogue non valide|Le nom de catalogue spécifié dans *StatementText* n’était pas valide.|  
|3F000|Nom de schéma non valide|Le nom de schéma spécifié dans *StatementText* n’était pas valide.|  
|40001|Échec de la sérialisation|La transaction a été annulée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|Échec de la connexion associée lors de l’exécution de cette fonction, et l’état de la transaction ne peut pas être déterminé.|  
|42000|Syntaxe ou violation d’accès|\**StatementText* contient une instruction SQL qui n’a pas été pouvant être préparé ou contenait une erreur de syntaxe.<br /><br /> L’utilisateur n’a pas l’autorisation d’exécuter l’instruction SQL contenue dans **StatementText*.|  
|42S01|Table de base ou la vue existe déjà|\**StatementText* contenait un **CREATE TABLE** ou **CREATE VIEW** instruction et que le nom de la table ou vue nom spécifié existe déjà.|  
|42 S 02|Table de base ou de vue introuvable|\**StatementText* contenait un **DROP TABLE** ou un **DROP VIEW** instruction et le nom de la table spécifiée ou le nom n’existe pas de vue.<br /><br /> \**StatementText* contenait un **ALTER TABLE** instruction et le nom de la table spécifiée n’existent pas.<br /><br /> \**StatementText* contenait un **CREATE VIEW** instruction et un nom de table ou la vue nom défini par la spécification de requête n’existe pas.<br /><br /> \**StatementText* contenait un **CREATE INDEX** instruction et le nom de la table spécifiée n’existent pas.<br /><br /> \**StatementText* contenait un **GRANT** ou **RÉVOQUER** instruction et le nom de la table spécifiée ou le nom n’existe pas de vue.<br /><br /> \**StatementText* contenait un **sélectionnez** instruction et une table spécifiée ou vue de nom n’existe pas.<br /><br /> \**StatementText* contenait un **supprimer**, **insérer**, ou **mise à jour** instruction et le nom de la table spécifiée n’existent pas.<br /><br /> \**StatementText* contenait un **CREATE TABLE** instruction et une table spécifiée dans une contrainte (faisant référence à une table autre que celui en cours de création) n’existent pas.<br /><br /> \**StatementText* contenait un **CREATE SCHEMA** instruction et une table spécifiée ou vue de nom n’existe pas.|  
|42S11|L’index existe déjà|\**StatementText* contenait un **CREATE INDEX** instruction et le nom de l’index spécifié existaient déjà.<br /><br /> \**StatementText* contenait un **CREATE SCHEMA** instruction et le nom de l’index spécifié existaient déjà.|  
|42S12|Index non trouvé|\**StatementText* contenait un **DROP INDEX** instruction et le nom de l’index spécifié n’existent pas.|  
|42S21|La colonne existe déjà|\**StatementText* contenait un **ALTER TABLE** instruction et la colonne spécifiée dans le **ajouter** clause n’est pas unique ou identifie une colonne existante dans la table de base.|  
|42S22|Colonne introuvable|\**StatementText* contenait un **CREATE INDEX** instruction et la valeur d’un ou plusieurs de la colonne spécifiées dans la liste des colonnes de noms n’existent pas.<br /><br /> \**StatementText* contenait un **GRANT** ou **RÉVOQUER** instruction et un nom de colonne spécifié n’existent pas.<br /><br /> \**StatementText* contenait un **sélectionnez**, **supprimer**, **insérer**, ou **mise à jour** instruction et un nom de colonne spécifiée n’existe pas.<br /><br /> \**StatementText* contenait un **CREATE TABLE** instruction et une colonne spécifiée dans une contrainte (faisant référence à une table autre que celui en cours de création) n’existent pas.<br /><br /> \**StatementText* contenait un **CREATE SCHEMA** instruction et un nom de colonne spécifié n’existent pas.|  
|44000|Violation de WITH CHECK OPTION|L’argument *StatementText* contenus un **insérer** instruction exécutée sur une table affichée ou une table dérivée à partir de la table affichée qui a été créée en spécifiant **WITH CHECK OPTION**, telle qu’une ou plusieurs lignes affectées par la **insérer** instruction ne pourra plus être présente dans la table affichée.<br /><br /> L’argument *StatementText* contenus un **mise à jour** instruction exécutée sur une table affichée ou une table dérivée à partir de la table affichée qui a été créée en spécifiant **WITH CHECK OPTION**, telle qu’une ou plusieurs lignes affectées par la **mise à jour** instruction ne pourra plus être présente dans la table affichée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) **StatementText* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLExecDirect** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) l’argument *TextLength* était inférieur ou égal à 0, mais pas égale à SQL_NTS.<br /><br /> Un paramètre défini avec **SQLBindParameter**était un pointeur null et n’a la valeur de paramètre de longueur 0, SQL_NULL_DATA, SQL_DATA_AT_EXEC, SQL_DEFAULT_PARAM, ou inférieure ou égale à SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Un paramètre défini avec **SQLBindParameter**n’était pas un pointeur null ; le type de données C a été SQL_C_BINARY ou SQL_C_CHAR ; et la valeur de paramètre de longueur est inférieure à 0 mais n’était pas SQL_NTS, SQL_NULL_DATA, SQL_DATA_AT_EXEC, SQL_DEFAULT_PARAM, ou inférieure ou égale à SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Une valeur de longueur de paramètre lié par **SQLBindParameter** a été défini sur SQL_DATA_AT_EXEC ; le type SQL a été SQL_LONGVARCHAR, SQL_LONGVARBINARY, ou un type de données de spécifique à la source de données de type long ; et le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** a été « Y ».|  
|HY105|Type de paramètre non valide|La valeur spécifiée pour l’argument *InputOutputType* dans **SQLBindParameter** SQL_PARAM_OUTPUT, et le paramètre était un paramètre d’entrée.|  
|HY109|Position du curseur non valide|\**StatementText* contenus une position de mettre à jour ou de l’instruction delete, et que le curseur a été positionné (par **SQLSetPos** ou **SQLFetchScroll**) sur une ligne qui a été supprimée ou ne peut pas être extraites.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’était pas pris en charge par la source de données ou de pilote.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant que la source de données a retourné le jeu de résultats. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 L’application appelle **SQLExecDirect** pour envoyer une instruction SQL à la source de données. Pour plus d’informations sur l’exécution directe, consultez [l’exécution directe](../../../odbc/reference/develop-app/direct-execution-odbc.md). Le pilote modifie l’instruction pour utiliser le formulaire de SQL utilisée par la source de données et puis l’envoie à la source de données. En particulier, le pilote modifie les séquences d’échappement utilisées pour définir certaines fonctionnalités dans SQL. Pour connaître la syntaxe des séquences d’échappement, consultez [les séquences d’échappement dans ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md).  
  
 L’application peut inclure un ou plusieurs des marqueurs de paramètre dans l’instruction SQL. Pour inclure un marqueur de paramètre, l’application incorpore un point d’interrogation ( ?) dans l’instruction SQL à la position appropriée. Pour plus d’informations sur les paramètres, consultez [paramètres de l’instruction](../../../odbc/reference/develop-app/statement-parameters.md).  
  
 Si l’instruction SQL est un **sélectionnez** instruction et si l’application a appelé **SQLSetCursorName** pour associer un curseur à une instruction, le pilote utilise ensuite le curseur spécifié. Dans le cas contraire, le pilote génère un nom de curseur.  
  
 Si la source de données est en mode de validation manuelle (nécessitant l’émission de la transaction explicite) et une transaction n’a pas déjà été initialisée, le pilote initie une transaction avant d’envoyer l’instruction SQL. Pour plus d’informations, consultez [Mode de validation manuelle](../../../odbc/reference/develop-app/manual-commit-mode.md).  
  
 Si une application utilise **SQLExecDirect** pour envoyer un **valider** ou **restauration** instruction, il ne sera pas interopérable entre les produits de SGBD. Pour valider ou restaurer une transaction, une application appelle **SQLEndTran**.  
  
 Si **SQLExecDirect** rencontre un paramètre à l’exécution, il retourne SQL_NEED_DATA. L’application envoie des données à l’aide **SQLParamData** et **SQLPutData**. Consultez [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md), [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md), et [envoi de données de type Long](../../../odbc/reference/develop-app/sending-long-data.md).  
  
 Si **SQLExecDirect** exécute une recherche de mise à jour, une insertion ou une instruction delete qui n’affecte pas les lignes à la source de données, l’appel à **SQLExecDirect** retourne SQL_NO_DATA.  
  
 Si la valeur de l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE est supérieure à 1 et que l’instruction SQL contient au moins un marqueur de paramètre, **SQLExecDirect** s’exécute l’instruction SQL une fois pour chaque ensemble de paramètres de valeurs dans les tableaux vers lequel pointe le *ParameterValuePointer* argument dans l’appel à **SQLBindParameter**. Pour plus d’informations, consultez [tableaux de valeurs de paramètre](../../../odbc/reference/develop-app/arrays-of-parameter-values.md).  
  
 Si les signets sont sous tension et exécution d’une requête qui ne peut pas prendre en charge des signets, le pilote doit tenter de forcer l’environnement qui prend en charge les signets en modifiant la valeur d’attribut et en retournant SQLSTATE 01 s 02 (Option la valeur modifiée). Si l’attribut ne peut pas être modifié, le pilote doit retourner la valeur SQLSTATE HY024 (valeur d’attribut non valide).  
  
> [!NOTE]  
>  Lorsque vous utilisez le regroupement de connexions, une application ne doit pas exécuter les instructions SQL qui modifient la base de données ou le contexte de la base de données, de telles que la **utilisez** *base de données* instruction dans SQL Server, ce qui modifie le catalogue utilisé par une source de données.  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md), et [exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Exécuter une opération de validation ou de restauration|[SQLEndTran, fonction](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Renvoi d’un nom de curseur|[SQLGetCursorName, fonction](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|L’extraction de tout ou partie d’une colonne de données|[SQLGetData, fonction](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Retourner le paramètre suivant pour envoyer des données pour|[SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Préparation de l’exécution d’une instruction|[SQLPrepare, fonction](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Envoi de données de paramètre au moment de l’exécution|[SQLPutData, fonction](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
|Définition d’un attribut d’instruction|[SQLSetStmtAttr, fonction](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
