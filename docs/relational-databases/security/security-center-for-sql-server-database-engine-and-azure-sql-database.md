---
title: Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database | Microsoft Docs
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.component: security
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- SQL Server, security
- security [SQL Server]
- database security [SQL Server]
- databases [SQL Server], security
ms.assetid: dfb39d16-722a-4734-94bb-98e61e014ee7
caps.latest.revision: 55
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3ebdcbdde0a274df0240b6e7d182ce600bd62ed7
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708097"
---
# <a name="security-center-for-sql-server-database-engine-and-azure-sql-database"></a>Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Cette page fournit des liens qui vous aideront à localiser les informations dont vous avez besoin sur la sécurité et la protection dans le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et la [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 **Légende**  
  
 ![security-center-legend](../../relational-databases/performance/media/security-center-legend.PNG "security-center-legend")  
  
##  <a name="Who"></a> Authentification : qui êtes-vous ?  
  
|||  
|-|-|  
|**Qui effectue l'authentification ?**<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Authentification Windows<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentification<br /><br /> ![security-center-sqldb](../../relational-databases/security/media/security-center-sqldb.png "security-center-sqldb") Azure Active Directory|Qui effectue l'authentification ? (Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])<br /><br /> [Choisir un mode d'authentification](../../relational-databases/security/choose-an-authentication-mode.md)<br /><br /> [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/)|  
|**Où est effectuée l’authentification ?**<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Auprès de la base de données MASTER : connexions et utilisateurs de base de données<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Auprès de la base de données utilisateur : utilisateurs de base de données à relation contenant-contenu|S’authentifier auprès de la base de données MASTER (connexions et utilisateurs de base de données)<br /><br /> [Créer une connexion SQL Server](../../relational-databases/security/authentication-access/create-a-login.md)<br /><br /> [Gestion des bases de données et des connexions dans Azure SQL Database](http://msdn.microsoft.com/library/ee336235.aspx)<br /><br /> [Créer un utilisateur de base de données](../../relational-databases/security/authentication-access/create-a-database-user.md)<br /><br /> <br /><br /> S'authentifier auprès d'une base de données utilisateur<br /><br /> [Utilisateurs de base de données à relation contenant-contenu - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md)|  
|**Utilisation d'autres identités**<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Informations d’identification<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Exécuter en tant qu’autre connexion<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Exécuter en tant qu’autre utilisateur de base de données|[Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)<br /><br /> [Exécuter en tant qu'autre connexion](../../t-sql/statements/execute-as-transact-sql.md)<br /><br /> [Exécuter en tant qu'autre utilisateur de base de données](../../t-sql/statements/execute-as-transact-sql.md)|  
  
##  <a name="What"></a> Autorisation : que pouvez-vous faire ?  
  
|||  
|-|-|  
|**Octroi, révocation et refus d'autorisations**<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Classes sécurisables<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Autorisations de serveur granulaires<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Autorisations de base de données granulaires|[Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)<br /><br /> [Autorisations](../../relational-databases/security/permissions-database-engine.md)<br /><br /> [Éléments sécurisables](../../relational-databases/security/securables.md)<br /><br /> [Prise en main des autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)|  
|**Sécurité par rôles**<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Rôles de niveau serveur<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Rôles de niveau base de données|[Rôles de niveau serveur](../../relational-databases/security/authentication-access/server-level-roles.md)<br /><br /> [Rôles au niveau de la base de données](../../relational-databases/security/authentication-access/database-level-roles.md)|  
|**Restriction de l'accès aux données aux éléments de données sélectionnés**<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Limiter l’accès aux données à l’aide de vues/procédures<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Sécurité au niveau des lignes<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Dynamic Data Masking<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Objets signés|Limiter l'accès aux données à l'aide de [Vues](../../relational-databases/views/views.md) et de [Procédures](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)<br /><br /> [Sécurité au niveau des lignes (SQL Server)](../../relational-databases/security/row-level-security.md)<br /><br /> [sécurité au niveau de la ligne (base de données SQL Azure)](http://msdn.microsoft.com/library/azure/dn765131.aspx)<br /><br /> [Masquage dynamique des données (SQL Server)](../../relational-databases/security/dynamic-data-masking.md)<br /><br /> [Masquage dynamique des données (base de données SQL Azure)](http://azure.microsoft.com/documentation/articles/sql-database-dynamic-data-masking-get-started/)<br /><br /> [Objets signés](../../t-sql/statements/add-signature-transact-sql.md)|  
  
##  <a name="Encrypt"></a> Chiffrement : stockage de données secrètes  
  
|||  
|-|-|  
|**Chiffrement de fichiers**<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Chiffrement BitLocker (au niveau du lecteur)<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Chiffrement NTFS (au niveau du dossier)<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Chiffrement Transparent Data Encryption (au niveau du fichier)<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Chiffrement de sauvegarde (au niveau du fichier)|[BitLocker (au niveau du lecteur)](http://support.microsoft.com/kb/2855131)<br /><br /> [Chiffrement NTFS (au niveau du dossier)](http://msdn.microsoft.com/library/dd163562.aspx)<br /><br /> [Chiffrement transparent des données (au niveau du fichier)](../../relational-databases/security/encryption/transparent-data-encryption.md)<br /><br /> [Chiffrement de la sauvegarde (au niveau du fichier)](../../relational-databases/backup-restore/backup-encryption.md)|  
|**Chiffrement des sources**<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Module Gestion de clés extensible<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Clés stockées dans le coffre de clés Azure Key Vault<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Always Encrypted|[Module Gestion de clés extensible](../../relational-databases/security/encryption/extensible-key-management-ekm.md)<br /><br /> [Clés stockées dans le coffre de clés Azure](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)<br /><br /> [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)|  
|**Colonne, données et chiffrement à clé**<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Chiffrer par certificat<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Chiffrer par clé symétrique<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Chiffrer par clé asymétrique<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Chiffrer par phrase secrète|[Chiffrer par certificat](../../t-sql/functions/encryptbycert-transact-sql.md)<br /><br /> [Chiffrer par clé asymétrique](../../t-sql/functions/encryptbyasymkey-transact-sql.md)<br /><br /> [Chiffrer par clé symétrique](../../t-sql/functions/encryptbykey-transact-sql.md)<br /><br /> [Chiffrer par mot de passe](../../t-sql/functions/encryptbypassphrase-transact-sql.md)<br /><br /> [Chiffrer une colonne de données](../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
##  <a name="Connect"></a> Sécurité de connexion : restriction et sécurisation  
  
|||  
|-|-|  
|**Protection par pare-feu**<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Paramètres de Pare-feu Windows<br /><br /> ![security-center-sqldb](../../relational-databases/security/media/security-center-sqldb.png "security-center-sqldb") Paramètres de pare-feu du service Azure<br /><br /> ![security-center-sqldb](../../relational-databases/security/media/security-center-sqldb.png "security-center-sqldb") Paramètres de pare-feu de la base de données|[Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)<br /><br /> [Paramètres de pare-feu Azure SQL Database](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)<br /><br /> [Paramètres de pare-feu du service Azure](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)|  
|**Chiffrement des données en transit**<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Connexions SSL forcées<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") Connexions SSL facultatives|[SSL (Secure Sockets Layer) pour le moteur de base de données](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)<br /><br /> [SSL (Secure Sockets Layer) pour SQL Database](https://msdn.microsoft.com/library/azure/ff394108.aspx)<br /><br /> [Prise en charge de TLS 1.2 pour Microsoft SQL Server](https://support.microsoft.com/kb/3135244)|  
  
##  <a name="Audit"></a> Audit : enregistrement de l’accès  
  
|||  
|-|-|  
|**Audit automatisé**<br /><br /> ![security-center-sqlserver](../../relational-databases/performance/media/security-center-sqlserver.png "security-center-sqlserver") [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit (au niveau du serveur et de la base de données)<br /><br /> ![security-center-sqldb](../../relational-databases/security/media/security-center-sqldb.png "security-center-sqldb") [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Audit (au niveau de la base de données)<br /><br /> ![security-center-sqldb](../../relational-databases/security/media/security-center-sqldb.png "security-center-sqldb") Détection des menaces| <br /><br /> [SQL Server Audit &#40moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)<br /><br /> [Audit SQL Database](http://azure.microsoft.com/documentation/articles/sql-database-auditing-get-started/)<br /><br /> [Prise en main de la fonction de détection des menaces de la base de données SQL](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/) <br /><br /> [SQL Database Vulnerability Assessment](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) |  
|**Audit personnalisé**<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Déclencheurs|Implémentation d’un audit personnalisé : création de [déclencheurs DDL](../../relational-databases/triggers/ddl-triggers.md) et de [déclencheurs DML](../../relational-databases/triggers/dml-triggers.md)|  
|**Conformité**<br /><br /> ![security-center-both](../../relational-databases/performance/media/security-center-both.png "security-center-both") Conformité|SQL Server :<br />                        [Critères courants](http://go.microsoft.com/fwlink/?LinkId=616319)<br /><br /> Base de données SQL :<br />                        [Microsoft Azure Trust Center : Conformité par fonctionnalité](http://azure.microsoft.com/support/trust-center/services/)|  
  
##  <a name="SQLInjection"></a> Injection SQL  
 Une attaque par injection SQL consiste à insérer un code malveillant dans les chaînes transmises ultérieurement à [!INCLUDE[ssDE](../../includes/ssde-md.md)] en vue de leur analyse et de leur exécution. Les procédures qui permettent de créer des instructions SQL doivent être vérifiées et analysées à la recherche d'éventuelles failles autorisant cette injection, car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute toutes les requêtes syntaxiquement correctes qu'il reçoit. Tous les systèmes de base de données peuvent être la cible d’une attaque par injection SQL, et la plupart des vulnérabilités sont introduites dans l’application qui interroge [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Vous pouvez déjouer les attaques par injection SQL en utilisant des procédures stockées et des commandes paramétrées, en évitant les instructions SQL dynamiques et en limitant les autorisations de l’ensemble des utilisateurs.  Pour plus d’informations, consultez [SQL Injection](../../relational-databases/security/sql-injection.md).  
  
 Liens supplémentaires pour les programmeurs d’applications :  
  
-   [Scénarios de sécurité des applications dans SQL Server](https://msdn.microsoft.com/library/bb669057.aspx)  
  
-   [Écriture d’une instruction SQL dynamique sécurisée dans SQL Server](https://msdn.microsoft.com/library/bb669091.aspx)  
  
-   [Procédure : se protéger contre l'injection de code SQL dans ASP.NET (page éventuellement en anglais)](https://msdn.microsoft.com/library/ff648339.aspx)  
  
## <a name="see-also"></a> Voir aussi  
 [Prise en main des autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)   
 [Sécurisation de SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Certificats et clés asymétriques SQL Server](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)   
 [Chiffrement SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md)   
 [Mots de passe forts](../../relational-databases/security/strong-passwords.md)   
 [Propriété de base de données TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md)   
 [Fonctionnalités et tâches du moteur de base de données](http://msdn.microsoft.com/library/d9efe145-3306-4d61-bd77-e2af43e19c34)  
 [Protection de votre propriété intellectuelle SQL Server](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
  
