---
title: 'Didacticiel : Activer la table d’utilisation exemple insight le widget d’espace dans les opérations de SQL Studio (version préliminaire) | Documents Microsoft'
description: Ce didacticiel montre comment activer la table d’utilisation exemple insight le widget d’espace sur le tableau de bord de base de données SQL opérations Studio (version préliminaire).
ms.custom: tools|sos
ms.date: 03/19/2018
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 3a09dd273bb005432eaf638bb88c55ff35a5bd90
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="tutorial-enable-the-table-space-usage-sample-insight-widget-using-includename-sosincludesname-sos-shortmd"></a>Didacticiel : Activer la table d’espace d’utilisation exemple insight widget à l’aide [!INCLUDE[name-sos](../includes/name-sos-short.md)]

Ce didacticiel montre comment activer un widget d’un aperçu du tableau de bord de base de données, en fournissant une vue en un coup de œil sur l’utilisation de l’espace pour toutes les tables dans une base de données. Au cours de ce didacticiel, vous apprendrez comment :

> [!div class="checklist"]
> * Activer rapidement un widget d’un aperçu à l’aide d’un exemple de widget insight intégrés
> * Afficher les détails d’utilisation d’espace de table
> * Filtrer les données et afficher les détails de l’étiquette sur un graphique d’analyse

## <a name="prerequisites"></a>Configuration requise

Ce didacticiel nécessite SQL Server ou la base de données SQL Azure *TutorialDB*. Pour créer le *TutorialDB* de base de données, effectuez l’une des Démarrages rapides suivants :

- [Se connecter et interroger à l’aide de SQL Server [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
- [Se connecter et interroger à l’aide de la base de données SQL Azure [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)


## <a name="turn-on-a-management-insight-on-includename-sosincludesname-sos-shortmds-database-dashboard"></a>Activer une vision de gestion sur [!INCLUDE[name-sos](../includes/name-sos-short.md)]du tableau de bord de base de données
[!INCLUDE[name-sos](../includes/name-sos-short.md)] a un widget exemples intégrés pour surveiller l’espace utilisé par les tables dans une base de données.

1. Ouvrez *paramètres utilisateur* en appuyant sur **Ctrl + Maj + P** pour ouvrir le *Palette de commandes*.
2. Type *paramètres* dans la zone de recherche et sélectionnez **préférences : ouvrir les paramètres utilisateur**.
2. Type *tableau de bord* zone de recherche de paramètres d’entrée et recherchez **dashboard.database.widgets**.

3. Pour personnaliser le **dashboard.database.widgets** paramètres, vous devez modifier le **dashboard.database.widgets** entrée dans le **paramètres utilisateur** section (la colonne dans le à droite). S’il existe aucune **dashboard.database.widgets** dans les **paramètres utilisateur** section, placez le curseur sur le **dashboard.database.widgets** texte dans la colonne de paramètres par défaut et cliquez sur l’icône de crayon apparaît à gauche du texte et cliquez sur **copie paramètres**. Si la fenêtre contextuelle est **remplacer dans les paramètres**, ne cliquez pas ! Accédez à la **paramètres utilisateur** colonne située à droite et recherchez le **dashboard.database.widgets** section et passer à l’étape suivante.

4. Dans le **dashboard.database.widgets** section, ajoutez le code suivant :

   ```json
        {
            "name": "Space Used by Tables",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "table-space-db-insight": null
            }
        },
    ```
Le **dashboard.database.widgets** section doit ressembler à l’image suivante :

   ![Paramètres de recherche](./media/tutorial-table-space-sql-server/insight-table-space.png)

5. Appuyez sur **Ctrl + S** pour enregistrer les paramètres.

6. Tableau de bord de base de données ouverte en cliquant sur **TutorialDB** et cliquez sur **gérer**.

7. Afficher le *espace de table* widget insight comme indiqué dans l’image suivante : 

   ![Widget](./media/tutorial-table-space-sql-server/insight-table-space-result.png)


## <a name="working-with-the-insight-chart"></a>Travailler avec le graphique d’analyse

[!INCLUDE[name-sos](../includes/name-sos-short.md)]du graphique d’insight fournit des informations de filtrage et de pointage. Pour essayer les étapes suivantes :

1. Cliquez sur et activer/désactiver le *row_count* légende sur le graphique. [!INCLUDE[name-sos](../includes/name-sos-short.md)] Affiche ou masque les séries de données lorsque vous faites une légende ou désactiver.
    
2. Placez le pointeur de la souris sur le graphique. [!INCLUDE[name-sos](../includes/name-sos-short.md)] affiche plus d’informations sur l’étiquette de série de données et sa valeur comme indiqué dans la capture d’écran suivante.

   ![bascule du graphique et la légende](./media/tutorial-table-space-sql-server/insight-table-space-toggle.png)


## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez appris comment :
> [!div class="checklist"]
> * Activer rapidement un widget d’un aperçu à l’aide d’un exemple de widget insight intégrés.
> * Afficher les détails d’utilisation d’espace de table.
> * Filtrer les données et afficher les détails de l’étiquette sur un graphique d’analyse

Pour savoir comment générer un widget insight personnalisées, exécutez le didacticiel suivant :

> [!div class="nextstepaction"]
> [Générer un widget insight personnalisé](tutorial-build-custom-insight-sql-server.md).