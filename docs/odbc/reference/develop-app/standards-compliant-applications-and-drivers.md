---
title: Applications conformes aux normes et des pilotes | Documents Microsoft
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
- standards-compliant applications and drivers [ODBC]
- ODBC drivers [ODBC], standards-compliant
- application features are standards-compliant [ODBC]
ms.assetid: a1145c4c-3094-4f3f-8cc2-e6bb1a930ab1
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fc733f6a9bf4d7edeae5c06f577b206f8232281b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="standards-compliant-applications-and-drivers"></a>Applications conformes aux normes et les pilotes
Une application conforme aux normes ou le pilote est celui qui est conforme à la spécification d’IAO groupe Open « données Management : SQL au niveau de l’appel Interface (CLI) » et la norme ISO/IEC 9075-3:1995 (E) Interface de niveau d’appel (SQL/CLI).  
  
 ODBC 3 *.x* garantit les fonctionnalités suivantes :  
  
-   Une application écrite pour les spécifications Open Group et ISO CLI fonctionnera avec un ODBC 3 *.x* pilote ou un pilote conforme aux normes lorsqu’il est compilé avec ODBC 3 *.x* en-tête des fichiers et liés par ODBC 3 *.x* bibliothèques, et quand il accède au pilote via l’ODBC 3 *.x* du Gestionnaire de pilotes.  
  
-   Un pilote écrit dans les spécifications Open Group et ISO CLI fonctionne avec un ODBC 3 *.x* application ou une application conforme aux normes lorsqu’il est compilé avec ODBC 3 *.x* en-tête des fichiers et liés par ODBC 3 *.x* bibliothèques, et lorsque l’application accède au pilote via l’ODBC 3 *.x* du Gestionnaire de pilotes.  
  
 Applications conformes aux normes et les pilotes sont compilés avec l’indicateur de compilation ODBC_STD.  
  
 Applications conformes aux normes exposer le comportement suivant :  
  
-   Si une application conforme aux normes appelle **SQLAllocEnv** (qui peut se produire car **SQLAllocEnv** est une fonction valide dans l’Open Group et ISO CLI), l’appel est mappé à **SQLAllocHandleStd** au moment de la compilation. Par conséquent, au moment de l’exécution, l’application appelle **SQLAllocHandleStd**. Au cours du traitement de cet appel, le Gestionnaire de pilotes définit l’attribut d’environnement SQL_ATTR_ODBC_VERSION à SQL_OV_ODBC3. Un appel à **SQLAllocHandleStd** équivaut à un appel à **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_ENV et un appel à **SQLSetEnvAttr** à la valeur SQL_ATTR_ODBC_VERSION SQL_OV_ODBC3.  
  
-   Si une application conforme aux normes appelle **SQLBindParam** (qui peut se produire car **SQLBindParam** est une fonction valide dans l’Open Group et ISO CLI), la version ODBC 3 *.x* du Gestionnaire de pilotes est mappé à l’appel à l’appel équivalent dans **SQLBindParameter**. (Consultez [SQLBindParam mappage](../../../odbc/reference/appendixes/sqlbindparam-mapping.md) dans l’annexe g : pilote recommandations pour la compatibilité descendante.)  
  
-   Pour les aligner avec la CLI ISO, les 3 ODBC *.x* fichiers d’en-tête contiennent des alias pour les types d’informations utilisés dans les appels à **SQLGetInfo**. Une application conforme aux normes peut utiliser ces alias au lieu de la version 3 ODBC *.x* types d’informations. Pour plus d’informations, consultez la rubrique suivante, [fichiers d’en-tête](../../../odbc/reference/develop-app/header-files.md).  
  
-   Une application conforme aux normes doit vérifier qu’il prend en charge de toutes les fonctionnalités sont prises en charge dans le pilote, avec qu'il fonctionnera. Définition de l’attribut d’instruction SQL_ATTR_CURSOR_SCROLLABLE SQL_SCROLLABLE et en affectant l’attribut d’instruction SQL_ATTR_CURSOR_SENSITIVITY SQL_INSENSITIVE ou SQL_SENSITIVE sont les fonctionnalités qui sont disponibles en tant que fonctionnalités facultatives dans les normes, mais ne sont pas incluses dans ODBC 3 *.x* Core niveau et par conséquent ne peut pas être pris en charge par tous les ODBC 3 *.x* pilotes. Si une application conforme aux normes utilise ces fonctionnalités, il doit vérifier que le pilote qui fonctionnent avec les prend en charge.
