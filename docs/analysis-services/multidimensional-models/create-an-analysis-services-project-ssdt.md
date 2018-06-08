---
title: Créer un projet Analysis Services (SSDT) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1634c864ba88afbcd9489732c5507800709f9931
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-an-analysis-services-project-ssdt"></a>Créer un projet Analysis Services (SSDT)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez définir un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] à l’aide du modèle de projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou de l’Assistant Importation de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour lire le contenu d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Si aucune solution n'est chargée actuellement dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], la création d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] crée automatiquement une solution. Sinon, le nouveau projet de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sera ajouté à la solution existante. Meilleures pratiques pour un appel de développement de solutions afin de créer des projets distincts pour différents types de données d'application, à l'aide d'une seule solution si les projets sont liés. Par exemple, vous pouvez avoir une seule solution qui contient des projets distincts pour les packages Integration Services, les bases de données Analysis Services et les rapports Reporting Services qui sont tous utilisés par la même application de gestion.  
  
 Un projet Analysis Services contient des objets utilisés dans une base de données Analysis Services. Les propriétés de déploiement du projet spécifient le serveur et le nom de la base de données sur lequel les métadonnées du projet seront déployées comme des objets instanciés.  
  
 Cette rubrique contient les sections suivantes :  
  
 [Créer un nouveau projet à l'aide du modèle de projet Analysis Services](#bkmk_NewUsingTemplate)  
  
 [Créer un nouveau projet reposant sur une base de données Analysis Services existante](#bkmk_NewUsingWizard)  
  
 [Ajouter un projet Analysis Services à une solution existante](#bkmk_AddtoExistingSolution)  
  
 [Générer et déployer la solution](#bkmk_buildDeploy)  
  
 [Dossiers de projet Analysis Services](#bkmk_ProjectFolders)  
  
 [Types de fichier Analysis Services](#bkmk_FileTypes)  
  
 [Modèles d'élément Analysis Services](#bkmk_ItemTemplates)  
  
##  <a name="bkmk_NewUsingTemplate"></a> Créer un nouveau projet à l'aide du modèle de projet Analysis Services  
 Utilisez ces instructions pour créer un projet vide dans lequel vous définissez des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que vous pouvez ensuite déployer en tant que nouvelle base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur **Fichier**, pointez sur **Nouveau**, puis cliquez sur **Projet**. Dans la boîte de dialogue **Nouveau projet** , dans le volet **Types de projets** , sélectionnez **Projets Business Intelligence**.  
  
2.  Dans la boîte de dialogue **Nouveau projet** , dans la catégorie **Modèles Visual Studio installés** , sélectionnez **Projet Analysis Services**.  
  
3.  Dans la zone de texte **Nom** , tapez le nom du projet. Le nom que vous entrez sera utilisé comme nom de base de données par défaut.  
  
4.  Dans la liste déroulante **Emplacement** , spécifiez ou sélectionnez le dossier où stocker les fichiers pour le projet, ou cliquez sur **Parcourir** pour sélectionner un dossier.  
  
5.  Pour ajouter le nouveau projet à la solution existante, dans la liste déroulante **Solution** , sélectionnez **Ajouter à la solution**.  
  
     —ou—  
  
     Pour créer une solution, dans la liste déroulante **Solution** , sélectionnez **Créer une nouvelle solution**. Pour créer un dossier pour la nouvelle solution, sélectionnez **Créer le répertoire pour la solution**. Dans la zone de texte **Nom de solution**, tapez le nom de la nouvelle solution.  
  
6.  Cliquez sur **OK**.  
  
##  <a name="bkmk_NewUsingWizard"></a> Créer un nouveau projet reposant sur une base de données Analysis Services existante  
 Utilisez l’Assistant Importation de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour créer un projet basé sur les objets de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante. Lorsque vous définissez un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] reposant sur une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante, les métadonnées de cette base de données s'ouvrent dans un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Vous pouvez ensuite modifier ces objets dans le projet, sans affecter les objets originaux, puis les déployer dans la même base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] si les propriétés du déploiement spécifient cette base de données ou dans une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] nouvellement créée à des fins de comparaison. Tant que les modifications ne sont pas déployées, elles n’ont aucune incidence sur la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante.  
  
 Vous pouvez également utiliser le modèle Importer une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour créer un projet dans une base de données de production dans laquelle des modifications ont été apportées directement depuis le déploiement du projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] initial.  
  
 Avant de traiter ou déployer le projet, vous devrez peut-être modifier le fournisseur de données qui est spécifié dans les sources de données. Si le logiciel SQL Server que vous utilisez est plus récent que le logiciel utilisé pour créer la base de données, le fournisseur de données spécifié dans votre projet peut ne pas être installé sur votre ordinateur. Au cours du traitement, le compte de service est utilisé pour récupérer les données de la base de données Analysis Services. Si la base de données se trouve sur un serveur distant, vérifiez si le service local dispose d'autorisations de lecture et de traitement sur ce serveur.  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur **Fichier**, pointez sur **Nouveau**, puis cliquez sur **Projet**. Dans la boîte de dialogue **Nouveau projet** , dans le volet **Types de projets** , sélectionnez **Projets Business Intelligence**.  
  
2.  Dans la boîte de dialogue **Nouveau projet** , dans la catégorie **Modèles Visual Studio installés** , sélectionnez **Importer une base de données Analysis Services**.  
  
3.  Entrez les informations de propriété pour le projet et la solution, notamment le nom et l'emplacement des fichiers. Cliquez sur **OK**.  
  
4.  Dans la page Bienvenue de **l’Assistant Importation de base de données Analysis Services** , cliquez sur **Suivant**.  
  
5.  Dans la page **Base de données source** , spécifiez le serveur et la base de données à partir de laquelle l’Assistant va extraire le contenu et créer le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis cliquez sur **Suivant**.  
  
     Les bases de données prises en charge sont celles créées dans les versions suivantes d'Analysis Services : [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]et [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
     Vous pouvez taper le nom de la base de données ou interroger le serveur pour afficher les bases de données existantes sur le serveur. Si la base de données se trouve sur un serveur distant ou un serveur de production, vous devrez peut-être demander l'autorisation de lire la base de données. Les paramètres de configuration du pare-feu peuvent restreindre un peu plus l'accès à une base de données. Si vous obtenez une erreur lors de la tentative de connexion à la base de données, commencez par vérifier les autorisations et les paramètres du pare-feu.  
  
6.  Une fois l’extraction du contenu de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] terminée, cliquez sur **Terminer** sur la page **Fin de l’Assistant** .  
  
7.  Ouvrez l'Explorateur de solutions pour afficher le contenu du projet.  
  
##  <a name="bkmk_AddtoExistingSolution"></a> Ajouter un projet Analysis Services à une solution existante  
 Si vous disposez déjà d'une solution qui contient tous les fichiers sources d'une application de gestion, vous pouvez ajouter un nouveau projet Analysis Services à cette solution.  
  
 L'ajout d'un projet existant à une solution associe, mais ne copie pas, le projet à la solution. Si le projet Analysis Services a été créé dans une autre solution, les fichiers de projet restent avec la solution d'origine pour laquelle ils ont été créés. Cela signifie que les modifications que vous apportez au projet via l'une ou l'autre solution traiteront le même ensemble de fichiers sources. Si ce comportement ne correspond pas à vos attentes, vous devez copier ou déplacer les fichiers projet dans le nouveau dossier de la solution, puis ajouter le projet à la solution.  
  
1.  Ouvrez la solution dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur la solution, pointez sur **Ajouter**, puis cliquez sur **Projet existant** pour sélectionner le projet à ajouter.  
  
2.  Sélectionnez un fichier .dwproj à ajouter à la solution.  
  
##  <a name="bkmk_buildDeploy"></a> Générer et déployer la solution  
 Par défaut, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] déploie un projet sur l'instance par défaut d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur l'ordinateur local. Vous pouvez modifier cette destination de déploiement en utilisant la boîte de dialogue **Pages de propriétés** du projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour modifier la propriété de configuration **Serveur** .  
  
> [!NOTE]  
>  Par défaut, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] traite uniquement les objets modifiés par le script de déploiement et les objets dépendants lors du déploiement d'une solution. Vous pouvez modifier cette fonctionnalité en utilisant la boîte de dialogue **Pages de propriétés** du projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour modifier la propriété de configuration Option de traitement.  
  
 Créez et déployez la solution dans une instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] afin de procéder à des essais. La création d'une solution valide les définitions des objets et les dépendances dans le projet et génère un script de déploiement. Le déploiement d'une solution utilise le moteur de déploiement d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour envoyer le script de déploiement à une instance spécifiée.  
  
 Après avoir déployé le projet, examinez et testez la base de données déployée. Vous pouvez ensuite modifier des définitions d'objets, générer et redéployer jusqu'à ce que le projet soit terminé.  
  
 Une fois le projet terminé, vous pouvez utiliser l'Assistant Déploiement pour déployer le script de déploiement, généré lorsque vous créez la solution, dans les instances de destination pour les tests, les mises en lot et le déploiement final.  
  
##  <a name="bkmk_ProjectFolders"></a> Dossiers de projet Analysis Services  
 Un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] contient les dossiers répertoriés ci-dessous, lesquels permettent d’organiser les éléments inclus dans le projet.  
  
