---
title: Fonction de SQLConfigDriver | Documents Microsoft
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
- SQLConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDriver
helpviewer_keywords:
- SQLConfigDriver function [ODBC]
ms.assetid: 4f681961-ac9f-4d88-b065-5258ba112642
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1a5fc4ac9aebbabf0181d42ae9d026aa697e8b58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlconfigdriver-function"></a>SQLConfigDriver (fonction)
**Mise en conformité**  
 Version introduite : ODBC 2.5  
  
 **Résumé**  
 **SQLConfigDriver** charge la DLL d’installation du pilote approprié et appelle le **ConfigDriver** (fonction).  
  
 La fonctionnalité de **SQLConfigDriver** sont également accessibles avec [ODBCCONF. EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLConfigDriver(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszArgs,  
     LPSTR    lpszMsg,  
     WORD     cbMsgMax,  
     WORD *   pcbMsgOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 [Entrée] Handle de fenêtre parente. La fonction n’affichera pas les boîtes de dialogue si le handle est null.  
  
 *fréquents*  
 [Entrée] Type de requête. *fréquents* doit contenir l’une des valeurs suivantes :  
  
 ODBC_CONFIG_DRIVER : Modifie le délai de connexion multiple utilisé par le pilote.  
  
 ODBC_INSTALL_DRIVER : Installe un nouveau pilote.  
  
 ODBC_REMOVE_DRIVER : Supprime un pilote existant.  
  
 Cette option peut également être spécifique au pilote, auquel cas la *fréquents* pour la première option doit démarrer à partir de ODBC_CONFIG_DRIVER_MAX + 1. Le *fréquents* pour toute autre option doit également démarrer à partir d’une valeur supérieure à ODBC_CONFIG_DRIVER_MAX + 1.  
  
 *lpszDriver*  
 [Entrée] Le nom du pilote tel qu’enregistré dans les informations système.  
  
 *lpszArgs*  
 [Entrée] Une chaîne terminée par le caractère null qui contient des arguments pour un pilote spécifique *fréquents*.  
  
 *lpszMsg*  
 [Sortie] Chaîne terminée par le caractère null qui contient un message de sortie à partir de l’installation du pilote.  
  
 *cbMsgMax*  
 [Entrée] Longueur de *lpszMsg.*  
  
 *pcbMsgOut*  
 [Sortie] Nombre total d’octets disponibles à renvoyer dans *lpszMsg*. Si le nombre d’octets à retourner est supérieur ou égal à *cbMsgMax*, le message de sortie dans *lpszMsg* est tronqué à *cbMsgMax* moins le caractère de fin de la valeur null. Le *pcbMsgOut* argument peut être un pointeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLConfigDriver** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|Le *lpszMsg* argument n’était pas valide.|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|Le *hwndParent* argument n’était pas valide.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|Le *fréquents* argument n’est pas une des opérations suivantes :<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> Le *fréquents* argument a une option spécifique au pilote qui était inférieur ou égal à ODBC_CONFIG_DRIVER_MAX.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|Le *lpszDriver* argument n’était pas valide. Il est introuvable dans le Registre.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé-valeur non valide|Le *lpszArgs* argument contenait une erreur de syntaxe.|  
|ODBC_ERROR_REQUEST_FAILED|*Demande* a échoué|Le programme d’installation n’a pas pu effectuer l’opération demandée par le *fréquents* argument. L’appel à **ConfigDriver** a échoué.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Impossible de charger la bibliothèque le programme d’installation de pilote ou de convertisseur|La bibliothèque d’installation du pilote n’a pas pu être chargée.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLConfigDriver** permet à une application d’appeler d’un pilote **ConfigDriver** routine sans avoir à connaître le nom et de charger la DLL d’installation spécifiques au pilote. Un programme d’installation appelle cette fonction après l’installation du pilote que DLL a été installé. Le programme appelant doit être conscient que cette fonction n’est peut-être pas disponible pour tous les pilotes. Dans ce cas, le programme appelant doit continuer sans erreur.  
  
## <a name="driver-specific-options"></a>Options spécifiques au pilote  
 Une application peut demander des fonctionnalités spécifiques au pilote exposées par le pilote à l’aide de la *fréquents* argument. Le *fréquents* pour la première option est ODBC_CONFIG_DRIVER_MAX + 1, et des options supplémentaires seront incrémentées de 1 à partir de cette valeur. Tous les arguments requis par le pilote pour cette fonction doit être fournie dans une chaîne se terminant par null passé dans le *lpszArgs* argument. Ces fonctionnalités de pilotes doivent gérer une table des options spécifiques au pilote. Les options doivent être entièrement documentées dans la documentation du pilote. Les auteurs d’applications qui utilisent des options spécifiques au pilote doivent être conscients que cette utilisation permettront à l’application moins interopérable.  
  
## <a name="setting-connection-pooling-timeout"></a>Paramètre de délai d’attente de regroupement de connexions  
 Les propriétés de délai d’attente de regroupement de connexions peuvent être définie lorsque vous définissez la configuration du pilote. **SQLConfigDriver** est appelée avec un *fréquents* de ODBC_CONFIG_DRIVER et *lpszArgs* la valeur **CPTimeout**. **CPTimeout** détermine la durée pendant laquelle une connexion peut rester dans le pool de connexions sans être utilisées. Lorsque le délai d’attente expire, la connexion est fermée et supprimée du pool. Le délai d’attente par défaut est 60 secondes.  
  
 Lorsque **SQLConfigDriver** est appelée avec *fréquents* valeur ODBC_INSTALL_DRIVER ou ODBC_REMOVE_DRIVER, le Gestionnaire de pilotes charge la DLL d’installation du pilote approprié et appelle le **ConfigDriver** (fonction). Lorsque **SQLConfigDriver** est appelée avec un *fréquents* de ODBC_CONFIG_DRIVER, tout le traitement est effectué dans le programme d’installation ODBC, afin que le programme d’installation du pilote DLL n’ait pas à charger.  
  
## <a name="messages"></a>Messages  
 Une routine d’installation de pilote peut envoyer un message texte à une application en tant que chaînes se terminant par null le *lpszMsg* mémoire tampon. Le message est tronqué à *cbMsgMax* moins le caractère de fin de la valeur null par le **ConfigDriver** fonctionner s’il est supérieur ou égal à *cbMsgMax* caractères.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’un pilote|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md)(dans la DLL d’installation)|  
|Suppression de la source de données par défaut|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|
