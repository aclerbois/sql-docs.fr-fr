---
title: CREATE USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- WITHOUT_LOGIN_TSQL
- CREATE_USER_TSQL
- SQL13.SWB.DATABASEUSER.OWNEDSCHEMAS.F1
- WITHOUT LOGIN
- CREATE USER
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- adding users
- WITHOUT LOGIN [SQL Server]
- CREATE USER statement
- database user additions [SQL Server]
- USER WITHOUT LOGIN [SQL Server]
- users [SQL Server], adding
- users [SQL Server]
ms.assetid: 01de7476-4b25-4d58-85b7-1118fe64aa80
caps.latest.revision: 111
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 0f7d8f9a4eadec915a52c3dfc39ba79f1ab177b9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-user-transact-sql"></a>CREATE USER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Ajoute un utilisateur à la base de données active. Onze types d’utilisateurs sont répertoriés ci-dessous avec un exemple de la syntaxe la plus basique :  
  
**Utilisateurs basés sur les comptes de connexion dans Master** Il s’agit du type d’utilisateur le plus courant.  
  
-   Utilisateur basé sur un compte de connexion basé sur un compte Windows Active Directory. `CREATE USER [Contoso\Fritz];`     
-   Utilisateur basé sur un compte de connexion basé sur un groupe Windows. `CREATE USER [Contoso\Sales];`   
-   Utilisateur basé sur un compte de connexion utilisant l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. `CREATE USER Mary;`  
  
**Utilisateurs qui s’authentifient auprès de la base de données** Recommandé pour accroître la portabilité de votre base de données.  
 Toujours autorisé dans [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. Autorisé uniquement dans une base de données à relation contenant-contenu dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].  
  
-   Utilisateur basé sur un utilisateur Windows qui ne dispose d'aucun compte de connexion. `CREATE USER [Contoso\Fritz];`    
-   Utilisateur basé sur un groupe Windows qui ne dispose d'aucun compte de connexion. `CREATE USER [Contoso\Sales];`  
-   Utilisateur dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ou [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] basé sur un utilisateur Azure Active Directory. `CREATE USER [Contoso\Fritz] FROM EXTERNAL PROVIDER;`     

-   Utilisateur de base de données à relation contenant-contenu avec mot de passe. (Non disponible dans [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].) `CREATE USER Mary WITH PASSWORD = '********';`   
  
**Utilisateurs basés sur des principaux Windows qui se connectent via des comptes de connexion de groupe Windows**  
  
-   Utilisateur basé sur un utilisateur Windows qui ne dispose d'aucun compte de connexion, mais peut se connecter au [!INCLUDE[ssDE](../../includes/ssde-md.md)] via une appartenance à un groupe Windows. `CREATE USER [Contoso\Fritz];`  
  
-   Utilisateur basé sur un groupe Windows qui ne dispose pas de compte de connexion, mais qui peut se connecter au [!INCLUDE[ssDE](../../includes/ssde-md.md)] via l’appartenance à un autre groupe Windows. `CREATE USER [Contoso\Fritz];`  
  
**Utilisateurs qui ne peuvent pas s’authentifier** Ces utilisateurs ne peuvent pas se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
-   Utilisateur sans compte de connexion. Impossible de se connecter, mais peut se voir accorder des autorisations. `CREATE USER CustomApp WITHOUT LOGIN;`    
-   Utilisateur basé sur un certificat. Impossible de se connecter, mais peut se voir accorder des autorisations et peut signer des modules. `CREATE USER TestProcess FOR CERTIFICATE CarnationProduction50;`  
-   Utilisateur basé sur une clé asymétrique. Impossible de se connecter, mais peut se voir accorder des autorisations et peut signer des modules. `CREATE User TestProcess FROM ASYMMETRIC KEY PacificSales09;`   
 
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
-- Syntax Users based on logins in master  
CREATE USER user_name   
    [   
        { FOR | FROM } LOGIN login_name   
    ]  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
