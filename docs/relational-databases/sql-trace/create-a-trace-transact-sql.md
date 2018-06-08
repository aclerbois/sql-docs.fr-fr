---
title: Créer une trace (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: sql-trace
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- traces [SQL Server], example
- traces [SQL Server], creating
ms.assetid: 79dd4254-e3c6-467a-bb6f-f99e51757e99
caps.latest.revision: 19
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: a187fc923c2dc47a178d4a1cecc8bc851a072598
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-trace-transact-sql"></a>Créer une trace (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment créer une trace à l'aide de procédures stockées.  
  
### <a name="to-create-a-trace"></a>Pour créer une trace  
  
1.  Exécutez **sp_trace_create** avec les paramètres nécessaires afin de créer une nouvelle trace. La nouvelle trace est à l’état arrêté (*status* a la valeur **0**).  
  
2.  Exécutez **sp_trace_setevent** avec les paramètres requis pour choisir les événements et les colonnes que vous voulez tracer.  
  
3.  Vous pouvez aussi exécuter **sp_trace_setfilter** pour définir un ou plusieurs filtres.  
  
     **sp_trace_setevent** et **sp_trace_setfilter** ne peuvent être exécutées que sur des traces existantes arrêtées.  
  
    > [!IMPORTANT]  
    >  Contrairement aux procédures stockées standard, les paramètres de toutes les procédures stockées de SQL Server Profiler (**sp_trace_* xx***) sont strictement typés et ne prennent pas en charge la conversion automatique des types de données. Si ces paramètres ne sont pas appelés à l'aide des types de données appropriés pour les paramètres d'entrée tels qu'ils sont spécifiés dans la description de l'argument, la procédure stockée retourne une erreur.  
  
## <a name="example"></a> Exemple  
 L'exemple de code suivant montre comment créer une trace à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il est divisé en trois sections : création de la trace, remplissage du fichier de trace et arrêt de la trace. Personnalisez la trace en ajoutant les événements dont vous souhaitez effectuer le suivi. Pour obtenir la liste des événements et des colonnes, consultez [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).  
  
 Le code suivant crée une trace, y ajoute des événements, puis la démarre :  
  
```  
DECLARE @RC int, @TraceID int, @on BIT  
EXEC @rc = sp_trace_create @TraceID output, 0, N'C:\SampleTrace'  
  
-- Select the return code to see if the trace creation was successful.  
SELECT RC = @RC, TraceID = @TraceID  
  
-- Set the events and data columns you need to capture.  
SELECT @on = 1  
  
-- 10 is RPC:Completed event. 1 is TextData column.   
EXEC sp_trace_setevent @TraceID, 10, 1, @on   
-- 13 is SQL:BatchStarting, 11 is LoginName  
EXEC sp_trace_setevent @TraceID, 13, 11, @on   
-- 13 is SQL:BatchStarting, 14 is StartTime  
EXEC sp_trace_setevent @TraceID, 13, 14, @on   
-- 12 is SQL:BatchCompleted, 15 is EndTime  
EXEC sp_trace_setevent @TraceID, 12, 15, @on   
-- 13 is SQL:BatchStarting, 1 is TextData  
EXEC sp_trace_setevent @TraceID, 13, 1, @on   
  
-- Set any filter. Not provided in this example  
--EXEC sp_trace_setfilter 1, 10, 0, 6, N'%Profiler%'  
  
-- Start Trace (status 1 = start)  
EXEC @RC = sp_trace_setstatus @TraceID, 1  
GO  
  
```  
  
## <a name="example"></a> Exemple  
 Maintenant que la trace a été créée et démarrée, exécutez le code ci-dessous pour la remplir avec l'activité.  
  
```  
SELECT * FROM master.sys.databases  
GO  
SELECT * FROM ::fn_trace_getinfo(default)  
GO  
  
```  
  
## <a name="example"></a> Exemple  
 La trace peut être arrêtée et redémarrée à tout moment. Dans cet exemple, exécutez le code ci-dessous pour arrêter et fermer la trace, puis supprimer sa définition.  
  
```  
DECLARE @TraceID int  
-- Populate a variable with the trace_id of the current trace  
SELECT  @TraceID = TraceID FROM ::fn_trace_getinfo(default) WHERE VALUE = N'C:\SampleTrace.trc'  
  
-- First stop the trace.   
EXEC sp_trace_setstatus @TraceID, 0  
  
-- Close and then delete its definition from SQL Server.   
EXEC sp_trace_setstatus @TraceID, 2  
  
```  
  
## <a name="example"></a> Exemple  
 Pour examiner le fichier de trace, ouvrez le fichier SampleTrace.trc à l'aide de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
## <a name="see-also"></a> Voir aussi  
 [Procédures stockées de SQL Server Profiler &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)  
  
  
