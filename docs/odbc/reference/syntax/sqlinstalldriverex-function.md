---
title: Fonction de SQLInstallDriverEx | Documents Microsoft
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
- SQLInstallDriverEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverEx
helpviewer_keywords:
- SQLInstallDriverEx function [ODBC]
ms.assetid: 1dd74544-f4e9-46e1-9b5f-c11d84fdab4c
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4c4ba55c94f36052d525c79c3c472e7e5c28d6a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlinstalldriverex-function"></a>SQLInstallDriverEx (fonction)
**Mise en conformité**  
 Version introduite : ODBC 3.0  
  
 **Résumé**  
 **SQLInstallDriverEx** ajoute des informations sur le pilote à l’entrée de fichier Odbcinst.ini dans les informations système et incrémente du pilote *UsageCount* par 1. Toutefois, si une version du pilote existe déjà mais le *UsageCount* valeur pour le pilote n’existe pas, la nouvelle *UsageCount* a la valeur 2.  
  
 Cette fonction ne copie pas réellement des fichiers. Il est responsable de programme appelant pour copier les fichiers du pilote dans le répertoire cible correctement.  
  
 La fonctionnalité de **SQLInstallDriverEx** sont également accessibles avec [ODBCCONF. EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLInstallDriverEx(  
     LPCSTR    lpszDriver,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszDriver*  
 [Entrée] Description du pilote (généralement le nom du SGBD associé) présentée à l’utilisateur au lieu du nom de pilote physique. Le *lpszDriver* argument doit contenir une liste doublement se terminant par null des paires mot clé-valeur décrivant le pilote. Pour plus d’informations sur les paires mot clé-valeur, consultez [pilote spécification sous-clés](../../../odbc/reference/install/driver-specification-subkeys.md). Pour plus d’informations sur la chaîne doublement se terminant par null, consultez [ConfigDSN fonction](../../../odbc/reference/syntax/configdsn-function.md).  
  
 *lpszPathIn*  
 [Entrée] Chemin d’accès complet du répertoire cible de l’installation, ou un pointeur null. Si *lpszPathIn* est un pointeur null, les pilotes sont installés dans le répertoire système.  
  
 *lpszPathOut*  
 [Sortie] Chemin d’accès du répertoire cible où le pilote doit être installé. Si le pilote n’a pas été précédemment installé, *lpszPathOut* doit être le même que *lpszPathIn*. Si le pilote a été précédemment installé, *lpszPathOut* est le chemin d’accès de l’installation précédente.  
  
 *cbPathOutMax*  
 [Entrée] Longueur de *lpszPathOut*.  
  
 *pcbPathOut*  
 [Sortie] Nombre total d’octets (sans le caractère de fin de la valeur null) disponibles à renvoyer dans *lpszPathOut*. Si le nombre d’octets à retourner est supérieur ou égal à *cbPathOutMax*, le chemin de sortie dans *lpszPathOut* est tronqué à *cbPathOutMax* moins le caractère de fin de la valeur null. Le *pcbPathOut* argument peut être un pointeur null.  
  
 *fréquents*  
 [Entrée] Type de requête. Le *fréquents* argument doit contenir l’une des valeurs suivantes :  
  
 ODBC_INSTALL_INQUIRY : Obtenir des informations sur l’installation d’un pilote.  
  
 ODBC_INSTALL_COMPLETE : Terminer la requête de l’installation.  
  
 *lpdwUsageCount*  
 [Sortie] Le décompte d’utilisation du pilote après que cette fonction a été appelée.  
  
 Applications ne doivent pas définir le nombre d’utilisations. ODBC conserve ce nombre.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLInstallDriverEx** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|Le *lpszPathOut* argument n’était pas suffisamment grande pour contenir le chemin de sortie. La mémoire tampon contient le chemin d’accès tronquée.<br /><br /> Le *cbPathOutMax* argument était 0, et *fréquents* a été ODBC_INSTALL_COMPLETE.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|Le *fréquents* argument n’est pas une des opérations suivantes :<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé-valeur non valide|Le *lpszDriver* argument contenait une erreur de syntaxe.|  
|ODBC_ERROR_INVALID_PATH|Le chemin d’installation non valide|Le *lpszPathIn* argument contenait un chemin d’accès non valide.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Impossible de charger la bibliothèque le programme d’installation de pilote ou de convertisseur|La bibliothèque d’installation du pilote n’a pas pu être chargée.|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Séquence de paramètres non valide|Le *lpszDriver* argument ne contenait pas d’une liste de paires mot clé-valeur.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou décrémenter le décompte d’utilisation de composant|Le programme d’installation a échoué incrémenter le décompte d’utilisation du pilote.|  
  
## <a name="comments"></a>Commentaires  
 Le *lpszDriver* argument est une liste d’attributs sous la forme de paires mot clé-valeur. Chaque paire se termine par un octet null, et la liste entière se termine par un octet null. (Autrement dit, deux octets null marquent la fin de la liste.) Le format de cette liste est la suivante :  
  
 *pilote-desc* **\\**0Driver**=***pilote-DLL-nom de fichier***\\**0 [le programme d’installation **= ***le programme d’installation-DLL-nom de fichier***\\**0]  
  
 [*pilote-attr-mot_clé1***=*** value1 ***\\**0] [* pilote-attr-mot Clé2***=*** value2 ***\\**0]... **\\**0  
  
 où \0 est un octet null et *pilote-attr-keywordn* est n’importe quel mot clé d’attribut pilote. Les mots clés doivent apparaître dans l’ordre spécifié. Par exemple, supposons qu’un pilote pour les fichiers de texte mis en forme possède de pilote séparé et le programme d’installation DLL et pouvez utiliser des fichiers portant les extensions .txt et .csv. Le *lpszDriver* argument pour ce pilote peut être comme suit :  
  
```  
Text\0Driver=TEXT.DLL\0Setup=TXTSETUP.DLL\0FileUsage=1\0  
FileExtns=*.txt,*.csv\0\0  
```  
  
 Supposez qu’un pilote pour SQL Server ne dispose pas d’une DLL d’installation distinct et n’a pas de mots clés n’importe quel attribut de pilote. Le *lpszDriver* argument pour ce pilote peut être comme suit :  
  
```  
SQL Server\0Driver=SQLSRVR.DLL\0\0  
```  
  
 Après avoir **SQLInstallDriverEx** récupère des informations sur le pilote à partir de la *lpszDriver* argument, il ajoute la description du pilote à la section [ODBC Drivers] de l’écriture du fichier Odbcinst.ini dans les informations système. Il crée une section intitulée avec la description du pilote et ajoute les chemins d’accès complets de la DLL du pilote et de la DLL d’installation. Enfin, elle retourne le chemin d’accès du répertoire cible de l’installation, mais ne copie pas les fichiers de pilote à celui-ci. Le programme appelant doit copier réellement les fichiers de pilote dans le répertoire cible.  
  
 **SQLInstallDriverEx** incrémente le décompte d’utilisation de composant pour le pilote installé par 1. Si une version du pilote existe déjà, mais le nombre d’utilisations de composant pour le pilote n’existe pas, la nouvelle valeur de nombre de l’utilisation de composant est définie à 2.  
  
 Le programme d’installation d’application est chargé pour la copie de physiquement le fichier de pilote et en conservant le décompte d’utilisation de fichier. Si le fichier de pilote n’a pas été précédemment installé, le programme d’installation d’application doit copier le fichier dans le *lpszPathIn* chemin d’accès et de créer le décompte d’utilisation de fichier. Si le fichier a déjà été installé, le programme d’installation simplement incrémente le décompte d’utilisation de fichier et retourne le chemin d’accès de l’installation précédente dans le *lpszPathOut* argument.  
  
> [!NOTE]  
>  Pour plus d’informations sur les compteurs d’utilisation de composant et le nombre de fichiers d’utilisation, consultez [en prenant en compte l’utilisation](../../../odbc/reference/install/usage-counting.md).  
  
 Si une ancienne version du fichier du pilote a été précédemment installée par l’application, le pilote doit être désinstallé et réinstallé, afin que le nombre d’utilisations du composant pilote n’est valide. **SQLConfigDriver** (avec un *fréquents* de ODBC_REMOVE_DRIVER) doit d’abord être appelée, puis **SQLRemoveDriver** doit être appelé pour décrémenter le décompte d’utilisation de composant. **SQLInstallDriverEx** doit ensuite être appelé pour réinstaller le pilote, incrémentant le nombre d’utilisations de composant. Le programme d’installation d’application doit remplacer l’ancien fichier avec le nouveau fichier. Le décompte d’utilisation de fichier reste la même, et toute autre application ayant utilisé l’ancien fichier de version utilise désormais une version plus récente.  
  
> [!NOTE]  
>  Si le pilote a été installé précédemment et **SQLInstallDriverEx** est appelée pour installer le pilote dans un répertoire différent, la fonction renvoie la valeur TRUE, mais *lpszPathOut* inclut le répertoire où le pilote a déjà été installé. Il n’inclut pas le répertoire entré dans le *lpszDriver* argument.  
  
 La longueur du chemin d’accès dans *lpszPathOut* dans **SQLInstallDriverEx** permet un processus d’installation en deux phases, pour une application peut déterminer quel *cbPathOutMax* doit être en appelant **SQLInstallDriverEx** avec un *fréquents* du mode ODBC_INSTALL_INQUIRY. Cela retourne le nombre total d’octets disponible dans le *pcbPathOut* mémoire tampon. **SQLInstallDriverEx** peut ensuite être appelée avec un *fréquents* de ODBC_INSTALL_COMPLETE et *cbPathOutMax* affectée à la valeur de l’argument le *pcbPathOut*mémoire tampon, ainsi que le caractère de fin de la valeur null.  
  
 Si vous choisissez de ne pas utiliser le modèle en deux phases pour **SQLInstallDriverEx**, vous devez définir *cbPathOutMax*, qui définit la taille du stockage pour le chemin d’accès du répertoire cible, à la valeur _MAX_PATH, tel que défini dans Stdlib.h, pour empêcher la troncation.  
  
 Lorsque *fréquents* est ODBC_INSTALL_COMPLETE, **SQLInstallDriverEx** n’autorise pas *lpszPathOut* null (ou *cbPathOutMax* est 0). Si *fréquents* est ODBC_INSTALL_COMPLETE, la valeur FALSE est renvoyée lorsque le nombre d’octets à retourner est supérieur ou égal à *cbPathOutMax*, avec le résultat que la troncation se produit.  
  
 Après avoir **SQLInstallDriverEx** a été appelée et que le programme d’installation d’application a copié le fichier de pilote (si nécessaire), le programme d’installation du pilote DLL doit appeler **SQLConfigDriver** pour définir la configuration pour le pilote.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Installation du Gestionnaire de pilotes|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
