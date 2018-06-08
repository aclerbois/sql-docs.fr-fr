---
title: Mettre à niveau le schéma des bases de données DQS après avoir installé la mise à jour SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: install-windows
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: c8f3fbae-02c4-464d-a35c-7108f48c58cb
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: daa91c745ff406abecf35ab91139f8e7fe763e4a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="upgrade-dqs-databases-schema-after-installing-sql-server-update"></a>Mettre à niveau le schéma des bases de données DQS après avoir installé la mise à jour SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Après avoir installé une mise à jour SQL Server (correctif, correctif logiciel ou mise à jour cumulative) sur une instance DQS configurée précédemment, vous pouvez être amené à mettre à niveau le schéma des bases de données DQS en exécutant le fichier DQSInstaller.exe avec le paramètre de ligne de commande **upgrade** . Sinon, l'erreur suivante peut s'afficher lors de la tentative de connexion au serveur de qualité des données à l'aide de votre client de qualité des données :  
  
```  
An error occurred in the Microsoft .NET Framework while trying to load assembly id 65581.  
```  
  
 La mise à niveau du schéma des bases de données DQS n'a aucun effet sur les données existantes dans les bases de données DQS (bases de connaissances, projets de qualité des données et résultats exportés dans la base de données DQS_STAGING_DATA). Cependant, vous devez sauvegarder vos bases de données DQS avant de mettre à niveau le schéma des bases de données DQS afin d'empêcher toute perte accidentelle de données lors de la mise à niveau du schéma. Pour plus d'informations sur la sauvegarde des bases de données DQS, consultez [Sauvegarde et restauration de bases de données DQS](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
  
> [!NOTE]  
>  La majorité des mises à jour SQL Server nécessite une mise à niveau du schéma des bases de données DQS. Pour plus d’informations sur les mises à jour SQL Server qui nécessitent une mise à niveau du schéma des bases de données DQS, consultez le graphique à l’étape 1.A dans [Upgrade DQS: Installing Cumulative Updates or Hotfix Patches on Data Quality Services](http://go.microsoft.com/fwlink/?LinkID=251565)(Mise à niveau de DQS : installation de mises à jour cumulatives ou de correctifs logiciels sur Data Quality Services.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
  
-   Vous devez être connecté en tant que membre du groupe Administrateurs sur l'ordinateur [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
-   Votre compte d'utilisateur Windows doit être membre du rôle serveur fixe sysadmin dans l'instance de SQL Server où le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] est installé.  
  
### <a name="to-upgrade-dqs-databases-schema"></a>Pour mettre à niveau le schéma des bases de données DQS  
  
1.  (Recommandé) Sauvegardez vos bases de données DQS avant de mettre à niveau le schéma. Pour plus d'informations sur la sauvegarde des bases de données DQS, consultez [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
  
2.  Démarrez l'invite de commandes.  
  
3.  À l'invite de commandes, remplacez votre répertoire à l'emplacement où DQSInstaller.exe est disponible. Si vous avez installé l’instance par défaut de SQL Server, le fichier DQSInstaller.exe est disponible dans C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn:  
  
    ```  
    cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn  
    ```  
  
4.  À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  
  
    ```  
    dqsinstaller.exe -upgrade  
    ```  
  
5.  Le programme d'installation vous invite à sauvegarder les bases de données DQS avant de continuer. Si vous avez déjà sauvegardé vos bases de données DQS, tapez **Y** ou **Yes** et appuyez sur Entrée pour poursuivre la mise à niveau.  
  
6.  Un message d'achèvement s'affiche une fois la mise à niveau du schéma des bases de données DQS terminée.  
  
## <a name="next-steps"></a>Next Steps  
 Connectez-vous au serveur de qualité des données à partir d'une application client de qualité des données.  
  
 Pour plus d’informations sur la mise à niveau du schéma des bases de données DQS après l’installation des mises à jour SQL Server et les procédures de dépannage associées, consultez [Upgrade DQS: Installing Cumulative Updates or Hotfix Patches on Data Quality Services](http://go.microsoft.com/fwlink/?LinkID=251565)(Mise à niveau de DQS : installation de mises à jour cumulatives ou de correctifs logiciels sur Data Quality Services).  
  
## <a name="see-also"></a> Voir aussi  
 [Installer Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Mettre à niveau des assemblys SQLCLR après une mise à jour de .NET Framework](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md)  
  
  
