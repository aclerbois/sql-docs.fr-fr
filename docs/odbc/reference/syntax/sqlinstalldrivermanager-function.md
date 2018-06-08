---
title: Fonction de SQLInstallDriverManager | Documents Microsoft
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
- SQLInstallDriverManager
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverManager
helpviewer_keywords:
- SQLInstallDriverManager function [ODBC]
ms.assetid: aebc439b-fffd-4d98-907a-0163f79aee8d
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 739af9f6d97dbd4595a3c18254ab53f2a9db10f6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlinstalldrivermanager-function"></a>SQLInstallDriverManager (fonction)
**Mise en conformité**  
 Version présenté : ODBC 1.0 : déconseillés dans Windows XP Service Pack 2, Windows Server 2003 Service Pack 1 et versions ultérieures  
  
 **Résumé**  
 **SQLInstallDriverManager** retourne le chemin d’accès du répertoire cible pour l’installation des composants ODBC. Le programme appelant doit réellement copiez le Gestionnaire de pilotes de fichiers dans le répertoire cible.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLInstallDriverManager(  
     LPSTR    lpszPath,  
     WORD     cbPathMax,  
     WORD *   pcbPathOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszPath*  
 [Sortie] Chemin d’accès du répertoire cible de l’installation.  
  
 *cbPathMax*  
 [Entrée] Longueur de *lpszPath*. Cela doit être au moins les octets _MAX_PATH.  
  
 *pcbPathOut*  
 [Sortie] Nombre total d’octets (à l’exception de l’octet de fin de null) retournée dans *lpszPath*. Si le nombre d’octets à retourner est supérieur ou égal à *cbPathMax*, le chemin d’accès dans *lpszPath* est tronqué à *cbPathMax* moins le caractère de fin de la valeur null. Le *pcbPathOut* argument peut être un pointeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLInstallDriverManager** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|Le *lpszPath* argument n’était pas suffisamment grande pour contenir le chemin de sortie. La mémoire tampon contient le chemin d’accès tronquée.<br /><br /> Le *cbPathMax* argument était inférieure à _MAX_PATH.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou décrémenter le décompte d’utilisation de composant|Le programme d’installation a échoué incrémenter le décompte d’utilisation ODBC core composant.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLInstallDriverManager** est appelée pour retourner le chemin d’accès pour comptent les composants principaux ODBC et l’incrément de l’utilisation du composant dans les informations système. Si une version du Gestionnaire de pilotes existe déjà, mais le nombre d’utilisations de composant pour le pilote n’existe pas, la nouvelle valeur de nombre de l’utilisation de composant est définie à 2.  
  
 Le programme d’installation d’application est chargé de la copie de physiquement les fichiers de composant de base et en conservant l’utilisation du fichier de compte. Si un fichier de composant de base n’a pas été précédemment installé, le programme d’installation d’application doit copier le fichier et créer le décompte d’utilisation de fichier. Si le fichier a déjà été installé, le programme d’installation incrémente simplement le nombre d’utilisations de fichier.  
  
 Si une version antérieure du Gestionnaire de pilotes a été précédemment installée par le programme d’installation d’application, les composants de base doivent être désinstallés et réinstallées, afin que le nombre d’utilisations du composant core n’est valide. **SQLRemoveDriverManager** doit tout d’abord être appelé pour décrémenter le décompte d’utilisation de composant. **SQLInstallDriverManager** doit ensuite être appelé pour incrémenter le décompte d’utilisation de composant. Le programme d’installation d’application doit remplacer les anciens fichiers de composant de base avec les nouveaux fichiers. Le nombre d’utilisations de fichier reste la même et autres applications qui a utilisé les anciens fichiers de composant core version utilise désormais les fichiers de version plus récente.  
  
 Dans une nouvelle installation des composants principaux ODBC, pilotes et convertisseurs, le programme d’installation d’application doit appeler les fonctions suivantes dans l’ordre : **SQLInstallDriverManager**, **SQLInstallDriverEx**, **SQLConfigDriver** (avec un *fréquents* de ODBC_INSTALL_DRIVER), puis **SQLInstallTranslatorEx**. Dans une désinstallation des composants principaux, des pilotes et des convertisseurs, le programme d’installation d’application doit appeler les fonctions suivantes dans l’ordre : **SQLRemoveTranslator**, **SQLRemoveDriver**, puis **SQLRemoveDriverManager**. Ces fonctions doivent être appelées dans cette séquence. Dans une mise à niveau de tous les composants, toutes les fonctions de désinstallation doivent être appelées dans la séquence et ensuite toutes les fonctions de l’installation doivent être appelées dans la séquence.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’un pilote|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|  
|Installation d’un pilote|[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|  
|L’installation d’un convertisseur|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|  
|Suppression d’un pilote|[SQLRemoveDriver](../../../odbc/reference/syntax/sqlremovedriver-function.md)|  
|Suppression du Gestionnaire de pilotes|[SQLRemoveDriverManager](../../../odbc/reference/syntax/sqlremovedrivermanager-function.md)|  
|Suppression d’un convertisseur|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
