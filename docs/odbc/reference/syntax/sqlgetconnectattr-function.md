---
title: Fonction SQLGetConnectAttr | Documents Microsoft
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
- SQLGetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConnectAttr
helpviewer_keywords:
- SQLGetConnectAttr function [ODBC]
ms.assetid: 2cb4ffa8-19d3-4664-8c2f-6682cdcc3f33
caps.latest.revision: 32
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 32334ddfa2e58a0e6ec80a50fda44e704d20dee4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetconnectattr-function"></a>Fonction SQLGetConnectAttr
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLGetConnectAttr** retourne la valeur actuelle de l’attribut de connexion.  
  
> [!NOTE]  
>  Pour plus d’informations sur ce que le Gestionnaire de pilotes mappe cette fonction pour lorsqu’un ODBC 3 *.x* application fonctionne avec une API ODBC 2 *.x* pilote, consultez [mappage des fonctions de remplacement pour la compatibilité descendante des Applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLGetConnectAttr(  
     SQLHDBC        ConnectionHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *Handle de connexion*  
 [Entrée] Handle de connexion.  
  
 *Attribut*  
 [Entrée] Attribut à récupérer.  
  
 *ValuePtr*  
 [Sortie] Pointeur vers la mémoire dans lequel retourner la valeur actuelle de l’attribut spécifié par *attribut*. Pour les attributs de type entier, certains pilotes peuvent écrire uniquement 32 bits inférieurs ou le bit d’ordre supérieur de 16 bits d’une mémoire tampon et la laisser inchangée. Par conséquent, les applications doivent utiliser une mémoire tampon de SQLULEN et initialiser la valeur 0 avant d’appeler cette fonction.  
  
 Si *ValuePtr* est NULL, *StringLengthPtr* retourne toujours le nombre total d’octets (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *ValuePtr*.  
  
 *BufferLength*  
 [Entrée] Si *attribut* est un attribut défini par ODBC et *ValuePtr* pointe vers une chaîne de caractères ou d’un tampon binaire, cet argument doit être la longueur de \* *ValuePtr*. Si *attribut* est un attribut défini par ODBC et \* *ValuePtr* est un entier, *BufferLength* est ignoré. Si la valeur de  *\*ValuePtr* est une chaîne Unicode (lors de l’appel **SQLGetConnectAttrW**), la *BufferLength* l’argument doit être un nombre pair.  
  
 Si *attribut* est un attribut définies par le pilote, l’application indique la nature de l’attribut pour le Gestionnaire de pilotes en définissant le *BufferLength* argument. *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si  *\*ValuePtr* est un pointeur vers une chaîne de caractères *BufferLength* est la longueur de la chaîne.  
  
-   Si  *\*ValuePtr* est un pointeur vers une mémoire tampon binaire, les emplacements de l’application le résultat de la SQL_LEN_BINARY_ATTR (*longueur*) macro dans *BufferLength*. Il s’ensuit une valeur négative dans *BufferLength*.  
  
-   Si  *\*ValuePtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si  *\*ValuePtr* contient un type de données de longueur fixe, *BufferLength* est SQL_IS_INTEGER ou SQL_IS_UINTEGER, selon le cas.  
  
 *StringLengthPtr*  
 [Sortie] Un pointeur vers une mémoire tampon dans lequel retourner le nombre total d’octets (sans le caractère de fin de la valeur null) disponibles à renvoyer dans \* *ValuePtr*. Si \* *ValuePtr* est un pointeur null, aucune longueur n’est retournée. Si la valeur d’attribut est une chaîne de caractères et le nombre d’octets à retourner est supérieur à *BufferLength* moins la longueur du caractère de fin de null, les données de  *\*ValuePtr* est tronqué à *BufferLength* moins la longueur du caractère de fin de null et se termine par null par le pilote.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetConnectAttr** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu à partir de la structure de données de diagnostic en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_DBC et un *gérer* de *handle de connexion*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLGetConnectAttr** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Les données retournées dans \* *ValuePtr* a été tronquée pour être *BufferLength* moins la longueur d’un caractère de fin de la valeur null. La longueur de la valeur de chaîne non tronqué est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08003|Connexion non ouverte|(DM) un *attribut* valeur nécessitant une connexion ouverte a été spécifiée.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné à partir de la structure de données de diagnostic par l’argument *MessageText* dans **SQLGetDiagField** décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire qui est requis pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) **SQLBrowseConnect** a été appelé pour le *handle de connexion* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant **SQLBrowseConnect** a retourné SQL_SUCCESS_WITH_INFO ou SQL_SUCCESS.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *handle de connexion* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM)  *\*ValuePtr* est une chaîne de caractères et BufferLength était inférieur à zéro, mais pas égale à SQL_NTS.|  
|HY092|Identificateur d’attribut/option non valide|La valeur spécifiée pour l’argument *attribut* n’était pas valide pour la version d’ODBC pris en charge par le pilote.|  
|HY114|Pilote ne prend pas en charge l’exécution d’une fonction asynchrone du niveau de connexion|(DM) une application a tenté d’activer l’exécution d’une fonction asynchrone avec sql_attr_async_dbc_functions_enable ne pour un pilote qui ne prend pas en charge les opérations de connexion asynchrone.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l’argument *attribut* a un attribut de connexion ODBC valide pour la version d’ODBC pris en charge par le pilote, mais n’était pas prise en charge par le pilote.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote qui correspond à la *handle de connexion* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Pour obtenir des informations générales sur les attributs de connexion, consultez [attributs de connexion](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Pour obtenir la liste des attributs qui peuvent être définies, consultez [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md). Notez que se *attribut* spécifie un attribut qui retourne une chaîne, *ValuePtr* doit être un pointeur vers une mémoire tampon pour la chaîne. La longueur maximale de la chaîne retournée, y compris le caractère null, sera *BufferLength* octets.  
  
 En fonction de l’attribut, une application ne possède pas d’établir une connexion avant d’appeler **SQLGetConnectAttr**. Toutefois, si **SQLGetConnectAttr** est appelée et l’attribut spécifié n’a pas de valeur par défaut et n’a pas été définie par un appel antérieur à **SQLSetConnectAttr**, **SQLGetConnectAttr** retourne SQL_NO_DATA.  
  
 Si *attribut* est SQL_ATTR_ TRACE ou un fichier de trace SQL_ATTR_, *handle de connexion* ne doivent pas être valide, et **SQLGetConnectAttr** ne renvoie pas SQL_ERROR ou SQL_INVALID_HANDLE si *handle de connexion* n’est pas valide. Ces attributs s’appliquent à toutes les connexions. **SQLGetConnectAttr** retourne SQL_ERROR ou SQL_INVALID_HANDLE si un autre argument n’est pas valide.  
  
 Bien qu’une application peut définir des attributs de l’instruction à l’aide de **SQLSetConnectAttr**, une application ne peut pas utiliser **SQLGetConnectAttr** pour récupérer l’attribut d’instruction valeurs ; il doit appeler **SQLGetStmtAttr** pour récupérer le paramètre d’attributs d’instruction.  
  
 Les attributs de connexion SQL_ATTR_AUTO_IPD et SQL_ATTR_CONNECTION_DEAD peuvent être retournées par un appel à **SQLGetConnectAttr** mais ne peut pas être définie par un appel à **SQLSetConnectAttr**.  
  
> [!NOTE]  
>  Il n’existe aucune prise en charge asynchrone pour **SQLGetConnectAttr**. Lors de l’implémentation **SQLGetConnectAttr**, un pilote peut améliorer les performances en réduisant le nombre de fois où des informations sont envoyées ou demandées à partir du serveur.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Retourner le paramètre d’un attribut d’instruction|[SQLGetStmtAttr, fonction](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Définition d’un attribut de connexion|[SQLSetConnectAttr, fonction](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Définition d’un attribut d’environnement|[SQLSetEnvAttr, fonction](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Définition d’un attribut d’instruction|[SQLSetStmtAttr, fonction](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
