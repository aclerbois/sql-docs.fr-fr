---
title: sp_validate_replica_hosts_as_publishers (Transact-SQL) | Documents Microsoft
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
- sp_validate_replica_hosts_as_publishers_TSQL
- sp_validate_replica_hosts_as_publishers
helpviewer_keywords:
- sp_validate_replica_hosts_as_publishers
ms.assetid: 45001fc9-2dbd-463c-af1d-aa8982d8c813
caps.latest.revision: 11
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 408d6c239afd528deeae25f925b8626968dff6bb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spvalidatereplicahostsaspublishers-transact-sql"></a>sp_validate_replica_hosts_as_publishers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  **sp_validate_replica_hosts_as_publishers** est une extension de **sp_validate_redirected_publisher** qui permet à tous les réplicas secondaires d’être validés, plutôt que de simplement le réplica principal actuel. **sp_validate_replicat_hosts_as_publisher** valide une topologie de réplication entière Always On. **sp_validate_replica_hosts_as_publishers** doit être exécutée directement sur le serveur de distribution à l’aide d’une session Bureau à distance afin d’éviter une erreur de sécurité de double-saut (21892).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_validate_replica_hosts_as_publishers   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @redirected_publisher = ] 'new_publisher' output  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@original_publisher** =] **'***original_publisher***'**  
 Nom de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a publié la base de données à l'origine. *original_publisher* est **sysname**, sans valeur par défaut.  
  
 [ **@publisher_db** = ] **'***publisher_db***'**  
 Nom de la base de données publiée. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [ **@redirected_publisher** =] **'***redirected_publisher***'**  
 La cible de redirection lorsque **sp_redirect_publisher** a été appelé pour le serveur de publication et de publication d’origine de la base de données paire. *redirected_publisher* est **sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 Si aucune entrée n’existe pour le serveur de publication et de la base de données de publication, **sp_validate_redirected_publisher** retourne la valeur null pour le paramètre de sortie *@redirected_publisher*. Sinon, le serveur de publication redirigé associé est retourné, à la fois en cas de réussite et d'échec.  
  
 Si la validation réussit, **sp_validate_redirected_publisher** retourne une indication de réussite.  
  
 Si la validation échoue, les erreurs appropriées sont générées.  **sp_validate_redirected_publisher** fait de son mieux pour déclencher tous les problèmes et pas seulement la première rencontré.  
  
> [!NOTE]  
>  **sp_validate_replica_hosts_as_publishers** échoue avec l’erreur suivante lors de la validation des hôtes de réplica secondaire qui n’autorisent pas l’accès en lecture, ou nécessitent la spécification de l’intention de lecture.  
>   
>  Msg 21899, Niveau 11, État 1, Procédure **sp_hadr_verify_subscribers_at_publisher**, Ligne 109  
>   
>  La requête au serveur de publication redirigé 'MyReplicaHostName' pour déterminer s'il y a des entrées sysserver pour les abonnés du serveur de publication d'origine 'MyOriginalPublisher' a échoué avec l'erreur '976', message d'erreur 'Erreur 976, Niveau 14, État 1, Message : La base de données cible, 'MyPublishedDB', participe à un groupe de disponibilité et n'est actuellement pas accessible pour les requêtes. Le déplacement des données est alors suspendu ou le réplica de disponibilité n'est pas activé pour l'accès en lecture. Pour autoriser l'accès en lecture seule à cette base de données et à d'autres dans le groupe de disponibilité, activez l'accès en lecture sur un ou plusieurs réplicas de disponibilité secondaires dans le groupe.  Pour plus d’informations, consultez l’instruction **ALTER AVAILABILITY GROUP** dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
>   
>  Une ou plusieurs erreurs de validation de serveur de publication ont été rencontrées pour l'hôte de réplica 'MyReplicaHostName'.  
  
## <a name="permissions"></a>Autorisations  
 L’appelant doit être un membre de la **sysadmin** rôle serveur fixe le **db_owner** rôle de base de données fixe pour la base de données de distribution ou un membre d’une liste d’accès à une publication définie associée à la base de données du serveur de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_get_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)  
  
  
