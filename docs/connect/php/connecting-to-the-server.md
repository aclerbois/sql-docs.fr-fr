---
title: Connexion au serveur | Documents Microsoft
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
ms.assetid: c251a239-e0bd-4f45-9207-b76651072dd0
caps.latest.revision: 44
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f21d84708f4d1fdb05fe422a5e176187118dae04
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-the-server"></a>Connexion au serveur
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Les rubriques de cette section décrivent les options et les procédures de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] avec [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  

[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] peut se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] à l’aide de l’authentification Windows ou à l’aide de l’authentification SQL Server. Par défaut, [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] essaie de se connecter au serveur à l’aide de l’authentification Windows.  

## <a name="in-this-section"></a>Dans cette section  

|Rubrique| Description|  
|---------|---------------|  
|[Procédure : se connecter avec l’authentification Windows](../../connect/php/how-to-connect-using-windows-authentication.md)|Décrit comment établir une connexion à l’aide de l’authentification Windows.|  
|[Procédure : se connecter à l’aide de l’authentification SQL Server](../../connect/php/how-to-connect-using-sql-server-authentication.md)|Décrit comment établir une connexion à l’aide de l’authentification SQL Server.|  
|[Guide pratique pour se connecter à l’aide de l’authentification Azure Active Directory](../../connect/php/azure-active-directory.md)|Décrit comment définir le mode d’authentification et de se connecter à l’aide d’identités Azure Active Directory.|  
|[Guide pratique pour se connecter sur un port spécifié](../../connect/php/how-to-connect-on-a-specified-port.md)|Décrit comment se connecter au serveur sur un port spécifique.|  
|[Regroupement de connexions](../../connect/php/connection-pooling-microsoft-drivers-for-php-for-sql-server.md)|Fournit des informations sur le regroupement de connexions dans le pilote.|  
|[Guide pratique pour désactiver MARS (Multiple Active Result Set)](../../connect/php/how-to-disable-multiple-active-resultsets-mars.md)|Décrit comment désactiver la fonctionnalité MARS pendant une connexion.|  
|[Options de connexion](../../connect/php/connection-options.md)|Répertorie les options autorisées dans le tableau associatif qui contient les attributs de connexion.|  
|[Prise en charge de la base de données locale](../../connect/php/php-driver-for-sql-server-support-for-localdb.md)|Décrit la prise en charge de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] pour la fonctionnalité LocalDB, ajoutée dans [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)].|  
|[Prise en charge des fonctionnalités de récupération d’urgence/haute disponibilité](../../connect/php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)|Explique comment votre application peut être configurée pour tirer parti de la récupération d’urgence haute disponibilité, des fonctionnalités ajoutées dans [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)].|  
|[Connexion à Microsoft Azure SQL Database](../../connect/php/connecting-to-microsoft-azure-sql-database.md)|Explique comment se connecter à une base de données SQL Azure.|  
|[Résilience des connexions](../../connect/php/connection-resiliency.md)|Décrit la fonctionnalité de résilience de connexion qui rétablit les connexions interrompues.|  

## <a name="see-also"></a>Voir aussi  
[Guide de programmation pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)

[Exemple d’application &#40;pilote SQLSRV&#41;](../../connect/php/example-application-sqlsrv-driver.md)  
