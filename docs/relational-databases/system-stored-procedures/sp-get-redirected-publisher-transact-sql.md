---
title: sp_get_redirected_publisher (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
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
- sp_get_redirected_publisher_TSQL
- sp_get_redirected_publisher
ms.assetid: d47a9ab5-f2cc-42a8-8be9-a33895ce44f0
caps.latest.revision: 10
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 28785969ea0bab2319d52461aa3e9a60f9be916d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spgetredirectedpublisher-transact-sql"></a>sp_get_redirected_publisher (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Utilisée par les agents de réplication pour interroger un serveur de distribution afin de déterminer si le serveur de publication d'origine a été redirigé.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_get_redirected_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @bypass_publisher_validation = ] [0 | 1 ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@original_publisher** =] **'***original_publisher***'**  
 Nom de la base de données publiée. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [ **@publisher_db** = ] **'***publisher_db***'**  
 Nom de la base de données publiée. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [ **@bypass_publisher_validation** = ] [0 | 1 ]  
 Utilisé pour ignorer la validation du serveur de publication redirigé. Si 0, la validation est effectuée. Si la valeur est 1, aucune validation n'est effectuée. *bypass_publisher_validation* est **bits**, avec 0 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**redirected_publisher**|**sysname**|Nom du serveur de publication après redirection.|  
|**error_number**|**int**|Numéro de l'erreur de validation.|  
|**error_severity**|**int**|Gravité de l'erreur de validation.|  
|**error_message**|**nvarchar(4000)**|Texte du message d'erreur de validation.|  
  
## <a name="remarks"></a>Notes  
 *redirected_publisher* renvoie le nom de serveur de publication actuel. Retourne la valeur null si le serveur de publication et les bases de données de publication n'ont pas été redirigés à l’aide de **sp_redirect_publisher**.  
  
 Si la validation n’est pas requise ou si aucune entrée n’existe pour le serveur de publication et de la base de données de publication, *error_number* et *error_severity* retournent 0 et *error_message* retourne la valeur null.  
  
 Si la validation est requise, la procédure stockée de la validation [sp_validate_redirected_publisher &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md) est appelée pour vérifier que la cible de la redirection est un hôte approprié pour la publication base de données. Si la validation réussit, **sp_get_redirected_publisher** retourne le nom du serveur de publication redirigé, 0 pour le *error_number* et *error_severity* colonnes et la valeur null dans le *error_message* colonne.  
  
 Si la validation est requise et échoue, le nom du serveur de publication redirigé est retourné avec les informations d'erreur.  
  
## <a name="permissions"></a>Autorisations  
 L’appelant doit être un membre de la **sysadmin** rôle serveur fixe le **db_owner** rôle de base de données fixe pour la base de données de distribution ou un membre d’une liste d’accès à une publication définie associée à la base de données du serveur de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
