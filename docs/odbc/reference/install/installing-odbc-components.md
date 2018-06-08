---
title: Installation des composants ODBC | Documents Microsoft
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
- installing ODBC components [ODBC]
- installing ODBC components [ODBC], about installing
- ODBC [ODBC], component installation
ms.assetid: b7e48e9c-8912-4003-b4ef-30aa44de06a7
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2cf4bec202249de89178e5618cdaa5f2770778fc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-odbc-components"></a>Installation des composants ODBC
> [!NOTE]  
>  ODBC à partir de Windows XP et Windows Server 2003, est inclus dans le système d’exploitation Windows. Vous devez explicitement uniquement installer ODBC dans les versions antérieures de Windows.  
  
 Cette section décrit comment les composants ODBC sont installées et supprimées. Les développeurs de pilote toujours installer un composant ODBC (leur pilote), il est nécessaire de lire cette section. Les développeurs d’applications doivent lire cette section uniquement si elles seront distribué ODBC (composants) avec leurs applications. ODBC (composants) incluent le Gestionnaire de pilotes, pilotes, traducteurs, le programme d’installation DLL, la bibliothèque de curseurs et les fichiers associés. Dans le cadre de cette section, les applications ODBC ne sont pas considérées comme des composants ODBC.  
  
> [!NOTE]  
>  Cette section est spécifique aux plateformes Microsoft Windows. Comment les composants ODBC sont installés sur d’autres plateformes sont spécifique à la plateforme.  
  
 ODBC (composants) sont installés et supprimés selon un composant par composant, pas une base de fichier par fichier. Par exemple, si un convertisseur compose du traducteur lui-même et un nombre de fichiers de données, ces fichiers sont installés et supprimés en tant que groupe ; non, ils doivent être installés et supprimés selon le fichier par fichier. La raison à cela consiste à vous assurer que les composants complets uniquement existent sur le système.  
  
 Pour des raisons de l’installation et la suppression de composants, les éléments suivants sont définis comme composants ODBC :  
  
-   **Composants principaux.** Le Gestionnaire de pilotes, bibliothèque de curseurs, programme d’installation DLL et n’importe quel autre liées fichiers constituent les principaux composants et doivent être installés et supprimés en tant que groupe.  
  
-   **Pilotes.** Chaque pilote est un composant distinct.  
  
-   **Traducteurs.** Chaque traduction est un composant distinct.  
  
 Avec la prise en charge d’Unicode dans ODBC 3.5 et versions ultérieures, certains doit en considération pour l’utilisation des composants OLE DB avec ODBC. La version 1.1 du fournisseur OLE DB pour ODBC a été écrite dans les spécifications Unicode spécifiques dans ODBC 3.0. Ces spécifications sont modifiées dans ODBC 3.5, il est nécessaire d’avoir la version 1.5 ou ultérieure du fournisseur lors de l’utilisation de ODBC 3.5 et versions ultérieures. Cette section contient les rubriques suivantes.  
  
-   [Composants d’installation](../../../odbc/reference/install/installation-components.md)  
  
-   [Nombre d’utilisations](../../../odbc/reference/install/usage-counting.md)
