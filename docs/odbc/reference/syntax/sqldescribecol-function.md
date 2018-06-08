---
title: Fonction SQLDescribeCol | Documents Microsoft
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
- SQLDescribeCol
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeCol
helpviewer_keywords:
- SQLDescribeCol function [ODBC]
ms.assetid: eddef353-83f3-4a3c-8f24-f9ed888890a4
caps.latest.revision: 35
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4e35649b865a41481de3bfe3356d8738888be39c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqldescribecol-function"></a>Fonction SQLDescribeCol
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLDescribeCol** retourne le descripteur de résultat : nom de colonne, type, taille de colonne, des chiffres décimaux et possibilité de valeur null, pour une colonne dans le résultat défini. Ces informations sont également disponibles dans les champs de l’IRD.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLDescribeCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLCHAR *      ColumnName,  
      SQLSMALLINT    BufferLength,  
      SQLSMALLINT *  NameLengthPtr,  
      SQLSMALLINT *  DataTypePtr,  
      SQLULEN *      ColumnSizePtr,  
      SQLSMALLINT *  DecimalDigitsPtr,  
      SQLSMALLINT *  NullablePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *ColumnNumber*  
 [Entrée] Numéro de colonne de données de résultat, ordonnés séquentiellement dans l’ordre croissant de colonne, en commençant à 1. Le *ColumnNumber* argument peut également être défini à 0 pour décrire le signet de colonne.  
  
 *ColumnName*  
 [Sortie] Pointeur vers une mémoire tampon de se terminant par null dans lequel retourner le nom de colonne. Cette valeur est lue à partir du champ SQL_DESC_NAME de l’IRD. Si la colonne est sans nom ou le nom de colonne ne peut pas être déterminé, le pilote retourne une chaîne vide.  
  
 Si *ColumnName* est NULL, *NameLengthPtr* retourne toujours le nombre total de caractères (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *ColumnName*.  
  
 *BufferLength*  
 [Entrée] Longueur de la **ColumnName* mémoire tampon, en caractères.  
  
 *NameLengthPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre total de caractères (à l’exception de la marque de fin null) disponibles à renvoyer dans \* *ColumnName*. Si le nombre de caractères à retourner est supérieur ou égal à *BufferLength*, le nom de colonne dans \* *ColumnName* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null.  
  
 *DataTypePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le type de données SQL de la colonne. Cette valeur est lue à partir du champ SQL_DESC_CONCISE_TYPE de l’IRD. Il s’agit d’une des valeurs dans [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md), ou un type de données SQL spécifique au pilote. Si le type de données ne peut pas être déterminé, le pilote retourne SQL_UNKNOWN_TYPE.  
  
 Dans ODBC 3. *x*, SQL_TYPE_DATE SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP est retourné dans  *\*DataTypePtr* pour date, heure ou données timestamp, respectivement ; dans ODBC 2. *x*, SQL_DATE, SQL_TIME ou SQL_TIMESTAMP est retourné. Le Gestionnaire de pilotes effectue les mappages requis lorsqu’un ODBC 2. *x* application fonctionne avec un ODBC 3. *x* pilote ou lorsqu’un ODBC 3. *x* application fonctionne avec une API ODBC 2. *x* pilote.  
  
 Lorsque *ColumnNumber* est égal à 0 (pour un signet de colonne), SQL_BINARY est retourné dans  *\*DataTypePtr* de signets de longueur variable. (SQL_INTEGER est retournée si les signets sont utilisés par un ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote ou par un ODBC 2. *x* application utilisant une ODBC 3. *x* pilote.)  
  
 Pour plus d’informations sur ces types de données, consultez [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) annexe d : Types de données. Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote.  
  
 *ColumnSizePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner la taille (en caractères) de la colonne sur la source de données. Si la taille de colonne ne peut pas être déterminée, le pilote retourne 0. Pour plus d’informations sur la taille de colonne, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.  
  
 *DecimalDigitsPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre de décimales de la colonne sur la source de données. Si le nombre de chiffres décimaux ne peut pas être déterminé ou n’est pas applicable, le pilote retourne 0. Pour plus d’informations sur les chiffres décimaux, voir [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.  
  
 *NullablePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner une valeur qui indique si la colonne autorise des valeurs NULL. Cette valeur est lue à partir du champ SQL_DESC_NULLABLE de l’IRD. Les valeurs possibles sont les suivantes :  
  
 SQL_NO_NULLS : La colonne n’autorise pas les valeurs NULL.  
  
 SQL_NULLABLE : La colonne autorise les valeurs NULL.  
  
 SQL_NULLABLE_UNKNOWN : Le pilote ne peut pas déterminer si la colonne autorise les valeurs NULL.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLDescribeCol** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLDescribeCol** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|La mémoire tampon \* *ColumnName* n’est pas suffisamment grande pour retourner le nom de la colonne entière, pour le nom de colonne a été tronqué. La longueur du nom de colonne non tronqué est retournée dans **NameLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07005|Instruction ne préparée pas une *spécification de curseur*|L’instruction associée le *au paramètre StatementHandle* n’a pas retourné un jeu de résultats. Il n’a aucune colonne pour décrire.|  
|07009|Index de descripteur non valide|(DM) la valeur spécifiée pour l’argument *ColumnNumber* était égale à 0, et l’option d’instruction SQL_ATTR_USE_BOOKMARKS SQL_UB_OFF.<br /><br /> La valeur spécifiée pour l’argument *ColumnNumber* était supérieur au nombre de colonnes dans le jeu de résultats.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Échec d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque **SQLDescribeCol** a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM), la fonction a été appelée avant d’appeler **SQLPrepare**, **SQLExecute**, ou une fonction de catalogue sur le handle d’instruction.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength* était inférieure à 0.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
 **SQLDescribeCol** peut retourner tout SQLSTATE qui peut être retournée par **SQLPrepare** ou **SQLExecute** lorsqu’elle est appelée après **SQLPrepare** et avant **SQLExecute**, en fonction lorsque la source de données évalue l’instruction SQL associée au handle d’instruction.  
  
 Pour des raisons de performances, une application ne doit pas appeler **SQLDescribeCol** avant d’exécuter une instruction.  
  
## <a name="comments"></a>Commentaires  
 Une application appelle généralement **SQLDescribeCol** après un appel à **SQLPrepare** et avant ou après l’appel associé à **SQLExecute**. Une application peut également appeler **SQLDescribeCol** après un appel à **SQLExecDirect**. Pour plus d’informations, consultez [métadonnées de valeur de résultat](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLDescribeCol** récupère le nom de colonne, le type et la longueur généré par un **sélectionnez** instruction. Si la colonne est une expression, **ColumnName* est une chaîne vide ou un nom défini par le pilote.  
  
> [!NOTE]  
>  ODBC prend en charge SQL_NULLABLE_UNKNOWN en tant qu’extension, même si la spécification Open Group et l’Interface de niveau appel SQL accès groupe ne spécifie pas l’option pour **SQLDescribeCol**.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Définie les colonnes de retour du nombre de résultats|[SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Préparation de l’exécution d’une instruction|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
