---
title: Fonctionnalité des dépendances du pilote JDBC de Microsoft pour SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 02/28/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 939a8773-2583-49a4-bf00-6b892fbe39dc
caps.latest.revision: 57
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 052628258ae1d8c1f31430ea132ed594a976be98
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="feature-dependencies-of-microsoft-jdbc-driver-for-sql-server"></a>Dépendances de fonctionnalité de Microsoft JDBC Driver pour SQL Server
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

 Cette page contient la liste des bibliothèques qui dépend de Microsoft JDBC Driver for SQL Server. Le projet comporte les dépendances suivantes.
 
 ## <a name="compile-time"></a>Moment de la compilation
 - `azure-keyvault` : Fournisseur de coffre de clés azure pour la fonctionnalité toujours chiffré Azure Key Vault (facultative)
 - `adal4j` : Bibliothèque d’Active Directory azure pour Java pour les fonctionnalités d’authentification Azure Active Directory et Azure Key Vault (facultatif)

 ##  <a name="test-time"></a>Durée du test
Les projets spécifiques qui nécessitent une des deux fonctionnalités ci-dessus doivent déclarer explicitement les dépendances respectives dans leur fichier pom :

***Par exemple :*** si vous utilisez *fonctionnalité d’authentification Azure Active Directory*, vous devez déclarer à *adal4j* dépendance dans le fichier pom du projet. Consultez l’extrait de code suivant : 
```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.4.0.jre8</version>
    <scope>compile</scope>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.4.0</version>
</dependency>
```

***Par exemple :*** si vous utilisez *fonctionnalité d’Azure Key Vault* vous avez besoin redéclarer *azure-keyvault* dépendance et *adal4j* dépendance dans votre fichier de projet pom. Consultez l’extrait de code suivant : 
```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.4.0.jre8</version>
    <scope>compile</scope>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.4.0</version>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```
 
 ## <a name="dependency-requirements-for-the-jdbc-driver"></a>Configuration requise de dépendance pour le pilote JDBC

 ### <a name="azure-keyvault-feature"></a>Fonctionnalité Keyvault Azure :
- Pilote JDBC version 6.0.0 
    - Versions de la dépendance : Azure-Keyvault (version 0.9.7), Adal4j (version version 1.3.0) et leurs dépendances ( [exemple d’application](../../connect/jdbc/azure-key-vault-sample-version-6.0.0.md))
- Le pilote JDBC version 6.2.2 et versions ultérieures (y compris la dernière 6.4.0)
    - Versions de la dépendance : Azure-Keyvault (version 1.0.0), Adal4j (version1.4.0) et leurs dépendances ([exemple d’Application](../../connect/jdbc/azure-key-vault-sample-version-6.2.2.md))

> [!NOTE]
>   À compter de v6.2.2, la dépendance d’azure-keyvault-java est mis à jour vers la version 1.0.0. Toutefois, la nouvelle version n’est pas compatible avec la version précédente (version 0.9.7) et par conséquent s’arrête l’implémentation existante dans le pilote. La nouvelle implémentation dans le pilote requiert des modifications de l’API, qui à son tour arrêter les programmes clients qui utilisent la fonctionnalité Azure Key Vault.

  
 ### <a name="azure-active-directory-authentication"></a>Authentification Azure Active Directory :
- Pilote JDBC version 6.0.0 
    - Versions de la dépendance : Adal4j (version version 1.3.0) et ses dépendances
        - Dans cette version du pilote, vous pouvez vous connecter à l’aide de *ActiveDirectoryIntegrated* uniquement sur une fenêtre de système d’exploitation et à l’aide de sqljdbc_auth.dll et la bibliothèque d’authentification Active Directory pour SQL Server (Mode d’authentification ADALSQL. (DLL). 
- Pilote JDBC version 6.4.0
    - Versions de la dépendance : Adal4j (version1.4.0) et ses dépendances
        - Dans cette version du pilote, votre application ne requiert pas l’utilisation de ADALSQL. DLL. Selon les systèmes d’exploitation. Pour **les systèmes d’exploitation Non Windows**, le pilote requiert le ticket Kerberos pour utiliser l’authentification de ActiveDirectoryIntegrated. Consultez [ticket Kerberos de définie sur Windows, Linux et Mac](https://docs.microsoft.com/sql/connect/jdbc/connecting-using-azure-active-directory-authentication#set-kerberos-ticket-on-windows-linux-and-mac) pour plus d’informations. Pour **systèmes d’exploitation Windows**, pilote par défaut vérifie si sqljdbc_auth.dll est chargé et ne nécessite pas de dépendance le programme d’installation ou adal4j du ticket Kerberos. Toutefois, si sqljdbc_auth.dll n’est pas chargé, pilote se comporte de la même façon que les systèmes d’exploitation non Windows, nécessite le programme d’installation, ce qui est décrit dans l’exemple suivant : un exemple d’application à l’aide de cette fonctionnalité, vous pouvez trouver [ici](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md) .

 ## <a name="see-also"></a>Voir aussi  
 [Référentiel GitHub de pilote JDBC](https://github.com/microsoft/mssql-jdbc)  
 [Référence d’API du pilote JDBC](../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  