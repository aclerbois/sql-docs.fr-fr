---
title: Fonction de SQLSetConfigMode | Documents Microsoft
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
- SQLSetConfigMode
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConfigMode
helpviewer_keywords:
- SQLSetConfigMode function [ODBC]
ms.assetid: 09eb88ea-b6f6-4eca-b19d-0951cebc6c0a
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 58cb5aabecfbccb3d8a2133f2d48e198e0fba0ce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetconfigmode-function"></a>SQLSetConfigMode (fonction)
**Mise en conformité**  
 Version introduite : ODBC 3.0  
  
 **Résumé**  
 **SQLSetConfigMode** définit le mode de configuration qui indique l’entrée du fichier Odbc.ini répertoriant les valeurs de la source de données dans les informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLSetConfigMode(  
     UWORD     wConfigMode);  
```  
  
## <a name="arguments"></a>Arguments  
 *wConfigMode*  
 [Entrée] Le mode de configuration du programme d’installation (voir « Commentaires »). La valeur de *wConfigMode* peut être :  
  
 ODBC_USER_DSN  
  
 ODBC_SYSTEM_DSN  
  
 ODBC_BOTH_DSN  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLSetConfigMode** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Séquence de paramètres non valide|Le *wConfigMode* argument ne contenait pas ODBC_USER_DSN, ODBC_SYSTEM_DSN ou ODBC_BOTH_DSN.|  
  
## <a name="comments"></a>Commentaires  
 Cette fonction est utilisée pour définir où l’entrée de fichier Odbc.ini répertoriant les valeurs de la source de données est dans les informations système. Si *wConfigMode* est ODBC_USER_DSN, la source de données est une source de données utilisateur et la fonction lit à partir de l’entrée de fichier Odbc.ini dans HKEY_CURRENT_USER. S’il s’agit de ODBC_SYSTEM_DSN, la source de données est une source de données système et la fonction lit à partir de l’entrée de fichier Odbc.ini dans HKEY_LOCAL_MACHINE. S’il s’agit de ODBC_BOTH_DSN, HKEY_CURRENT_USER est tentée, et en cas d’échec, HKEY_LOCAL_MACHINE est utilisé.  
  
 Cette fonction n’affecte pas **SQLCreateDataSource** et **SQLDriverConnect**. Le mode de configuration doit être définie lorsqu’un pilote lit à partir du Registre en appelant **SQLGetPrivateProfileString** ou écrit dans le Registre en appelant **SQLWritePrivateProfileString**. Les appels à **SQLGetPrivateProfileString** et **SQLWritePrivateProfileString** permet de savoir quelle partie du Registre pour fonctionner sur le mode de configuration.  
  
> [!CAUTION]  
>  **SQLSetConfigMode** doit être appelée uniquement lorsque cela est nécessaire ; si le mode est défini correctement, le programme d’installation ODBC peut ne pas fonctionner correctement.  
  
 **SQLSetConfigMode** apporte une modification directe du Registre du mode de configuration. Il s’agit en dehors du processus de modification du mode de configuration par un appel à **SQLConfigDataSource**. Un appel à **SQLConfigDataSource** définit le mode de configuration pour distinguer les sources de données système et utilisateur lors de la modification d’une source de données. Avant de retourner, **SQLConfigDataSource** BOTHDSN rétablit le mode de configuration.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Création d’une source de données|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|  
|Connexion à une source de données à l’aide d’une boîte de dialogue ou la chaîne de connexion|[SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Le mode de configuration de la récupération des|[SQLGetConfigMode](../../../odbc/reference/syntax/sqlgetconfigmode-function.md)|
