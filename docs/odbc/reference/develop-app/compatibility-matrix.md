---
title: Matrice de compatibilité | Documents Microsoft
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
- driver compatibility issues [ODBC]
- ODBC drivers [ODBC], backward compatibility
- backward compatibility [ODBC], application and driver compatibility
- compatibility [ODBC], application and driver compatibility
- application compatibility issues [ODBC]
- application upgrades [ODBC], compatibility matrix
- upgrading applications [ODBC], compatibility matrix
ms.assetid: 0690b463-15a1-48fa-9d0b-9cc9e5bf7fc6
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 09f09d5a8b6e15c677969b2b865e3908200108e4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="compatibility-matrix"></a>Matrice de compatibilité
Le tableau suivant décrit la compatibilité des types d’applications et des pilotes définies précédemment dans cette section.  
  
|Type d'application<br /><br /> et la version|ODBC 32 bits<br /><br /> 2.*x* pilote|ODBC 3. *x*<br /><br /> Pilote|Pilotes ODBC 3.8|ISO et ouvrez groupe compatibles avec le pilote|  
|--------------------------------------|-----------------------------------|---------------------------|---------------------|-----------------------------------------|  
|application 16 bits, n’importe quelle version|Compatible|Compatible|Compatible|Compatible|  
|2 pure. *x* application|Compatible|Compatible|Compatible|Non compatible avec [3]|  
|2 pure. *x* recompiler l’application|Compatible|Compatibilité [1]|Compatibilité [1]|Non compatible avec [3]|  
|2 pure. *x* application Unicode|Compatible|Compatibilité [1]|Compatibilité [1]|Non compatible avec [3]|  
|Application pure Open Group et compatible ISO|N’est pas compatible|Compatibilité [2]|Compatibilité [2]|Compatibilité [2]|  
|Application 3.0 pure|N’est pas compatible|Compatible|Compatible|Non compatible avec [4]|  
|Application 3.5 pure|N’est pas compatible|Compatible|Compatible|Non compatible avec [4]|  
|Application pure de 3.8 (ou version ultérieure)|Non compatible avec [5]|Non compatible avec [5]|Compatible|Non compatible avec [4]|  
|Application remplacée|Compatible|Compatible|Compatible|Non compatible avec [3]|  
  
 [1], l’application doit recompilez à l’aide des en-têtes ODBC 3.5 (ou version ultérieure) avec l’option UNICODE (s’il est une application Unicode) et doit ODBCVER 0x0250.  
  
 [2] de l’application doit compiler à l’aide d’en-têtes ODBC 3.5 (ou version ultérieure) et lien avec le Gestionnaire de pilotes ODBC. Il doit également définir l’indicateur d’en-tête ODBC_STD.  
  
 [3] de cette configuration peut potentiellement pas, car il existe des fonctionnalités dans ODBC 2. *x* qui ne sont pas dans les normes, telles que les signets.  
  
 [4], cette configuration peut potentiellement ne fonctionnent pas, car il existe des fonctionnalités dans ODBC 3 *.x* qui ne sont pas dans les normes, telles que les signets.  
  
 [5], cette configuration peut potentiellement être défaillants, car il existe des fonctionnalités qui ne sont pas dans ODBC 2.x ou 3.x pilotes, tels que spécifiques au pilote dans ODBC 3.8 [des Types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
## <a name="driver-manager-compatibility"></a>Compatibilité du Gestionnaire de pilotes  
 Une application ODBC 3.0 qui doit fonctionner avec toutes les versions du Gestionnaire de pilotes doit procéder au démarrage :  
  
-   Allouer un handle d’environnement.  
  
-   Affectez l’attribut d’environnement SQL_ATTR_ODBC_VERSION SQL_OV_ODBC3_80. Si le Gestionnaire de pilotes retourne SQL_ERROR, le Gestionnaire de pilotes est antérieure à 3.8. Rétablir SQL_ATTR_ODBC_VERSION SQL_OV_ODBC3 ou SQL_OV_ODBC2, selon le cas correspondre au Gestionnaire de pilote.  
  
-   Allouer un handle de connexion.  
  
-   Établir une connexion.  
  
-   Appelez SQLGetInfo pour SQL_DRIVER_ODBC_VER déterminer la version du pilote. Si le pilote est un pilote ODBC 3.8, vous pouvez utiliser des types spécifiques au pilote C. Sinon, n’utilisez pas les types de données C spécifiques au pilote.  
  
 Notez qu’une application de 3.x ODBC recompilée peut utiliser les fonctions ODBC 3.8 autres que les types de C spécifiques au pilote sans spécifier SQL_OV_ODBC3_80 pour SQL_ATTR_ODBC_VERSION. Cela est similaire à une application ODBC 2.x recompilée à l’aide des fonctionnalités ODBC 3.x.  
  
## <a name="using-sqlcancelhandle-in-an-application-compatible-with-all-driver-managers"></a>À l’aide de SQLCancelHandle dans une Application Compatible avec tous les gestionnaires de pilotes  
 Étant donné que [SQLCancelHandle fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md) n’est pas pris en charge dans les gestionnaires de pilotes qui ont été publiés avant Windows 7, une application ne peut pas être chargée dans les versions antérieures de Windows si elle appelle **SQLCancelHandle** directement. Travailler avec toutes les versions de gestionnaires de pilotes et utiliser **SQLCancelHandle** sur les nouvelles versions de Windows, une application doit appeler **SQLCancelHandle** indirectement à l’aide **LoadLibrary** et **GetProcAddress.**  
  
## <a name="see-also"></a>Voir aussi  
 [Nouveautés d’ODBC 3.8](../../../odbc/reference/what-s-new-in-odbc-3-8.md)
