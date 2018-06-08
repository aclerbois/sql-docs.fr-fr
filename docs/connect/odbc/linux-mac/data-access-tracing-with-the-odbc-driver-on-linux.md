---
title: Traçage de l’accès aux données avec le pilote ODBC sur Linux et macOS | Documents Microsoft
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
- data access tracing
- tracing
ms.assetid: 3149173a-588e-47a0-9f50-edb8e9adf5e8
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 64a04e7c448161c22ca9a671e5fdbe706829bced
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-access-tracing-with-the-odbc-driver-on-linux-and-macos"></a>Traçage de l’accès aux données avec le pilote ODBC sur Linux et Mac OS
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Le Gestionnaire de pilotes unixODBC sur macOS et Linux prend en charge le traçage de l’entrée des appels API ODBC et de sortie du pilote ODBC pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].

Pour tracer le comportement de votre application ODBC, vous devez modifier le `odbcinst.ini` du fichier `[ODBC]` section pour définir les valeurs `Trace=Yes` et `TraceFile` pour le chemin d’accès du fichier qui doit contenir la trace de sortie ; par exemple :

```  
Trace=Yes
TraceFile=/home/myappuser/odbctrace.log
```  

(Vous pouvez également utiliser `/dev/stdout` ou tout autre nom de périphérique pour envoyer la sortie il au lieu d’un fichier permanent de trace.) Avec les paramètres ci-dessus, chaque fois qu’une application charge le Gestionnaire de pilotes unixODBC il enregistre tous les appels d’API ODBC qui il effectuée dans le fichier de sortie.

Une fois que vous avez terminé de traçage de votre application, supprimez `Trace=Yes` à partir de la `odbcinst.ini` pour éviter l’altération des performances du suivi de fichier et vérifiez les fichiers de trace inutiles sont supprimés.
  
Le traçage s’applique à toutes les applications qui utilisent le pilote dans `odbcinst.ini`. Pour ne pas tracer toutes les applications (par exemple, pour éviter la divulgation d’informations sensibles par utilisateur), vous pouvez tracer une instance d’application individuelle en lui fournissant l’emplacement de privé `odbcinst.ini`, à l’aide du `ODBCSYSINI` variable d’environnement. Par exemple :  
  
```  
$ ODBCSYSINI=/home/myappuser myapp
```  
  
Dans ce cas, vous pouvez ajouter `Trace=Yes` à la `[ODBC Driver 13 for SQL Server]` section de `/home/myappuser/odbcinst.ini`.

## <a name="determining-which-odbcini-file-the-driver-is-using"></a>Détermination du fichier odbc.ini que le pilote est à l’aide

Les pilotes ODBC de Linux et macOS ne savez pas quelle `odbc.ini` est utilisé, ou le chemin d’accès à la `odbc.ini` fichier. Toutefois, les informations sur les `odbc.ini` fichier se trouve dans utilisation n’est disponible à partir des outils unixODBC `odbc_config` et `odbcinst`et à partir de la documentation du Gestionnaire de pilotes unixODBC.  
  
Par exemple, la commande suivante imprime (parmi d’autres informations), l’emplacement du système et utilisateur `odbc.ini` les fichiers qui contiennent des sources de données système et utilisateur, respectivement :

```
$ odbcinst -j
unixODBC 2.3.1
DRIVERS............: /etc/odbcinst.ini
SYSTEM DATA SOURCES: /etc/odbc.ini
FILE DATA SOURCES..: /etc/ODBCDataSources
USER DATA SOURCES..: /home/odbcuser/.odbc.ini`
SQLULEN Size.......: 8
SQLLEN Size........: 8
SQLSETPOSIROW Size.: 8
```

Le [documentation unixODBC](http://www.unixodbc.org/doc/UserManual/) explique les différences entre l’utilisateur et les sources de données système. En résumé :  

- Sources de données utilisateur---il s’agit de sources de données qui sont uniquement disponibles pour un utilisateur spécifique. Les utilisateurs peuvent se connecter à l’aide, ajouter, modifier et supprimer leur propres sources de données utilisateur. Sources de données utilisateur sont stockées dans un fichier dans le répertoire de base ou un sous-répertoire.
  
- Système DSN---ces sources de données sont disponibles pour chaque utilisateur sur le système de se connecter à leur utilisation, mais peuvent uniquement être ajoutés, modifiés et supprimés par un administrateur système. Si un utilisateur a un DSN utilisateur avec le même nom qu’un DSN système, le DSN utilisateur sera utilisé lors des connexions à cet utilisateur.

## <a name="see-also"></a>Voir aussi
[Instructions de programmation](../../../connect/odbc/linux-mac/programming-guidelines.md)
