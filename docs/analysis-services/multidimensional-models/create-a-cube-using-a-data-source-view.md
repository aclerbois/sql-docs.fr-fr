---
title: Créer un Cube à l’aide d’une vue de Source de données | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 958120b827c7861069e17ab1271d578ae498afb5
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-cube-using-a-data-source-view"></a>Créer un cube à l'aide d'une vue de source de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Utilisez cette méthode de génération d'un nouveau cube si vous envisagez d'utiliser une vue de source de données existante. Avec cette méthode, vous spécifiez la vue de source de données et sélectionnez les tables de faits et de dimension à utiliser dans la vue de source de données. Vous choisissez ensuite les dimensions et les mesures à inclure dans le cube.  
  
 Pour créer un cube avec une source de données, dans l’Explorateur de solutions, cliquez avec le bouton droit sur **Cubes** et sélectionnez **Nouveau cube**. L'Assistant Cube s'ouvre.  
  
## <a name="selecting-the-build-method"></a>Sélection de la méthode de construction  
 Sur la page **Sélectionner la méthode de construction** de l'Assistant, cliquez sur **Construire le cube en utilisant une source de données**.  
  
 Si vous activez la case à cocher **Génération automatique** , l'Assistant analyse la vue de source de données pour configurer le cube et ses dimensions à votre place. L'Assistant identifie les tables de faits et de dimension, sélectionne les mesures à inclure dans le cube et génère des hiérarchies. Sur chaque page de l'Assistant, vous pouvez examiner et modifier les choix effectués par l'Assistant lors de la sélection de l'option **Génération automatique** . Si vous ne sélectionnez pas **Génération automatique**, vous effectuez tous ces choix manuellement.  
  
 Si vous sélectionnez **Génération automatique**, vous pouvez cliquer sur **Terminer** sur n'importe quelle page de l'Assistant afin d'accéder à la dernière page et accepter les configurations par défaut pour toutes les pages restantes de l'Assistant. Sur la dernière page de l'Assistant, vous pouvez revoir la structure du cube avant de mettre fin à l'Assistant.  
  
 Si vous ne sélectionnez pas **Génération automatique**, vous devez sélectionner les tables de faits et de dimension vous-même. L'Assistant génère toutes les dimensions que vous choisissez de créer, mais vous devez utiliser le concepteur de dimensions pour générer manuellement des hiérarchies définies par l'utilisateur dans les dimensions. Cette condition peut être sans importance si vous avez déjà créé les dimensions que vous souhaitez utiliser dans le cube avant d'exécuter l'Assistant Cube.  
  
## <a name="selecting-the-data-source-view"></a>Sélection de la vue de source de données  
 Si vous utilisez une source de données existante pour créer un cube, la première étape consiste à spécifier la vue de source de données sur laquelle baser le cube. Sur la page **Sélectionner une vue de source de données** de l'Assistant, sélectionnez une vue de source de données existante. Dans le volet de visualisation, vous pouvez afficher les tables d'une vue de source de données sélectionnée. Pour afficher le schéma d'une vue de source de données sélectionnée, cliquez sur **Parcourir**.  
  
 Si la vue de source de données que vous souhaitez utiliser n'est pas répertoriée, dans l'Assistant Cube, cliquez sur **Annuler**et ouvrez l'Assistant Vue de source de données. Vous pouvez également cliquer sur **Ajouter un nouvel élément** dans le menu **Fichier** pour ajouter une vue de source de données existante d’une autre base de données (ou d’un autre emplacement). Pour plus d’informations sur la création de vues de source de données, consultez [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md).  
  
> [!NOTE]  
>  Une vue de source de données doit contenir au moins une table à répertorier dans cette page. Vous ne pouvez pas créer un cube basé sur une vue de source de données qui ne comporte aucune table.  
  
