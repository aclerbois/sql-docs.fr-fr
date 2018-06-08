---
title: sp_helplogins (Transact-SQL) | Documents Microsoft
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
- sp_helplogins_TSQL
- sp_helplogins
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helplogins
ms.assetid: f9ad3767-5b9f-420d-8922-b637811404f7
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 40a25164c12e9a1c886a7cba6b8f9b0277daf0ed
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelplogins-transact-sql"></a>sp_helplogins (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Fournit des informations sur les connexions et les utilisateurs associés dans chaque base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helplogins [ [ @LoginNamePattern = ] 'login' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@LoginNamePattern =** ] **'***connexion***'**  
 Est un nom de connexion. *login* est de type **sysname**, avec NULL comme valeur par défaut. *connexion* doit exister s’il est spécifié. Si *connexion* est ne pas spécifié, les informations sur toutes les connexions sont retournées.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le premier rapport contient des informations sur chaque connexion spécifiée (voir le tableau ci-dessous).  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Nom de connexion.|  
|**SID**|**varbinary(85)**|ID de sécurité de la connexion (SID).|  
|**DefDBName**|**sysname**|Par défaut de base de données qui **LoginName** utilise pour se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**DefLangName**|**sysname**|Langue par défaut utilisée par **LoginName**.|  
|**Auser**|**char (5)**|Oui = **LoginName** a un nom d’utilisateur associé dans une base de données.<br /><br /> Non = **LoginName** n’a pas de nom d’utilisateur associé.|  
|**À distance**|**car (7)**|Oui = **LoginName** a une connexion distante.<br /><br /> Non = **LoginName** n’a pas de connexion associée.|  
  
 Le deuxième rapport contient des informations à propos des utilisateurs mappés à chaque connexion et des appartenances aux rôles de la connexion, comme illustré dans le tableau suivant.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Nom de connexion.|  
|**DBName**|**sysname**|Par défaut de base de données qui **LoginName** utilise pour se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**UserName**|**sysname**|Compte d’utilisateur **LoginName** est mappé dans **DBName**et les rôles qui **LoginName** est un membre dans **DBName**.|  
|**UserOrAlias**|**char (8)**|MemberOf = **nom d’utilisateur** est un rôle.<br /><br /> Utilisateur = **nom d’utilisateur** est un compte d’utilisateur.|  
  
## <a name="remarks"></a>Notes  
 Avant de supprimer un compte de connexion, utilisez **sp_helplogins** pour identifier les comptes d’utilisateur qui sont mappées à la connexion.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **securityadmin** rôle serveur fixe.  
  
 Pour identifier tous les comptes d’utilisateur mappés à une connexion donnée, **sp_helplogins** doit vérifier toutes les bases de données du serveur. Par conséquent, chaque base de données du serveur doit remplir une des conditions suivantes :  
  
-   L’utilisateur qui exécute **sp_helplogins** a l’autorisation d’accéder à la base de données.  
  
-   Le **invité** compte d’utilisateur est activé dans la base de données.  
  
 Si **sp_helplogins** ne peut pas accéder à une base de données, **sp_helplogins** renverra autant d’informations que possible et affiche le message d’erreur 15622.  
  
## <a name="examples"></a>Exemples  
 Le code exemple suivant fournit des informations sur la connexion `John`.  
  
```  
EXEC sp_helplogins 'John';  
GO  
  
LoginName SID                        DefDBName DefLangName AUser ARemote   
--------- -------------------------- --------- ----------- ----- -------   
John      0x23B348613497D11190C100C  master    us_english  yes   no  
  
(1 row(s) affected)  
  
LoginName   DBName   UserName   UserOrAlias   
---------   ------   --------   -----------   
John        pubs     John       User          
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_helpdb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
