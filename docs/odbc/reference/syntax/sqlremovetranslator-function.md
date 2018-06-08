---
title: Fonction de SQLRemoveTranslator | Documents Microsoft
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
- SQLRemoveTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveTranslator
helpviewer_keywords:
- SQLRemoveTranslator function [ODBC]
ms.assetid: c6feda49-0359-4224-8de9-77125cf2397b
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 495348c07ea707907f664358daea510951e7b208
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlremovetranslator-function"></a>SQLRemoveTranslator (fonction)
**Mise en conformité**  
 Version introduite : ODBC 3.0  
  
 **Résumé**  
 **SQLRemoveTranslator** supprime les informations relatives à un traducteur à partir de la section du fichier Odbcinst.ini des informations système et décrémente décompte d’utilisation du composant de conversion de 1.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLRemoveTranslator(  
     LPCSTR    lpszTranslator,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszTranslator*  
 [Entrée] Le nom du traducteur tel qu’enregistré dans la clé Odbcinst.ini des informations système.  
  
 *lpdwUsageCount*  
 [Sortie] Le nombre d’utilisations du traducteur une fois que cette fonction a été appelée.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec. Si aucune entrée n’existe dans les informations système lorsque cette fonction est appelée, la fonction retourne FALSE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLRemoveTranslator** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Composant introuvable dans le Registre|Le programme d’installation n’a pas pu supprimer les informations de traducteur, car il n’existe pas dans le Registre ou est introuvable dans le Registre.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|Le *lpszTranslator* argument n’était pas valide.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou décrémenter le décompte d’utilisation de composant|Le programme d’installation a échoué décrémenter le décompte d’utilisation du pilote.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLRemoveTranslator** complète la [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md) fonction et les mises à jour le composant décompte d’utilisation dans les informations système. Cette fonction doit être appelée uniquement à partir d’une application d’installation.  
  
 **SQLRemoveTranslator** décrémente le décompte d’utilisation de composant par 1. Si le nombre d’utilisations de composant passe à 0, l’entrée de convertisseur dans les informations système sera supprimée. L’entrée de convertisseur est à l’emplacement suivant dans les informations système, sous le nom de la traduction :  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
 **SQLRemoveTranslator** ne supprime pas réellement les fichiers. Le programme appelant est responsable de la suppression de fichiers et en conservant le décompte d’utilisation de fichier. Uniquement lorsque le nombre d’utilisations de composant et le nombre d’utilisations de fichier ont atteint zéro est un fichier supprimé physiquement. Certains fichiers dans un composant peuvent être supprimés, et d’autres pas supprimés, selon si les fichiers sont utilisés par d’autres applications qui ont incrémenté le décompte d’utilisation de fichier.  
  
 **SQLRemoveTranslator** est également appelée dans le cadre d’un processus de mise à niveau. Si une application détecte qu’il est à effectuer une mise à niveau et qu’il a installé précédemment le pilote, le pilote doit être supprimé et réinstallé. **SQLRemoveTranslator** doit tout d’abord être appelé pour décrémenter le décompte d’utilisation du composant, puis **SQLInstallTranslatorEx** doit être appelé pour incrémenter le décompte d’utilisation de composant. Le programme d’installation d’application doit physiquement Remplacez les anciens fichiers par les nouveaux fichiers. Le décompte d’utilisation de fichier reste la même, et les autres applications qui utilisent les anciens fichiers de version utilise désormais une version plus récente.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|L’installation d’un convertisseur|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|
