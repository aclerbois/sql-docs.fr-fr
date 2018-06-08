---
title: Bonnes pratiques de sécurité pour les bases de données à relation contenant-contenu | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- contained database, threats
ms.assetid: 026ca5fc-95da-46b6-b882-fa20f765b51d
caps.latest.revision: 14
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 360fd0262fdfad643e86fa7c8fd9eb075300c682
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="security-best-practices-with-contained-databases"></a>Bonnes pratiques de sécurité recommandées avec les bases de données à relation contenant-contenu
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Les bases de données à relation contenant-contenu présentent quelques menaces originales que les administrateurs du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] doivent connaître et limiter. La plupart de ces menaces sont liées au processus d’authentification **USER WITH PASSWORD** , qui déplace la limite de l’authentification du niveau du [!INCLUDE[ssDE](../../includes/ssde-md.md)] vers celui de la base de données.  
  
## <a name="threats-related-to-users"></a>Menaces associées aux utilisateurs  
 Les utilisateurs d’une base de données à relation contenant-contenu qui disposent de l’autorisation **ALTER ANY USER** , tels que les membres des rôles de base de données fixes **db_owner** et **db_securityadmin** , peuvent accorder un accès à la base de données, sans que l’administrateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le sache ou l’autorise. Autoriser des utilisateurs à accéder à une base de données à relation contenant-contenu augmente la surface d'exposition potentielle aux attaques contre l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entière. Les administrateurs doivent comprendre cette délégation du contrôle d'accès et être très prudents lorsqu'ils accordent à des utilisateurs l'autorisation **ALTER ANY USER** dans la base de données à relation contenant-contenu. Tous les propriétaires de base de données disposent de l'autorisation **ALTER ANY USER** . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent procéder périodiquement à un audit des utilisateurs dans une base de données à relation contenant-contenu.  
  
### <a name="accessing-other-databases-using-the-guest-account"></a>Accès à d'autres bases de données à l'aide du compte Invité  
 Les propriétaires de base de données et les utilisateurs de base de données disposant de l'autorisation **ALTER ANY USER** peuvent créer des utilisateurs de base de données à relation contenant-contenu. Une fois connecté à une base de données à relation contenant-contenu sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un utilisateur d'une base de données à relation contenant-contenu peut accéder à d'autres bases de données sur le [!INCLUDE[ssDE](../../includes/ssde-md.md)], si ces bases de données ont activé le compte **Invité** .  
  
### <a name="creating-a-duplicate-user-in-another-database"></a>Création d'un utilisateur en double dans une autre base de données  
 Certaines applications peuvent nécessiter qu'un utilisateur puisse accéder à plusieurs bases de données. Pour cela, il convient de créer des utilisateurs de base de données à relation contenant-contenu dans chaque base de données. Utilisez l'option SID pour créer le second utilisateur avec le mot de passe. L'exemple suivant crée deux utilisateurs identiques dans deux bases de données.  
  
```  
USE DB1;  
GO  
CREATE USER Carlo WITH PASSWORD = '<strong password>';   
-- Return the SID of the user  
SELECT SID FROM sys.database_principals WHERE name = 'Carlo';  
  
-- Change to the second database  
USE DB2;  
GO  
CREATE USER Carlo WITH PASSWORD = '<same password>', SID = <SID from DB1>;  
GO  
```  
  
 Pour exécuter une requête de bases de données croisées, vous devez définir l’option **TRUSTWORTHY** sur la base de données appelante. Par exemple, si l’utilisateur (Carlo) défini ci-dessus figure dans DB1, pour exécuter `SELECT * FROM db2.dbo.Table1;` , le paramètre **TRUSTWORTHY** doit être activé pour la base de données DB1. Exécutez le code suivant pour activer le paramètre **TRUSTWORTHY** .  
  
```  
ALTER DATABASE DB1 SET TRUSTWORTHY ON;  
```  
  
### <a name="creating-a-user-that-duplicates-a-login"></a>Création d'un utilisateur qui duplique une connexion  
 Lorsqu'un utilisateur de base de données à relation contenant-contenu est créé avec un mot de passe, si vous utilisez le même nom qu'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et si la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'effectue en spécifiant la base de données à relation contenant-contenu comme catalogue initial, la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sera impossible. La connexion sera évaluée en tant qu'utilisateur de base de données à relation contenant-contenu avec principal de mot de passe sur la base de données à relation contenant-contenu et non en tant qu'utilisateur basé sur la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cela risque de provoquer un déni de service intentionnel ou accidentel pour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Il est recommandé que les membres du rôle serveur fixe **sysadmin** se connectent toujours sans utiliser l'option de catalogue initial. Cela connecte la connexion à la base de données master et évite toute utilisation malveillante de la tentative de connexion de la part d'un propriétaire de base de données. L’administrateur peut ensuite passer à la base de données à relation contenant-contenu en utilisant l’instruction **USE***\<base_de_données>*. Vous pouvez également définir la base de données par défaut de la connexion sur la base de données à relation contenant-contenu, qui complète la connexion à **master**, puis transférer la connexion vers la base de données à relation contenant-contenu.  
  
-   Il est recommandé de ne pas créer d'utilisateurs de base de données à relation contenant-contenu avec mot de passe en utilisant le même nom que les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   En cas de connexion en double, connectez-vous à la base de données **master** sans spécifier de catalogue initial, puis exécutez la commande **USE** pour choisir comme catalogue initial la base de données à relation contenant-contenu.  
  
