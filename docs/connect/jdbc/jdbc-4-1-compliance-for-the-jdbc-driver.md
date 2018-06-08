---
title: JDBC 4.1 conformité pour le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f087fd40-8451-478e-b465-43112c711515
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1a24d3ee01bacaecba12b3b1988da316387f0da6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="jdbc-41-compliance-for-the-jdbc-driver"></a>Compatibilité avec JDBC 4.1 pour le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

    
> [!NOTE]  
>  Les versions antérieures de Microsoft JDBC Driver 4.2 pour SQL Server sont compatibles avec les spécifications de l'API Java Database Connectivity 4.0. Cette section ne s'applique pas aux versions antérieures à la version 4.2.  
  
 La spécification de l'API Java Database Connectivity 4.1 est prise en charge par Microsoft JDBC Driver 4.2 pour SQL Server, avec les méthodes API suivantes.  
  
 **Classe SQLServerConnection**  
  
|Nouvelle méthode| Description|Implémentation du pilote JDBC|  
|----------------|-----------------|--------------------------------|  
|void abort(Executor executor)|Met fin à une connexion ouverte à SQL Server.|Implémenté comme décrit dans l'interface java.sql.Connection. Pour plus d’informations, consultez [java.sql.Connection](http://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|  
|void setSchema(String schema)|Définit le schéma de la connexion actuelle.|SQL Server ne prend pas en charge la définition du schéma pour la session actuelle. Le pilote enregistre en mode silencieux un message d'avertissement si cette méthode est appelée. Pour plus d’informations, consultez [java.sql.Connection](http://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|  
|String getSchema()|Retourne le nom du schéma pour la connexion actuelle.|SQL Server ne prenant pas en charge la définition du schéma pour la connexion actuelle, le pilote retourne à la place le schéma par défaut de l'utilisateur. Pour plus d’informations, consultez [java.sql.Connection](http://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|  
  
 **Classe SQLServerDatabaseMetaData**  
  
|Nouvelle méthode| Description|Implémentation du pilote JDBC|  
|----------------|-----------------|--------------------------------|  
|boolean generatedKeyAlwaysReturned()|Retourne la valeur true, car le pilote prend en charge la récupération des clés générées|Implémenté comme décrit dans l'interface java.sql. DatabaseMetaData. Pour plus d’informations, consultez [java.sql.DatabaseMetaData](http://docs.oracle.com/javase/7/docs/api/java/sql/DatabaseMetaData.html).|  
|ResultSet getPseudoColumns(String catalog, String schemaPattern,String tableNamePattern,String columnNamePattern)|Récupère une description des pseudo colonnes/colonnes masquées|Retourne un jeu de résultats vide, car SQL Server n'a pas de notion formelle des pseudo colonnes. Pour plus d’informations, consultez [java.sql.DatabaseMetaData](http://docs.oracle.com/javase/7/docs/api/java/sql/DatabaseMetaData.html).|  
  
 **Classe SQLServerStatement**  
  
|Nouvelle méthode| Description|Implémentation du pilote JDBC|  
|----------------|-----------------|--------------------------------|  
|void closeOnCompletion()|Spécifie que cette instruction est fermée quand tous ses jeux de résultats dépendants sont fermés.|Implémenté comme décrit dans l'interface java.sql.Statement. Pour plus d’informations, consultez [java.sql.Statement](http://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html).|  
|boolean isCloseOnCompletion()|Retourne une valeur indiquant si cette instruction est fermée quand tous ses jeux de résultats dépendants sont fermés.|Implémenté comme décrit dans l'interface java.sql.Statement. Pour plus d’informations, consultez [java.sql.Statement](http://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html).|  
  
 La spécification de l'API Java Database Connectivity 4.1 est prise en charge par Microsoft JDBC Driver 4.2 pour SQL Server, avec les fonctionnalités suivantes.  
  
|Nouvelle fonctionnalité| Description|  
|-----------------|-----------------|  
|Nouvelle fonction d'échappement<br /><br /> Échappement limité des lignes retournées|Partiellement pris en charge<br /><br /> Syntaxe d’échappement : limite \<lignes > [décalage < row_offset >](using-sql-escape-sequences.md).|  
  
 La spécification de l'API Java Database Connectivity 4.1 est prise en charge par Microsoft JDBC Driver 4.2 pour SQL Server, avec les mappages de type de données suivants.  
  
|Mappages de type de données| Description|  
|------------------------|-----------------|  
|Les nouveaux mappages de type de données sont désormais pris en charge dans les méthodes PreparedStatement.setObject() et PreparedStatement.setNull().|1. Nouveau mappage de type Java à JDBC<br /><br /> (a) java.math.BigInteger à JDBC BIGINT<br /><br /> (b) java.util.Date et java.util.Calendar à JDBC TIMESTAMP<br /><br /> 2. Nouvelles conversions de types de données :<br /><br /> (a) java.math.BigInteger en CHAR, VARCHAR, LONGVARCHAR et BIGINT<br /><br /> (b) java.util.Date et java.util.Calendar en CHAR, VARCHAR, LONGVARCHAR, DATE, TIME et TIMESTAMP<br /><br /> Pour plus d'informations, consultez la spécification JDBC 4.1.|  
  
  
