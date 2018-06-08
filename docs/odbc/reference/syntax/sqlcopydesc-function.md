---
title: Fonction de SQLCopyDesc | Documents Microsoft
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
- SQLCopyDesc
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCopyDesc
helpviewer_keywords:
- SQLCopyDesc function [ODBC]
ms.assetid: d5450895-3824-44c4-8aa4-d4f9752a9602
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9b87ee611e74cc9adc0015ce0f9b6b0c15c3bc7a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcopydesc-function"></a>SQLCopyDesc (fonction)
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLCopyDesc** copie les informations de descripteur à partir d’un descripteur handle vers un autre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLCopyDesc(  
     SQLHDESC     SourceDescHandle,  
     SQLHDESC     TargetDescHandle);  
```  
  
## <a name="arguments"></a>Arguments  
 *SourceDescHandle*  
 [Entrée] Handle de descripteur de source.  
  
 *TargetDescHandle*  
 [Entrée] Handle de descripteur de cible. Le *TargetDescHandle* argument peut être un handle de descripteur d’une application ou un IPD. *TargetDescHandle* ne peut pas être définie pour un handle vers un IRD, ou **SQLCopyDesc** retournera HY016 SQLSTATE (Impossible de modifier un descripteur de ligne d’implémentation).  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLCopyDesc** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_DESC et un *gérer* de *TargetDescHandle*. Si non valide *SourceDescHandle* a été passé dans l’appel, SQL_INVALID_HANDLE sera retourné mais aucune SQLSTATE n’est retourné. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLCopyDesc** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
 Lorsqu’une erreur est retournée, l’appel à **SQLCopyDesc** est immédiatement abandonnée et le contenu des champs dans le *TargetDescHandle* descripteur ne sont pas définis.  
  
 Étant donné que **SQLCopyDesc** peut être implémentée en appelant **SQLGetDescField** et **SQLSetDescField**, **SQLCopyDesc** peut retourner SQLSTATE retournée par **SQLGetDescField** ou **SQLSetDescField**.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY007|L’instruction associée n’est pas préparée.|*SourceDescHandle* a été associé à un IRD, et le handle d’instruction associée n’était pas dans l’état préparée ou exécutée.|  
|HY010|Erreur de séquence de fonction|(DM) le descripteur de gérer dans *SourceDescHandle* ou *TargetDescHandle* a été associé à un *au paramètre StatementHandle* pour une fonction de façon asynchrone en cours d’exécution (pas celui-ci) qui a été appelée et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) le descripteur de gérer dans *SourceDescHandle* ou *TargetDescHandle* a été associé à un *au paramètre StatementHandle* pour lequel **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelée et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *SourceDescHandle* ou *TargetDescHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLCopyDesc** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour une des poignées d’instruction associées à la *SourceDescHandle* ou *TargetDescHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY016|Impossible de modifier un descripteur de ligne d’implémentation|*TargetDescHandle* a été associé à un IRD.|  
|HY021|Informations de descripteur incohérentes|Les informations de descripteur vérifiées pendant une vérification de cohérence n’étaient pas cohérentes. Pour plus d’informations, consultez « Vérifications de cohérence » dans **SQLSetDescField**.|  
|HY092|Identificateur d’attribut/option non valide|L’appel à **SQLCopyDesc** vous y êtes invité à un appel à **SQLSetDescField**, mais  *\*ValuePtr* n’était pas valide pour le *FieldIdentifier* argument sur *TargetDescHandle*.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *SourceDescHandle* ou *TargetDescHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Un appel à **SQLCopyDesc** copie les champs du descripteur de source de gérer le handle de descripteur cible. Les champs peuvent être copiés qu’à un descripteur de l’application ou un IPD, mais pas à un IRD. Les champs peuvent être copiés à partir d’une application ou d’un descripteur d’implémentation.  
  
 Les champs peuvent être copiés depuis un IRD uniquement si le handle d’instruction est dans l’état préparée ou exécutée ; Sinon, la fonction retourne SQLSTATE HY007 (instruction d’associé n’est pas préparée).  
  
 Les champs peuvent être copiés à partir d’un IPD ou non une instruction a été préparée. Si une instruction SQL avec des paramètres dynamiques a été préparée et que le remplissage automatique de l’IPD est pris en charge et activé, l’IPD est remplie par le pilote. Lorsque **SQLCopyDesc** est appelée avec l’IPD comme le *SourceDescHandle*, les champs remplis sont copiés. Si le périphérique intégré n’est pas rempli par le pilote, le contenu des champs à l’origine de l’IPD est copié.  
  
 Tous les champs du descripteur, à l’exception de SQL_DESC_ALLOC_TYPE (qui spécifie si le handle de descripteur a été automatiquement ou explicitement alloué), sont copiés, le champ est défini pour le descripteur de destination ou non. Champs copiés remplacement les champs existants.  
  
 Le pilote copie tous les champs de descripteur si le *SourceDescHandle* et *TargetDescHandle* arguments associés à la même pilote, même si les pilotes se trouvent sur deux connexions différentes ou des environnements. Si le *SourceDescHandle* et *TargetDescHandle* arguments sont associés à différents pilotes, le Gestionnaire de pilotes copie des champs définis par ODBC, mais ne copie pas les champs définis par le pilote ou des champs qui ne sont pas définis par ODBC pour le type du descripteur.  
  
 L’appel à **SQLCopyDesc** est abandonné immédiatement si une erreur se produit.  
  
 Lorsque le champ SQL_DESC_DATA_PTR est copié, une vérification de cohérence est effectuée sur le descripteur de la cible. Si la vérification de cohérence échoue, SQLSTATE HY021 (informations de descripteur incohérentes) sont retournées et l’appel à **SQLCopyDesc** est immédiatement abandonnée. Pour plus d’informations sur les vérifications de cohérence, voir « Vérifications de cohérence » dans [SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 Poignées de descripteur peuvent être copiées sur des connexions, même si les connexions sont sous différents environnements. Si le Gestionnaire de pilote détecte que la source et le descripteur de destination handles n’appartiennent pas à la même connexion et les deux connexions appartiennent pour séparer les pilotes, il implémente **SQLCopyDesc** en effectuant une copie de champ par champ à l’aide de **SQLGetDescField** et **SQLSetDescField**.  
  
 Lorsque **SQLCopyDesc** est appelée avec un *SourceDescHandle* sur un pilote et un *TargetDescHandle* sur un autre pilote, la file d’attente de l’erreur de la *SourceDescHandle* est désactivée. Cela se produit car **SQLCopyDesc** dans ce cas est implémentée par les appels à **SQLGetDescField** et **SQLSetDescField**.  
  
> [!NOTE]  
>  Une application peut être en mesure d’associer un handle de descripteur alloué explicitement avec une *au paramètre StatementHandle*, au lieu d’appeler **SQLCopyDesc** pour copier les champs à partir d’un descripteur vers un autre. Un descripteur explicitement alloué peut être associé à un autre *au paramètre StatementHandle* sur le même *handle de connexion* en définissant l’attribut d’instruction SQL_ATTR_APP_ROW_DESC ou SQL_ATTR_APP_PARAM_DESC pour le handle du descripteur alloué de manière explicite. Dans ce cas, **SQLCopyDesc** n’a pas à être appelée pour copier les valeurs de champ de descripteur à partir d’un descripteur vers une autre. Un handle de descripteur ne peut pas être associé un *au paramètre StatementHandle* sur un autre *handle de connexion*, toutefois, pour utiliser les mêmes valeurs de champ de descripteur sur *StatementHandles* sur différents *ConnectionHandles*, **SQLCopyDesc** doit être appelée.  
  
 Pour obtenir une description des champs dans un en-tête de descripteur ou un enregistrement, consultez [SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Pour plus d’informations sur les descripteurs, consultez [descripteurs](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="copying-rows-between-tables"></a>Copie de lignes entre les Tables  
 Une application peut copier des données d’une table à l’autre sans copier les données au niveau de l’application. Pour ce faire, l’application lie les mémoires tampons de données même et les informations de descripteur à une instruction qui extrait les données et l’instruction qui insère les données dans une copie. Cela peut être accompli en partageant un descripteur de l’application (liaison d’un descripteur explicitement alloué à la fois le ARD à une seule instruction et le descripteur APD dans un autre) ou à l’aide de **SQLCopyDesc** pour copier les liaisons entre le ARD et le descripteur APD des deux instructions. Si les instructions se trouvent sur des connexions différentes, **SQLCopyDesc** doit être utilisé. En outre, **SQLCopyDesc** doit être appelée pour copier les liaisons entre l’IRD et l’IPD des deux instructions. Lors de la copie entre des instructions sur la même connexion, le type d’informations SQL_ACTIVE_STATEMENTS retournée par le pilote pour un appel à **SQLGetInfo** doit être supérieure à 1 pour cette opération réussisse. (Cela n’est pas le cas lors de la copie sur des connexions.)  
  
### <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, les opérations de descripteur sont utilisées pour copier les champs de la table PartsSource dans la table PartsCopy. Le contenu de la table PartsSource est lues dans les tampons de l’ensemble de lignes dans *hstmt0*. Ces valeurs sont utilisées en tant que paramètres d’une instruction INSERT sur *hstmt1* pour remplir les colonnes de la table PartsCopy. Pour ce faire, les champs de l’IRD de *hstmt0* sont copiés dans les champs de l’IPD de *hstmt1*et les champs de la ARD de *hstmt0* sont copiés dans les champs de l’APD de *hstmt1*. Utilisez **SQLSetDescField** pour définir l’attribut SQL_DESC_PARAMETER_TYPE de l’IPD pour SQL_PARAM_INPUT lorsque vous copiez des champs IRD à partir d’une instruction avec des paramètres de sortie dans les champs IPD qui doivent être des paramètres d’entrée.  
  
```  
#define ROWS 100  
#define DESC_LEN 50  
#define SQL_SUCCEEDED(rc) (rc == SQL_SUCCESS || rc == SQL_SUCCESS_WITH_INFO)  
  
