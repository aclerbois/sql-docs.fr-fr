---
title: Référence d’API du pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e4e1ae9d-18a6-41db-8bd2-9cf0eee4cccb
caps.latest.revision: 46
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2b9dc05cfa6e96fc3dd987095917a932883df4e2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="jdbc-driver-api-reference"></a>Référence de l'API du pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] fournit une API qui peut être utilisée dans Java code de programmation pour vous connecter à et d’interagir avec un [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données.  
  
> [!NOTE]  
>  Pour obtenir des informations conceptuelles sur l’utilisation du pilote JDBC, consultez [vue d’ensemble du pilote JDBC](../../../connect/jdbc/overview-of-the-jdbc-driver.md).  
  
> [!IMPORTANT]  
>  Pour JDBC 4.1 et 4.2 prennent en charge de la conformité, utilisez Microsoft JDBC Driver 4.2 (ou version ultérieure) pour SQL Server. Les versions antérieures 4.1 et 4.0 du pilote Microsoft JDBC ne prennent pas en charge les nouvelles méthodes introduites avec JDBC 4.1 ou 4.2.  
>   
>  Cette section ne comprend pas d'informations sur l'API pour la compatibilité avec JDBC 4.1. Consultez [JDBC 4.1 conformité pour le pilote JDBC](../../../connect/jdbc/jdbc-4-1-compliance-for-the-jdbc-driver.md).  
>   
>  Cette section ne comprend pas d’informations sur l’API pour la compatibilité avec JDBC 4.2. Consultez [JDBC 4.2 de conformité pour le pilote JDBC](../../../connect/jdbc/jdbc-4-2-compliance-for-the-jdbc-driver.md).  
>   
>  Détails de l’API de copie en bloc, disponible à partir de Microsoft JDBC Driver 4.2 pour SQL Server, ne figurent pas dans cette section. Consultez [à l’aide de la copie en bloc avec le pilote JDBC](../../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md).  
>   
>  Détails de l’API pour Always Encrypted, disponible à partir de Microsoft JDBC Driver 6.0 pour SQL Server, ne figurent pas dans cette section. Consultez [Always Encrypted référence des API pour le pilote JDBC](../../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)  
>   
>  Détails de l’API pour les paramètres Using Table-Valued, disponible à partir de Microsoft JDBC Driver 6.0 pour SQL Server, ne figurent pas dans cette section. Consultez [à l’aide des paramètres table](../../../connect/jdbc/using-table-valued-parameters.md)  
>   
>  Le pilote Microsoft JDBC 6.4 prend en charge la compilation avec JDK 7.0, 8.0 et 9.0.  
>   
>  Le pilote Microsoft JDBC 6.2 prend en charge la compilation avec JDK 7.0 et 8.0.  
>   
>  Pilotes Microsoft JDBC 6.0 et 4.2 prennent en charge la compilation avec JDK 5.0, 6.0, 7.0 et 8.0.  
>   
>  Le Pilote JDBC 4.1 Microsoft prend en charge la compilation avec JDK 5.0, 6.0 et 7.0.  

## <a name="interfaces"></a>Interfaces  
  
|Nom de l'interface| Description|  
|--------------------|-----------------|  
|[ISQLServerCallableStatement, interface](../../../connect/jdbc/reference/isqlservercallablestatement-interface.md)|Permet de spécifier le nom de la procédure stockée à appeler avec les paramètres d'entrée et de sortie.|  
|[ISQLServerConnection, interface](../../../connect/jdbc/reference/isqlserverconnection-interface.md)|Représente une connexion JDBC à une [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données.|  
|[SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)|Représente une liste des propriétés spécifiques à la connexion à un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données en utilisant un [ISQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[ISQLServerPreparedStatement](../../../connect/jdbc/reference/isqlserverpreparedstatement-interface.md)|Représente l'implémentation de base de la fonctionnalité d'instruction préparée JDBC.|  
|[ISQLServerResultSet](../../../connect/jdbc/reference/isqlserverresultset-interface.md)|Représente un jeu de résultats JDBC.|  
|[ISQLServerStatement](../../../connect/jdbc/reference/isqlserverstatement-interface.md)|Représente l'implémentation de base de la fonctionnalité d'instruction JDBC.|  
  
## <a name="classes"></a>Classes  
  
|Nom de la classe| Description|  
|----------------|-----------------|  
|[DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md)|Représente un objet de type microsoft.sql.DateTimeOffset.|  
|[SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)|Représente un objet blob.|  
|[SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)|Implémente ISQLServerCallableStatement.|  
|[SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)|Représente un CLOB (Character Large Binary Object).|  
|[SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)|Implémente ISQLServerConnectopn.|  
|[SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)|Représente des connexions de bases de données physiques pour les gestionnaires de regroupement de connexions.|  
|[SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)|Représente les métadonnées pour la base de données.|  
|[SQLServerDataSource](../../../connect/jdbc/reference/isqlserverdatasource-interface.md)|Représente une liste des propriétés spécifiques à la connexion à un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données en utilisant un [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) objet.|  
|[SQLServerDataSourceObjectFactory](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-class.md)|Représente une fabrique d'objet permettant de matérialiser des sources de données à partir de JNDI (Java Naming and Directory Interface).|  
|[SQLServerDriver](../../../connect/jdbc/reference/sqlserverdriver-class.md)|Représente le pilote JDBC. Cette classe inclut des méthodes pour la connexion à un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données et pour obtenir des informations sur le pilote JDBC.|  
|[SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)|Représente une exécution incorrecte ou incomplète d'une instruction SQL.|  
|[SQLServerNClob, classe](../../../connect/jdbc/reference/sqlservernclob-class.md)|Représente un CLOB utilisant le jeu de caractères nationaux.|  
|[SQLServerParameterMetaData](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md)|Représente les métadonnées pour les paramètres d'instructions préparées.|  
|[SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)|Représente une connexion de base de données physique dans un regroupement de connexions.|  
|[SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)|Implémente ISQLServerPreparedStatement.|  
|[SQLServerResource](../../../connect/jdbc/reference/sqlserverresource-class.md)|Représente une ressource de chaîne d'erreur localisée. Cette classe est réservée à l'usage interne uniquement.|  
|[SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)|Implémente ISQLServerResultSet.|  
|[SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)|Représente les métadonnées des colonnes contenues dans un jeu de résultats.|  
|[SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md)|Représente le point de contrôle auquel la transaction peut être restaurée.|  
|[SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)|Implémente ISQLServerStatement.|  
|[SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md)|Représente les connexions JDBC qui peuvent participer à des transactions distribuées (XA).|  
|[SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md)|Représente une fabrique pour [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md) objets qui est utilisée en interne.|  
|[SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md)|Représente une XAResource pour XA de gestion des transactions distribuées.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
