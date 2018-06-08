---
title: Fonction de SQLInstallTranslatorEx | Documents Microsoft
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
- SQLInstallTranslatorEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallTranslatorEx
helpviewer_keywords:
- SQLInstallTranslatorEx function [ODBC]
ms.assetid: a0630602-53c1-4db0-98ce-70d160aedf8d
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2bf6eda5909aa7cec78a2c23e35126c90f566117
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlinstalltranslatorex-function"></a>SQLInstallTranslatorEx (fonction)
**Mise en conformité**  
 Version introduite : ODBC 3.0  
  
 **Résumé**  
 **SQLInstallTranslatorEx** ajoute des informations sur un traducteur à la section Odbcinst.ini des informations système (HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST. INI\ODBC traducteurs clé de Registre).  
  
 La fonctionnalité de **SQLInstallTranslatorEx** sont également accessibles avec [ODBCCONF. EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLInstallTranslatorEx(  
     LPCSTR    lpszTranslator,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszTranslator*  
 [Entrée] Cela doit contenir une liste doublement se terminant par null des paires mot clé-valeur décrivant le traducteur. Pour plus d’informations sur la syntaxe de la paire mot clé-valeur, consultez [traducteur spécification sous-clés](../../../odbc/reference/install/translator-specification-subkeys.md).  
  
 Le **traducteur** et **le programme d’installation** mots clés doivent être inclus dans le *lpszTranslator* chaîne. La traduction de DLL est répertorié avec le **traducteur** (mot clé) et le programme d’installation du traducteur DLL est répertorié avec le **le programme d’installation** (mot clé). Chaque paire se termine par un octet NULL, et la liste entière se termine par un octet NULL. (Autrement dit, deux octets NULL marquent la fin de la liste.) Le format de *lpszTranslator* est comme suit :  
  
 \0Translator=*traducteur-DLL-nom de fichier*\0[Setup=*le programme d’installation-DLL-nom de fichier*\0]\0  
  
 *lpszPathIn*  
 [Entrée] Chemin d’accès complet où le traducteur doit être installé ou un pointeur null. Si *lpszPath* est un pointeur null, les convertisseurs seront installés dans le répertoire système.  
  
 *lpszPathOut*  
 [Sortie] Le chemin d’accès du répertoire cible où le traducteur doit être installé. Si le convertisseur n’a jamais été installé, *lpszPathOut* est identique à *lpszPathIn*. S’il existe une installation antérieure du traducteur, *lpszPathOut* est le chemin d’accès de l’installation précédente.  
  
 *cbPathOutMax*  
 [Entrée] Longueur de *lpszPathOut.*  
  
 *pcbPathOut*  
 [Sortie] Nombre total d’octets disponibles à renvoyer dans *lpszPathOut*. Si le nombre d’octets à retourner est supérieur ou égal à *cbPathOutMax*, le chemin de sortie dans *lpszPathOut* est tronqué à *pcbPathOutMax* moins le caractère de fin de la valeur null. Le *pcbPathOut* argument peut être un pointeur null.  
  
 *fréquents*  
 [Entrée] Type de requête. *fréquents* doit contenir l’une des valeurs suivantes :  
  
 ODBC_INSTALL_INQUIRY : Savoir où un traducteur peut être installé.  
  
 ODBC_INSTALL_COMPLETE : Terminer la requête de l’installation.  
  
 *lpdwUsageCount*  
 [Sortie] Le nombre d’utilisations du traducteur une fois que cette fonction a été appelée.  
  
 Applications ne doivent pas définir le nombre d’utilisations. ODBC conserve ce nombre.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLInstallTranslatorEx** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|Le *lpszPathOut* argument n’était pas suffisamment grande pour contenir le chemin de sortie. La mémoire tampon contient le chemin d’accès tronquée.<br /><br /> Le *cbPathOutMax* argument était 0 et le *fréquents* argument a été ODBC_INSTALL_COMPLETE.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|Le *fréquents* argument n’est pas une des opérations suivantes :<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé-valeur non valide|Le *lpszTranslator* argument contenait une erreur de syntaxe.|  
|ODBC_ERROR_INVALID_PATH|Le chemin d’installation non valide|Le *lpszPathIn* argument contenait un chemin d’accès non valide.|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Séquence de paramètres non valide|Le *lpszTranslator* argument ne contenait pas d’une liste de paires mot clé-valeur.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou décrémenter le décompte d’utilisation du composant du Registre|Le programme d’installation a échoué incrémenter le décompte d’utilisation de conversion.|  
  
## <a name="comments"></a>Commentaires  
 **SQLInstallTranslatorEx** fournit un mécanisme permettant d’installer uniquement le traducteur. Cette fonction ne copie pas réellement des fichiers. Le programme appelant est responsable de la copie des fichiers de traduction.  
  
 **SQLInstallTranslatorEx** incrémente le décompte d’utilisation de composant pour le traducteur installé par 1. Si une version du traducteur existe déjà, mais le nombre d’utilisations de composant pour le traducteur n’existe pas, la nouvelle valeur de nombre de l’utilisation de composant est définie à 2.  
  
 Le programme d’installation d’application est chargé de physiquement copie du fichier de traduction et en conservant le décompte d’utilisation de fichier. Si le fichier de traduction n’a pas été précédemment installé, le programme d’installation d’application doit copier les fichiers et créer l’ou les fichiers de décompte d’utilisation. Si le fichier a déjà été installé, le programme d’installation incrémente le décompte d’utilisation de fichier.  
  
 Si une version antérieure du traducteur a été précédemment installée par l’application, le convertisseur doit être désinstallé et réinstallé, afin que le nombre d’utilisations du composant traducteur n’est valide. **SQLRemoveTranslator** doit être appelé pour décrémenter le décompte d’utilisation du composant, puis **SQLInstallTranslatorEx** doit être appelé pour incrémenter le décompte d’utilisation de composant. Le programme d’installation d’application doit remplacer les anciens fichiers avec le nouveau fichier. Le décompte d’utilisation de fichier reste la même, et les autres applications qui utilisent ce fichier de version antérieur utilise maintenant la version plus récente.  
  
 La longueur du chemin d’accès dans *lpszPathOut* dans **SQLInstallTranslatorEx** permet un processus d’installation en deux phases, pour une application peut déterminer quel *cbPathOutMax* doit être en appelant **SQLInstallTranslatorEx** avec un *fréquents* du mode ODBC_INSTALL_INQUIRY. Cela retourne le nombre total d’octets disponible dans le *pcbPathOut* mémoire tampon. **SQLInstallTranslatorEx** peut ensuite être appelée avec un *fréquents* de ODBC_INSTALL_COMPLETE et *cbPathOutMax* affectée à la valeur de l’argument le *pcbPathOut* tampon, ainsi que le caractère de fin de la valeur null.  
  
 Si vous choisissez de ne pas utiliser le modèle en deux phases pour **SQLInstallTranslatorEx**, vous devez définir *cbPathOutMax*, qui définit la taille du stockage pour le chemin d’accès du répertoire cible, à la valeur _MAX_PATH, tel que défini dans Stdlib.h, pour empêcher la troncation.  
  
 Lorsque *fréquents* est ODBC_INSTALL_COMPLETE, **SQLInstallTranslatorEx** n’autorise pas *lpszPathOut* null (ou *cbPathOutMax* est 0). Si *fréquents* est ODBC_INSTALL_COMPLETE, la valeur FALSE est renvoyée lorsque le nombre d’octets à retourner est supérieur ou égal à *cbPathOutMax*, avec le résultat que la troncation se produit.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Renvoi d’une option de traduction par défaut|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|Sélection des traducteurs|[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|  
|Suppression des traducteurs|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