--Users that authenticate at the database  
CREATE USER   
    {  
      windows_principal [ WITH <options_list> [ ,... ] ]  
  
    | user_name WITH PASSWORD = 'password' [ , <options_list> [ ,... ]   
    | Azure_Active_Directory_principal FROM EXTERNAL PROVIDER   
    }  
  
 [ ; ]  
  
--Users based on Windows principals that connect through Windows group logins  
CREATE USER   
    {   
          windows_principal [ { FOR | FROM } LOGIN windows_principal ]  
        | user_name { FOR | FROM } LOGIN windows_principal  
}  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
--Users that cannot authenticate   
CREATE USER user_name   
    {  
         WITHOUT LOGIN [ WITH <limited_options_list> [ ,... ] ]  
       | { FOR | FROM } CERTIFICATE cert_name   
       | { FOR | FROM } ASYMMETRIC KEY asym_key_name   
    }  
 [ ; ]  
  
<options_list> ::=  
      DEFAULT_SCHEMA = schema_name  
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }  
    | SID = sid   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name ]   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
-- SQL Database syntax when connected to a federation member  
CREATE USER user_name  
[;]  
```  

```  
-- Syntax for Azure SQL Data Warehouse  
  
CREATE USER user_name   
    [ { { FOR | FROM } { LOGIN login_name }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]

CREATE USER Azure_Active_Directory_principal FROM EXTERNAL PROVIDER  
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]
``` 
  
```  
-- Syntax for Parallel Data Warehouse  
  
