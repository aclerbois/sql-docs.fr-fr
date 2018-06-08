---
title: CREATE LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREATE_LOGIN_TSQL
- CREATE LOGIN
- LOGIN_TSQL
- LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- passwords [SQL Server], logins
- mapping logins [SQL Server]
- logins [SQL Server], creating
- CREATE LOGIN statement
- permissions [SQL Server], logins
- Windows domain accounts [SQL Server]
- re-hashing passwords
- certificates [SQL Server], logins
ms.assetid: eb737149-7c92-4552-946b-91085d8b1b01
caps.latest.revision: 101
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a50bc5103cfee248bbbbefb3ebd6740fc9179958
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-login-transact-sql"></a>CREATE LOGIN (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Crée une connexion de [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server  
  
CREATE LOGIN login_name { WITH <option_list1> | FROM <sources> }  
  
<option_list1> ::=   
    PASSWORD = { 'password' | hashed_password HASHED } [ MUST_CHANGE ]  
    [ , <option_list2> [ ,... ] ]  
  
<option_list2> ::=    
    SID = sid  
    | DEFAULT_DATABASE = database      
    | DEFAULT_LANGUAGE = language  
    | CHECK_EXPIRATION = { ON | OFF}  
    | CHECK_POLICY = { ON | OFF}  
    | CREDENTIAL = credential_name   
  
<sources> ::=  
    WINDOWS [ WITH <windows_options>[ ,... ] ]  
    | CERTIFICATE certname  
    | ASYMMETRIC KEY asym_key_name  
  
<windows_options> ::=        
    DEFAULT_DATABASE = database  
    | DEFAULT_LANGUAGE = language  
```  
  
```  
-- Syntax for Azure SQL Database and Azure SQL Data Warehouse  
  
CREATE LOGIN login_name  
 { WITH <option_list3> }  
  
<option_list3> ::=   
    PASSWORD = { 'password' }  
    [ SID = sid ]  
```  
  
```  
-- Syntax for Parallel Data Warehouse  
  
CREATE LOGIN loginName { WITH <option_list1> | FROM WINDOWS }  
  
<option_list1> ::=   
    PASSWORD = { 'password' } [ MUST_CHANGE ]  
    [ , <option_list2> [ ,... ] ]  
  
<option_list2> ::=    
      CHECK_EXPIRATION = { ON | OFF}  
    | CHECK_POLICY = { ON | OFF}  
