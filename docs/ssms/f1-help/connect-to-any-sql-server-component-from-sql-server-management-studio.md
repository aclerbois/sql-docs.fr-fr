---
title: Se connecter à n’importe quel composant de SQL Server à partir de SSMS | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-f1
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], SQL Server Management Studio
- saving connections
- components [SQL Server], connections
- SQL Server Management Studio [SQL Server], connections
ms.assetid: 5eeb41bd-b25b-4d3b-a005-a7d9e4b5978e
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 58bd639a16003fe718a226b1aea7a3bb3bf5b470
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-any-sql-server-component-from-sql-server-management-studio"></a>Se connecter à n'importe quel composant de SQL Server à partir de SQL Server Management Studio
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] fournit des fonctionnalités pour la gestion de tous les composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Utilisez [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] pour vous connecter à :  
  
-   Une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)].  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)].  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion_md.md)].  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion_md.md)].  
  
Bien que [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] vous permette d'utiliser des requêtes sans que vous deviez établir une connexion à une source de données, la plupart des autres tâches requièrent une connexion. [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] contient la boîte de dialogue **Se connecter au serveur** qui vous permet de configurer les propriétés de connexion aux composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] . Quand [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] démarre, il ouvre la boîte de dialogue **Se connecter au serveur** et vous invite à vous connecter à un serveur. Cette boîte de dialogue conserve les paramètres de connexion depuis leur dernière utilisation.  
  
> [!NOTE]  
> Cette fonctionnalité peut être désactivée pour qu'aucune connexion ne soit automatiquement établie. Pour plus d’informations, consultez [Options de démarrage du service moteur de base de données](http://msdn.microsoft.com/en-us/d373298b-f6cf-458a-849d-7083ecb54ef5).  
  
## <a name="saving-connections"></a>enregistrement des connexions  
Vous pouvez enregistrer des connexions à des serveurs spécifiques dans les serveurs inscrits ou des connexions dans des projets à l'aide de l'Explorateur de solutions.  
  
### <a name="saving-connections-in-registered-servers"></a>Enregistrement des connexions dans les serveurs inscrits  
Lorsque vous inscrivez un serveur, [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] enregistre les informations de connexion dans les serveurs inscrits. Pour vous connecter à un serveur inscrit, double-cliquez sur le nom du serveur dans les serveurs inscrits. L'Explorateur d'objets ouvre alors une connexion au serveur.  
  
### <a name="saving-connections-in-solution-explorer"></a>Enregistrement des connexions dans l'Explorateur de solutions  
L'Explorateur de solutions vous permet de stocker les requêtes, les scripts, les connexions et d'autres informations associées dans un projet. Chaque projet de script contient un nœud appelé **Connexions**où vous pouvez enregistrer une ou plusieurs connexions. Pour ajouter une connexion, cliquez avec le bouton droit sur **Connexions**, puis cliquez sur **Nouvelle connexion**. Pour accéder à une connexion enregistrée, développez **Connexions** et double-cliquez sur la connexion. [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] ouvre une fenêtre de requête associée à cette connexion. Une fois enregistrés, les scripts conservent leur association à une connexion spécifique.  
  
## <a name="see-also"></a> Voir aussi  
[Utiliser SQL Server Management Studio](../../ssms/use-sql-server-management-studio.md)  
[Explorateur d'objets](../../ssms/object/object-explorer.md)  
  