## <a name="identify-fact-and-dimension-tables"></a>Identifier les tables de faits et de dimension  
 Dans l'Assistant Cube, utilisez la page **Identifier les tables de faits et de dimension** de l'Assistant pour sélectionner les tables de faits et de dimension requises pour créer le cube. Si vous avez sélectionné la case à cocher **Génération automatique** pour créer le cube, les tables de faits et de dimension que l'Assistant détecte sont sélectionnées lorsque cette page s'affiche pour la première fois. Si l'Assistant détecte une table qui est à la fois une table de faits et une table de dimension, les deux colonnes sont sélectionnées. Si l'Assistant détecte une table qui n'est ni l'une ni l'autre, aucune colonne n'est sélectionnée. Si vous n'avez pas besoin de table pour la conception de cube, désactivez les cases à cocher **Fait** et **Dimension** .  
  
 Si vous n'avez pas sélectionné la case à cocher **Génération automatique** , vous devez effectuer toutes les sélections manuellement. Utilisez l'onglet **Tables** ou l'onglet **Diagramme** :  
  
-   L'onglet **Tables** répertorie les tables au format tabulaire. Activez la case à cocher dans la colonne **Fait** ou la colonne **Diagramme** .  
  
-   L'onglet **Diagramme** affiche le schéma de vue de source de données. Les tables sont codées par couleur pour indiquer un fait ou une dimension. Cliquez sur une table dans le schéma, puis sur **Fait** ou sur **Dimension** pour activer ou désactiver le paramètre sur cette table. Utilisez le bouton **Zoom** pour modifier le grossissement.  
  
> [!NOTE]  
>  Sous l’onglet **Diagramme** , vous pouvez agrandir ou maximiser la fenêtre de l’Assistant pour visualiser le schéma.  
  
 S'il existe une table de dimension de temps dans la vue de source de données, sélectionnez-la dans la liste **Table de dimension de temps** . S’il en existe aucun, laissez  **\<aucun >** sélectionné. Il s'agit de l'élément par défaut dans la liste. La sélection d'une table comme table de dimension de temps sélectionne également cette dernière comme table de dimension sous les onglets **Tables** et **Diagramme** .  
  
## <a name="defining-time-periods"></a>Définition de périodes  
 Si vous spécifiez une table de dimension de temps tout en sélectionnant des types de tables, utilisez la page **Définir des périodes** de l'Assistant pour spécifier les colonnes de la table qui correspondent aux périodes standard. Recherchez les périodes standard sous **Nom de la propriété de temps**. Pour chaque ligne qui a une colonne correspondante dans la table de dimension de temps, choisissez la colonne correcte sous **Colonnes de la table de temps**. L'Assistant utilise les associations que vous spécifiez pour créer des attributs et pour proposer les hiérarchies de temps qui se justifient pour vos données. Ces associations définissent également la propriété **Type** pour les attributs correspondant dans la nouvelle dimension de temps. L'Assistant crée ensuite une dimension de temps selon une table de dimension de temps.  
  
 Après avoir créé le cube, vous pouvez utiliser l'Assistant Business Intelligence pour apporter au cube des améliorations de décisionnel au niveau du temps. Ces améliorations sont de type période-à-date, moyenne mobile et période-à-période.  
  
## <a name="selecting-dimensions"></a>Sélection de dimensions  
 Utilisez la page **Sélectionner des dimensions** de l'Assistant pour ajouter les dimensions existantes au cube. Cette page s'affiche uniquement s'il existe déjà des dimensions partagées qui correspondent aux tables de dimension dans le nouveau cube.  
  
 Pour ajouter des dimensions existantes, sélectionnez une ou plusieurs dimensions dans la liste **Dimensions partagées** et cliquez sur le bouton fléché droit (**>**) pour les déplacer dans la liste **Dimensions du cube** . Cliquez sur le bouton à deux flèches (**>>**) pour déplacer toutes les dimensions dans la liste.  
  
 Si une dimension existante n'apparaît pas dans la liste et que vous pensez qu'elle le devrait, vous pouvez cliquer sur **Précédent** et modifier les paramètres de type de table pour une ou plusieurs tables. Une dimension existante doit également être liée à au moins une des tables de faits du cube pour apparaître dans la liste des **dimensions partagées** .  
  