```  
  
## <a name="arguments"></a>Arguments  
 *login_name*  
 Spécifie le nom de la connexion créée. Il existe quatre types de connexions : connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], connexions Windows, connexions mappées par certificat et connexions mappées par clé asymétrique. Quand vous créez des connexions mappées à partir d’un compte de domaine Windows, vous devez utiliser le nom d’ouverture de session de l’utilisateur antérieur à Windows 2000 en respectant le format [\<domainName>\\<login_name>]. Vous ne pouvez pas utiliser un nom UPN au format login_name@DomainName. Consultez l'exemple D plus loin dans cette rubrique. Les connexions d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont de type **sysname** et doivent se conformer aux règles applicables aux [identificateurs](http://msdn.microsoft.com/library/ms175874.aspx) et ne peuvent pas contenir de barre oblique inverse (**\\**). Les connexions Windows peuvent contenir une barre oblique inverse (**\\**). Les connexions basées sur les utilisateurs Active Directory sont limitées aux noms de moins de 21 caractères.  
  
 PASSWORD **='***password***'**  
 S’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie le mot de passe de la connexion à créer. Il est recommandé d'utiliser un mot de passe fort. Pour plus d’informations, consultez [Mots de passe forts](../../relational-databases/security/strong-passwords.md) et [Stratégie de mot de passe](../../relational-databases/security/password-policy.md). Depuis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les informations de mot de passe stockées sont calculées à l’aide de la valeur salt SHA-512 du mot de passe.  
  
 Les mots de passe respectent la casse. Les mots de passe doivent comporter au moins 8 caractères, et ne peuvent pas dépasser 128 caractères.  Les mots de passe peuvent inclure les caractères de A à Z, en minuscules ou en majuscules, les chiffres de 0 à 9 et la plupart des caractères non alphanumériques. Les mots de passe ne peuvent pas contenir de guillemets simples, ni le *login_name*.  
  
 PASSWORD **=***hashed_password*  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 S'applique uniquement au mot clé HASHED. Spécifie la valeur hachée du mot de passe de la connexion créée.  
  
 HASHED  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 S’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie que le mot de passe entré après l'argument PASSWORD est déjà haché. Si cette option n'est pas sélectionnée, la chaîne de caractères entrée comme mot de passe est hachée avant d'être stockée dans la base de données. Cette option doit être utilisée uniquement pour effectuer une migration de bases de données d'un serveur vers un autre. N'utilisez pas l'option HASHED pour créer des connexions. L'option HASHED ne peut pas être utilisée avec des hachages créés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7 ou antérieur,  
  
 MUST_CHANGE  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 S’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous incluez cette option, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] demande à l'utilisateur un nouveau mot de passe lors de la première utilisation de la nouvelle connexion.  
  
 CREDENTIAL **=***credential_name*  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Nom des informations d'identification à associer à la nouvelle connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les informations d'identification doivent déjà exister sur le serveur. À l'heure actuelle, cette option lie uniquement l'information d'authentification à une connexion. Les informations d’identification ne peuvent pas être mappées à la connexion de l’administrateur système.  
  
 SID = *sid*  
 Utilisé pour recréer une connexion. S’applique aux connexions d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] uniquement, et non aux connexions d’authentification Windows. Spécifie le SID de la nouvelle connexion d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si cette option n’est pas sélectionnée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribue automatiquement un SID. La structure SID dépend de la version [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   SID de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : valeur littérale 16 octets (**binary(16)**) basée sur un GUID. Par exemple, `SID = 0x14585E90117152449347750164BA00A7`.  
  
-   SID de connexion [!INCLUDE[ssSDS](../../includes/sssds-md.md)] : structure SID valide pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Il s’agit généralement d’un littéral 32 octets (**binary(32)**) composé de `0x01060000000000640000000000000000` plus 16 octets représentant un GUID. Par exemple, `SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`.  
  
DEFAULT_DATABASE **=***database*  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie la base de données par défaut à affecter à la connexion. Si cette option est omise, la base de données par défaut est master.  
  
DEFAULT_LANGUAGE **=***language*  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie la langue par défaut à affecter à la connexion. Si cette option est omise, la langue par défaut est la langue par défaut actuellement définie pour le serveur. Si la langue par défaut du serveur est changée par la suite, la langue par défaut de la connexion reste la même.  
  
CHECK_EXPIRATION **=** { ON | **OFF** }  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 S’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie si les règles d'expiration des mots de passe doivent être imposées sur cette connexion. La valeur par défaut est OFF.  
  
CHECK_POLICY **=** { **ON** | OFF }  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 S’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie que les stratégies de mot de passe Windows de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doivent être imposées sur cette connexion. La valeur par défaut est ON.  
  
 Si la stratégie windows requiert des mots de passe forts, les mots de passe doivent avoir au moins trois des quatre caractéristiques suivantes :  
  
-   Une majuscule (A-Z).  
-   Une minuscule (a-z).  
-   Un chiffre (0-9).  
-   Un des caractères non alphanumériques, tels qu'un espace, _, @, *, ^, % ! , $, # ou &.  
  
WINDOWS  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie que la connexion doit être mappée sur une connexion Windows.  
  
CERTIFICATE *certname*  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie le nom d'un certificat à associer à cette connexion. Ce certificat doit déjà se trouver dans la base de données master.  
  
ASYMMETRIC KEY *asym_key_name*  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie le nom d'une clé asymétrique à associer à cette connexion. Cette clé doit déjà se trouver dans la base de données master.  
  
## <a name="remarks"></a>Notes   
 Les mots de passe respectent la casse.  
  
 Le hachage préalable des mots de passe est pris en charge uniquement lorsque vous créez des connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si MUST_CHANGE est spécifié, CHECK_EXPIRATION et CHECK_POLICY doivent prendre la valeur ON. Sans quoi, l'instruction échoue.  
  
 La combinaison CHECK_POLICY = OFF et CHECK_EXPIRATION = ON n'est pas prise en charge.  
  
 Quand CHECK_POLICY a la valeur OFF, *lockout_time* est réinitialisé et CHECK_EXPIRATION prend la valeur OFF.  
  
> [!IMPORTANT]  
>  CHECK_EXPIRATION et CHECK_POLICY sont imposés uniquement sur [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] et les versions ultérieures. Pour plus d'informations, consultez [Password Policy](../../relational-databases/security/password-policy.md).  
  
 Les connexions créées à partir de certificats ou de clés asymétriques ne sont utilisées que pour la signature de code. Elles ne peuvent pas être utilisées pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez créer une connexion à partir d'un certificat ou d'une clé asymétrique uniquement lorsque ceux-ci existent déjà dans la base de données master.  
  
 Pour obtenir un script de transfert des connexions, consultez [Comment transférer les connexions et les mots de passe entre des instances de SQL Server 2005 et SQL Server 2008](http://support.microsoft.com/kb/918992).  
  
 La création d'une connexion active automatiquement la nouvelle connexion et accorde à la connexion l'autorisation **CONNECT SQL** au niveau du serveur.  
 
 Le [mode d’authentification](../../relational-databases/security/choose-an-authentication-mode.md) du serveur doit correspondre au type de connexion pour autoriser l’accès.
  
 Pour plus d’informations sur la conception d’un système d’autorisations, voir [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
## <a name="includesssdsfullincludessssdsfull-mdmd-and-includesssdwincludessssdw-mdmd-logins"></a>Connexions [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], l’instruction **CREATE LOGIN** doit être la seule instruction d’un traitement.  
  
 Dans certaines méthodes de connexion à [!INCLUDE[ssSDS](../../includes/sssds-md.md)], comme **sqlcmd**, vous devez ajouter le nom du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] au nom de connexion dans la chaîne de connexion à l’aide de la notation *\<connexion>*@*\<serveur>*. Par exemple, si votre connexion est `login1` et que le nom complet du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] est `servername.database.windows.net`, le paramètre *username* de la chaîne de connexion doit être `login1@servername`. Puisque la longueur totale du paramètre *username* est de 128 caractères, *login_name* est limité à 127 caractères moins la longueur du nom du serveur. Dans l'exemple, `login_name` peut contenir seulement 117 caractères car `servername` inclut 10 caractères.  
  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], vous devez être connecté à la base de données master pour créer une connexion.  
  
 Les règles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permettent de créer une connexion d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au format \<nom_connexion>@\<nom_serveur>. Si votre serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] est **myazureserver** et que l’identifiant de connexion est **myemail@live.com**, vous devez fournir votre identifiant de connexion comme suit : **myemail@live.com@myazureserver**.  
  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les données de connexion exigées pour authentifier une connexion et les règles de pare-feu de niveau serveur sont temporairement mises en cache dans chaque base de données. Ce cache est régulièrement actualisé. Pour forcer une actualisation du cache d’authentification et garantir qu’une base de données a la version la plus récente de la table de connexions, exécutez [DBCC FLUSHAUTHCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).  
  
 Pour plus d’informations sur les connexions [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consultez [Gestion des bases de données et des connexions dans Microsoft Azure SQL Database](http://msdn.microsoft.com/library/ee336235.aspx).  
  
## <a name="permissions"></a>Autorisations  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nécessite l’autorisation **ALTER ANY LOGIN** sur le serveur ou l’appartenance au rôle serveur fixe **securityadmin**.  
  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], seule la connexion principale au niveau du serveur (créée par le processus de configuration) ou les membres du rôle de base de données `loginmanager` dans la base de données master peuvent créer de nouvelles connexions.  
  
 Si l'option **CREDENTIAL** est utilisée, l'autorisation **ALTER ANY CREDENTIAL** est également exigée sur le serveur.  
  
## <a name="next-steps"></a>Next Steps  
 Après la création d’une connexion, celle-ci peut se connecter au [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou à [!INCLUDE[ssSDS](../../includes/sssds-md.md)], mais dispose uniquement des autorisations accordées au rôle **public**. Envisagez d'effectuer certaines des activités suivantes.  
  
-   Pour vous connecter à une base de données, créez un utilisateur de base de données pour la connexion. Pour plus d’informations, consultez [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md).  
  
-   Créez un rôle serveur défini par l’utilisateur à l’aide de [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md). Utilisez **ALTER SERVER ROLE** ... **ADD MEMBER** pour ajouter la nouvelle connexion au rôle serveur défini par l’utilisateur. Pour plus d’informations, consultez [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md) et [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
-   Utilisez **sp_addsrvrolemember** pour ajouter la connexion à un rôle serveur fixe. Pour plus d’informations, consultez [Rôles de niveau serveur](../../relational-databases/security/authentication-access/server-level-roles.md) et [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md).  
  
-   Utilisez l’instruction **GRANT** pour accorder des autorisations au niveau du serveur à la nouvelle connexion ou à un rôle qui contient la connexion. Pour plus d’informations, consultez [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-login-with-a-password"></a>A. Création d'une connexion avec un mot de passe  
 L'exemple suivant crée une connexion pour un utilisateur particulier et attribue un mot de passe.  
  
```  
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';  
GO  
```  
  
### <a name="b-creating-a-login-with-a-password-that-must-be-changed"></a>B. Création d’une connexion avec un mot de passe qui doit être changé
 L'exemple suivant crée une connexion pour un utilisateur particulier et attribue un mot de passe. L'option `MUST_CHANGE` exige que les utilisateurs modifient ce mot de passe la première fois qu'ils se connectent au serveur.  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>' 
    MUST_CHANGE,  CHECK_EXPIRATION = ON;
GO  
```  
[!NOTE] L'option MUST_CHANGE ne peut être utilisée lorsque l'option CHECK_EXPIRATION est désactivée.
  
