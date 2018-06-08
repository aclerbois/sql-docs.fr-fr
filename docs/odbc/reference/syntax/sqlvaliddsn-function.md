---
title: Fonction de SQLValidDSN | Documents Microsoft
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
- SQLValidDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLValidDSN
helpviewer_keywords:
- SQLValidDSN [ODBC]
ms.assetid: 930d1d89-337a-4429-85a2-84ee10555ac9
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4560f4645bf8e4e8c255b94c940b0922483cf5f6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlvaliddsn-function"></a>SQLValidDSN (fonction)
**Mise en conformité**  
 Version introduite : ODBC version 2.0  
  
 **Résumé**  
 **SQLValidDSN** vérifie la longueur et la validité du nom de la source de données avant que le nom est ajouté aux informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLValidDSN(  
     LPCSTR    lpszDSN);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszDSN*  
 [Entrée] Source de données à vérifier.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne la valeur TRUE si le nom de source de données est valide. Retourne FALSE si le nom de source de données n’est pas valide ou l’échec de l’appel de fonction.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLValidDSN** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. A  *\*pfErrorCode* est retourné uniquement si l’appel de fonction échoue, pas si la valeur FALSE est retournée, car le nom de source de données n’est pas valide. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLValidDSN** est appelée par un pilote de [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) pour vérifier la validité des caractères individuels dans le nom de source de données et la longueur du nom de la source de données. Il vérifie si la longueur du nom est supérieure à SQL_MAX_DSN_LENGTH, tel que défini dans Sqlext.h. (La longueur du nom de la source de données est également vérifiée par [SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md).) **SQLValidDSN** vérifie si un des caractères non valides suivants sont inclus dans le nom de source de données :  
  
 [ ] { } ( ) , ; ? * = ! @ \  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’une source de données|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) (dans la DLL d’installation)|  
|Ajout, modification ou suppression d’une source de données|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|L’écriture d’un nom de source de données pour les informations système|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
