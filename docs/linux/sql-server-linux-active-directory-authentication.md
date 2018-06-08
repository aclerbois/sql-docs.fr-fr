---
title: Didacticiel de l’authentification Active Directory pour SQL Server sur Linux | Documents Microsoft
description: Ce didacticiel fournit les étapes de configuration pour l’authentification AD pour SQL Server sur Linux.
author: meet-bhagdev
ms.date: 02/23/2018
ms.author: meetb
manager: craigg
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: df3cea6d47d50464fe0b8a7f2573c230585b9cb1
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="tutorial-use-active-directory-authentication-with-sql-server-on-linux"></a>Didacticiel : L’authentification utilisation d’Active Directory avec SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce didacticiel explique comment configurer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux pour prendre en charge l’authentification Active Directory (AD), également appelée authentification intégrée. Pour une vue d’ensemble, consultez [authentification Active Directory pour SQL Server sur Linux](sql-server-linux-active-directory-auth-overview.md).

Ce didacticiel comprend les tâches suivantes :

> [!div class="checklist"]
> * Joindre un hôte [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à un domaine Active Directory.
> * Créer un utilisateur Active Directory pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et définir le nom principal de service
> * Configurer le service keytab de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]
> * Créer des comptes de connexion basée sur Active Directory dans Transact-SQL
> * Se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide de l’authentification Active Directory

## <a name="prerequisites"></a>Configuration requise

Avant de configurer l’authentification Active Directory, vous devez :

