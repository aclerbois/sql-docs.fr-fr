---
title: Fonction de ConfigDriver | Documents Microsoft
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
- ConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDriver
helpviewer_keywords:
- ConfigDriver [ODBC]
ms.assetid: 9473f48f-bcae-4784-89c1-7839bad4ed13
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 100cf1cc68287c2eb5914176cb307f9a450344d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configdriver-function"></a>ConfigDriver (fonction)
**Mise en conformité**  
 Version introduite : ODBC 2.5  
  
 **Résumé**  
 **ConfigDriver** permet à un programme d’installation effectuer l’installation et désinstallation des fonctions sans nécessiter le programme d’appeler **ConfigDSN**. Cette fonction effectue les fonctions spécifiques au pilote telles que la création des informations spécifiques au pilote système et effectuer des conversions de la source de données lors de l’installation, ainsi que des modifications des informations de système de nettoyage pendant la désinstallation. Cette fonction est exposée par le programme d’installation du pilote DLL ou une DLL d’installation distinct.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL ConfigDriver(  
      HWND    hwndParent,  
      WORD    fRequest,  
      LPCSTR  lpszDriver,  
      LPCSTR  lpszArgs,  
      LPSTR   lpszMsg,  
      WORD    cbMsgMax,  
      WORD *  pcbMsgOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 [Entrée] Handle de fenêtre parente. La fonction n’affichera pas les boîtes de dialogue si le handle est null.  
  
 *fréquents*  
 [Entrée] Type de requête. Le *fréquents* argument doit contenir l’une des valeurs suivantes :  
  
 ODBC_INSTALL_DRIVER : Installer un nouveau pilote.  
  
 ODBC_REMOVE_DRIVER : Supprimer un pilote.  
  
 Cette option peut également être spécifique au pilote, auquel cas la *fréquents* argument pour la première option doit démarrer à partir de ODBC_CONFIG_DRIVER_MAX + 1. Le *fréquents* argument des options supplémentaires doive également démarrer à partir d’une valeur supérieure à ODBC_CONFIG_DRIVER_MAX + 1.  
  
 *lpszDriver*  
 [Entrée] Le nom du pilote tel qu’enregistré dans la clé Odbcinst.ini des informations système.  
  
 *lpszArgs*  
 [Entrée] Chaîne se terminant par null contenant les arguments pour un pilote spécifique *fréquents*.  
  
 *lpszMsg*  
 [Sortie] Chaîne se terminant par null qui contient un message de sortie à partir de l’installation du pilote.  
  
 *cbMsgMax*  
 [Entrée] Longueur de *lpszMsg*.  
  
 *pcbMsgOut*  
 [Sortie] Nombre total d’octets disponibles à renvoyer dans *lpszMsg*.  
  
 Si le nombre d’octets à retourner est supérieur ou égal à *cbMsgMax*, le message de sortie dans *lpszMsg* est tronqué à *cbMsgMax* moins le caractère de fin de la valeur null. Le *pcbMsgOut* argument peut être un pointeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **ConfigDriver** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur est validée dans la mémoire tampon erreur de programme d’installation par un appel à **SQLPostInstallerError** et peut être obtenu en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|Le *hwndParent* argument n’était pas valide.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|Le *fréquents* argument n’est pas une des opérations suivantes :<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> L’option spécifiques au pilote était inférieur ou égal à ODBC_CONFIG_DRIVER_MAX.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|Le *lpszDriver* argument n’était pas valide. Il est introuvable dans le Registre.|  
|ODBC_ERROR_REQUEST_FAILED|*Demande* a échoué|Impossible d’effectuer l’opération demandée par le *fréquents* argument.|  
|ODBC_ERROR_DRIVER_SPECIFIC|Erreur spécifique du pilote ou du traducteur|Une erreur spécifique au pilote pour lequel il n’existe aucune erreur de programme d’installation ODBC définie. Le *SzError* argument dans un appel à la **SQLPostInstallerError** la fonction doit contenir le message d’erreur spécifique au pilote.|  
  
## <a name="comments"></a>Commentaires  
  
### <a name="driver-specific-options"></a>Options spécifiques au pilote  
 Une application peut demander des fonctionnalités spécifiques au pilote exposées par le pilote à l’aide de la *fréquents* argument. Le *fréquents* pour la première option est ODBC_CONFIG_DRIVER_MAX plus 1, et des options supplémentaires seront incrémentées de 1 à partir de cette valeur. Tous les arguments requis par le pilote pour cette fonction doit être fournie dans une chaîne se terminant par null passé dans le *lpszArgs* argument. Ces fonctionnalités de pilotes doivent gérer une table des options spécifiques au pilote. Les options doivent être entièrement documentées dans la documentation du pilote. Les auteurs d’applications qui utilisent des options spécifiques au pilote doivent être conscients que cela permettra l’application moins interopérable.  
  
### <a name="messages"></a>Messages  
 Une routine d’installation de pilote peut envoyer un message texte à une application sous forme de chaîne se terminant par null dans le *lpszMsg* mémoire tampon. Le message est tronqué à *cbMsgMax* moins le caractère de fin de la valeur null par le **ConfigDriver** fonctionner s’il est supérieur ou égal à *cbMsgMax* caractères.
