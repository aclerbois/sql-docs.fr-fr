---
title: À l’aide de l’authentification intégrée | Documents Microsoft
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
- integrated authentication
ms.assetid: 9499ffdf-e0ee-4d3c-8bca-605371eb52d9
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c70de16565cd90c3ca594fffcbbcc82bae89b90e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-integrated-authentication"></a>Utilisation de l’authentification intégrée
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Linux et macOS prend en charge les connexions qui utilisent Kerberos de l’authentification intégrée. Il prend en charge le centre de Distribution de clés (KDC) Kerberos MIT et fonctionne avec les bibliothèques v5 Security Services Application programme GSSAPI (Generic Interface) et Kerberos.
  
## <a name="using-integrated-authentication-to-connect-to-includessnoversionincludesssnoversionmdmd-from-an-odbc-application"></a>À l’aide de l’authentification intégrée pour se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] à partir d’une Application ODBC  

Vous pouvez activer l’authentification intégrée Kerberos en spécifiant **Trusted_Connection = yes** dans la chaîne de connexion de **SQLDriverConnect** ou **SQLConnect**. Par exemple :  

```
Driver='ODBC Driver 13 for SQL Server';Server=your_server;Trusted_Connection=yes  
```
  
Lorsque vous vous connectez avec un DSN, vous pouvez également ajouter **Trusted_Connection = yes** à l’entrée de source de données dans `odbc.ini`.
  
Le `-E` option de `sqlcmd` et `-T` option de `bcp` peut également être utilisé pour spécifier l’authentification intégrée, consultez [connexion avec **sqlcmd** ](../../../connect/odbc/linux-mac/connecting-with-sqlcmd.md) et [ Connexion avec **bcp** ](../../../connect/odbc/linux-mac/connecting-with-bcp.md) pour plus d’informations.

Vérifiez que le principal de client qui va se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] est déjà authentifié avec le KDC Kerberos.
  
**ServerSPN** et **FailoverPartnerSPN** ne sont pas pris en charge.  
  
## <a name="deploying-a-linux-or-macos-odbc-driver-application-designed-to-run-as-a-service"></a>Déploiement d’une Linux ou le macOS ODBC Driver Application conçue pour exécuter en tant que Service

Un administrateur système peut déployer une application de s’exécuter en tant que service qui utilise l’authentification Kerberos pour se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].  
  
Vous devez d’abord configurer Kerberos sur le client et vérifiez que l’application peut utiliser les informations d’identification Kerberos du principal par défaut.

Assurez-vous que vous utilisez `kinit` ou PAM (Pluggable Authentication Module) pour obtenir et mettre en cache le ticket TGT pour le principal utilisé par la connexion, via l’une des méthodes suivantes :  
  
-   Exécutez `kinit`, en passant un nom de principal et le mot de passe.  
  
-   Exécutez `kinit`, en passant un nom de principal et l’emplacement d’un fichier keytab qui contient la clé du principal créée par `ktutil`.  
  
-   Vérifiez que la connexion au système a été effectuée à l’aide du module de Kerberos PAM (Pluggable Authentication Module).

Lorsqu’une application s’exécute en tant que service, car l’expirent des informations d’identification Kerberos par sa conception, renouveler les informations d’identification pour garantir une disponibilité continue du service. Le pilote ODBC ne renouvelle pas d’informations d’identification lui-même ; Vérifiez qu’il existe un `cron` tâche ou un script qui s’exécute périodiquement pour renouveler les informations d’identification avant leur expiration. Pour éviter d’exiger le mot de passe à chaque renouvellement, vous pouvez utiliser un fichier keytab.  
  
L’article[Kerberos Configuration and Use](http://commons.oreilly.com/wiki/index.php/Linux_in_a_Windows_World/Centralized_Authentication_Tools/Kerberos_Configuration_and_Use) fournit des détails sur l’implémentation de Kerberos pour les services sur Linux.
  
## <a name="tracking-access-to-a-database"></a>Suivi de l’accès à une base de données

Un administrateur de base de données peut créer une piste d’audit de l’accès à une base de données lors de l’utilisation des comptes système pour accéder à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] à l’aide de l’authentification intégrée.  
  
Connexion à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] utilise le compte système et il n’existe aucune fonctionnalité sur Linux pour emprunter l’identité du contexte de sécurité. Ainsi, des opérations supplémentaires sont nécessaires pour déterminer l’utilisateur.
  
Pour auditer les activités dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] pour le compte d’utilisateur autre que le compte système, l’application doit utiliser [!INCLUDE[tsql](../../../includes/tsql_md.md)] **EXECUTE AS**.  
  
