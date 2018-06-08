---
title: SQL Server Express LocalDB en-tête et les informations de Version | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: localdb
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apilocation:
- sqluserinstance.dll
ms.assetid: 506b5161-b902-4894-b87b-9192d7b1664a
caps.latest.revision: 16
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: ec6642e7f975041b3aa8f279eeee80a867fe9f43
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-express-localdb-header-and-version-information"></a>En-tête et informations de version SQL Server Express LocalDB
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Il n'y a aucun fichier d'en-tête distinct pour l'API de l'instance de base de données locale SQL Server Express (LocalDB) ; les signatures de la fonction et les codes d'erreur de LocalDB sont définis dans le fichier d'en-tête SQL Server Native Client (sqlncli.h). Pour utiliser l'API d'instance de LocalDB, vous devez inclure le fichier d'en-tête sqlncli.h dans votre projet.  
  
## <a name="localdb-versioning"></a>Contrôle de version de LocalDB  
 L'installation de LocalDB utilise un jeu unique de binaires par version principale de SQL Server. Ces versions de LocalDB sont conservées et des correctifs de logiciel sont appliqués indépendamment. Cela signifie que l'utilisateur doit spécifier la version de base de LocalDB (autrement dit, la version principale de SQL Server) qu'il utilisera. La version est spécifiée dans le format de la version standard défini par le .NET Framework **System.Version** classe :  
  
 *major.minor]]*  
  
 Les deux premiers nombres dans la chaîne de version (*majeure* et *secondaire*) sont obligatoires. Les deux derniers nombres dans la chaîne de version (*générer* et *révision*) sont facultatifs et la valeur par défaut zéro si l’utilisateur les omet. Cela signifie que si l'utilisateur ne spécifie que « 12.2 » comme numéro de version de LocalDB, le numéro de version sera traité comme si « 12.2.0.0 » avait été spécifié.  
  
 La version pour l'installation de LocalDB est définie dans la clé de Registre MSSQLServer\CurrentVersion sous la clé de Registre de l'instance de SQL Server, par exemple :  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL13E.LOCALDB\ MSSQLServer\CurrentVersion: "CurrentVersion"="12.0.2531.0"  
```  
  
 Plusieurs versions de LocalDB sur la même station de travail sont prises en charge côte à côte. Toutefois, le code utilisateur utilise toujours la dernière version disponible **SQLUserInstance** DLL sur l’ordinateur local pour se connecter aux instances de LocalDB.  
  
## <a name="locating-the-sqluserinstance-dll"></a>Recherche de la DLL SQLUserInstance  
 Pour localiser le **SQLUserInstance** DLL, le fournisseur client utilise la clé de Registre suivante :  
  
```  
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Local DB\Installed Versions]  
```  
  
 Sous cette clé il existe une liste des clés, une pour chaque version de LocalDB installée sur l'ordinateur. Chacune de ces clés est nommé avec le numéro de version de base de données locale dans le format  *\<version majeure >*. *\<version mineure >* (par exemple, la clé pour [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] nommé 13.0). Sous chaque clé de version il existe des paires nom-valeur de `InstanceAPIPath` qui définissent le chemin d'accès complet au fichier de SQLUserInstance.dll installé avec cette version. L’exemple suivant montre les entrées de Registre pour un ordinateur qui a des versions de LocalDB 11.0 et 13.0 installé :  
  
```  
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Local DB\Installed Versions\13.0]  
"InstanceAPIPath"="C:\\Program Files\\Microsoft SQL Server\\130\\LocalDB\\Binn\\SqlUserInstance.dll"  
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Local DB\Installed Versions\13.0]  
"InstanceAPIPath"="C:\\Program Files\\Microsoft SQL Server\\130\\LocalDB\\Binn\\SqlUserInstance.dll"]  
```  
  
 Le fournisseur client doit rechercher la version la plus récente parmi toutes les versions installées et charge le **SQLUserInstance** fichier DLL d’associé `InstanceAPIPath` valeur.  
  
### <a name="wow64-mode-on-64-bit-windows"></a>Mode WOW64 sur Windows 64 bits  
 Les installations 64 bits de LocalDB ont un jeu supplémentaire de clés de Registre pour permettre aux applications 32 bits s'exécutant en mode Windows-32-on-Windows-64 (WOW64) d'utiliser LocalDB. En particulier, sur Windows 64 bits, le fichier MSI de LocalDB crée les clés de Registre suivantes :  
  
```  
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Wow6432Node\Microsoft SQL Server Local DB\Installed Versions\13.0]  
"InstanceAPIPath"="C:\\Program Files (x86)\\Microsoft SQL Server\\130\\LocalDB\\Binn\\SqlUserInstance.dll"  
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Wow6432Node\Microsoft SQL Server Local DB\Installed Versions\13.0]  
"InstanceAPIPath"="C:\\Program Files (x86)\\Microsoft SQL Server\\130\\LocalDB\\Binn\\SqlUserInstance.dll"]  
  
```  
  
 programmes 64 bits lors de la lecture le `Installed Versions` clé s’affiche les valeurs qui pointent vers les versions 64 bits de la **SQLUserInstance** DLL, tandis que les programmes 32 bits (en cours d’exécution sur Windows 64 bits en mode WOW64) seront automatiquement redirigés vers un `Installed Versions` clé se trouve sous le `Wow6432Node` hive. Cette clé contient des valeurs qui pointent vers les versions 32 bits de le **SQLUserInstance** DLL.  
  
## <a name="using-localdbdefineproxyfunctions"></a>Utilisation de LOCALDB_DEFINE_PROXY_FUNCTIONS  
 L’API d’instance de LocalDB définit une constante nommée LOCALDB_DEFINE_PROXY_FUNCTIONS qui automatise la découverte et le chargement de la **SqlUserInstance** DLL.  
  
 La section de code activée par cette constante fournit une implémentation des proxys pour chacune des API de LocalDB. Les implémentations de proxy utilisent une fonction commune pour lier à des points d’entrée dans la dernière version installée **SqlUserInstance** DLL, puis transférer les demandes.  
  
 Les fonctions de proxy ne sont activées que si la constante LOCALDB_DEFINE_PROXY_FUNCTIONS est définie dans le code utilisateur avant d'inclure le fichier sqlncli.h. La constante doit être définie dans un seul module source (fichier .cpp), car elle définit les noms de fonctions externes pour tous les points d'entrée d'API. Elle fournit une implémentation des proxys pour chacune des API de LocalDB.  
  
 L'exemple de code suivant montre comment utiliser la macro du code C++ natif :  
  
```  
// Define the LOCALDB_DEFINE_PROXY_FUNCTIONS constant to enable the LocalDB proxy functions   
// The #define has to take place BEFORE the API header file (sqlncli.h) is included  
#define LOCALDB_DEFINE_PROXY_FUNCTIONS  
#include <sqlncli.h>  
…  
HRESULT hr = S_OK;  
  
// Create LocalDB instance by calling the create API proxy function included by macro  
if (FAILED(hr = LocalDBCreateInstance( L"12.0", L"name", 0)))  
{  
…  
}  
…  
  
```  
  
  
