---
title: Fonction de cas | Documents Microsoft
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
- SQLGetEnvAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetEnvAttr
helpviewer_keywords:
- SQLGetEnvAttr function [ODBC]
ms.assetid: 01f4590f-427a-4280-a1c3-18de9f7d86c1
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b1a31f7d374a06a4e9cfe963c92b73fa681a41d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetenvattr-function"></a>Cas (fonction)
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **Cas** retourne le paramètre actuel d’un attribut de l’environnement.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLGetEnvAttr(  
     SQLHENV        EnvironmentHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 [Entrée] Handle d’environnement.  
  
 *Attribut*  
 [Entrée] Attribut à récupérer.  
  
 *ValuePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner la valeur actuelle de l’attribut spécifié par *attribut*.  
  
 Si *ValuePtr* est NULL, *StringLengthPtr* retourne toujours le nombre total d’octets (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *ValuePtr*.  
  
 *BufferLength*  
 [Entrée] Si *ValuePtr* pointe vers une chaîne de caractères, cet argument doit être la longueur de \* *ValuePtr*. Si \* *ValuePtr* est un entier, *BufferLength* est ignoré. Si  *\*ValuePtr* est une chaîne Unicode (lors de l’appel **SQLGetEnvAttrW**), la *BufferLength* l’argument doit être un nombre pair. Si la valeur d’attribut n’est pas une chaîne de caractères *BufferLength* n’est pas utilisée.  
  
 *StringLengthPtr*  
 [Sortie] Un pointeur vers une mémoire tampon dans lequel retourner le nombre total d’octets (sans le caractère de fin de la valeur null) disponibles à renvoyer dans  *\*ValuePtr*. Si *ValuePtr* est un pointeur null, aucune longueur n’est retournée. Si la valeur d’attribut est une chaîne de caractères et le nombre d’octets à retourner est supérieur ou égal à *BufferLength*, les données de \* *ValuePtr* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null et se termine par null par le pilote.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **cas** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_ENV et un *gérer* de *EnvironmentHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **cas** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Les données retournées dans \* *ValuePtr* a été tronquée pour être *BufferLength* moins le caractère de fin de la valeur null. La longueur de la valeur de chaîne non tronqué est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) **SQL_ATTR_ODBC_VERSION** n’a pas encore été définie **SQLSetEnvAttr**. Vous n’avez pas besoin de définir **SQL_ATTR_ODBC_VERSION** explicitement si vous utilisez **SQLAllocHandleStd**.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY092|Identificateur d’attribut/option non valide|La valeur spécifiée pour l’argument *attribut* n’était pas valide pour la version d’ODBC pris en charge par le pilote.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l’argument *attribut* a un attribut d’environnement ODBC valide pour la version d’ODBC pris en charge par le pilote, mais n’était pas prise en charge par le pilote.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant à la *EnvironmentHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Pour obtenir la liste des attributs, consultez [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md). Il n’existe pas d’attributs spécifiques au pilote environnement. Si *attribut* spécifie un attribut qui retourne une chaîne, *ValuePtr* doit être un pointeur vers une mémoire tampon dans lequel retourner la chaîne. La longueur maximale de la chaîne, y compris l’octet de valeur null, sera *BufferLength* octets.  
  
 **Cas** peut être appelée à tout moment entre l’allocation et la libération d’un handle d’environnement. Tous les attributs d’environnement a été définis par l’application pour l’environnement sont conservés jusqu'à **SQLFreeHandle** est appelée sur le *EnvironmentHandle* avec un *HandleType* de SQL_HANDLE_ENV. Plus d’un handle d’environnement peut être alloué simultanément dans ODBC 3 *.x*. Un attribut de l’environnement sur un environnement n’est pas affecté quand un autre environnement a été alloué.  
  
> [!NOTE]  
>  L’attribut d’environnement SQL_ATTR_OUTPUT_NTS est pris en charge par les applications conformes aux normes. Lorsque **cas** est appelée, la version 3 ODBC *.x* du Gestionnaire de pilotes retourne toujours SQL_TRUE pour cet attribut. SQL_ATTR_OUTPUT_NTS peut être définie à SQL_TRUE uniquement par un appel à **SQLSetEnvAttr**.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Retourner la valeur de l’attribut de connexion|[SQLGetConnectAttr, fonction](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Retourner le paramètre d’un attribut d’instruction|[SQLGetStmtAttr, fonction](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Définition d’un attribut de connexion|[SQLSetConnectAttr, fonction](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Définition d’un attribut d’environnement|[SQLSetEnvAttr, fonction](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Définition d’un attribut d’instruction|[SQLSetStmtAttr, fonction](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
