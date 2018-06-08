---
title: Mettre à niveau Data Quality Services | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f396666b-7754-4efc-9507-0fd114cc32d5
caps.latest.revision: 12
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 7b48c28d2e144dd681a712b405912198f84e94c1
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34770715"
---
# <a name="upgrade-data-quality-services"></a>Mettre à niveau Data Quality Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fournit des informations sur la mise à niveau de votre installation existante de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Data Quality Services (DQS). Dans le cadre de la mise à niveau de Data Quality Server [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], vous devez également mettre à niveau le schéma des bases de données DQS.  
  
> [!IMPORTANT]  
>  -   Vous devez sauvegarder vos bases de données DQS avant de mettre à niveau DQS afin d'empêcher toute perte accidentelle de données lors de la mise à niveau du schéma. Pour plus d'informations sur la sauvegarde des bases de données DQS, consultez [Sauvegarde et restauration de bases de données DQS](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
> -   Connectez-vous à Data Quality Server [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en utilisant la version actuelle ou une version antérieure de Data Quality Client, ou la [transformation de nettoyage DQS](../../integration-services/data-flow/transformations/dqs-cleansing-transformation.md) dans Integration Services pour effectuer les tâches de qualité des données.  
> -   Une fois la mise à niveau de Data Quality Services et de Master Data Services effectuée, toutes les versions antérieures du complément Master Data Services pour Excel cesseront de fonctionner. Vous pouvez télécharger la version [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] du complément Master Data Services pour Excel [ici](http://go.microsoft.com/fwlink/?LinkID=506665).  
  
##  <a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté en tant que membre du groupe Administrateurs sur l'ordinateur [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
-   Votre compte d'utilisateur Windows doit être membre du rôle serveur fixe sysadmin dans l'instance de SQL Server où le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] est installé.  
  
##  <a name="Upgrade"></a> Mise à niveau de DQS  
 Pour mettre à niveau DQS :  
  
1.  Sauvegardez vos bases de données DQS avant de lancer le processus de mise à niveau. Pour plus d'informations sur la sauvegarde des bases de données DQS, consultez [Sauvegarde et restauration de bases de données DQS](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
  
2.  Mettez à niveau l’instance de SQL Server où DQS est installé.  
  
    1.  Exécutez l'Assistant Installation de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    2.  Dans le volet gauche, cliquez sur **Installation**.  
  
    3.  Dans le volet droit, cliquez sur **Mettre à niveau depuis...** une version précédente de SQL Server.  
  
    4.  Exécutez toutes les étapes de l'Assistant Installation.  
  
        > [!NOTE]  
        >  Votre instance de SQL Server sera mise à niveau vers [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] et vous installerez la dernière version de Data Quality Client, si Data Quality Client était déjà installé sur cet ordinateur. Si Data Quality Client est installé sur d'autres ordinateurs, vous devez exécuter les sous-étapes de l'Étape 2 sur ces ordinateurs pour installer la version actuelle de Data Quality Client. L'Assistant Installation installe la version actuelle de Data Quality Client à côté de la version existante. Après la mise à niveau du schéma de bases de données DQS, connectez-vous à la version [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de Data Quality Server en utilisant la version actuelle ou une version antérieure de Data Quality Client.  
  
3.  Mettez à niveau le schéma de bases de données DQS.  
  
    1.  Démarrez une invite de commandes d'administrateur.  
  
    2.  À l'invite de commandes, remplacez votre répertoire à l'emplacement où DQSInstaller.exe est disponible. Pour l’instance par défaut de SQL Server, le fichier DQSInstaller.exe est disponible dans C:\Program Files\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn :  

      >[!NOTE]
      >Dans le chemin du dossier, remplacez [nn] par le numéro de version de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].
      >- Pour SQL Server 2016 : 13
      >- Pour SQL Server 2017 : 14

        ```  
        cd C:\Program Files\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn  
        ```  
  
    3.  À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  
  
        ```  
        dqsinstaller.exe -upgrade  
        ```  
  
    4.  Le programme d'installation vous invite à sauvegarder les bases de données DQS avant de continuer. Si vous avez déjà sauvegardé vos bases de données DQS, tapez **Y** ou **Yes**et appuyez sur Entrée pour continuer avec la mise à niveau.  
  
    5.  Un message d'achèvement s'affiche une fois la mise à niveau du schéma des bases de données DQS terminée.  
  
##  <a name="Verify"></a> Vérification de la mise à niveau du schéma des bases de données DQS  
 Pour vérifier que le schéma de bases de données DQS a été correctement mis à niveau, vérifiez la version actuelle dans les bases de données DQS_MAIN et DQS_PROJECTS en interrogeant la table A_DB_VERSION dans chaque base de données. Pour cela :  
  
1.  Démarrez SQL Server Management Studio et connectez-vous à l'instance de SQL Server qui contient les bases de données DQS mises à niveau.  
  
2.  Exécutez la requête suivante :  
  
    ```  
    SELECT * FROM DQS_MAIN.dbo.A_DB_VERSION WHERE STATUS=2;  
    SELECT * FROM DQS_PROJECTS.dbo.A_DB_VERSION WHERE STATUS=2;  
    ```  
  
3.  La sortie indiquera une entrée pour chaque mise à niveau, avec la date correspondante. Les valeurs VERSION_ID et ASSEMBLY_VERSION maximales de la dernière date correspondent à la version actuelle. Une valeur de 2 dans la colonne STATUS indique que l'opération a réussi. Si une erreur s'est produite, elle sera indiquée dans la colonne ERROR. Exemple de sortie :  
  
    |ID|UPGRADE_DATE|VERSION_ID|ASSEMBLY_VERSION|USER_NAME|STATUS|d’erreur|  
    |--------|-------------------|-----------------|-----------------------|----------------|------------|-----------|  
    |1000|2013-08-11 05:26:39.567|1200|11.0.3000.0|\<DOMAINE\nom d’utilisateur>|2||  
    |1001|2013-09-19 15:09:37.750|1600|12.0.xxxx.0|\<DOMAINE\nom d’utilisateur>|2||  
  
## <a name="see-also"></a> Voir aussi  
 [Installer Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Supprimer des objets Data Quality Server](../../sql-server/install/remove-data-quality-server-objects.md)   
 [Mise à niveau vers SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
  
  
