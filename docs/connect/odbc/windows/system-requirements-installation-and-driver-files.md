---
title: Configuration système requise, Installation et fichiers de pilote | Documents Microsoft
ms.custom: ''
ms.date: 02/14/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d90fa182-1dab-4d6f-bd85-a04dd1479986
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 10e4d020bac05317e76dabea46495d58def71cf0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="system-requirements-installation-and-driver-files"></a>Configuration système requise, installation et fichiers de pilote
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

ODBC Driver 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] prend en charge les connexions à SQL Server 2014, SQL Server 2012 R2, [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro_md.md)], [!INCLUDE[ssKatmai](../../../includes/sskatmai_md.md)]et [!INCLUDE[ssVersion2005](../../../includes/ssversion2005_md.md)].  
  
ODBC Driver 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows peut être installé sur un ordinateur qui possède également une ou plusieurs versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] Native Client.  
  
ODBC Driver 13 et 13.1 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)], outre les éléments ci-dessus, prend en charge de SQL Server 2016. 

Le 17 du pilote ODBC pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] prend en charge toutes celles ci-dessus et également sur SQL Server 2017.
  
Le nom du pilote que vous spécifiez dans une chaîne de connexion est `ODBC Driver 11 for SQL Server` ou `ODBC Driver 13 for SQL Server` (pour 13 et 13.1) ou `ODBC Driver 17 for SQL Server`.
  
## <a name="supported-operating-systems"></a>Systèmes d'exploitation pris en charge

Vous pouvez exécuter des applications avec le pilote sur les systèmes d’exploitation Windows suivants :  

-   Windows Server 2008 R2 
-   Windows Server 2012
-   Windows Server 2012 R2    
-   Windows Vista SP2 *(le pilote ODBC 11 uniquement)*  
-   Windows 7  
-   Windows 8
-   Windows 8.1
-   Windows 10
  
## <a name="installing-microsoft-odbc-driver-for-sql-server"></a>Installation du pilote ODBC de Microsoft pour SQL Server

Le pilote est installé lorsque vous exécutez `msodbcsql.msi` à partir d’un des liens suivants :

