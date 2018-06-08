---
title: 'Exemple : Création d’une alerte de l’Agent SQL Server avec le fournisseur WMI | Documents Microsoft'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: wmi
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Agent [WMI]
- WMI Provider for Server Events, samples
- sample applications [WMI]
ms.assetid: d44811c7-cd46-4017-b284-c863ca088e8f
caps.latest.revision: 16
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 9f99fcb708788c996847161b36c138cdfa65994b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sample-creating-a-sql-server-agent-alert-with-the-wmi-provider"></a>Exemple : Création d’une alerte de l’Agent SQL Server avec le fournisseur WMI
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Une façon courante d'utiliser le fournisseur d'événements WMI consiste à créer des alertes de l'Agent SQL Server qui répondent à des événements spécifiques. L'exemple suivant présente une alerte simple qui enregistre les événements du graphique de blocage XML dans une table pour leur analyse ultérieure. L'Agent SQL Server soumet une demande WQL, reçoit des événements WMI et exécute un travail en réponse à l'événement. Remarquez que, bien que plusieurs objets Service Broker soient impliqués dans le traitement du message de notification, le fournisseur d'événements WMI gère les détails de la création et de la gestion de ces objets.  
  
## <a name="example"></a>Exemple  
 En premier lieu, une table est créée dans la base de données `AdventureWorks` pour contenir l'événement du graphique du blocage. La table contient deux colonnes : la colonne `AlertTime` contient l'heure à laquelle l'alerte s'exécute et la colonne `DeadlockGraph` contient le document XML qui inclut le graphique du blocage.  
  
 Ensuite, l'alerte est créée. Le script commence par créer le travail que l'alerte exécutera, ajoute une étape de travail au travail et cible le travail sur l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le script crée alors l'alerte.  
  
 L’étape de travail récupère le **TextData** propriété de l’instance d’événement WMI et insère cette valeur dans la **DeadlockGraph** colonne de la **DeadlockEvents** table. Remarquez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit implicitement la chaîne au format XML. Comme l'étape de travail utilise le sous-système [!INCLUDE[tsql](../../includes/tsql-md.md)], l'étape de travail ne spécifie pas de proxy.  
  
 L'alerte exécute le travail chaque fois qu'un événement de trace du graphique du blocage est consigné. Pour une alerte WMI, l'Agent SQL Server crée une requête de notification à l'aide de l'espace de noms et de l'instruction WQL spécifiés. Pour cette alerte, l'Agent SQL Server analyse l'instance par défaut sur l'ordinateur local. L'instruction WQL demande un événement `DEADLOCK_GRAPH` quelconque dans l'instance par défaut. Pour modifier l'instance que l'alerte surveille, substituez le nom de l'instance pour `MSSQLSERVER` dans le `@wmi_namespace` pour l'alerte.  
  
> [!NOTE]  
>  Pour l’Agent SQL Server recevoir les événements WMI, [!INCLUDE[ssSB](../../includes/sssb-md.md)] doit être activé dans **msdb** et [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks ;  
GO  
  
IF OBJECT_ID('DeadlockEvents', 'U') IS NOT NULL  
BEGIN  
    DROP TABLE DeadlockEvents ;  
END ;  
GO  
  
CREATE TABLE DeadlockEvents  
    (AlertTime DATETIME, DeadlockGraph XML) ;  
GO  
-- Add a job for the alert to run.  
  
EXEC  msdb.dbo.sp_add_job @job_name=N'Capture Deadlock Graph',   
    @enabled=1,   
    @description=N'Job for responding to DEADLOCK_GRAPH events' ;  
GO  
  
-- Add a jobstep that inserts the current time and the deadlock graph into  
-- the DeadlockEvents table.  
  
EXEC msdb.dbo.sp_add_jobstep  
    @job_name = N'Capture Deadlock Graph',  
    @step_name=N'Insert graph into LogEvents',  
    @step_id=1,   
    @on_success_action=1,   
    @on_fail_action=2,   
    @subsystem=N'TSQL',   
    @command= N'INSERT INTO DeadlockEvents  
                (AlertTime, DeadlockGraph)  
                VALUES (getdate(), N''$(ESCAPE_SQUOTE(WMI(TextData)))'')',  
    @database_name=N'AdventureWorks' ;  
GO  
  
-- Set the job server for the job to the current instance of SQL Server.  
  
EXEC msdb.dbo.sp_add_jobserver @job_name = N'Capture Deadlock Graph' ;  
GO  
  
-- Add an alert that responds to all DEADLOCK_GRAPH events for  
-- the default instance. To monitor deadlocks for a different instance,  
-- change MSSQLSERVER to the name of the instance.  
  
EXEC msdb.dbo.sp_add_alert @name=N'Respond to DEADLOCK_GRAPH',   
@wmi_namespace=N'\\.\root\Microsoft\SqlServer\ServerEvents\MSSQLSERVER',   
    @wmi_query=N'SELECT * FROM DEADLOCK_GRAPH',   
    @job_name='Capture Deadlock Graph' ;  
GO  
```  
  
## <a name="testing-the-sample"></a>Test de l'exemple  
 Pour voir le travail s'exécuter, provoquez un blocage. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez deux **requête SQL** onglets et connecter les deux requêtes à la même instance. Exécutez le script ci-dessous sous l'un des onglets de requête. Ce script produit un jeu de résultats et se termine.  
  
```  
USE AdventureWorks ;  
GO  
  
BEGIN TRANSACTION ;  
GO  
  
SELECT TOP(1) Name FROM Production.Product WITH (XLOCK) ;  
GO  
```  
  
 Exécutez le script ci-dessous sous le second onglet de requête. Ce script produit un jeu de résultats, puis se bloque, en attendant d'acquérir un verrou sur `Production.Product`.  
  
```  
USE AdventureWorks ;  
GO  
  
BEGIN TRANSACTION ;  
GO  
  
SELECT TOP(1) Name FROM Production.Location WITH (XLOCK) ;  
GO  
  
SELECT TOP(1) Name FROM Production.Product WITH (XLOCK) ;  
GO  
```  
  
 Exécutez le script ci-dessous sous le premier onglet de requête. Ce script se bloque, en attendant d'acquérir un verrou sur `Production.Location`. Après un court délai d'attente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] choisira soit ce script, soit le script dans l'exemple comme victime du blocage et mettra un terme à la transaction.  
  
```  
SELECT TOP(1) Name FROM Production.Location WITH (XLOCK) ;  
GO  
```  
  
 Après avoir provoqué le blocage, attendez un certain temps que l'Agent SQL Server active l'alerte et exécute le travail. Examinez le contenu de la table `DeadlockEvents` en exécutant le script suivant :  
  
```  
SELECT * FROM DeadlockEvents ;  
GO  
```  
  
 La colonne `DeadlockGraph` doit contenir un document XML qui indique toutes les propriétés de l'événement du graphique du blocage.  
  
## <a name="see-also"></a>Voir aussi  
 [Fournisseur WMI pour les concepts des événements de serveur](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)  
  
  
