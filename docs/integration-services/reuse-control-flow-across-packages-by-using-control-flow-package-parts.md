---
title: Réutiliser un flux de contrôle sur des packages à l’aide de composants de package de flux de contrôle | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.toolboxcontrolflowtemplate.f1
- sql13.dts.designer.addcopyexistingtemplate.f1
- sql13.dts.designer.addcopyexistingpackagepart.f1
- sql13.dts.designer.packagepart.general.f1
ms.assetid: 1edc91d9-1fab-4fe5-aed3-6f581fe32c18
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f9272ce373bbd59b76104b4437f55a98ec89d429
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reuse-control-flow-across-packages-by-using-control-flow-package-parts"></a>Réutiliser un flux de contrôle sur des packages à l’aide de composants de package de flux de contrôle
  Enregistrez une tâche ou un conteneur de flux de contrôle couramment utilisés dans un fichier de pièce (fichier « .dtsxp ») autonome, et réutilisez celui-ci plusieurs fois dans un ou plusieurs packages à l’aide de composants de package de flux de contrôle. Cette réutilisation facilite la conception et la gestion des packages SSIS.  
  
## <a name="create-a-new-control-flow-package-part"></a>Créer un composant de package de flux de contrôle  
 Pour créer un composant de package de flux contrôle, dans l’Explorateur de solutions, développez le dossier **Package Parts** (Composants de package). Cliquez avec le bouton droit sur **Control Flow** (Flux de contrôle), puis sélectionnez **New Control Flow Package Part**(Nouveau composant de package de flux de contrôle).  
  
 ![Créer un modèle de flux de contrôle](../integration-services/media/control-flow-templates-create-new.png "Créer un modèle de flux de contrôle")  
  
 Un nouveau fichier de composant portant l’extension « .dtsxp » est créé dans le dossier **Package Parts | Control Flow** (Composants de Package | Flux de contrôle). En même temps, un nouvel élément du même nom est ajouté à la boîte à outils SSIS. (L’élément de boîte à outils est visible uniquement pendant que vous avez un projet contenant le composants ouvert dans Visual Studio.)  
  
 ![Modèles de flux de contrôle dans la boîte à outils](../integration-services/media/control-flow-templates-in-toolbox.png "Modèles de flux de contrôle dans la boîte à outils")  
  
## <a name="design-a-control-flow-package-part"></a>Concevoir un composant de package de flux de contrôle  
 Pour ouvrir l’éditeur de composant de package, double-cliquez sur le fichier du composant dans l’Explorateur de solutions. Vous pouvez concevoir le composant de la même manière qu’un package.  
  
 ![Étape 1 de la conception de modèle de flux de contrôle](../integration-services/media/control-flow-template-design-step-1.png "Étape 1 de la conception de modèle de flux de contrôle")  
  
 ![Étape 2 de la conception de modèle de flux de contrôle](../integration-services/media/control-flow-template-design-step-2.png "Étape 2 de la conception de modèle de flux de contrôle")  
  
 Vérifiez que les composants du flux de contrôle présentent les limitations suivantes.  
  
-   Un composant ne peut avoir qu’une seule tâche ou un seul conteneur de niveau supérieur. Si vous voulez inclure plusieurs tâches ou conteneurs, placez-les tous dans un conteneur de séquences unique.  
  
-   Vous ne pouvez pas exécuter ou déboguer un composant directement dans le concepteur.  
  
## <a name="add-an-existing-control-flow-package-part-to-a-package"></a>Ajouter un composant de package de flux contrôle existant à un package  
 Vous pouvez réutiliser des composants enregistrés dans le projet Integration Services en cours ou dans un autre projet.  
  
-   Pour réutiliser un composant faisant partie du projet actuel, glissez-déplacez le composant à partir de la boîte à outils.  
  
-   Pour réutiliser un composant faisant partie d’un autre projet, utilisez la commande **Add Existing Control Flow Package Part** (Ajouter un composant de package de flux de contrôle existant).  
  