- [Télécharger le 17 du pilote Microsoft ODBC pour SQL Server sur Windows](https://www.microsoft.com/download/details.aspx?id=56567)
- [Télécharger Microsoft ODBC Driver 13.1 for SQL Server sur Windows](https://www.microsoft.com/download/details.aspx?id=53339)
- [Télécharger Microsoft ODBC Driver 13 for SQL Server sur Windows](https://www.microsoft.com/download/details.aspx?id=50420)
- [Télécharger Microsoft ODBC Driver 11 for SQL Server sur Windows](https://www.microsoft.com/download/details.aspx?id=36434). 

Il peut être installé côte à côte avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] Native Client.  

Quand vous appelez `msodbcsql.msi`, seuls les composants clients sont installés par défaut. Les composants clients sont des fichiers qui prennent en charge l’exécution d’une application qui a été développée à l’aide du pilote. Pour installer les composants du Kit de développement logiciel, spécifiez `ADDLOCAL=ALL` sur la ligne de commande. Par exemple :  
  
```  
msiexec /i msodbcsql.msi ADDLOCAL=ALL  
```  
  
 Spécifiez `IACCEPTMSODBCSQLLICENSETERMS=YES` pour accepter les termes du contrat de licence utilisateur final, si vous utilisez la `/passive`, `/qn`, `/qb`, ou `/qr` option d’installation. Cette option doit être spécifiée tout en majuscules. Par exemple :  
  
```  
msiexec /quiet /passive /qn /i msodbcsql.msi IACCEPTMSODBCSQLLICENSETERMS=YES ADDLOCAL=ALL  
```  
  
 Pour effectuer une désinstallation sans assistance :  
  
```  
msiexec /quiet /passive /qn /uninstall msodbcsql.msi  
```  
  
Lorsqu’une application utilise le pilote, l’application doit indiquer qu’elle dépend du pilote via l’option d’installation `APPGUID`. Ainsi, le programme d’installation du pilote pour signaler les applications dépendantes avant de désinstaller. Pour spécifier une dépendance sur le pilote, définissez la `APPGUID` un paramètre de ligne de commande à votre code de produit lors de l’installation sans assistance du pilote. (Un code de produit doit être créé si vous utilisez Microsoft Installer pour regrouper votre programme d’installation d’application.) Par exemple :  
  
```  
msiexec /i msodbcsql.msi APPGUID={ <Your dependent application's APPGUID> }  
```  

## <a name="command-line-tools-sqlcmdexe-and-bcpexe"></a>Outils de ligne de commande : sqlcmd.exe et bcp.exe

Le `bcp.exe` et `sqlcmd.exe` des outils pour l’utiliser avec le pilote peut être téléchargée à [utilitaires de ligne de commande Microsoft 11 pour SQL Server](http://www.microsoft.com/download/details.aspx?id=36433), [13 d’utilitaires de ligne de commande Microsoft pour SQL Server](https://www.microsoft.com/download/details.aspx?id=52680), ou [Utilitaires de ligne de commande Microsoft 13.1 pour SQL Server](https://www.microsoft.com/download/details.aspx?id=53591). Le pilote est un composant requis pour installer `sqlcmd.exe` et `bcp.exe`.
  
`bcp.exe` et `sqlcmd.exe` sont installés dans le `110\Tools` sous-dossier de `%PROGRAMFILES%\Microsoft SQL Server\Client SDK\ODBC` pour la version 11, et `130\Tools` pour 13 et 13.1.

Une application qui utilise les fonctions BCP doit spécifier le pilote à partir de la même version que celle qui est fournie avec le fichier d’en-tête et la bibliothèque utilisés pour compiler l’application.  

Par exemple, lorsque vous compilez une application ODBC avec `msodbcsql11.lib` et `msodbcsql.h`, utilisez « DRIVER = {ODBC Driver 11 for SQL Server} » dans la chaîne de connexion.

## <a name="components-of-the-microsoft-odbc-driver-for-includessnoversionincludesssnoversionmdmd-on-windows"></a>Composants de Microsoft ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows 
 Le pilote ODBC sur Windows contient les composants suivants :
 
|Composant| Description|  
|---------------|-----------------|  
|msodbcsql17.dll ou <br> msodbcsql13.dll ou <br> msodbcsql11.dll|Fichier DDL (Dynamic-Link Library) contenant l’ensemble des fonctionnalités du pilote. Ce fichier est installé dans % SYSTEMROOT%\System32.|  
|msodbcdiag17.dll ou <br> msodbcdiag13.dll ou <br> msodbcdiag11.dll|Le fichier de bibliothèque de liens dynamiques (DLL) qui contient l’interface de diagnostics (suivi) du pilote. Ce fichier est installé dans % SYSTEMROOT%\System32.|
|msodbcsqlr17.rll ou <br> msodbcsqlr13.rll ou <br> msodbcsqlr11.rll|Fichier de ressources qui accompagne la bibliothèque du pilote. Ce fichier est installé dans % SYSTEMROOT%\System32\1033.| 
|s13ch_msodbcsql.chm ou <br> s11ch_msodbcsql.chm |Le fichier d’aide Assistant Source de données qui explique comment créer une source de données pour le pilote. Ce fichier est installé dans %SYSTEMROOT%\System32\1033 <br /> <br /> **Remarque :** aucun fichier chm 17 du pilote ODBC. |  
|msodbcsql.h|Le fichier d’en-tête qui contient toutes les nouvelles définitions nécessaires à l’utilisation du pilote.<br /><br /> **Remarque :**  vous ne pouvez pas référencer msodbcsql.h et odbcss.h dans le même programme.<br /><br /> msodbcsql.h 17 du pilote ODBC ou 13 est installé dans %PROGRAMFILES%\Microsoft SQL Server\Client SDK\ODBC\130\SDK. <br /> msodbcsql.h pour ODBC Driver 11 est installé dans %PROGRAMFILES%\Microsoft SQL Server\Client SDK\ODBC\110\SDK.| 
|msodbcsql17.lib ou <br> msodbcsql13.lib ou <br> msodbcsql11.lib|Le fichier bibliothèque nécessaire pour appeler le **bcp** fonctions utilitaires qui font partie du pilote.<br /><br /> **Remarque :** si vous référencez ce fichier de bibliothèque dans votre programme, assurez-vous qu’il est dans votre chemin système et dans le chemin d’accès de système de ceux qui utilisent l’application.<br /><br /> msodbcsql17.lib ou msodbcsql13.lib est installé dans %PROGRAMFILES%\Microsoft SQL Server\Client SDK\ODBC\130\SDK.<br /> msodbcsql11.lib est installé dans %PROGRAMFILES%\Microsoft SQL Server\Client SDK\ODBC\110\SDK.|

  
## <a name="see-also"></a>Voir aussi  
 [Pilote Microsoft ODBC pour SQL Server sur Windows](../../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)  
  
  
