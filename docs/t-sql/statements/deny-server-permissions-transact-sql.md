---
title: DENY – Refuser des autorisations sur un serveur (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], servers
- denying permissions [SQL Server], servers
- servers [SQL Server], permissions
- DENY statement, servers
ms.assetid: 68d6b2a9-c36f-465a-9cd2-01d43a667e99
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d4b4363b3ef41a60ccedcbbef223a0f0e31251ef
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deny-server-permissions-transact-sql"></a>DENY – refus d'autorisations de serveur (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Permet de refuser des autorisations sur un serveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
DENY permission [ ,...n ]   
    TO <grantee_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS <grantor_principal> ]   
  
<grantee_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
  
<grantor_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
```  
  
## <a name="arguments"></a>Arguments  
 *permission*  
 Spécifie une autorisation qui peut être refusée sur un serveur. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 CASCADE  
 Indique que l'autorisation à refuser est également refusée pour les autres principaux auxquels elle a été accordée par ce principal.  
  
 TO \<server_principal>  
 Spécifie le principal pour lequel l'autorisation doit être refusée.  
  
 AS \<grantor_principal>  
 Spécifie le principal à partir duquel le principal qui exécute cette requête dérive son droit de refuser l'autorisation.  
  
 *SQL_Server_login*  
 Spécifie une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_mapped_to_Windows_login*  
 Spécifie une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur une connexion Windows.  
  
 *SQL_Server_login_mapped_to_Windows_group*  
 Spécifie une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur un groupe Windows.  
  
 *SQL_Server_login_mapped_to_certificate*  
 Spécifie un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappé à un certificat.  
  
 *SQL_Server_login_mapped_to_asymmetric_key*  
 Spécifie un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappé à une clé asymétrique.  
  
 *server_role*  
 Spécifie un rôle de serveur.  
  
## <a name="remarks"></a>Notes   
 Les autorisations dans l'étendue du serveur peuvent être refusées seulement lorsque la base de données en cours est master.  
  
 Les informations sur les autorisations de serveur peuvent être consultées dans la vue de catalogue [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md), tandis que les informations sur les principaux de serveur peuvent être consultées dans la vue de catalogue [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Des informations sur l’appartenance des rôles de serveur peuvent être consultées dans la vue de catalogue [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
 Un serveur représente le plus haut niveau de la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de refuser sur un serveur sont répertoriées dans le tableau ci-dessous.  
  
|Autorisation de serveur|Déduite d'une autorisation de serveur|  
|-----------------------|----------------------------------|  
|ADMINISTER BULK OPERATIONS|CONTROL SERVER|  
|ALTER ANY AVAILABILITY GROUP<br /><br /> **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|CONTROL SERVER|  
|ALTER ANY CONNECTION|CONTROL SERVER|  
|ALTER ANY CREDENTIAL|CONTROL SERVER|  
|ALTER ANY DATABASE|CONTROL SERVER|  
|ALTER ANY ENDPOINT|CONTROL SERVER|  
|ALTER ANY EVENT NOTIFICATION|CONTROL SERVER|  
|ALTER ANY EVENT SESSION|CONTROL SERVER|  
|ALTER ANY LINKED SERVER|CONTROL SERVER|  
|ALTER ANY LOGIN|CONTROL SERVER|  
|ALTER ANY SERVER AUDIT|CONTROL SERVER|  
|ALTER ANY SERVER ROLE<br /><br /> **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|CONTROL SERVER|  
|ALTER RESOURCES|CONTROL SERVER|  
|ALTER SERVER STATE|CONTROL SERVER|  
|ALTER SETTINGS|CONTROL SERVER|  
|ALTER TRACE|CONTROL SERVER|  
|AUTHENTICATE SERVER|CONTROL SERVER|  
|CONNECT ANY DATABASE<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|CONTROL SERVER|  
|CONNECT SQL|CONTROL SERVER|  
|CONTROL SERVER|CONTROL SERVER|  
|CREATE ANY DATABASE|ALTER ANY DATABASE|  
|Créer un groupe de disponibilité<br /><br /> **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|ALTER ANY AVAILABILITY GROUP|  
|CREATE DDL EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|CREATE ENDPOINT|ALTER ANY ENDPOINT|  
|CREATE SERVER ROLE<br /><br /> **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|ALTER ANY SERVER ROLE|  
|CREATE TRACE EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|EXTERNAL ACCESS ASSEMBLY|CONTROL SERVER|  
|IMPERSONATE ANY LOGIN<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|CONTROL SERVER|  
|SELECT ALL USER SECURABLES<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|CONTROL SERVER|  
|SHUTDOWN|CONTROL SERVER|  
|UNSAFE ASSEMBLY|CONTROL SERVER|  
|VIEW ANY DATABASE|VIEW ANY DEFINITION|  
|VIEW ANY DEFINITION|CONTROL SERVER|  
|VIEW SERVER STATE|ALTER SERVER STATE|  
  
## <a name="remarks"></a>Notes   
 Les trois autorisations de serveur suivantes ont été ajoutées dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Autorisation **CONNECT ANY DATABASE**  
 Accordez l'autorisation **CONNECT ANY DATABASE** à une connexion qui doit se connecter à toutes les bases de données existantes et à celles qui pourront être créées. N'accorde pas d'autorisation dans une base de données au-delà de la connexion. Associez cette autorisation à **SELECT ALL USER SECURABLES** ou **VIEW SERVER STATE** pour autoriser un processus d’audit à consulter toutes les données ou toutes les bases de données sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Autorisation **IMPERSONATE ANY LOGIN**  
 Lorsque cette autorisation est accordée, elle permet à un processus de niveau intermédiaire d'emprunter l'identité du compte des clients qui se connectent, à mesure qu'il se connecte aux bases de données. Si cette autorisation est refusée, une connexion dotée de privilèges élevés peut être bloquée à partir de l'emprunt d'identité d'autres connexions. Par exemple, une connexion dotée de l'autorisation **CONTROL SERVER** peut être bloquée à partir de l'emprunt d'identité d'autres connexions.  
  
 Autorisation **SELECT ALL USER SECURABLES**  
 Lorsque cette autorisation est accordée, une connexion telle qu'un auditeur peut afficher les données de toutes les bases de données auxquelles l'utilisateur se connecte. Quand cette autorisation est refusée, elle empêche l’accès aux objets sauf s’ils se trouvent dans le schéma **sys**.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER ou la propriété de l'élément sécurisable. Si vous utilisez la clause AS, le principal spécifié doit posséder l'élément sécurisable sur lequel les autorisations doivent être refusées.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-denying-connect-sql-permission-to-a-sql-server-login-and-principals-to-which-the-login-has-regranted-it"></a>A. Refus d'une autorisation CONNECT SQL à une connexion SQL Server et aux principaux auxquels la connexion a accordé à son tour l'autorisation  
 Dans l'exemple ci-dessous, l'autorisation `CONNECT SQL` est refusée à la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `Annika` et aux principaux auxquels elle a accordé l'autorisation.  
  
```  
USE master;  
DENY CONNECT SQL TO Annika CASCADE;  
GO  
```  
  
### <a name="b-denying-create-endpoint-permission-to-a-sql-server-login-using-the-as-option"></a>B. Refus d'une autorisation CREATE ENDPOINT à une connexion SQL Server avec l'option AS  
 Dans l'exemple ci-dessous, l'autorisation `CREATE ENDPOINT` est refusée à l'utilisateur `ArifS`. L'exemple utilise l'option `AS` pour spécifier `MandarP` comme principal à partir duquel le principal en charge dérive l'autorité d'agir ainsi.  
  
```  
USE master;  
DENY CREATE ENDPOINT TO ArifS AS MandarP;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [DENY - Refuser des autorisations sur un serveur (Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE - Révoquer des autorisations sur un serveur &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md)   
 [Hiérarchie des autorisations &#40;Moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
  
