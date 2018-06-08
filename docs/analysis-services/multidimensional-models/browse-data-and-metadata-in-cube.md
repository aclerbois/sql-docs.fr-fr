---
title: Parcourir les données et métadonnées de Cube | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7bd1940a21407375015e6b732ab129653bff01e6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="browse-data-and-metadata-in-cube"></a>Parcourir les données et métadonnées de cube
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Utilisez l'onglet **Navigateur** du Concepteur de cube pour parcourir les données du cube. Vous pouvez utiliser cette vue pour examiner la structure d'un cube et vérifier des données, le calcul, la mise en forme, ainsi que la sécurité des objets de base de données. Vous pouvez examiner rapidement un cube tel que les utilisateurs finaux le voient dans les outils de création de rapports ou d'autres applications clientes. Lorsque vous parcourez des données de cube, vous pouvez afficher différentes dimensions, explorer les membres et découper les dimensions.  
  
 Avant de parcourir un cube, vous devez le traiter et vous y reconnecter. Une fois le traitement terminé, ouvrez l'onglet **Navigateur** du Concepteur de cube. Cliquez sur le bouton Reconnecter dans la barre d'outils pour actualiser la connexion.  
  
 L'onglet **Navigateur** comporte trois volets, le volet des métadonnées, le volet de filtre et le volet de données. Utilisez le volet Métadonnées pour examiner la structure du cube dans le format d'arborescence. Utilisez le volet Filtre en haut de l'onglet **Navigateur** pour définir tout sous-cube auquel vous souhaitez accéder. Utilisez le volet Données pour afficher le jeu de résultats et explorer les hiérarchies de dimension.  
  
## <a name="setting-up-the-browser"></a>Configuration du navigateur  
 Pour préparer l'exploration d'un cube, vous pouvez spécifier une perspective ou une traduction à utiliser. Vous ajoutez des mesures et des dimensions au volet Données et spécifiez tous les filtres dans le volet Filtre.  
  
##### <a name="specifying-a-perspective"></a>Spécification d'une perspective  
 Utilisez la liste **Perspective** pour choisir une perspective qui est définie pour le cube. Les perspectives sont définies dans l'onglet **Perspectives** du Concepteur de cube. Pour basculer vers une perspective différente, sélectionnez-en une dans la liste.  
  
##### <a name="specifying-a-translation"></a>Spécification d'une traduction  
 Utilisez la liste **Langue** pour choisir une traduction définie pour le cube. Les traductions sont définies dans l'onglet **Traductions** du Concepteur de cube. L'onglet **Navigateur** affiche initialement des légendes pour la langue par défaut, spécifiée par la propriété **Langue** pour le cube. Pour basculer vers une autre langue, sélectionnez celle de votre choix dans la liste.  
  
##### <a name="formatting-the-data-pane"></a>Mise en forme du volet Données  
 Vous pouvez mettre en forme des légendes et des cellules dans le volet Données. Cliquez avec le bouton droit sur les cellules sélectionnées ou les légendes que vous souhaitez mettre en forme, puis cliquez sur **Commandes et options**. En fonction de la sélection, la boîte de dialogue **Commandes et options** affiche les paramètres pour **Format**, **Filtre et groupe**, **Rapport**et **Comportement**.  
  
## <a name="setting-up-the-data"></a>Configuration des données  
  
##### <a name="adding-or-removing-measures"></a>Ajout ou suppression de mesures  
 Faites glisser les mesures auxquelles vous souhaitez accéder du volet Métadonnées vers la zone de détails du volet Données, qui est le volet vide en bas à droite de l'espace de travail. Lorsque vous faites glisser des mesures supplémentaires, elles sont ajoutées sous forme de colonnes. Une ligne verticale indique la destination de chaque mesure supplémentaire. Faire glisser le dossier **Mesures** supprime toutes les mesures dans la zone de détails.  
  
 Pour supprimer une mesure de la zone de détails, faites-la glisser hors du volet Données ou cliquez avec le bouton droit, puis cliquez sur **Supprimer** dans le menu contextuel.  
  
##### <a name="adding-or-removing-dimensions"></a>Ajout ou suppression de dimensions  
 Faites glisser les hiérarchies ou les dimensions vers les zones de ligne ou de filtre.  
  
 Si vous souhaitez utiliser plusieurs dimensions, utilisez Analyser dans Excel. Analyser dans Excel est un raccourci qui démarre Excel s'il est installé sur le même ordinateur que [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Excel ouvre un classeur contenant une connexion existante à la base de données active et une liste de champs de tableau croisé dynamique préchargée avec des mesures et des dimensions. Pour plus d’informations, consultez [Analyser dans Excel &#40;onglet Explorateur, Concepteur de cube&#41; &#40;Analysis Services - Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/890ed457-137e-44ac-9b2c-83344a1b8fc9).  
  
 Pour supprimer une dimension, faites-la glisser hors du volet Données ou cliquez avec le bouton droit, puis cliquez sur **Supprimer** dans le menu contextuel.  
  
##### <a name="adding-or-removing-filters"></a>Ajout ou suppression de filtres  
 Vous pouvez utiliser l'une des deux méthodes suivantes pour ajouter un filtre :  
  
-   Développez une dimension dans le volet Métadonnées, puis faites glisser une hiérarchie vers le volet Filtre.  
  
     \- ou -  
  
-   Dans le **Dimension** colonne de la **filtre** volet, cliquez sur  **\<sélectionner une dimension >** et sélectionnez une dimension dans la liste, puis cliquez sur  **\<sélectionnez hiérarchie >** dans les **hiérarchie** colonne et sélectionnez une hiérarchie dans la liste.  
  
 Après avoir spécifié la hiérarchie, spécifiez l'opérateur et l'expression de filtre. Le tableau suivant décrit les opérateurs et les expressions de filtre.  
  
