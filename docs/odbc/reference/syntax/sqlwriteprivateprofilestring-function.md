---
title: Fonction de SQLWritePrivateProfileString | Documents Microsoft
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
- SQLWritePrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWritePrivateProfileString
helpviewer_keywords:
- SQLWritePrivateProfileString [ODBC]
ms.assetid: 526f36a4-92ed-4874-9725-82d27c0b86f9
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cd2ef7d8e5fdab610c5bc37f9e58c1dd031b80e9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlwriteprivateprofilestring-function"></a>SQLWritePrivateProfileString (fonction)
**Mise en conformité**  
 Version introduite : ODBC version 2.0  
  
 **Résumé**  
 **SQLWritePrivateProfileString** écrit un nom de la valeur et les données à la sous-clé Odbc.ini des informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLWritePrivateProfileString(  
     LPCSTR     lpszSection,  
     LPCSTR     lpszEntry,  
     LPCSTR     lpszString,  
     LPCSTR     lpszFilename);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszSection*  
 [Entrée] Pointe vers une chaîne terminée par le caractère null qui contient le nom de la section à laquelle la chaîne sera copiée. Si la section n’existe pas, il est créé. Le nom de la section est indépendante de la casse ; la chaîne peut être toute combinaison de lettres majuscules et les minuscules.  
  
 *lpszEntry*  
 [Entrée] Pointe vers une chaîne terminée par le caractère null qui contient le nom de la clé à associer à une chaîne. Si la clé n’existe pas dans la section spécifiée, il est créé. Si cet argument est NULL, la totalité de la section, y compris toutes les entrées dans la section, est supprimé.  
  
 *lpszString*  
 [Entrée] Pointe vers une chaîne se terminant par null à écrire dans le fichier. Si cet argument est NULL, la clé vers laquelle pointe le *lpszEntry* argument est supprimé.  
  
 *lpszFilename*  
 [Sortie] Pointe vers une chaîne se terminant par null qui désigne le fichier d’initialisation.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLWritePrivateProfileString** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_REQUEST_FAILED|Échoué de la demande|Les informations système demandé n’a pas pu être écrit.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLWritePrivateProfileString** est fournie comme un moyen simple pour les pilotes de ports et les DLL d’installation du pilote à partir de Microsoft® Windows® pour Microsoft Windows et Windows 2000. Les appels à **WritePrivateProfileString** qui écrivent une chaîne de profil dans le fichier Odbc.ini doit être remplacée par les appels à **SQLWritePrivateProfileString**. **SQLWritePrivateProfileString** appelle des fonctions dans l’API Win32® à ajouter le nom de valeur spécifié et les données à la sous-clé Odbc.ini des informations système.  
  
 Un mode de configuration indique l’entrée du fichier Odbc.ini répertoriant les valeurs de la source de données dans les informations système. Si la source de données est une source de données utilisateur (la variable d’état est USERDSN_ONLY), la fonction écrit à l’entrée de fichier Odbc.ini dans HKEY_CURRENT_USER. Si la source de données est une source de données système (SYSTEMDSN_ONLY), la fonction écrit à l’entrée de fichier Odbc.ini dans HKEY_LOCAL_MACHINE. Si la variable d’état est BOTHDSN, HKEY_CURRENT_USER est tentée, et en cas d’échec, HKEY_LOCAL_MACHINE est utilisé.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention d’une valeur à partir des informations système|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|
