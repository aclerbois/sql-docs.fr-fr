---
title: Fonction SQLGetStmtAttr | Documents Microsoft
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
- SQLGetStmtAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetStmtAttr
helpviewer_keywords:
- SQLGetStmtAttr function [ODBC]
ms.assetid: e321d460-e997-4527-aee6-207cf5a498e9
caps.latest.revision: 25
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1a45674ecbb65fcedd64551af3b7a2440b2b4621
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetstmtattr-function"></a>Fonction SQLGetStmtAttr
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLGetStmtAttr** retourne le paramètre actuel d’un attribut d’instruction.  
  
> [!NOTE]  
>  Pour plus d’informations sur les le Gestionnaire de pilotes mappe cette fonction lorsqu’un ODBC 3. *x* application fonctionne avec une API ODBC 2. *x* pilote, consultez [mappage des fonctions de remplacement pour la compatibilité descendante des Applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLGetStmtAttr(  
     SQLHSTMT        StatementHandle,  
     SQLINTEGER      Attribute,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *Attribut*  
 [Entrée] Attribut à récupérer.  
  
 *ValuePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner la valeur de l’attribut spécifié dans *attribut*.  
  
 Si *ValuePtr* est NULL, *StringLengthPtr* retourne toujours le nombre total d’octets (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *ValuePtr*.  
  
 *BufferLength*  
 [Entrée] Si *attribut* est un attribut défini par ODBC et *ValuePtr* pointe vers une chaîne de caractères ou d’un tampon binaire, cet argument doit être la longueur de \* *ValuePtr*. Si *attribut* est un attribut défini par ODBC et \* *ValuePtr* est un entier, *BufferLength* est ignoré. Si la valeur retournée dans  *\*ValuePtr* est une chaîne Unicode (lors de l’appel **SQLGetStmtAttrW**), la *BufferLength* l’argument doit être un nombre pair.  
  
 Si *attribut* est un attribut définies par le pilote, l’application indique la nature de l’attribut pour le Gestionnaire de pilotes en définissant le *BufferLength* argument. *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si  *\*ValuePtr* est un pointeur vers une chaîne de caractères, puis *BufferLength* est la longueur de la chaîne ou le SQL_NTS.  
  
-   Si  *\*ValuePtr* est un pointeur vers une mémoire tampon binaire, puis l’application place le résultat de la SQL_LEN_BINARY_ATTR (*longueur*) macro dans *BufferLength*. Il s’ensuit une valeur négative dans *BufferLength*.  
  
-   Si  *\*ValuePtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou binaires, puis *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si  *\*ValuePtr* est contient un type de données de longueur fixe, puis *BufferLength* est SQL_IS_INTEGER ou SQL_IS_UINTEGER, selon le cas.  
  
 *StringLengthPtr*  
 [Sortie] Un pointeur vers une mémoire tampon dans lequel retourner le nombre total d’octets (sans le caractère de fin de la valeur null) disponibles à renvoyer dans  *\*ValuePtr*. Si *ValuePtr* est un pointeur null, aucune longueur n’est retournée. Si la valeur d’attribut est une chaîne de caractères, et le nombre d’octets à retourner est supérieur ou égal à *BufferLength*, les données de  *\*ValuePtr* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null et se termine par null par le pilote.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetStmtAttr** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLGetStmtAttr** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Les données retournées dans  *\*ValuePtr* a été tronquée pour être *BufferLength* moins la longueur d’un caractère de fin de la valeur null. La longueur de la valeur de chaîne non tronqué est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|24000|État de curseur non valide|L’argument *attribut* a été SQL_ATTR_ROW_NUMBER et le curseur n’est pas ouvert, ou le curseur est positionné avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans l’argument *MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLGetStmtAttr** fonction a été appelée.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|*(DM) \*ValuePtr* est une chaîne de caractères et BufferLength était inférieur à zéro, mais pas égale à SQL_NTS.|  
|HY092|Identificateur d’attribut/option non valide|La valeur spécifiée pour l’argument *attribut* n’était pas valide pour la version d’ODBC pris en charge par le pilote.|  
|HY109|Position du curseur non valide|Le *attribut* argument a été SQL_ATTR_ROW_NUMBER et la ligne a été supprimée ou qu’il ne peut pas être extraites.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l’argument *attribut* a un attribut d’instruction ODBC valide pour la version d’ODBC pris en charge par le pilote, mais n’était pas prise en charge par le pilote.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Pour obtenir des informations générales sur les attributs d’instruction, consultez [les attributs d’instruction](../../../odbc/reference/develop-app/statement-attributes.md).  
  
 Un appel à **SQLGetStmtAttr** retourne dans  *\*ValuePtr* la valeur de l’attribut spécifié dans l’instruction *attribut*. Cette valeur peut être une valeur SQLULEN ou une chaîne de caractères terminée par null. Si la valeur est une valeur SQLULEN, certains pilotes peuvent écrire uniquement 32 bits inférieurs ou le bit d’ordre supérieur de 16 bits d’une mémoire tampon et la laisser inchangée. Par conséquent, les applications doivent utiliser une mémoire tampon de SQLULEN et initialiser la valeur 0 avant d’appeler cette fonction. En outre, le *BufferLength* et *StringLengthPtr* arguments ne sont pas utilisés. Si la valeur est une chaîne terminée par null, l’application spécifie la longueur maximale de cette chaîne dans le *BufferLength* argument et le pilote retourne la longueur de cette chaîne dans le  *\*StringLengthPtr* mémoire tampon.  
  
 Pour autoriser les applications qui appellent **SQLGetStmtAttr** pour travailler avec ODBC 2. *x* pilotes, un appel à **SQLGetStmtAttr** est mappé dans le Gestionnaire de pilotes à **SQLGetStmtOption**.  
  
 Les attributs d’instruction suivants sont en lecture seule, peut donc être récupéré par **SQLGetStmtAttr**, mais pas définie par **SQLSetStmtAttr**:  
  
-   SQL_ATTR_IMP_PARAM_DESC  
  
-   SQL_ATTR_IMP_ROW_DESC  
  
-   SQL_ATTR_ROW_NUMBER  
  
 Pour obtenir la liste d’attributs qui peut être défini et récupérées, consultez [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Retourner la valeur de l’attribut de connexion|[SQLGetConnectAttr, fonction](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Définition d’un attribut de connexion|[SQLSetConnectAttr, fonction](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Définition d’un attribut d’instruction|[SQLSetStmtAttr, fonction](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
