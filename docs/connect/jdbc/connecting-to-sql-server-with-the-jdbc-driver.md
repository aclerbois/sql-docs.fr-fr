---
title: Connexion à SQL Server avec le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 94bcfbe3-f00e-4774-bda8-bb7577518fec
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6af836edb1585a07d54fb0742b73ac10a4852d8a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-sql-server-with-the-jdbc-driver"></a>Connexion à SQL Server à l'aide du pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Une des principales choses à faire avec le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] consiste à établir une connexion à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données. Toutes les interactions avec la base de données s’effectue via le [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) objet, et parce que le pilote JDBC présente une architecture plate, pratiquement tout comportement intéressant touche l’objet SQLServerConnection.  
  
 Si un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est uniquement à l’écoute sur le port IPv6, définissez la propriété système java.net.preferIPv6Addresses pour vous assurer que IPv6 est utilisé au lieu de IPv4 pour se connecter à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]:  
  
```  
System.setProperty("java.net.preferIPv6Addresses", "true");  
```  
  
 Les rubriques de cette section décrivent comment créer et utiliser une connexion à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Création de l’URL de connexion](../../connect/jdbc/building-the-connection-url.md)|Décrit comment créer une URL pour la connexion à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données. Décrit également la connexion à des instances nommées d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.|  
|[Définition des propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md)|Décrit les différentes propriétés de connexion et comment ils peuvent être utilisés lorsque vous vous connectez à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.|  
|[Définition des propriétés de la source de données](../../connect/jdbc/setting-the-data-source-properties.md)|Explique comment utiliser les sources de données dans un environnement Java EE (Java Platform, Enterprise Edition).|  
|[Utilisation d’une connexion](../../connect/jdbc/working-with-a-connection.md)|Décrit les différentes méthodes permettant de créer une instance d’une connexion à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.|  
|[Utilisation d’un regroupement de connexions](../../connect/jdbc/using-connection-pooling.md)|Décrit la manière dont le pilote JDBC prend en charge l'utilisation d'un regroupement de connexions.|  
|[À l’aide de la mise en miroir de base de données &#40;JDBC&#41;](../../connect/jdbc/using-database-mirroring-jdbc.md)|Décrit la manière dont le pilote JDBC prend en charge l'utilisation d'une mise en miroir de bases de données.|  
|[Prise en charge de la haute disponibilité et de la récupération d’urgence par le pilote JDBC](../../connect/jdbc/jdbc-driver-support-for-high-availability-disaster-recovery.md)|Explique comment développer une application qui se connecte à un groupe de disponibilité AlwaysOn.|  
|[Utilisation de l’authentification intégrée Kerberos pour se connecter à SQL Server](../../connect/jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server.md)|Aborde l’implémentation Java pour les applications pour se connecter à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] de base de données à l’aide de l’authentification intégrée Kerberos.|  
|[Connexion à une base de données Azure SQL Database](../../connect/jdbc/connecting-to-an-azure-sql-database.md)|Aborde les problèmes de connectivité des bases de données installées sur SQL Azure.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
