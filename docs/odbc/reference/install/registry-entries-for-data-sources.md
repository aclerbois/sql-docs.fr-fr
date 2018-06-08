---
title: Entrées de Registre pour les Sources de données | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- subkeys [ODBC]
- data sources [ODBC], registry entries
- registry entries for data sources [ODBC], about registry entries
- data sources [ODBC], configuring
- registry entries for data sources [ODBC]
ms.assetid: 78aaa3d3-d081-4550-80e3-720c910d5996
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d052b4d64e858837a51a907fc77548bac231c35d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="registry-entries-for-data-sources"></a>Entrées de Registre pour les Sources de données
> [!NOTE]  
>  ODBC à partir de Windows XP et Windows Server 2003, est inclus dans le système d’exploitation Windows. Vous devez explicitement uniquement installer ODBC dans les versions antérieures de Windows.  
  
 Le programme d’installation DLL conserve les informations dans le Registre sur chaque source de données. Dans Microsoft Windows NT et Windows 2000 et Microsoft Windows 95/98, ces informations sont stockées dans les sous-clés sous un des deux clés suivantes dans le Registre :  
  
 HKEY_LOCAL_MACHINE  
  
 LOGICIEL  
  
 ODBC  
  
 ODBC.ini  
  
 HKEY_CURRENT_USER  
  
 LOGICIEL  
  
 ODBC  
  
 ODBC.ini  
  
 Clé utilisée dépend de la source de données un *source de données système,* qui est disponible pour tous les utilisateurs, ou un *source de données utilisateur,* qui est disponible uniquement pour l’utilisateur actuel. Sources de données système sont stockés sur l’arborescence HKEY_LOCAL_MACHINE et sources de données utilisateur sont stockées sur l’arborescence HKEY_CURRENT_USER. Dans tous les autres égards, les sources de données système et des sources de données utilisateur sont identiques.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Sous-clé des sources de données ODBC](../../../odbc/reference/install/odbc-data-sources-subkey.md)  
  
-   [Sous-clés de spécification de source de données](../../../odbc/reference/install/data-source-specification-subkeys.md)  
  
-   [Sous-clé par défaut](../../../odbc/reference/install/default-subkey.md)  
  
-   [Sous-clé ODBC](../../../odbc/reference/install/odbc-subkey.md)