CREATE USER user_name   
    [ { { FOR | FROM }  
      {   
        LOGIN login_name   
      }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *user_name*  
 Spécifie le nom qui identifie l'utilisateur dans cette base de données. *user_name* est de type **sysname**. Il peut comporter jusqu'à 128 caractères. Lors de la création d'un utilisateur basé sur un principal Windows, le nom du principal Windows devient le nom d'utilisateur sauf si un autre nom d'utilisateur est spécifié.  
  
 LOGIN *login_name*  
 Spécifie le compte de connexion pour lequel l'utilisateur de base de données est créé. *login_name* doit être un compte de connexion valide sur le serveur. Peut être un compte de connexion basé sur un principal Windows (utilisateur ou groupe), ou un compte de connexion utilisant l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque ce compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accède à la base de données, il prend le nom et l'ID de l'utilisateur de base de données que vous créez. Quand vous créez un compte de connexion mappé à partir d’un principal Windows, utilisez le format **[***\<NomDomaine>***\\***\<NomCompteConnexion>***]**. Pour obtenir des exemples, consultez [Résumé de syntaxe](#SyntaxSummary).  
  
 Si l'instruction CREATE USER est la seule instruction d'un lot SQL, Microsoft Azure SQL Database prend en charge la clause WITH LOGIN. Si l'instruction CREATE USER n'est pas la seule instruction d'un lot SQL ou est exécutée en SQL dynamique, la clause WITH LOGIN n'est pas prise en charge.  
  
 WITH DEFAULT_SCHEMA = *schema_name*  
 Spécifie le premier schéma que le serveur doit interroger pour résoudre les noms des objets associés à cet utilisateur de base de données.  
  
 '*windows_principal*'  
 Spécifie le principal Windows pour lequel l'utilisateur de la base de données est créé. *windows_principal* peut être un utilisateur Windows ou un groupe Windows. L’utilisateur est créé même si le *windows_principal* ne dispose pas de compte de connexion. Lors de la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si *windows_principal* ne dispose pas de compte de connexion, le principal Windows doit s’authentifier auprès du [!INCLUDE[ssDE](../../includes/ssde-md.md)] via l’appartenance à un groupe Windows qui dispose d’un compte de connexion, ou la chaîne de connexion doit spécifier la base de données à relation contenant-contenu comme catalogue initial. Quand vous créez un utilisateur à partir d’un principal Windows, utilisez le format **[***\<NomDomaine>***\\***\<NomCompteConnexion>***]**. Pour obtenir des exemples, consultez [Résumé de syntaxe](#SyntaxSummary). Les utilisateurs basés sur des utilisateurs Active Directory sont limités aux noms de moins de 21 caractères.    
  
 '*Azure_Active_Directory_principal*'  
 **S’applique à** : [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].  
  
 Spécifie le principal Azure Active Directory pour lequel l’utilisateur de la base de données est créé. *Azure_Active_Directory_principal* peut être un utilisateur Azure Active Directory ou un groupe Azure Active Directory. (Les utilisateurs Azure Active Directory ne peuvent pas avoir de comptes de connexion d’authentification Windows [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ; seuls les utilisateurs de base de données le peuvent.) La chaîne de connexion doit spécifier la base de données à relation contenant-contenu comme catalogue initial. 

 Pour les utilisateurs, vous utilisez l’alias complet de leur principal de domaine.   
 
-   `CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;`  
  
-   `CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;`

 Pour les groupes de sécurité, vous utilisez le *nom d’affichage* du groupe de sécurité. Pour le groupe de sécurité *Nurses*, vous devez utiliser le code suivant :  
  
-   `CREATE USER [Nurses] FROM EXTERNAL PROVIDER;`  
  
 Pour plus d’informations, voir [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication).  
  
WITH PASSWORD = '*password*'  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Peut être utilisé uniquement dans une base de données à relation contenant-contenu. Spécifie le mot de passe de l'utilisateur en cours de création. Depuis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les informations de mot de passe stockées sont calculées à l’aide de la valeur salt SHA-512 du mot de passe.  
  
WITHOUT LOGIN  
 Indique que l'utilisateur ne doit pas être mappé à une connexion existante.  
  
CERTIFICATE *cert_name*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Spécifie le certificat pour lequel l'utilisateur de base de données est créé.  
  
ASYMMETRIC KEY *asym_key_name*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Spécifie la clé asymétrique pour laquelle l'utilisateur de base de données est créé.  
  
DEFAULT_LANGUAGE = *{ NONE | \<lcid> | \<language name> | \<language alias> }*  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Spécifie la langue par défaut du nouvel utilisateur. Si une langue par défaut est spécifiée pour l'utilisateur et que la langue par défaut de la base de données est changée ultérieurement, la langue par défaut des utilisateurs reste comme spécifié. Si aucune langue par défaut n'est spécifiée, la langue par défaut de l'utilisateur correspondra à la langue par défaut de la base de données. Si la langue par défaut n'est pas spécifiée pour l'utilisateur et que la langue par défaut de la base de données est changée ultérieurement, la langue par défaut de l'utilisateur est remplacée par la nouvelle langue par défaut de la base de données.  
  
> [!IMPORTANT]  
>  *DEFAULT_LANGUAGE* est utilisé uniquement pour un utilisateur de base de données à relation contenant-contenu.  
  
SID = *sid*  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 S'applique uniquement aux utilisateurs munis de mots de passe (authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) dans une base de données à relation contenant-contenu. Spécifie le SID du nouvel utilisateur de base de données. Si cette option n'est pas sélectionnée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribue automatiquement un SID. Utilisez le paramètre SID pour créer des utilisateurs dans plusieurs bases de données qui ont la même identité (SID). Cela s’avère utile lors de la création d’utilisateurs dans plusieurs bases de données pour la préparation du basculement AlwaysOn. Pour déterminer le SID d’un utilisateur, interrogez sys.database_principals.  
  
ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ] ]  
 **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Supprime les contrôles de métadonnées de chiffrement sur le serveur dans les opérations de copie en bloc. Cela permet à l’utilisateur de copier en bloc des données chiffrées entre des tables ou des bases de données, sans déchiffrer les données. La valeur par défaut est OFF.  
  
> [!WARNING]  
>  Une utilisation incorrecte de cette option peut entraîner une altération des données. Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
## <a name="remarks"></a>Notes   
 Si vous omettez FOR LOGIN, le nouvel utilisateur de base de données est mappé à la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du même nom.  
  
 Le schéma par défaut correspond au premier schéma que le serveur doit interroger pour résoudre les noms des objets associés à cet utilisateur de base de données. Sauf spécification contraire, le schéma par défaut sera le propriétaire des objets créés par cet utilisateur de la base de données.  
  
 Si l'utilisateur possède un schéma par défaut, ce schéma par défaut est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, mais qu'il est membre d'un groupe qui dispose d'un schéma par défaut, le schéma par défaut du groupe est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, et qu'il est membre de plus d'un groupe, le schéma par défaut de l'utilisateur sera celui du groupe Windows avec le principal_id le plus bas et un schéma par défaut défini explicite. (Il n'est pas possible de sélectionner explicitement l'un des schémas par défaut disponibles comme schéma préférentiel.) Si aucun schéma par défaut ne peut être déterminé pour un utilisateur, le schéma **dbo** est utilisé.  
  
 Il est possible de définir DEFAULT_SCHEMA avant de créer le schéma vers lequel il pointe.  
  
 Vous ne pouvez pas spécifier DEFAULT_SCHEMA lorsque vous créez un utilisateur mappé à un certificat ou à une clé asymétrique.  
  
 La valeur de DEFAULT_SCHEMA est ignorée si l'utilisateur est membre du rôle serveur fixe sysadmin. Tous les membres du rôle serveur fixe sysadmin possèdent le schéma par défaut `dbo`.  
  
 La clause WITHOUT LOGIN crée un utilisateur qui n'est pas mappé à un compte de connexion SQL Server. Il peut se connecter à d'autres bases de données en tant qu'invité. Les autorisations peuvent être attribuées à cet utilisateur sans compte de connexion et lorsque le contexte de sécurité est modifié en utilisateur sans compte de connexion, les utilisateurs d'origine reçoivent les autorisations de l'utilisateur sans compte de connexion. Consultez l’exemple [D. Création et utilisation d’un utilisateur sans compte de connexion](#withoutLogin).  
  
 Seuls les utilisateurs mappés à des principaux Windows peuvent contenir la barre oblique inverse (**\\**).  
  
 Vous ne pouvez pas utiliser CREATE USER pour créer un utilisateur invité, car ce dernier existe déjà dans toutes les bases de données. Vous pouvez activer l'utilisateur invité en lui accordant l'autorisation CONNECT comme suit :  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
 Les informations relatives aux utilisateurs de base de données sont consultables dans la vue de catalogue [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md).  
  
##  <a name="SyntaxSummary"></a> Résumé de syntaxe  
 **Utilisateurs basés sur des comptes de connexion dans Master**  
  
 La liste suivante affiche la syntaxe possible pour les utilisateurs basés sur des comptes de connexion. Les options de schéma par défaut ne sont pas répertoriées.  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FOR LOGIN SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FROM LOGIN SQLAUTHLOGIN`  
  
**Utilisateurs qui s’authentifient auprès de la base de données**  
  
 La liste suivante affiche la syntaxe possible pour les utilisateurs qui peuvent être utilisés uniquement dans une base de données à relation contenant-contenu. Les utilisateurs créés ne seront liés à aucun compte de connexion dans la base de données **Master**. Les options de langue et de schéma par défaut ne sont pas répertoriées.  
  
> [!IMPORTANT]  
>  Cette syntaxe accorde aux utilisateurs un accès à la base de données, ainsi qu’un nouvel accès au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER Barry WITH PASSWORD = 'sdjklalie8rew8337!$d'`  
  
**Utilisateurs basés sur des principaux Windows sans comptes de connexion dans Master**  
  
 La liste suivante affiche la syntaxe possible pour les utilisateurs qui ont accès au [!INCLUDE[ssDE](../../includes/ssde-md.md)] via un groupe Windows, mais qui ne disposent pas d’un compte de connexion dans **Master**. Cette syntaxe peut être utilisée dans tous les types de bases de données. Les options de langue et de schéma par défaut ne sont pas répertoriées.  
  
 Cette syntaxe est semblable aux utilisateurs basés sur des comptes de connexion dans master, mais cette catégorie d'utilisateurs ne dispose pas de compte de connexion dans master. L’utilisateur doit avoir accès au [!INCLUDE[ssDE](../../includes/ssde-md.md)] via un compte de connexion de groupe Windows.  
  
 Cette syntaxe est semblable à celle utilisées pour les utilisateurs de base de données à relation contenant-contenu basés sur des principaux Windows, mais cette catégorie d’utilisateurs n’obtient pas un nouvel accès au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
  
**Utilisateurs qui ne peuvent pas s’authentifier**  
  
 La liste suivante affiche la syntaxe possible pour les utilisateurs qui ne peuvent pas se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   `CREATE USER RIGHTSHOLDER WITHOUT LOGIN`  
-   `CREATE USER CERTUSER FOR CERTIFICATE SpecialCert`  
-   `CREATE USER CERTUSER FROM CERTIFICATE SpecialCert`  
-   `CREATE USER KEYUSER FOR ASYMMETRIC KEY SecureKey`  
-   `CREATE USER KEYUSER FROM ASYMMETRIC KEY SecureKey`  
  
## <a name="security"></a>Sécurité  
 La création d'un utilisateur accorde l'accès à une base de données, mais n'accorde pas automatiquement l'accès aux objets d'une base de données. Après avoir créé un utilisateur, les actions communes consistent à ajouter des utilisateurs aux rôles de base de données qui ont l'autorisation d'accéder aux objets de base de données ou d'octroyer des autorisations relatives aux objets à l'utilisateur. Pour plus d’informations sur la conception d’un système d’autorisations, voir [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
### <a name="special-considerations-for-contained-databases"></a>Considérations spéciales pour les bases de données à relation contenant-contenu  
 Lors de la connexion à une base de données à relation contenant-contenu, si l’utilisateur ne dispose pas de compte de connexion dans la base de données **Master**, la chaîne de connexion doit inclure le nom de la base de données à relation contenant-contenu comme catalogue initial. Le paramètre de catalogue initial est toujours requis pour un utilisateur de base de données à relation contenant-contenu avec mot de passe.  
  
 Dans une base de données à relation contenant-contenu, la création d’utilisateurs permet de séparer la base de données de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] afin que la base de données puisse être déplacée facilement vers une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md) et [Utilisateurs de base de données à relation contenant-contenu - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md). Pour changer un utilisateur de base de données basé sur un compte de connexion d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisateur de base de données à relation contenant-contenu avec mot de passe, consultez [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).  
  
 Dans une base de données à relation contenant-contenu, les utilisateurs n’ont pas besoin de disposer d’un compte de connexion dans la base de données **Master**. Les administrateurs du [!INCLUDE[ssDE](../../includes/ssde-md.md)] doivent comprendre que l'accès à une base de données à relation contenant-contenu peut être accordé au niveau de la base de données, plutôt qu'au niveau du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Pour plus d'informations, consultez [Bonnes pratiques de sécurité avec les bases de données à relation contenant-contenu](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
 En cas d'utilisateurs de base de données à relation contenant-contenu [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], configurez l'accès à l'aide d'une règle de pare-feu de niveau base de données, et non d'une règle de pare-feu de niveau serveur. Pour plus d’informations, consultez [sp_set_database_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md).
 
