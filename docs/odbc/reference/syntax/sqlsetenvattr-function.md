---
title: Fonction SQLSetEnvAttr | Documents Microsoft
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
- SQLSetEnvAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetEnvAttr
helpviewer_keywords:
- SQLSetEnvAttr function [ODBC]
ms.assetid: 0343241c-4b15-4d4b-aa2b-2e8ab5215cd2
caps.latest.revision: 38
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f0593bf413a9d3341927f8108df4e6a490efa927
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetenvattr-function"></a>Fonction SQLSetEnvAttr
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLSetEnvAttr** définit les attributs qui régissent les aspects des environnements.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLSetEnvAttr(  
     SQLHENV      EnvironmentHandle,  
     SQLINTEGER   Attribute,  
     SQLPOINTER   ValuePtr,  
     SQLINTEGER   StringLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 [Entrée] Handle d’environnement.  
  
 *Attribut*  
 [Entrée] Attribut à définir, répertoriés dans « Commentaires ».  
  
 *ValuePtr*  
 [Entrée] Pointeur vers la valeur à associer à *attribut*. Selon la valeur de *attribut*, *ValuePtr* sera une valeur d’entier 32 bits ou pointe vers une chaîne de caractères terminée par null.  
  
 *stringLength*  
 [Entrée] Si *ValuePtr* pointe vers une chaîne de caractères ou d’un tampon binaire, cet argument doit être la longueur de **ValuePtr*. Pour les données de chaîne de caractères, cet argument doit contenir le nombre d’octets dans la chaîne.  
  
 Si *ValuePtr* est un entier, *StringLength* est ignoré.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLSetEnvAttr** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_ENV et un *gérer* de *EnvironmentHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLSetEnvAttr** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire. Si un pilote ne prend pas en charge un attribut de l’environnement, l’erreur peut être retournée uniquement au moment de se connecter.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01S02|Valeur de l’option modifiée|Le pilote ne prenait pas en charge la valeur spécifiée dans *ValuePtr* et une valeur similaire. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY009|Utilisation non valide d’un pointeur null|L’argument d’attribut identifié un attribut d’environnement qui requiert une valeur de chaîne, et le *ValuePtr* argument était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) un handle de connexion a été alloué sur *EnvironmentHandle*.<br /><br /> (DM) **SQL_ATTR_ODBC_VERSION** n’a pas été défini avec **SQLSetEnvAttr** et *attribut* n’est pas égal à **SQL_ATTR_ODBC_VERSION**. Vous n’avez pas besoin de définir **SQL_ATTR_ODBC_VERSION** explicitement si vous utilisez **SQLAllocHandleStd**.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY024|Valeur d’attribut non valide|Étant donné le spécifié *attribut* , une valeur non valide a été spécifiée dans *ValuePtr*.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|Le *StringLength* argument était inférieure à 0 mais pas SQL_NTS.|  
|HY092|Identificateur d’attribut/option non valide|(DM) la valeur spécifiée pour l’argument *attribut* n’était pas valide pour la version d’ODBC pris en charge par le pilote.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l’argument *attribut* a un attribut d’environnement ODBC valide pour la version d’ODBC pris en charge par le pilote, mais n’était pas prise en charge par le pilote.<br /><br /> (DM) le *attribut* SQL_ATTR_OUTPUT_NTS, a été l’argument et *ValuePtr* a été SQL_FALSE.|  
  
## <a name="comments"></a>Commentaires  
 Une application peut appeler **SQLSetEnvAttr** uniquement si aucun handle de connexion n’est alloué sur l’environnement. Tous les attributs d’environnement a été définis par l’application pour l’environnement sont conservés jusqu'à **SQLFreeHandle** est appelée sur l’environnement. Plus d’un handle d’environnement peut être alloué simultanément dans ODBC 3 *.x*.  
  
 Le format des informations définies via *ValuePtr* dépend spécifié *attribut*. **SQLSetEnvAttr** acceptera les informations d’attribut dans un des deux formats : une chaîne de caractères terminée par une valeur null ou une valeur d’entier 32 bits. Le format de chaque est indiqué dans la description de l’attribut.  
  
 Il n’existe pas d’attributs spécifiques au pilote environnement.  
  
 Attributs de connexion ne peut pas être définis par un appel à **SQLSetEnvAttr**. Cette tentative retourne SQLSTATE HY092 (identificateur d’attribut/option non valide).  
  
