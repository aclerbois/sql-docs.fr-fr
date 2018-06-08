---
title: Pilote spécification sous-clés | Documents Microsoft
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
- subkeys [ODBC], driver specification subkeys
- driver specification subkeys [ODBC]
- registry entries for components [ODBC], driver specification subkeys
- drivers subkey [ODBC]
ms.assetid: b4d802ef-b199-4e64-b7a5-6f2b3e5e2c80
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 017e2ff63f93fed46df72e35f1a4f678a147aa04
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="driver-specification-subkeys"></a>Pilote spécification sous-clés
Chaque pilote répertorié dans la sous-clé de pilotes ODBC a une sous-clé qui lui sont propres. Cette sous-clé a le même nom que la valeur correspondante sous la sous-clé de pilotes ODBC. Les valeurs sous cette sous-clé répertorient les chemins d’accès complets du pilote et d’installation du pilote DLL, les valeurs des mots clés du pilote retournés par **SQLDrivers**et le nombre d’utilisations. Les formats des valeurs sont comme indiqué dans le tableau suivant.  
  
|Nom|Type de données|data|  
|----------|---------------|----------|  
|APILevel|REG_SZ|**0** &#124; **1** &#124; **2**|  
|ConnectFunctions|REG_SZ|{**Y**&AMP;#124;**N**} {**Y**&AMP;#124;**N**} {**Y**&AMP;#124;**N**}|  
|CreateDSN|REG_SZ|*description du pilote*|  
|Pilote|REG_SZ|*chemin de DLL de pilote*|  
|DriverODBCVer|REG_SZ|*nn.nn*|  
|FileExtns|REG_SZ|**\*.** *fichier-extension1*[**,\*.** *fichier-extension2*]...|  
|FileUsage|REG_SZ|**0** &#124; **1** &#124; **2**|  
|Programme d'installation|REG_SZ|*le programme d’installation-DLL-path.*|  
|SQLLevel|REG_SZ|**0** &#124; **1** &#124; **2**|  
|UsageCount|REG_DWORD|*nombre*|  
  
 L’utilisation de chaque mot clé est indiquée dans le tableau suivant.  
  
|Mot clé|Utilisation|  
|-------------|-----------|  
|**APILevel**|Un nombre indiquant la ODBC pris en charge par le pilote de niveau de conformité de l’interface :<br /><br /> 0 = Aucun<br /><br /> 1 = le niveau 1 est pris en charge<br /><br /> 2 = niveau 2 est pris en charge<br /><br /> Cela doit être identique à la valeur retournée pour l’option SQL_ODBC_INTERFACE_CONFORMANCE **SQLGetInfo**.|  
|**CreateDSN**|Le nom d’une ou plusieurs sources de données doit être créé lorsque le pilote est installé. Les informations système doivent inclure une section de spécification de source de données pour chaque source de données répertorié avec le **CreateDSN** (mot clé). Ces sections ne doivent pas inclure le **pilote** (mot clé), car cela est spécifié dans la section de la spécification du pilote, mais que vous devez inclure suffisamment d’informations pour le **ConfigDSN** fonction dans la DLL pour créer une spécification de source de données sans afficher les boîtes de dialogue d’installation du pilote. Pour le format d’une section de spécification de source de données, consultez [sous-clés de spécification de Source de données](../../../odbc/reference/install/data-source-specification-subkeys.md).|  
|**ConnectFunctions**|Une chaîne de trois caractères qui indique si le pilote prend en charge **SQLConnect**, **SQLDriverConnect**, et **SQLBrowseConnect**. Si le pilote prend en charge **SQLConnect**, le premier caractère est « Y » ; sinon, elle est « N ». Si le pilote prend en charge **SQLDriverConnect**, le deuxième caractère est « Y » ; sinon, elle est « N ». Si le pilote prend en charge **SQLBrowseConnect**, le troisième caractère est « Y » ; sinon, elle est « N ». Par exemple, si un pilote prend en charge **SQLConnect** et **SQLDriverConnect** mais pas **SQLBrowseConnect**, la chaîne de trois caractères est « YYN ».|  
|**DriverODBCVer**|Une chaîne de caractères avec la version d’ODBC qui prend en charge par le pilote. La version est au format *nn.nn*, où les deux premiers chiffres correspondent à la version principale et les deux chiffres suivants à la version secondaire. Pour obtenir la version d’ODBC décrite dans ce manuel, le pilote doit retourner « 03.00 ».<br /><br /> Cela doit être identique à la valeur retournée pour l’option SQL_DRIVER_ODBC_VER **SQLGetInfo**.|  
|**FileExtns**|Pour les pilotes basée sur un fichier, une liste séparée par des virgules des extensions des fichiers le pilote peut utiliser. Par exemple, un pilote dBASE peut spécifier \*.dbf et un pilote de fichier texte mis en forme peuvent spécifier \*.txt,\*.csv. Pour obtenir un exemple de la façon dont une application peut utiliser ces informations, consultez la **FileUsage** (mot clé).|  
|**FileUsage**|Un nombre qui indique comment un pilote de fichier traite directement les fichiers dans une source de données.<br /><br /> 0 = le pilote n’est pas un pilote de fichier. Par exemple, un pilote ORACLE est un pilote de SGBD.<br /><br /> 1 = fichiers traite basée sur le fichier de pilote dans une source de données en tant que tables. Par exemple, un pilote Xbase traite chaque fichier Xbase sous forme de table.<br /><br /> 2 = un basé sur le fichier de pilote traite les fichiers dans une source de données en tant que catalogue. Par exemple, un pilote Microsoft® Access traite chaque fichier Microsoft Access en tant qu’une base de données complète.<br /><br /> Une application peut utiliser cela pour déterminer la façon dont les utilisateurs sélectionnera les données. Par exemple, les utilisateurs Xbase et Paradox pensent souvent des données stockées dans les fichiers, alors que les utilisateurs ORACLE et Microsoft Access de réflexion généralement des données stockées dans les tables.<br /><br /> Lorsqu’un utilisateur sélectionne **des fichiers de données ouverts** à partir de la **fichier** menu, une application peut afficher le **Windows File Open** boîte de dialogue commune. Utilisez les extensions de fichier spécifiées avec la liste des types de fichiers le **FileExtns** mot clé pour les pilotes qui spécifient un **FileUsage** valeur de 1 et « Y » en tant que le deuxième caractère de la valeur de la **ConnectFunctions** (mot clé). Une fois que l’utilisateur sélectionne un fichier, l’application appelle **SQLDriverConnect** avec la **pilote** (mot clé), puis exécutez une **sélectionnez \* FROM *-nom de la table***  instruction.<br /><br /> Lorsque l’utilisateur sélectionne **importer des données** à partir de la **fichier** menu, une application peut afficher une liste de descriptions pour les pilotes qui spécifient un **FileUsage** valeur 0 ou 2 et « Y » en tant que le deuxième caractère de la valeur de la **ConnectFunctions** (mot clé). Une fois que l’utilisateur sélectionne un pilote, l’application appelle **SQLDriverConnect** avec la **pilote** (mot clé) et l’affichage puis personnalisé **sélectionner une Table** boîte de dialogue.|  
|**SQLLevel**|Nombre indiquant la grammaire SQL-92 pris en charge par le pilote :<br /><br /> 0 = entrée SQL-92<br /><br /> 1 = transitoire FIPS127-2<br /><br /> 2 = SQL-92 intermédiaire<br /><br /> 3 = complet de SQL-92.<br /><br /> Cela doit être identique à la valeur retournée pour l’option SQL_SQL_CONFORMANCE **SQLGetInfo**.|  
  
 Pour plus d’informations sur les compteurs d’utilisation, consultez [en prenant en compte l’utilisation](../../../odbc/reference/install/usage-counting.md) plus haut dans cette section.  
  
 Applications ne doivent pas définir le nombre d’utilisations. ODBC conserve ce nombre.  
  
 Par exemple, supposons qu’un pilote pour les fichiers de texte mis en forme a un DLL nommée Text.dll du pilote, un programme d’installation de pilote séparé DLL nommé Txtsetup.dll et a été installé à trois reprises. Si le pilote prend en charge le niveau de conformité de niveau 1 API prend en charge le niveau de conformité de grammaire SQL minimale, traite les fichiers en tant que tables et pouvez utiliser des fichiers portant les extensions .txt et .csv, il se peut que les valeurs sous la sous-clé de texte peuvent être comme suit :  
  
```  
APILevel : REG_SZ : 1  
ConnectFunctions : REG_SZ : YYN  
Driver : REG_SZ : C:\WINDOWS\SYSTEM32\TEXT.DLL  
DriverODBCVer : REG_SZ : 03.00.00  
FileExtns : REG_SZ : *.txt,*.csv  
FileUsage : REG_SZ : 1  
Setup : REG_SZ : C:\WINDOWS\SYSTEM32\TXTSETUP.DLL  
SQLLevel : REG_SZ : 0  
UsageCount : REG_DWORD : 0x3  
```