### <a name="c-creating-a-login-mapped-to-a-credential"></a>C. Création d'une connexion mappée sur une information d'identification  
 L'exemple suivant crée la connexion pour un utilisateur particulier, à l'aide de l'utilisateur. Cette connexion est mappée à l'information d'identification.  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>',   
    CREDENTIAL = <credentialName>;  
GO  
```  
  
### <a name="d-creating-a-login-from-a-certificate"></a>D. Création d'une connexion à partir d'un certificat  
 L’exemple suivant crée la connexion pour un utilisateur particulier à partir d’un certificat dans master.  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
USE MASTER;  
CREATE CERTIFICATE <certificateName>  
    WITH SUBJECT = '<login_name> certificate in master database',  
    EXPIRY_DATE = '12/05/2025';  
GO  
CREATE LOGIN <login_name> FROM CERTIFICATE <certificateName>;  
GO  
```  
  
### <a name="e-creating-a-login-from-a-windows-domain-account"></a>E. Création d'une connexion à partir d'un compte de domaine Windows  
 L'exemple suivant crée une connexion à partir d'un compte de domaine Windows.  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
CREATE LOGIN [<domainName>\<login_name>] FROM WINDOWS;  
GO  
```  
  
### <a name="f-creating-a-login-from-a-sid"></a>F. Création d'une connexion à partir d'un SID  
 L’exemple suivant crée d’abord une connexion d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et détermine le SID de la connexion.  
  
```  
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';  
  
SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';  
GO  
```  
  
 Ma requête retourne 0x241C11948AEEB749B0D22646DB1A19F2 comme SID. Votre requête retourne une valeur différente. Les instructions suivantes suppriment la connexion, puis recréent la connexion. Utilisez le SID de votre requête précédente.  
  
```  
DROP LOGIN TestLogin;  
GO  
  
CREATE LOGIN TestLogin   
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;  
  
SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';  
GO  
```  
  
## <a name="examples-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="g-creating-a-sql-server-authentication-login-with-a-password"></a>G. Création d’une connexion d’authentification SQL Server avec un mot de passe  
 L’exemple suivant crée la connexion `Mary7` avec le mot de passe `A2c3456`.  
  
```sql  
CREATE LOGIN Mary7 WITH PASSWORD = 'A2c3456$#' ;  
```  
  
### <a name="h-using-options"></a>H. Utilisation d’options  
 L’exemple suivant crée la connexion `Mary8` avec le mot de passe et certains arguments facultatifs.  
  
```  
CREATE LOGIN Mary8 WITH PASSWORD = 'A2c3456$#' MUST_CHANGE,  
CHECK_EXPIRATION = ON,  
CHECK_POLICY = ON;  
```  
  
### <a name="i-creating-a-login-from-a-windows-domain-account"></a>I. Création d'une connexion à partir d'un compte de domaine Windows  
 L’exemple suivant crée une connexion à partir d’un compte de domaine Windows nommé `Mary` dans le domaine `Contoso`.  
  
```  
CREATE LOGIN [Contoso\Mary] FROM WINDOWS;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Prise en main des autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Stratégie de mot de passe](../../relational-databases/security/password-policy.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [DROP LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/drop-login-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Créer un compte de connexion](../../relational-databases/security/authentication-access/create-a-login.md)  
  
  