-   Lorsque des bases de données à relation contenant-contenu sont présentes, les utilisateurs de bases de données sans relation contenant-contenu doivent se connecter au [!INCLUDE[ssDE](../../includes/ssde-md.md)] sans utiliser de catalogue initial ou en spécifiant le nom d'une base de données sans relation contenant-contenu comme catalogue initial. Cela évite de se connecter à la base de données à relation contenant-contenu sur laquelle les administrateurs du [!INCLUDE[ssDE](../../includes/ssde-md.md)] exercent un contrôle moins direct.  
  
### <a name="increasing-access-by-changing-the-containment-status-of-a-database"></a>Augmentation de l'accès en modifiant l'état de la relation contenant-contenu d'une base de données  
 Les connexions qui disposent de l’autorisation **ALTER ANY DATABASE** , telles que les membres du rôle serveur fixe **dbcreator** , et les utilisateurs d’une base de données sans relation contenant-contenu qui disposent de l’autorisation **CONTROL DATABASE** , tels que les membres du rôle de base de données fixe **db_owner** , peuvent modifier le paramètre de relation contenant-contenu d’une base de données. Si le paramètre de relation contenant contenu d'une base de données est modifié de **NONE** en **PARTIAL** ou **FULL**, des accès utilisateur peuvent être accordés en créant des utilisateurs de base de données à relation contenant-contenu avec mots de passe. Cela permet de fournir un accès sans le consentement des administrateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou sans qu'ils le sachent. Pour empêcher des bases de données de présenter une relation contenant-contenu, affectez la valeur 0 à l’option [!INCLUDE[ssDE](../../includes/ssde-md.md)]**contained database authentication** . Pour empêcher que des utilisateurs de base de données à relation contenant-contenu avec mots de passe se connectent aux bases de données à relation contenant-contenu sélectionnées, utilisez des déclencheurs de connexion pour annuler leurs tentatives de connexion.  
  
### <a name="attaching-a-contained-database"></a>Attacher une base de données à relation contenant-contenu  
 En attachant une base de données à relation contenant-contenu, un administrateur risque de donner un accès à l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]à des utilisateurs non souhaités. Un administrateur conscient de ce risque peut mettre la base de données en ligne en mode **RESTRICTED_USER** , ce qui empêche l’authentification des utilisateurs de base de données à relation contenant-contenu avec mots de passe. Seuls des principaux autorisés par le biais de connexions seront en mesure d’accéder au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 Les utilisateurs sont créés selon les exigences de mot de passe en vigueur au moment de leur création et les mots de passe ne sont pas revérifiés lorsqu'une base de données est attachée. En attachant une base de données à relation contenant-contenu autorisant des mots de passe faibles sur un système avec une stratégie de mot de passe plus stricte, un administrateur peut autoriser des mots de passe qui ne respectent pas la stratégie de mot de passe actuelle sur le [!INCLUDE[ssDE](../../includes/ssde-md.md)]d’attachement. Les administrateurs peuvent éviter de conserver les mots de passe faibles en exigeant que tous les mots de passe soient réinitialisés pour la base de données attachée.  
  
### <a name="password-policies"></a>Stratégies de mot de passe  
 Les mots de passe d'une base de données peuvent être obligatoirement des mots de passe forts, mais ils ne peuvent pas être protégés par des stratégies de mot de passe fiables. Utilisez l'Authentification Windows chaque fois que possible pour tirer parti des stratégies de mot de passe plus étendues disponibles dans Windows.  
  
### <a name="kerberos-authentication"></a>Authentification Kerberos  
 Les utilisateurs de base de données à relation contenant-contenu avec mots de passe ne peuvent pas utiliser l'Authentification Kerberos. Lorsque c'est possible, utilisez l'Authentification Windows pour tirer parti des fonctionnalités de Windows telles que Kerberos.  
  
### <a name="offline-dictionary-attack"></a>Attaque par dictionnaire hors connexion  
 Les hachages de mot de passe des utilisateurs de base de données à relation contenant-contenu avec mots de passe sont stockés dans la base de données à relation contenant-contenu. Quiconque disposant d'un accès aux fichiers de la base de données pourrait effectuer une attaque par dictionnaire contre des utilisateurs de base de données à relation contenant-contenu avec mots de passe sur un système non vérifié. Pour atténuer cette menace, restreignez l'accès aux fichiers de base de données, ou n'autorisez des connexions aux bases de données à relation contenant-contenu qu'en utilisant l'Authentification Windows.  
  
## <a name="escaping-a-contained-database"></a>S'échapper d'une base de données à relation contenant-contenu  
 Si une base de données est partiellement à relation contenant-contenu, les administrateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent périodiquement auditer les fonctions des utilisateurs et des modules dans les bases de données à relation contenant-contenu.  
  
## <a name="denial-of-service-through-autoclose"></a>Déni de service via AUTO_CLOSE  
 Ne configurez pas de bases de données à relation contenant-contenu pour la fermeture automatique. Si la base de données est fermée, son ouverture pour authentifier un utilisateur consomme des ressources supplémentaires et risque de contribuer à une attaque par déni de service.  
  
## <a name="see-also"></a> Voir aussi  
 [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md)   
 [Migrer vers une base de données partiellement à relation contenant-contenu](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)  
  
  