Pour les utilisateurs de base de données à relation contenant-contenu [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)], SSMS peut prendre en charge l’authentification multifacteur. Pour plus d’informations, consultez [Prise en charge SSMS pour Azure AD MFA avec SQL Database et SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-database-ssms-mfa-authentication/).  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY USER sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-database-user-based-on-a-sql-server-login"></a>A. Création d'un utilisateur de base de données basé sur un compte de connexion SQL Server  
 L'exemple suivant crée d'abord un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommé `AbolrousHazem`, puis crée un utilisateur de base de données correspondant nommé `AbolrousHazem` dans `AdventureWorks2012`.  
  
```  
CREATE LOGIN AbolrousHazem   
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
```   
Changez de base de données utilisateur. Par exemple, dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez l’instruction `USE AdventureWorks2012`. Dans [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], vous devez établir une nouvelle connexion à la base de données utilisateur.

```   
CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
GO   
```  
  
### <a name="b-creating-a-database-user-with-a-default-schema"></a>B. Création d'un utilisateur de base de données avec un schéma par défaut  
 L'exemple suivant crée d'abord une connexion serveur nommée `WanidaBenshoof` avec un mot de passe, puis crée un utilisateur de base de données correspondant nommé `Wanida` avec le schéma par défaut `Marketing`.  
  
