---
title: Utiliser l’authentification Active Directory (Kerberos) lors de la connexion avec les opérations de SQL Studio (version préliminaire) | Documents Microsoft
description: Découvrez comment activer l’authentification Kerberos utiliser l’authentification Active Directory pour les opérations de SQL Studio (version préliminaire)
ms.custom: tools|sos
ms.date: 11/17/2017
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: meet-bhagdev
ms.author: meetb
manager: craigg
ms.openlocfilehash: 847638bc0693d83ba38dec6c8fec5e4ca030e01f
ms.sourcegitcommit: b3bb41424249de198f22d9c6d40df4996f083aa6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/18/2018
---
# <a name="connect-includename-sosincludesname-sos-shortmd-to-your-sql-server-using-windows-authentication---kerberos"></a>Se connecter [!INCLUDE[name-sos](../includes/name-sos-short.md)] à votre serveur SQL Server à l’aide de l’authentification Windows - Kerberos 

[!INCLUDE[name-sos](../includes/name-sos-short.md)] prend en charge la connexion à SQL Server à l’aide de Kerberos.

Pour pouvoir utiliser l’authentification intégrée (authentification Windows) sur macOS ou Linux, vous devez configurer un **ticket Kerberos** votre utilisateur en cours de liaison à un compte de domaine Windows. 

## <a name="prerequisites"></a>Configuration requise

- Accès à un ordinateur appartenant à un domaine Windows pour pouvoir interroger votre contrôleur de domaine Kerberos.
- SQL Server doit être configuré pour autoriser l’authentification Kerberos. Pour le pilote du client en cours d’exécution sous Unix, l’authentification intégrée est uniquement prise en charge Kerberos. Plus d’informations sur la configuration de Sql Server pour utiliser l’authentification Kerberos peut être trouvé [ici](https://support.microsoft.com/en-us/help/319723/how-to-use-kerberos-authentication-in-sql-server). Il doit y avoir des noms SPN inscrits pour chaque instance de Sql Server, vous essayez de vous connecter à. Pour plus d’informations sur le format des noms principaux de service SQL Server sont répertoriés [ici](https://technet.microsoft.com/en-us/library/ms191153%28v=sql.105%29.aspx#SPN%20Formats)


## <a name="checking-if-sql-server-has-kerberos-setup"></a>Vérification si Kerberos le programme d’installation de Sql Server

Connexion à l’ordinateur hôte de Sql Server. À partir de l’invite de commandes Windows, utilisez le `setspn -L %COMPUTERNAME%` pour répertorier tous les noms de Principal du Service pour l’hôte. Vous devez voir des entrées qui commencent par MSSQLSvc/HostName.Domain.com ce qui signifie que Sql Server a inscrit un nom principal de service est prêt à accepter l’authentification Kerberos. 
- Si vous n’avez pas accès à l’hôte de Sql Server, puis à partir de n’importe quel autre système d’exploitation Windows joint au même Active Directory, vous pouvez utiliser la commande `setspn -L <SQLSERVER_NETBIOS>` où < SQLSERVER_NETBIOS > est le nom de l’ordinateur de l’hôte de Sql Server.


## <a name="get-the-kerberos-key-distribution-center"></a>Obtenir le centre de Distribution de clés Kerberos

Recherche la valeur de configuration Kerberos KDC (Key Distribution Center). Sur un ordinateur Windows qui est joint à votre domaine Active Directory, exécutez la commande suivante : 

Démarrer `cmd.exe` et exécutez `nltest`.

```
nltest /dsgetdc:DOMAIN.COMPANY.COM (where “DOMAIN.COMPANY.COM” maps to your domain’s name)

Sample Output
DC: \\dc-33.domain.company.com
Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
...
The command completed successfully
```
Copiez le nom du contrôleur de domaine qui est la valeur de configuration KDC requise, dans ce cas dc-33.domain.company.com

## <a name="join-your-os-to-the-active-directory-domain-controller"></a>Joindre votre système d’exploitation pour le contrôleur de domaine Active Directory

### <a name="ubuntu"></a>Ubuntu
```bash
sudo apt-get install realmd krb5-user software-properties-common python-software-properties packagekit
```

Modifier la `/etc/network/interfaces` de fichiers afin que l’adresse de votre contrôleur de domaine Active Directory est répertorié comme un serveur de noms dns. Par exemple : 

```/etc/network/interfaces
<...>
# The primary network interface
auth eth0
iface eth0 inet dhcp
dns-nameservers **<AD domain controller IP address>**
dns-search **<AD domain name>**
```

> [!NOTE]
> L’interface réseau (eth0) peut-être différer pour des ordinateurs différents. Pour déterminer l’application que vous utilisez, exécutez ifconfig et copier l’interface qui possède une adresse IP et les octets transmis et reçus.

Après avoir modifié ce fichier, redémarrez le service réseau :

```bash
sudo ifdown eth0 && sudo ifup eth0
```

Ensuite, vérifier que votre `/etc/resolv.conf` fichier contient une ligne semblable à la suivante :  

```Code
nameserver **<AD domain controller IP address>**
```

```bash
sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
<...>
* Success
```
   
### <a name="redhat-enterprise-linux"></a>RedHat Enterprise Linux
```bash
sudo yum install realmd krb5-workstation
```

Modifier la `/etc/sysconfig/network-scripts/ifcfg-eth0` fichier (ou autres config de l’interface de fichiers selon le cas) afin que l’adresse de votre contrôleur de domaine Active Directory est répertorié comme un serveur DNS :

```/etc/sysconfig/network-scripts/ifcfg-eth0
<...>
PEERDNS=no
DNS1=**<AD domain controller IP address>**
```

Après avoir modifié ce fichier, redémarrez le service réseau :

```bash
sudo systemctl restart network
```

Ensuite, vérifier que votre `/etc/resolv.conf` fichier contient une ligne semblable à la suivante :  

```Code
nameserver **<AD domain controller IP address>**
```

```bash
sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
<...>
* Success
   
```

### <a name="macos"></a>macOS

- Joindre votre macOS pour le contrôleur de domaine Active Directory [comme suit] (https://support.apple.com/kb/PH26282?viewlocale=en_US&locale=en_US).



## <a name="configure-kdc-in-krb5conf"></a>Configurer le KDC dans krb5.conf

Modifier la `/etc/krb5.conf` dans un éditeur de votre choix. Configurer les clés suivantes

```bash
sudo vi /etc/krb5.conf

[libdefaults]
  default_realm = DOMAIN.COMPANY.COM
 
[realms]
DOMAIN.COMPANY.COM = {
   kdc = dc-33.domain.company.com
}
```

Puis enregistrez le fichier krb5.conf et quitter

> [!NOTE]
> Domaine doit être en majuscules


## <a name="test-the-ticket-granting-ticket-retrieval"></a>Test de la récupération de Ticket Granting Ticket

Obtenir un Ticket d’accord de Ticket (TGT ticket) à partir du contrôleur de domaine Kerberos.

```bash
kinit username@DOMAIN.COMPANY.COM
```

Afficher les tickets disponibles à l’aide de kinit. Si le kinit a réussi, vous devez voir un ticket. 

```bash
klist

krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.
```

## <a name="connect-using-includename-sosincludesname-sos-shortmd"></a>Se connecter à l’aide de [!INCLUDE[name-sos](../includes/name-sos-short.md)]

* Créer un nouveau profil de connexion

* Choisissez **l’authentification Windows** en tant que le type d’authentification

* Terminez le profil de connexion, cliquez sur **se connecter**

Après vous être connecté, votre serveur s’affiche dans le *serveurs* barre latérale.