// Template for a row  
typedef struct {  
   SQLINTEGER   sPartID;  
   SQLINTEGER   cbPartID;  
   SQLUCHAR     szDescription[DESC_LENGTH];  
   SQLINTEGER   cbDescription;  
   REAL         sPrice;  
   SQLINTEGER   cbPrice;  
} PartsSource;  
  
PartsSource    rget[ROWS];          // rowset buffer  
SQLUSMALLINT   sts_ptr[ROWS];       // status pointer  
SQLHSTMT       hstmt0, hstmt1;  
SQLHDESC       hArd0, hIrd0, hApd1, hIpd1;  
  
// ARD and IRD of hstmt0  
SQLGetStmtAttr(hstmt0, SQL_ATTR_APP_ROW_DESC, &hArd0, 0, NULL);  
SQLGetStmtAttr(hstmt0, SQL_ATTR_IMP_ROW_DESC, &hIrd0, 0, NULL);  
  
// APD and IPD of hstmt1  
SQLGetStmtAttr(hstmt1, SQL_ATTR_APP_PARAM_DESC, &hApd1, 0, NULL);  
SQLGetStmtAttr(hstmt1, SQL_ATTR_IMP_PARAM_DESC, &hIpd1, 0, NULL);  
  
// Use row-wise binding on hstmt0 to fetch rows  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_BIND_TYPE, (SQLPOINTER) sizeof(PartsSource), 0);  
  
