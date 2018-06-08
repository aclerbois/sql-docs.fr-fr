---
title: Entrées de Registre pour ODBC (composants) | Documents Microsoft
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
- installing ODBC components [ODBC], registry entries
- registry entries for components [ODBC]
- subkeys [ODBC], for components
- registry entries for components [ODBC], about registry entries
ms.assetid: c90aa8a4-6ece-48de-901c-17d23739a9ff
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4cf61535ecda3e95f25dbd9e1b01a1d3ad25d4ab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="registry-entries-for-odbc-components"></a>Entrées de Registre pour ODBC (composants)
> [!NOTE]  
>  ODBC à partir de Windows XP et Windows Server 2003, est inclus dans le système d’exploitation Windows. Vous devez explicitement uniquement installer ODBC dans les versions antérieures de Windows.  
  
 Le programme d’installation DLL conserve les informations dans le Registre sur chaque composant ODBC installé. Sur les ordinateurs exécutant Microsoft Windows NT et Microsoft Windows 95/98, ces informations sont stockées dans les sous-clés de la clé suivante dans le Registre :  
  
 HKEY_LOCAL_MACHINE  
  
 LOGICIEL  
  
 ODBC  
  
 ODBCINST.ini  
  
 Odbcinst.ini étant une sous-clé de l’arborescence HKEY_LOCAL_MACHINE, les informations sur les composants ODBC sont disponibles pour tous les utilisateurs de l’ordinateur.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Sous-clé des composants principaux ODBC](../../../odbc/reference/install/odbc-core-subkey.md)  
  
-   [Sous-clé des pilotes ODBC](../../../odbc/reference/install/odbc-drivers-subkey.md)  
  
-   [Sous-clés de spécification de pilote](../../../odbc/reference/install/driver-specification-subkeys.md)  
  
-   [Sous-clé du pilote par défaut](../../../odbc/reference/install/default-driver-subkey.md)  
  
-   [Sous-clé de convertisseurs ODBC](../../../odbc/reference/install/odbc-translators-subkey.md)  
  
-   [Sous-clés de spécification de convertisseur](../../../odbc/reference/install/translator-specification-subkeys.md)
