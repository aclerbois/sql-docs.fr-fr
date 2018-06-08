---
title: Fonction de SQLReadFileDSN | Documents Microsoft
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
- SQLReadFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLReadFileDSN
helpviewer_keywords:
- SQLReadFileDSN function [ODBC]
ms.assetid: ead464aa-cdc3-47dd-a0c0-997711205d31
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 32804ed9657a4438c3509fc57b266eb97af9db5b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlreadfiledsn-function"></a>SQLReadFileDSN (fonction)
**Mise en conformité**  
 Version introduite : ODBC 3.0  
  
 **Résumé**  
 **SQLReadFileDSN** lit les informations dans un fichier DSN.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLReadFileDSN(  
     LPCSTR   lpszFileName,  
     LPCSTR   lpszAppName,  
     LPCSTR   lpszKeyName,  
     LPSTR    lpszString,  
     WORD     cbString,  
     WORD *   pcbString);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszFileName*  
 [Entrée] Pointeur vers le tampon de données contenant le nom du fichier .dsn. Une extension .dsn est ajoutée à tous les noms de fichiers qui n’ont pas déjà d’une extension .dsn. La valeur de  *\*lpszFileName* doit être une chaîne se terminant par null.  
  
 *lpszAppName*  
 [Entrée] Pointeur vers le tampon de données contenant le nom de l’application. C’est la section ODBC « ODBC ». La valeur de  *\*lpszAppName* doit être une chaîne se terminant par null.  
  
 *lpszKeyName*  
 [Entrée] Pointeur vers le tampon de données contenant le nom de la clé à lire. Consultez « Commentaires » pour les mots clés réservés. La valeur de  *\*lpszAppName* doit être une chaîne se terminant par null.  
  
 *lpszString*  
 [Sortie] Pointeur vers le tampon de données qui contient la chaîne associée à la clé à lire.  
  
 Si  *\*lpszFileName* est un nom de fichier .dsn valide mais la *lpszAppName* argument est un pointeur null et les *lpszKeyName* argument est un pointeur null, puis  *\*lpszString* contient une liste d’applications valides. Si  *\*lpszFileName* est un nom de fichier .dsn valide et  *\*lpszAppName* est un nom d’application valide, mais la *lpszKeyName* argument est un pointeur null, puis  *\*lpszString* contient une liste de mots clés réservés valides dans la section appropriée du fichier source de données, séparée par des points-virgules. Si  *\*lpszFileName* est un nom de fichier .dsn valide mais  *\*lpszAppName* est un pointeur null et les *lpszKeyName* argument est un pointeur null, puis  *\*lpszString* contient une liste des sections dans le fichier de source de données, séparées par des points-virgules.  
  
 *cbString*  
 [Entrée] Longueur de la  *\*lpszString* mémoire tampon.  
  
 *pcbString*  
 [Sortie] Nombre total d’octets disponibles à renvoyer dans  *\*lpszString*. Si le nombre d’octets à retourner est supérieur ou égal à *cbString*, la chaîne de sortie dans  *\*lpszString* est tronqué à *cbString* moins le caractère de fin de la valeur null. Le *pcbString* argument peut être un pointeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLReadFileDSN** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|Le *lpszString* argument était NULL.<br /><br /> Le *cbString* argument était inférieur ou égal à 0.|  
|ODBC_ERROR_INVALID_PATH|Le chemin d’installation non valide|Le chemin d’accès du nom de fichier spécifié dans le *lpszFileName* argument n’était pas valide.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|Le *lpszAppName* argument a la valeur NULL, alors que le *lpszKeyName* argument était valide.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
|ODBC_ERROR_OUTPUT_STRING_TRUNCATED|Chaîne de sortie tronquée|La chaîne retournée dans  *\*lpszString* a été tronqué, car la valeur de *cbString* était inférieur ou égal à la valeur de  *\*pcbString*.|  
|ODBC_ERROR_REQUEST_FAILED|Échoué de la demande|Le mot clé n’existe pas dans le fichier de source de données.|  
  
## <a name="comments"></a>Commentaires  
 ODBC réserve le nom de la section [ODBC] dans lequel stocker les informations de connexion. Les mots clés réservés pour cette section sont les mêmes que celles réservées pour une chaîne de connexion dans **SQLDriverConnect**. (Pour plus d’informations, consultez la [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) description de fonction.)  
  
 Applications peuvent utiliser ces mots clés réservés pour lire les informations dans un fichier DSN. Si une application souhaite connaître la chaîne de connexion sans DSN associée à un fichier DSN, il peut appeler **SQLReadFileDSN** pour un des mots clés de chaîne de connexion réservé dans la section [ODBC]. La chaîne de connexion complète passée dans une connexion sans DSN est une combinaison de tous les mots clés (réservées et spécifiques au pilote) dans la section [ODBC].  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Écrire des informations dans un fichier DSN|[SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)|
