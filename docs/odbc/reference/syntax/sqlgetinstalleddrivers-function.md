---
title: Fonction de SQLGetInstalledDrivers | Documents Microsoft
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
- SQLGetInstalledDrivers
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetInstalledDrivers
helpviewer_keywords:
- SQLGetInstalledDrivers function [ODBC]
ms.assetid: a1983a2e-0edf-422e-bd1b-ec5db40a34bc
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6b45bd7c06b5c8e87c13fd8d9e956072ffebe858
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetinstalleddrivers-function"></a>SQLGetInstalledDrivers (fonction)
**Mise en conformité**  
 Version introduite : ODBC 1.0  
  
 **Résumé**  
 **SQLGetInstalledDrivers** lit la section [ODBC Drivers] les informations système et retourne une liste des descriptions des pilotes installés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLGetInstalledDrivers(  
     LPSTR   lpszBuf,  
     WORD    cbBufMax,  
     WORD *  pcbBufOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszBuf*  
 [Sortie] Liste des descriptions des pilotes installés. Pour plus d’informations sur la structure de la liste, consultez « Commentaires ».  
  
 *cbBufMax*  
 [Entrée] Longueur de *lpszBuf*.  
  
 *pcbBufOut*  
 [Sortie] Nombre total d’octets (à l’exception de l’octet de fin de null) retournée dans *lpszBuf*. Si le nombre d’octets à retourner est supérieur ou égal à *cbBufMax*, la liste des descriptions de pilote dans *lpszBuf* est tronqué à *cbBufMax* moins le caractère de fin de la valeur null. Le *pcbBufOut* argument peut être un pointeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetInstalledDrivers** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|Le *lpszBuf* argument était NULL ou non valide, ou le *cbBufMax* argument était inférieur ou égal à 0.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Composant introuvable dans le Registre|Le programme d’installation n’a pas trouvé la section [ODBC Drivers] dans le Registre.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 Description de chaque pilote se termine par un octet null, et la liste entière se termine par un octet null. (Autrement dit, deux octets null marquent la fin de la liste.) Si la mémoire tampon allouée n’est pas suffisamment grande pour contenir l’intégralité de la liste, la liste est tronquée sans erreur. Une erreur est retournée si un pointeur null est passé comme *lpszBuf*.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Renvoi de descriptions de pilote et attributs|[SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|