|Dossier|Description|  
|------------|-----------------|  
|Sources de données|Contient les sources de données d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez créer ces objets à l'aide de l'Assistant Source de données et les modifier dans le Concepteur de source de données.|  
|Vues de source de données|Contient les vues de source de données d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez créer ces objets à l'aide de l'Assistant Vues de source de données et les modifier dans le Concepteur de vues de source de données.|  
|Cubes|Contient les cubes d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez créer ces objets à l'aide de l'Assistant Cube et les modifier dans le Concepteur de cube.|  
|Dimensions|Contient les dimensions d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez créer ces objets à l'aide de l'Assistant Dimension ou de l'Assistant Cube, et les modifier dans le Concepteur de dimensions.|  
|Structures d'exploration de données|Contient les structures d'exploration de données d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez créer ces objets à l'aide de l'Assistant Modèle d'exploration de données et les modifier dans le Concepteur d'exploration de données.|  
|Rôles|Contient les rôles de base de données d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez créer et gérer des rôles dans le Concepteur de rôles.|  
|Assemblys|Contient les références aux bibliothèques COM et aux assemblys [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez créer des références avec la boîte de dialogue **Ajouter une référence** .|  
|Divers|Contient tout type de fichier à l'exception des types de fichiers [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Utilisez ce dossier pour ajouter des fichiers divers, tels que les fichiers texte contenant des notes sur le projet.|  
  
##  <a name="bkmk_FileTypes"></a> Types de fichier Analysis Services  
 Une solution [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] peut contenir plusieurs types de fichiers, en fonction des projets que vous avez inclus dans la solution et des éléments que vous avez inclus dans chaque projet pour cette solution. En règle générale, les fichiers de chaque projet dans une solution [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] sont stockés dans le dossier de la solution, dans un dossier distinct pour chaque projet.  
  
> [!NOTE]  
>  La copie d'un fichier pour un objet dans un dossier du projet n'ajoute pas l'objet au projet. Vous devez utiliser la commande **Ajouter** du menu contextuel du projet dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour ajouter la définition d’un objet existant à un projet.  
  
 Le dossier de projet d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut contenir les types de fichiers répertoriés dans le tableau ci-dessous.  
  
|Type de fichier|Description|  
|---------------|-----------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]fichier de définition de projet (.dwproj)|Contient des métadonnées sur les éléments, les configurations et les références d’assembly définis et inclus dans le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|Paramètres utilisateur du projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (.dwproj.user)|Contient les informations de configuration du projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour un utilisateur spécifique.|  
|Fichier de source de données (.ds)|Contient les éléments ASSL (Analysis Services Scripting Language) [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui définissent les métadonnées pour une source de données.|  
|Fichier de vue de source de données (.dsv)|Contient les éléments ASSL qui définissent des métadonnées pour une vue de source de données.|  
|Fichier de cube (.cube)|Contient les éléments ASSL qui définissent des métadonnées pour un cube, y compris des groupes de mesures, des mesures et des dimensions de cube.|  
|Fichier de partition (.partitions)|Contient les éléments ASSL qui définissent des métadonnées pour les partitions d'un cube spécifié.|  
|Fichier de dimension (.dim)|Contient les éléments ASSL qui définissent des métadonnées pour une dimension de base de données.|  
|Fichier de structure d'exploration de données (.dmm)|Contient les éléments ASSL qui définissent des métadonnées pour une structure d'exploration de données et les modèles d'exploration de données associés.|  
|Fichier de base de données (.database)|Contient les éléments ASSL qui définissent des métadonnées pour une base de données, y compris des types de comptes, des traductions et des autorisations de base de données.|  
|Fichier de rôle de base de données (.role)|Contient les éléments ASSL qui définissent des métadonnées pour un rôle de base de données, y compris les membres du rôle.|  
  
##  <a name="bkmk_ItemTemplates"></a> Modèles d'élément Analysis Services  
 Si vous utilisez la boîte de dialogue **Ajouter un nouvel élément** pour ajouter de nouveaux éléments à un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vous avez la possibilité d’utiliser un modèle d’élément, qui est un script ou une instruction prédéfinie qui montre comment effectuer une action spécifiée.  
  
 Les modèles d’éléments répertoriés dans le tableau ci-dessous sont disponibles dans la catégorie Éléments du projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , dans la boîte de dialogue **Ajouter un nouvel élément** .  
  
|Catégorie|Modèle d'élément|Description|  
|--------------|-------------------|-----------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]Éléments de projet|Cube|Démarre l’Assistant Cube pour ajouter un nouveau cube au projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
||Source de données|Démarre l’Assistant Source de données pour ajouter une nouvelle source de données au projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
||Vue de source de données|Démarre l’Assistant Source de données pour ajouter une nouvelle vue de source de données au projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
||Rôle de base de données|Ajoute un nouveau rôle de base de données dans le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis affiche le Concepteur de rôles pour le nouveau rôle de base de données.|  
||Dimension|Démarre l’Assistant Dimension pour ajouter une nouvelle dimension de base de données au projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
||Structure d'exploration de données|Démarre l’Assistant Exploration de données pour ajouter une nouvelle structure d’exploration de données et le modèle d’exploration de données associé au projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer les propriétés de projet Analysis Services & #40 ; SSDT & #41 ;](../../analysis-services/multidimensional-models/configure-analysis-services-project-properties-ssdt.md)   
 [Générer des projets Analysis Services & #40 ; SSDT & #41 ;](../../analysis-services/multidimensional-models/build-analysis-services-projects-ssdt.md)   
 [Déployer des projets Analysis Services & #40 ; SSDT & #41 ;](../../analysis-services/multidimensional-models/deploy-analysis-services-projects-ssdt.md)  
  
  
