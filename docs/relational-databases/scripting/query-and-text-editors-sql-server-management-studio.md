---
title: Éditeurs de texte et de requête (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- VS.TextEditor
helpviewer_keywords:
- Query Editor [SQL Server Management Studio]
- Code Editor [SQL Server Management Studio], about Query Editor
- Query Editor [SQL Server Management Studio], full screen mode
- Query Editor [Database Engine], templates
- full screen mode [SQL Server Management Studio]
- SQL Server Management Studio [SQL Server], templates
- writing scripts
- modifying scripts
- SQL Server Management Studio [SQL Server], query editor
- Query Editor [SQL Server Management Studio], about Query Editor
- writing queries
- SQL Server Management Studio [SQL Server], editor
- scripts [SQL Server], SQL Server Management Studio
- queries [SQL Server], SQL Server Management Studio
ms.assetid: 062051e4-4b77-4969-98ae-d2547c24ce3e
caps.latest.revision: 53
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: db76bac6cdbdf863d85895f4743c954eff2fef3e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="query-and-text-editors-sql-server-management-studio"></a>Éditeurs de texte et de requête (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Vous pouvez utiliser l'un des éditeurs [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour modifier et tester de manière interactive un script [!INCLUDE[tsql](../../includes/tsql-md.md)], MDX, DMX ou XML/A, ou pour modifier un fichier texte brut ou XML. Chaque éditeur est pris en charge par un service propre à un langage qui met en couleurs les mots clés et qui vérifie la syntaxe et les erreurs d'utilisation. L'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprend un débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] que vous pouvez utiliser pour résoudre les problèmes présents dans le code [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
## <a name="sql-server-management-studio-editors"></a>Éditeurs SQL Server Management Studio  
 Les quatre éditeurs dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] partagent une architecture commune. L'éditeur de texte implémente le niveau de base des fonctionnalités, et peut être utilisé comme éditeur de base pour les fichiers texte. Les trois autres éditeurs, ou éditeurs de requête, étendent cette base de fonctionnalité en incluant un service de langage qui définit la syntaxe d'un des langages pris en charge dans SQL Server. Les éditeurs de requête implémentent également différents niveaux de prise en charge des fonctionnalités de l'éditeur comme IntelliSense et le débogage. Les éditeurs de requête incluent l'éditeur de requête du moteur de base de données à utiliser dans l'élaboration de scripts contenant des instructions Transact-SQL et XQuery, l'éditeur MDX pour le langage MDX, l'éditeur DMX pour le langage DMX et l'éditeur XML/A pour le langage XML for Analysis.  
  
## <a name="common-components"></a>Composants communs  
 Tous les éditeurs dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] partagent les composants suivants :  
  
 **Volet Code**  
 Zone où vous entrez les requêtes ou le texte. Dans les éditeurs de requête, elle contient les fonctionnalités du Générateur d'instructions, qui sont disponibles pour le langage utilisé. L'environnement d'édition de texte prend en charge la fonction de recherche et remplacement, les commentaires en bloc, les polices et les couleurs personnalisées.  
  
 Vous pouvez définir des options qui ont une incidence sur le comportement du texte dans le volet Code en matière de mise en retrait, de tabulation, de glissement et déplacement du texte, etc. Les fenêtres de requête peuvent être configurées pour s'exécuter comme onglets dans la fenêtre de document, ou dans des documents distincts.  
  
 **Marge de sélection**  
 Colonne d'espacement entre la barre des indicateurs en marge et le texte du code, où vous pouvez cliquer pour sélectionner des lignes de texte. Vous pouvez masquer ou afficher la marge de sélection.  
  
 **Barres de défilement horizontales et verticales**  
 Ces barres vous permettent de faire défiler le volet Code horizontalement et verticalement, de sorte que vous pouvez afficher le code qui s'étend au-delà des extrémités visibles du volet.  
  
 **Numérotation des lignes**  
 Affiche le numéro des lignes à gauche du texte ou du code dans l'Éditeur. Vous pouvez accéder à des numéros de ligne spécifiques.  
  
 **Retour automatique à la ligne**  
 Affiche les longues lignes de texte ou de code sous forme de plusieurs lignes afin que vous puissiez voir tout le texte sur une ligne. Le retour automatique à la ligne n'a aucune incidence sur l'apparence du texte lorsqu'il est exécuté ou imprimé. Le retour automatique à la ligne est activé dans la boîte de dialogue **Outils**, **Options** de l'Éditeur de texte, de Tous les langages, de la page Général ou d'une page spécifique de l'Éditeur.  
  