|*Attribut*|*ValuePtr* contenu|  
|-----------------|-------------------------|  
|SQL_ATTR_CONNECTION_POOLING (ODBC 3.8)|Valeur 32 bits SQLUINTEGER qui active ou désactive le regroupement de connexions au niveau de l’environnement. Les valeurs suivantes sont utilisées :<br /><br /> SQL_CP_OFF = connexion le regroupement est mis hors tension. Il s'agit du paramètre par défaut.<br /><br /> SQL_CP_ONE_PER_DRIVER = un seul pool de connexions est pris en charge pour chaque pilote. Chaque connexion dans un pool est associée à un pilote.<br /><br /> SQL_CP_ONE_PER_HENV = un seul pool de connexions est pris en charge pour chaque environnement. Chaque connexion dans un pool est associée à un environnement.<br /><br /> SQL_CP_DRIVER_AWARE = utiliser la fonctionnalité de reconnaissance du pool de connexions du pilote, si elle est disponible. Si le pilote ne prend pas en charge la reconnaissance du pool de connexions, SQL_CP_DRIVER_AWARE est ignorée et SQL_CP_ONE_PER_HENV est utilisé. Pour plus d’informations, consultez [le regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md). Dans un environnement où certains pilotes prennent en charge certains pilotes ne prennent pas en charge reconnaissance du pool de connexions, SQL_CP_DRIVER_AWARE peuvent activer la fonctionnalité de reconnaissance de pool de connexions de ces pilotes de prise en charge, mais il est équivalent au paramètre SQL_CP_ONE_PER_HENV sur les pilotes qui ne prennent pas en charge la fonctionnalité de reconnaissance de pool de connexions.<br /><br /> Le regroupement de connexions est activé en appelant **SQLSetEnvAttr** pour définir l’attribut SQL_ATTR_CONNECTION_POOLING SQL_CP_ONE_PER_DRIVER ou SQL_CP_ONE_PER_HENV. Cet appel doit être effectué avant que l’application alloue de l’environnement partagé pour quelle connexion de regroupement doit être activé. Le handle d’environnement dans l’appel à **SQLSetEnvAttr** est définie sur null, ce qui rend SQL_ATTR_CONNECTION_POOLING un attribut au niveau du processus. Une fois le regroupement de connexions est activé, puis l’application alloue un environnement partagé implicit en appelant **SQLAllocHandle** avec la *InputHandle* argument défini à SQL_HANDLE_ENV.<br /><br /> Une fois que le regroupement de connexions a été activé et un environnement partagé a été sélectionné pour une application, SQL_ATTR_CONNECTION_POOLING ne peut pas être réinitialisé pour cet environnement, car **SQLSetEnvAttr** est appelée avec un handle d’environnement null lors de la définition de cet attribut. Si cet attribut est défini alors que le regroupement de connexions est déjà activé sur un environnement partagé, l’attribut affecte uniquement les environnements partagés qui sont affectés par la suite.<br /><br /> Il est également possible d’activer le regroupement de connexions sur un environnement. Notez les éléments suivants concernant le regroupement de connexions environnement :<br /><br /> -Activation d’un handle NULL de regroupement de connexions est un attribut au niveau du processus. Environnements alloués par la suite seront un environnement partagé et héritent des paramètres de regroupement de connexion au niveau du processus.<br />-Une fois un environnement est alloué, une application peut toujours modifier son paramètre de pool de connexions.<br />-Si le regroupement de connexions environnement est activé et que les pilotes de la connexion utilise le regroupement de pilote, le regroupement d’environnement prend préférence.<br /><br /> SQL_ATTR_CONNECTION_POOLING est implémentée dans le Gestionnaire de pilotes. Un pilote n’a pas besoin d’implémenter SQL_ATTR_CONNECTION_POOLING. ODBC 2.0 et 3.0 applications peuvent définir cet attribut de l’environnement.<br /><br /> Pour plus d’informations, consultez [le regroupement de connexions ODBC](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md).|  
|SQL_ATTR_CP_MATCH (ODBC 3.0)|Une valeur 32 bits SQLUINTEGER qui détermine la façon dont une connexion est choisie à partir d’un pool de connexions. Lorsque **SQLConnect** ou **SQLDriverConnect** est appelée, le Gestionnaire de pilotes détermine quelle connexion est réutilisée dans le pool. Le Gestionnaire de pilotes tente correspondent aux options de connexion dans l’appel et les attributs de connexion définies par l’application pour les mots clés et les attributs de connexion des connexions dans le pool. La valeur de cet attribut détermine le niveau de précision des critères de correspondance.<br /><br /> Les valeurs suivantes sont utilisées pour définir la valeur de cet attribut :<br /><br /> SQL_CP_STRICT_MATCH = uniquement les connexions qui correspondent exactement les options de connexion dans l’appel et la connexion, les attributs définis par l’application sont réutilisées. Il s'agit du paramètre par défaut.<br /><br /> SQL_CP_RELAXED_MATCH = connexions avec correspondance de chaîne de connexion de mots clés peuvent être utilisés. Mots clés doivent correspondre, mais pas tous les attributs de connexion doivent correspondre.<br /><br /> Pour plus d’informations sur la façon dont le Gestionnaire de pilotes effectue la correspondance à se connecter à une connexion regroupée, consultez [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Pour plus d’informations sur le regroupement de connexions, consultez [le regroupement de connexions ODBC](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md).|  
|SQL_ATTR_ODBC_VERSION (ODBC 3.0)|Un entier 32 bits qui détermine si certaines fonctionnalités expose ODBC 2 *.x* comportement ou ODBC 3 *.x* comportement. Les valeurs suivantes sont utilisées pour définir la valeur de cet attribut :<br /><br /> SQL_OV_ODBC3_80 = le Gestionnaire de pilotes et comportement du pilote annexe le ODBC 3.8 suivantes :<br /><br /> -Le pilote retourne et attend ODBC 3. *x* codes pour la date, time et timestamp.<br />-Le pilote retourne ODBC 3. *x* les codes SQLSTATE lorsque **SQLError**, **SQLGetDiagField**, ou **SQLGetDiagRec** est appelée.<br />-La *CatalogName* argument dans un appel à **SQLTables** accepte un modèle de recherche.<br />-Le Gestionnaire de pilote prend en charge l’extensibilité du type de données C. Pour plus d’informations sur l’extensibilité de type de données C, consultez [des Types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).<br /><br /> Pour plus d’informations, consultez [Nouveautés ODBC 3.8](../../../odbc/reference/what-s-new-in-odbc-3-8.md).<br /><br /> SQL_OV_ODBC3 = le Gestionnaire de pilotes et l’exposition de pilote le 3 ODBC suivantes *.x* comportement :<br /><br /> -Le pilote retourne et attend ODBC 3 *.x* codes pour la date, time et timestamp.<br />-Le pilote retourne ODBC 3 *.x* les codes SQLSTATE lorsque **SQLError**, **SQLGetDiagField**, ou **SQLGetDiagRec** est appelée.<br />-La *CatalogName* argument dans un appel à **SQLTables** accepte un modèle de recherche.<br />-Le Gestionnaire de pilote ne prend pas en charge l’extensibilité du type de données C.<br /><br /> SQL_OV_ODBC2 = le Gestionnaire de pilotes et l’exposition du pilote ODBC 2 suivant *.x* comportement. Cela est particulièrement utile pour un ODBC 2 *.x* application utilisant une ODBC 3 *.x* pilote.<br /><br /> -Le pilote retourne et attend ODBC 2 *.x* codes pour la date, time et timestamp.<br />-Le pilote retourne ODBC 2 *.x* les codes SQLSTATE lorsque **SQLError**, **SQLGetDiagField**, ou **SQLGetDiagRec** est appelée.<br />-La *CatalogName* argument dans un appel à **SQLTables** n’accepte pas d’un modèle de recherche.<br />-Le Gestionnaire de pilote ne prend pas en charge l’extensibilité du type de données C.<br /><br /> Une application doit définir cet attribut d’environnement avant d’appeler toute fonction qui a un argument SQLHENV ou l’appel retourne SQLSTATE HY010 (erreur de séquence de fonction). Il est spécifique au pilote, si un comportement supplémentaire existe pour ces indicateurs de l’environnement.<br /><br /> -Pour plus d’informations, consultez [déclarant ODBC Version l’Application](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md) et [modifications comportementales](../../../odbc/reference/develop-app/behavioral-changes.md).|  
|SQL_ATTR_OUTPUT_NTS (ODBC 3.0)|Un entier 32 bits qui détermine comment le pilote retourne les données de chaîne. Si SQL_TRUE, le pilote retourne des données de chaîne se terminant par null. Si SQL_FALSE, le pilote ne retourne pas de données de chaîne se terminant par null.<br /><br /> Par défaut, cet attribut est SQL_TRUE. Un appel à **SQLSetEnvAttr** lui affectez la valeur SQL_TRUE retourne SQL_SUCCESS. Un appel à **SQLSetEnvAttr** pour le définir retourne SQL_FALSE SQL_ERROR et SQLSTATE HYC00 (fonctionnalité facultative non implémentée).|  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Retourner le paramètre d’un attribut d’environnement|[SQLGetEnvAttr, fonction](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Nouveautés d’ODBC 3.8](../../../odbc/reference/what-s-new-in-odbc-3-8.md)
