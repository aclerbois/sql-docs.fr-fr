---
title: Fonction SQLConfigDataSource | Documents Microsoft
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
- SQLConfigDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDataSource
helpviewer_keywords:
- SQLConfigDataSource function [ODBC]
ms.assetid: f8d6e342-c010-434e-b1cd-f5371fb50a14
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ddff49350de9d4e713b065848ab53649b25cb094
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlconfigdatasource-function"></a>Fonction SQLConfigDataSource
**Mise en conformité**  
 Version introduite : ODBC 1.0  
  
 **Résumé**  
 **SQLConfigDataSource** ajoute, modifie ou supprime des sources de données.  
  
 La fonctionnalité de **SQLConfigDataSource** sont également accessibles avec [ODBCCONF. EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLConfigDataSource(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszAttributes);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 [Entrée] Handle de fenêtre parente. La fonction n’affichera pas les boîtes de dialogue si le handle est null.  
  
 *fréquents*  
 [Entrée] Type de requête. Le *fréquents* argument doit contenir l’une des valeurs suivantes :  
  
 ODBC_ADD_DSN : Ajouter une nouvelle source de données utilisateur.  
  
 ODBC_CONFIG_DSN : Configurer (modifier) une source de données utilisateur existante.  
  
 ODBC_REMOVE_DSN : Supprimer une source de données utilisateur existante.  
  
 ODBC_ADD_SYS_DSN : Ajouter une nouvelle source de données système.  
  
 ODBC_CONFIG_SYS_DSN : Modifier une source de données système.  
  
 ODBC_REMOVE_SYS_DSN : Supprimer une source de données système.  
  
 ODBC_REMOVE_DEFAULT_DSN : Supprimez la section de spécification de source de données par défaut à partir des informations système. (Elle supprime également la section Spécification du pilote par défaut de l’entrée de fichier Odbcinst.ini dans les informations système. Cela *fréquents* effectue la même fonction que déconseillées **SQLRemoveDefaultDataSource** fonction.) Lorsque cette option est spécifiée, tous les autres paramètres dans l’appel à **SQLConfigDataSource** doivent être des valeurs null ; s’ils ne sont pas NULL, elles seront ignorées.  
  
 *lpszDriver*  
 [Entrée] Description du pilote (généralement le nom du SGBD associé) présentée à l’utilisateur au lieu du nom de pilote physique.  
  
 *lpszAttributes*  
 [Entrée] Liste d’attributs sous la forme de paires mot clé-valeur doublement se terminant par null. Pour plus d’informations, consultez [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec. Si aucune entrée n’existe dans les informations système lorsque cette fonction est appelée, la fonction retourne FALSE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLConfigDataSource** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|Le *hwndParent* argument était non valide ou NULL.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|Le *fréquents* argument n’est pas une des opérations suivantes :<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN ODBC_ADD_SYS_DSN ODBC_CONFIG_SYS_DSN ODBC_REMOVE_SYS_DSN ODBC_REMOVE_DEFAULT_DSN|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|Le *lpszDriver* argument n’était pas valide. Il est introuvable dans le Registre.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé-valeur non valide|Le *lpszAttributes* argument contenait une erreur de syntaxe.|  
|ODBC_ERROR_REQUEST_FAILED|*Demande* a échoué|Le programme d’installation n’a pas pu effectuer l’opération demandée par le *fréquents* argument. L’appel à **ConfigDSN** a échoué.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Impossible de charger la bibliothèque le programme d’installation de pilote ou de convertisseur|La bibliothèque d’installation du pilote n’a pas pu être chargée.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLConfigDataSource** utilise la valeur de *lpszDriver* pour lire le chemin d’accès complet de la DLL d’installation pour le pilote à partir des informations système. Il charge la DLL et les appels **ConfigDSN** avec les mêmes arguments qu’ont passés à ce dernier.  
  
 **SQLConfigDataSource** renvoie la valeur FALSE s’il est impossible de trouver ou de charger la DLL d’installation ou si l’utilisateur annule la boîte de dialogue. Sinon, elle retourne l’état reçue **ConfigDSN**.  
  
 **SQLConfigDataSource** mappe le DSN système *fréquents*s pour la source de données utilisateur *fréquents*s (ODBC_ADD_SYS_DSN à ODBC_ADD_DSN, ODBC_CONFIG_SYS_DSN ODBC_CONFIG_DSN et ODBC_REMOVE_SYS_ DSN à ODBC_REMOVE_DSN). Pour distinguer les utilisateurs et les sources de données système, **SQLConfigDataSource** définit le programme d’installation en mode de configuration selon le tableau suivant. Avant de retourner, **SQLConfigDataSource** BOTHDSN rétablit le mode de configuration. **ConfigDSN** (implémenté par les pilotes) doit appeler **SQLWriteDSNToIni** et **SQLWritePrivateProfileString** pour prendre en charge un DSN système. Pour plus d’informations, consultez [ConfigDSN fonction](../../../odbc/reference/syntax/configdsn-function.md).  
  
|*fréquents*|Mode de configuration|  
|----------------|------------------------|  
|ODBC_ADD_DSN|USERDSN_ONLY|  
|ODBC_CONFIG_DSN|USERDSN_ONLY|  
|ODBC_REMOVE_DSN|USERDSN_ONLY|  
|ODBC_ADD_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_CONFIG_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_REMOVE_SYS_DSN|SYSTEMDSN_ONLY|  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’une source de données|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) (dans la DLL d’installation)|  
|Suppression d’un nom de source de données à partir des informations système|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|Ajout d’un nom de source de données pour les informations système|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
