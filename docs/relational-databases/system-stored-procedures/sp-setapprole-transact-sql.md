---
title: sp_setapprole (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_setapprole
- sp_setapprole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setapprole
ms.assetid: cf0901c0-5f90-42d4-9d5b-8772c904062d
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ac733afa3f8afef74a9d6affb16e0f9dbcd5b4a9
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsetapprole-transact-sql"></a>sp_setapprole (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Active les autorisations associées à un rôle d'application dans la base de données active.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_setapprole [ @rolename = ] 'role',  
    [ @password = ] { encrypt N'password' }   
      |  
        'password' [ , [ @encrypt = ] { 'none' | 'odbc' } ]  
        [ , [ @fCreateCookie = ] true | false ]  
    [ , [ @cookie = ] @cookie OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@rolename =** ] **'***rôle***'**  
 Nom du rôle d'application défini dans la base de données active. *rôle* est **sysname**, sans valeur par défaut. *rôle* doit exister dans la base de données actuelle.  
  
 [  **@password =** ] **{chiffrer N'***mot de passe***'}**  
 Mot de passe nécessaire pour activer le rôle d'application. *mot de passe* est **sysname**, sans valeur par défaut. *mot de passe* peut être obscurci à l’aide de ODBC **chiffrer** (fonction). Lorsque vous utilisez la **chiffrer** (fonction), le mot de passe doit être converti en une chaîne Unicode en plaçant **N** avant le premier guillemet.  
  
 L’option encrypt n’est pas pris en charge sur les connexions qui sont à l’aide de **SqlClient**.  
  
> [!IMPORTANT]  
>  ODBC **chiffrer** (fonction) ne fournit pas de chiffrement. Vous ne devez pas compter sur cette fonction pour protéger des mots de passe transmis sur un réseau. Si de telles informations doivent être transmises sur un réseau, utilisez SSL ou IPSec.  
  
 **@encrypt = 'none'**  
 Indique qu'aucun codage ne doit être utilisé. Le mot de passe est transmis à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous forme de texte brut. Il s'agit du paramètre par défaut.  
  
 **@encrypt= « odbc »**  
 Spécifie que ODBC est obfusquer le mot de passe à l’aide de ODBC **chiffrer** fonction avant d’envoyer le mot de passe pour le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Ceci peut être spécifié seulement si vous utilisez un client ODBC ou le fournisseur OLE DB pour SQL Server.  
  
 [  **@fCreateCookie =** ] **true** | **false**  
 Indique si un cookie doit être créé. **true** est implicitement converti en 1. **false** est implicitement converti en 0.  
  
 [  **@cookie =** ]  **@cookie SORTIE**  
 Spécifie le paramètre de sortie qui doit contenir le cookie. Le cookie est généré uniquement si la valeur de **@fCreateCookie** est **true**. **varbinary(8000)**  
  
> [!NOTE]  
>  Le paramètre **OUTPUT** de cookie pour **sp_setapprole** est actuellement documenté comme **varbinary(8000)** , ce qui correspond à la longueur maximale correcte. Cependant, l’implémentation actuelle retourne **varbinary(50)**. Les applications doivent continuer à réserver **varbinary (8000)** afin que l’application continue à fonctionner correctement si le cookie de taille de retour augmente dans une version ultérieure.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) et 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Une fois qu’une application rôle est activé à l’aide de **sp_setapprole**, il demeure actif jusqu'à ce que l’utilisateur se déconnecte du serveur ou exécute **sp_unsetapprole**. **sp_setapprole** peuvent être exécutées directement par [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions. **sp_setapprole** ne peut pas être exécutée dans une autre procédure stockée ou dans une transaction définie par l’utilisateur.  
  
 Pour une vue d’ensemble des rôles d’application, consultez [les rôles d’Application](../../relational-databases/security/authentication-access/application-roles.md).  
  
> [!IMPORTANT]  
>  Pour protéger le mot de passe d'un rôle d'application lors de sa transmission sur un réseau, vous devez toujours utiliser une connexion chiffrée pour activer un rôle d'application.  
>   
>  Le [!INCLUDE[msCoName](../../includes/msconame-md.md)] ODBC **chiffrer** option n’est pas pris en charge par **SqlClient**. Si vous devez stocker des informations d'identification, chiffrez-les à l'aide des fonctions API de chiffrement. Le paramètre *mot de passe* est stocké comme un hachage unidirectionnel. Pour préserver la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], stratégie de complexité de mot de passe n’est pas appliquée par **sp_addapprole**. Pour appliquer la stratégie de complexité de mot de passe, utilisez [CREATE APPLICATION ROLE](../../t-sql/statements/create-application-role-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au **public** et la connaissance du mot de passe pour le rôle.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-activating-an-application-role-without-the-encrypt-option"></a>A. Activation d'un rôle d'application sans l'option encrypt  
 Dans l'exemple ci-dessous, un rôle d'application nommé `SalesAppRole` est activé avec le mot de passe en texte brut `AsDeF00MbXX`, créé avec les autorisations spécialement conçues pour l'application utilisée par l'utilisateur en cours.  
  
```  
EXEC sp_setapprole 'SalesApprole', 'AsDeF00MbXX';  
GO  
```  
  
### <a name="b-activating-an-application-role-with-a-cookie-and-then-reverting-to-the-original-context"></a>B. Activation d'un rôle d'application avec un cookie, puis retour au contexte d'origine  
 Dans l'exemple ci-dessous, le rôle d'application `Sales11` est activé avec le mot de passe `fdsd896#gfdbfdkjgh700mM` et un cookie est créé. L'exemple retourne le nom de l'utilisateur actuel, puis revient au contexte d'origine en exécutant `sp_unsetapprole`.  
  
```  
DECLARE @cookie varbinary(8000);  
EXEC sp_setapprole 'Sales11', 'fdsd896#gfdbfdkjgh700mM'  
    , @fCreateCookie = true, @cookie = @cookie OUTPUT;  
-- The application role is now active.  
SELECT USER_NAME();  
-- This will return the name of the application role, Sales11.  
EXEC sp_unsetapprole @cookie;  
-- The application role is no longer active.  
-- The original context has now been restored.  
GO  
SELECT USER_NAME();  
-- This will return the name of the original user.   
GO   
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [sp_unsetapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unsetapprole-transact-sql.md)  
  
  
