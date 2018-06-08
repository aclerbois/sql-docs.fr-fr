---
title: À l’aide de Kerberos intégré d’authentification pour se connecter à SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 687802dc-042a-4363-89aa-741685d165b3
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6e2019cba57870cf9e22bdeb4d27f52c055fcdc8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-kerberos-integrated-authentication-to-connect-to-sql-server"></a>Utilisation de l’authentification intégrée Kerberos pour se connecter à SQL Server
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  À compter de [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)], une application peut utiliser le **authenticationScheme** propriété de connexion pour indiquer qu’il souhaite se connecter à une base de données à l’aide de l’authentification intégrée Kerberos type 4. Consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md) pour plus d’informations sur les propriétés de connexion. Pour plus d’informations sur Kerberos, consultez [Microsoft Kerberos](http://go.microsoft.com/fwlink/?LinkID=100758).  
  
 Lorsque vous utilisez l’authentification intégrée avec le Java **Krb5LoginModule**, vous pouvez configurer le module à l’aide de [classe Krb5LoginModule](https://docs.oracle.com/javase/8/docs/jre/api/security/jaas/spec/com/sun/security/auth/module/Krb5LoginModule.html).  
  
 Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] définit les propriétés suivantes pour les machines virtuelles Java IBM :  
  
-   **useDefaultCcache = true**  
  
-   **moduleBanner = false**  
  
 Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] définit les propriétés suivantes pour toutes les autres machines virtuelles Java :  
  
-   **useTicketCache = true**  
  
-   **doNotPrompt = true**  
  
## <a name="remarks"></a>Notes  
 Antérieures à [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)], les applications peuvent spécifier l’authentification intégrée (à l’aide de Kerberos ou NTLM, selon ce qui est disponible) à l’aide de la **integratedSecurity** propriété de connexion et en référençant  **sqljdbc_auth.dll**, comme décrit dans [générer l’URL de connexion](../../connect/jdbc/building-the-connection-url.md).  
  
 À compter de [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)], une application peut utiliser le **authenticationScheme** authentification pure Kerberos Java intégrée de propriété de connexion pour indiquer qu’il souhaite se connecter à une base de données à l’aide de Kerberos implémentation :  
  
-   Si vous souhaitez à l’aide de l’authentification intégrée **Krb5LoginModule**, vous devez toujours spécifier le **integratedSecurity = true** propriété de connexion. Puis spécifiez également le **authenticationScheme = Java Kerberos** propriété de connexion.  
  
-   Pour continuer à utiliser l’authentification intégrée avec **sqljdbc_auth.dll**, il suffit de spécifier **integratedSecurity = true** propriété de connexion (et éventuellement **authenticationScheme = NativeAuthentication**).  
  
-   Si vous spécifiez **authenticationScheme = Java Kerberos** mais ne spécifiez pas **integratedSecurity = true**, le pilote ignorera la **authenticationScheme** propriété de connexion et s’attendra à trouver des informations d’identification de nom et mot de passe utilisateur dans la chaîne de connexion.  
  
 Lorsque vous utilisez une source de données pour créer des connexions, vous pouvez définir par programme le schéma d’authentification à l’aide de setAuthenticationScheme et (éventuellement) de définir le SPN pour les connexions Kerberos à l’aide de **setServerSpn**.  
  
 Un nouvel enregistreur d'événements a été ajouté pour la prise en charge de l'authentification Kerberos : com.microsoft.sqlserver.jdbc.internals.KerbAuthentication. Pour plus d’informations, consultez [fonctionnement du pilote de traçage](../../connect/jdbc/tracing-driver-operation.md).  
  
 Les consignes suivantes vous aideront à configurer Kerberos :  
  