* Configurer un contrôleur de domaine Active Directory (Windows) sur votre réseau  
* Installer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].
  * [Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
  * [SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
  * [Ubuntu](quickstart-install-connect-ubuntu.md)

## <a id="join"></a> Joindre [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] hôte de domaine Active Directory.

Utilisez les étapes suivantes pour joindre un hôte [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à un domaine Active Directory :

1. Utilisez **[realmd](https://www.freedesktop.org/software/realmd/docs/guide-active-directory-join.html)** pour joindre votre ordinateur hôte à votre domaine Active Directory. Si vous ne l’avez pas encore fait, installer les packages du client Kerberos et realmd sur l'ordinateur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] hôte à l’aide du Gestionnaire de package de votre distribution Linux :

   ```bash
   # RHEL
   sudo yum install realmd krb5-workstation

   # SUSE
   sudo zypper install realmd krb5-client

   # Ubuntu
   sudo apt-get install realmd krb5-user software-properties-common python-software-properties packagekit
   ```

1. Si l’installation du package client Kerberos vous invite à entrer un nom de domaine, entrez votre nom de domaine en majuscules.

   > [!NOTE]
   > Cette procédure pas à pas utilise « contoso.com » et « CONTOSO.COM » en tant qu’exemples de noms de domaine et le domaine, respectivement. Vous devez remplacer par vos propres valeurs. Ces commandes respectent la casse, par conséquent, assurez-vous que vous utilisez majuscules partout où il est utilisé dans cette procédure pas à pas.

1. Configurer votre ordinateur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] hôte pour utiliser l’adresse de votre contrôleur de domaine Active Directory en tant qu’un serveur de noms DNS.  

   - **Ubuntu**:

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

      Ensuite, vérifier que votre `/etc/resolv.conf` fichier contient une ligne à l’exemple suivant :  

      ```Code
      nameserver **<AD domain controller IP address>**
      ```

   - **RHEL**:

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

     Ensuite, vérifier que votre `/etc/resolv.conf` fichier contient une ligne à l’exemple suivant :  

     ```Code
     nameserver **<AD domain controller IP address>**
     ```

1. Joindre le domaine

   Une fois que vous avez vérifié que votre serveur DNS est correctement configuré, joindre le domaine en exécutant la commande suivante. Vous devez vous authentifier à l’aide d’un compte Active Directory qui dispose de privilèges suffisants dans Active Directory pour joindre un ordinateur au domaine.

   Plus précisément, cette commande crée un nouveau compte d’ordinateur dans Active Directory, créez le `/etc/krb5.keytab` héberger le fichier keytab et configurer le domaine dans `/etc/sssd/sssd.conf`:

   ```bash
   sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
   <...>
   * Successfully enrolled machine in realm
   ```

   > [!NOTE]
   > Si vous voyez une erreur, « les packages nécessaires ne sont pas installés », vous devez alors installer ces packages à l’aide du Gestionnaire de package de votre distribution Linux avant d’exécuter le `realm join` réexécutez la commande.
   >
   > Si vous recevez une erreur, « Autorisations insuffisantes pour joindre le domaine, » vous devez vérifier avec un administrateur de domaine que vous disposez des autorisations suffisantes pour joindre des ordinateurs Linux à votre domaine.
   >
   > Si vous recevez une erreur « réponse du contrôleur de domaine Kerberos ne correspondait pas aux attentes, » puis vous ne pouvez pas avoir spécifié le nom de domaine correct pour l’utilisateur. Noms de domaine respectent la casse, généralement en majuscules et peuvent être identifiés avec la commande `realm discover contoso.com`.
   
   > SQL Server utilise SSSD et NSS pour le mappage des comptes d’utilisateurs et des groupes aux identificateurs de sécurité (SID). SSSD doit être configuré et en cours d’exécution dans l’ordre pour SQL Server créer les connexions AD avec succès. Realmd généralement effectue automatiquement dans le cadre de joindre le domaine, mais dans certains cas vous devez le faire séparément.
   >
   > Consultez les ressources suivantes pour configurer [SSSD manuellement](https://access.redhat.com/articles/3023951), et [configurer NSS pour travailler avec SSSD](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system-level_authentication_guide/configuring_services#Configuration_Options-NSS_Configuration_Options)

  
5. Vérifiez que vous pouvez maintenant collecter des informations relatives à un utilisateur du domaine, et que vous pouvez obtenir un ticket Kerberos en tant qu’utilisateur.

   L’exemple suivant utilise **id**,  **[kinit](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html)**, et **[klist](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/klist.html)** commandes pour cela.

   ```bash
   id user@contoso.com
   uid=1348601103(user@contoso.com) gid=1348600513(domain group@contoso.com) groups=1348600513(domain group@contoso.com)

   kinit user@CONTOSO.COM
   Password for user@CONTOSO.COM:

   klist
   Ticket cache: FILE:/tmp/krb5cc_1000
   Default principal: user@CONTOSO.COM
   <...>
   ```

   > [!NOTE]
   > Si `id user@contoso.com` retourne, « Utilisateur inexistant, » assurez-vous que le service SSSD a démarré en exécutant la commande `sudo systemctl status sssd`. Si le service est en cours d’exécution et que vous rencontrez l’erreur « Aucun utilisateur », essayez d’activer la journalisation documentée pour SSSD. Pour plus d’informations, consultez la documentation de Red Hat pour [SSSD de résolution des problèmes](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/System-Level_Authentication_Guide/trouble.html#SSSD-Troubleshooting).
   >
   > Si `kinit user@CONTOSO.COM` retourne, « la réponse du contrôleur de domaine Kerberos ne correspond pas à attentes lors de l’obtention des informations d’identification initiales, » vérifiez que vous avez spécifié le domaine en majuscules.

Pour plus d’informations, consultez la documentation de Red Hat pour [découverte et la jointure de domaines d’identité](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/realmd-domain.html). 

## <a id="createuser"></a> Créer un utilisateur Active Directory pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et définissez le nom principal de service

  > [!NOTE]
  > Étapes de la prochaine utilisent votre [nom de domaine complet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Si vous êtes sur **Azure**, vous devez **[créer un](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/portal-create-fqdn)** avant de continuer.

1. Sur votre contrôleur de domaine, exécutez le [New-ADUser](https://technet.microsoft.com/library/ee617253.aspx) commande PowerShell pour créer un nouvel utilisateur AD avec un mot de passe n’expire jamais. Cet exemple montre comment le nom du compte « mssql », mais le nom du compte peut être comme vous le souhaitez. Vous devrez entrer un mot de passe pour le compte :

   ```PowerShell
   Import-Module ActiveDirectory

   New-ADUser mssql -AccountPassword (Read-Host -AsSecureString "Enter Password") -PasswordNeverExpires $true -Enabled $true
   ```

   > [!NOTE]
   > Il est recommandé d’avoir un compte Active Directory dédié pour SQL Server, afin que les informations d’identification de SQL Server ne sont pas partagées avec d’autres services utilisant le même compte. Toutefois, vous pouvez réutiliser un compte Active Directory existant si vous préférez, si vous connaissez le mot de passe (requis pour générer un fichier keytab à l’étape suivante).

2. Définir le nom principal de service (SPN) pour ce compte en utilisant le `setspn.exe` outil. Le SPN doit être formaté exactement comme indiqué dans l’exemple suivant. Vous pouvez trouver le nom de domaine complet de le [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ordinateur hôte en exécutant `hostname --all-fqdns` sur la [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] hôte et le port TCP doivent être 1433, sauf si vous avez configuré [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] d’utiliser un numéro de port différent.

   ```PowerShell
   setspn -A MSSQLSvc/**<fully qualified domain name of host machine>**:**<tcp port>** mssql
   ```

   > [!NOTE]
   > Si vous recevez une erreur, « droits d’accès insuffisants », vous devez vérifier avec un administrateur de domaine que vous disposez des autorisations suffisantes pour définir un nom SPN sur ce compte.
   >
   > Si vous modifiez le port TCP à l’avenir, vous devez exécuter la commande setspn à nouveau avec le nouveau numéro de port. Vous devez également ajouter le nouveau nom SPN pour le fichier keytab service de SQL Server en suivant les étapes décrites dans la section suivante.

3. Pour plus d’informations, consultez [Inscrire un nom de principal du service pour les connexions Kerberos](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).

## <a id="configurekeytab"></a> Configurer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] keytab de service

1. Vérifier le numéro de Version de clé (kvno) pour le compte AD créé à l’étape précédente. Il est généralement de 2, mais il peut être un autre entier si vous avez modifié le mot de passe plusieurs fois. Sur le [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ordinateur hôte, exécutez la commande suivante :

   ```bash
   kinit user@CONTOSO.COM

   kvno MSSQLSvc/**<fully qualified domain name of host machine>**:**<tcp port>**
   ```

2. Créer un fichier keytab avec **[ktutil](https://web.mit.edu/kerberos/krb5-1.12/doc/admin/admin_commands/ktutil.html)** pour l’utilisateur Active Directory que vous avez créé à l’étape précédente. Lorsque vous y êtes invité, entrez le mot de passe pour ce compte Active Directory.

   ```bash
   sudo ktutil

   ktutil: addent -password -p MSSQLSvc/**<fully qualified domain name of host machine>**:**<tcp port>**@CONTOSO.COM -k **<kvno from above>** -e aes256-cts-hmac-sha1-96

   ktutil: addent -password -p MSSQLSvc/**<fully qualified domain name of host machine>**:**<tcp port>**@CONTOSO.COM -k **<kvno from above>** -e rc4-hmac

   ktutil: wkt /var/opt/mssql/secrets/mssql.keytab

   quit
   ```

   > [!NOTE]
   > L’outil ktutil ne valide pas le mot de passe ; par conséquent, assurez-vous que vous l’entrez correctement.

3. Toute personne ayant accès à ce fichier `keytab` peut emprunter l’identité [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  sur le domaine, assurez-vous donc de restreindre l’accès au fichier tel que seul le compte `mssql` ait accès en lecture :

   ```bash
   sudo chown mssql:mssql /var/opt/mssql/secrets/mssql.keytab
   sudo chmod 400 /var/opt/mssql/secrets/mssql.keytab
   ```

4. Configurer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour utiliser ce fichier `keytab` pour l’authentification Kerberos :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
   sudo systemctl restart mssql-server
   ```

## <a id="createsqllogins"></a> Créer des comptes de connexion basée sur Active Directory dans Transact-SQL

1. Se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et créer une nouvelle connexion basée sur Active Directory :

   ```sql
   CREATE LOGIN [CONTOSO\user] FROM WINDOWS;
   ```

2. Vérifiez que la connexion est maintenant répertoriée dans l' affichage de catalogue système [sys.server_principals](../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) :

   ```sql
   SELECT name FROM sys.server_principals;
   ```

## <a id="connect"></a> Se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide de l’authentification Active Directory

Connectez-vous à un ordinateur client à l’aide de vos informations d’identification de domaine. Vous pouvez maintenant vous connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sans devoir entrer de nouveau votre mot de passe, à l’aide de l’authentification Active Directory. Si vous créez une connexion pour un groupe AD, tout utilisateur d’Active Directory qui est un membre de ce groupe peut se connecter de la même façon.

Le paramètre de chaîne de connexion spécifique pour les clients à utiliser l’authentification Active Directory varie selon le pilote que vous utilisez. Observez les exemples suivants :

* `sqlcmd` sur un client Linux appartenant au domaine

   Se connecter à un client Linux appartenant au domaine à l’aide de `ssh` et vos informations d’identification de domaine :

   ```bash
   ssh -l user@contoso.com client.contoso.com
   ```

   Vérifiez que vous avez installé le [mssql-tools](sql-server-linux-setup-tools.md) du package, puis se connecter à l’aide de `sqlcmd` sans spécifier d’informations d’identification :

   ```bash
   sqlcmd -S mssql.contoso.com
   ```

* SSMS sur un client Windows appartenant au domaine

   Connectez-vous à un client Windows appartenant au domaine à l’aide de vos informations d’identification de domaine. Assurez-vous que [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] est installé, puis connectez-vous à votre instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  en spécifiant **Authentification Windows** dans la boîte de dialogue **se connecter au serveur**.

* Authentification d’Active Directory à l’aide d’autres pilotes clients

  * JDBC : [à l’aide de Kerberos intégré à l’authentification de connexion SQL Server](https://docs.microsoft.com/sql/connect/jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server)
  * ODBC : [à l’aide de l’authentification intégrée](https://docs.microsoft.com/sql/connect/odbc/linux/using-integrated-authentication)
  * ADO.NET : [syntaxe de chaîne de connexion](https://msdn.microsoft.com/library/system.data.sqlclient.sqlauthenticationmethod(v=vs.110).aspx)
  
## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, nous avons passé en revue la définition de l’authentification Active Directory avec SQL Server sur Linux. Vous avez appris à : 
> [!div class="checklist"]
> * Joindre un hôte [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à un domaine Active Directory.
> * Créer un utilisateur Active Directory pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et définir le nom principal de service
> * Configurer le service keytab de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]
> * Créer des comptes de connexion basée sur Active Directory dans Transact-SQL
> * Se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide de l’authentification Active Directory

Ensuite, Explorer d’autres scénarios de sécurité pour SQL Server sur Linux.

> [!div class="nextstepaction"]
>[Chiffrement des connexions à SQL Server sur Linux](sql-server-linux-encrypted-connections.md)
