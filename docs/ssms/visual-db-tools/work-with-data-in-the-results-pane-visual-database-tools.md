---
title: Utiliser des données du volet de résultats (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-visual-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- View Designer, Results pane
- queries [Visual Database Tools]
- result sets [SQL Server], queries
- Query Designer [SQL Server], Results pane
- results [SQL Server], query
- Visual Database Tools [SQL Server], queries
- queries [SQL Server], results
- Results pane
ms.assetid: 4f8a0080-91ef-4442-83ae-53be2f478c54
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5c45059cf8296e912853cac57d4e4da440ec03cc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="work-with-data-in-the-results-pane-visual-database-tools"></a>Utiliser des données du volet de résultats (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Après avoir exécuté une requête ou une vue, les résultats sont affichés dans le volet Résultats. Vous pouvez ensuite travailler avec ces résultats. Par exemple, vous pouvez ajouter ou supprimer des lignes, entrer ou modifier des données, et naviguer facilement parmi les vastes ensembles de résultats.  
  
Les informations suivantes peuvent vous aider à éviter certains problèmes et à utiliser efficacement ces résultats.  
  
## <a name="returning-the-results-set"></a>Retour de l'ensemble de résultats  
Vous pouvez retourner des résultats soit d'une requête, soit d'une vue, et vous pouvez choisir d'ouvrir juste le volet Résultats ou tous les volets. Dans l'un ou l'autre des cas, la requête ou la vue s'ouvre dans le Concepteur de requêtes et de vues. La différence réside dans le fait que l'une s'ouvre uniquement avec le volet Résultats, et l'autre s'ouvre avec toutes les fenêtres qui ont été sélectionnées dans la boîte de dialogue Options. La valeur par défaut est les quatre volets ouverts (Résultats, SQL, Schéma et Critères).  
  
Pour plus d’informations, consultez [Ouvrir des requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/open-queries-visual-database-tools.md).  
  
Pour modifier la conception de la requête ou de la vue pour qu'elle retourne un ensemble de résultats différent ou des enregistrements dans un ordre différent, consultez les rubriques répertoriées dans [Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md).  
  
Vous avez aussi la possibilité de retourner tout ou partie des ensembles de résultats de deux façons différentes : arrêter la requête pendant son exécution, ou choisir le nombre de résultats à retourner avant l'exécution de la requête.  
  
## <a name="navigating-in-the-results-pane"></a>Navigation dans le volet Résultats  
Vous pouvez naviguer rapidement au sein des enregistrements à l'aide de la barre de navigation située au bas du volet Résultats.  
  
Vous disposez de boutons permettant d'accéder aux premiers et aux derniers enregistrements, aux enregistrements suivants et précédents, ainsi qu'à un enregistrement particulier.  
  
Pour accéder à un enregistrement particulier, tapez le numéro de la ligne dans la zone de texte située dans la barre de navigation, puis appuyez sur ENTRÉE.  
  
Pour plus d’informations sur l’utilisation des raccourcis clavier dans le Concepteur de requêtes et de vues, consultez [Naviguer dans le Concepteur de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/navigate-in-the-query-and-view-designer-visual-database-tools.md).  
  
## <a name="committing-changes-to-the-database"></a>Validation des modifications à la base de données.  
Le volet Résultats utilise le contrôle concurrentiel optimiste pour que la grille affiche une copie des données dans la base de données plutôt qu'une vue directe. Ainsi, les modifications ne sont enregistrées dans la base de données qu'une fois que vous avez quitté une ligne. Cela permet à plusieurs utilisateurs de travailler en même temps sur la base de données. En cas de conflits (par exemple, si un autre utilisateur a modifié la ligne que vous aviez déjà modifiée et l'a validée dans la base de données avant que ne vous l'ayez fait), un message vous informe du conflit et vous propose des solutions pour les résoudre.  
  
## <a name="undo-changes-using-esc"></a>Annuler des modifications à l'aide de la touche Échap  
Vous ne pouvez annuler une modification que si elle n'a pas encore été validée dans la base de données. Les données ne sont pas enregistrées si vous n'avez pas quitté l'enregistrement ou si, une fois que vous l'avez quitté, vous recevez un message d'erreur indiquant que la modification ne sera pas validée. Si elle n'a pas été validée, vous pouvez annuler la modification à l'aide de la touche Échap.  
  
Pour annuler toutes les modifications d'une ligne, déplacez-vous dans une cellule de la ligne que vous n'avez pas modifiée et appuyez sur la touche Échap.  
  
Pour annuler des modifications sur une cellule spécifique que vous avez modifiée, déplacez-vous sur cette cellule et appuyez sur la touche Échap.  
  
## <a name="adding-or-deleting-data-in-the-database"></a>Ajout ou suppression de données dans la base de données  
Pour observer le fonctionnement de votre conception de base de données, vous devrez peut-être ajouter des données d'exemple à la base de données. Vous pouvez les entrer directement dans le volet Résultats ou les copier à partir d'un autre programme, tel que bloc-notes ou Excel, et le coller dans le volet Résultats.  
  
En plus de copier des lignes dans le volet Résultats, vous pouvez ajouter de nouveaux enregistrements ou bien en modifier ou supprimer. Pour plus d’informations, consultez [Ajouter des lignes dans le volet de résultats &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-new-rows-in-the-results-pane-visual-database-tools.md), [Supprimer des lignes dans le volet Résultats &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/delete-rows-in-the-results-pane-visual-database-tools.md) et [Modifier les lignes dans le volet Résultats &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/edit-rows-in-the-results-pane-visual-database-tools.md).  
  
## <a name="tips-for-working-with-null-values-and-empty-cells"></a>Conseils pour travailler avec les valeurs NULL et cellules vides  
Quand vous cliquez sur une ligne vide pour ajouter un nouvel enregistrement, la valeur initiale pour toutes les colonnes est *NULL*. Si une colonne autorise des valeurs Null, vous pouvez la laisser ainsi.  
  
Si vous souhaitez remplacer une valeur non NULL par NULL, tapez NULL en majuscules. Le volet Résultats affiche le mot en italique pour indiquer qu'il doit être reconnu comme une valeur Null plutôt que comme une chaîne.  
  
Pour taper la chaîne « null », saisissez les lettres sans guillemets. Tant qu'au moins l'une des lettres est en minuscule, la valeur est traitée comme une chaîne plutôt que comme une valeur Null.  
  
Les Valeurs pour les colonnes avec un type de données binary ont les valeurs NULL par défaut. Ces valeurs ne peuvent pas être modifiées dans le volet Résultats.  
  
Pour entrer un espace vide au lieu d'utiliser la valeur Null, supprimez le texte existant et quittez la cellule.  
  
## <a name="validating-data"></a>Validation des données  
Le Concept de requêtes et de vues peut valider certaines données par rapport aux propriétés des colonnes. Par exemple, si vous entrez « abc » dans une colonne avec un type de données float, vous recevez une erreur et la modification ne sera pas validée dans la base de données.  
  
La façon la plus rapide de consulter le type de données d'une colonne lorsque vous êtes dans le volet Résultats est d'ouvrir le volet Schéma et de pointer sur le nom de la colonne dans la table ou l'objet table.  
  
> [!NOTE]  
> La longueur maximale que le volet Résultats peut afficher pour un type de données texte est 2 147 483 647.  
  
## <a name="keeping-the-results-set-synchronized-with-the-query-definition"></a>Préserver la synchronisation du jeu de résultats avec la définition de la requête  
Lorsque vous travaillez sur les résultats d'une requête ou d'une vue, il est possible que les enregistrements du volet Résultats ne soient plus synchronisés avec la définition des requêtes. Par exemple, si vous avez exécuté une requête pour quatre des cinq colonnes dans une table, puis que vous avez utilisé le volet Schéma pour ajouter la cinquième colonne à la définition de la requête, les données de cette cinquième colonne ne sont pas automatiquement ajoutées au volet Résultats. Pour que le volet Résultats reflète la nouvelle définition de requête, exécutez à nouveau la requête.  
  
Si cela se produit, une icône d'alerte et le texte « Requête modifiée » apparaissent dans le coin inférieur droit du volet Résultats et l'icône s'affiche également dans le coin supérieur gauche du volet.  
  
## <a name="reconciling-changes-made-by-multiple-users"></a>Réconciliation des modifications effectuées par plusieurs utilisateurs  
Pendant que vous travaillez sur les résultats d'une requête ou d'une vue, les enregistrements peuvent être modifiés par un utilisateur différent de celui qui travaille également avec la base de données.  
  
Dans ce cas, vous recevez un avis de conflit dès que vous quittez la cellule. Vous pourrez alors remplacer la modification de l'autre utilisateur, mettre à jour votre volet Résultats avec la modification de l'autre utilisateur ou continuer à modifier votre volet Résultats sans réconcilier les différences. Si vous choisissez de ne pas réconcilier les différences, vos modifications ne seront pas validées dans la base de données.  
  
## <a name="limitations-in-the-results-pane"></a>Limitations dans le volet Résultats  
  
### <a name="what-can-not-be-updated"></a>Ce qui ne peut pas être mis à jour  
Ces conseils peuvent vous aider à utiliser efficacement les données du volet Résultats.  
  
-   Les requêtes qui incluent des colonnes de plus d'une table ou d'une vue ne peuvent pas être mises à jour.  
  
-   Les vues peuvent uniquement être mises à jour si les contraintes de la base de données l'autorisent.  
  
-   Les résultats retournés par une procédure stockée ne peuvent pas être mis à jour.  
  
-   Les requêtes ou vues qui utilisent les clauses GROUP BY, DISTINCT ou TO XML ne peuvent pas être mises à jour.  
  
-   Les résultats retournés par les fonctions table ne peuvent être mis à jour que dans certains cas.  
  
-   Les données dans les colonnes qui résultent d'une expression dans la requête.  
  
-   Les données qui n'ont pas été traduites avec succès par le fournisseur.  
  
### <a name="what-can-not-be-represented-fully"></a>Ce qui ne peut pas être représenté pleinement  
Ce qui est retourné au volet Résultats par la base de données est étroitement contrôlé par le fournisseur pour la source de données que vous utilisez. Le volet Résultats ne peut pas toujours traduire les données de tous les systèmes de gestion de base de données. Voici quelques exemples où c'est le cas.  
  
-   Les types de données binary sont souvent inutiles pour ceux qui travaillent dans le volet Résultats, et leur téléchargement peut être très long. Ils sont donc représentés par *<Binary data>* ou *Null*.  
  
-   Les valeurs Precision et Scale ne peuvent pas toujours être conservées. Par exemple, le volet Résultats prend en charge une précision de 27. Si les données sont d'un type de données de précision supérieure, elles peuvent être tronquées ou représentées par *<Unable to read data>*.  
  
## <a name="see-also"></a> Voir aussi  
[Effectuer des opérations de base concernant les requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
[Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
