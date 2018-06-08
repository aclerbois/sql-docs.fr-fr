---
title: Chiffrement des connexions à SQL Server sur Linux | Documents Microsoft
description: Cet article décrit le chiffrement des connexions à SQL Server sur Linux.
author: tmullaney
ms.date: 01/30/2018
ms.author: meetb
manager: craigg
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: ''
helpviewer_keywords:
- Linux, encrypted connections
ms.openlocfilehash: f7b1dd57af300fc3e3fa61e469646211536b4653
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="encrypting-connections-to-sql-server-on-linux"></a>Chiffrement des connexions à SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux peuvent utiliser sécurité TLS (Transport Layer) pour chiffrer les données transmises sur un réseau entre une application cliente et une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge les mêmes protocoles TLS sur Windows et Linux : TLS 1.0, 1.1 et 1.2. Toutefois, les étapes de configuration TLS sont spécifiques au système d’exploitation sur lequel [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est en cours d’exécution.  

## <a name="requirements-for-certificates"></a>Exigences relatives aux certificats 
Avant de commencer, vous devez vous assurer que vos certificats de respecter les règles suivantes :
- L’heure système actuelle doit être après le valide à partir de la propriété du certificat et avant le valide à la propriété du certificat.
- Le certificat doit être destiné à une authentification serveur. Cela nécessite la propriété utilisation améliorée de la clé du certificat pour spécifier l’authentification du serveur (1.3.6.1.5.5.7.3.1).
- Le certificat doit être créé à l’aide de l’option KeySpec de AT_KEYEXCHANGE. En règle générale, propriété d’utilisation de la clé du certificat (KEY_USAGE) inclut également le chiffrage de clés (CERT_KEY_ENCIPHERMENT_KEY_USAGE).
- La propriété de sujet du certificat doit indiquer que le nom commun (CN) est le même que le nom d’hôte ou le nom de domaine complet (FQDN) de l’ordinateur serveur. Remarque : les certificats génériques sont pris en charge. 

## <a name="overview"></a>Vue d'ensemble
TLS est utilisé pour chiffrer les connexions à partir d’une application cliente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. En cas de correctement configuré, TLS fournit la confidentialité et l’intégrité des données pour les communications entre le client et le serveur.  Les connexions TLS peuvent être initié par le client ou initiée par le serveur. 


## <a name="client-initiated-encryption"></a>Chiffrement initiée par le client 
- **Générer le certificat** (CN doit correspondre à votre nom de domaine complet d’hôte SQL Server)

> [!NOTE]
> Pour cet exemple, nous utilisons un certificat auto-signé, cela ne doit pas utilisé pour les scénarios de production. Vous devez utiliser des certificats d’autorité de certification. 

        openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
        sudo chown mssql:mssql mssql.pem mssql.key 
        sudo chmod 600 mssql.pem mssql.key   
        sudo mv mssql.pem /etc/ssl/certs/ 
        sudo mv mssql.key /etc/ssl/private/ 

- **Configurer SQL Server**

        systemctl stop mssql-server 
        cat /var/opt/mssql/mssql.conf 
        sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssqlfqdn.pem 
        sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssqlfqdn.key 
        sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
        sudo /opt/mssql/bin/mssql-conf set network.forceencryption 0 

- **Inscrire le certificat sur votre ordinateur client (Windows, Linux ou macOS)**

    -   Si vous utilisez un certificat signé d’autorité de certification, vous devez copier le certificat d’autorité de certification (CA) au lieu du certificat de l’utilisateur sur l’ordinateur client. 
    -   Si vous utilisez le certificat auto-signé, simplement copier le fichier .pem dans les dossiers suivants correspondant à la distribution et exécutez les commandes pour leur permettre de 
        - **Ubuntu**: certificat copie à ```/usr/share/ca-certificates/``` renommer extension .crt utiliser des certificats d’autorité de certification dpkg-reconfigure pour l’activer en tant que certificat de système d’autorité de certification. 
        - **RHEL**: certificat copie à ```/etc/pki/ca-trust/source/anchors/``` utiliser ```update-ca-trust``` pour l’activer en tant que certificat de système d’autorité de certification.
        - **SUSE**: certificat copie à ```/usr/share/pki/trust/anchors/``` utiliser ```update-ca-certificates``` pour l’activer en tant que certificat de système d’autorité de certification.
        - **Windows**: importer le fichier .pem en tant que certificat sous utilisateur actuel -> approuvé autorités de certification racine -> certificats
        - **macOS**: 
           - Copiez le certificat à ```/usr/local/etc/openssl/certs```
           - Exécutez la commande suivante pour obtenir la valeur de hachage : ```/usr/local/Cellar/openssql/1.0.2l/openssql x509 -hash -in mssql.pem -noout```
           - Renommer le certificat à la valeur. Par exemple : ```mv mssql.pem dc2dd900.0```. Assurez-vous que dc2dd900.0 se trouve dans ```/usr/local/etc/openssl/certs```
    
-   **Exemples de chaîne de connexion** 

    - **[!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]**   
  ![Boîte de dialogue de connexion SSMS](media/sql-server-linux-encrypted-connections/ssms-encrypt-connection.png "boîte de dialogue de connexion SSMS")  
  
    - **SQLCMD** 

            sqlcmd  -S <sqlhostname> -N -U sa -P '<YourPassword>' 
    - **ADO.NET** 

            "Encrypt=True; TrustServerCertificate=False;" 
    - **ODBC** 

            "Encrypt=Yes; TrustServerCertificate=no;" 
    - **JDBC** 
    
            "encrypt=true; trustServerCertificate=false;" 

## <a name="server-initiated-encryption"></a>Chiffrement occasionnés par le serveur 

- **Générer le certificat** (CN doit correspondre à votre nom de domaine complet d’hôte SQL Server)
        
        openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
        sudo chown mssql:mssql mssql.pem mssql.key 
        sudo chmod 600 mssql.pem mssql.key   
        sudo mv mssql.pem /etc/ssl/certs/ 
        sudo mv mssql.key /etc/ssl/private/ 

- **Configurer SQL Server**

        systemctl stop mssql-server 
        cat /var/opt/mssql/mssql.conf 
        sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssqlfqdn.pem 
        sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssqlfqdn.key 
        sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
        sudo /opt/mssql/bin/mssql-conf set network.forceencryption 1 
        
-   **Exemples de chaîne de connexion** 

    - **SQLCMD**

            sqlcmd  -S <sqlhostname> -U sa -P '<YourPassword>' 
    - **ADO.NET** 

            "Encrypt=False; TrustServerCertificate=False;" 
    - **ODBC** 

            "Encrypt=no; TrustServerCertificate=no;"  
    - **JDBC** 
    
            "encrypt=false; trustServerCertificate=false;" 
            
> [!NOTE]
> Définissez **TrustServerCertificate** à la valeur True si le client ne peut pas se connecter à valider l’authenticité du certificat de l’autorité de certification

## <a name="common-connection-errors"></a>Erreurs de connexion courantes  

|Message d'erreur |Fix |
|--- |--- |
|La chaîne de certificats a été émise par une autorité qui n’est pas approuvée.  |Cette erreur se produit lorsque les clients ne peuvent pas vérifier la signature sur le certificat présenté par SQL Server pendant la négociation TLS. Assurez-vous que le client approuve soit le [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] certificat directement, ou l’autorité de certification qui a signé le certificat SQL Server. |
|Le nom du principal cible est incorrect.  |Vérifiez que champ de nom commun de certificat SQL Server correspond au nom de serveur spécifié dans la chaîne de connexion du client. |  
|Une connexion existante a dû être fermée par l’hôte distant. |Cette erreur peut se produire lorsque le client ne prend pas en charge la version du protocole TLS requise par SQL Server. Par exemple, si [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est configuré pour exiger le protocole TLS 1.2, assurez-vous que vos clients prennent également en charge le protocole TLS 1.2. |
| | |   
