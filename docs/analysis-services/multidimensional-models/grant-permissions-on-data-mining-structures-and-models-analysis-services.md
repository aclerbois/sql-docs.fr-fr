---
title: Accorder des autorisations sur les structures d’exploration de données et des modèles (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 91e805488ff5a90b4f358cb908fe2efa6fd8d04a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="grant-permissions-on-data-mining-structures-and-models-analysis-services"></a>Octroyer des autorisations sur des modèles et des structures d'exploration de données (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Par défaut, seul un administrateur de serveur Analysis Services est autorisé à afficher des structures ou des modèles d'exploration de données dans la base de données. Suivez les instructions ci-dessous pour accorder des autorisations à des utilisateurs non-administrateurs.  
  
## <a name="set-permissions-to-access-a-mining-structure"></a>Définir des autorisations d'accès à une structure d'exploration de données  
  
1.  Dans SSMS, connectez-vous à Analysis Services. Si vous avez besoin d’aide pour ces étapes, consultez [Connexion à partir d’applications clientes &#40;Analysis Services&#41;](../../analysis-services/instances/connect-from-client-applications-analysis-services.md).  
  
2.  Ouvrez le dossier **Bases de données** et sélectionnez une base de données dans l'Explorateur d'objets.  
  
3.  Cliquez avec le bouton droit sur le dossier **Rôles** et choisissez **Nouveau rôle**.  
  
4.  Dans la page Général, entrez un nom et éventuellement une description. La page contient également plusieurs autorisations de base de données, telles que Contrôle total, Traiter la base de données et Lire la définition. Aucune de ces autorisations n'est nécessaire pour l'accès à l'exploration des données. Pour plus d’informations sur les autorisations de base de données, consultez [Octroyer des autorisations de base de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md).  
  
5.  Dans le volet **Structure d’exploration de données**, sélectionnez **Lecture** ou **Lecture/Écriture** pour chaque structure d’exploration de données.  
  
6.  Dans le volet **Appartenance** , entrez les comptes d'utilisateurs et de groupes Windows qui se connectent à Analysis Services à l'aide de ce rôle.  
  
7.  Cliquez sur **OK** pour terminer la création du rôle.  
  
## <a name="set-permissions-to-access-a-mining-model"></a>Définir des autorisations d'accès à un modèle d'exploration de données  
 Pour un modèle d’exploration de données, un rôle peut avoir des autorisations **Lecture** ou **Lecture/Écriture** , ainsi que des autorisations **Extraction** et **Lire la définition** qui autorisent l’affichage et le parcours des données sous-jacentes.  
  
 **Remarque** Si vous activez l'extraction à la fois sur la structure d'exploration de données et le modèle d'exploration de données, tout utilisateur membre d'un rôle qui a des autorisations d'extraction sur le modèle d'exploration de données et la structure d'exploration de données peut également afficher les colonnes de la structure d'exploration de données, même si ces colonnes ne sont pas incluses dans le modèle d'exploration de données. Par conséquent, vous devez installer la vue de source de données pour masquer des informations personnelles afin de protéger les informations sensibles, et autoriser uniquement l'accès d'extraction sur la structure d'exploration de données lorsque nécessaire.  
  
 Pour accorder des autorisations de lecture ou de lecture/écriture à un rôle de base de données, un utilisateur doit être membre du rôle de serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou d'un rôle de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ayant les autorisations Contrôle total (Administrateur).  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance d’[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], développez **Rôles** pour la base de données appropriée dans l’Explorateur d’objets, puis cliquez sur un rôle de base de données (ou créez un rôle de base de données).  
  
2.  Dans le volet **Structure d’exploration de données** , recherchez le modèle d’exploration de données dans la liste **Modèles d’exploration de données** , puis sélectionnez **Lecture**, **Lecture/Écriture**, **Extraire**ou **Parcourir** .  
  
3.  Dans le volet **Appartenance** , entrez les comptes d'utilisateurs et de groupes Windows qui se connectent à Analysis Services à l'aide de ce rôle.  
  
4.  Cliquez sur **OK** pour terminer la création du rôle.  
  
 Pour utiliser une source de données dans une requête d’extraction qui utilise la clause DMX (Data Mining Extensions) OPENQUERY, le rôle de base de données doit également disposer d’une autorisation de lecture/écriture sur l’objet de source de données approprié. Pour plus d’informations, consultez [Octroyer des autorisations sur un objet de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-permissions-on-a-data-source-object-analysis-services.md) et [OPENQUERY &#40;DMX&#41;](../../dmx/source-data-query-openquery.md).  
  
> [!NOTE]  
>  Par défaut, la soumission de requêtes DMX à l’aide d’OPENROWSET est désactivée.  
  
## <a name="see-also"></a>Voir aussi  
 [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md)   
 [Grant – octroi cube ou autorisations de modèle & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md)   
 [Octroyer un accès personnalisé à la dimension de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-custom-access-to-dimension-data-analysis-services.md)   
 [Octroyer un accès personnalisé à la cellule de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-custom-access-to-cell-data-analysis-services.md)  
  
  