1.  Définissez **AllowTgtSessionKey** 1 dans le Registre Windows. Pour plus d’informations, consultez [entrées de Registre et les clés de configuration KDC dans Windows Server 2003 du protocole Kerberos](http://support.microsoft.com/kb/837361).  
  
2.  Assurez-vous que la configuration Kerberos (krb5.conf dans les environnements UNIX) référence le bon domaine et le centre de distribution de clés (KDC) convenant à votre environnement.  
  
3.  Initialisez le cache des tickets TGT à l'aide de l'outil kinit, en vous connectant au domaine.  
  
4.  Lorsqu’une application qui utilise **authenticationScheme = Java Kerberos** s’exécute sur le Windows Vista ou Windows 7 de systèmes d’exploitation, vous devez utiliser un compte d’utilisateur standard. Toutefois, si vous exécutez l'application à l'aide d'un compte d'administrateur, vous devrez l'exécuter à l'aide de privilèges administrateur.  
  
> [!NOTE]  
>  L’attribut de connexion serverSpn est uniquement pris en charge par Microsoft JDBC Driver 4.2 plus élevée.  
  
## <a name="service-principal-names"></a>Noms des principaux du service  
 Le nom de principal du service (SPN) est le nom par lequel un client identifie de manière unique l'instance d'un service.  
  
 Vous pouvez spécifier le nom principal de service à l’aide de la **serverSpn** propriété de connexion, ou simplement laisser le pilote le créer (la valeur par défaut).  Cette propriété est sous la forme : «MSSQLSvc/fqdn:port@REALM» où fqdn désigne le nom de domaine complet, le port est le numéro de port et REALM désigne le domaine Kerberos du serveur SQL en majuscules.  La partie domaine de cette propriété est facultative si le domaine par défaut de votre configuration Kerberos est le même que celui du serveur et n'est pas inclus par défaut.  Si vous voulez prendre en charge un scénario d'authentification entre domaines où le domaine par défaut dans la configuration Kerberos est différent de celui du serveur, vous devez définir le SPN avec la propriété serverSpn.  
  
 Par exemple, votre SPN peut ressembler à : «MSSQLSvc/some-server.zzz.corp.contoso.com:1433@ZZZZ.CORP.CONTOSO.COM»  
  
 Pour plus d'informations sur les noms des principaux du service (SPN), consultez :  
  
-   [Comment utiliser l’authentification Kerberos dans SQL Server](http://support.microsoft.com/kb/319723)  
  
-   [À l’aide de Kerberos avec SQL Server](http://go.microsoft.com/fwlink/?LinkId=207814)  

> [!NOTE]  
> Avant de 6.2.0 de version du pilote JDBC, pour une utilisation correcte du protocole Kerberos Realm croisée, vous devez définir explicitement la **serverSpn**.
>
> À compter de la 6.2.0 version, le pilote sera en mesure de générer le **serverSpn** par défaut, même si vous utilisez Kerberos de domaine Cross. Bien que vous pouvez utiliser **serverSpn** explicitement trop. 
  
## <a name="creating-a-login-module-configuration-file"></a>Création d'un fichier de configuration d'un module de connexion  
 Vous pouvez, si vous le désirez, créer un fichier de configuration Kerberos. Si vous ne spécifiez pas de fichier de configuration, les paramètres suivants seront appliqués :  
  
 Machine virtuelle Java Sun  
 com.sun.security.auth.module.Krb5LoginModule requis useTicketCache = true ;  
  
 Machine virtuelle Java IBM  
 com.ibm.security.auth.module.Krb5LoginModule required useDefaultCcache = true ;  
  
 Si vous décidez de créer un fichier de configuration de module de connexion, le fichier doit respecter le format suivant :  
  
```  
<name> {  
    <LoginModule> <flag> <LoginModule options>;  
    <optional_additional_LoginModules, flags_and_options>;  
};  
```  
  
 Un fichier de configuration de connexion est composé d'une ou plusieurs entrées, chacune spécifiant la technologie d'authentification sous-jacente qui doit être utilisée pour une application en particulier. Par exemple :  
  
```  
SQLJDBCDriver {  
   com.sun.security.auth.module.Krb5LoginModule required useTicketCache=true;  
};  
```  
  
 Chaque entrée de fichier de configuration de module de connexion correspond à un nom suivi d'une ou de plusieurs entrées spécifiques à LoginModule, où chaque entrée spécifique à LoginModule est suivie d'un point-virgule et où la totalité des entrées spécifiques à LoginModule est comprise entre accolades. Chaque entrée du fichier de configuration se termine par un point-virgule.  
  
 En plus de permettre au pilote d'acquérir les informations d'identification Kerberos à l'aide des paramètres spécifiés dans le fichier de configuration du module de connexion, le pilote peut utiliser les informations d'identification existantes. Cela peut se révéler utile si votre application doit créer des connexions utilisant les informations d'identification de plusieurs utilisateurs.  
  
 Le pilote tentera d'utiliser les informations d'identification existantes si disponibles, avant de tenter de se connecter au module de connexion spécifié. Par conséquent, lorsque vous utilisez la méthode Subject.doAs pour exécuter le code dans un contexte particulier, une connexion sera créée avec les informations d'identification transmises à l'appel Subject.doAs.  
  
 Pour plus d’informations, consultez [JAAS Login Configuration File](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jgss/tutorials/LoginConfigFile.html) et [classe Krb5LoginModule](https://docs.oracle.com/javase/8/docs/jre/api/security/jaas/spec/com/sun/security/auth/module/Krb5LoginModule.html).  

 À compter de Microsoft JDBC Driver 6.2, nom du fichier de configuration de module de connexion permettre éventuellement être passée à l’aide de jaasConfigurationName de propriété de connexion, cela permet à chaque connexion à avoir sa propre configuration de la connexion.
 
## <a name="creating-a-kerberos-configuration-file"></a>Création d'un fichier de configuration Kerberos  
 Pour plus d’informations sur les fichiers de configuration de Kerberos, consultez [Kerberos Requirements](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jgss/tutorials/KerberosReq.html).  
  
 Voici un exemple de fichier de configuration de domaine, où YYYY et ZZZZ correspondent aux noms de domaine de votre site.  
  
```  
[libdefaults]  
default_realm = YYYY.CORP.CONTOSO.COM  
dns_lookup_realm = false  
dns_lookup_kdc = true  
ticket_lifetime = 24h  
forwardable = yes  
  
[domain_realm]  
.yyyy.corp.contoso.com = YYYY.CORP.CONTOSO.COM  
.zzzz.corp.contoso.com = ZZZZ.CORP.CONTOSO.COM  
  
[realms]  
        YYYY.CORP.CONTOSO.COM = {  
  kdc = krbtgt/YYYY.CORP. CONTOSO.COM @ YYYY.CORP. CONTOSO.COM  
  default_domain = YYYY.CORP. CONTOSO.COM  
}  
  
        ZZZZ.CORP. CONTOSO.COM = {  
  kdc = krbtgt/ZZZZ.CORP. CONTOSO.COM @ ZZZZ.CORP. CONTOSO.COM  
  default_domain = ZZZZ.CORP. CONTOSO.COM  
}  
  
```  
  
## <a name="enabling-the-domain-configuration-file-and-the-login-module-configuration-file"></a>Activation du fichier de configuration de domaine et du fichier de configuration du module de connexion  
 Le fichier de configuration de domaine peut être activé à l'aide de -Djava.security.krb5.conf. Vous pouvez activer un fichier de configuration de module de connexion avec **-Djava.security.auth.login.config**.  
  
 Par exemple, lorsque vous démarrez votre application, vous pouvez utiliser la ligne de commande suivante :  
  
```  
Java.exe -Djava.security.auth.login.config=SQLJDBCDriver.conf -Djava.security.krb5.conf=krb5.ini <APPLICATION_NAME>  
  
```  
  
## <a name="verifying-that-sql-server-can-be-accessed-via-kerberos"></a>Vérification de l'accessibilité de SQL Server via Kerberos  
 Exécutez la requête suivante dans SQL Server Management Studio :  
  
 **Sélectionnez auth_scheme sys.dm_exec_connections où session_id = @@spid**  
  
 Assurez-vous de disposer de l'autorisation nécessaire pour exécuter cette requête.  

## <a name="constrained-delegation"></a>La délégation contrainte
À compter de Microsoft JDBC Driver 6.2, le pilote prend en charge la délégation contrainte Kerberos. Les informations d’identification déléguées peuvent être passées dans en tant qu’objet org.ietf.jgss.GSSCredential, ces informations d’identification sont utilisées par le pilote pour établir la connexion. 

```
Properties driverProperties = new Properties();
GSSCredential impersonatedUserCredential = [userCredential]
driverProperties.setProperty("integratedSecurity", "true");
driverProperties.setProperty("authenticationScheme", "JavaKerberos");
driverProperties.put("gsscredential", impersonatedUserCredential);
Connection conn = DriverManager.getConnection(CONNECTION_URI, driverProperties);
```

## <a name="kerberos-connection-using-principal-names-and-password"></a>Connexions Kerberos à l’aide des noms de Principal et le mot de passe
À compter de Microsoft JDBC Driver 6.2, le pilote peut établir Kerberos à l’aide du nom de Principal et le mot de passe de connexion transmise dans la chaîne de connexion. 
```
jdbc:sqlserver://servername=server_name;integratedSecurity=true;authenticationScheme=JavaKerberos;userName=user@REALM;password=****
```
La propriété de nom d’utilisateur ne nécessite pas de domaine si l’utilisateur appartient à la default_realm définie dans le fichier krb5.conf. Lorsque `userName` et `password` est défini avec `integratedSecurity=true;` et `authenticationScheme=JavaKerberos;` propriété, la connexion est établie avec la valeur du nom d’utilisateur en tant que le Principal Kerberos le long avec le mot de passe fourni.
 
## <a name="see-also"></a>Voir aussi  
 [Connexion à SQL Server avec le pilote JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
  
  