|Opérateur|Expression de filtre|Description|  
|--------------|-----------------------|-----------------|  
|Égal à|Un ou plusieurs membres|Les valeurs doivent être égales à un membre spécifié.<br /><br /> (Fournit plusieurs sélections de membres pour les hiérarchies d'attribut, autres que les hiérarchies parent-enfant, et une seule sélection de membre pour d'autres hiérarchies.)|  
|Non égal|Un ou plusieurs membres|Les valeurs ne doivent pas être égales à un membre spécifié.<br /><br /> (Fournit plusieurs sélections de membres pour les hiérarchies d'attribut, autres que les hiérarchies parent-enfant, et une seule sélection de membre pour d'autres hiérarchies.)|  
|Dans|Un ou plusieurs jeux nommés|Les valeurs doivent se trouver dans un jeu nommé spécifié.<br /><br /> (Pris en charge pour les hiérarchies d'attribut uniquement.)|  
|Pas dans|Un ou plusieurs jeux nommés|Les valeurs ne doivent pas se trouver dans un jeu nommé spécifié.<br /><br /> (Pris en charge pour les hiérarchies d'attribut uniquement.)|  
|Plage (limites incluses)|Un ou deux membres de délimitation d'une plage|Les valeurs doivent se situer entre les membres de délimitation ou être égales à ceux-ci. Si les membres de délimitation sont égaux ou qu'un seul membre est spécifié, aucune plage n'est appliquée et toutes les valeurs sont autorisées.<br /><br /> (Pris en charge pour les hiérarchies d'attribut uniquement. La plage doit se situer sur un niveau d'une hiérarchie. Les plages illimitées ne sont pas prises en charge actuellement.)|  
|Plage (limites exclues)|Un ou deux membres de délimitation d'une plage|Les valeurs doivent se situer entre les membres de délimitation. Si les membres de délimitation sont égaux ou qu'un seul membre est spécifié, les valeurs doivent être supérieures ou inférieures au membre de délimitation.<br /><br /> (Pris en charge pour les hiérarchies d'attribut uniquement. La plage doit se situer sur un niveau d'une hiérarchie. Les plages illimitées ne sont pas prises en charge actuellement.)|  
|MDX|Expression MDX qui retourne un ensemble de membres|Les valeurs doivent se trouver dans le jeu de membres calculés. La sélection de cette option affiche la boîte de dialogue **Générateur de membres calculés** pour créer des expressions MDX.|  
  
 Pour les hiérarchies définies par l'utilisateur, dans lesquelles plusieurs membres peuvent être spécifiés dans l'expression de filtre, tous les membres spécifiés doivent être au même niveau et partager le même parent. Cette restriction ne s'applique pas aux hiérarchies de type parent-enfant.  
  
## <a name="working-with-data"></a>Utilisation des données  
  
##### <a name="drilling-down-into-a-member"></a>Exploration d'un membre  
 Pour explorer un membre spécifique, cliquez sur le signe plus (+) en regard du membre ou double-cliquez sur le membre.  
  
##### <a name="slicing-through-cube-dimensions"></a>Découpage des dimensions de cube  
 Pour filtrer les données de cube, cliquez sur la zone de liste déroulante sur le titre de colonne de niveau supérieur. Vous pouvez développer la hiérarchie et sélectionner ou effacer un membre à n'importe quel niveau afin de l'afficher ou de le masquer, ainsi que tous ses descendants. Décochez la case en regard de **(Tous)** pour effacer tous les membres de la hiérarchie.  
  
 Après avoir défini ce filtre sur les dimensions, vous pouvez l’activer ou le désactiver en cliquant avec le bouton droit dans le volet Données, puis en cliquant sur **Filtre automatique**.  
  
##### <a name="filtering-data"></a>Filtrage des données  
 Vous pouvez utiliser la zone de filtre pour définir un sous-cube à parcourir. Vous pouvez ajouter un filtre en cliquant sur une dimension dans le volet Filtre ou en développant une dimension dans le volet Métadonnées puis en faisant glisser une hiérarchie vers le volet Filtre. Spécifiez ensuite un **Opérateur** et une **Expression de filtre**.  
  
##### <a name="performing-actions"></a>Exécution des actions  
 Un triangle signale n'importe quel titre ou cellule du volet Données pour lequel il existe une action. Cela peut s'appliquer à une dimension, un niveau, un membre ou une cellule de cube. Déplacez le pointeur sur l'objet de titre pour obtenir une liste des actions disponibles. Cliquez sur le triangle dans la cellule pour afficher un menu et lancer le processus associé.  
  
 Pour des raisons de sécurité, l'onglet **Navigateur** prend uniquement en charge les actions suivantes :  
  
-   URL  
  
-   Ensemble de lignes  
  
-   Extraction  
  
 La ligne de commande, l'instruction, et les actions propriétaires ne sont pas prises en charge. Les actions d'URL sont aussi sécurisées que la page Web à laquelle elles sont liées.  
  
##### <a name="viewing-member-properties-and-cube-cell-information"></a>Affichage des propriétés de membre et des informations de cellule de cube  
 Pour afficher des informations sur un objet dimension ou une valeur de cellule, déplacez le pointeur sur la cellule.  
  
##### <a name="showing-or-hiding-empty-cells"></a>Affichage ou masquage de cellules vides  
 Vous pouvez masquer les cellules vides dans la grille de données en cliquant avec le bouton droit dans le volet Données, puis en cliquant sur **Afficher les cellules vides**.  
  
  
