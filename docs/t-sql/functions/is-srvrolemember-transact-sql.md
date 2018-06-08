---
title: IS_SRVROLEMEMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- IS_SRVROLEMEMBER_TSQL
- IS_SRVROLEMEMBER
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_SRVROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 3241a44a-6958-415b-b8b7-2a1207c36ab3
caps.latest.revision: 65
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 525b5ad479dcba8caa584988498e02a6d25dc44f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="issrvrolemember-transact-sql"></a>IS_SRVROLEMEMBER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Indique si un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appartient au rôle de serveur spécifié.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
IS_SRVROLEMEMBER ( 'role' [ , 'login' ] )  
```  
  
## <a name="arguments"></a>Arguments  
 **'** *role* **'**  
 Nom du rôle de serveur vérifié. *role* est de type **sysname**.  
  
 Les valeurs valides pour *role* sont des rôles serveur définis par l’utilisateur, et les rôles serveur fixes suivants :  
  
|||  
|-|-|  
|sysadmin|serveradmin|  
|dbcreator|setupadmin|  
|bulkadmin|securityadmin|  
|diskadmin|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> public|  
|processadmin||  
  
 **'** *login* **'**  
 Nom du compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à vérifier. *login* est de type **sysname**, avec NULL comme valeur par défaut. Si aucune valeur n’est spécifiée, le résultat est basé sur le contexte d’exécution actuel. Si le paramètre contient le mot NULL, retourne NULL.  
  
## <a name="return-types"></a>Types de retour  
 **Int**  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|0|*login* n’est pas membre de *role*.<br /><br /> Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cette instruction retourne toujours 0.|  
| 1|*login* est membre de *role*.|  
|NULL|*role* ou *login* n’est pas valide, ou vous ne disposez pas de l’autorisation nécessaire pour afficher l’appartenance au rôle.|  
  
## <a name="remarks"></a>Notes   
 Utilisez IS_SRVROLEMEMBER pour déterminer si l’utilisateur actuel peut réaliser une action nécessitant les autorisations du rôle serveur.  
  
 Si une connexion Windows, telle que Contoso\Mary5, est spécifiée pour *login*, **IS_SRVROLEMEMBER** retourne **NULL** à moins de s’être vu attribuer ou refuser l’accès direct à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si le paramètre *login* facultatif n’est pas fourni et si *login* correspond à un domaine Windows, il peut s’agir d’un membre du rôle serveur fixe via une appartenance à un groupe Windows. Pour résoudre de telles appartenances indirectes, IS_SRVROLEMEMBER demande des informations sur l'appartenance au groupe Windows à partir du contrôleur du domaine. Si le contrôleur du domaine est inaccessible ou ne répond pas, **IS_SRVROLEMEMBER** retourne des informations sur l’appartenance au rôle, en prenant uniquement en considération l’utilisateur et ses groupes locaux. Si l'utilisateur spécifié n'est pas l'utilisateur actuel, la valeur retournée par IS_SRVROLEMEMBER peut différer de la dernière mise à jour de données de l'authentificateur (par exemple Active Directory) sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si le paramètre de connexion facultatif est fourni, la connexion Windows interrogée doit être présente dans sys.server_principals, sinon IS_SRVROLEMEMBER retourne la valeur NULL. Cela indique que la connexion n'est pas valide.  
  
 Lorsque le paramètre de connexion est un compte de connexion de domaine ou lorsqu'il est basé sur un groupe Windows et que le contrôleur de domaine n'est pas accessible, les appels à IS_SRVROLEMEMBER échouent et peuvent retourner des données incorrectes ou incomplètes.  
  
 Si le contrôleur de domaine n'est pas accessible, l'appel à IS_SRVROLEMEMBER retourne des informations exactes lorsque le principe Windows peut être authentifié localement, par exemple un compte Windows local ou une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **IS_SRVROLEMEMBER** retourne toujours 0 lorsqu’un groupe Windows est utilisé comme argument de connexion et que ce groupe Windows est membre d’un autre groupe Windows qui est, à son tour, membre du rôle serveur spécifié.  
  
 Le paramètre de contrôle de compte d’utilisateur (UAC) peut entraîner le renvoi de résultats différents. Cela varie selon que l'utilisateur a accédé au serveur en tant que membre de groupe Windows ou en tant qu'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifique.  
  
 Cette fonction évalue l'appartenance au rôle, et non l'autorisation sous-jacente. Par exemple, le rôle serveur fixe **sysadmin** dispose de l’autorisation **CONTROL SERVER**. Si l’utilisateur dispose de l’autorisation **CONTROL SERVER**, mais n’est pas membre du rôle, cette fonction signalera correctement que l’utilisateur n’est pas un membre du rôle **sysadmin**, bien que l’utilisateur dispose des mêmes autorisations.  
  
## <a name="related-functions"></a>Fonctions connexes  
 Pour déterminer si l’utilisateur actuel est membre du groupe Windows spécifié ou du rôle de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md). Pour déterminer si une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est membre d’un rôle de base de données, utilisez [IS_ROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-rolemember-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DEFINITION sur le rôle de serveur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant indique si la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'utilisateur actuel est membre du rôle serveur fixe `sysadmin`.  
  
```  
IF IS_SRVROLEMEMBER ('sysadmin') = 1  
   print 'Current user''s login is a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') = 0  
   print 'Current user''s login is NOT a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') IS NULL  
   print 'ERROR: The server role specified is not valid.';  
```  
  
 L’exemple suivant indique si la connexion de domaine Pat est un membre du rôle serveur fixe **diskadmin**.  
  
```  
SELECT IS_SRVROLEMEMBER('diskadmin', 'Contoso\Pat');  
```  
  
## <a name="see-also"></a> Voir aussi  
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
