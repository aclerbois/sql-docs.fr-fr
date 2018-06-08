---
title: WAITFOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- WAITFOR
- WAITFOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- TIME option
- delaying executions [SQL Server]
- batches [SQL Server], execution times
- stored procedures [SQL Server], executing
- DELAY
- time [SQL Server], execution delays
- blocking executions
- transactions [SQL Server], execution times
- WAITFOR statement
- timing executions
ms.assetid: 8e896e73-af27-4cae-a725-7a156733f3bd
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: bbf4b982faa988290573368818f4d75961fd38c2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="waitfor-transact-sql"></a>WAITFOR (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Bloque l'exécution d'un traitement, d'une procédure stockée ou d'une transaction jusqu'à ce que l'heure ou l'intervalle de temps spécifié soit atteint ou qu'une instruction spécifiée modifie ou retourne au moins une ligne.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
WAITFOR   
{  
    DELAY 'time_to_pass'   
  | TIME 'time_to_execute'   
  | [ ( receive_statement ) | ( get_conversation_group_statement ) ]   
    [ , TIMEOUT timeout ]  
}  
```  
  
## <a name="arguments"></a>Arguments  
 DELAY  
 Période de temps qui doit s'écouler, dans la limite de 24 heures, avant la poursuite de l'exécution d'un traitement d'instructions, d'une procédure stockée ou d'une transaction.  
  
 '*time_to_pass*'  
 Durée de l'attente. *time_to_pass* peut être spécifié dans tout format accepté pour les données **datetime** ou en tant que variable locale. Vous ne pouvez pas spécifier de dates ; par conséquent, la partie date de la valeur **datetime** n’est pas autorisée. Elle est au format hh:mm[[:ss].mss].
  
 TIME  
 Heure spécifiée de l'exécution du traitement d'instructions, de la procédure stockée ou de la transaction.  
  
 '*time_to_execute*'  
 Heure à laquelle se termine l'instruction WAITFOR. *time_to_execute* peut être spécifié dans tout format accepté pour les données **datetime** ou en tant que variable locale. Vous ne pouvez pas spécifier de dates ; par conséquent, la partie date de la valeur **datetime** n’est pas autorisée. Elle est au format hh:mm[[:ss].mss] et peut facultativement inclure la date au format 1900-01-01.
  
 *receive_statement*  
 Instruction RECEIVE valide.  
  
> [!IMPORTANT]  
>  WAITFOR avec *receive_statement* s’applique uniquement aux messages [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Pour plus d’informations, consultez [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md).  
  
 *get_conversation_group_statement*  
 Instruction GET CONVERSATION GROUP valide.  
  
> [!IMPORTANT]  
>  WAITFOR avec *get_conversation_group_statement* s’applique uniquement aux messages [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Pour plus d’informations, consultez [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
 TIMEOUT *timeout*  
 Spécifie la période de temps, en millisecondes, pendant laquelle un message peut rejoindre la file d'attente.  
  
> [!IMPORTANT]  
>  Vous pouvez appliquer WAITFOR avec TIMEOUT uniquement aux messages [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Pour plus d’informations, consultez [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md) et [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
## <a name="remarks"></a>Notes   
 Pendant l'exécution de l'instruction WAITFOR, la transaction est exécutée et aucune autre demande ne peut s'exécuter sous cette même transaction.  
  
 Le délai réel peut être différent du délai spécifié dans *time_to_pass*, *time_to_execute* ou *timeout*, et il dépend du niveau d’activité du serveur. Le compteur démarre lorsque le thread associé à l'instruction WAITFOR est planifié. Si le serveur est occupé, il est possible que le thread ne soit pas immédiatement planifié ; par conséquent, le délai peut être supérieur à la durée spécifiée.  
  
 WAITFOR ne modifie pas la sémantique d'une requête. Si une requête ne peut pas retourner de lignes, WAITFOR attend indéfiniment ou jusqu'à ce que la valeur TIMEOUT soit atteinte, si celle-ci est spécifiée.  
  
 Vous ne pouvez pas ouvrir de curseurs sur les instructions WAITFOR.  
  
 Vous ne pouvez pas définir de vues sur les instructions WAITFOR.  
  
 Lorsque la requête dépasse la limite définie par l'option query wait, l'argument de l'instruction WAITFOR peut prendre fin sans être exécuté. Pour plus d’informations sur l’option de configuration, consultez [Configurer l’option de configuration du serveur query wait](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md). Pour afficher les processus actifs et en attente, utilisez [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md).  
  
 À chaque instruction WAITFOR est associé un thread. Si de nombreuses instructions WAITFOR sont spécifiées sur le même serveur, de nombreux threads peuvent être bloqués dans l'attente de l'exécution de ces instructions. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] surveille le nombre de threads associés aux instructions WAITFOR et en sélectionne certains aléatoirement afin de les libérer si le serveur commence à manquer de threads.  
  
 Vous pouvez créer un blocage en exécutant une requête avec WAITFOR dans une transaction qui détient des verrous empêchant la modification de l'ensemble de lignes auquel l'instruction WAITFOR essaie d'accéder. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifie ces scénarios et retourne un jeu de résultats vide si le risque d'un tel blocage existe.  
  
> [!CAUTION]  
>  Inclure WAITFOR affiche la fin du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et peut générer un message de délai de connexion dépassé dans l'application. Si nécessaire, réglez le paramètre de délai de connexion dépassé pour la connexion au niveau de l'application.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-waitfor-time"></a>A. Utilisation de WAITFOR TIME  
 L'exemple suivant exécute la procédure stockée `sp_update_job` dans la base de données msdb à 22 h 20. (`22:20`).  
  
```  
EXECUTE sp_add_job @job_name = 'TestJob';  
BEGIN  
    WAITFOR TIME '22:20';  
    EXECUTE sp_update_job @job_name = 'TestJob',  
        @new_name = 'UpdatedJob';  
END;  
GO  
```  
  
### <a name="b-using-waitfor-delay"></a>B. Utilisation de WAITFOR DELAY  
 L'exemple suivant exécute la procédure stockée après un délai de deux heures.  
  
```  
BEGIN  
    WAITFOR DELAY '02:00';  
    EXECUTE sp_helpdb;  
END;  
GO  
```  
  
### <a name="c-using-waitfor-delay-with-a-local-variable"></a>C. Utilisation de WAITFOR DELAY avec une variable locale  
 L'exemple suivant illustre l'utilisation d'une variable locale avec l'option `WAITFOR DELAY`. Une procédure stockée est créée, qui reste en attente pendant un certain temps puis retourne à l'utilisateur les informations relatives au nombre d'heures, de minutes et de secondes qui se sont écoulées.  
  
```  
IF OBJECT_ID('dbo.TimeDelay_hh_mm_ss','P') IS NOT NULL  
    DROP PROCEDURE dbo.TimeDelay_hh_mm_ss;  
GO  
CREATE PROCEDURE dbo.TimeDelay_hh_mm_ss   
    (  
    @DelayLength char(8)= '00:00:00'  
    )  
AS  
DECLARE @ReturnInfo varchar(255)  
IF ISDATE('2000-01-01 ' + @DelayLength + '.000') = 0  
    BEGIN  
        SELECT @ReturnInfo = 'Invalid time ' + @DelayLength   
        + ',hh:mm:ss, submitted.';  
        -- This PRINT statement is for testing, not use in production.  
        PRINT @ReturnInfo   
        RETURN(1)  
    END  
BEGIN  
    WAITFOR DELAY @DelayLength  
    SELECT @ReturnInfo = 'A total time of ' + @DelayLength + ',   
        hh:mm:ss, has elapsed! Your time is up.'  
    -- This PRINT statement is for testing, not use in production.  
    PRINT @ReturnInfo;  
END;  
GO  
/* This statement executes the dbo.TimeDelay_hh_mm_ss procedure. */  
EXEC TimeDelay_hh_mm_ss '00:00:10';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `A total time of 00:00:10, in hh:mm:ss, has elapsed. Your time is up.`  
  
## <a name="see-also"></a> Voir aussi  
 [Langage de contrôle de flux &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
  