### <a name="drag-and-drop-a-control-flow-package-part"></a>Glisser-déplacer un composant de package de flux de contrôle  
 Pour réutiliser un composant dans un projet, glissez-déplacez simplement l’élément composant comme tout autre conteneur ou tâche à partir de la boîte à outils. Vous pouvez glisser-déplacer le composant vers un package plusieurs fois afin de réutiliser la logique en plusieurs emplacements du package. Recourez à cette méthode pour réutiliser un composant faisant partie du projet actuel.  
  
 ![Ajouter un modèle de flux de contrôle à un package](../integration-services/media/control-flow-templates-add-to-package.png "Ajouter un modèle de flux de contrôle à un package")  
  
 ![Package avec plusieurs modèles de flux de contrôle](../integration-services/media/control-flow-templates-in-package.png "Package avec plusieurs modèles de flux de contrôle")  
  
 Lorsque vous enregistrez le package, le concepteur SSIS vérifie s’il existe des instances du composant dans le package.  
  
-   Si le package contient des instances du composant, le concepteur génère un nouveau .dtsx.designer contenant toutes les informations relatives au composant.  
  
-   Si le package n’utilise pas de composant, le concepteur supprime tout fichier .dtsx.designer créé précédemment pour le package (autrement dit, tout fichier .dtsx.designer portant le même nom que le package).  
  
 ![Explorateur de solutions avec des modèles de flux de contrôle](../integration-services/media/control-flow-templates-in-solution-explorer.png "Explorateur de solutions avec des modèles de flux de contrôle")  
  
### <a name="add-a-copy-of-an-existing-control-flow-package-part-or-a-reference-to-an-existing-part"></a>Ajouter une copie d’un composant de package de flux contrôle existant ou une référence à un composant existant  
 Pour ajouter à un package une copie d’un composant existant dans le système de fichiers, dans l’Explorateur de solutions, développez le dossier **Package Parts** (Composants de package). Cliquez avec le bouton droit sur **Control Flow** (Flux de contrôle), puis sélectionnez **Add Existing Control Flow Package Part**(Ajouter un composant de package de flux de contrôle existant).  
  
 ![Ajouter un nouveau modèle de flux de contrôle à partir du menu](../integration-services/media/control-flow-templates-add-from-menu.png "Ajouter un nouveau modèle de flux de contrôle à partir du menu")  
  
 ![Boîte de dialogue Ajouter une copie de modèles existants](../integration-services/media/control-flow-templates-add-copy-dialog.png "Boîte de dialogue Ajouter une copie de modèles existants")  
  
 **Options**  
  
 **Chemin d’accès au composant de package**  
 Tapez le chemin de fichier du composant, ou cliquez sur le bouton Parcourir (...) pour localiser le fichier de composant à copier ou à référencer.  
  
 **Ajouter en tant que référence**  
 -   Si le composant est sélectionné, il est ajouté au projet Integration Services en tant que référence. Sélectionnez cette option si vous voulez référencer une copie unique d’un fichier de composant dans plusieurs projets Integration Services.  
  
-   Si cette option est désactivée, une copie du fichier du composant est ajoutée au projet.  
  
## <a name="configure-a-control-flow-package-part"></a>Configurer un composant de package de flux de contrôle  
 Pour configurer des composants de package de flux de contrôle après les avoir ajoutés au flux de contrôle d’un package, utilisez la boîte de dialogue **Package Part Configuration**  (Configuration de composant de package).  
  
#### <a name="to-open-the-package-part-configuration-dialog-box"></a>Pour ouvrir la boîte de dialogue Package Part Configuration (Configuration de composant de package)  
  
1.  Pour configurer une instance de composant, double-cliquez dessus dans le flux de contrôle. Vous pouvez également cliquer avec le bouton droit sur l’instance de composant, puis sélectionner **Modifier**. La boîte de dialogue **Package Part Configuration** (Configuration de composant de package).  
  
2.  Configurez les propriétés et les gestionnaires de connexion pour l’instance de composant.  
  
### <a name="properties-tab"></a>Onglet Propriétés  
 Utilisez l’onglet **Properties** (Propriétés) de la boîte de dialogue **Package Part Configuration**  (Configuration de composant de package).  
  
 ![Onglet Propriétés de la boîte de dialogue Configuration du modèle](../integration-services/media/template-configuration-properties-tab.png "Onglet Propriétés de la boîte de dialogue Configuration du modèle")  
  
 La hiérarchie d’arborescence dans le volet gauche répertorie toutes les propriétés configurables de l’instance de composant.  
  