## <a name="code-editor-components"></a>Composants de l'éditeur de code  
 Les éditeurs de code contiennent les fonctionnalités suivantes en plus de celles partagées avec les éditeurs de texte et XML :  
  
 **Résultats**  
 Cette fenêtre permet d'afficher les résultats d'une requête. La fenêtre peut afficher les résultats dans une grille ou du texte, ou les résultats peuvent être dirigés vers un fichier. Les grilles de résultats peuvent être affichées comme des fenêtres avec onglets distinctes.  
  
 **IntelliSense**  
 Dans les éditeurs, dans le menu **Edition** , pointez sur **IntelliSense**pour consulter les options [!INCLUDE[msCoName](../../includes/msconame-md.md)] IntelliSense.  
  
 **codage en couleurs ;**  
 Affiche différentes couleurs pour chaque type d'élément syntaxique, qui améliore la lisibilité des instructions complexes.  
  
 **Mise en relief du code**  
 Affiche les groupes de code avec des lignes de mise en relief à gauche du code. Les groupes de code peuvent être développés ou réduits pour en faciliter la révision.  
  
 **Modèle**  
 Les modèles sont des fichiers qui comprennent la structure de base des instructions nécessaires à la création d'objets dans une base de données. Ils peuvent être utilisés pour accélérer la création de scripts.  
  
 **Messages**  
 Affiche des erreurs, des avertissements et des messages d'information retournés par le serveur lors de l'exécution d'un script. La liste de messages ne change pas tant que le script n'est pas réexécuté.  
  
 **Barre d'état**  
 Affiche des informations système associées à la fenêtre de l'éditeur de requête, par exemple l'instance à laquelle l'éditeur de requête est connecté.  
  
## <a name="database-engine-query-editor-components"></a>Composants de l'éditeur de requête du moteur de base de données  
 Ces composants sont uniquement disponibles dans l'éditeur de requête du moteur de base de données :  
  
 **Débogueur**  
 Vous permet de suspendre l'exécution du code au niveau d'instructions spécifiques. Vous pouvez ensuite examiner des données et des informations système pour vous aider à trouver les erreurs présentes dans le code.  
  
 **Liste d'erreurs**  
 Affiche les erreurs syntaxiques et sémantiques détectées par IntelliSense. La liste d'erreurs se met à jour de manière dynamique lors de la modification des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 **Plan d'exécution de requêtes graphique**  
 Affiche les étapes logiques établies dans le plan d'exécution d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 **Statistiques du client**  
 Affiche des informations regroupées par catégorie sur l'exécution de la requête. Lorsque l'option **Inclure les statistiques du client** est sélectionnée dans le menu **Requête** , une fenêtre **Statistiques du client** est affichée lors de l'exécution de la requête. Les statistiques provenant d'exécutions de requêtes successives sont répertoriées avec les valeurs moyennes. Sélectionnez **Réinitialiser les statistiques du client** dans le menu **Requête** pour réinitialiser la moyenne.  
  
 **Extraits de code**  
 Modèles que vous pouvez utiliser comme point de départ lors de l'ajout d'instructions dans l'éditeur de requête du moteur de base de données. Vous pouvez insérer les extraits de code prédéfinis fournis avec SQL Server ou ajouter vos propres extraits de code.  
  
 **Mode SQLCMD**  
 Exécute des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] qui incluent le jeu de commandes prises en charge par l'utilitaire sqlcmd. Pour plus d’informations, consultez [Rubriques de procédures liées à sqlcmd](http://msdn.microsoft.com/library/dd7a2d2b-6327-4d77-ac5a-580d36073ad4).  
  
