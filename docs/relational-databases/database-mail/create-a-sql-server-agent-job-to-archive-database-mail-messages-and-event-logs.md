---
title: Créer un travail de SQL Server Agent pour archiver les messages et les journaux d’événements de la messagerie de base de données | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: database-mail
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- archiving mail messages and attachments [SQL Server]
- removing mail messages and attachements
- Database Mail [SQL Server], archiving
- saving mail messages and attachments
ms.assetid: 8f8f0fba-f750-4533-9b76-a9cdbcdc3b14
caps.latest.revision: 19
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 3aa7cfc10721e6d596b1f021c09b0a9b118cddc1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs"></a>Créer un travail d'Agent SQL Server pour archiver les messages et les journaux d'événements de la messagerie de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Des copies des messages de la messagerie de base de données et de leurs pièces jointes sont conservées dans les tables **msdb** avec le journal d'événements de la messagerie de base de données. Il peut être utile d'archiver périodiquement les messages et les événements dont vous n'avez plus besoin afin de réduire la taille des tables. Les procédures suivantes permettent de créer un travail de l'Agent SQL Server pour automatiser le processus.  
  
-   **Avant de commencer :**  , [Conditions préalables](#Prerequisites), [Recommandations](#Recommendations), [Autorisations](#Permissions)  
  
-   **Pour archiver les messages et les journaux de la messagerie de base de données à l'aide de :**  [Agent SQL Server](#Process_Overview)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Les nouvelles tables de stockage des données d'archive peuvent se trouver dans une base de données d'archive spéciale. Vous pouvez également exporter les lignes vers un fichier texte.  
   
###  <a name="Recommendations"></a> Recommandations  
 Dans votre environnement de production, vous pouvez ajouter des fonctionnalités supplémentaires de vérification des erreurs et faire envoyer un message électronique aux opérateurs en cas d'échec du travail.  
  
  
###  <a name="Permissions"></a> Permissions  
 Vous devez être membre du rôle serveur fixe **sysadmin** pour pouvoir exécuter les procédures stockées décrites dans cette rubrique.  
  
  
###  <a name="Process_Overview"></a> Vue d'ensemble du processus  
  
-   La première procédure crée un travail intitulé Archiver la messagerie de base de données avec les étapes suivantes.  
  
    1.  Copiez tous les messages contenus dans les tables de la messagerie de base de données vers une nouvelle table nommée d’après le mois précédent au format **DBMailArchive_***<année_mois>*.  
  
    2.  Copiez les pièces jointes associées aux messages copiés à la première étape, à partir des tables de la messagerie de base de données vers une nouvelle table nommée d’après le mois précédent au format **DBMailArchive_Attachments_***<année_mois>*.  
  
    3.  Copiez les événements du journal des événements de la messagerie de base de données qui sont associés aux messages copiés à la première étape, à partir des tables de la messagerie de base de données vers une nouvelle table nommée d’après le mois précédent au format **DBMailArchive_Log_***<année_mois>*.  
  
    4.  Supprimez des tables de la messagerie de base de données les enregistrements des éléments de messagerie transférés.  
  
    5.  Supprimez du journal des événements de la messagerie de base de données les événements associés aux éléments de messagerie transférés.  
  
-   Planifiez une exécution périodique du travail.  
  
  
## <a name="to-create-a-sql-server-agent-job"></a>Pour créer un travail de l'Agent SQL Server  
  
1.  Dans l’Explorateur d’objets, développez l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez avec le bouton droit sur **Travaux**, puis cliquez sur **Nouveau travail**.  
  
2.  Dans la boîte de dialogue **Nouveau travail** , dans la zone **Nom** , tapez **Archiver la messagerie de base de données**.  
  
3.  Dans la zone **Propriétaire** , confirmez que le propriétaire est membre du rôle serveur fixe **sysadmin** .  
  
4.  Dans la zone **Catégorie** , cliquez sur **Maintenance de la base de données**.  
  
5.  Dans la zone **Description** , tapez **Archiver les messages de la messagerie de base de données**, puis cliquez sur **Étapes**.  
  
 [Vue d'ensemble](#Process_Overview)  
  
## <a name="to-create-a-step-to-archive-the-database-mail-messages"></a>Pour créer une étape permettant d'archiver les messages de la messagerie de base de données  
  
1.  Dans la page **Étapes** , cliquez sur **Nouveau**.  
  
2.  Dans la zone **Nom de l'étape** , tapez **Copier les éléments de la messagerie de base de données**.  
  
3.  Dans la zone **Type** , sélectionnez **Script Transact-SQL (T-SQL)**.  
  
4.  Dans la zone **Base de données** , sélectionnez **msdb**.  
  
5.  Dans la zone **Commande** , tapez l'instruction suivante pour créer une table nommée d'après le mois précédent, contenant les lignes antérieures au début du mois actuel :  
  
    ```sql  
    DECLARE @LastMonth nvarchar(12);  
    DECLARE @CopyDate nvarchar(20) ;  
    DECLARE @CreateTable nvarchar(250) ;  
    SET @LastMonth = (SELECT CAST(DATEPART(yyyy,GETDATE()) AS CHAR(4)) + '_' + CAST(DATEPART(mm,GETDATE())-1 AS varchar(2))) ;  
    SET @CopyDate = (SELECT CAST(CONVERT(char(8), CURRENT_TIMESTAMP- DATEPART(dd,GETDATE()-1), 112) AS datetime))  
    SET @CreateTable = 'SELECT * INTO msdb.dbo.[DBMailArchive_' + @LastMonth + '] FROM sysmail_allitems WHERE send_request_date < ''' + @CopyDate +'''';  
    EXEC sp_executesql @CreateTable ;  
    ```  
  
6.  Cliquez sur **OK** pour enregistrer l'étape.  
  
 [Vue d’ensemble](#Process_Overview)  
  
## <a name="to-create-a-step-to-archive-the-database-mail-attachments"></a>Pour créer une étape permettant d'archiver les pièces jointes de la messagerie de base de données  
  
1.  Dans la page **Étapes** , cliquez sur **Nouveau**.  
  
2.  Dans la zone **Nom de l'étape** , tapez **Copier les pièces jointes de la messagerie de base de données**.  
  
3.  Dans la zone **Type** , sélectionnez **Script Transact-SQL (T-SQL)**.  
  
4.  Dans la zone **Base de données** , sélectionnez **msdb**.  
  
5.  Dans la zone **Commande** , tapez l'instruction suivante pour créer une table de pièces jointes nommée d'après le mois précédent, contenant les pièces jointes qui correspondent aux messages transférés à l'étape précédente :  
  
    ```sql  
    DECLARE @LastMonth nvarchar(12);  
    DECLARE @CopyDate nvarchar(20) ;  
    DECLARE @CreateTable nvarchar(250) ;  
    SET @LastMonth = (SELECT CAST(DATEPART(yyyy,GETDATE()) AS CHAR(4)) + '_' + CAST(DATEPART(mm,GETDATE())-1 AS varchar(2))) ;  
    SET @CopyDate = (SELECT CAST(CONVERT(char(8), CURRENT_TIMESTAMP- DATEPART(dd,GETDATE()-1), 112) AS datetime))  
    SET @CreateTable = 'SELECT * INTO msdb.dbo.[DBMailArchive_Attachments_' + @LastMonth + '] FROM sysmail_attachments   
     WHERE mailitem_id in (SELECT DISTINCT mailitem_id FROM [DBMailArchive_' + @LastMonth + '] )';  
    EXEC sp_executesql @CreateTable ;  
    ```  
  
6.  Cliquez sur **OK** pour enregistrer l'étape.  
  
 [Vue d’ensemble](#Process_Overview)  
  
## <a name="to-create-a-step-to-archive-the-database-mail-log"></a>Pour créer une étape permettant d'archiver le journal de la messagerie de base de données  
  
1.  Dans la page **Étapes** , cliquez sur **Nouveau**.  
  
2.  Dans la zone **Nom de l'étape** , tapez **Copier le journal de la messagerie de base de données**.  
  
3.  Dans la zone **Type** , sélectionnez **Script Transact-SQL (T-SQL)**.  
  
4.  Dans la zone **Base de données** , sélectionnez **msdb**.  
  
5.  Dans la zone **Commande** , tapez l'instruction suivante pour créer une table de journal nommée d'après le mois précédent, contenant les entrées de journal qui correspondent aux messages transférés à l'étape antérieure :  
  
    ```sql  
    DECLARE @LastMonth nvarchar(12);  
    DECLARE @CopyDate nvarchar(20) ;  
    DECLARE @CreateTable nvarchar(250) ;  
    SET @LastMonth = (SELECT CAST(DATEPART(yyyy,GETDATE()) AS CHAR(4)) + '_' + CAST(DATEPART(mm,GETDATE())-1 AS varchar(2))) ;  
    SET @CopyDate = (SELECT CAST(CONVERT(char(8), CURRENT_TIMESTAMP- DATEPART(dd,GETDATE()-1), 112) AS datetime))  
    SET @CreateTable = 'SELECT * INTO msdb.dbo.[DBMailArchive_Log_' + @LastMonth + '] FROM sysmail_Event_Log   
     WHERE mailitem_id in (SELECT DISTINCT mailitem_id FROM [DBMailArchive_' + @LastMonth + '] )';  
    EXEC sp_executesql @CreateTable ;  
    ```  
  
6.  Cliquez sur **OK** pour enregistrer l'étape.  
  
 [Vue d’ensemble](#Process_Overview)  
  
## <a name="to-create-a-step-to-remove-the-archived-rows-from-database-mail"></a>Pour créer une étape permettant de supprimer les lignes archivées du journal de la messagerie de base de données  
  
1.  Dans la page **Étapes** , cliquez sur **Nouveau**.  
  
2.  Dans la zone **Nom de l'étape** , tapez **Supprimer les lignes de la messagerie de base de données**.  
  
3.  Dans la zone **Type** , sélectionnez **Script Transact-SQL (T-SQL)**.  
  
4.  Dans la zone **Base de données** , sélectionnez **msdb**.  
  
5.  Dans la zone **Commande** , tapez l'instruction suivante pour supprimer des tables de la messagerie de base de données les lignes antérieures au mois actuel :  
  
    ```sql  
    DECLARE @CopyDate nvarchar(20) ;  
    SET @CopyDate = (SELECT CAST(CONVERT(char(8), CURRENT_TIMESTAMP- DATEPART(dd,GETDATE()-1), 112) AS datetime)) ;  
    EXECUTE msdb.dbo.sysmail_delete_mailitems_sp @sent_before = @CopyDate ;  
    ```  
  
6.  Cliquez sur **OK** pour enregistrer l'étape.  
  
 [Vue d’ensemble](#Process_Overview)  
  
## <a name="to-create-a-step-to-remove-the-archived-items-from-database-mail-event-log"></a>Pour créer une étape permettant de supprimer les éléments archivés du journal des événements de la messagerie de base de données  
  
1.  Dans la page **Étapes** , cliquez sur **Nouveau**.  
  
2.  Dans la zone **Nom de l'étape** , tapez **Supprimer les lignes du journal des événements de la messagerie de base de données**.  
  
3.  Dans la zone **Type** , sélectionnez **Script Transact-SQL (T-SQL)**.  
  
4.  Dans la zone **Commande** , tapez l'instruction suivante pour supprimer du journal des événements de la messagerie de base de données les lignes antérieures au mois actuel :  
  
    ```sql  
    DECLARE @CopyDate nvarchar(20) ;  
    SET @CopyDate = (SELECT CAST(CONVERT(char(8), CURRENT_TIMESTAMP- DATEPART(dd,GETDATE()-1), 112) AS datetime)) ;  
    EXECUTE msdb.dbo.sysmail_delete_log_sp @logged_before = @CopyDate ;  
    ```  
  
5.  Cliquez sur **OK** pour enregistrer l'étape.  
  
 [Vue d’ensemble](#Process_Overview)  
  
## <a name="to-schedule-the-job-to-run-periodically"></a>Pour planifier une exécution périodique du travail  
  
1.  Dans la boîte de dialogue **Nouveau travail** , cliquez sur **Planifications**.  
  
2.  Dans la page **Planifications** , cliquez sur **Nouvelle**.  
  
3.  Dans la zone **Nom** , tapez **Archiver la messagerie de base de données**.  
  
4.  Dans la zone **Type de planification** , sélectionnez **Périodique**.  
  
5.  Dans la zone **Fréquence** , sélectionnez les options appropriées pour exécuter périodiquement le travail, par exemple une fois par mois.  
  
6.  Dans la zone **Fréquence quotidienne**, sélectionnez **Une fois à \<heure>**.  
  
7.  Vérifiez que les autres options sont configurées à votre convenance, puis cliquez sur **OK** pour enregistrer la planification.  
  
8.  Cliquez sur **OK** pour enregistrer le travail.  
  
 [Vue d’ensemble](#Process_Overview)  
  
  