// Set rowset size for hstmt0  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
  
// Execute a select statement  
SQLExecDirect(hstmt0, "SELECT PARTID, DESCRIPTION, PRICE FROM PARTS ORDER BY 3, 1, 2"",  
               SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt0, 1, SQL_C_SLONG, rget[0].sPartID, 0,   
   &rget[0].cbPartID);  
SQLBindCol(hstmt0, 2, SQL_C_CHAR, &rget[0].szDescription, DESC_LEN,   
   &rget[0].cbDescription);  
SQLBindCol(hstmt0, 3, SQL_C_FLOAT, rget[0].sPrice,   
   0, &rget[0].cbPrice);  
  
// Perform parameter bindings on hstmt1.   
SQLCopyDesc(hArd0, hApd1);  
SQLCopyDesc(hIrd0, hIpd1);  
  
// Set the array status pointer of IRD  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_STATUS_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the ARRAY_STATUS_PTR field of APD to be the same  
// as that in IRD.  
SQLSetStmtAttr(hstmt1, SQL_ATTR_PARAM_OPERATION_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the hIpd1 records as input parameters  
rc = SQLSetDescField(hIpd1, 1, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 2, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 3, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
  
// Prepare an insert statement on hstmt1. PartsCopy is a copy of  
// PartsSource  
SQLPrepare(hstmt1, "INSERT INTO PARTS_COPY VALUES (?, ?, ?)", SQL_NTS);  
  
// In a loop, fetch a rowset, and copy the fetched rowset to PARTS_COPY  
  
rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
while (SQL_SUCCEEDED(rc)) {  
  
   // After the call to SQLFetchScroll, the status array has row   
   // statuses. This array is used as input status in the APD  
   // and hence determines which elements of the rowset buffer  
   // are inserted.  
   SQLExecute(hstmt1);  
  
   rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
} // while  
```  
  
### <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention de plusieurs champs de descripteur|[SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Définition d’un champ de descripteur unique|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Définition de plusieurs champs de descripteur|[SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
