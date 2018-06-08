---
title: CREATE QUEUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- QUEUE_TSQL
- CREATE QUEUE
- QUEUE
- CREATE_QUEUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE QUEUE statement
- internal activation [Service Broker]
- stored procedure activation [Service Broker]
- message retention [Service Broker]
- unavailable queues [Service Broker]
- activation stored procedures [Service Broker]
- queues [Service Broker], creating
ms.assetid: fce80faf-2bdc-475d-8ca1-31438ed41fb0
caps.latest.revision: 67
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 23def6b6c02b49ae953c68a9e927de516582a605
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="create-queue-transact-sql"></a>CREATE QUEUE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée une file d'attente dans une base de données. Les files d'attente stockent les messages. Lorsqu'un message destiné à un service se présente, [!INCLUDE[ssSB](../../includes/sssb-md.md)] le place dans la file d'attente associée au service.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
CREATE QUEUE <object>  
   [ WITH  
     [ STATUS = { ON | OFF }  [ , ] ]  
     [ RETENTION = { ON | OFF } [ , ] ]   
     [ ACTIVATION (  
         [ STATUS = { ON | OFF } , ]   
           PROCEDURE_NAME = <procedure> ,  
           MAX_QUEUE_READERS = max_readers ,   
           EXECUTE AS { SELF | 'user_name' | OWNER }   
            ) [ , ] ]  
     [ POISON_MESSAGE_HANDLING (  
         [ STATUS = { ON | OFF } ] ) ] 
    ]  
     [ ON { filegroup | [ DEFAULT ] } ]  
[ ; ]  
  
<object> ::=  
{  
    [ database_name. [ schema_name ] . | schema_name. ]  
        queue_name  
}   
  
<procedure> ::=  
{  
    [ database_name. [ schema_name ] . | schema_name. ]  
        stored_procedure_name  
}  
  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name* (objet)  
 Nom de la base de données dans laquelle la file d'attente doit être créée. *database_name* doit spécifier le nom d’une base de données existante. Quand *database_name* n’est pas fourni, la file d’attente est créée dans la base de données active.  
  
 *schema_name* (objet)  
 Nom du schéma auquel la nouvelle file d'attente appartient. Le schéma par défaut est le schéma par défaut de l'utilisateur exécutant l'instruction. *schema_name* peut indiquer un autre schéma que celui associé au nom de la connexion active si l’instruction CREATE QUEUE est exécutée par un membre du rôle serveur fixe sysadmin ou par un membre du rôle de base de données fixe db_dbowner ou db_ddladmin dans la base de données spécifiée par *database_name*. Sinon, *schema_name* doit être le schéma par défaut de l’utilisateur qui exécute l’instruction.  
  
 *queue_name*  
 Nom de la file d'attente à créer. Ce nom doit respecter les règles définies pour les identificateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 STATUS (File d'attente)  
 Indique si la file d'attente est disponible (ON) ou indisponible (OFF). Lorsque la file d'attente est indisponible, aucun message ne peut y être ajouté ou supprimé. Vous pouvez créer une file d'attente indisponible pour empêcher l'arrivée des messages jusqu'au moment où cette file d'attente est rendue disponible avec l'instruction ALTER QUEUE. Si cette clause est omise, la valeur par défaut est ON et la file d'attente est disponible.  
  
 RETENTION  
 Spécifie le paramètre de rétention pour la file d'attente. Si RETENTION = ON (activé), tous les messages des conversations qui ont été envoyés ou reçus sont conservés dans leur file d'attente jusqu'à ce que les conversations s'achèvent. Vous pouvez ainsi garder ces messages pour effectuer des audits ou procéder à des transactions de compensation si une erreur se produit. Si cette clause n'est pas spécifiée, la valeur par défaut du paramètre de rétention est OFF.  
  