Pour améliorer ses performances, une application peut utiliser le regroupement de connexions avec l’authentification intégrée et l’audit. Toutefois, la combinaison de connexion de l’audit et de regroupement, l’authentification intégrée, crée un risque de sécurité, car le Gestionnaire de pilotes unixODBC permet à différents utilisateurs de réutiliser les connexions regroupées. Pour plus d’informations, consultez [ODBC Connection Pooling](http://www.unixodbc.org/doc/conn_pool.html).  

Avant d’être réutilisée, une application doit réinitialiser les connexions regroupées en exécutant `sp_reset_connection`.  

## <a name="using-active-directory-to-manage-user-identities"></a>Utilisation d’Active Directory pour gérer les identités des utilisateurs

Un administrateur système n’a pas gérer des ensembles distincts d’informations d’identification de connexion pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]. Il est possible de configurer Active Directory comme Centre de distribution de clés pour l’authentification intégrée. Consultez [Microsoft Kerberos](https://msdn.microsoft.com/en-us/library/windows/desktop/aa378747(v=vs.85).aspx) pour plus d’informations.

## <a name="using-linked-server-and-distributed-queries"></a>Utilisation de serveur lié et de requêtes distribuées

Les développeurs peuvent déployer une application qui utilise un serveur lié ou des requêtes distribuées sans qu’un administrateur de base de données ait à tenir à jour des ensembles distincts d’informations d’identification SQL. Dans ce cas, un développeur doit configurer une application pour utiliser l’authentification intégrée :  
  
-   L’utilisateur se connecte à un ordinateur client et s’authentifie auprès du serveur d’applications.  
  
-   Le serveur d’applications s’authentifie en tant qu’une autre base de données et se connecte à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] authentifie en tant qu’un utilisateur de base de données à une autre base de données ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].  
  
Une fois l’authentification intégrée configurée, les informations d’identification sont transmises au serveur lié.  
  
## <a name="integrated-authentication-and-sqlcmd"></a>Authentification intégrée et sqlcmd
Pour accéder à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] à l’aide de l’authentification intégrée, utilisez le `-E` option de `sqlcmd`. Vérifiez que le compte qui exécute `sqlcmd` est associé à l’entité de sécurité du client Kerberos par défaut.

## <a name="integrated-authentication-and-bcp"></a>Authentification intégrée et bcp
Pour accéder à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] à l’aide de l’authentification intégrée, utilisez le `-T` option de `bcp`. Vérifiez que le compte qui exécute `bcp` est associé à l’entité de sécurité du client Kerberos par défaut. 
  
Il s’agit d’une erreur à utiliser `-T` avec la `-U` ou `-P` option.
  
## <a name="supported-syntax-for-an-spn-registered-by-includessnoversionincludesssnoversionmdmd"></a>Syntaxe prise en charge pour un SPN inscrit par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]

La syntaxe qui utilisent des noms principaux de service dans la chaîne de connexion ou les attributs de connexion est la suivante :  

|Syntaxe| Description|  
|----------|---------------|  
|MSSQLSvc/*fqdn*:*port*|Nom principal de service par défaut, généré par le fournisseur, lorsque le protocole TCP est utilisé. *port* est un numéro de port TCP. *fqdn* est un nom de domaine complet.|  
  
## <a name="authenticating-a-linux-or-macos-computer-with-active-directory"></a>Authentification un macOS ordinateur auprès d’Active Directory ou Linux

Pour configurer Kerberos, entrer des données dans le `krb5.conf` fichier. `krb5.conf` est de `/etc/` , mais vous pouvez faire référence à un autre fichier, par exemple, à l’aide de la syntaxe `export KRB5_CONFIG=/home/dbapp/etc/krb5.conf`. Voici un exemple `krb5.conf` fichier :  
  
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
```  
  
Si votre ordinateur Linux ou macOS est configuré pour utiliser la Configuration de protocole DHCP (Dynamic Host) avec un serveur DHCP Windows fournit les serveurs DNS à utiliser, vous pouvez utiliser **dns_lookup_kdc = true**. Maintenant, vous pouvez utiliser Kerberos pour se connecter à votre domaine en émettant la commande `kinit alias@YYYY.CORP.CONTOSO.COM`. Paramètres passés à `kinit` respectent la casse et le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] ordinateur configuré pour être dans le domaine doit avoir cet utilisateur `alias@YYYY.CORP.CONTOSO.COM` ajoutés pour la connexion. Vous pouvez maintenant utiliser des connexions approuvées (**Trusted_Connection = YES** dans une chaîne de connexion, **bcp -T**, ou **sqlcmd-e**).  
  
L’heure sur l’ordinateur Linux ou macOS et l’heure sur le centre de Distribution de clés Kerberos (KDC) doivent être proches. Assurez-vous que l’heure système est correctement définie, par exemple, à l’aide du protocole NTP (Network Time).  

Si l’authentification Kerberos échoue, le pilote ODBC sur Linux ou macOS n’utilise pas l’authentification NTLM.  

Pour plus d’informations sur l’authentification des ordinateurs Linux ou macOS avec Active Directory, consultez [authentifier les Clients Linux avec Active Directory](http://technet.microsoft.com/magazine/2008.12.linux.aspx#id0060048) et [meilleures pratiques pour l’intégration OS X avec Active Directory](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf). Pour plus d’informations sur la configuration de Kerberos, consultez le [MIT Kerberos Documentation](https://web.mit.edu/kerberos/krb5-1.12/doc/index.html).

## <a name="see-also"></a>Voir aussi  
[Instructions de programmation](../../../connect/odbc/linux-mac/programming-guidelines.md)

[Notes de publication](../../../connect/odbc/linux-mac/release-notes.md)

[À l’aide d’Azure Active Directory](../../../connect/odbc/using-azure-active-directory.md)
