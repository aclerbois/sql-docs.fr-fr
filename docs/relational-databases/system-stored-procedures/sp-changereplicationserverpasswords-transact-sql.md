---
title: sp_changereplicationserverpasswords (Transact-SQL) | Documents Microsoft
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
- sp_changereplicationserverpasswords_TSQL
- sp_changereplicationserverpasswords
helpviewer_keywords:
- sp_changereplicationserverpasswords
ms.assetid: 9333da96-3a1c-4adb-9a74-5dac9ce596df
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7c27142aa0e628e7041202429ae6bbaee0b87a1f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spchangereplicationserverpasswords-transact-sql"></a>sp_changereplicationserverpasswords (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les mots de passe stockés pour la [!INCLUDE[msCoName](../../includes/msconame-md.md)] compte Windows ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion utilisée par les agents de réplication lors de la connexion aux serveurs dans une topologie de réplication. En temps normal, vous devriez modifier un mot de passe pour chacun des Agents en cours d'exécution sur un serveur, même s'ils utilisent tous la même connexion ou le même compte. Cette procédure stockée vous permet de modifier le mot de passe pour toutes les instances d'une connexion ou d'un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donnés utilisés par tous les Agents de réplication exécutés sur un serveur. Cette procédure stockée est exécutée sur la base de données master de n'importe quel serveur de la topologie de réplication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changereplicationserverpasswords [ @login_type = ] login_type  
        , [ @login = ] 'login'   
        , [ @password = ] 'password'  
    [ , [ @server = ] 'server' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@login_type** =] *login_type*  
 Type d'authentification des informations d'identification fournies. *LOGIN_TYPE* est **tinyint**, sans valeur par défaut.  
  
 **1** = authentification intégrée de Windows  
  
 **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification  
  
 [ **@login** =] **'***connexion***'**  
 Nom du compte Windows ou de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours de modification. *connexion* est **nvarchar (257)**, sans valeur par défaut  
  
 [ **@password** =] **'***mot de passe***'**  
 Nouveau mot de passe doit être stocké spécifié *connexion*. *mot de passe* est **sysname**, sans valeur par défaut.  
  
> [!NOTE]  
>  Après avoir modifié un mot de passe de réplication, vous devez arrêter puis redémarrer chaque Agent qui utilise ce mot de passe afin que les modifications apportées prennent effet.  
  
 [ **@server** =] **'***server***'**  
 Connexion serveur pour laquelle le mot de passe stocké est en cours de changement. *serveur* est **sysname**, et peut prendre l’une des valeurs suivantes :  
  
|Valeur| Description|  
|-----------|-----------------|  
|**Serveur de distribution**|Toutes les connexions d'Agent au serveur de distribution.|  
|**publisher** (serveur de publication)|Toutes les connexions d'Agent au serveur de publication.|  
|**subscriber** (Abonné)|Toutes les connexions d'Agent à l'Abonné.|  
|**%** (par défaut)|Toutes les connexions d'Agent à tous les serveurs d'une topologie de réplication.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changereplicationserverpasswords** est utilisé avec tous les types de réplication.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_changereplicationserverpasswords**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les paramètres de sécurité de la réplication](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
