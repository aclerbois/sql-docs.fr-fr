---
title: Mettre à niveau des assemblys SQLCLR après une mise à jour de .NET Framework | Microsoft Docs
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
ms.assetid: b1a008cc-7e6b-4655-a869-bd429f986400
caps.latest.revision: 16
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ad7e492344fe15574f80a4312bc2eff02e777981
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="upgrade-sqlclr-assemblies-after-net-framework-update"></a>Mettre à niveau des assemblys SQLCLR après une mise à jour de .NET Framework

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) est un ensemble de routines Common Language Runtime SQL (SQLCR) qui font référence à des assemblys Microsoft .NET Framework 4. Lorsque vous installez sur votre ordinateur toutes les mises à jour.NET framework qui affectent un tel assembly. NET Framework référencé, cela entraîne une modification dans l'ID de version du module (MVID) de l'assembly dans Global Assembly Cache (GAC). Cela provoque une discordance entre les MVID de l'assembly référencé dans le GAC et de l'assembly dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Si la mise à jour .NET Framework nécessite de redémarrer le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , les assemblys SQLCLR affectés sont mis à niveau automatiquement pour résoudre le problème d'incompatibilité de MVID au redémarrage du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] . Toutefois, pour les mises à jour .NET Framework qui ne nécessitent pas de redémarrage de votre [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , une erreur se produit en raison d'une incohérence dans les MVID des assemblys lorsque vous essayez de vous connecter à un [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] à l'aide d'un [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]:  
  
```  
A new version of .NET was installed on this machine. In order to continue to work with DQS please run dqsinstaller.exe –upgradedlls.  
```  
  
 Pour résoudre ce problème, les assemblys SQLCLR affectés dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] doivent être mis à niveau. Pour ce faire, vous pouvez exécuter le fichier DQSInstaller.exe avec le paramètre de ligne de commande **upgradedlls** pour ignorer la recréation des bases de données DQS et mettre à niveau uniquement les assemblys concernés. Cela garantit que vos bases de connaissances, projets de qualité des données et toutes autres données dans DQS sont conservés.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
  
-   Vous devez être connecté en tant que membre du groupe Administrateurs sur l'ordinateur [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
-   Votre compte d'utilisateur Windows doit être membre du rôle serveur fixe sysadmin dans l'instance de SQL Server où le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] est installé.  
  
### <a name="to-upgrade-sqlclr-assemblies"></a>Pour mettre à niveau des assemblys SQLCLR  
  
1.  Démarrez l'invite de commandes.  
  
2.  À l'invite de commandes, remplacez votre répertoire à l'emplacement où DQSInstaller.exe est disponible. Si vous avez installé l’instance par défaut de SQL Server, le fichier DQSInstaller.exe est disponible dans C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn:  
  
    ```  
    cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn  
    ```  
  
3.  À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  
  
    ```  
    dqsinstaller.exe -upgradedlls  
    ```  
  
4.  Les étapes restantes sont les mêmes que les étapes 2 à 6 de la section [Exécuter DQSInstaller.exe à partir du menu Démarrer ou de l’Explorateur Windows](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md#WindowsExplorer) de l’article [Exécuter DQSInstaller.exe pour terminer l’installation du serveur DQS](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Installer Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Mettre à niveau le schéma des bases de données DQS après avoir installé la mise à jour SQL Server](../../data-quality-services/install-windows/upgrade-dqs-databases-schema-after-installing-sql-server-update.md)  
  
  
