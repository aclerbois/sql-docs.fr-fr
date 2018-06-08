---
title: Alertes | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent alerts, event types
- SQL Server Agent alerts, names
- alerts [SQL Server], performance conditions
- alerts [SQL Server], about alerts
- WMI event responses [SQL Server Agent alerts]
- events [WMI]
- performance condition alerts [SQL Server Agent]
- alerts [SQL Server], event types
- SQL Server Agent alerts, performance conditions
- SQL Server Agent alerts, about alerts
- alerts [SQL Server], names
ms.assetid: 3f57d0f0-4781-46ec-82cd-b751dc5affef
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 70abaa95f0ef8f87501e6951c081aba086943808
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alerts"></a>Alertes
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Les événements sont générés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et entrés dans le journal des applications [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent lit le journal des applications et compare les événements qui y sont écrits aux alertes que vous avez définies. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent trouve une correspondance, il déclenche une alerte, qui est une réponse automatisée à un événement. Outre l'analyse des événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent peut aussi analyser les conditions de performance, ainsi que les événements WMI (Windows Management Instrumentation).  
  
Pour définir une alerte, vous spécifiez :  
  
-   Le nom de l'alerte.  
  
-   L'événement ou la condition de performance qui déclenche l'alerte.  
  
-   L'action qu'exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent en réponse à l'événement ou à la condition de performance.  
  
## <a name="naming-an-alert"></a>Désignation d'une alerte  
Chaque alerte doit posséder un nom. Les noms d’alertes doivent être uniques dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et ne peuvent excéder **128** caractères.  
  
## <a name="selecting-an-event-type"></a>Sélection d'un type d'événement  
Une alerte est une réponse à un événement d'un type spécifique. Les alertes répondent aux types d'événements suivants :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Événements  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Conditions de performance  
  
-   Événements WMI  
  
Le type d'événement détermine les paramètres que vous utilisez pour spécifier l'événement en question.  
  
## <a name="specifying-a-sql-server-event"></a>Spécification d'un événement SQL Server  
Vous pouvez préciser qu'une alerte doit avoir lieu en réponse à un ou plusieurs événements. Pour spécifier les événements qui déclenchent une alerte, utilisez les paramètres suivants :  
  
-   **Numéro d'erreur**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent déclenche une alerte quand une erreur spécifique se produit. Par exemple, vous pouvez spécifier le numéro d'erreur 2571 pour répondre aux tentatives non autorisées d'appels de commandes de console de base de données (DBCC).  
  
-   **Niveau de gravité**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent déclenche une alerte quand se produit une erreur du niveau de gravité indiqué. Ainsi, vous pouvez spécifier un niveau de gravité de 15 pour répondre aux erreurs de syntaxe dans les instructions Transact-SQL.  
  
-   **Sauvegarde de la base de données**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent ne déclenche une alerte que si l’événement se produit dans une base de données déterminée. Cette option s'applique en plus du numéro d'erreur ou du niveau de gravité. Par exemple, si une instance contient une base de données utilisée pour la production et une autre servant à la génération de rapports, vous pouvez définir une alerte répondant aux erreurs de syntaxe rencontrées uniquement dans la base de données de production.  
  
-   **Texte de l'événement**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent déclenche une alerte si le message de l’événement spécifié contient une chaîne de texte déterminée. Par exemple, vous pouvez définir une alerte en réponse aux messages contenant le nom d'une table donnée ou une contrainte déterminée.  
  
## <a name="selecting-a-performance-condition"></a>Sélection d'une condition de performances  
Vous pouvez préciser qu'une alerte doit avoir lieu en réponse à une condition de performances déterminée. Dans ce cas, vous indiquez le compteur de performances qui doit être surveillé, un seuil d'alerte et le comportement que doit afficher le compteur si l'alerte se produit. Pour définir une condition de performances, vous devez définir les éléments suivants dans la page [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Général **de la boîte de dialogue** Nouvelle alerte **ou** Propriétés de l’alerte **de** Agent :  
  
-   **Objet**  
  
    L'objet est l'élément de performance à surveiller.  
  
-   **Compteur**  
  
    Un compteur est un attribut de l'élément à surveiller.  
  
-   **Instance**  
  
    L'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] définit l'instance spécifique de l'attribut à surveiller, le cas échéant.  
  
-   **Alerte si le compteur** et **Valeur**  
  
    Indique le seuil d'alerte et le comportement qui déclenche l'alerte. Le seuil est une valeur numérique. Le comportement est l’un **des suivants : tombe sous**, **devient égal à**ou **s’élève au-dessus d’une valeur numérique déterminée**. La **valeur** est un nombre qui décrit le compteur des conditions de performances. Par exemple, pour définir le déclenchement d’une alerte pour l’objet de performances **SQLServer:Locks** quand le **Temps d’attente des verrous** dépasse 30 minutes, vous choisiriez **s’élève au-dessus** et **indiqueriez une valeur de 30**.  
  
    De même, vous pourriez définir le déclenchement d’une alerte pour l’objet de performance **SQLServer:Transactions** quand l’espace disponible dans **tempdb** tombe en dessous de 1000 Ko. Pour définir ceci, choisissez le compteur **Espace disponible dans tempdb (Ko)**, **tombe sous**, et une **valeur** de **1000**.  
  
    > [!NOTE]  
    > Les données de performances sont régulièrement échantillonnées, ce qui peut entraîner un léger décalage (de quelques secondes) entre l'atteinte du seuil et le déclenchement de l'alerte de performance.  
  
## <a name="selecting-a-wmi-event"></a>Sélection d'un événement WMI  
Vous pouvez préciser qu'une alerte doit avoir lieu en réponse à un événement WMI déterminé. Pour sélectionner un événement WMI, vous devez définir les éléments suivants dans la page [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Général **de la boîte de dialogue** Nouvelle alerte **ou** Propriétés de l’alerte **de** Agent :  
  
-   **Espace de noms**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent s’inscrit en tant que client WMI dans l’espace de noms WMI fourni pour rechercher des événements.  
  
-   **Requête**  
  
    [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent utilise l’instruction WQL (WMI Query Language) fournie pour identifier l’événement en question.  
  
Vous trouverez ci-dessous des liens traitant des tâches courantes :  
  
**Pour créer une alerte en fonction d'un numéro de message**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-an-error-number.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/d9b41853-e22d-4813-a79f-57efb4511f09)  
  
**Pour créer une alerte en fonction de niveaux de gravité**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-an-alert-using-severity-level.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/d9b41853-e22d-4813-a79f-57efb4511f09)  
  
**Pour créer une alerte en fonction d'un événement WMI**  
  
-   [SQL Server Management Studio](../../ssms/agent/create-a-wmi-event-alert.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/d9b41853-e22d-4813-a79f-57efb4511f09)  
  
**Pour définir la réponse à une alerte**  
  
-   [SQL Server Management Studio](../../ssms/agent/define-the-response-to-an-alert-sql-server-management-studio.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/0525e0a2-ed0b-4e69-8a4c-a9e3e3622fbd)  
  
**Pour créer un message d'erreur d'événement défini par l'utilisateur**  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/54746d30-f944-40e5-a707-f2d9be0fb9eb)  
  
**Pour modifier un message d'erreur d'événement défini par l'utilisateur**  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/1b28f280-8ef9-48e9-bd99-ec14d79abaca)  
  
**Pour supprimer un message d'erreur d'événement défini par l'utilisateur**  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/17287a15-cdde-43d1-bb18-9f920bc15db8)  
  
**Pour désactiver ou réactiver une alerte**  
  
-   [SQL Server Management Studio](../../ssms/agent/disable-or-reactivate-an-alert.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/4bbaeaab-8aca-4c9e-abc1-82ce73090bd3)  
  
## <a name="see-also"></a> Voir aussi  
[sp_update_alert (Transact-SQL)](http://msdn.microsoft.com/en-us/bcd731b1-3c4e-4086-b58a-af7a3af904ad)  
  
