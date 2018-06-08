---
title: Exécuter SQL Server Profiler | Documents Microsoft
ms.custom: ''
ms.date: 7/7/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.suite: sql
ms.technology: profiler
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Profiler [SQL Server Profiler], starting
- SQL Server Profiler, starting
- starting SQL Server Profiler
- Profiler [SQL Server Profiler], running
- SQL Server Profiler, running
- running SQL Server Profiler
ms.assetid: 22e57ffa-63b0-4de3-b92e-df297dda1226
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 250ea8227a4bd045883322ba2fec3e923336a5e2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="run-sql-server-profiler"></a>Exécuter SQL Server Profiler
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez exécuter [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] de différentes façons pour prendre en charge la collecte des sorties de trace dans différents scénarios. Vous pouvez démarrer [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] depuis le menu **Démarrer** de Windows 10, depuis le menu **Outils** de l’Assistant Paramétrage de [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou depuis plusieurs autres emplacements dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Quand vous démarrez [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour la première fois et que vous sélectionnez **Nouvelle trace** dans le menu **Fichier**, l’application affiche la boîte de dialogue **Se connecter au serveur**, où vous pouvez spécifier l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous voulez vous connecter.  
## <a name="to-start-sql-server-profiler-from-the-windows-10-start-menu"></a>Pour démarrer le SQL Server Profiler à partir du menu Démarrer de Windows 10  
-  Cliquez sur les fenêtres **Démarrer** ou appuyez sur les fenêtres de clé et commencent à taper « SQL Server Profiler 17 ». Lorsque le **SQL Server Profiler 17** vignette s’affiche, cliquez dessus.   

## <a name="to-start-sql-server-profiler-in-database-engine-tuning-advisor"></a>Pour démarrer le Générateur de profils SQL Server dans l'Assistant Paramétrage du moteur de base de données  
-  Dans le menu [!INCLUDE[ssDE](../../includes/ssde-md.md)] Outils **de l’Assistant Paramétrage du** , cliquez sur **SQL Server Profiler**.  

## <a name="to-start-sql-server-profiler-in-sql-server-management-studio"></a>Pour démarrer le Générateur de profils SQL Server dans SQL Server Management Studio  
 Vous pouvez démarrer [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] depuis plusieurs emplacements dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Quand [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] démarre, il charge le contexte de connexion, le modèle de trace et le contexte du filtre de son point de lancement. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] démarre chaque session de SQL Server Profiler dans sa propre instance et Profiler continue de s’exécuter si vous arrêtez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
### <a name="to-start-sql-server-profiler-from-the-tools-menu"></a>Pour démarrer le Générateur de profils SQL Server à partir du menu Outils  
-  Dans le menu [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Tools** menu, click **SQL Server Profiler**.  

### <a name="to-start-sql-server-profiler-from-the-query-editor"></a>Pour démarrer le Générateur de profils SQL Server à partir de l’Éditeur de requête  
- Dans l’Éditeur de requête, cliquez avec le bouton droit, puis sélectionnez **Requête de trace dans SQL Server Profiler**.  

  > [!NOTE]  
  >  Le contexte de connexion est la connexion d'éditeur, le modèle de trace est TSQL_SPs, et le filtre appliqué est SPID = SPID de la fenêtre de requête.  
    
### <a name="to-start-sql-server-profiler-from-activity-monitor"></a>Pour démarrer le Générateur de profils SQL Server à partir du moniteur d'activité  
- Dans le Moniteur d’activité, cliquez sur le volet **Processus**, cliquez avec le bouton droit sur le processus à profiler, puis cliquez sur **Tracer le processus dans SQL Server Profiler**.  

    > [!NOTE]  
    >  Lorsqu'un processus est sélectionné, le contexte de connexion correspond à la connexion de l'Explorateur d'objets à l'ouverture du moniteur d'activité. Le modèle de trace correspond à la valeur par défaut selon le type de serveur et le SPID est égal au SPID du processus sélectionné.  
    
## <a name="net-framework-security"></a>Sécurité du .NET Framework  
- Avec le mode d’authentification Windows, le compte d’utilisateur qui exécute [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] doit avoir l’autorisation de se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- Pour effectuer une opération de traçage à l'aide du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], les utilisateurs doivent également avoir l'autorisation ALTER TRACE.  

## <a name="next-steps"></a>Étapes suivantes  
 [Vue d’ensemble de SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [Utiliser SQL Server Management Studio](http://msdn.microsoft.com/library/f289e978-14ca-46ef-9e61-e1fe5fd593be)  