```  
CREATE LOGIN WanidaBenshoof   
    WITH PASSWORD = '8fdKJl3$nlNv3049jsKK';  
USE AdventureWorks2012;  
CREATE USER Wanida FOR LOGIN WanidaBenshoof   
    WITH DEFAULT_SCHEMA = Marketing;  
GO  
```  
  
### <a name="c-creating-a-database-user-from-a-certificate"></a>C. Création d'un utilisateur de base de données à partir d'un certificat  
 L'exemple suivant crée un utilisateur de base de données `JinghaoLiu` à partir du certificat `CarnationProduction50`.  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
USE AdventureWorks2012;  
CREATE CERTIFICATE CarnationProduction50  
    WITH SUBJECT = 'Carnation Production Facility Supervisors',  
    EXPIRY_DATE = '11/11/2011';  
GO  
CREATE USER JinghaoLiu FOR CERTIFICATE CarnationProduction50;  
GO   
```  
  
###  <a name="withoutLogin"></a> D. Création et utilisation d'un utilisateur sans connexion  
 L'exemple suivant crée un utilisateur de base de données `CustomApp` qui n'est mappé à aucune connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'exemple accorde ensuite à un utilisateur `adventure-works\tengiz0` l'autorisation d'emprunter l'identité de l'utilisateur `CustomApp`.  
  
```  
USE AdventureWorks2012 ;  
CREATE USER CustomApp WITHOUT LOGIN ;  
GRANT IMPERSONATE ON USER::CustomApp TO [adventure-works\tengiz0] ;  
GO   
```  
  
 Pour utiliser les informations d'identification `CustomApp`, l'utilisateur `adventure-works\tengiz0` exécute l'instruction suivante.  
  
```  
EXECUTE AS USER = 'CustomApp' ;  
GO  
```  
  
 Pour revenir aux informations d'identification `adventure-works\tengiz0`, l'utilisateur exécute l'instruction suivante.  
  
```  
REVERT ;  
GO  
```  
  
### <a name="e-creating-a-contained-database-user-with-password"></a>E. Création d'un utilisateur de base de données à relation contenant-contenu avec mot de passe  
 L'exemple suivant crée un utilisateur de base de données à relation contenant-contenu avec mot de passe. Cet exemple ne peut être exécuté que dans une base de données à relation contenant-contenu.  
  
**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Cet exemple fonctionne dans [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] si DEFAULT_LANGUAGE est supprimé.  
  
```  
USE AdventureWorks2012 ;  
GO  
CREATE USER Carlo  
WITH PASSWORD='RN92piTCh%$!~3K9844 Bl*'  
    , DEFAULT_LANGUAGE=[Brazilian]  
    , DEFAULT_SCHEMA=[dbo]  
