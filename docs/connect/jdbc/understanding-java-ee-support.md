---
title: Présentation de prise en charge de Java EE | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a9448b80-b7a3-49cf-8bb4-322c73676005
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fb626e0f956d057b27f8a469d51dea67df428742
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-java-ee-support"></a>Fonctionnement de la prise en charge Java EE
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Les sections suivantes expliquent comment le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit la prise en charge de la plate-forme Java, Enterprise Edition (Java EE) et fonctionnalités facultatives de l’API JDBC 3.0. Les exemples de code source fournis dans ce système d'aide constituent une bonne référence pour débuter avec ces fonctionnalités.  
  
 En premier lieu, assurez-vous que votre environnement Java (JDK, JRE) inclut le package javax.sql. Il s'agit d'un package requis pour toute application JDBC qui utilise l'API facultative. JDK 1.5 et versions ultérieures contiennent déjà ce package ; il est donc inutile de l'installer séparément.  
  
## <a name="driver-name"></a>Nom du pilote  
 Le nom de classe du pilote est **com.microsoft.sqlserver.jdbc.SQLServerDriver**. Pour les pilotes de JDBC 4.1, 4.2 et 6.0, le pilote est contenu dans le fichier sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar ou sqljdbc42.jar. Pour 6.2 du pilote JDBC, le pilote est contenu dans mssql-jdbc-6.2.1.jre7.jar ou mssql-jdbc-6.2.1.jre8.jar. Pour 6.4 du pilote JDBC, le pilote est contenu dans mssql-jdbc-6.4.0.jre7.jar, mssql-jdbc-6.4.0.jre8.jar ou mssql-jdbc-6.4.0.jre9.jar.
  
 Le nom de classe est utilisé chaque fois que vous chargez le pilote avec la classe DriverManager de JDBC. Il est également utilisé lorsque vous devez spécifier le nom de classe du pilote dans une configuration de pilote. Par exemple, la configuration d'une source de données dans un serveur d'applications Java EE peut exiger que vous entriez le nom de classe du pilote.  
  
## <a name="data-sources"></a>Sources de données  
 Le pilote JDBC assure la prise en charge des sources de données Java EE / JDBC 3.0. Le pilote JDBC [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md) classe est implémentée par **com.microsoft.sqlserver.jdbc.SQLServerXADataSource**.  
  
### <a name="datasource-names"></a>Noms de sources de données  
 Vous pouvez établir des connexions de base de données à l'aide de sources de données. Les sources de données disponibles avec le pilote JDBC sont décrites dans le tableau suivant :  
  
|Type DataSource|Description et nom de classe|  
|---------------|--------------------------|  
|DataSource|com.microsoft.sqlserver.jdbc.SQLServerDataSource <br/> <br/> Source de données sans regroupement.|  
|ConnectionPoolDataSource|com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource <br/> <br/> Source de données pour configurer des regroupements de connexions de serveur d'applications JAVA EE. Utilisée en général lorsque l'application s'exécute sur un serveur d'applications JAVA EE.|  
|XADataSource|com.microsoft.sqlserver.jdbc.SQLServerXADataSource <br/> <br/> Source de données pour configurer des sources de données JAVA EE XA. Utilisée en général lorsque l'application s'exécute sur un serveur d'applications JAVA EE et un gestionnaire de transactions XA.|  
  
### <a name="data-source-properties"></a>Propriétés de Source de données  
 Toutes les sources de données prennent en charge la capacité à définir et à obtenir toute propriété associée à l'ensemble des propriétés du pilote sous-jacent.  
  
 Exemples :  
  
 `setServerName("localhost");`  
  
 `setDatabaseName("AdventureWorks");`  
  
 L'exemple suivant montre comment une application se connecte en utilisant une source de données :  
  
```  
initialize JNDI ..  
Context ctx = new InitialContext(System.getProperties());  
...  
DataSource ds = (DataSource) ctx.lookup("MyDataSource");  
Connection c = ds.getConnection("user", "pwd");  
```  
  
 Pour plus d’informations sur les propriétés de source de données, consultez [définissant les propriétés de la Source de données](../../connect/jdbc/setting-the-data-source-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
