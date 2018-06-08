---
title: Fonction de ConfigTranslator | Documents Microsoft
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
- ConfigTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigTranslator
helpviewer_keywords:
- ConfigTranslator [ODBC]
ms.assetid: 7c22f07e-36de-425b-aa67-e32a84afae92
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5b38bc6340ec456ce180eb2a9cc266d5b8a19305
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configtranslator-function"></a>ConfigTranslator (fonction)
**Mise en conformité**  
 Version introduite : ODBC version 2.0  
  
 **Résumé**  
 **ConfigTranslator** renvoie une option de traduction par défaut pour un traducteur. Il peut être dans la DLL de conversion ou d’une DLL d’installation distinct.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL ConfigTranslator(  
     HWND     hwndParent,  
     DWORD *  pvOption);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 [Entrée] Handle de fenêtre parente. La fonction n’affichera pas les boîtes de dialogue si le handle est null.  
  
 *pvOption*  
 [Sortie] Une option de traduction de 32 bits.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **ConfigTranslator** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur est validée dans la mémoire tampon erreur de programme d’installation par un appel à **SQLPostInstallerError** et peut être obtenu en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|Le *hwndParent* argument était non valide ou NULL.|  
|ODBC_ERROR_DRIVER_SPECIFIC|Erreur spécifique du pilote ou du traducteur|Une erreur spécifique au pilote pour lequel il n’existe aucune erreur de programme d’installation ODBC définie. Le *SzError* argument dans un appel à la **SQLPostInstallerError** la fonction doit contenir le message d’erreur spécifique au pilote.|  
|ODBC_ERROR_INVALID_OPTION|Option de conversion non valide|Le *pvOption* argument contenue une valeur non valide.|  
  
## <a name="comments"></a>Commentaires  
 Si le convertisseur prend en charge uniquement une option de traduction unique, **ConfigTranslator** retourne la valeur TRUE et définit *pvOption* à l’option 32 bits. Dans le cas contraire, il détermine l’option de traduction par défaut à utiliser. **ConfigTranslator** peut afficher une boîte de dialogue avec laquelle un utilisateur sélectionne une option de traduction par défaut.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention d’une option de traduction|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|En sélectionnant un traducteur|[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|  
|Définition d’une option de traduction|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
