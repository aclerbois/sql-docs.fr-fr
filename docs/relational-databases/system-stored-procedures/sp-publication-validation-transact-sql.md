---
title: sp_publication_validation (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
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
- sp_publication_validation
- sp_publication_validation_TSQL
helpviewer_keywords:
- sp_publication_validation
ms.assetid: 06be2363-00c0-4936-97c1-7347f294a936
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f893c6a42d16c9d36d2a28a2c77c80bc62fa56bb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sppublicationvalidation-transact-sql"></a>sp_publication_validation (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Lance une demande de validation pour chaque article de la publication spécifiée. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_publication_validation [ @publication = ] 'publication'  
    [ , [ @rowcount_only = ] type_of_check_requested ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [**@publication=**] **' *** publication'*  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [**@rowcount_only=**] *rowcount_only*  
 Indique s'il faut ou non retourner uniquement le nombre de lignes de la table. *rowcount_only* est **smallint** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Effectue une somme de contrôle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0.<br /><br /> Remarque : Lorsqu’un article est filtré horizontalement, une opération du nombre de lignes est effectuée à la place d’une opération de somme de contrôle.|  
|**1** (par défaut)|Effectue un contrôle du nombre de lignes uniquement.|  
|**2**|Effectue un comptage du nombre de lignes et une somme de contrôle binaire.<br /><br /> Remarque : Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés version 7.0, seule la validation du nombre de lignes est effectuée.|  
  
 [**@full_or_fast=**] *full_or_fast*  
 Méthode utilisée pour calculer le nombre de lignes. *full_or_fast* est **tinyint** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Effectue un comptage total à l'aide de COUNT(*).|  
|**1**|Effectue un comptage de rapide **sysindexes.rows**. Le décompte de lignes [sys.sysindexes](../../relational-databases/system-compatibility-views/sys-sysindexes-transact-sql.md) est beaucoup plus rapide que le décompte de lignes dans la table. Toutefois, étant donné que [sys.sysindexes](../../relational-databases/system-compatibility-views/sys-sysindexes-transact-sql.md) est immédiatement mis à jour, le nombre de lignes peut être inexact.|  
|**2** (par défaut)|Exécute un décompte rapide conditionnel en essayant d'abord la méthode rapide. Si la méthode rapide affiche des différences, revient à la méthode totale. Si *expected_rowcount* a la valeur NULL et la procédure stockée est en cours d’utilisation pour obtenir la valeur, un Count complète est toujours utilisée.|  
  
 [  **@shutdown_agent=**] *shutdown_agent*  
 Indique si l'Agent de distribution doit se fermer immédiatement à la fin de la validation. *shutdown_agent* est **bits**, avec une valeur par défaut **0**. Si **0**, l’agent de réplication ne s’arrête pas. Si **1**, l’agent de réplication s’arrête après le dernier article est validé.  
  
 [ **@publisher** =] **'***publisher***'**  
 Spécifie un serveur de publication non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé lors de la demande de validation sur un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_publication_validation** est utilisé dans la réplication transactionnelle.  
  
 **sp_publication_validation** peut être appelée à tout moment après ont activé les articles associés à la publication. La procédure peut être exécutée manuellement (une fois) ou en tant que partie d'une tâche régulièrement planifiée qui valide les données.  
  
 Si votre application comporte une mise à jour immédiate des abonnés, **sp_publication_validation** peut détecter des erreurs inexistantes. **sp_publication_validation** calcule d’abord le nombre de lignes ou la somme de contrôle sur le serveur de publication, puis sur l’abonné. Étant donné que les déclencheurs à mise à jour immédiate peuvent propager une mise à jour de l'Abonné vers le serveur de publication une fois que le calcul du nombre de lignes ou de la somme de contrôle est terminé au niveau du serveur de publication, mais avant qu'il soit effectué au niveau de l'Abonné, les valeurs peuvent changer. Pour vous assurer que les valeurs pour l'Abonné et le serveur de publication ne changent pas au cours de la validation d'une publication, arrêtez le service Microsoft Distributed Transaction Coordinator (MS DTC) sur le serveur de publication durant cette validation.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_publication_validation**.  
  
## <a name="see-also"></a>Voir aussi  
 [Valider des données sur l’abonné](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)   
 [sp_table_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-validation-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
