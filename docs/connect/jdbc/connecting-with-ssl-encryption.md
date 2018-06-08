---
title: La connexion avec le chiffrement SSL | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ec91fa8a-ab7e-4c1e-a05a-d7951ddf33b1
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 49c6aaf771bb5335b8ba649869a4a8cf13894e82
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-with-ssl-encryption"></a>Connexion avec le chiffrement SSL
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Les exemples de cette rubrique décrivent comment utiliser les propriétés de chaîne de connexion qui permettent aux applications d'utiliser le chiffrement SSL (Secure Sockets Layer) dans une application Java. Pour plus d’informations sur ces connexions nouvelle chaîne propriétés tel que **chiffrer**, **trustServerCertificate**, **trustStore**,  **trustStorePassword**, et **hostNameInCertificate**, consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
 Lorsque le **chiffrer** est définie sur **true** et **trustServerCertificate** est définie sur **true**, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ne doit pas valider la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] certificat SSL. Cela est généralement requis pour autoriser les connexions dans les environnements de test, notamment l’emplacement du [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance possède seulement un certificat auto-signé.  
  
 L’exemple de code suivant montre comment définir la **trustServerCertificate** propriété dans une chaîne de connexion :  
  
```  
String connectionUrl =   
    "jdbc:sqlserver://localhost:1433;" +  
     "databaseName=AdventureWorks;integratedSecurity=true;" +  
     "encrypt=true;trustServerCertificate=true";  
```  
  
 Lorsque le **chiffrer** est définie sur **true** et **trustServerCertificate** est définie sur **false**, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] permet de valider le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] certificat SSL. La validation du certificat de serveur est une partie de la négociation SSL qui garantit qu'il s'agit du serveur correct avec lequel établir une connexion. Pour valider le certificat de serveur, les informations d’approbation doivent être fournies au moment de la connexion à l’aide **trustStore** et **trustStorePassword** propriétés de connexion explicitement, ou à l’aide de approbations par défaut de la sous-jacent Machine virtuelle Java (JVM) du magasin de manière implicite.  
  
 Le **trustStore** propriété spécifie le chemin d’accès (y compris le nom de fichier) au fichier trustStore de certificat, qui contient la liste de certificats approuvée par le client. Le **trustStorePassword** propriété spécifie le mot de passe utilisé pour vérifier l’intégrité des données trustStore. Pour plus d’informations sur l’utilisation du magasin d’approbations par défaut de la machine virtuelle Java, consultez le [configuration du Client pour le chiffrement SSL](../../connect/jdbc/configuring-the-client-for-ssl-encryption.md).  
  
 L’exemple de code suivant montre comment définir la **trustStore** et **trustStorePassword** propriétés dans une chaîne de connexion :  
  
```  
String connectionUrl =   
    "jdbc:sqlserver://localhost:1433;" +  
     "databaseName=AdventureWorks;integratedSecurity=true;" +  
     "encrypt=true; trustServerCertificate=false;" +  
     "trustStore=storeName;trustStorePassword=storePassword";  
```  
  
 Le pilote JDBC fournit une propriété supplémentaire, **hostNameInCertificate**, qui spécifie le nom d’hôte du serveur. La valeur de cette propriété doit correspondre à la propriété de sujet du certificat.  
  
 L’exemple de code suivant montre comment utiliser le **hostNameInCertificate** propriété dans une chaîne de connexion :  
  
```  
String connectionUrl =   
    "jdbc:sqlserver://localhost:1433;" +  
     "databaseName=AdventureWorks;integratedSecurity=true;" +  
     "encrypt=true; trustServerCertificate=false;" +  
     "trustStore=storeName;trustStorePassword=storePassword" +  
     "hostNameInCertificate=hostName";  
```  
  
> [!NOTE]  
>  Vous pouvez également définir la valeur des propriétés de connexion en respectant **setter** méthodes fournies par le [SQLServerDataSource](../../connect/jdbc/reference/sqlserverdatasource-class.md) classe.  
  
 Si le **chiffrer** est définie sur **true** et **trustServerCertificate** est définie sur **false** et si le nom du serveur dans le chaîne de connexion ne correspond pas au nom du serveur dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] certificat SSL, le message d’erreur suivant s’affichera : le pilote n’a pas pu établir une connexion sécurisée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] en utilisant le chiffrement de la couche de Sockets sécurisée (SSL). Erreur : « java.security.cert.CertificateException : Échec de la validation du nom de serveur dans un certificat lors de l'initialisation SSL (Secure Sockets Layer). »  
  
## <a name="see-also"></a>Voir aussi  
 [À l’aide du chiffrement SSL](../../connect/jdbc/using-ssl-encryption.md)   
 [Sécurisation des applications de pilote JDBC](../../connect/jdbc/securing-jdbc-driver-applications.md)  
  
  