> [!NOTE]  
>  Les performances peuvent se trouver altérées si RETENTION = ON. Ce paramètre doit être utilisé seulement si l'application le nécessite.  
  
 ACTIVATION  
 Spécifie des informations sur la procédure stockée à démarrer pour traiter les messages dans cette file d'attente.  
  
 STATUS (Activation)  
 Spécifie si [!INCLUDE[ssSB](../../includes/sssb-md.md)] démarre ou non la procédure stockée. Lorsque STATUS = ON, la file d'attente lance la procédure stockée spécifiée avec PROCEDURE_NAME, quand le nombre de procédures actuellement en cours d'exécution est inférieur à la valeur de MAX_QUEUE_READERS et que la réception des messages dans la file d'attente est plus rapide que la réception des messages par les procédures stockées. Lorsque STATUS = OFF, la file d'attente ne démarre pas la procédure stockée. Quand cette clause est omise, la valeur par défaut est ON.  
  
 PROCEDURE_NAME = \<procedure>  
 Spécifie le nom de la procédure stockée à démarrer pour traiter les messages dans cette file d'attente. Cette valeur doit être un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *database_name* (procédure)  
 Nom de la base de données contenant la procédure stockée.  
  
 *schema_name* (procédure)  
 Nom du schéma qui contient la procédure stockée.  
  
 *procedure_name*  
 Nom de la procédure stockée.  
  
 MAX_QUEUE_READERS =*max_readers*  
 Spécifie le nombre maximal d'instances de la procédure stockée d'activation lancées simultanément par la file d'attente. La valeur de *max_readers* doit être comprise entre **0** et **32 767**.  
  
 EXECUTE AS  
 Spécifie le compte d'utilisateur de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous lequel la procédure stockée d'activation s'exécute. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de contrôler les autorisations de cet utilisateur au moment où la file d'attente démarre la procédure stockée. Pour un utilisateur de domaine, le serveur doit être connecté au domaine lorsque la procédure est démarrée, sinon l'activation échoue. Pour un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le serveur peut toujours vérifier les autorisations.  
  
 SELF  
 Spécifie que la procédure stockée s'exécute en tant qu'utilisateur actuel. (Le principal de la base de données exécutant cette instruction CREATE QUEUE.)  
  
 '*user_name*'  
 Nom de l'utilisateur sous lequel la procédure stockée s'exécute. Le paramètre *user_name* doit être un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide spécifié en tant qu’identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’utilisateur actuel doit disposer de l’autorisation IMPERSONATE pour la valeur *user_name* spécifiée.  
  
 OWNER  
 Spécifie que la procédure stockée s'exécute en tant que propriétaire de la file d'attente.  
  
 POISON_MESSAGE_HANDLING  
 Spécifie si la gestion des messages incohérents est activée pour la file d'attente. La valeur par défaut est ON.  
  
 Une file d'attente dont la gestion des messages incohérents a la valeur OFF ne sera pas désactivée après cinq restaurations de transactions consécutives. L'application peut ainsi définir un système personnalisé de gestion des messages incohérents.  
  
 ON *filegroup |* [**DEFAULT**]  
 Spécifie le groupe de fichiers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur lequel créer cette file d'attente. Vous pouvez utiliser ce paramètre *filegroup* pour identifier un groupe de fichiers ou utiliser l’identificateur DEFAULT pour utiliser le groupe de fichiers par défaut pour la base de données Service Broker. Dans le contexte de cette clause, DEFAULT n'est pas un mot clé et il doit être délimité comme un identificateur. Quand aucun groupe de fichiers n'est spécifié, la file d'attente utilise le groupe de fichiers par défaut pour la base de données.  
  
## <a name="remarks"></a>Notes   
 Une file d'attente peut être la cible d'une instruction SELECT. Toutefois, le contenu d'une file d'attente est uniquement modifiable par le biais d'instructions s'exécutant sur des conversations [!INCLUDE[ssSB](../../includes/sssb-md.md)], par exemple SEND, RECEIVE et END CONVERSATION. Une file d'attente ne peut pas être la cible d'une instruction INSERT, UPDATE, DELETE ou TRUNCATE.  
  
 Une file d'attente ne peut pas être un objet temporaire. Les noms de file d’attente commençant par **#** ne sont donc pas valides.  
  
 La création d'une file d'attente inactive permet de mettre en place l'infrastructure d'un service avant d'autoriser la réception de messages dans cette file d'attente.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] n'arrête pas les procédures stockées d'activation lorsqu'il n'existe aucun message dans la file d'attente. Une procédure stockée d'activation doit s'achever quand aucun message n'est disponible dans la file d'attente après un court laps de temps.  
  
 Les autorisations correspondant à la procédure stockée d'activation sont vérifiées lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] démarre la procédure stockée et non lorsque la file d'attente est créée. L'instruction CREATE QUEUE ne vérifie pas si l'utilisateur spécifié dans la clause EXECUTE AS dispose de l'autorisation d'exécution de la procédure stockée spécifiée dans la clause PROCEDURE NAME.  
  
 Lorsqu'une file d'attente est indisponible, [!INCLUDE[ssSB](../../includes/sssb-md.md)] conserve les messages destinés aux services qui utilisent cette file dans la file d'attente de transmission de la base de données. L'affichage catalogue sys.transmission_queue donne une vue de la file d'attente de transmission.  
  
 Une file d'attente est un objet appartenant à un schéma. Les files d'attente apparaissent dans l'affichage catalogue sys.objects.  
  
 Le tableau suivant donne la liste des colonnes d'une file d'attente.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|status|**tinyint**|État du message. L’instruction RECEIVE retourne tous les messages pour lesquels status est égal à **1**. Si la rétention des messages est activée, status a la valeur 0. Si la rétention des messages est désactivée, le message est supprimé de la file d'attente. Dans la file d’attente, les messages peuvent contenir l’une des valeurs suivantes :<br /><br /> **0** = Message reçu conservé<br /><br /> **1** = Prêt à recevoir<br /><br /> **2** = Pas encore terminé<br /><br /> **3** = Message envoyé conservé|  
