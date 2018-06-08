---
title: Configuration système requise pour les pilotes Microsoft pour PHP pour SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 03/23/2018
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- requirements
ms.assetid: 5db4b75f-c605-4785-9560-399a533c0fc9
caps.latest.revision: 93
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1c1eae99587a1f447b809becee9509d5a04136e3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="system-requirements-for-the-microsoft-drivers-for-php-for-sql-server"></a>Configuration système requise pour les pilotes Microsoft pour PHP pour SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Ce document répertorie les composants qui doivent être installés sur votre système pour accéder aux données dans une base de données SQL Azure ou de SQL Server à l’aide du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].

Versions 3.1 et ultérieures des pilotes Microsoft PHP pour SQL Server sont officiellement prises en charge. Pour plus d’informations sur les cycles de vie de prise en charge et configuration requise, y compris les versions antérieures des pilotes PHP, consultez la [matrice de prise en charge](../../connect/php/microsoft-php-drivers-for-sql-server-support-matrix.md).

## <a name="php"></a>PHP

Pour plus d’informations sur la façon de télécharger et installer les derniers fichiers binaires de stable PHP, consultez [le site web PHP](http://php.net).  Le Microsoft Drivers for PHP for SQL Server nécessitent les versions suivantes de PHP :

|PHP pour la version du pilote SQL Server&#8594;<br />&#8595;Version de PHP|5.2<br />&nbsp;|4.3<br />&nbsp;|4.0<br />&nbsp;|3.2<br />&nbsp;|3.1<br />&nbsp;|
|---|---|---|---|---|---|
|7.2|7.2.1+ sur Windows<br/>7.2.0+ sur d’autres plateformes | | | | |
|7.1|7.1.0+ |7.1.0+ |       |        |        |
|7.0|7.0.0+ |7.0.0+ |7.0.0+ |        |        |
|5.6|       |       |       |5.6.4 +  |        |
|5.5|       |       |       |5.5.16 + |5.5.16 + |
|5.4|       |       |       |5.4.32  |5.4.32  |

-   Une version du fichier de pilote doit se trouver dans votre répertoire d’extensions PHP. Consultez [Versions de pilotes](#driver-versions) pour plus d’informations sur les différents fichiers de pilotes.  Pour télécharger les pilotes, consultez [télécharger le Microsoft Drivers for PHP pour SQL Server](download-drivers-php-sql-server.md). Pour plus d’informations sur la configuration du pilote pour PHP, consultez [chargement le Microsoft Drivers for PHP pour SQL Server](../../connect/php/loading-the-php-sql-driver.md).

-   Un serveur web est nécessaire. Votre serveur web doit être configuré pour exécuter PHP. Pour plus d’informations sur l’hébergement d’applications PHP avec IIS, consultez la [didacticiel sur le site web de PHP](http://php.net/manual/fa/install.windows.iis.php).  

    Le [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] a été testé à l’aide d’IIS 10 avec FastCGI.  

    > [!NOTE]  
    > Microsoft prend en charge uniquement IIS.  

## <a name="odbc-driver"></a>Pilote ODBC

La version correcte du pilote Microsoft ODBC pour SQL Server est requis sur l’ordinateur sur lequel s’exécute PHP. Télécharger à partir des liens suivants :
- [Pilote ODBC Microsoft 17 pour SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=56567)
- [Microsoft ODBC Driver 13.1 for SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=53339)
- [Microsoft ODBC Driver 13 for SQL Server](https://www.microsoft.com/download/details.aspx?id=50420)
- [Microsoft ODBC Driver 11 pour SQL Server](http://www.microsoft.com/download/details.aspx?id=36434)

Si vous utilisez un système d’exploitation de 64 bits, le programme d’installation ODBC 64 bits installe des pilotes ODBC 32 bits et 64 bits. Si vous utilisez un système d’exploitation 32 bits, utilisez ODBC x86 programme d’installation.

|PHP pour la version du pilote SQL Server&#8594;<br />&#8595;Version du pilote ODBC|5.2<br />&nbsp;|4.3<br />&nbsp;|4.0<br />&nbsp;|3.2<br />&nbsp;|3.1<br />&nbsp;|
|---|---|---|---|---|---|
|Pilote ODBC 17  |O| | | | |
|ODBC Driver 13.1|O|O|O| | |
|ODBC Driver 13  | | |O| | |
|Pilote ODBC 11  |O|O|O|O|O|

Si vous utilisez le pilote SQLSRV, [sqlsrv_client_info](../../connect/php/sqlsrv-client-info.md) retourne des informations sur la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Microsoft ODBC Driver for SQL Server est utilisé par le [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Si vous utilisez le pilote PDO_SQLSRV, vous pouvez utiliser [PDO::getAttribute](../../connect/php/pdo-getattribute.md) pour découvrir la version.  

## <a name="sql-server"></a>SQL Server

Bases de données SQL Azure sont pris en charge. Pour plus d’informations, consultez [la connexion à la base de données SQL Microsoft Azure](../../connect/php/connecting-to-microsoft-azure-sql-database.md).

|PHP pour la version du pilote SQL Server&#8594;<br />&#8595;Version de SQL Server|5.2<br />&nbsp;|4.3<br />&nbsp;|4.0<br />&nbsp;|3.2<br />&nbsp;|3.1<br />&nbsp;|
|---|---|---|---|---|---|
|Instance gérée de SQL Azure<br/> (Version préliminaire privée étendue)|O|O| | | |
|Azure SQL Data Warehouse|O|O| | | |
|SQL Server 2017   |O|O| | | |
|SQL Server 2016   |O|O|O| | |
|SQL Server 2014   |O|O|O|O|O|
|SQL Server 2012   |O|O|O|O|O|
|SQL Server 2008 R2|O|O|O|O|O|
|SQL Server 2008   | | |O|O|O|

## <a name="operating-systems"></a>Systèmes d'exploitation
Les systèmes d’exploitation pris en charge pour les versions du pilote sont les suivantes :

|PHP pour la version du pilote SQL Server&#8594;<br />&#8595;Système d'exploitation|5.2<br />&nbsp;|4.3<br />&nbsp;|4.0<br />&nbsp;|3.2<br />&nbsp;|3.1<br />&nbsp;|
|---|---|---|---|---|---|
|Windows Server 2016                      |O|O| | | |
|Windows Server 2012 R2                   |O|O|O|O|O|
|Windows Server 2012                      |O|O|O|O|O|
|Windows Server 2008 R2 SP1               | | |O|O|O|
|Windows Server 2008 SP2                  | | |O|O|O|
|Windows 10                               |O|O|O| | |
|Windows 8.1                              |O|O|O|O|O|
|Windows 8                                | |O|O|O|O|
|Windows 7 SP1                            | | |O|O|O|
|Windows Vista SP2                        | | |O|O|O|
|Ubuntu 17.10 (64 bits)                    |O| | | | |
|Ubuntu 16.04 (64 bits)                    |O|O|O| | |
|15.10 d’Ubuntu (64 bits)                    | |O| | | |
|Ubuntu 15.04 (64 bits)                    | | |O| | |
|Debian 9 (64 bits)                        |O| | | | |
|Debian 8 (64 bits)                        |O|O| | | |
|Red Hat Enterprise Linux 7 (64 bits)      |O|O|O| | |
|SuSE Enterprise Linux 12 (64 bits)        |O| | | | |
|macOS Sierra (64 bits)                    |O|O| | | |
|macOS El Capitan (64 bits)                |O|O| | | |

## <a name="driver-versions"></a>Versions de pilotes  
Cette section répertorie les fichiers de pilote qui sont incluses avec chaque version de la [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Chaque package d’installation contient les fichiers de pilote SQLSRV et PDO_SQLSRV dans des variants et non-thread. Sous Windows, ils sont également disponibles dans les variantes 32 bits et 64 bits. Pour configurer le pilote à utiliser avec le runtime PHP, suivez les instructions d’installation dans [chargement le Microsoft Drivers for PHP pour SQL Server](../../connect/php/loading-the-php-sql-driver.md).

Sur les versions prises en charge de Linux et macOS, les pilotes appropriés peuvent être installés à l’aide du système de package PECL de PHP, suivant le [des instructions d’installation Linux et macOS](../../connect/php/installation-tutorial-linux-mac.md). Vous pouvez également télécharger binaires prédéfinis pour votre plateforme de la [Microsoft Drivers for PHP for SQL Server](https://github.com/Microsoft/msphpsql/releases) page de projet Github--les tableaux ci-dessous répertorient les fichiers figurant dans les packages binaire prégénérées.

**Pilotes Microsoft 5.2 SQL Server pour PHP :**  

Sous Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version de PHP|Thread-safe ?|Utiliser avec .dll PHP|  
|---------------|---------------|----------------|---------------------|  
|php_sqlsrv_7_nts.dll de 32 bits <br />php_pdo_sqlsrv_7_nts.dll de 32 bits |7.0|non |php7.dll de 32 bits|
|php_sqlsrv_7_ts.dll de 32 bits  <br />php_pdo_sqlsrv_7_ts.dll de 32 bits  |7.0|oui|php7ts.dll de 32 bits|
|php_sqlsrv_7_nts.dll de 64 bits <br />php_pdo_sqlsrv_7_nts.dll de 64 bits |7.0|non |php7.dll de 64 bits|  
|php_sqlsrv_7_ts.dll de 64 bits  <br />php_pdo_sqlsrv_7_ts.dll de 64 bits  |7.0|oui|php7ts.dll de 64 bits|
|php_sqlsrv_71_nts.dll de 32 bits<br />php_pdo_sqlsrv_71_nts.dll de 32 bits|7.1|non |php7.dll de 32 bits|  
|php_sqlsrv_71_ts.dll de 32 bits <br />php_pdo_sqlsrv_71_ts.dll de 32 bits |7.1|oui|php7ts.dll de 32 bits|  
|php_sqlsrv_71_nts.dll de 64 bits<br />php_pdo_sqlsrv_71_nts.dll de 64 bits|7.1|non |php7.dll de 64 bits|  
|php_sqlsrv_71_ts.dll de 64 bits <br />php_pdo_sqlsrv_71_ts.dll de 64 bits |7.1|oui|php7ts.dll de 64 bits|   
|php_sqlsrv_72_nts.dll de 32 bits<br />php_pdo_sqlsrv_72_nts.dll de 32 bits|7.2|non |php7.dll de 32 bits|  
|php_sqlsrv_72_ts.dll de 32 bits <br />php_pdo_sqlsrv_72_ts.dll de 32 bits |7.2|oui|php7ts.dll de 32 bits|  
|php_sqlsrv_72_nts.dll de 64 bits<br />php_pdo_sqlsrv_72_nts.dll de 64 bits|7.2|non |php7.dll de 64 bits|  
|php_sqlsrv_72_ts.dll de 64 bits <br />php_pdo_sqlsrv_72_ts.dll de 64 bits |7.2|oui|php7ts.dll de 64 bits|  

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version de PHP|Thread-safe ?|
|---------------|---------------|----------------|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|non |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|oui|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|non |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|oui|  
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|non |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|oui|

**Pilotes Microsoft 4.3 pour PHP pour SQL Server :**  

Sous Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version de PHP|Thread-safe ?|Utiliser avec .dll PHP|  
|---------------|---------------|----------------|---------------------|  
|php_sqlsrv_7_nts.dll de 32 bits <br />php_pdo_sqlsrv_7_nts.dll de 32 bits |7.0|non |php7.dll de 32 bits|
|php_sqlsrv_7_ts.dll de 32 bits  <br />php_pdo_sqlsrv_7_ts.dll de 32 bits  |7.0|oui|php7ts.dll de 32 bits|
|php_sqlsrv_7_nts.dll de 64 bits <br />php_pdo_sqlsrv_7_nts.dll de 64 bits |7.0|non |php7.dll de 64 bits|  
|php_sqlsrv_7_ts.dll de 64 bits  <br />php_pdo_sqlsrv_7_ts.dll de 64 bits  |7.0|oui|php7ts.dll de 64 bits|
|php_sqlsrv_71_nts.dll de 32 bits<br />php_pdo_sqlsrv_71_nts.dll de 32 bits|7.1|non |php7.dll de 32 bits|  
|php_sqlsrv_71_ts.dll de 32 bits <br />php_pdo_sqlsrv_71_ts.dll de 32 bits |7.1|oui|php7ts.dll de 32 bits|  
|php_sqlsrv_71_nts.dll de 64 bits<br />php_pdo_sqlsrv_71_nts.dll de 64 bits|7.1|non |php7.dll de 64 bits|  
|php_sqlsrv_71_ts.dll de 64 bits <br />php_pdo_sqlsrv_71_ts.dll de 64 bits |7.1|oui|php7ts.dll de 64 bits|   

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version de PHP|Thread-safe ?|
|---------------|---------------|----------------|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|non |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|oui|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|non |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|oui|  

**Pilotes Microsoft 4.0 pour PHP pour SQL Server :**  

Sous Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version de PHP|Thread-safe ?|Utiliser avec .dll PHP|  
|---------------|---------------|----------------|---------------------|  
|php_sqlsrv_7_nts_x86.dll<br />php_pdo_sqlsrv_7_nts_x86.dll|7.0|non|php7.dll de 32 bits|  
|php_sqlsrv_7_ts_x86.dll<br />php_pdo_sqlsrv_7_ts_x86.dll|7.0|oui|php7ts.dll de 32 bits|  
|php_sqlsrv_7_nts_x64.dll<br />php_pdo_sqlsrv_7_nts_x64.dll|7.0|non|php7.dll de 64 bits|  
|php_sqlsrv_7_ts_x64.dll<br />php_pdo_sqlsrv_7_ts_x64.dll|7.0|oui|php7ts.dll de 64 bits|   

Sur Linux, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version de PHP|Thread-safe ?|
|---------------|---------------|----------------|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|non |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|oui|

**Pilotes Microsoft 3.2 pour PHP pour SQL Server :**  

Sous Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version de PHP|Thread-safe ?|Utiliser avec .dll PHP|  
|---------------|---------------|----------------|---------------------|  
|php_sqlsrv_54_nts.dll<br />php_pdo_sqlsrv_54_nts.dll|5.4|non|php5.dll|  
|php_sqlsrv_54_ts.dll<br />php_pdo_sqlsrv_54_ts.dll|5.4|oui|php5ts.dll|  
|php_sqlsrv_55_nts.dll<br />php_pdo_sqlsrv_55_nts.dll|5.5|non|php5.dll|  
|php_sqlsrv_55_ts.dll<br />php_pdo_sqlsrv_55_ts.dll|5.5|oui|php5ts.dll|  
|php_sqlsrv_56_nts.dll<br />php_pdo_sqlsrv_56_nts.dll|5.6|non|php5.dll|  
|php_sqlsrv_56_ts.dll<br />php_pdo_sqlsrv_56_ts.dll|5.6|oui|php5ts.dll|  

**Pilotes Microsoft 3.1 pour PHP pour SQL Server :**  

Sous Windows, les versions suivantes du pilote sont incluses :

|Fichier de pilote|Version de PHP|Thread-safe ?|Utiliser avec .dll PHP|  
|---------------|---------------|----------------|---------------------|  
|php_sqlsrv_54_nts.dll<br />php_pdo_sqlsrv_54_nts.dll|5.4|non|php5.dll|  
|php_sqlsrv_54_ts.dll<br />php_pdo_sqlsrv_54_ts.dll|5.4|oui|php5ts.dll|  
|php_sqlsrv_55_nts.dll<br />php_pdo_sqlsrv_55_nts.dll|5.5|non|php5.dll|  
|php_sqlsrv_55_ts.dll<br />php_pdo_sqlsrv_55_ts.dll|5.5|oui|php5ts.dll|  

## <a name="see-also"></a>Voir aussi  
[Mise en route avec les pilotes Microsoft PHP pour SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md)

[Guide de programmation pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)

[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Référence d’API du pilote PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)  
