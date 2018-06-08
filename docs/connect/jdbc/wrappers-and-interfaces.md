---
title: Wrappers et Interfaces | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 27fc9b72-9f21-4728-abcb-5c015f28a6ab
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9a7316e5daa6fa27209a31a07ddf0ace84c191b0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="wrappers-and-interfaces"></a>Wrappers et interfaces
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge les interfaces qui vous permettent de créer un proxy d’une classe, ainsi que les wrappers qui vous permettent d’accéder aux extensions de l’API JDBC qui sont spécifiques à la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] via une interface proxy.  
  
## <a name="wrappers"></a>Wrappers  
 Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge l’interface java.sql.Wrapper. Cette interface fournit un mécanisme d’accès aux extensions de l’API JDBC qui sont spécifiques à la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] via une interface proxy.  
  
 L’interface java.sql.Wrapper définit deux méthodes : **isWrapperFor** et **unwrap**. Le **isWrapperFor** méthode vérifie si l’objet d’entrée spécifié implémente cette interface. Le **unwrap** méthode retourne un objet qui implémente cette interface pour permettre l’accès à la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] des méthodes spécifiques.  
  
 **isWrapperFor** et **unwrap** méthodes sont exposées comme suit :  
  
-   [Méthode isWrapperFor &#40;SQLServerCallableStatement&#41;](../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)  
  
-   [Méthode Unwrap &#40;SQLServerCallableStatement&#41;](../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)  
  
-   [Méthode isWrapperFor &#40;SQLServerConnectionPoolDataSource&#41;](../../connect/jdbc/reference/iswrapperfor-method-sqlserverconnectionpooldatasource.md)  
  
-   [Méthode Unwrap &#40;SQLServerConnectionPoolDataSource&#41;](../../connect/jdbc/reference/unwrap-method-sqlserverconnectionpooldatasource.md)  
  
-   [Méthode isWrapperFor &#40;SQLServerDataSource&#41;](../../connect/jdbc/reference/iswrapperfor-method-sqlserverdatasource.md)  
  
-   [Méthode Unwrap &#40;SQLServerDataSource&#41;](../../connect/jdbc/reference/unwrap-method-sqlserverdatasource.md)  
  
-   [Méthode isWrapperFor &#40;SQLServerPreparedStatement&#41;](../../connect/jdbc/reference/iswrapperfor-method-sqlserverpreparedstatement.md)  
  
-   [Méthode Unwrap &#40;SQLServerPreparedStatement&#41;](../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)  
  
-   [Méthode isWrapperFor &#40;SQLServerStatement&#41;](../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md)  
  
-   [Méthode Unwrap &#40;SQLServerStatement&#41;](../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md)  
  
-   [Méthode isWrapperFor &#40;SQLServerXADataSource&#41;](../../connect/jdbc/reference/iswrapperfor-method-sqlserverxadatasource.md)  
  
-   [Méthode Unwrap &#40;SQLServerXADataSource&#41;](../../connect/jdbc/reference/unwrap-method-sqlserverxadatasource.md)  
  
## <a name="interfaces"></a>Interfaces  
 À compter de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] version 3.0 du pilote JDBC, les interfaces sont disponibles pour un serveur d’applications accéder à une méthode spécifique du pilote à partir de la classe associée. Le serveur d’applications peut encapsuler la classe en créant un proxy, exposant le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-des fonctionnalités spécifiques à partir d’une interface. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge les interfaces qui ont le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] des méthodes spécifiques et des constantes pour un serveur d’applications peut créer un proxy de la classe.  
  
 Les interfaces sont dérivées d’interfaces Java afin de pouvoir utiliser le même objet une fois qu’il est désencapsulé pour accéder aux fonctionnalités du pilote spécifique ou générique standard [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fonctionnalité.  
  
 Les interfaces suivantes ont été ajoutées :  
  
-   [ISQLServerCallableStatement](../../connect/jdbc/reference/isqlservercallablestatement-interface.md)  
  
-   [ISQLServerConnection](../../connect/jdbc/reference/isqlserverconnection-interface.md)  
  
-   [ISQLServerDataSource](../../connect/jdbc/reference/isqlserverdatasource-interface.md)  
  
-   [ISQLServerPreparedStatement](../../connect/jdbc/reference/isqlserverpreparedstatement-interface.md)  
  
-   [ISQLServerResultSet](../../connect/jdbc/reference/isqlserverresultset-interface.md)  
  
-   [ISQLServerStatement](../../connect/jdbc/reference/isqlserverstatement-interface.md)  
  
## <a name="example"></a>Exemple  
  
### <a name="description"></a> Description  
 Cet exemple montre comment accéder à un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-fonction spécifique à partir d’un objet de source de données. Cette classe de source de données peut avoir été encapsulée par un serveur d’applications. Pour accéder à la fonction de spécifiques au pilote JDBC ou une constante, vous pouvez désencapsuler la source de données à une interface ISQLServerDataSource et utilisez les fonctions déclarées dans cette interface.  
  
### <a name="code"></a>Code  
  
```  
import javax.sql.*;  
import java.sql.*;  
import com.microsoft.sqlserver.jdbc.*;  
  
public class UnWrapTest {  
   public static void main(String[] args) {  
      // This is a test.  This DataSource object could be something from an appserver   
      // which has wrapped the real SQLServerDataSource with its own wrapper  
      SQLServerDataSource ds = new SQLServerDataSource();  
      checkSendStringParametersAsUnicode(ds);  
   }  
  
   // Unwrap to the ISQLServerDataSource interface to access the getSendStringParametersAsUnicode function  
   static void checkSendStringParametersAsUnicode(DataSource ds) {  
      try {  
         final ISQLServerDataSource sqlServerDataSource = ds.unwrap(ISQLServerDataSource.class);  
         boolean sendStringParametersAsUnicode = sqlServerDataSource.getSendStringParametersAsUnicode();  
  
         System.out.println("Send string as parameter value is:-" + sendStringParametersAsUnicode);  
  
      } catch (SQLException sqlE) {  
         System.out.println("Exception:-" + sqlE);  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
  
  
