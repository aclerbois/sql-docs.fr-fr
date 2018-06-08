---
title: xp_logininfo (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- xp_logininfo_TSQL
- xp_logininfo
dev_langs:
- TSQL
helpviewer_keywords:
- xp_logininfo
ms.assetid: ee7162b5-e11f-4a0e-a09c-1878814dbbbd
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ae6820bc76ff2bb98360a77c4c3a5432d1e18af5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="xplogininfo-transact-sql"></a>xp_logininfo (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les utilisateurs Windows et les groupes Windows.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
xp_logininfo [ [ @acctname = ] 'account_name' ]   
     [ , [ @option = ] 'all' | 'members' ]   
     [ , [ @privilege = ] variable_name OUTPUT]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@acctname =** ] **'***account_name***'**  
 Nom d'un utilisateur ou groupe Windows autorisé à accéder à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *account_name* est **sysname**, avec NULL comme valeur par défaut. Si *account_name* n’est pas spécifié, tous les groupes Windows et les utilisateurs Windows qui ont été explicitement accordé l’autorisation de se connecter sont signalés. *account_name* doivent être qualifiés complets. Par exemple, « ADVWKS4\macraes » ou « BUILTIN\Administrators ».  
  
 **'all'** | **« membres »**  
 Spécifie s'il faut rapporter les informations sur tous les chemins d'autorisation du compte ou sur les membres du groupe Windows. **@option** est **varchar (10)**, avec NULL comme valeur par défaut. À moins que **tous les** est spécifié, seul le chemin de la première autorisation s’affiche.  
  
 [  **@privilege =** ] *nom_variable*  
 Paramètre de sortie qui retourne le niveau de privilège du compte Windows spécifié. *nom_variable* est **varchar (10)**, avec une valeur par défaut « Not wanted ». Le niveau de privilège renvoyé est **utilisateur**, **admin**, ou **null**.  
  
 OUTPUT  
 Place *nom_variable* dans le paramètre de sortie.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**Nom du compte**|**sysname**|Nom de compte Windows complet.|  
|**type**|**char (8)**|Type de compte Windows. Les valeurs valides sont **utilisateur** ou **groupe**.|  
|**privilège**|**char(9)**|Privilège d'accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les valeurs valides sont **admin**, **utilisateur**, ou **null**.|  
|**nom de connexion mappé**|**sysname**|Pour les comptes d’utilisateurs qui ont des privilèges d’utilisateur, **mappé le nom de connexion** indique le nom de connexion mappé que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] essaie d’utiliser lors de la connexion avec ce compte en utilisant les règles de mappage avec le nom de domaine est ajouté avant qu’il.|  
|**chemin d’accès de l’autorisation**|**sysname**|Membre du groupe qui autorise l'accès au compte|  
  
## <a name="remarks"></a>Notes  
 Si *account_name* est spécifié, **xp_logininfo** indique le niveau de privilège plus élevé de l’utilisateur Windows spécifié ou le groupe. Si un utilisateur Windows dispose d'autorisations d'accès en tant qu'administrateur système et utilisateur de domaine, il est indiqué comme administrateur système. Si l'utilisateur est membre de plusieurs groupes Windows dotés des mêmes niveaux de privilèges, seul le groupe qui a obtenu le premier l'accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est indiqué.  
  
 Si *account_name* est un utilisateur Windows valide ou un groupe qui n’est pas associé à une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion, le jeu de résultats vide est retourné. Si *account_name* ne peut pas être identifié comme un utilisateur Windows valide ou un groupe, un message d’erreur est retourné.  
  
 Si *account_name* et **tous les** sont spécifiés, tous les chemins d’accès d’autorisation pour l’utilisateur ou groupe Windows sont retournés. Si *account_name* est un membre de plusieurs groupes qui ont été accordé l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], plusieurs lignes sont retournées. Le **admin** lignes avec le privilège sont retournés avant le **utilisateur** privilège des lignes et au sein d’un privilège au niveau lignes sont retournées dans l’ordre dans lequel correspondant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexions ont été créées.  
  
 Si *account_name* et **membres** sont spécifiés, une liste des membres du groupe de niveau suivant est retournée. Si *account_name* est un groupe local, la liste peut inclure les utilisateurs locaux, des utilisateurs de domaine et des groupes. Si *account_name* est un compte de domaine, la liste est composée d’utilisateurs du domaine. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit se connecter au contrôleur de domaine pour extraire les informations d'appartenance au groupe. Si le serveur ne peut pas contacter le contrôleur de domaine, aucune information n'est renvoyée.  
  
 **xp_logininfo** retourne uniquement les informations de groupes globaux Active Directory, les groupes universels pas.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au **sysadmin** rôle serveur fixe ou l’appartenance au **public** rôle de base de données fixe dans le **master** base de données avec l’autorisation EXECUTE accordée.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant affiche des informations sur la `BUILTIN\Administrators` groupe Windows.  
  
```  
EXEC xp_logininfo 'BUILTIN\Administrators';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_denylogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées étendues générales &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)  
  
  
