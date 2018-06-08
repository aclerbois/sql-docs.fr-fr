---
title: sp_help_publication_access (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_help_publication_access
- sp_help_publication_access_TSQL
helpviewer_keywords:
- sp_help_publication_access
ms.assetid: 9408fa13-54a0-4cb1-8fb0-845e5536ef50
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f4a8503a1c93283c2af8e6b4e2494cfdaff632b2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelppublicationaccess-transact-sql"></a>sp_help_publication_access (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne la liste de toutes les connexions accordées pour une publication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_publication_access [ @publication = ] 'publication'  
    [ , [ @return_granted = ] 'return_granted' ]   
    [ , [ @login = ] 'login' ]  
    [ , [ @initial_list = ] initial_list ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication à laquelle accéder. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@return_granted=**] **'***return_granted***'**  
 ID de la connexion. *return_granted* est **bits**, avec 1 comme valeur par défaut. Si **0** est spécifié et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification est utilisée, les connexions disponibles qui apparaissent sur le serveur de publication mais pas sur le serveur de distribution sont retournées. Si **0** est spécifié et l’authentification Windows est utilisée, les connexions ne sont pas spécifiquement d’interdiction d’accès à l’éditeur ou distributeur sont renvoyées.  
  
 [  **@login=**] **'***connexion***'**  
 ID de connexion de sécurité standard. *connexion* est **sysname**, avec une valeur par défaut **%**.  
  
 [  **@initial_list =**] *initial_list*  
 Indique s'il faut retourner tous les membres bénéficiant d'un accès à la publication ou uniquement ceux qui y avaient accès avant l'ajout de nouveaux membres à la liste. *initial_list* est de type bit, avec une valeur par défaut **0**.  
  
 **1** renvoie des informations pour tous les membres de la **sysadmin** rôle serveur fixe avec des connexions valides sur le serveur de distribution qui existait lorsque la publication a été créée, ainsi que la connexion actuelle.  
  
 **0** renvoie des informations pour tous les membres de la **sysadmin** rôle serveur fixe avec des connexions valides sur le serveur de distribution qui existait au moment de la création de la publication ainsi que tous les utilisateurs dans la liste d’accès qui n’ont pas appartiennent à la **sysadmin** rôle serveur fixe.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**LoginName**|**nvarchar (256)**|Nom de connexion réel.|  
|**isntname**|**int**|**0** = connexion n’est pas un utilisateur Windows.<br /><br /> **1** = connexion est un utilisateur Windows.|  
|**isntgroup**|**int**|**0** = connexion n’est pas un groupe Windows.<br /><br /> **1** = connexion est un groupe Windows.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_help_publication_access** est utilisée dans tous les types de réplication.  
  
 Lorsque les deux **Isntname** et **Isntgroup** dans le résultat sont **0**, il est supposé que la connexion est une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_help_publication_access**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_grant_publication_access &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md)   
 [sp_revoke_publication_access &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
