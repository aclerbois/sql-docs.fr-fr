---
title: Les widgets Insight permet d’analyser les serveurs et bases de données SQL opérations Studio (version préliminaire) | Documents Microsoft
description: Découvrez les widgets insight dans Studio des opérations SQL (version préliminaire).
ms.custom: tools|sos
ms.date: 11/15/2017
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: article"
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 77f34ceebb4f02c829b2df3efcae5e64c2eaa1bf
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="manage-servers-and-databases-with-insight-widgets-in-includename-sosincludesname-sos-shortmd"></a>Gérer les serveurs et bases de données avec les widgets d’un aperçu [!INCLUDE[name-sos](../includes/name-sos-short.md)]

Les widgets Insight prennent les requêtes Transact-SQL (T-SQL) vous permet de surveiller les serveurs et bases de données et les transforme en visualisations détaillées. 

Insights sont personnalisables des graphiques que vous ajoutez au serveur et des tableaux de bord d’analyse de la base de données. Afficher une vue d’ensemble des serveurs et des bases de données, puis Explorez plus de détails et lancer des actions de gestion que vous définissez. 

Vous pouvez générer impressionnant serveur et base de données de gestion des tableaux de bord similaire à l’exemple suivant :

![tableau de bord de base de données](media/insight-widgets/database-dashboard.png)


Pour lancer et commencer à créer différents types de widgets d’informations, consultez les didacticiels suivants :

- [Générer un widget insight personnalisé](tutorial-build-custom-insight-sql-server.md)
- *Activer les widgets intégrés insight*
   - [Activer la surveillance d’un aperçu des performances](tutorial-qds-sql-server.md)
   - [Activer l’analyse de l’utilisation d’espace table](tutorial-table-space-sql-server.md)


## <a name="sql-queries"></a>Requêtes SQL 

[!INCLUDE[name-sos](../includes/name-sos-short.md)] tente d’éviter d’introduire encore un autre utilisateur de langue ou nombre élevé de l’interface afin d’essayer d’utiliser autant que possible le T-SQL avec une configuration minimale JSON. Configuration des widgets d’insight avec T-SQL s’appuie sur le nombre d’innombrables des sources existantes de requêtes T-SQL utiles qui peuvent être activés dans les widgets instructif.

Les widgets Insight sont composées d’une ou deux requêtes T-SQL :
* *Requête de widget Insight* est obligatoire et est la requête qui retourne les données qui apparaissent dans le widget.
* *Requête de détails Insight* est nécessaire uniquement si vous créez une page de détails de l’analyse.

Une requête de widget insight définit un jeu de données qui affiche un nombre, un graphique ou un graphique. Requête de détails de l’aperçu est utilisée pour répertorier des informations de détail des informations pertinentes dans un format tabulaire dans le volet de détails de l’analyse. 

[!INCLUDE[name-sos](../includes/name-sos-short.md)] exécute des requêtes de widget insight et mappe le jeu de résultats de requête au jeu de données d’un graphique, puis effectue le rendu. Lorsque les utilisateurs ouvrent les détails d’une information, il exécute la requête de détails d’un aperçu et imprime le résultat dans un affichage de grille dans la boîte de dialogue.

L’idée est d’écrire une requête T-SQL d’une manière afin qu’il peut être utilisé comme un jeu de données d’un nombre, le graphique et le widget de graphique. 

## <a name="summary"></a>Résumé

La requête T-SQL et son jeu de résultats déterminent le comportement du widget insight. Écriture d’une requête pour un type de graphique ou le mappage d’un type de graphique pour la requête existante est l’aspect clé pour générer un widget insight efficace.



## <a name="additional-resources"></a>Ressources supplémentaires
- [Éditeur de requête](tutorial-sql-editor.md)

