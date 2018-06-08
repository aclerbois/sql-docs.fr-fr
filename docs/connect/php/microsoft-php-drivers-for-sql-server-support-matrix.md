---
title: Matrice de Support Microsoft Drivers for PHP for SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
caps.latest.revision: 1
author: David-Engel
ms.author: v-daveng
manager: ''
ms.openlocfilehash: b8f1fad66e906b71ff5ea247ba4615e7be774f63
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-php-drivers-for-sql-server-support-matrix"></a>Pilotes Microsoft PHP pour la matrice de prise en charge SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

  Cette page contient la stratégie de cycle de vie prise en charge et de la matrice de prise en charge des pilotes Microsoft PHP pour SQL Server.

## <a name="microsoft-php-drivers-support-lifecycle-matrix-and-policy"></a>Matrice de cycle de vie de prise en charge de pilotes Microsoft PHP et stratégie
 La politique de support de Microsoft fournit des informations transparentes et prévisibles concernant la politique de support des produits Microsoft. Versions de pilotes PHP 3.x, 4.x et 5.x ont cinq ans de support standard à partir de la date de version du pilote. Support standard est défini sur le [site Web du cycle de vie de prise en charge de Microsoft](https://support.microsoft.com/lifecycle).

 Options de support étendu et personnalisé ne sont pas disponibles pour les pilotes Microsoft PHP.

 Pilotes Microsoft PHP suivants sont pris en charge jusqu'à la date de fin de Support indiquée.

|Nom du pilote|Version de Package de pilotes|Fin de Support standard|
|-|-|-|
|Pilotes Microsoft PHP 5.2 pour SQL Server|5.2|9 février 2023|
|4.3 les pilotes Microsoft PHP pour SQL Server|4.3|6 juillet 2022|
|Pilotes Microsoft PHP 4.0 pour SQL Server|4.0|11 juillet 2021|
|3.2 les pilotes Microsoft PHP pour SQL Server|3.2|9 mars 2020|
|3.1 les pilotes Microsoft PHP pour SQL Server|3.1|12 décembre 2019|

 Pilotes Microsoft PHP suivants ne sont plus prises en charge.

|Nom du pilote|Version de Package de pilotes|Fin de Support standard|
|-|-|-|
|Pilotes Microsoft PHP 3.0 pour SQL Server|3|6 mars 2017|
|Pilotes Microsoft PHP 2.0 pour SQL Server|2|10 août 2015|
|Pilotes Microsoft PHP 1.0 pour SQL Server|1.0|28 avril 2014|

## <a name="sql-server-version-certified-compatibility"></a>Certifié de compatibilité de la Version de SQL Server
 Le tableau suivant répertorie les versions de SQL Server qui ont été testées et certifiées comme étant compatibles avec la version du pilote correspondant. Nous nous efforçons d’assurer la compatibilité descendante avec les versions précédentes du pilote, mais uniquement le dernier pilote pris en charge est été testé et certifié par de nouvelles versions de SQL Server que SQL Server est libérée.

|PHP pour la version du pilote SQL Server&#8594;<br />&#8595;Version de SQL Server|5.2<br />&nbsp;|4.3<br />&nbsp;|4.0<br />&nbsp;|3.2<br />&nbsp;|3.1<br />&nbsp;|3<br />&nbsp;|2<br />&nbsp;|
|---|---|---|---|---|---|---|---|
|Instance gérée de SQL Azure<br/> (Version préliminaire privée étendue)|O|O| | | | | |
|Azure SQL Data Warehouse|O|O| | | | | |
|SQL Server 2017   |O|O| | | | | |
|SQL Server 2016   |O|O|O| | | | |
|SQL Server 2014   |O|O|O|O|O| | |
|SQL Server 2012   |O|O|O|O|O|O| |
|SQL Server 2008 R2|O|O|O|O|O|O|O|
|SQL Server 2008   | | |O|O|O|O|O|

## <a name="php-version-support"></a>Prise en charge de la Version PHP
 Les versions suivantes de PHP sont pris en charge avec cette version des pilotes Microsoft PHP :

|PHP pour la version du pilote SQL Server&#8594;<br />&#8595;Version de PHP|5.2<br />&nbsp;|4.3<br />&nbsp;|4.0<br />&nbsp;|3.2<br />&nbsp;|3.1<br />&nbsp;|3<br />&nbsp;|2<br />&nbsp;|
|---|---|---|---|---|---|---|---|
|7.2|7.2.1+ sur Windows<br/>7.2.0+ sur d’autres plateformes| | | | | | |
|7.1|7.1.0+ |7.1.0+ |       |        |        |        |        |
|7.0|7.0.0+ |7.0.0+ |7.0.0+ |        |        |        |        |
|5.6|       |       |       |5.6.4 +  |        |        |        |
|5.5|       |       |       |5.5.16 + |5.5.16 + |        |        |
|5.4|       |       |       |5.4.32  |5.4.32  |5.4.32  |        |
|5.3|       |       |       |        |        |5.3.0   |5.3.0   |
|5.2|       |       |       |        |        |        |5.2.4<br />5.2.13|

## <a name="supported-operating-systems"></a>Systèmes d'exploitation pris en charge
 Les versions de système d’exploitation Windows suivantes sont pris en charge avec cette version des pilotes Microsoft PHP :

|PHP pour la version du pilote SQL Server&#8594;<br />&#8595;Système d'exploitation|5.2<br />&nbsp;|4.3<br />&nbsp;|4.0<br />&nbsp;|3.2<br />&nbsp;|3.1<br />&nbsp;|3<br />&nbsp;|2<br />&nbsp;|
|---|---|---|---|---|---|---|---|
|Windows Server 2016                 |O  |O  |   |   |   |   |   |
|Windows Server 2012 R2              |O  |O  |O  |O  |O  |   |   |
|Windows Server 2012                 |O  |O  |O  |O  |O  |   |   |
|Windows Server 2008 R2 SP1          |   |   |O  |O  |O  |O  |   |
|Windows Server 2008 R2              |   |   |   |   |   |   |O  |
|Windows Server 2008 SP2             |   |   |O  |O  |O  |O  |   |
|Windows Server 2008                 |   |   |   |   |   |   |O  |
|Windows Server 2003 SP1             |   |   |   |   |   |   |O  |
|Windows 10                          |O  |O  |O  |   |   |   |   |
|Windows 8.1                         |O  |O  |O  |O  |O  |   |   |
|Windows 8                           |   |O  |O  |O  |O  |   |   |
|Windows 7 SP1                       |   |   |O  |O  |O  |O  |   |
|Windows Vista SP2                   |   |   |O  |O  |O  |O  |O  |
|Windows XP SP3                      |   |   |   |   |   |   |O  |

 Les Linux et Mac versions de système (64 bits uniquement) d’exploitation suivants sont pris en charge avec cette version des pilotes Microsoft PHP :

|PHP pour la version du pilote SQL Server&#8594;<br />&#8595;Système d'exploitation|5.2<br />&nbsp;|4.3<br />&nbsp;|4.0<br />&nbsp;|3.2<br />&nbsp;|3.1<br />&nbsp;|3<br />&nbsp;|2<br />&nbsp;|
|---|---|---|---|---|---|---|---|
|Ubuntu 17.10 (64 bits)               |O  |   |   |   |   |   |   |
|Ubuntu 16.04 (64 bits)               |O  |O  |O  |   |   |   |   |
|15.10 d’Ubuntu (64 bits)               |   |O  |   |   |   |   |   |
|Ubuntu 15.04 (64 bits)               |   |   |O  |   |   |   |   |
|Debian 9 (64 bits)                   |O  |   |   |   |   |   |   |
|Debian 8 (64 bits)                   |O  |O  |   |   |   |   |   |
|Red Hat Enterprise Linux 7 (64 bits) |O  |O  |O  |   |   |   |   |
|SuSE Enterprise Linux 12 (64 bits)   |O  |   |   |   |   |   |   |
|macOS Sierra (64 bits)               |O  |O  |   |   |   |   |   |
|macOS El Capitan (64 bits)           |O  |O  |   |   |   |   |   |

## <a name="see-also"></a>Voir aussi  
[Notes de publication](../../connect/php/release-notes-for-the-php-sql-driver.md)

[Ressources de support](../../connect/php/support-resources-for-the-php-sql-driver.md)

[Configuration système requise](../../connect/php/system-requirements-for-the-php-sql-driver.md)
