---
title: Configurer l’accès en lecture seule sur un réplica de disponibilité (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 22387419-22c4-43fa-851c-5fecec4b049b
caps.latest.revision: 35
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0bbeaa627bc1183fa6bdcb72538887132a5ba2b3
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34768995"
---
# <a name="configure-read-only-access-on-an-availability-replica-sql-server"></a>Configurer l'accès en lecture seule sur un réplica de disponibilité (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Par défaut l’accès en lecture/écriture et l’intention de lecture sont autorisés sur le réplica principal et aucune connexion directe n’est autorisée sur les réplicas secondaires d’un groupe de disponibilité Always On. Cette rubrique explique comment configurer l’accès à la connexion sur un réplica de disponibilité d’un groupe de disponibilité Always On dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou PowerShell.  
  
 Pour plus d’informations sur les conséquences de l’activation de l’accès en lecture seule pour un réplica secondaire et pour obtenir une présentation de l’accès à la connexion, consultez [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md) et [Secondaires actifs : réplicas secondaires lisibles &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
-   **Avant de commencer :**  
  
     [Conditions préalables requises et restrictions](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'accès sur un réplica de disponibilité à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’accès en lecture seule pour un réplica de disponibilité](#FollowUp)  
  
-   [Tâches associées](#RelatedTasks)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables requises et restrictions  
  
-   Pour configurer un autre accès à la connexion, vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
  
|Tâche|Autorisations|  
|----------|-----------------|  
|Pour configurer des réplicas lors de la création d'un groupe de disponibilité|Requiert le rôle serveur fixe **sysadmin** et l’autorisation de serveur CREATE AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER.|  
|Pour modifier un réplica de disponibilité :|Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.|  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour configurer l'accès sur un réplica de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez sur le groupe de disponibilité dont vous souhaitez modifier le réplica.  
  
4.  Cliquez avec le bouton droit sur le réplica de disponibilité, puis cliquez sur **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés du réplica de disponibilité** , vous pouvez modifier l'accès à la connexion pour le rôle principal et le rôle secondaire, comme suit :  
  
    -   Pour le rôle secondaire, sélectionnez une nouvelle valeur dans la liste déroulante **Rôle secondaire accessible en lecture** , comme suit :  
  
         **Non**  
         Aucune connexion utilisateur n'est autorisée aux bases de données secondaires de ce réplica. Elles ne sont pas disponibles pour l'accès en lecture. Il s'agit du paramètre par défaut.  
  
         **Tentative de lecture uniquement**  
         Seules les connexions en lecture seule sont autorisées aux bases de données secondaires de ce réplica. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
         **Oui**  
         Toutes les connexions sont autorisées aux bases de données secondaires de ce réplica, mais uniquement pour l'accès en lecture. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
    -   Pour le rôle principal, sélectionnez une nouvelle valeur dans la liste déroulante **Connexions en rôle principal** , comme suit :  
  
         **Autoriser toutes les connexions**  
         Toutes les connexions aux bases de données sont autorisées dans le réplica principal. Il s'agit du paramètre par défaut.  
  
         **Autoriser les connexions en lecture/écriture**  
         Lorsque la propriété d'intention de l'application a la valeur **ReadWrite** ou si cette propriété n'est pas définie, la connexion est autorisée. Les connexions où la propriété de connexion d'intention de l'application a la valeur **ReadOnly** ne sont pas autorisées. Cela peut permettre d'éviter aux clients de se connecter à une charge de travail de tentative de lecture au réplica primaire par erreur. Pour plus d'informations sur la propriété de connexion d'intention de l'application, consultez [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour configurer l'accès sur un réplica de disponibilité**  
  
> [!NOTE]  
>  Pour obtenir un exemple de cette procédure, consultez [Exemple (Transact-SQL)](#TsqlExample)plus loin dans cette section.  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Si vous spécifiez un réplica pour un nouveau groupe de disponibilité, utilisez l'instruction [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Si vous ajoutez ou modifiez un réplica d’un groupe de disponibilité existant, utilisez l’instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Pour configurer l'accès à la connexion pour le rôle secondaire, dans la clause ADD REPLICA ou MODIFY REPLICA WITH, spécifiez l'option SECONDARY_ROLE, comme suit :  
  
         SECONDARY_ROLE **(** ALLOW_CONNECTIONS **=** { NO | READ_ONLY | ALL } **)**  
  
         où :  
  
         Non  
         Aucune connexion directe n'est autorisée aux bases de données secondaires de ce réplica. Elles ne sont pas disponibles pour l'accès en lecture. Il s'agit du paramètre par défaut.  
  
         READ_ONLY  
         Seules les connexions en lecture seule sont autorisées aux bases de données secondaires de ce réplica. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
         ALL  
         Toutes les connexions sont autorisées aux bases de données secondaires de ce réplica, mais uniquement pour l'accès en lecture. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
3.  Pour configurer l'accès à la connexion pour le rôle principal, dans la clause ADD REPLICA ou MODIFY REPLICA WITH, spécifiez l'option PRIMARY_ROLE, comme suit :  
  
     PRIMARY_ROLE **(** ALLOW_CONNECTIONS **=** { READ_WRITE | ALL } **)**  
  
     où :  
  
     READ_WRITE  
     Les connexions où la propriété de connexion d’intention de l’application a la valeur **ReadOnly** ne sont pas autorisées.  Lorsque la propriété d'intention de l'application a la valeur **ReadWrite** ou si cette propriété n'est pas définie, la connexion est autorisée. Pour plus d'informations sur la propriété de connexion d'intention de l'application, consultez [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
     ALL  
     Toutes les connexions aux bases de données sont autorisées dans le réplica principal. Il s'agit du paramètre par défaut.  
  
###  <a name="TsqlExample"></a> Exemple (Transact-SQL)  
 L'exemple suivant ajoute un réplica secondaire à un groupe de disponibilité nommé *AG2*. Une instance de serveur autonome, *COMPUTER03\HADR_INSTANCE*, est spécifiée pour héberger le nouveau réplica de disponibilité. Ce réplica est configuré pour autoriser uniquement les connexions en lecture/écriture pour le rôle principal et pour autoriser uniquement les connexions de tentative de lecture pour le rôle secondaire.  
  
```  
ALTER AVAILABILITY GROUP AG2   
   ADD REPLICA ON   
      'COMPUTER03\HADR_INSTANCE' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03:7022',  
         PRIMARY_ROLE ( ALLOW_CONNECTIONS = READ_WRITE ),  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY )  
         );   
GO  
```  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour configurer l'accès sur un réplica de disponibilité**  
  
> [!NOTE]  
>  Pour obtenir un exemple de code, consultez [Exemple (PowerShell)](#PSExample)plus loin dans cette section.  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Quand vous ajoutez un réplica de disponibilité à un groupe de disponibilité, utilisez l’applet de commande **New-SqlAvailabilityReplica** . Quand vous modifiez un réplica de disponibilité existant, utilisez l’applet de commande **Set-SqlAvailabilityReplica** . Les paramètres pertinents sont les suivants :  
  
    -   Pour configurer l’accès à la connexion pour le rôle secondaire, spécifiez le paramètre **ConnectionModeInSecondaryRole***mot_clé_rôle_secondaire*, où *mot_clé_rôle_secondaire* correspond à une des valeurs suivantes :  
  
         **AllowNoConnections**  
         Aucune connexion directe n'est autorisée aux bases de données dans le réplica secondaire et les bases de données ne sont pas disponibles pour un accès en lecture. Il s'agit du paramètre par défaut.  
  
         **AllowReadIntentConnectionsOnly**  
         Seules sont autorisées les connexions aux bases de données dans le réplica secondaire où la propriété d’intention de l’application est définie sur **ReadOnly**. Pour plus d'informations sur cette propriété, consultez [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
         **AllowAllConnections**  
         Toutes les connexions sont autorisées aux bases de données dans le réplica secondaire pour un accès en lecture seule.  
  
    -   Pour configurer l’accès à la connexion pour le rôle principal, spécifiez **ConnectionModeInPrimaryRole***mot_clé_rôle_principal*, où *mot_clé_rôle_principal* correspond à une des valeurs suivantes :  
  
         **AllowReadWriteConnections**  
         Les connexions où la propriété de connexion d'intention de l'application a la valeur ReadOnly ne sont pas autorisées. Lorsque la propriété d'intention de l'application a la valeur ReadWrite ou si cette propriété n'est pas définie, la connexion est autorisée. Pour plus d'informations sur la propriété de connexion d'intention de l'application, consultez [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
         **AllowAllConnections**  
         Toutes les connexions aux bases de données sont autorisées dans le réplica principal. Il s'agit du paramètre par défaut.  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../relational-databases/scripting/get-help-sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../relational-databases/scripting/sql-server-powershell-provider.md)  
  
###  <a name="PSExample"></a> Exemple (PowerShell)  
 L'exemple suivant définit les paramètres **ConnectionModeInSecondaryRole** et **ConnectionModeInPrimaryRole** sur **AllowAllConnections**.  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
Set-SqlAvailabilityReplica -ConnectionModeInSecondaryRole "AllowAllConnections" `   
-InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ConnectionModeInPrimaryRole "AllowAllConnections" `   
-InputObject $primaryReplica  
  
```  
  
##  <a name="FollowUp"></a> Suivi : après avoir configuré l'accès en lecture seule pour un réplica de disponibilité  
 **Accès en lecture seule aux réplicas secondaires accessibles en lecture.**  
  
-   Quand vous utilisez [bcp Utility](../../../tools/bcp-utility.md) ou [sqlcmd Utility](../../../tools/sqlcmd-utility.md), vous pouvez spécifier l’accès en lecture seule à n’importe quel réplica secondaire qui est activé pour l’accès en lecture seule en spécifiant le commutateur **-K ReadOnly** .  
  
-   Pour permettre aux applications clientes de se connecter aux réplicas secondaires accessibles en lecture :  
  
    ||Condition préalable|Lien|  
    |-|------------------|----------|  
    |![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Assurez-vous que le groupe de disponibilité possède un écouteur.|[Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
    |![Case à cocher](../../../database-engine/availability-groups/windows/media/checkboxemptycenterxtraspacetopandright.gif "Case à cocher")|Configurez le routage en lecture seule pour le groupe de disponibilité.|[Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)|  
  
 **Facteurs qui peuvent affecter les déclencheurs et les travaux à la suite d'un basculement**  
  
 Si vous avez des déclencheurs et des travaux qui vont échouer lors de l'exécution sur une base de données secondaire inaccessible en lecture ou sur une base de données secondaire accessible en lecture, vous devez générer un script pour les déclencheurs et travaux à vérifier sur un réplica donné afin de déterminer si la base de données est une base de données principale ou une base de données secondaire accessible en lecture. Pour obtenir ces informations, utilisez la fonction [DATABASEPROPERTYEX](../../../t-sql/functions/databasepropertyex-transact-sql.md) pour retourner la propriété **Updatability** de la base de données. Pour identifier une base de données en lecture seule, spécifiez la valeur READ_ONLY, comme suit :  
  
```  
DATABASEPROPERTYEX([db name],’Updatability’) = N’READ_ONLY’  
```  
  
 Pour identifier une base de données en lecture-écriture, spécifiez la valeur READ_WRITE.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [Always On : Proposition de valeur du réplica secondaire accessible en lecture](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-value-proposition-of-readable-secondary.aspx)  
  
-   [Always On : Pourquoi existe-t-il deux options pour activer un réplica secondaire pour la charge de travail en lecture ?](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-why-there-are-two-options-to-enable-a-secondary-replica-for-read-workload.aspx)  
  
-   [Always On : Installation du réplica de secondaire accessible en lecture](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-setting-up-readable-seconary-replica.aspx)  
  
-   [Always On : J’ai activé un réplica secondaire accessible en lecture mais ma requête est bloquée](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-i-just-enabled-readble-secondary-but-my-query-is-blocked.aspx)  
  
-   [Always On : Mise à disposition des dernières statistiques disponibles sur le réplica secondaire accessible en lecture, dans la base de données en lecture seule et dans l’instantané de base de données](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-making-upto-date-statistics-available-on-readable-secondary-read-only-database-and-database-snapshot.aspx)  
  
-   [Always On : Problèmes avec des statistiques dans la base de données en lecture seule, l’instantané de base de données et le réplica secondaire](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-challenges-with-statistics-on-readonly-database-database-snapshot-and-secondary-replica.aspx)  
  
-   [Always On : Impact sur la charge de travail principale lorsque vous exécutez la charge de travail de rapports sur le réplica secondaire](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-impact-on-the-primary-workload-when-you-run-reporting-workload-on-the-secondary-replica.aspx)  
  
-   [Always On : Impact de la charge de travail de création de rapports de mappage sur le réplica secondaire accessible en lecture à l’isolement d’instantané](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-impact-of-mapping-reporting-workload-to-snapshot-isolation-on-readable-secondary.aspx)  
  
-   [Always On : Réduction des blocages de thread REDO lors de l’exécution d’une charge de travail de rapports sur le réplica secondaire](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On-minimizing-blocking-of-redo-thread-when-running-reporting-workload-on-secondary-replica.aspx)  
  
-   [Always On : réplica secondaire accessible en lecture et latence des données](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2011/12/22/Always%20On.aspx)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Secondaires actifs : réplicas secondaires lisibles &#40;Groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
  
