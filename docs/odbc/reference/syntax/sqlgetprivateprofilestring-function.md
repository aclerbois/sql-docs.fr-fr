---
title: Fonction de SQLGetPrivateProfileString | Documents Microsoft
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
- SQLGetPrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetPrivateProfileString
helpviewer_keywords:
- SQLGetPrivateProfileString function [ODBC]
ms.assetid: b72ca065-4d67-48df-baac-e18379a8320a
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4538c94012ab6ccc7c532436cedef1abae3fc936
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetprivateprofilestring-function"></a>SQLGetPrivateProfileString (fonction)
**Mise en conformité**  
 Version introduite : ODBC version 2.0  
  
 **Résumé**  
 **SQLGetPrivateProfileString** Obtient une liste de noms de valeurs ou des données correspond à la valeur des informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int SQLGetPrivateProfileString(  
     LPCSTR   lpszSection,  
     LPCSTR   lpszEntry,  
     LPCSTR   lpszDefault,  
     LPCSTR   RetBuffer,  
     INT      cbRetBuffer,  
     LPCSTR   lpszFilename);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszSection*  
 [Entrée] Pointe vers une chaîne se terminant par null qui spécifie la section qui contient le nom de clé. Si cet argument est NULL, la fonction de copie tous les noms de section dans le fichier dans la mémoire tampon fournie.  
  
 *lpszEntry*  
 [Entrée] Pointe vers la chaîne se terminant par null qui contient le nom de clé dont la chaîne associée doit être récupéré. Si cet argument est NULL, tous les noms de clé de dans la section spécifiée par le *lpszSection* argument sont copiés dans la mémoire tampon spécifiée par le *RetBuffer* argument.  
  
 *lpszDefault*  
 [Entrée] Pointe vers une chaîne se terminant par null qui spécifie la valeur par défaut pour la clé donnée si la clé ne peut pas être trouvée dans le fichier d’initialisation. Cet argument ne peut pas être NULL.  
  
 *RetBuffer*  
 [Sortie] Pointe vers la mémoire tampon qui reçoit la chaîne récupérée.  
  
 *cbRetBuffer*  
 [Entrée] Spécifie la taille, en caractères, de la mémoire tampon vers laquelle pointé le *RetBuffer* argument.  
  
 *lpszFilename*  
 [Entrée] Pointe vers une chaîne se terminant par null qui désigne le fichier d’initialisation. Si cet argument ne contient pas un chemin d’accès complet au fichier, le répertoire par défaut est recherché.  
  
## <a name="returns"></a>Valeur renvoyée  
 **SQLGetPrivateProfileString** retourne une valeur entière qui indique le nombre de caractères lus.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsqu’un appel à **SQLGetPrivateProfileString** échoue, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLGetPrivateProfileString** est fournie comme un moyen simple pour les pilotes de ports et les DLL d’installation du pilote à partir de Microsoft® Windows® pour Microsoft Windows et Windows 2000. Les appels à **GetPrivateProfileString** qui récupèrent une chaîne de profil à partir du fichier Odbc.ini doit être remplacée par des appels à **SQLGetPrivateProfileString**. **SQLGetPrivateProfileString** appelle des fonctions dans l’API Win32® pour récupérer les noms demandés de valeurs ou les données correspondant à une valeur de la sous-clé Odbc.ini des informations système.  
  
 Le mode de configuration (comme défini par **SQLSetConfigMode**) indique l’entrée du fichier Odbc.ini répertoriant les valeurs de la source de données dans les informations système. Si la source de données est une source de données utilisateur (le mode de configuration est USERDSN_ONLY), la fonction lit à partir de l’entrée de fichier Odbc.ini dans HKEY_CURRENT_USER. Si la source de données est une source de données système (SYSTEMDSN_ONLY), la fonction lit à partir de l’entrée de fichier Odbc.ini dans HKEY_LOCAL_MACHINE. Si le mode de configuration est BOTHDSN, HKEY_CURRENT_USER est tentée, et en cas d’échec, HKEY_LOCAL_MACHINE est utilisé.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Écriture d’une valeur pour les informations système|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