-   Si elle est désactivée, la propriété n’est pas configurée dans l’instance de composant. L’instance de composant utilise la valeur par défaut de la propriété, qui est définie dans le composant de package de flux de contrôle.  
  
-   Si elle est sélectionnée, la valeur que vous entrez ou sélectionnez remplace la valeur par défaut.  
  
 La table dans le volet droit répertorie les propriétés à configurer.  
  
-   **Property Path**(Chemin d’accès à la propriété). Chemin d’accès à la propriété.  
  
-   **Property Type**(Type de propriété). Type de données de la propriété.  
  
-   **Valeur**. Valeur configurée. Cette valeur remplace la valeur par défaut.  
  
### <a name="connection-managers-tab"></a>Onglet Connection Managers (Gestionnaires de connexions)  
 L’onglet **Connection Managers** (Gestionnaires de connexions) dans la boîte de dialogue **Package Part Configuration**  (Configuration de composant de package) permet de spécifier les propriétés des gestionnaires de connexions pour l’instance de composant.  
  
 ![Onglet Gestionnaires de connexions de la boîte de dialogue Configuration du modèle](../integration-services/media/template-configuration-connection-managers-tab.png "Onglet Gestionnaires de connexions de la boîte de dialogue Configuration du modèle")  
  
 Le tableau dans le volet gauche répertorie tous les gestionnaires de connexions définis dans le composant de flux de contrôle. Choisissez le gestionnaire de connexions à configurer.  
  
 La liste dans le volet droit répertorie les propriétés du gestionnaire de connexions sélectionné.  
  
-   **Set**(Définie). Option activée si la propriété est configurée pour l’instance de composant.  
  
-   **Property Name**(Nom de la propriété). Nom de la propriété.  
  
-   **Valeur**. Valeur configurée. Cette valeur remplace la valeur par défaut.  
  
## <a name="delete-a-control-flow-part"></a>Supprimer un composant de flux de contrôle  
 Pour supprimer un composant, dans l’Explorateur de solutions, cliquez avec le bouton droit sur le composant, puis sélectionnez **Delete**(Supprimer). Sélectionnez **OK** pour confirmer la suppression, ou sélectionnez **Cancel** (Annuler) pour conserver le composant.  
  
 Si vous supprimez un composant d’un projet, il est supprimé définitivement du système de fichiers et ne peut pas être restauré.  
  
> [!NOTE]  
>  Si vous voulez supprimer un composant d’un projet Integration Services, mais continuer à l’utiliser dans d’autres projets, utilisez l’option **Exclude from Project**  (Exclure du projet ) au lieu de l’option **Delete** (Supprimer).  
  
## <a name="package-parts-are-a-design-time-feature-only"></a>Les composants de package sont des fonctionnalités disponibles uniquement au moment de la conception.  
 Les composants de package sont des fonctionnalités disponibles au moment du design caractéristiques. Le concepteur SSIS crée, ouvre, enregistre et met à jour des composants, et ajoute, configure ou supprime des instances de composant dans un package. En revanche, le runtime SSIS n’est pas informé de la présence de composants. Voici comment le concepteur accomplit cette séparation.  
  
-   Le concepteur enregistre les instances de composant de package avec leurs propriétés configurées pour un fichier « .dtsx.designer ».  
  
-   Lorsque le concepteur enregistre le fichier « .dtsx.designer », il extrait également le contenu des composants référencés par ce fichier et remplace les instances de composant dans le package par le contenu des composants.  
  
-   Enfin, tout le contenu, qui n’inclut plus d’informations sur le composant, est réenregistré dans le fichier de package « .dtsx ». Il s’agit du fichier que le runtime SSIS exécute.  
  
 Le diagramme ci-dessous illustre la relation entre les composants (fichiers « .dtsxp »), le concepteur SSIS et le runtime SSIS.  
  
 ![Fichiers de modèles de flux de contrôle et flux](../integration-services/media/control-flow-templates-intro.png "Fichiers de modèles de flux de contrôle et flux")  
  
  