|priority|**tinyint**|Niveau de priorité assigné à ce message.|  
|queuing_order|**bigint**|Numéro d'ordre du message dans la file d'attente.|  
|conversation_group_id|**uniqueidentifier**|Identificateur du groupe de conversations auquel ce message appartient.|  
|conversation_handle|**uniqueidentifier**|Descripteur de conversation dont ce message fait partie.|  
|message_sequence_number|**bigint**|Numéro de séquence du message dans la conversation.|  
|service_name|**nvarchar(512)**|Nom du service auquel la conversation est destinée.|  
|service_id|**Int**|Identificateur d'objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du service auquel la conversation est destinée.|  
|service_contract_name|**nvarchar (256)**|Nom du contrat suivi par la conversation.|  
|service_contract_id|**Int**|Identificateur d'objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du contrat suivi par la conversation.|  
|message_type_name|**nvarchar (256)**|Nom du type de message décrivant le message.|  
|message_type_id|**Int**|Identificateur d'objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du type de message décrivant le message.|  
|validation|**nchar(2)**|Validation utilisée pour le message.<br /><br /> E=Vide<br /><br /> N=Aucune<br /><br /> X=XML|  
|message_body|**varbinary(max)**|Contenu du message.|  
|message_id|**uniqueidentifier**|Identificateur unique du message.|  
  
## <a name="permissions"></a>Autorisations  
 L'autorisation de création d'une file d'attente est accordée aux membres du rôle de base de données fixe db_ddladmin ou db_owner et aux membres du rôle serveur fixe sysadmin.  
  
 L'autorisation REFERENCES pour une file d'attente est accordée par défaut au propriétaire de la file d'attente, aux membres du rôle de base de données fixe db_ddladmin ou db_owner et aux membres du rôle serveur fixe sysadmin.  
  
 L'autorisation RECEIVE pour une file d'attente est accordée par défaut au propriétaire de la file d'attente, aux membres du rôle de base de données fixe db_owner et aux membres du rôle serveur fixe sysadmin.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-queue-with-no-parameters"></a>A. Création d'une file d'attente sans paramètres  
 L'exemple suivant crée une file d'attente disponible pour la réception de messages. Aucune procédure stockée d'activation n'est spécifiée pour la file d'attente.  
  
```  
CREATE QUEUE ExpenseQueue ;  
```  
  
### <a name="b-creating-an-unavailable-queue"></a>B. Création d'une file d'attente indisponible  
 L'exemple suivant crée une file d'attente indisponible pour la réception de messages. Aucune procédure stockée d'activation n'est spécifiée pour la file d'attente.  
  
```  
CREATE QUEUE ExpenseQueue WITH STATUS=OFF ;  
```  
  
### <a name="c-creating-a-queue-and-specify-internal-activation-information"></a>C. Création d'une file d'attente et spécification d'informations d'activation internes  
 L'exemple suivant crée une file d'attente disponible pour la réception de messages. La file d'attente lance la procédure stockée `expense_procedure` quand un message entre dans la file d'attente. Cette procédure stockée s'exécute en tant qu'utilisateur `ExpenseUser`. La file d'attente démarre au maximum `5` instances de la procédure stockée.  
  
```  
CREATE QUEUE ExpenseQueue  
    WITH STATUS=ON,  
    ACTIVATION (  
        PROCEDURE_NAME = expense_procedure,  
        MAX_QUEUE_READERS = 5,  
        EXECUTE AS 'ExpenseUser' ) ;  
```  
  
### <a name="d-creating-a-queue-on-a-specific-filegroup"></a>D. Création d'une file d'attente dans un groupe de fichiers spécifique  
 L'exemple suivant crée une file d'attente dans le groupe de fichiers `ExpenseWorkFileGroup`.  
  
```  
CREATE QUEUE ExpenseQueue  
    ON ExpenseWorkFileGroup ;  
```  
  
### <a name="e-creating-a-queue-with-multiple-parameters"></a>E. Création d'une file d'attente avec plusieurs paramètres  
 L’exemple suivant crée une file d’attente dans le groupe de fichiers `DEFAULT`. La file d'attente n'est pas disponible. Les messages sont conservés dans la file d'attente jusqu'à ce que la conversation dont ils font partie se termine. Lorsque la file d'attente est rendue disponible via l'instruction ALTER QUEUE, elle démarre la procédure stockée `2008R2.dbo.expense_procedure` pour traiter les messages. La procédure stockée s'exécute sous l'utilisateur qui a lancé l'instruction `CREATE QUEUE`. La file d'attente démarre au maximum `10` instances de la procédure stockée.  
  
```  
CREATE QUEUE ExpenseQueue  
    WITH STATUS = OFF,  
      RETENTION = ON,  
      ACTIVATION (  
          PROCEDURE_NAME = AdventureWorks2012.dbo.expense_procedure,  
          MAX_QUEUE_READERS = 10,  
          EXECUTE AS SELF )  
    ON [DEFAULT] ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
