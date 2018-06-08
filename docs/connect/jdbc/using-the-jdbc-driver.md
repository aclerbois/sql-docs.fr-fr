---
title: L’utilisation du pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 6faaf05b-8b70-4ed2-9b44-eee5897f1cd0
caps.latest.revision: 54
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 03ea3e895fb7d70b392e0c25372536770308049d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-the-jdbc-driver"></a>Utilisation du pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cette section fournit des instructions de démarrage rapide pour établir une connexion simple à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données à l’aide de la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. Avant de vous connectez à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] doit tout d’abord être installé sur votre ordinateur local ou sur un serveur, et le pilote JDBC doit être installé sur votre ordinateur local.  
  
## <a name="choosing-the-right-jar-file"></a>Choix du fichier JAR approprié  
 Le 6.4 de pilote JDBC Microsoft pour SQL Server fournit **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar**, et **mssql-jdbc-6.4.0.jre9.jar** bibliothèque de classes fichiers à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés.  
 
 Le 6.2 de pilote JDBC Microsoft pour SQL Server fournit **mssql-jdbc-6.2.1.jre7.jar**, et **mssql-jdbc-6.2.1.jre8.jar** classe les fichiers de bibliothèque à utiliser en fonction de votre environnement d’exécution Java par défaut Paramètres d’environnement (JRE).  
  
  Les pilotes Microsoft JDBC 6.0 et 4.2 pour SQL Server fournissent **sqljdbc41.jar**, et **sqljdbc42.jar** classe les fichiers de bibliothèque à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés.  
  
 Le Microsoft JDBC Driver 4.1 pour SQL Server fournit le **sqljdbc41.jar** fichier de bibliothèque de classes à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés.  
    
 Votre choix détermine également les fonctionnalités disponibles. Pour plus d’informations sur le fichier JAR à choisir, consultez [configuration système requise pour le pilote JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="setting-the-classpath"></a>Définition de Classpath  
 Le pilote JDBC ne fait pas partie du Kit de développement logiciel (SDK) Java. Si vous souhaitez l’utiliser, vous devez définir l’instruction classpath pour inclure la **sqljdbc.jar** fichier **sqljdbc4.jar** fichier, le **sqljdbc41.jar** fichier, ou le  **sqljdbc42.jar** fichier. Si l’instruction classpath pour inclure la valeur à l’aide de JDBC Driver 6.2, le **mssql-jdbc-6.2.1.jre7.jar** ou **mssql-jdbc-6.2.1.jre8.jar**. Si l’instruction classpath pour inclure la valeur à l’aide de JDBC Driver 6.4, le **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** ou **mssql-jdbc-6.4.0.jre9.jar**. S’il manque une entrée dans l’instruction classpath, votre application lève l’exception courante « Classe introuvable ».  
  
### <a name="for-microsoft-jdbc-driver-64"></a>Pour Microsoft JDBC Driver 6.4
 Le **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** ou **mssql-jdbc-6.4.0.jre9.jar** fichiers sont installés dans l’emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \mssql-jdbc-6.4.0.jre7.jar 
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \mssql-jdbc-6.4.0.jre8.jar
 
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \mssql-jdbc-6.4.0.jre9.jar
  
 Voici un exemple de l'instruction CLASSPATH utilisée pour une application Windows :  
  
 `CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.4 for SQL Server\sqljdbc_6.4\enu\mssql-jdbc-6.4.0.jre9.jar`  
  
 Voici un exemple de l'instruction CLASSPATH utilisée pour une application Unix/Linux :  
  
 `CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.4/enu/mssql-jdbc-6.4.0.jre9.jar`  
  
 Vous devez vous assurer que l’instruction CLASSPATH contient un seul [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], tels que mssql-jdbc-6.4.0.jre7.jar, mssql-jdbc-6.4.0.jre8.jar ou mssql-jdbc-6.4.0.jre9.jar.   

### <a name="for-microsoft-jdbc-driver-62"></a>Pour Microsoft JDBC Driver 6.2
 Le **mssql-jdbc-6.2.1.jre7.jar** ou **mssql-jdbc-6.2.1.jre8.jar** fichiers sont installés dans l’emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \mssql-jdbc-6.2.1.jre7.jar 
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \mssql-jdbc-6.2.1.jre8.jar
  
 Voici un exemple de l'instruction CLASSPATH utilisée pour une application Windows :  
  
 `CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.2 for SQL Server\sqljdbc_6.2\enu\mssql-jdbc-6.2.1.jre8.jar`  
  
 Voici un exemple de l'instruction CLASSPATH utilisée pour une application Unix/Linux :  
  
 `CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.2/enu/mssql-jdbc-6.2.1.jre8.jar`  
  
 Vous devez vous assurer que l’instruction CLASSPATH contient un seul [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], tel que mssql-jdbc-6.2.1.jre7.jar ou mssql-jdbc-6.2.1.jre8.jar.  

### <a name="for-microsoft-jdbc-driver-41-42-and-60"></a>Pour Microsoft JDBC Driver 6.0, 4.2 et 4.1
 Les fichiers sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar et sqljdbc42.jar sont installés aux emplacements suivants :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \sqljdbc.jar  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \sqljdbc4.jar  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \sqljdbc41.jar  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \sqljdbc42.jar  
  
 Voici un exemple de l'instruction CLASSPATH utilisée pour une application Windows :  
  
 `CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.0 for SQL Server\sqljdbc_4.2\enu\sqljdbc42.jar`  
  
 Voici un exemple de l'instruction CLASSPATH utilisée pour une application Unix/Linux :  
  
 `CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_4.2/enu/sqljdbc42.jar`  
  
 Vous devez vous assurer que l’instruction CLASSPATH contient un seul [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], tel que sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar ou sqljdbc42.jar.  
  
> [!NOTE]  
>  Sous Windows, les noms de répertoires d'une longueur supérieure à celle établie par la convention des noms de fichiers 8.3 ou les noms de dossiers contenant des espaces peuvent générer des problèmes avec les instructions classpath. Si vous pensez que ces types de problèmes, vous devez déplacer temporairement le fichier sqljdbc.jar, sqljdbc4.jar fichier ou le fichier sqljdbc41.jar dans un nom de répertoire simple tels que `C:\Temp`, modifiez l’instruction classpath et vérifiez si le problème.  
  
### <a name="applications-that-are-run-directly-at-the-command-prompt"></a>Applications exécutées directement à l'invite de commandes  
 L'instruction classpath est configurée dans le système d'exploitation. Ajoutez sqljdbc.jar, sqljdbc4.jar ou sqljdbc41.jar à l'instruction classpath du système. Vous pouvez également spécifier l’instruction classpath dans la ligne de commande Java qui exécute l’application à l’aide de la `java -classpath` option.  
  
### <a name="applications-that-run-in-an-ide"></a>Applications qui s'exécutent dans un IDE  
 Chaque fournisseur d'IDE fournit une méthode différente pour définir l'instruction classpath dans son IDE. La seule définition de l'instruction classpath dans le système d'exploitation ne fonctionne pas. Vous devez ajouter sqljdbc.jar, sqljdbc4.jar ou sqljdbc41.jar à l'instruction classpath de l'IDE.  
  
### <a name="servlets-and-jsps"></a>Servlets et JSP  
 Les servlets et les JSP sont exécutés dans un moteur de servlet/JSP tel que Tomcat. L'instruction classpath doit être définie conformément aux spécifications de la documentation relative au moteur de servlet/JSP. La seule définition de l'instruction classpath dans le système d'exploitation ne fonctionne pas. Certains moteurs de servlet/JSP affichent des écrans de configuration permettant de définir l'instruction classpath du moteur. Dans ce cas, vous devez ajouter le fichier JAR du pilote JDBC approprié à l'instruction classpath existante du moteur, puis redémarrer le moteur. Dans d'autres cas, vous pouvez déployer le pilote en copiant sqljdbc.jar, sqljdbc4.jar ou sqljdbc41.jar dans un répertoire spécifique, tel que lib, pendant l'installation du moteur. Vous pouvez également spécifier l'instruction classpath du pilote de moteur dans un fichier de configuration spécifique au moteur.  
  
### <a name="enterprise-java-beans"></a>EJB (Enterprise Java Beans)  
 Les EJB (Enterprise Java Beans) sont exécutés dans un conteneur EJB. Les conteneurs EJB proviennent de différents fournisseurs. Les applets Java s'exécutent dans un navigateur, mais sont téléchargés à partir d'un serveur web. Copiez sqljdbc.jar, sqljdbc4.jar ou sqljdbc41.jar à la racine du serveur web et spécifiez le nom du fichier JAR sous l’onglet d’archive HTML de l’applet, par exemple, `<applet ... archive=sqljdbc.jar>`.  
  
## <a name="making-a-simple-connection-to-a-database"></a>Création d'une connexion simple à une base de données  
 À l'aide de la bibliothèque de classes sqljdbc.jar, les applications doivent d'abord inscrire le pilote comme suit :  
  
 `Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");`  
  
 Lorsque le pilote est chargé, vous pouvez établir une connexion à l’aide d’une URL de connexion et la méthode getConnection de la classe DriverManager :  
  
```  
String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
   "databaseName=AdventureWorks;user=MyUserName;password=*****;";  
Connection con = DriverManager.getConnection(connectionUrl);  
```  
  
 Dans l’API JDBC 4.0, la méthode DriverManager.getConnection est améliorée pour charger automatiquement les pilotes JDBC. Par conséquent, les applications n’ont pas besoin d’appeler la méthode Class.forName pour inscrire ou charger le pilote lors de l’utilisation de la sqljdbc4.jar, sqljdbc41.jar ou sqljdbc42.jar les bibliothèque de classes.  
  
 Lorsque la méthode getConnection de la classe DriverManager est appelée, un pilote approprié est localisé parmi l’ensemble des pilotes JDBC inscrits. le fichier « META-INF/services/java.sql.Driver », qui contient le fichier sqljdbc4.jar, sqljdbc41.jar ou sqljdbc42.jar inclut le **com.microsoft.sqlserver.jdbc.SQLServerDriver** comme un pilote inscrit. Les applications existantes, qui chargent actuellement les pilotes à l’aide de la méthode Class.forName, continueront de fonctionner sans modification.  
  
> [!NOTE]  
>  Vous ne pouvez pas utiliser les bibliothèques de classe sqljdbc4.jar, sqljdbc41.jar et sqljdbc42.jar avec les anciennes versions de Java Runtime Environment (JRE). Consultez [configuration système requise pour le pilote JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md) pour obtenir la liste des versions JRE prises en charge par le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].  
  
 Pour plus d’informations sur la façon de se connecter aux sources de données et une URL de connexion, consultez [générer l’URL de connexion](../../connect/jdbc/building-the-connection-url.md) et [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
