---
title: Accorder des autorisations de cube ou du modèle (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f028885e91dd67869e15bab4d7b64cc97b64f9bb
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="grant-cube-or-model-permissions-analysis-services"></a>Octroyer des autorisations de cube ou de modèle (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un cube ou modèle tabulaire est le principal objet de requête dans un modèle de données Analysis Services. Lors de la connexion à des données tabulaires ou multidimensionnelles à partir d'Excel pour l'exploration de données ad hoc, les utilisateurs commencent en général par sélectionner un cube ou modèle tabulaire spécifique comme structure de données derrière l'objet de rapport de tableau croisé dynamique. Cette rubrique explique comment accorder les autorisations nécessaires pour l'accès aux données tabulaires ou de cube.  
  
 Par défaut, seul un Administrateur de serveur ou Administrateur de base de données est autorisé à interroger des cubes dans une base de données. L'accès à un cube par un non-administrateur requiert l'appartenance à un rôle créé pour la base de données contenant le cube. L'appartenance est prise en charge pour les comptes d'utilisateurs ou de groupes Windows définis dans Active Directory ou sur l'ordinateur local. Avant de commencer, identifiez les comptes qui appartiendront aux rôles que vous allez créer.  
  
 Le fait d’avoir un accès **Lecture** à un cube permet d’avoir aussi des autorisations d’accès aux dimensions, groupes de mesures et perspectives de ce cube. La plupart des administrateurs accordent des autorisations au niveau du cube, puis limitent les autorisations sur des objets spécifiques, des données associées ou selon l'identité de l'utilisateur.  
  
 Pour conserver les définitions des rôles d'un déploiement de solution au suivant, une meilleure pratique consiste à définir des rôles dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] comme partie intégrante du modèle, puis à faire en sorte qu'un administrateur de base de données assigne des appartenances aux rôles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] une fois la base de données publiée. Vous pouvez cependant utiliser l'un ou l'autre outil pour ces deux tâches. Pour simplifier l'exercice, nous allons utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour la définition et l'appartenance aux rôles.  
  
> [!NOTE]  
>  Seuls les administrateurs de serveur, ou les administrateurs de base de données ayant des autorisations Contrôle total, peuvent déployer un cube à partir de fichiers sources vers un serveur ou créer des rôles et assigner des membres. Pour obtenir des informations sur ces niveaux d’autorisation, consultez [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md) et [Octroyer des autorisations de base de données&#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md).  
  
#### <a name="step-1-create-the-role"></a>Étape 1 : Créer le rôle  
  
1.  Dans SSMS, connectez-vous à Analysis Services. Si vous avez besoin d’aide sur cette étape, consultez [Connexion à partir d’applications clientes &#40;Analysis Services&#41;](../../analysis-services/instances/connect-from-client-applications-analysis-services.md).  
  
2.  Ouvrez le dossier **Bases de données** dans l’Explorateur d’objets et sélectionnez une base de données.  
  
3.  Cliquez avec le bouton droit sur **Rôles** et choisissez **Nouveau rôle**. Notez que les rôles sont créés au niveau de la base de données et s'appliquent aux objets de cette base de données. Vous ne pouvez pas partager de rôles entre des bases de données.  
  
4.  Dans le volet **Général** , entrez un nom et éventuellement une description. Ce volet contient également plusieurs autorisations de base de données, telles que Contrôle total, Traiter la base de données et Lire la définition. Aucune de ces autorisations n'est nécessaire pour interroger un cube ou modèle tabulaire. Pour plus d'informations sur ces autorisations, consultez [Octroyer des autorisations de base de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md).  
  
5.  Continuez à l'étape suivante après avoir entré un nom et une description facultative.  
  
#### <a name="step-2-assign-membership"></a>Étape 2 : Assigner l’appartenance  
  
1.  Dans le volet **Appartenance** , cliquez sur **Ajouter** pour entrer les comptes d’utilisateurs ou de groupes Windows qui accéderont au cube à l’aide de ce rôle. Analysis Services prend uniquement en charge les identités de sécurité Windows. Notez que vous ne créez pas de connexions de base de données lors de cette étape. Dans Analysis Services, les utilisateurs se connectent par l'intermédiaire de comptes Windows.  
  
2.  Continuez à l'étape suivante, la définition des autorisations de cube.  
  
     Notez que nous ignorons le volet Source de données. La plupart des consommateurs ordinaires de données Analysis Services n'ont pas besoin d'autorisations sur l'objet source de données. Pour obtenir des informations sur ces niveaux d’autorisation, consultez [Octroyer des autorisations sur un objet de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-permissions-on-a-data-source-object-analysis-services.md) .  
  
#### <a name="step-3-set-cube-permissions"></a>Étape 3 : Définir les autorisations du cube  
  
1.  Dans le volet **Cubes** , sélectionnez un cube, puis cliquez sur l’accès **Lecture** ou **Lecture/Écriture** .  
  
     L’accès**Lecture** est suffisant pour la plupart des opérations. **Lecture/Écriture** sert uniquement à l’écriture différée, et non au traitement. Pour plus d’informations sur cette fonctionnalité, consultez [Définir l’écriture différée de partition](../../analysis-services/multidimensional-models/set-partition-writeback.md) .  
  
     Notez que vous pouvez sélectionner plusieurs cubes, ainsi que d'autres objets disponibles dans la boîte de dialogue Créer un rôle. L'accord d'autorisations d'accès à un cube permet également d'accéder aux dimensions et aux perspectives associées au cube. Il n'est pas nécessaire d'ajouter manuellement des objets déjà représentés dans le cube.  
  
     Si vous devez varier l'autorisation selon l'objet ou l'utilisateur, par exemple pour rendre certaines mesures indisponibles, vous pouvez autoriser ou refuser l'accès de manière atomique sur des objets spécifiques, et même sur des cellules. Pour plus d’informations, consultez [Octroyer un accès personnalisé à des données de dimension &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-custom-access-to-dimension-data-analysis-services.md) et [Octroyer un accès personnalisé à des données de cellule &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-custom-access-to-cell-data-analysis-services.md).  
  
2.  À ce stade, une fois que vous avez cliqué sur **OK**, tous les membres de ce rôle ont accès aux cubes, aux niveaux d'autorisation que vous avez spécifiés.  
  
     Notez que dans le volet **Cubes** , vous pouvez accorder aux utilisateurs l’autorisation de créer des cubes locaux à partir d’un cube serveur via **Extraction et cube local**ou autoriser uniquement l’extraction via l'autorisation **Extraction** .  
  
     Pour finir, ce volet vous permet d’accorder des droits **Traiter la base de données** sur le cube pour permettre à tous les membres de ce rôle de traiter des données pour ce cube. Le traitement étant généralement une opération restreinte, nous vous recommandons de laisser cette tâche aux administrateurs ou de définir des rôles spécifiquement pour cette tâche. Pour plus d’informations sur les bonnes pratiques concernant les autorisations de traitement, consultez [Octroyer des autorisations de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-process-permissions-analysis-services.md).  
  
#### <a name="step-4-test"></a>Étape 4 : Tester  
  
1.  Utilisez Excel pour tester les autorisations d'accès au cube. Vous pouvez également utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en suivant la même technique que celle décrite ci-dessous (exécution de l’application comme utilisateur non-administrateur).  
  
    > [!NOTE]  
    >  Si vous êtes administrateur Analysis Services, les autorisations administrateur seront combinées avec des rôles ayant des autorisations moindres, ce qui rend difficile le test des autorisations de rôle de manière isolée. Pour simplifier les tests, nous vous suggérons d'ouvrir une seconde instance de SSMS, à l'aide du compte assigné au rôle que vous testez.  
  
2.  Maintenez la touche Maj enfoncée et cliquez avec le bouton droit sur le raccourci **Excel** pour accéder à l’option **Exécuter en tant qu'autre utilisateur** . Entrez l'un des comptes d'utilisateurs ou de groupes Windows ayant une appartenance à ce rôle.  
  
3.  Une fois Excel ouvert, utilisez l'onglet Données pour vous connecter à Analysis Services. Comme vous exécutez Excel en tant qu’autre utilisateur Windows, l’option **Utiliser l’authentification Windows** est le type d’informations d’identification correct à utiliser lors du test des rôles. Si vous avez besoin d’aide sur cette étape, consultez [Connexion à partir d’applications clientes &#40;Analysis Services&#41;](../../analysis-services/instances/connect-from-client-applications-analysis-services.md).  
  
     Si vous recevez des erreurs lors de la connexion, vérifiez la configuration des ports pour Analysis Services et vérifiez que le serveur accepte les connexions à distance. Pour la configuration des ports, consultez [Configurer le pare-feu Windows pour autoriser l’accès à Analysis Services](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md) .  
  
#### <a name="step-5-script-role-definition-and-assignments"></a>Étape 5 : Écrire un script de définition et d’assignation des rôles  
  
1.  En guise d'étape finale, vous devez générer un script qui capture la définition de rôle que vous venez de créer.  
  
     Le redéploiement d’un projet à partir de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] remplace tous les rôles ou appartenances aux rôles qui ne sont pas définis dans le projet. Le moyen le plus rapide de recréer des rôles et des appartenances aux rôles après un redéploiement consiste à recourir à un script.  
  
2.  Dans SSMS, accédez au dossier **Rôles** et cliquez avec le bouton droit sur un rôle existant.  
  
3.  Sélectionnez **Générer un script du rôle en tant que** | **CREATE TO** | **fichier**.  
  
4.  Enregistrez le fichier avec une extension .xmla. Pour tester le script, supprimez le rôle actif, ouvrez le fichier dans SSMS, puis appuyez sur la touche F5 pour exécuter le script.  
  
## <a name="next-step"></a>Étape suivante  
 Vous pouvez affiner les autorisations de cube de façon à limiter l'accès à des données de dimension ou de cellule. Pour plus d’informations, consultez [Octroyer un accès personnalisé à des données de dimension &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-custom-access-to-dimension-data-analysis-services.md) et [Octroyer un accès personnalisé à des données de cellule &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-custom-access-to-cell-data-analysis-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodologies d'authentification prises en charge par Analysis Services](../../analysis-services/instances/authentication-methodologies-supported-by-analysis-services.md)   
 [Accorder des autorisations sur les structures d’exploration de données et modèles &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-permissions-on-data-mining-structures-and-models-analysis-services.md)   
 [Accorder des autorisations sur un objet de source de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-permissions-on-a-data-source-object-analysis-services.md)  
  
  
