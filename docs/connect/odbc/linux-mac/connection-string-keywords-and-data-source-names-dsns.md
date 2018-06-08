---
title: Connexion à SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data source names
- connection string keywords
- DSNs
ms.assetid: f95cdbce-e7c2-4e56-a9f7-8fa3a920a125
caps.latest.revision: 41
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8f49c0105f96f446c627f5ec1b5f47ee62b28558
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-sql-server"></a>Connexion à SQL Server
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Cette rubrique explique comment vous pouvez créer une connexion à un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données.  
  
## <a name="connection-properties"></a>Propriétés de connexion  

Consultez [DSN et les mots clés de chaîne de connexion et les attributs](../../../connect/odbc/dsn-connection-string-attribute.md) pour tous les mots clés de chaîne de connexion et les attributs pris en charge sur Mac et Linux

> [!IMPORTANT]  
> Quand vous vous connectez à une base de données qui utilise la mise en miroir de bases de données (qui a un partenaire de basculement), ne spécifiez pas le nom de la base de données dans la chaîne de connexion. Au lieu de cela, envoyez un **utiliser** *nom_base_de_données* commande se connecter à la base de données avant l’exécution de vos requêtes.  
  
La valeur passée à la **pilote** mot clé peut être une des valeurs suivantes :  
  
-   le nom que vous avez utilisé quand vous avez installé le pilote,

-   Le chemin d’accès à la bibliothèque de pilotes, qui a été spécifiée dans le fichier .ini de modèle utilisé pour installer le pilote.  

Pour créer une source de données, créer (si nécessaire) et modifiez le fichier **~/.odbc.ini** (`.odbc.ini` dans votre répertoire de base) pour une source de données utilisateur accessible uniquement à l’utilisateur actuel, ou `/etc/odbc.ini` pour une source de données système (des privilèges d’administrateur requis.) Voici un exemple de fichier qui affiche les entrées requises minimale pour une source de données :  

```  
[MSSQLTest]  
Driver = ODBC Driver 13 for SQL Server  
Server = [protocol:]server[,port]  
#   
# Note:  
# Port is not a valid keyword in the odbc.ini file  
# for the Microsoft ODBC driver on Linux or macOS
#  
```  

Vous pouvez éventuellement spécifier le protocole et le port à connecter au serveur. Par exemple, **Server = tcp :***nom_serveur***, 12345**. Notez que le seul protocole pris en charge par les pilotes de Linux et macOS est `tcp`.

Pour vous connecter à une instance nommée sur un port statique, utilisez <b>Server =</b>*nom_serveur*,**numéro_port**. Connexion à un port dynamique n’est pas pris en charge.  

Ou bien, vous pouvez ajouter les informations de source de données à un fichier de modèle et exécutez la commande suivante pour l’ajouter à `~/.odbc.ini` :
 - **ODBCINST -i -s -f** *template_file*  
 
Vous pouvez vérifier que votre pilote fonctionne à l’aide de `isql` pour tester la connexion, ou vous pouvez utiliser cette commande :
 - **master.INFORMATION_SCHEMA.TABLES bcp out -S un fichier OutFile.dat <server> - U <name> - P <password>**  

## <a name="using-secure-sockets-layer-ssl"></a>Utilisation du protocole SSL (Secure Sockets Layer)  
Vous pouvez utiliser Secure Sockets Layer (SSL) pour chiffrer les connexions à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]. Le protocole SSL protège [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] noms d’utilisateur et mots de passe sur le réseau. Il vérifie également l’identité du serveur à des fins de protection contre les attaques de l’intercepteur (« man in the middle »).  

L’activation du chiffrement renforce la sécurité au détriment des performances.

Pour plus d’informations, consultez [le chiffrement des connexions à SQL Server](http://go.microsoft.com/fwlink/?LinkId=220900) et [à l’aide du chiffrement sans Validation](https://docs.microsoft.com/en-us/sql/relational-databases/native-client/features/using-encryption-without-validation).

Quels que soient les paramètres pour **Encrypt** et **TrustServerCertificate**, les informations d’identification de connexion serveur (nom d’utilisateur et mot de passe) sont toujours chiffrées. Le tableau suivant montre l’effet des paramètres **Encrypt** et **TrustServerCertificate** .  

||**TrustServerCertificate=no**|**TrustServerCertificate = yes**|  
|-|-------------------------------------|------------------------------------|  
|**Chiffrer = non**|Le certificat de serveur n’est pas vérifié.<br /><br />Les données envoyées entre le client et le serveur ne sont pas chiffrées.|Le certificat de serveur n’est pas vérifié.<br /><br />Les données envoyées entre le client et le serveur ne sont pas chiffrées.|  
|**Chiffrer = Oui**|Le certificat de serveur est vérifié.<br /><br />Les données envoyées entre le client et le serveur sont chiffrées.<br /><br />Le nom (ou adresse IP) dans un nom d’objet courant (CN) ou un autre nom (objet) dans un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] certificat SSL doit correspondre exactement le nom de serveur (ou adresse IP) spécifié dans la chaîne de connexion.|Le certificat de serveur n’est pas vérifié.<br /><br />Les données envoyées entre le client et le serveur sont chiffrées.|  

Par défaut, les connexions chiffrées vérifient toujours le certificat du serveur. Toutefois, si vous vous connectez à un serveur qui possède un certificat auto-signé, également ajouter la `TrustServerCertificate` option pour ignorer la vérification du certificat à la liste des autorités de certification approuvées :  

```  
Driver={ODBC Driver 13 for SQL Server};Server=ServerNameHere;Encrypt=YES;TrustServerCertificate=YES  
```  
  
Le protocole SSL utilise la bibliothèque OpenSSL. Le tableau suivant présente les versions minimales prises en charge d’OpenSSL et les emplacements du magasin d’approbations de certificat par défaut pour chaque plateforme :

|Plateforme|Version OpenSSL minimale|Emplacement du magasin d’approbations de certificat par défaut|  
|------------|---------------------------|--------------------------------------------|
|Debian 9|1.1.0|/etc/ssl/certs|
|Debian 8.71 |1.0.1|/etc/ssl/certs|
|macOS 10.13|version 1.0.2|/usr/local/etc/OpenSSL/Certs|
|macOS 10.12|version 1.0.2|/usr/local/etc/OpenSSL/Certs|
|OS X 10.11|version 1.0.2|/usr/local/etc/OpenSSL/Certs|
|Red Hat Enterprise Linux 7|1.0.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 6|1.0.0-10|/etc/pki/tls/cert.pem|
|SuSE Linux Enterprise 12 |1.0.1|/etc/ssl/certs|
|SuSE Linux Enterprise 11 |0.9.8|/etc/ssl/certs|
|Ubuntu 17.10 |version 1.0.2|/etc/ssl/certs|
|Ubuntu 16.10 |version 1.0.2|/etc/ssl/certs|
|Ubuntu 16.04 |version 1.0.2|/etc/ssl/certs|
  
Vous pouvez également spécifier le chiffrement dans la chaîne de connexion à l’aide de la `Encrypt` option lors de l’utilisation **SQLDriverConnect** pour se connecter.

## <a name="see-also"></a>Voir aussi  
[L’installation de Microsoft ODBC Driver for SQL Server sur Linux et Mac OS](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)  
[Instructions de programmation](../../../connect/odbc/linux-mac/programming-guidelines.md)
