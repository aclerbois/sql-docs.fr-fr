---
title: Connexion à l’aide de Sources de données fichier | Documents Microsoft
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
- connecting to driver [ODBC], file data sources
- SQLDriverConnect function [ODBC], connecting using file data sources
- connecting to data source [ODBC], SQLDriverConnect
- connecting to driver [ODBC], SQLDriverConnect
- connecting to data source [ODBC], file data sources
- file data sources [ODBC]
ms.assetid: 3003f8c2-8be6-41cc-8d9c-612e9bd0f3ae
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e9e8fa3bbc90af4dc17d1dfc7e161fcab0177db7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-using-file-data-sources"></a>Connexion à l’aide de Sources de données fichier
Les informations de connexion pour une source de données de fichier sont stockées dans un fichier .dsn. Par conséquent, la chaîne de connexion peut être utilisée à plusieurs reprises à un seul utilisateur ou partagée par plusieurs utilisateurs, s’ils ont le pilote approprié installé. Le fichier contient un nom de pilote (ou un autre nom de source de données dans le cas d’une source de données fichier partageable) et le cas échéant, la chaîne de connexion qui peut être utilisée par **SQLDriverConnect**. Le Gestionnaire de pilotes génère la chaîne de connexion pour l’appel à **SQLDriverConnect** à partir de mots clés dans le fichier .dsn.  
  
 Une source de données de fichier permet à une application spécifier les options de connexion sans avoir à créer une chaîne de connexion pour une utilisation avec **SQLDriverConnect**. La source de données de fichier est généralement créée en spécifiant le **SAVEFILE** (mot clé), ce qui entraîne le Gestionnaire de pilotes pour enregistrer la chaîne de connexion de sortie créée par un appel à **SQLDriverConnect** au fichier .dsn. Que la chaîne de connexion peut être utilisée à plusieurs reprises en appelant **SQLDriverConnect** avec la **FILEDSN** (mot clé). Cela simplifie le processus de connexion et fournit une source persistante de la chaîne de connexion.  
  
 Sources de données de fichier peuvent également être créés en appelant **SQLCreateDataSource** dans la DLL de programme d’installation. Informations peuvent être écrits dans le fichier .dsn en appelant **SQLWriteFileDSN**et lire le fichier .dsn en appelant **SQLReadFileDSN**; ces deux fonctions sont également dans le programme d’installation DLL. Pour plus d’informations sur le programme d’installation DLL, consultez [configuration des Sources de données](../../../odbc/reference/install/configuring-data-sources.md).  
  
 Les mots clés utilisés pour les informations de connexion sont dans la section [ODBC] d’un fichier .dsn. Les informations minimales qu’un fichier .dsn partageable aurait dans la section [ODBC] sont le mot clé DRIVER :  
  
```  
DRIVER = SQL Server  
```  
  
 Le fichier .dsn partageable contient généralement une chaîne de connexion, comme suit :  
  
```  
DRIVER = SQL Server  
UID = Larry  
DATABASE = MyDB  
```  
  
 Lorsque la source de données de fichier est partageable, le fichier .dsn contient uniquement un **DSN** (mot clé). Lorsque le Gestionnaire de pilotes est envoyé les informations dans une source de données fichier partageable, il se connecte que nécessaire pour la source de données indiquée par la **DSN** (mot clé). Un fichier .dsn partageable doit contenir le mot clé suivant :  
  
```  
DSN = MyDataSource  
```  
  
 La chaîne de connexion utilisée pour une source de données de fichier est l’union entre les mots clés spécifiés dans le fichier .dsn et les mots clés spécifiés dans la chaîne de connexion dans l’appel à **SQLDriverConnect**. Si les mots clés dans le fichier .dsn sont en conflit avec les mots clés dans la chaîne de connexion, le Gestionnaire de pilotes décide dont la valeur de mot clé doit être utilisée. Pour plus d’informations, consultez [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="see-also"></a>Voir aussi  
 [http://support.microsoft.com/kb/165866](http://support.microsoft.com/kb/165866)
