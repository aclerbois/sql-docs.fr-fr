---
title: sp_repldone (Transact-SQL) | Documents Microsoft
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
- sp_repldone
- sp_repldone_TSQL
helpviewer_keywords:
- sp_repldone
ms.assetid: 045d3cd1-712b-44b7-a56a-c9438d4077b9
caps.latest.revision: 19
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: b06b31afb6daae2f6faa8436f0ec6ed88ef494ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sprepldone-transact-sql"></a>sp_repldone (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Met à jour l'enregistrement identifiant la dernière transaction distribuée du serveur. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!CAUTION]  
>  Si vous exécutez **sp_repldone** manuellement, vous pouvez invalider l’ordre et la cohérence des transactions délivrées. **sp_repldone** doit uniquement être utilisé pour la résolution des problèmes de réplication comme indiqué par un professionnel expérimenté de la réplication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_repldone [ @xactid= ] xactid   
        , [ @xact_seqno= ] xact_seqno   
    [ , [ @numtrans= ] numtrans ]   
    [ , [ @time= ] time   
    [ , [ @reset= ] reset ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@xactid=**] *xactid*  
 Est le numéro de séquence du journal (LSN) du premier enregistrement de la dernière transaction distribuée du serveur. *xactid* est **Binary (10)**, sans valeur par défaut.  
  
 [  **@xact_seqno=**] *xact_seqno*  
 Est le numéro LSN du dernier enregistrement de la dernière transaction distribuée du serveur. *xact_seqno* est **Binary (10)**, sans valeur par défaut.  
  
 [  **@numtrans=**] *numtrans*  
 Est le nombre de transactions distribuées. *numtrans* est **int**, sans valeur par défaut.  
  
 [  **@time=**] *heure*  
 Nombre de millisecondes, le cas échéant, nécessaires pour distribuer la dernière série de transactions. *temps* est **int**, sans valeur par défaut.  
  
 [  **@reset=**] *réinitialiser*  
 État de la réinitialisation. *réinitialiser* est **int**, sans valeur par défaut. Si **1**, tous les répliquées dans le journal des transactions sont marquées comme distribuées. Si **0**, le journal des transactions est réinitialisé à la première transaction répliquée et aucune transaction répliquée n’est marquées comme distribuées. *réinitialiser* est valide uniquement lorsque les deux *xactid* et *xact_seqno* ont la valeur NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_repldone** est utilisé dans la réplication transactionnelle.  
  
 **sp_repldone** est utilisé par le processus de lecture du journal pour suivre les transactions qui ont été distribuées.  
  
 Avec **sp_repldone**, vous pouvez informer manuellement le serveur qu’une transaction a été répliquée (envoyée au serveur de distribution). Elle vous permet également de modifier la transaction marquée comme étant la prochaine en attente de réplication. Il est possible d'avancer et de reculer dans la liste des transactions répliquées. (Toutes les transactions antérieures ou simultanées à cette transaction seront signalées comme distribuées).  
  
 Les paramètres requis *xactid* et *xact_seqno* peut être obtenu à l’aide de **sp_repltrans** ou **sp_replcmds**.  
  
## <a name="permissions"></a>Autorisations  
 Membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_repldone**.  
  
## <a name="examples"></a>Exemples  
 Lorsque *xactid* est NULL, *xact_seqno* est NULL, et *réinitialiser* est **1**, tous les répliquées dans le journal des transactions sont marquées comme distribuées. Cette fonction est utile lorsque il y a des transactions répliquées situées dans le journal des transactions qui ne sont plus valides et que vous désirez tronquer le journal, par exemple :  
  
```  
EXEC sp_repldone @xactid = NULL, @xact_segno = NULL, @numtrans = 0,     @time = 0, @reset = 1  
```  
  
> [!CAUTION]  
>  Cette procédure peut être utilisée dans les situations d'urgence pour permettre la troncature du journal des transactions lorsque des transactions en attente de réplication sont présentes.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_replflush &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [sp_repltrans &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
