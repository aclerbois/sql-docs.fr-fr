---
title: Ajouter de nouveaux éléments à un projet | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-solutions
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- projects [SQL Server Management Studio], item additions
- adding project items
ms.assetid: 76af8692-324f-4f5e-b1a0-d72ca8a107e3
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 01a3d385d52f90eb48cf9c4ca6f9b19c9639e596
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-new-items-to-a-project"></a>Ajouter de nouveaux éléments à un projet
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Vous pouvez ajouter des éléments à un projet pour étendre la fonctionnalité de l'application. Un nouvel élément peut être une requête ou une connexion. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] comporte deux types de projets : les projets de script [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et les projets de script Analysis Services. Les éléments que vous pouvez ajouter au projet sont déterminés par le type du projet. Par exemple, vous pouvez ajouter une requête [!INCLUDE[tsql](../../includes/tsql_md.md)] (fichier doté de l'extension .sql) à un projet de script [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , mais vous ne pouvez pas l'ajouter à un projet de script de services d'analyse.  
  
[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] ne vous autorise pas à créer des dossiers dans les projets. Pour organiser votre travail, créez plusieurs projets dans la solution.  
  
### <a name="to-add-a-new-query-to-an-existing-project"></a>Pour ajouter une nouvelle requête à un projet existant  
  
1.  Dans l'Explorateur de solutions, sélectionnez un projet cible.  
  
2.  Dans le menu **Projet** , cliquez sur **Ajouter un nouvel élément**.  
  
3.  Dans cette boîte de dialogue **Ajouter un nouvel élément** , sélectionnez une catégorie dans le volet gauche.  
  
4.  Sélectionnez un modèle de requête dans le volet droit, puis cliquez sur **Ajouter**. La nouvelle requête est ajoutée au dossier **Requêtes** du projet.  
  
5.  Dans la boîte de dialogue **Se connecter au moteur de base de données** , spécifiez une connexion pour la nouvelle requête, puis cliquez sur **Se connecter**. Vous pouvez cliquer sur **Annuler** dans la boîte de dialogue de connexion si vous ne souhaitez pas associer de connexion à la nouvelle requête.  
  
6.  Si vous le souhaitez, renommez la requête dans l'Explorateur de solutions.  
  
### <a name="to-add-a-new-connection-to-an-existing-project"></a>Pour ajouter une nouvelle connexion à un projet existant  
  
1.  Dans l'Explorateur de solutions, sélectionnez un projet cible.  
  
2.  Dans le menu **Projet** , cliquez sur **Ajouter un nouvel élément**.  
  
3.  Sélectionnez **Connexion** dans le volet gauche.  
  
4.  Sélectionnez **Nouvelle connexion** dans le volet droit, puis cliquez sur **Ajouter**.  
  
5.  Dans la boîte de dialogue **Se connecter au moteur de base de données** , spécifiez une connexion pour la nouvelle requête, puis cliquez sur **Se connecter**. La nouvelle connexion est ajoutée au dossier **Connexions** du projet.  
  
## <a name="see-also"></a> Voir aussi  
[Explorateur de solutions](../../ssms/solution/solution-explorer.md)  
[Association d’extensions de fichier à un éditeur de code](http://msdn.microsoft.com/en-us/193630f4-93de-4950-8f36-68702531f925)  
[Ajouter des éléments existants à un projet](../../ssms/solution/add-existing-items-to-a-project.md)  
[Enlever ou supprimer un élément ou un projet](../../ssms/solution/remove-or-delete-an-item-or-project.md)  
  
