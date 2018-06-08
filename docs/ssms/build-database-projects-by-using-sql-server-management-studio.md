---
title: Créer des projets de base de données à l’aide de SQL Server Management Studio | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], database projects
- database projects [SQL Server]
- projects [SQL Server Management Studio], about projects
- projects [SQL Server Management Studio]
ms.assetid: c2e80045-894d-44cf-b65c-e547ed738947
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e3dda4b94a26b0a2e88d6ad9be75abfa6721cd77
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="build-database-projects-by-using-sql-server-management-studio"></a>Créer des projets de base de données à l'aide de SQL Server Management Studio
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Un projet de script de base de données est un ensemble organisé de scripts, d'informations de connexion et de modèles qui sont tous associés à une base de données ou une partie d'une base de données. [!INCLUDE[msCoName](../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion_md.md)] fournit [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] pour l’administration et la conception de bases de données [!INCLUDE[ssNoVersion](../includes/ssnoversion_md.md)] dans le contexte d’un projet de script. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] inclut des concepteurs, des éditeurs, des guides et des assistants pour aider les utilisateurs lors du développement, du déploiement et de la gestion de bases de données.  
  
## <a name="sql-server-management-studio"></a>SQL Server Management Studio  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] est une suite d’outils d’administration pour la gestion des composants appartenant à [!INCLUDE[ssNoVersion](../includes/ssnoversion_md.md)]. Cet environnement intégré permet aux utilisateurs d'effectuer de nombreuses tâches, telles que la sauvegarde de données, l'édition de requêtes et l'automatisation de fonctions courantes, au sein d'une même interface.  
  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] propose les outils suivants :  
  
-   L'Éditeur de code, qui est un éditeur de scripts très fourni pour l'écriture et l'édition de scripts. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] fournit quatre versions de l’Éditeur de code : l’Éditeur de requête de [!INCLUDE[ssDE](../includes/ssde_md.md)] pour les scripts [!INCLUDE[tsql](../includes/tsql_md.md)] , l’Éditeur de requête DMX, l’Éditeur de requête MDX et l’Éditeur de requête XML/A.  
  
-   L'Explorateur d'objets pour la localisation, la modification, l'écriture de script ou l'exécution d'objets appartenant aux instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion_md.md)].  
  
-   L'Explorateur de modèles pour la localisation et l'écriture de scripts de modèles.  
  
-   L'Explorateur de solutions pour l'organisation et le stockage de scripts associés en tant que parties d'un projet.  
  
-   La fenêtre Propriétés pour l'affichage des propriétés actuelles des objets sélectionnés.  
  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] prend en charge des processus de travail efficaces en fournissant :  
  
-   L'accès déconnecté. Vous pouvez écrire et éditer des scripts sans vous connecter à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion_md.md)].  
  
-   L'écriture de script à partir de n'importe quelle boîte de dialogue. Vous pouvez créer un script à partir de n'importe quelle boîte de dialogue afin de pouvoir lire, modifier, stocker et réutiliser les scripts après les avoir créés.  
  
-   Boîtes de dialogue non modales. Lorsque vous accédez à une boîte de dialogue de l'interface utilisateur, vous pouvez parcourir d'autres ressources dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] sans fermer la boîte de dialogue.  
  
## <a name="solutions-and-script-projects"></a>Solutions et projets de script  
L'Explorateur de solutions est un utilitaire qui stocke et rouvre les solutions de bases de données. Les solutions organisent les fichiers et projets de script associés. Les projets de script stockent les fichiers de script [!INCLUDE[ssNoVersion](../includes/ssnoversion_md.md)] , les modèles SQL, les informations de connexion et d'autres fichiers divers. Lorsqu'un script est enregistré dans un projet de script, les utilisateurs peuvent :  
  
-   Maintenir le contrôle des versions sur les scripts.  
  
-   Stocker les options de résultats avec un script.  
  
-   Organiser les scripts associés dans un projet de script unique.  
  
-   Enregistrer les informations de connexion avec des scripts.  
  
L'Explorateur de solutions est un outil destiné aux développeurs qui créent et réutilisent des scripts associés à un même projet. Si une tâche similaire est requise ultérieurement, vous pouvez réutiliser un groupe de scripts stockés dans un projet. Si vous avez déjà créé des applications avec [!INCLUDE[msCoName](../includes/msconame_md.md)] [!INCLUDE[vsprvs](../includes/vsprvs_md.md)], l’Explorateur de solutions vous semblera très familier.  
  
Une solution est constituée d'un ou plusieurs projets de script. Un projet est constitué d'un ou plusieurs scripts ou connexions. Un projet peut également comprendre des fichiers qui ne sont pas des scripts.  
  
## <a name="see-also"></a> Voir aussi  
[Utiliser SQL Server Management Studio](../ssms/use-sql-server-management-studio.md)  
[Écriture, analyse et modification de requêtes à l’aide de SQL Server Management Studio](http://msdn.microsoft.com/en-us/062051e4-4b77-4969-98ae-d2547c24ce3e)  
[Solutions &#40;SQL Server Management Studio&#41;](../ssms/solution/solutions-sql-server-management-studio.md)  
  
