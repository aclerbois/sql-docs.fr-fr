---
title: sp_redirect_publisher (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
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
- sp_redirect_publisher_TSQL
- sp_redirect_publisher
helpviewer_keywords:
- sp_redirect_publisher
ms.assetid: af45e2b2-57fb-4bcd-a58b-e61401fb3b26
caps.latest.revision: 14
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e8df59d709ef04d94626ac2ab6a3ba268d63ab76
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spredirectpublisher-transact-sql"></a>sp_redirect_publisher (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Spécifie un serveur de publication redirigé pour une paire serveur de publication/base de données existante. Si la base de données du serveur de publication appartient à un groupe de disponibilité AlwaysOn, le serveur de publication redirigé est le nom d’écouteur associé au groupe de disponibilité.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_redirect_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name'   
    [ , [ @redirected_publisher = ] 'new_publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@original_publisher** =] **'***original_publisher***'**  
 Nom de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a publié la base de données à l'origine. *original_publisher* est **sysname**, sans valeur par défaut.  
  
 [ **@publisher_db** = ] **'***publisher_db***'**  
 Nom de la base de données publiée. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [ **@redirected_publisher** =] **'***redirected_publisher***'**  
 Nom d'écouteur associé au groupe de disponibilité qui sera le nouveau serveur de publication. *redirected_publisher* est **sysname**, sans valeur par défaut. Lorsque l'écouteur du groupe de disponibilité est configuré sur un port non défini par défaut, spécifiez le numéro de port avec le nom d'écouteur, par exemple `'Listenername,51433'`  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_redirect_publisher** est utilisé pour autoriser un serveur de publication de réplication à être redirigées vers le serveur principal actuel d’un groupe de disponibilité Always On en associant la paire serveur de publication/base de données à un écouteur d’un groupe de disponibilité. Exécutez **sp_redirect_publisher** une fois que l’écouteur du groupe de disponibilité a été configuré pour le groupe de disponibilité qui contient la base de données publiée.  
  
 Si la base de données de publication sur le serveur de publication d’origine est supprimée d’un groupe de disponibilité du réplica principal, exécutez **sp_redirect_publisher** sans spécifier de valeur pour le *@redirected_publisher* paramètre à supprimer de la redirection de la paire serveur de publication/base de données. Pour plus d’informations sur la redirection de l’éditeur, consultez [maintenance une base de données de Publication AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md).  
  
## <a name="permissions"></a>Autorisations  
 L’appelant doit être un membre de la **sysadmin** rôle serveur fixe le **db_owner** rôle de base de données fixe pour la base de données de distribution ou un membre d’une liste d’accès à une publication définie associée à la base de données du serveur de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_get_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
