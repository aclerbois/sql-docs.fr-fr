---
title: Notes de publication pour les pilotes Microsoft pour PHP pour SQL Server | Documents Microsoft
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
helpviewer_keywords:
- what's new in version 1.1
ms.assetid: 91cca3d2-ba99-4a6d-b0de-beb9699cb3f8
caps.latest.revision: 37
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e73a70fbd5d02a829f8f69770a79155c94a4316d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="release-notes-for-the-microsoft-drivers-for-php-for-sql-server"></a>Notes de publication pour les pilotes Microsoft SQL Server pour PHP
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette page explique ce qui a été ajouté dans chaque version de la [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  

## <a name="whats-new-in-version-52"></a>Quelles sont les nouveautés dans la Version 5.2

- Prise en charge de PHP 7.2.1). et à distance sur Windows et 7.2.0 et sur d’autres plateformes
- Prise en charge pour Microsoft ODBC Driver 17
  - Version du 17 est désormais la valeur par défaut sur toutes les plateformes
- Prise en charge pour Suse Enterprise Linux 12, 9 Debian et Ubuntu 17.10
- Prise en charge supprimée pour 15.10 d’Ubuntu
- Prise en charge Always Encrypted avec fonctionnalités CRUD sur Windows. Pour plus d’informations, consultez [utilisation du chiffrement intégral avec les pilotes PHP pour SQL Server](../../connect/php/using-always-encrypted-php-drivers.md)
  - Prise en charge pour le magasin de certificats Windows
  - Always Encrypted est uniquement pris en charge avec Microsoft ODBC Driver 17 ci-dessus
- Prise en charge pour les paramètres régionaux non UTF8 sur Linux et Mac OS
  - Paramètres régionaux de non-UTF8 sur Linux et macOS uniquement sont pris en charge que dans Microsoft ODBC Driver 17 et versions ultérieures
- Prise en charge de l’entrepôt de données SQL Azure
- Prise en charge pour l’Instance gérée de SQL Azure (version préliminaire privée étendue)


## <a name="whats-new-in-version-43"></a>Nouveautés de la Version 4.3

- Prise en charge de PHP 7.1
- Prise en charge de macOS Sierra et macOS El Capitan
- Prise en charge d’Ubuntu 15.10 et Debian 8
- Prise en charge supprimée pour Ubuntu 15.04
- Prise en charge pour les groupes de disponibilité Always On via la résolution IP réseau Transparent. Pour plus d’informations, consultez [Connection Options](../../connect/php/connection-options.md).
- Prise en charge supplémentaire pour le type de données sql_variant limitation.
- Prise en charge de résilience des connexions inactive dans Windows. Pour plus d’informations, consultez [Connection Options](../../connect/php/connection-options.md).
- Connexion mise en pool de prise en charge de Linux et macOS. Pour plus d’informations, consultez [le regroupement de connexions](../../connect/php/connection-pooling-microsoft-drivers-for-php-for-sql-server.md).
- Prise en charge pour l’authentification Azure Active Directory avec ActiveDirectoryPassword et SqlPassword. Pour plus d’informations, consultez [Connection Options](../../connect/php/connection-options.md).

## <a name="whats-new-in-version-40"></a>Quelles sont les nouveautés dans la Version 4.0

- Prise en charge de PHP 7.0  
- Prise en charge 64 bits
- Prise en charge d’Ubuntu 15.04 et Ubuntu 16.04 RedHat 7

## <a name="whats-new-in-version-32"></a>Nouveautés de la version 3.2

- Prise en charge de PHP 5.6   
- Inclut les dernières mises à jour pour les versions antérieures PHP 5.5 et 5.4   
- Nécessite Microsoft ODBC Driver 11 pour SQL Server  

## <a name="whats-new-in-version-31"></a>Nouveautés de la version 3.1

- Prise en charge de PHP 5.5  
- Nécessite Microsoft ODBC Driver 11 pour SQL Server. Les versions précédentes nécessitaient SQL Native Client.  

## <a name="whats-new-in-version-30"></a>Nouveautés de la version 3.0  

- Prise en charge de PHP 5.4.  PHP 5.2 n’est pas pris en charge dans la version 3 du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
- Ajout de l’option de connexion AttachDBFileName. Pour plus d’informations, consultez [Connection Options](../../connect/php/connection-options.md).  
- Prise en charge de la base de données locale, qui a été ajoutée dans [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)]. Pour plus d’informations, consultez [prise en charge de la base de données locale](../../connect/php/php-driver-for-sql-server-support-for-localdb.md).
- Ajout de l’option de connexion AttachDBFileName. Pour plus d’informations, consultez [Connection Options](../../connect/php/connection-options.md).  
- Prise en charge des fonctionnalités de récupération d’urgence/haute disponibilité. Pour plus d’informations, consultez [prise en charge pour la haute disponibilité, la récupération d’urgence](../../connect/php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).
- Prise en charge des curseurs côté client (mise en cache d’un jeu de résultats en mémoire). Pour plus d’informations, consultez [Types de curseurs &#40;pilote SQLSRV&#41; ](../../connect/php/cursor-types-sqlsrv-driver.md) et [Types de curseurs &#40;pilote PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).
- Ajout de l’attribut PDO::ATTR_EMULATE_PREPARES. Pour plus d’informations, consultez [PDO::prepare](../../connect/php/pdo-prepare.md).  

## <a name="whats-new-in-version-20"></a>Nouveautés de la version 2.0  
Dans la version 2.0, la prise en charge du pilote PDO_SQLSRV a été ajoutée. Pour plus d’informations, consultez [Référence de pilote PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md).  

## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble des pilotes Microsoft SQL Server pour PHP](../../connect/php/overview-of-the-php-sql-driver.md)
