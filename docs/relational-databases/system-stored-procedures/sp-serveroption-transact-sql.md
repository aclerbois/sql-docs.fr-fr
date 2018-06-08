---
title: sp_serveroption (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_serveroption_TSQL
- sp_serveroption
dev_langs:
- TSQL
helpviewer_keywords:
- 7343 (Database Engine error)
- sp_serveroption
ms.assetid: 47d04a2b-dbf0-4f15-bd9b-81a2efc48131
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 99f936f0a8d127dd33ebce8b86c0a958cafccf66
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "33260945"
---
# <a name="spserveroption-transact-sql"></a>sp_serveroption (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Définit les options de serveur pour les serveurs distants et les serveurs liés.  
  
 
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_serveroption [@server = ] 'server'   
      ,[@optname = ] 'option_name'       
      ,[@optvalue = ] 'option_value' ;  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@server =** ] **'***server***'**  
 Nom du serveur pour lequel l'option doit être définie. *server* est de type **sysname**et n'a pas de valeur par défaut.  
  
 [  **@optname =** ] **'***option_name***'**  
 Option à définir pour le serveur spécifié. *option_name* est **varchar (** 35 **)**, sans valeur par défaut. *option_name* peut être une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**compatible avec le classement**|Concerne l'exécution des requêtes distribuées sur les serveurs liés. Si cette option est définie sur **true**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suppose que tous les caractères du serveur lié sont compatibles avec le serveur local, en ce qui concerne la séquence de classement et de jeu de caractères (ou ordre de tri). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut alors envoyer au fournisseur des comparaisons sur les colonnes de caractères. Si cette option n'est pas activée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compare toujours les colonnes de caractères en local.<br /><br /> Cette option ne doit être définie que si la source de données correspondant au serveur lié possède le même jeu de caractères et respecte le même ordre de tri que le serveur local.|  
|**Nom de classement**|Spécifie le nom du classement utilisé par la source de données distante si **utiliser le classement distant** est **true** et la source de données n’est pas un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source de données. Le nom doit être l'un des classements pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Utilisez cette option lors d'un accès à une source de données OLE DB autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais dont le classement correspond à l'un des classements de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Le serveur lié doit prendre en charge un classement unique utilisable pour toutes les colonnes du serveur. Ne définissez pas cette option si le serveur lié prend en charge plusieurs classements dans une source de données unique ou si le classement du serveur lié ne correspond peut-être pas à l'un des classements de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Délai de connexion**|Secondes valuein de délai d’attente pour la connexion à un serveur lié.<br /><br /> Si **0**, utilisez le **sp_configure** par défaut.|  
|**Accès aux données**|Active ou désactive un serveur lié pour l'accès des requêtes distribuées. Peut être utilisé uniquement pour **sys.server** entrées ajoutées via **sp_addlinkedserver**.|  
|**serveur de distribution**|Serveur de distribution.|  
|**validation de schéma différée**|Détermine si le schéma des tables distantes doit être vérifié.<br /><br /> Si **true**, ignorer la vérification du schéma des tables distantes au début de la requête.|  
|**pub**|Serveur de publication.|  
|**délai de requête**|Valeur du délai d'expiration des requêtes par rapport à un serveur lié.<br /><br /> Si **0**, utilisez le **sp_configure** par défaut.|  
|**rpc**|Active RPC à partir du serveur donné.|  
|**sortie RPC**|Active RPC vers le serveur donné.|  
|**sub**|Abonné.|  
|**system**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**utiliser le classement distant**|Détermine si le classement d'une colonne distante ou d'un serveur local doit être utilisé.<br /><br /> Si **true**, le classement des colonnes distantes est utilisé pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des sources de données et le classement spécifié dans **nom de classement** est utilisé pour non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des sources de données.<br /><br /> Si **false**, les requêtes distribuées utilisent toujours le classement par défaut du serveur local, tandis que **nom de classement** et le classement des colonnes distantes sont ignorés. La valeur par défaut est **false**. (Le **false** valeur est compatible avec la sémantique de classement utilisée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0.)|  
|**promotion des transactions de procédures**|Cette option permet de protéger les actions d'une procédure de serveur à serveur par le biais d'une transaction MS DTC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator). Lorsque cette option a la valeur TRUE (ou) appelant une procédure stockée distante démarre une transaction distribuée et inscrit la transaction avec MS DTC. L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelant la procédure stockée distante constitue l'élément créateur de la transaction et qui contrôle l'exécution jusqu'à son terme. Si une instruction COMMIT TRANSACTION ou ROLLBACK TRANSACTION est ensuite émise pour la connexion, le serveur de contrôle demande à MS DTC de gérer l'achèvement de la transaction distribuée sur tous les ordinateurs concernés.<br /><br /> Une fois qu'une transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)] a démarré, des appels de procédures stockées distantes peuvent être effectués envers d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ont été définies en tant que serveurs liés. Les serveurs liés sont tous inscrits dans la transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)] et MS DTC garantit que la transaction est exécutée jusqu'à son terme sur chaque serveur lié.<br /><br /> Si cette option a la valeur FALSE (ou OFF), une transaction locale ne sera pas promue en transaction distribuée lors de l'exécution d'un appel de procédure distante sur un serveur lié.<br /><br /> Si, avant d'exécuter un appel de procédure de serveur à serveur, la transaction est déjà une transaction distribuée, cette option n'a pas effet. L'appel de procédure sur le serveur lié s'exécutera sous la même transaction distribuée.<br /><br /> Si, avant d'exécuter un appel de procédure de serveur à serveur, aucune transaction n'est active dans la connexion, cette option n'a pas effet. La procédure s'exécute ensuite sur le serveur lié sans transactions actives.<br /><br /> La valeur par défaut de cette option est TRUE (ou ON).|  
  
 [  **@optvalue =**] **'***argument option_value***'**  
 Spécifie ou non le *option_name* doit être activée (**TRUE** ou **sur**) ou désactivé (**FALSE** ou **hors**). *l’argument option_value* est **varchar (** 10 **)**, sans valeur par défaut.  
  
 *l’argument option_value* peut être un entier non négatif pour le **délai de connexion** et **délai de requête** options. Pour le **nom de classement** option, *argument option_value* peut être un nom de classement ou une valeur NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Si le **compatible avec le classement** option est définie sur TRUE, **nom de classement** prend automatiquement la valeur null. Si **nom de classement** est défini sur une valeur non null, **compatible avec le classement** prend automatiquement la valeur False.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite une autorisation ALTER ANY LINKED SERVER sur le serveur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant configure un serveur lié correspondant à une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `SEATTLE3`, , de façon à ce qu'il soit compatible avec le classement de l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
USE master;  
EXEC sp_serveroption 'SEATTLE3', 'collation compatible', 'true';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Distributed des procédures stockées de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
