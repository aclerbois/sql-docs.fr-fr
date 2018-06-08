---
title: Autoriser l’accès aux objets et des opérations (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 88290b9598ffdbbcfc90a738654a9485107da464
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="authorizing-access-to-objects-and-operations-analysis-services"></a>Autorisation de l'accès à des objets et des opérations (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'accès utilisateur non-administratif aux cubes, aux dimensions et aux modèles d'exploration de données au sein d'une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est octroyé via l'appartenance à un ou à plusieurs rôles de base de données. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] les administrateurs créent ces rôles de base de données, octroient des autorisations en lecture ou en lecture/écriture sur des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis ajoutent des groupes et des utilisateurs [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows pour chaque rôle.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] détermine les autorisations effectives d'un utilisateur ou d'un groupe Windows en combinant les autorisations associées à chaque rôle de base de données auquel l'utilisateur ou le groupe appartient. Par conséquent, si un rôle de base de données n'autorise pas un utilisateur ou un groupe à afficher une dimension, une mesure ou un attribut et qu'un autre rôle de base de données autorise l'utilisateur ou le groupe, l'utilisateur ou le groupe peut afficher l'objet.  
  
> [!IMPORTANT]  
>  Les membres du rôle Administrateur de serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et les membres d'un rôle de base de données ayant des autorisations Contrôle total (Administrateur) peuvent accéder à toutes les données et métadonnées de la base de données et n'ont pas besoin d'autorisations supplémentaires pour afficher des objets spécifiques. De plus, les membres du rôle de serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne peuvent pas être empêchés d’accéder aux objets d’une base de données, et les membres d’un rôle de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] avec des autorisations Contrôle total (Administrateurs) dans une base de données ne peuvent pas être empêchés d’accéder aux objets de cette base de données. Les opérations d'administration spécifiques, telles que le traitement, peuvent être autorisées via des rôles séparés avec des autorisations moins élevées. Pour plus d’informations, consultez [Octroyer des autorisations de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-process-permissions-analysis-services.md).  
  
## <a name="list-roles-defined-for-your-database"></a>Lister les rôles définis pour votre base de données  
 Les administrateurs peuvent exécuter une simple requête DMV dans SQL Server Management Studio pour obtenir une liste de tous les rôles définis sur le serveur.  
  
1.  Dans SSMS, cliquez avec le bouton droit sur une base de données, puis sélectionnez **Nouvelle requête** | **MDX**.  
  
2.  Tapez la requête suivante et appuyez sur F5 pour l'exécuter :  
  
    ```  
    Select * from $SYSTEM.DBSCHEMA_CATALOGS  
    ```  
  
     Les résultats incluent le nom de la base de données, la description, le nom du rôle et la date de dernière modification. En vous servant de ces informations comme point de départ, vous pouvez vérifier les appartenances et les autorisations d'un rôle spécifique pour des bases de données individuelles.  
  
## <a name="top-down-overview-of-analysis-services-authorization"></a>Vue d'ensemble verticale de l'autorisation Analysis Services  
 Cette section traite du flux de travail de base pour la configuration des autorisations.  
  
 **Étape 1 : Administration du serveur**  
  
 En guise de première étape, identifiez qui disposera de droits d'administrateur au niveau du serveur. Pendant l'installation, l'administrateur local qui installe SQL Server doit spécifier un ou plusieurs comptes Windows en tant qu'administrateur serveur Analysis Services. Les administrateurs de serveur disposent de toutes les autorisations possibles sur un serveur, notamment l'autorisation pour afficher, modifier et supprimer les objets sur le serveur ou afficher les données associées. Une fois l'installation terminée, un administrateur de serveur peut ajouter ou supprimer des comptes pour changer l'appartenance de ce rôle. Pour plus d’informations sur ce niveau d’autorisation, consultez [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md) .  
  
 **Étape 2 : Administration de bases de données**  
  
 Ensuite, une fois qu'une solution tabulaire ou multidimensionnelle a été créée, elle est déployée sur le serveur en tant que base de données. Un administrateur de serveur peut déléguer les tâches d'administration de base de données en définissant un rôle qui dispose des autorisations Contrôle total pour la base de données en question. Les membres de ce rôle peuvent traiter ou interroger les objets dans la base de données, et créer des rôles supplémentaires pour accéder aux cubes, dimensions et autres objets au sein de la base de données elle-même. Pour plus d’informations, consultez [Octroyer des autorisations de base de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md).  
  
 **Étape 3 : Activer l’accès à un cube ou à un modèle pour les charges de travail de requête et de traitement**  
  
 Par défaut, seuls les administrateurs du serveur et de la base de données ont accès aux cubes ou aux modèles tabulaires. L'accès à ces structures de données par d'autres personnes de votre organisation nécessite des attributions de rôle supplémentaires qui correspondent aux comptes d'utilisateur et de groupe Windows aux cubes ou aux modèles, ainsi que des autorisations qui accordent des privilèges **Read** . Pour plus d’informations, consultez [Octroyer des autorisations de cube ou de modèle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md).  
  
 Les tâches de traitement peuvent être isolées d'autres fonctions d'administration, ce qui permet aux administrateurs de serveur et de base de données de déléguer cette tâche à d'autres personnes ou de configurer un traitement sans assistance en indiquant des comptes de service qui exécutent des logiciels de planification. Pour plus d’informations, consultez [Octroyer des autorisations de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-process-permissions-analysis-services.md).  
  
> [!NOTE]  
>  Les utilisateurs ne nécessitent pas d’autorisations pour accéder aux tables relationnelles de la base de données relationnelle sous-jacente à partir de laquelle [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] charge ses données, et ils ne nécessitent pas non plus d’autorisations sur les fichiers de l’ordinateur sur lequel l’instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est exécutée.  
  
 **Étape 4 (facultative) : Accorder ou refuser l’accès à des objets de cube intérieurs**  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit des paramètres de sécurité pour définir des autorisations sur des objets spécifiques, notamment des membres de dimension et des cellules dans un modèle de données. Pour plus d’informations, consultez [Octroyer un accès personnalisé à des données de dimension &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-custom-access-to-dimension-data-analysis-services.md) et [Octroyer un accès personnalisé à des données de cellule &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-custom-access-to-cell-data-analysis-services.md).  
  
 Vous pouvez également faire varier les autorisations en fonction de l'identité d'un utilisateur. Ceci est souvent appelé sécurité dynamique ; vous utilisez la fonction [UserName &#40;MDX&#41;](../../mdx/username-mdx.md) pour l’implémenter.  
  
## <a name="best-practices"></a>Meilleures pratiques  
 Pour mieux gérer les autorisations, nous vous suggérons l'approche suivante :  
  
1.  Créer des rôles par fonction (par exemple, dbadmin, cubedeveloper, processadmin) afin que la personne qui est responsable de ces rôles puisse savoir d'un coup d'œil ce que ces rôles autorisent. Comme déjà indiqué, vous pouvez définir des rôles dans la définition du modèle, ce qui permet de conserver ces rôles pour d'autres déploiements de solution.  
  
2.  Créer un groupe de sécurité Windows correspondant dans Active Directory, puis tenir à jour le groupe de sécurité dans Active Directory afin qu'il contienne les comptes individuels appropriés. Les spécialistes de la sécurité sont ainsi responsables de l'appartenance au groupe de sécurité ce qui est un avantage dans la mesure où ils disposent déjà des outils et des connaissances nécessaires à la maintenance des comptes dans votre organisation.  
  
3.  Générer des scripts dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] afin de répliquer rapidement des assignations de rôle quand le modèle est redéployé à partir des fichiers sources sur un serveur. Pour plus d’informations sur la façon de générer rapidement un script, consultez [Octroyer des autorisations de cube ou de modèle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md).  
  
4.  Adopter une convention d'attribution de noms qui reflète la portée et l'appartenance du rôle. Les noms de rôle sont visibles uniquement dans les outils de conception et d'administration, il est donc recommandé d'utiliser une convention d'attribution de nom qui soit parlante pour vos spécialistes de la sécurité du cube. Par exemple, **processadmin-windowsgroup1** indique un accès en lecture, plus des droits de traitement, pour les personnes de votre organisation dont les comptes d’utilisateur Windows individuels sont membres du groupe de sécurité **windowsgroup1** .  
  
     L'ajout des informations de compte peut vous aider à savoir quels comptes sont utilisés dans les différents rôles. Dans la mesure où les rôles sont additifs, les rôles combinés associés à **windowsgroup1** composent le jeu d'autorisations effectif pour les personnes qui appartiennent à ce groupe de sécurité.  
  
5.  Les développeurs de cube ont besoin des autorisations Contrôle total pour les modèles et les bases de données en développement, mais n'auront besoin que des autorisations Lecture une fois une base de données déployée sur un serveur de produit. N'oubliez pas de développer des définitions de rôle et des assignations pour tous les scénarios, notamment le développement, le test et les déploiements en production.  
  
 Cela permet de minimiser l'actualisation des définitions de rôle et de l'appartenance de rôle dans le modèle et fournit une meilleure visibilité sur les assignations de rôle ce qui facilite l'implémentation et la mise à jour des autorisations du cube.  
  
## <a name="see-also"></a>Voir aussi  
 [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md)   
 [Rôles et autorisations &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/roles-and-permissions-analysis-services.md)   
 [Méthodologies d’authentification pris en charge par Analysis Services](../../analysis-services/instances/authentication-methodologies-supported-by-analysis-services.md)  
  
  
