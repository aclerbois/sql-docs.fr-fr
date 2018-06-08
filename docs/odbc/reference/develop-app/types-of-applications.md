---
title: Types d’Applications | Documents Microsoft
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
- upgrading applications [ODBC], application types
- backward compatibility [ODBC], application and driver compatibility
- compatibility [ODBC], application and driver compatibility
- application upgrades [ODBC], application types
- application compatibility issues [ODBC]
ms.assetid: d346a64e-a32c-4153-a40f-5b53c2f57ef2
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1c2283c81c1ad5389a8662c14ce79942a3cc4b45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="types-of-applications"></a>Types d’Applications
Les applications ODBC peuvent être classées comme suit :  
  
-   **Pure ODBC 2.**  
     ***x* Application** les applications 32 bits qui :  
  
    -   Appelle uniquement ODBC 2. *x* fonctions (y compris la fonction ODBC 1.0 **SQLSetParam**). Ceux-ci incluent ODBC 1. *x* les applications qui ont été transférées à 32 bits.  
  
    -   Attend ODBC 2. *x* comportement pour les fonctionnalités qui ont connu des changements de comportement. (Consultez [modifications comportementales](../../../odbc/reference/develop-app/behavioral-changes.md) pour plus d’informations.)  
  
    -   N’a pas été recompilée avec des en-têtes de ODBC 3.5.  
  
-   **Pure ODBC 2.**  
     ***x* recompiler l’Application** une pure ODBC 2. *x* application qui a été compilée à l’aide des fichiers d’en-tête ODBC 3.5, en définissant ODBCVER = 0x0250.  
  
-   **Pure ODBC 2.**  
     ***x* Application Unicode** une pure ODBC 2. *x* recompiler l’application qui est compatible avec Unicode et utilise le type de données SQL_WCHAR.  
  
-   **Pure Open Group et ISO**–**Application ODBC compatible** les applications 32 bits qui :  
  
    -   Appelle les fonctions définies dans les normes Open Group ou ISO CLI. (Ces fonctions peuvent inclure des 3.0 fonctions déconseillées.)  
  
    -   N’utilise pas les types de données Unicode.  
  
    -   Attend que le comportement ODBC 3.0 pour les fonctionnalités qui ont connu des changements de comportement.  
  
-   **Application de pure ODBC 3.0** les applications 32 bits qui :  
  
    -   Est compilé avec des 3.0 en-têtes.  
  
    -   Appelle une fonction ODBC 3.0, éventuellement, y compris ceux qui sont déconseillées.  
  
    -   Attend que le comportement ODBC 3.0 pour les fonctionnalités qui ont connu des changements de comportement.  
  
-   **Application de pure ODBC 3.5** de 32 ou 64 bits application qui :  
  
    -   Peut utiliser des types de données Unicode.  
  
    -   Appelle une fonction ODBC 3.5, éventuellement, y compris ceux qui sont déconseillées.  
  
    -   Attend que le comportement ODBC 3.5 pour les fonctionnalités qui ont connu des changements de comportement.  
  
-   **ODBC 3.8 (ou version ultérieure) Application pure** application 32 bits ou 64 bits qui :  
  
    -   Peut utiliser des types de données Unicode.  
  
    -   Appelle une fonction ODBC 3.8, éventuellement, y compris ceux qui sont déconseillées.  
  
    -   Attend que le comportement ODBC 3.8 pour les fonctionnalités qui ont connu des changements de comportement.  
  
-   **Remplacer l’Application** de 32 ou 64 bits application qui :  
  
    -   Implémente nouveau comportement pour la fonctionnalité en double.  
  
    -   Utilise des nouvelles fonctionnalités dans une version ultérieure d’ODBC uniquement dans le code conditionnel.  
  
    -   Dispose d’un code conditionnel pour gérer les changements de comportement limité ou s’est inscrit pour être une version antérieure de l’application ODBC.