## <a name="selecting-measures"></a>Sélection de mesures  
 Utilisez la page **Sélectionner les mesures** de l'Assistant pour sélectionner les mesures à inclure dans le cube. Chaque table marquée comme table de faits apparaît en tant que groupe de mesures dans cette liste, et chaque colonne non-clé numérique apparaît comme mesure dans la liste. Par défaut, chaque mesure de chaque groupe de mesures est sélectionnée. Vous pouvez désactiver la case à cocher en regard des mesures que vous ne souhaitez pas inclure dans le cube. Pour supprimer toutes les mesures d’un groupe de mesures du cube, décochez la case **Groupes de mesures/Mesures** .  
  
 Les noms des mesures répertoriées sous **Groupes de mesures/Mesures** reflètent les noms de colonnes. Vous pouvez cliquer sur la cellule qui contient un nom pour modifier ce dernier.  
  
 Pour afficher les données pour une mesure, cliquez avec le bouton droit sur l’une des lignes de mesure dans la liste, puis cliquez sur **Afficher des données d’exemple**. Ce faisant, vous ouvrez la **Visionneuse des données d'exemple** qui affiche les 1 000 premiers enregistrements de la table de faits correspondante.  
  
## <a name="reviewing-new-dimensions"></a>Examen de nouvelles dimensions  
 Utilisez la page **Vérifier les nouvelles dimensions** de l'Assistant pour examiner les structures de toutes les dimensions créées par l'Assistant. Les dimensions sont répertoriées dans cette page de l'Assistant dans l'arborescence **Nouvelles dimensions** . Vous pouvez examiner les dimensions des manières suivantes :  
  
-   Développez toute dimension pour afficher ses attributs et hiérarchies.  
  
-   Développez le dossier **Attributs** sous toute dimension pour laquelle vous souhaitez consulter les attributs.  
  
-   Développez le dossier **Hiérarchie** sous toute dimension pour laquelle vous voulez consulter les hiérarchies.  
  
-   Développez une hiérarchie pour afficher ses niveaux.  
  
> [!NOTE]  
>  Vous pouvez augmenter ou optimiser la fenêtre de l'Assistant pour afficher l'arborescence de manière plus appropriée.  
  
 Pour supprimer un objet dans l'arborescence du cube, désactivez la case à cocher en regard de celle-ci. Désactiver la case à cocher en regard d'un objet supprime également tous les objets situés sous celui-ci. Les dépendances entre les objets sont appliquées. Par conséquent, si vous supprimez un attribut, tous les niveaux de la hiérarchie dépendants de l'attribut sont également supprimés. Par exemple, la désactivation d'une case à cocher en regard d'une hiérarchie efface les cases à cocher en regard de tous les niveaux de la hiérarchie et supprime les niveaux ainsi que les hiérarchies. Il n'est pas possible de supprimer l'attribut clé d'une dimension.  
  
 Vous pouvez renommer une dimension, attribut, hiérarchie ou un niveau soit en cliquant sur le nom ou en cliquant sur le nom, puis sur le menu contextuel en cliquant sur **renommer \<objet >**, où  **\<objet >** est **Dimension**, **attribut**, ou **niveau**.  
  
 Il n’existe pas nécessairement une relation un-à-un entre le nombre de tables de dimension définies dans la page **Identifier les tables de faits et de dimension** de l’Assistant et le nombre de dimensions répertoriées dans cette page de l’Assistant. Selon les relations entre les tables dans la vue de source de données, l'Assistant peut utiliser deux tables ou plus pour générer une dimension (comme requis, par exemple, par un schéma en flocons).  
  
## <a name="completing-the-cube-wizard"></a>Fin de l'Assistant Cube  
 Sur la page **Fin de l'Assistant** de l'Assistant, vous pouvez afficher les groupes de mesures, les mesures et les dimensions du nouveau cube. Dans la zone **Nom du cube** , entrez un nom pour le cube. Ensuite, si vous êtes satisfait du cube, cliquez sur **Terminer**. Cliquez sur **Précédent** pour revenir à toute page précédente de l'Assistant et apporter des modifications.  
  
  
