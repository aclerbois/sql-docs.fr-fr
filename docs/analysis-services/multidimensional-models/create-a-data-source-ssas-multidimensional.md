---
title: Créer une Source de données (SSAS multidimensionnel) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c461f38e7e2d0cf43cf206fee0c474f6fd74d35a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-data-source-ssas-multidimensional"></a>Créer une source de données (SSAS Multidimensionnel)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans un modèle multidimensionnel [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , un objet de source de données représente une connexion à la source de données depuis laquelle vous traitez (ou importez) des données. Un modèle multidimensionnel doit contenir au moins un objet de source de données, mais rien ne vous empêche d'en ajouter plus pour combiner des données émanant de plusieurs entrepôts de données. Suivez les instructions de cette rubrique pour créer un objet de source de données pour votre modèle. Pour plus d’informations sur la définition des propriétés de cet objet, consultez [Définir les propriétés de la source de données &#40;SSAS Multidimensionnel&#41;](../../analysis-services/multidimensional-models/set-data-source-properties-ssas-multidimensional.md).  
  
 Cette rubrique comprend les sections suivantes :  
  
 [Choisir un fournisseur de données](#bkmk_provider)  
  
 [Définir les options d'emprunt d'identité et les informations d'identification](#bkmk_impersonation)  
  
 [Afficher ou modifier les propriétés de connexion](#bkmk_ConnectionString)  
  
 [Créer une source de données à l'aide de l'Assistant Source de données](#bkmk_steps)  
  
 [Créer une source de données à l'aide d'une connexion existante](#bkmk_connection)  
  
 [Ajouter plusieurs sources de données à un modèle](#bkmk_multipleDS)  
  
##  <a name="bkmk_provider"></a> Choisir un fournisseur de données  
 Vous pouvez vous connecter via un fournisseur OLE DB [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework managé ou natif. Le fournisseur de données recommandé pour les sources de données SQL Server est SQL Server Native Client car il offre généralement de meilleures performances.  
  
 Pour Oracle et d'autres sources de données tierces, vérifiez si le tiers fournit un fournisseur OLE DB natif et essayez celui-ci en premier. Si vous rencontrez des erreurs, essayez les autres fournisseurs .NET ou fournisseurs OLE DB natifs répertoriés dans le Gestionnaire de connexions. Assurez-vous que tous les fournisseurs de données que vous utilisez sont installés sur tous les ordinateurs utilisés pour développer et exécuter la solution [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
##  <a name="bkmk_impersonation"></a> Définir les options d'emprunt d'identité et les informations d'identification  
 Une connexion de source de données peut parfois utiliser l'authentification Windows ou un service d'authentification fourni par le système de gestion de base de données, comme l'authentification SQL Server, lors de la connexion aux bases de données SQL Azure. Le compte que vous spécifiez doit avoir une connexion sur le serveur de base de données distant et des autorisations de lecture sur la base de données externe.  
  
### <a name="windows-authentication"></a>Authentification Windows  
 Les connexions qui utilisent l’authentification Windows sont spécifiées sous l’onglet **Informations d’emprunt d’identité** du Concepteur de source de données. Utilisez cet onglet pour choisir l'option d'emprunt d'identité qui spécifie le compte sous lequel [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] s'exécute lors de la connexion à la source de données externe. Toutes les options ne peuvent pas être utilisées dans tous les cas. Pour plus d’informations sur ces options et sur leur utilisation, consultez [Définir les options d’emprunt d’identité &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/set-impersonation-options-ssas-multidimensional.md).  
  
### <a name="database-authentication"></a>Authentification de base de données  
 Comme alternative à l'authentification Windows, vous pouvez spécifier une connexion qui utilise un service d'authentification fourni par le système de gestion de base de données. Dans certains cas, l'utilisation de l'authentification de base de données est requise. Les scénarios qui nécessitent l'utilisation de l'authentification de base de données incluent l'utilisation de l'authentification SQL Server pour la connexion à une base de données SQL Windows Azure, ou l'accès à une source de données relationnelle qui s'exécute sur un système d'exploitation différent ou dans un domaine non approuvé.  
  
 Pour une source de données qui utilise l'authentification de base de données, le nom d'utilisateur et le mot de passe d'une connexion à une base de données sont spécifiés dans la chaîne de connexion. Les informations d'identification sont ajoutées à la chaîne de connexion lorsque vous entrez un nom d'utilisateur et un mot de passe dans le Gestionnaire de connexions lors de la configuration de la connexion à la source de données dans votre modèle [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . N'oubliez pas de spécifier une identité d'utilisateur qui a des autorisations de lecture des données.  
  
 Lors de la récupération des données, la bibliothèque cliente qui établi la connexion formule une demande de connexion qui inclut les informations d'identification dans la chaîne de connexion. Les options relatives aux informations d'identification de l'authentification Windows dans l'onglet Informations d'emprunt d'identité ne sont pas utilisées dans la connexion, mais elles peuvent être utilisées pour d'autres opérations, telles que l'accès aux ressources sur l'ordinateur local. Pour plus d’informations, consultez [Définir les options d’emprunt d’identité &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/set-impersonation-options-ssas-multidimensional.md).  
  
 Après avoir enregistré l'objet de source de données dans votre modèle, la chaîne de connexion et le mot de passe sont chiffrés.  Pour des raisons de sécurité, toutes les traces visibles du mot de passe sont supprimées de la chaîne de connexion lorsque vous l'affichez ensuite dans les outils, le script ou le code.  
  
> [!NOTE]  
>  Par défaut, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] n’enregistre pas les mots de passe avec la chaîne de connexion. Si le mot de passe n’est pas enregistré, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vous invite à le taper si nécessaire. Si vous avez choisi d'enregistrer le mot de passe, ce dernier est stocké dans un format chiffré dans la chaîne de connexion de données. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] chiffre les informations de mot de passe pour les sources de données à l’aide de la clé de chiffrement de la base de données qui contient la source de données. Avec les informations de connexion chiffrées, vous devez utiliser le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour modifier le mot de passe ou le compte de service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , sinon les informations chiffrées ne peuvent pas être récupérées. Pour plus d’informations, consultez [SQL Server Configuration Manager](../../relational-databases/sql-server-configuration-manager.md).  
  
### <a name="defining-impersonation-information-for-data-mining-objects"></a>Définition des informations d'emprunt d'identité pour les objets d'exploration de données  
 Les requêtes d'exploration de données peuvent être exécutées dans le contexte du compte de service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , mais également dans le contexte de l'utilisateur qui soumet la requête ou dans le contexte d'un utilisateur spécifique. Le contexte dans lequel une requête est exécutée peut avoir une incidence sur les résultats de la requête. Pour les opérations d’exploration de données de type **OPENQUERY** , vous pouvez exécuter la requête d’exploration de données dans le contexte de l’utilisateur actuel ou dans le contexte d’un utilisateur spécifique (quel que soit l’utilisateur qui soumet la requête) plutôt que dans le contexte d’un compte de service. Cela permet d'exécuter la requête avec des informations d'identification de sécurité limitées. Si vous souhaitez que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] emprunte l’identité de l’utilisateur actuel ou d’un utilisateur spécifique, sélectionnez l’option **Utiliser un nom d’utilisateur et un mot de passe spécifiques** ou **Utiliser les infos d’identification de l’utilisateur actuel** .  
  
##  <a name="bkmk_steps"></a> Créer une source de données à l'aide de l'Assistant Source de données  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou connectez-vous à la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans laquelle vous souhaitez définir la source de données.  
  
2.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **Sources de données** , puis sélectionnez **Nouvelle source de données** pour démarrer **l’Assistant Source de données**.  
  
3.  Dans la page **Sélectionner la méthode de définition de la connexion** , choisissez **Créer une source de données basée sur une connexion existante ou nouvelle** , puis cliquez sur **Nouveau** pour ouvrir le **Gestionnaire de connexions**.  
  
     Les nouvelles connexions sont créées dans le Gestionnaire de connexions. Dans le Gestionnaire de connexions, sélectionnez un fournisseur, puis spécifiez les propriétés de chaîne de connexion utilisées par ce fournisseur pour la connexion aux données sous-jacentes. Les informations exactes nécessaires dépendent du fournisseur, mais ces informations incluent en général un serveur ou une instance de service, des informations pour l'enregistrement dans un journal sur le serveur ou l'instance de service, un nom de base de données ou de fichier, ainsi que d'autres paramètres spécifiques au fournisseur. Pour le reste de cette procédure, nous allons supposer qu'une connexion de base de données SQL Server existe.  
  
4.  Sélectionnez [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework ou le fournisseur OLE DB natif à utiliser pour la connexion.  
  
     Le fournisseur par défaut d’une nouvelle connexion est le fournisseur OLE DB natif\\[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Ce fournisseur est utilisé pour se connecter à une instance du moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide d’OLE DB. Pour les connexions à une base de données relationnelle SQL Server, l'utilisation du mode natif OLE DB\SQL Server Native Client 11.0 est souvent plus rapide que l'utilisation d'autres fournisseurs.  
  
     Vous pouvez choisir un fournisseur différent pour accéder à d'autres sources de données. Pour obtenir la liste des fournisseurs et des bases de données relationnelles pris en charge par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Sources de données prises en charge &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/supported-data-sources-ssas-multidimensional.md).  
  
5.  Tapez les informations requises par le fournisseur sélectionné pour établir la connexion à la source de données sous-jacente. Si le fournisseur **OLE DB natif\SQL Server Native Client** est sélectionné, tapez les informations suivantes :  
  
    1.  **Nom du serveur** correspond au nom réseau de l’instance du moteur de base de données. Il peut être spécifié comme adresse IP, nom NetBIOS de l'ordinateur ou nom de domaine complet. Si le serveur est installé comme instance nommée, vous devez inclure le nom d’instance (par exemple, \<nom_ordinateur >\\< nom_instance\>).  
  
    2.  **Connexion au serveur** spécifie le mode d’authentification de la connexion. **Utiliser l’authentification Windows** fait appel à l’authentification Windows. **Utiliser l’authentification SQL Server** spécifie une connexion utilisateur de base de données pour une base de données SQL Microsoft Azure ou une instance SQL Server qui prend en charge le mode d’authentification mixte.  
  
        > [!IMPORTANT]  
        >  Le gestionnaire de connexions inclut une case à cocher **Enregistrer mon mot de passe** pour les connexions qui utilisent l’authentification SQL Server. Bien que la case à cocher soit toujours visible, elle n'est pas toujours utilisée.  
        >   
        >  Les conditions dans lesquelles Analysis Services n'utilise pas cette case à cocher incluent l'actualisation ou le traitement des données relationnelles SQL Server qui sont utilisées dans une base de données Analysis Services active. Que vous désactiviez ou activiez l’option **Enregistrer mon mot de passe**, Analysis Services chiffre et enregistre toujours le mot de passe. Le mot de passe est chiffré et stocké dans les fichiers .abf et dans les fichiers de données. Ce comportement a lieu car Analysis Services ne prend pas en charge le stockage du mot de passe de session sur le serveur.  
        >   
        >  Ce comportement s'applique uniquement aux bases de données qui a) sont conservées sur une instance de serveur Analysis Services, et b) utilisent l'authentification SQL Server pour actualiser ou traiter les données relationnelles. Il ne s'applique pas aux connexions à la source de données que vous configurez dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] utilisées uniquement pour la durée d'une session. Lorsqu'il est impossible de supprimer un mot de passe déjà stocké, vous pouvez utiliser d'autres informations d'identification, ou l'authentification Windows, pour remplacer les informations utilisateur qui sont actuellement stockées avec la base de données.  
  
    3.  **Sélectionner ou entrer un nom de base de données** ou **Attacher un fichier de base de données** permettent de spécifier la base de données.  
  
    4.  Dans la partie gauche de la boîte de dialogue, cliquez sur **Tous** pour afficher des paramètres supplémentaires pour cette connexion, notamment tous les paramètres par défaut pour ce fournisseur.  
  
    5.  Modifiez les paramètres en fonction de votre environnement, puis cliquez sur **OK**.  
  
         La nouvelle connexion apparaît dans le volet **Connexions de données** de la page **Sélectionner la méthode de définition de la connexion** dans l’Assistant Source de données.  
  
6.  Cliquez sur **Suivant**.  
  
7.  Sous **Informations d’emprunt d’identité**, spécifiez les informations d’identification ou l’identité d’utilisateur Windows qu’Analysis Services utilisera au moment de la connexion à la source de données externe. Si vous utilisez l'authentification de base de données, ces paramètres sont ignorés pour la connexion.  
  
     Les instructions de sélection d'une option d'emprunt d'identité varient selon le mode d'utilisation de la source de données. Pour les tâches de traitement, le service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit s'exécuter dans le contexte de sécurité du compte de service ou d'un compte d'utilisateur spécifié lors de la connexion à une source de données.  
  
    -   **Utiliser un nom d’utilisateur et un mot de passe Windows spécifiques** pour spécifier un ensemble unique d’informations d’identification doté de privilèges minimaux.  
  
    -   **Utiliser le compte de service** pour traiter les données à l’aide de l’identité de service.  
  
     Le compte que vous spécifiez doit disposer d'autorisations en lecture sur la source de données.  
  
8.  Cliquez sur **Suivant**.  Dans **Fin de l’Assistant**, entrez un nom de source de données ou utilisez le nom par défaut. Le nom par défaut correspond au nom de la base de données spécifiée dans la connexion. Le volet **Aperçu** affiche la chaîne de connexion pour cette nouvelle source de données.  
  
9. Cliquez sur **Terminer**.  La nouvelle source de données apparaît dans le dossier **Sources de données** , dans l’Explorateur de solutions.  
  
##  <a name="bkmk_connection"></a> Créer une source de données à l'aide d'une connexion existante  
 Si vous utilisez un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , la source de données peut reposer sur une source de données existante dans la solution ou sur un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . L'Assistant Source de données fournit plusieurs options pour créer l'objet de source de données, notamment l'utilisation d'une connexion existante dans le même projet.  
  
-   Créer une source de données basée sur une source de données existante dans votre solution vous permet de définir une source de données synchronisée avec la source de données existante. Lorsque le projet contenant cette nouvelle source de données est généré, les paramètres de source de données de la source de données sous-jacente sont utilisés.  
  
-   Créer une source de données basée sur un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vous permet de référencer un autre projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans la solution du projet en cours. La nouvelle source de données utilise le fournisseur MSOLAP.3 et ses propriétés **Source de données** et **Catalogue initial** acquises à partir des propriétés **TargetServer** et **TargetDatabase** du projet sélectionné. Cette fonctionnalité est utile dans des solutions où vous utilisez plusieurs projets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour gérer des partitions distantes, car les bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] source et de destination requièrent des sources de données réciproques pour prendre en charge le stockage et le traitement sur les partitions distantes.  
  
 Si vous référencez un objet source de données, vous pouvez modifier cet objet uniquement dans l'objet ou le projet référencé. Vous ne pouvez pas modifier les informations de connexion dans l'objet de source de données qui contient la référence. Les modifications apportées aux informations de connexion dans l'objet ou le projet référencé apparaissent dans la nouvelle source de données lorsqu'elle est générée. Les informations de chaîne de connexion qui apparaissent dans le fichier de source de données (.ds), dans le projet, sont synchronisées lorsque vous créez le projet ou lorsque vous supprimez la référence dans le Concepteur de source de données.  
  
##  <a name="bkmk_ConnectionString"></a> Afficher ou modifier les propriétés de connexion  
 La chaîne de connexion est formulée sur la base des propriétés que vous sélectionnez dans le Concepteur de source de données ou dans l'Assistant Nouvelle source de données. Vous pouvez afficher la chaîne de connexion et d’autres propriétés dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
 **Pour modifier la chaîne de connexion**  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], double-cliquez sur l’objet de source de données dans l’Explorateur de solutions.  
  
2.  Cliquez sur **Modifier**, puis sur **Tout** dans le volet de navigation gauche.  
  
3.  La grille des propriétés apparaît, avec la liste des propriétés disponibles pour le fournisseur de données que vous utilisez. Pour plus d'informations sur ces propriétés, consultez la documentation produit du fournisseur.  Pour SQL Server Native Client, consultez [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Si vous disposez de plusieurs objets de source de données dans la solution et préférez conserver la chaîne de connexion à un emplacement, vous pouvez configurer la source de données active pour référencer l'autre objet de source de données.  
  
 Une *référence de source de données* est une association à un autre projet ou source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans la même solution. Les références permettent de synchroniser les sources de données entre les objets d'une solution. Les informations de la chaîne de connexion sont synchronisées chaque fois que vous générez le projet. Pour modifier la chaîne de connexion d’une source de données qui fait référence à un autre objet, vous devez modifier la chaîne de connexion de l’objet référencé.  
  
 Vous pouvez supprimer la référence en désactivant la case à cocher. Cela met un terme à la synchronisation entre les objets et vous permet de modifier la chaîne de connexion dans la source de données.  
  
##  <a name="bkmk_multipleDS"></a> Ajouter plusieurs sources de données à un modèle  
 Vous pouvez créer plusieurs objets de source de données pour prendre en charge la connexion à des sources de données supplémentaires. Chaque source de données doit comporter des colonnes pouvant servir à créer des relations.  
  
> [!NOTE]  
>  Si plusieurs sources de données sont définies et que les données sont interrogées à partir de plusieurs sources dans une seule requête, notamment pour une dimension en flocon, vous devez définir une source de données qui prend en charge les requêtes distantes à l’aide **d’OpenRowset**. Il s'agit généralement d'une source de données Microsoft SQL Server.  
  
 Les spécifications liées à l'utilisation de plusieurs sources de données sont les suivantes :  
  
-   Désignez une source de données comme source de données principale. La source de données principale est celle utilisée pour créer une vue de source de données.  
  
-   Une source de données principale doit prendre en charge la fonction **OpenRowset** .  Pour plus d’informations sur cette fonction dans SQL Server, consultez <xref:Microsoft.SqlServer.TransactSql.ScriptDom.TSqlTokenType.OpenRowSet>.  
  
 Utilisez la méthode suivante pour combiner des données provenant de plusieurs sources de données :  
  
1.  Créez les sources de données dans votre modèle.  
  
2.  Créez une vue de source de données à l'aide d'une base de données relationnelle SQL Server servant de source de données. Il s'agit de votre source de données principale.  
  
3.  Dans le Concepteur de vue de source de données, à l’aide de la vue de source de données que vous venez de créer, cliquez avec le bouton droit dans la zone de travail, puis sélectionnez **Ajouter/Supprimer des tables**.  
  
4.  Sélectionnez la seconde source de données, puis les tables que vous souhaitez ajouter.  
  
5.  Recherchez et sélectionnez la table que vous avez ajoutée. Cliquez avec le bouton droit sur la table et sélectionnez **Nouvelle relation**. Choisissez les colonnes source et de destination qui contiennent des données correspondantes.  
  
## <a name="see-also"></a>Voir aussi  
 [Sources de données prises en charge &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/supported-data-sources-ssas-multidimensional.md)   
 [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md)  
  
  
