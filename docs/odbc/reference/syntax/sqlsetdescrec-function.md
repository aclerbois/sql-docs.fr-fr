---
title: SQLSetDescRec, fonction | Documents Microsoft
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
- SQLSetDescRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescRec
helpviewer_keywords:
- SQLSetDescRec function [ODBC]
ms.assetid: bf55256c-7eb7-4e3f-97ef-b0fee09ba829
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 085543aca838cfe1a8123f891c30f618b86eb15d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetdescrec-function"></a>SQLSetDescRec, fonction
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 Le **SQLSetDescRec** fonction définit plusieurs champs de descripteur qui affectent le type de données et la mémoire tampon liée à une colonne ou un paramètre de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLSetDescRec(  
      SQLHDESC      DescriptorHandle,  
      SQLSMALLINT   RecNumber,  
      SQLSMALLINT   Type,  
      SQLSMALLINT   SubType,  
      SQLLEN        Length,  
      SQLSMALLINT   Precision,  
      SQLSMALLINT   Scale,  
      SQLPOINTER    DataPtr,  
      SQLLEN *      StringLengthPtr,  
      SQLLEN *      IndicatorPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *DescriptorHandle*  
 [Entrée] Handle de descripteur. Cela ne doit pas être un handle IRD.  
  
 *RecNumber*  
 [Entrée] Indique que l’enregistrement de descripteur qui contient les champs à définir. Enregistrements de descripteurs sont numérotés de 0, avec le numéro d’enregistrement 0 en cours de l’enregistrement de signet. Cet argument doit être égale ou supérieure à 0. Si *RecNumber* est supérieure à la valeur de SQL_DESC_COUNT, SQL_DESC_COUNTis sur la valeur de *RecNumber*.  
  
 *Type*  
 [Entrée] La valeur à laquelle définir le champ SQL_DESC_TYPE pour l’enregistrement de descripteur.  
  
 *SubType*  
 [Entrée] Pour les enregistrements dont le type est SQL_DATETIME ou SQL_INTERVAL, il s’agit de la valeur à laquelle définir le champ de valeur SQL_DESC_DATETIME_INTERVAL_CODE.  
  
 *Longueur*  
 [Entrée] La valeur à laquelle définir le champ SQL_DESC_OCTET_LENGTH pour l’enregistrement de descripteur.  
  
 *Précision*  
 [Entrée] La valeur à laquelle définir le champ SQL_DESC_PRECISION pour l’enregistrement de descripteur.  
  
 *Échelle*  
 [Entrée] La valeur à laquelle définir le champ SQL_DESC_SCALE pour l’enregistrement de descripteur.  
  
 *DataPtr*  
 [Différée entrée ou sortie] La valeur à laquelle définir le champ SQL_DESC_DATA_PTR pour l’enregistrement de descripteur. *DataPtr* peut être défini à un pointeur null.  
  
 Le *DataPtr* argument peut être défini à un pointeur null pour définir le champ SQL_DESC_DATA_PTR à un pointeur null. Si le handle de la *DescriptorHandle* argument associé à un ARD, cela annule la liaison de la colonne.  
  
 *StringLengthPtr*  
 [Différée entrée ou sortie] La valeur à laquelle définir le champ SQL_DESC_OCTET_LENGTH_PTR pour l’enregistrement de descripteur. *StringLengthPtr* peut être définie à un pointeur null, pour définir le champ SQL_DESC_OCTET_LENGTH_PTR à un pointeur null.  
  
 *IndicatorPtr*  
 [Différée entrée ou sortie] La valeur à laquelle définir le champ SQL_DESC_INDICATOR_PTR pour l’enregistrement de descripteur. *IndicatorPtr* peut être définie à un pointeur null, pour définir le champ SQL_DESC_INDICATOR_PTR à un pointeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLSetDescRec** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_DESC et un *gérer* de *DescriptorHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLSetDescRec** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07009|Index de descripteur non valide|Le *RecNumber* argument a été défini sur 0 et le *DescriptorHandle* auquel un descripteur IPD.<br /><br /> Le *RecNumber* argument était inférieure à 0.<br /><br /> Le *RecNumber* argument était supérieur au nombre maximal de colonnes ou des paramètres de la source de données peut prendre en charge, et le *DescriptorHandle* argument était un APD, IPD ou ARD.<br /><br /> Le *RecNumber* argument était égal à 0 et le *DescriptorHandle* argument auquel une APD implicitement alloué. (Cette erreur ne se produit pas avec un descripteur de l’application attribuée explicitement, car il n’est pas connu qu’un descripteur de l’application explicitement alloué soit APD ou ARD jusqu'à ce que l’exécution.)|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) le *DescriptorHandle* a été associé à un *au paramètre StatementHandle* pour une fonction de façon asynchrone en cours d’exécution (pas celui-ci) qui a été appelée et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* avec lequel le *DescriptorHandle* a été associé et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *DescriptorHandle*. Cette fonction aynchronous toujours en cours d’exécution lorsque le **SQLSetDescRec** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour une des poignées d’instruction associées à la *DescriptorHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY016|Impossible de modifier un descripteur de ligne d’implémentation|Le *DescriptorHandle* argument a été associé à un IRD.|  
|HY021|Informations de descripteur incohérentes|Le *Type* champ, ou tout autre champ n’est associé au champ SQL_DESC_TYPE dans le descripteur, n’était pas valide ou cohérent.<br /><br /> Les informations de descripteur vérifiées pendant une vérification de cohérence n’étaient pas cohérentes. (Voir « Vérifications de cohérence, » plus loin dans cette section).|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) le pilote a été un ODBC 2 *.x* pilote, le descripteur a été un ARD, le *ColumnNumber* argument a été défini sur 0 et la valeur spécifiée pour l’argument *BufferLength* n’est pas égale à 4.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *DescriptorHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Une application peut appeler **SQLSetDescRec** pour définir les champs suivants pour une seule colonne ou un paramètre :  
  
-   SQL_DESC_TYPE  
  
-   SQL_DESC_DATETIME_INTERVAL_CODE (pour les enregistrements dont le type est SQL_DATETIME ou SQL_INTERVAL)  
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_DATA_PTR  
  
-   SQL_DESC_OCTET_LENGTH_PTR  
  
-   SQL_DESC_INDICATOR_PTR  
  
> [!NOTE]  
>  Si un appel à **SQLSetDescRec** échoue, le contenu de l’enregistrement de descripteur identifié par le *RecNumber* argument ne sont pas définis.  
  
 Lors de la liaison d’une colonne ou un paramètre **SQLSetDescRec** vous permet de modifier plusieurs champs qui affectent la liaison sans appeler **SQLBindCol** ou **SQLBindParameter** ou effectuer plusieurs appels à **SQLSetDescField**. **SQLSetDescRec** peut définir des champs sur un descripteur pas actuellement associé à une instruction. Notez que **SQLBindParameter** définit plus de champs que **SQLSetDescRec**peut définir des champs sur un APD et un IPD dans un seul appel et ne nécessite pas d’un handle de descripteur.  
  
> [!NOTE]  
>  L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS doit toujours être défini avant d’appeler **SQLSetDescRec** avec un *RecNumber* l’argument 0 pour définir les champs de signet. Alors que cela n’est pas obligatoire, il est fortement recommandé.  
  
## <a name="consistency-checks"></a>Vérifications de cohérence  
 Une vérification de cohérence est effectuée par le pilote automatiquement chaque fois qu’une application définit le champ SQL_DESC_DATA_PTR d’APD, ARD ou IPD. Si l’un des champs n’est pas cohérente avec les autres champs, **SQLSetDescRec** retourne SQLSTATE HY021 (informations de descripteur incohérentes).  
  
 Chaque fois qu’une application définit le champ SQL_DESC_DATA_PTR d’APD, ARD ou IPD, le pilote vérifie que la valeur du champ SQL_DESC_TYPE et les valeurs applicables à ce champ SQL_DESC_TYPE sont valides et cohérents. Cette vérification est toujours effectuée lorsque **SQLBindParameter** ou **SQLBindCol** est appelée ou lorsque **SQLSetDescRec** est appelée pour un APD, ARD ou IPD. Cette vérification de cohérence inclut les vérifications suivantes sur les champs de descripteur :  
  
-   Le champ SQL_DESC_TYPE doit être valide ODBC C ou types SQL ou un type de pilote SQL. Le champ SQL_DESC_CONCISE_TYPE doit être un des types C ODBC ou SQL valides ou un type de C ou SQL spécifiques au pilote, y compris les types datetime et interval concis.  
  
-   Si le champ d’enregistrement SQL_DESC_TYPE est SQL_DATETIME ou SQL_INTERVAL, le champ de valeur SQL_DESC_DATETIME_INTERVAL_CODE doit être un des codes d’intervalle datetime valide. (Consultez la description du champ de valeur SQL_DESC_DATETIME_INTERVAL_CODE [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).)  
  
-   Si le champ SQL_DESC_TYPE indique un type numérique, les champs SQL_DESC_PRECISION et SQL_DESC_SCALE sont vérifiées pour être valide.  
  
-   Si le champ SQL_DESC_CONCISE_TYPE est un type de données heure ou timestamp, un intervalle de type avec un composant « secondes », ou l’un des types de données avec un composant d’heure intervalle le champ SQL_DESC_PRECISION est vérifié pour être une précision de secondes valide.  
  
-   Si le SQL_DESC_CONCISE_TYPE est un type de données d’intervalle, le champ SQL_DESC_DATETIME_INTERVAL_PRECISION est vérifié pour être une valeur de précision de début valide d’intervalle.  
  
 Le champ SQL_DESC_DATA_PTR d’un IPD n’est pas défini normalement ; Toutefois, une application peut faire pour forcer une vérification de cohérence de champs IPD. Une vérification de cohérence ne peut pas être effectuée sur un IRD. La valeur définie pour le champ SQL_DESC_DATA_PTR de l’IPD n’est pas réellement stockée et ne peut pas être récupérée par un appel à **SQLGetDescField** ou **SQLGetDescRec**; le paramètre fait uniquement pour forcer la vérification de cohérence.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une colonne|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Un paramètre de liaison|[SQLBindParameter, fonction](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Obtention d’un champ de descripteur unique|[SQLGetDescField, fonction](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Obtention de plusieurs champs de descripteur|[SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Définition des champs de descripteur unique|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