## <a name="editor-tasks"></a>Tâches de l'éditeur  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Décrit comment afficher et utiliser les fonctionnalités de base dans l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|[Éditeur de requête du moteur de base de données &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/database-engine-query-editor-sql-server-management-studio.md)|  
|Décrit comment afficher et utiliser les fonctionnalités de base dans l'éditeur de requête MDX.|[Éditeur de requête MDX &#40;Analysis Services - Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/777f2c23-1c1c-4b72-9d19-48a4866551f8)|  
|Décrit comment afficher et utiliser les fonctionnalités de base dans l'éditeur de requête DMX.|[Éditeur de requête DMX &#40;Analysis Services – Exploration de données&#41;](http://msdn.microsoft.com/library/7ac877a1-0f29-46b9-9a51-73b02172bef1)|  
|Décrit comment afficher et utiliser les fonctionnalités de base dans l'éditeur XML/A.|[Éditeur XML &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/xml-editor-sql-server-management-studio.md)|  
|Décrit comment configurer les options des différents éditeurs, tels que la numérotation des lignes et les options IntelliSense.|[Configurer des éditeurs &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/configure-editors-sql-server-management-studio.md)|  
|Décrit les différentes façons d'ouvrir les éditeurs dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].|[Ouvrir un éditeur &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/open-an-editor-sql-server-management-studio.md)|  
|Décrit comment gérer le mode d'affichage, tel que le retour automatique à la ligne, le fractionnement d'une fenêtre, ou les onglets.|[Gérer l'Éditeur et le mode d'affichage](../../relational-databases/scripting/manage-the-editor-and-view-mode.md)|  
|Explique comment définir les options de mise en forme, telles que le texte masqué ou la mise en retrait.|[Gérer la mise en forme du code](../../relational-databases/scripting/manage-code-formatting.md)|  
|Décrit comment parcourir le texte dans une fenêtre d'éditeur à l'aide de fonctionnalités telles que la recherche incrémentielle ou Atteindre.|[Naviguer dans le code et le texte](../../relational-databases/scripting/navigate-code-and-text.md)|  
|Explique comment définir les options de codage en couleurs pour différentes classes de syntaxe, ce qui facilite la lecture des instructions complexes.|[Codage en couleurs dans les éditeurs de requête](../../relational-databases/scripting/color-coding-in-query-editors.md)|  
|Décrit comment utiliser la mise en relief du code pour masquer des parties de scripts complexes que vous n'utilisez pas actuellement.|[Mise en relief du code](../../relational-databases/scripting/code-outlining.md)|  
|Explique comment faire glisser le texte d'un emplacement dans un script et le placer à un nouvel emplacement.|[Glisser et déplacer du texte](../../relational-databases/scripting/drag-and-drop-text.md)|  
|Décrit comment effectuer une opération de recherche et de remplacement globale, par exemple lors de la modification des noms des colonnes.|[Recherche et remplacement](../../relational-databases/scripting/search-and-replace.md)|  
|Décrit comment définir des signets afin de rechercher plus facilement les segments de code importants.|[Gérer les signets](../../relational-databases/scripting/manage-bookmarks.md)|  
|Explique comment imprimer des scripts ou les résultats dans une fenêtre ou une grille.|[Imprimer le code et les résultats](../../relational-databases/scripting/print-code-and-results.md)|  
|Décrit comment utiliser les fonctionnalités sqlcmd dans l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|[Modifier des scripts SQLCMD à l'aide de l'Éditeur de requête](../../relational-databases/scripting/edit-sqlcmd-scripts-with-query-editor.md)|  
|Décrit comment utiliser des fonctionnalités IntelliSense telles que la saisie semi-automatique des noms d'objets à mesure que vous les tapez, ou la garantie que les points d'arrêt sont placés dans des emplacements valides.|[IntelliSense &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/intellisense-sql-server-management-studio.md)|  
|Décrit comment utiliser les extraits de code dans l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Les extraits de code sont des modèles pour des instructions ou des blocs couramment utilisés, et peuvent être personnalisés ou étendus pour inclure les extraits de code spécifiques au site.|[Extraits de code Transact-SQL](../../relational-databases/scripting/transact-sql-code-snippets.md)|  
|Décrit comment utiliser le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] pour parcourir le code et afficher les informations de débogage telles que les valeurs des variables et des paramètres.|[Débogueur Transact-SQL](../../relational-databases/scripting/transact-sql-debugger.md)|  
|Décrit comment définir des couleurs personnalisées pour différentes instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)]et faire en sorte que ces couleurs soient définies comme arrière-plan de la barre d'état dans des fenêtres de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|[Barre d’état &#40;éditeur de requête du moteur de base de données&#41;](../../relational-databases/scripting/status-bar-database-engine-query-editor.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Raccourcis clavier dans SQL Server Management Studio](../../tools/sql-server-management-studio/sql-server-management-studio-keyboard-shortcuts.md)  
  
  
