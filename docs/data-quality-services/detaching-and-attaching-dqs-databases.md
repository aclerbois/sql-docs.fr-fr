---
title: Attachement et détachement de bases de données DQS | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 830e33bc-dd15-4f8e-a4ac-d8634b78fe45
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e64c42a1e8df09f970be453cf85ad2359a69c368
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="detaching-and-attaching-dqs-databases"></a>Attachement et détachement de bases de données DQS

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment attacher et détacher les bases de données DQS.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Limitations"></a> Limitations et restrictions  
 Pour obtenir la liste des limitations et restrictions, consultez [Attacher et détacher une base de données &#40;SQL Server&#41;](../relational-databases/databases/database-detach-and-attach-sql-server.md).  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Vérifiez qu'aucune activité ou aucun processus n'est en cours d'exécution dans DQS. Pour ce faire, utilisez l'écran **Analyse des activités** . Pour plus d'informations sur l'utilisation de cet écran, consultez [Monitor DQS Activities](../data-quality-services/monitor-dqs-activities.md).  
  
-   Assurez-vous qu'aucun utilisateur n'est connecté au [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)].  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
  
-   Votre compte d'utilisateur Windows doit être membre du rôle serveur fixe db_owner dans l'instance SQL Server pour pouvoir détacher des bases de données DQS.  
  
-   Votre compte d'utilisateur Windows doit disposer d'une autorisation CREATE DATABASE, CREATE ANY DATABASE ou ALTER ANY DATABASE pour pouvoir attacher une base de données.  
  
-   Vous devez disposer du rôle dqs_administrator sur la base de données DQS_MAIN pour mettre fin à toutes les activités en cours d'exécution ou arrêter tous les processus en cours d'exécution dans DQS.  
  
##  <a name="Detach"></a> Détacher des bases de données DQS  
 Lorsque vous détachez une base de données DQS à l'aide de SQL Server Management Studio, les fichiers détachés restent sur votre ordinateur et peuvent être rattachés à la même instance de SQL Server ou être déplacés vers un autre serveur et être attachés à cet endroit. Les fichiers de base de données DQS se trouvent généralement à l’emplacement suivant sur votre ordinateur Data Quality Services : C:\Program Files\Microsoft SQL Server\MSSQL13.*>nom_instance>* \MSSQL\DATA.  
  
1.  Démarrez Microsoft SQL Server Management Studio et connectez-vous à l'instance de SQL Server appropriée.  
  
2.  Dans l'Explorateur d'objets, développez le nœud **Bases de données** .  
  
3.  Cliquez avec le bouton droit sur la base de données **DQS_MAIN** , pointez sur **Tâches**, puis sélectionnez **Détacher**. La boîte de dialogue **Détacher la base de données** apparaît.  
  
4.  Activez la case à cocher sous la colonne **Supprimer** , puis cliquez sur **OK** pour détacher la base de données DQS_MAIN.  
  
5.  Répétez les étapes 3 et 4 avec les bases de données DQS_PROJECTS et DQS_STAGING_DATA afin de les détacher.  
  
 Vous pouvez également détacher les bases de données DQS à l'aide d'instructions Transact-SQL avec la procédure stockée sp_detach_db. Pour plus d'informations sur le détachement de bases de données à l'aide d'instructions Transact-SQL, consultez [Using Transact-SQL](../relational-databases/databases/detach-a-database.md#TsqlProcedure) dans [Detach a Database](../relational-databases/databases/detach-a-database.md).  
  
##  <a name="Attach"></a> Attacher des bases de données DQS  
 Utilisez les instructions suivantes pour attacher une base de données DQS à la même instance SQL Server (depuis laquelle vous avez procédé au détachement) ou à une autre instance de SQL Server où [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] est installé.  
  
1.  Démarrez Microsoft SQL Server Management Studio et connectez-vous à l'instance de SQL Server appropriée.  
  
2.  Dans l'Explorateur d'objets, cliquez avec le bouton droit sur **Bases de données**, puis sélectionnez **Attacher**. La boîte de dialogue **Attacher des bases de données** apparaît.  
  
3.  Pour spécifier la base de données à attacher, cliquez sur **Ajouter**. La boîte de dialogue **Rechercher les fichiers de base de données** apparaît.  
  
4.  Sélectionnez l'unité de disque contenant la base de données et développez l'arborescence du répertoire pour rechercher et sélectionner le fichier .mdf de la base de données. Par exemple, pour la base de données DQS_MAIN :  
  
    ```  
    C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\DATA\DQS_MAIN.mdf  
    ```  
  
5.  Le volet (inférieur) des **détails de la base de données** affiche les noms des fichiers à attacher. Pour vérifier ou modifier le nom du chemin d'accès d'un fichier, cliquez sur le bouton **Parcourir** (…).  
  
6.  Cliquez sur **OK** pour attacher la base de données DQS_MAIN.  
  
7.  Répétez les étapes 2 à 6 pour les bases de données DQS_PROJECTS et DQS_STAGING_DATA afin de les attacher.  
  
8.  Vous devez également exécuter les instructions Transact-SQL à l'étape suivante après la restauration de la base de données DQS_MAIN ; sinon, un message d'erreur s'affiche lorsque vous tentez de vous connecter à Data Quality Server à l'aide de l'application Data Quality Client et que vous ne pouvez pas vous connecter. Toutefois, vous n'avez pas besoin de suivre les étapes 9 et 10 si vous venez d'attacher la base de données DQS_PROJECTS ou DQS_STAGING_DATA, et non DQS_MAIN.  
  
     Pour exécuter les instructions Transact-SQL, dans l'Explorateur d'objets, cliquez avec le bouton droit sur le serveur et sélectionnez **Nouvelle requête**.  
  
9. Dans la fenêtre Éditeur de requête, copiez les instructions SQL suivantes :  
  
    ```  
    ALTER DATABASE [DQS_MAIN] SET TRUSTWORTHY ON;  
    EXEC sp_configure 'clr enabled', 1;  
    RECONFIGURE WITH OVERRIDE  
    ALTER DATABASE [DQS_MAIN] SET ENABLE_BROKER  
    ALTER AUTHORIZATION ON DATABASE::[DQS_MAIN] TO [##MS_dqs_db_owner_login##]  
    ALTER AUTHORIZATION ON DATABASE::[DQS_PROJECTS] TO [##MS_dqs_db_owner_login##]  
  
    ```  
  
10. Appuyez sur F5 pour exécuter les instructions. Consultez le volet de résultats pour vérifier que les instructions ont été correctement exécutées. Vous recevrez le message suivant : `Configuration option 'clr enabled' changed from 1 to 1. Run the RECONFIGURE statement to install.`  
  
11. Connectez-vous à Data Quality Server à l'aide de l'application Data Quality Client afin de vérifier que vous pouvez vous connecter correctement.  
  
 Vous pouvez également attacher des bases de données DQS à l'aide d'instructions Transact-SQL. Pour plus d'informations sur l'attachement de bases de données à l'aide d'instructions Transact-SQL, consultez [Using Transact-SQL](../relational-databases/databases/attach-a-database.md#TsqlProcedure) dans [Attach a Database](../relational-databases/databases/attach-a-database.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Manage DQS Databases](../data-quality-services/manage-dqs-databases.md)  
  
  