GO   
```  
  
### <a name="f-creating-a-contained-database-user-for-a-domain-login"></a>F. Création d'un utilisateur de base de données à relation contenant-contenu pour un compte de connexion de domaine  
 L'exemple suivant crée un utilisateur de base de données à relation contenant-contenu pour une connexion nommée Fritz dans un domaine appelé Contoso. Cet exemple ne peut être exécuté que dans une base de données à relation contenant-contenu.  
  
**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
USE AdventureWorks2012 ;  
GO  
CREATE USER [Contoso\Fritz] ;  
GO   
```  
  
### <a name="g-creating-a-contained-database-user-with-a-specific-sid"></a>G. Création d'un utilisateur de base de données à relation contenant-contenu avec un SID spécifique  
 L'exemple suivant crée un utilisateur de base de données à relation contenant-contenu authentifié par SQL Server et nommé CarmenW. Cet exemple ne peut être exécuté que dans une base de données à relation contenant-contenu.  
  
**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
USE AdventureWorks2012 ;  
GO  
CREATE USER CarmenW WITH PASSWORD = 'a8ea v*(Rd##+'  
, SID = 0x01050000000000090300000063FF0451A9E7664BA705B10E37DDC4B7;  
  
```  
  
### <a name="h-creating-a-user-to-copy-encrypted-data"></a>H. Création d’un utilisateur pour copier des données chiffrées  
 L’exemple suivant crée un utilisateur qui peut copier des données protégées par la fonctionnalité Always Encrypted d’un ensemble de tables contenant des colonnes chiffrées vers un autre ensemble de tables avec des colonnes chiffrées (dans la même base de données ou dans une autre base de données).  Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```  
CREATE USER [Chin]   
WITH   
      DEFAULT_SCHEMA = dbo  
    , ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = ON ;  
```  
  

## <a name="next-steps"></a>Étapes suivantes  
Une fois l’utilisateur créé, envisagez d’ajouter l’utilisateur à un rôle de base de données à l’aide de l’instruction [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md).  
Vous pouvez également [octroyer (GRANT) des autorisations sur un objet](../../t-sql/statements/grant-object-permissions-transact-sql.md) au rôle afin qu’il puisse accéder aux tables. Pour obtenir des informations générales sur le modèle de sécurité SQL Server, consultez [Autorisations](../../relational-databases/security/permissions-database-engine.md).   
  
## <a name="see-also"></a> Voir aussi  
 [Créer un utilisateur de base de données](../../relational-databases/security/authentication-access/create-a-database-user.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md)   
 [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication)   
 [Prise en main des autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
  


