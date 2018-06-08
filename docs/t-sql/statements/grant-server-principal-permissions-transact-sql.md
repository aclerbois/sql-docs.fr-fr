---
title: GRANT - Accorder des autorisations de principal de serveur (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- impersonate [SQL Server], granting
- granting permissions [SQL Server], logins
- permissions [SQL Server], impersonate
- permissions [SQL Server], logins
- GRANT statement, impersonation
- GRANT statement, logins
- logins [SQL Server], granting access
- granting permissions [SQL Server], impersonation
ms.assetid: 4cbed281-5e1e-4d8b-b410-4c18a6cd0205
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3c07fe67131d5b8ef60c74b48d20b9177851c474
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="grant-server-principal-permissions-transact-sql"></a>GRANT – octroi d'autorisations de principal de serveur (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet d'accorder des autorisations sur un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
GRANT permission [ ,...n ] }   
    ON   
    { [ LOGIN :: SQL_Server_login ]  
      | [ SERVER ROLE :: server_role ] }   
    TO <server_principal> [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
    SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey   
    | server_role  
```  
  
## <a name="arguments"></a>Arguments  
 *permission*  
 Spécifie une autorisation qui peut être accordée sur une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 LOGIN **::** *SQL_Server_login*  
 Spécifie la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle l'autorisation est accordée. Le qualificateur d’étendue (**::**) est obligatoire.  
  
 SERVER ROLE **::** *server_role*  
 Spécifie le rôle de serveur défini par l'utilisateur sur lequel l'autorisation est accordée. Le qualificateur d’étendue (**::**) est obligatoire.  
  
 TO \<server_principal> spécifie le rôle serveur ou la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auquel accorder l’autorisation.  
  
 *SQL_Server_login*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir d'une connexion Windows.  
  
 *SQL_Server_login_from_certificate*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur un certificat.  
  
 *SQL_Server_login_from_AsymKey*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur une clé asymétrique.  
  
 *server_role*  
 Spécifie le nom d'un rôle de serveur défini par l'utilisateur.  
  
 WITH GRANT OPTION  
 Indique que le principal a également la possibilité d'accorder l'autorisation spécifiée à d'autres principaux.  
  
 AS *SQL_Server_login*  
 Spécifie la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de laquelle le principal qui exécute cette requête dérive son droit d'accorder l'autorisation.  
  
## <a name="remarks"></a>Notes   
 Les autorisations dans l'étendue du serveur peuvent être accordées seulement lorsque la base de données en cours est master.  
  
 Les informations sur les autorisations de serveur sont consultables dans la vue de catalogue [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md). Les informations sur les principaux de serveur sont consultables dans la vue de catalogue [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Les comptes de connexion et les rôles de serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont des éléments sécurisables au niveau du serveur. Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur un rôle de serveur ou un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Compte de connexion SQL Server ou autorisation de rôle de serveur|Déduite d'une autorisation de rôle de serveur ou de compte de connexion SQL Server|Déduite d'une autorisation de serveur|  
|------------------------------------------------|-----------------------------------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL SERVER|  
|IMPERSONATE|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|ALTER|CONTROL|ALTER ANY LOGIN<br /><br /> ALTER ANY SERVER ROLE|  
  
## <a name="permissions"></a>Autorisations  
 Pour les comptes de connexion, requiert l'autorisation CONTROL sur le compte de connexion ou l'autorisation ALTER ANY LOGIN sur le serveur.  
  
 Pour les rôles de serveur, requiert l'autorisation CONTROL sur le rôle de serveur ou l'autorisation ALTER ANY SERVER ROLE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-granting-impersonate-permission-on-a-login"></a>A. Octroi d'une autorisation IMPERSONATE sur une connexion  
 Dans l’exemple ci-dessous, l’autorisation `IMPERSONATE` sur la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `WanidaBenshoof` est accordée à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir du compte d’utilisateur Windows `AdvWorks\YoonM`.  
  
```  
USE master;  
GRANT IMPERSONATE ON LOGIN::WanidaBenshoof to [AdvWorks\YoonM];  
GO  
```  
  
### <a name="b-granting-view-definition-permission-with-grant-option"></a>B. Octroi d'une autorisation VIEW DEFINITION avec l'option GRANT OPTION  
 Dans l'exemple ci-dessous, l'autorisation `VIEW DEFINITION` sur la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `EricKurjan` est accordée à la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `RMeyyappan` avec l'option `GRANT OPTION`.  
  
```  
USE master;  
GRANT VIEW DEFINITION ON LOGIN::EricKurjan TO RMeyyappan   
    WITH GRANT OPTION;  
GO   
```  
  
### <a name="c-granting-view-definition-permission-on-a-server-role"></a>C. Octroi d'une autorisation VIEW DEFINITION sur un rôle de serveur  
 L'exemple suivant accorde `VIEW DEFINITION` sur le rôle de serveur `Sales` au rôle de serveur `Auditors`.  
  
```  
USE master;  
GRANT VIEW DEFINITION ON SERVER ROLE::Sales TO Auditors ;  
GO   
```  
  
## <a name="see-also"></a> Voir aussi  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  

