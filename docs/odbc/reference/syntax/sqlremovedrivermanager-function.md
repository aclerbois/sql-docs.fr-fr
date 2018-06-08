---
title: Fonction de SQLRemoveDriverManager | Documents Microsoft
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
- SQLRemoveDriverManager
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDriverManager
helpviewer_keywords:
- SQLRemoveDriverManager function function [ODBC]
ms.assetid: 3a41511f-6603-4b81-a815-7883874023c4
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ad7508ef6825bda50adee02fe92665e4d3445ee5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlremovedrivermanager-function"></a>SQLRemoveDriverManager (fonction)
**Mise en conformité**  
 Version présenté : ODBC 3.0 : déconseillés dans Windows XP Service Pack 2, Windows Server 2003 Service Pack 1 et les systèmes d’exploitation ultérieurs.  
  
 **Résumé**  
 **SQLRemoveDriverManager** modifie ou supprime des informations sur les composants principaux ODBC à partir de l’entrée de fichier Odbcinst.ini dans les informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLRemoveDriverManager(  
     LPDWORD     pdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *pdwUsageCount*  
 [Sortie] Le nombre d’utilisations du Gestionnaire de pilotes une fois que cette fonction a été appelée.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec. Si aucune entrée n’existe dans les informations système lorsque cette fonction est appelée, la fonction retourne FALSE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLRemoveDriverManager** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Composant introuvable dans le Registre|Le programme d’installation n’a pas pu supprimer les informations du Gestionnaire de pilotes, car il n’existe pas dans le Registre ou est introuvable dans le Registre.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou décrémenter le décompte d’utilisation de composant|Le programme d’installation a échoué décrémenter le décompte d’utilisation du Gestionnaire de pilotes.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLRemoveDriverManager** complète la **SQLInstallDriverManager** fonction et les mises à jour le composant décompte d’utilisation dans les informations système. Cette fonction doit être appelée uniquement à partir d’une application d’installation.  
  
 **SQLRemoveDriverManager** décrémente le décompte d’utilisation du composant core par 1. Si le nombre d’utilisations de composant passe à 0, les informations de système d’entrée seront supprimées. L’entrée de composant principal est à l’emplacement suivant dans les informations système, sous le titre « ODBC Core » :  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
> [!CAUTION]  
>  Une application ne supprimez pas physiquement les fichiers du Gestionnaire de pilotes lorsque le nombre d’utilisations de composant et le nombre d’utilisations de fichier atteint zéro.  
  
 **SQLRemoveDriverManager** ne supprime pas réellement les fichiers. Le programme appelant est responsable de la suppression de fichiers et la gestion de l’utilisation du fichier de compte. Les fichiers du Gestionnaire de pilotes ne doit pas, être supprimé lorsque le nombre d’utilisations de composant et le nombre d’utilisations de fichier ont atteint zéro, car ces fichiers peuvent être utilisés par d’autres applications qui n’ont pas incrémenté le décompte d’utilisation de fichier.  
  
 **SQLRemoveDriverManager** est appelée dans le cadre du processus de désinstallation. Les composants principaux ODBC (y comprennent le Gestionnaire de pilotes, bibliothèque de curseurs, programme d’installation, bibliothèque de langue, administrateur, fichiers thunking et ainsi de suite) sont désinstallés dans sa globalité. Les fichiers suivants ne sont pas supprimés lorsque **SQLRemoveDriverManager** est appelée dans le cadre du processus de désinstallation :  
  
|||  
|-|-|  
|ODBC32DLL|ODBCCP32. DLL|  
|ODBCCR32. DLL|ODBC16GT. DLL|  
|ODBCCU32. DLL|ODBC32GT. DLL|  
|ODBCINT. DLL|DS16GT. DLL|  
|ODBCTRAC. DLL|DS32GT. DLL|  
|MSVCRT40. DLL|ODBCAD32. EXE|  
|ODBCCP32. PANNEAU DE CONFIGURATION||  
  
 **SQLRemoveDriverManager** est également appelée dans le cadre d’un processus de mise à niveau. Si une application détecte qu’il est à effectuer une mise à niveau et qu’il a installé précédemment le pilote, le pilote doit être supprimé et réinstallé.  
  
 **SQLRemoveDriverManager** doit tout d’abord être appelé pour décrémenter le décompte d’utilisation de composant. **SQLInstallDriverEx** doit ensuite être appelé pour incrémenter le décompte d’utilisation de composant. Le programme d’installation d’application doit remplacer les anciens fichiers de composant de base avec les nouveaux fichiers. Le nombre d’utilisations de fichier reste la même, et les autres applications qui utilisent les anciens fichiers de composant core version utilise désormais les fichiers de version plus récente.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|L’installation d’un gestionnaire de pilotes|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
