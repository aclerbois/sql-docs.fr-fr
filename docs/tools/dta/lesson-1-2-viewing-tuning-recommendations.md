---
title: Affichage des recommandations de paramétrage | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: dta
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: e4e690c9-434f-4b01-b4de-0b905323ddd6
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 9def661b232efcd07d3b2b9ac84e1daabcd596cf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-2---viewing-tuning-recommendations"></a>Leçon 1-2 : Affichage des recommandations pour le paramétrage
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Cette tâche est basée sur la session de paramétrage que vous avez créée dans [Paramétrage d’une charge de travail](../../tools/dta/lesson-1-1-tuning-a-workload.md). Une fois la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] paramétrée au moyen du script [!INCLUDE[tsql](../../includes/tsql-md.md)] MyScript.sql, l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] affiche ses résultats sous l’onglet **Recommendations** . La tâche suivante présente l’onglet **Recommendations** de l’interface graphique utilisateur de l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et vous guide pour explorer les informations que ce dernier fournit sur les résultats de la session de paramétrage.  
  
### <a name="view-tuning-recommendations"></a>Afficher les recommandations de paramétrage  
  
1.  Démarrez l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Consultez [Lancement de l’Assistant Paramétrage du moteur de base de données](../../tools/dta/lesson-1-1-launching-database-engine-tuning-advisor.md). Assurez-vous de vous connecter à la même instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que celle utilisée au cours de l’exercice [Paramétrage d’une charge de travail](../../tools/dta/lesson-1-1-tuning-a-workload.md).  
  
2.  Double-cliquez sur **MySession** dans le volet **Moniteur de session** . [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage du moteur de base de données charge les informations de session de votre session de paramétrage précédente et affiche l’onglet **Recommandations** . Notez que l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’a fourni aucune **Recommandation de partition** ; en effet, vous avez accepté toutes les valeurs par défaut des options de paramétrage et l’option **Aucun partitionnement** a été sélectionnée sous l’onglet **Options de paramétrage** .  
  
3.  Sous l’onglet **Recommandations** , utilisez la barre de défilement en bas de la page à onglet pour afficher toutes les colonnes **Recommandations d’index** . Chaque ligne représente un objet de base de données (index ou vues indexées) que l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] recommande de supprimer ou de créer. Faites défiler l’écran vers la colonne la plus à droite et sélectionnez une **Définition**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage du moteur de base de données affiche la fenêtre **Aperçu de script SQL** , dans laquelle vous pouvez voir le script [!INCLUDE[tsql](../../includes/tsql-md.md)] qui permet de créer ou de supprimer l’objet de base de données sur cette ligne. Cliquez sur **Terminer** pour fermer la fenêtre de prévisualisation.  
  
    Si vous avez du mal à trouver une **Définition** contenant un lien, décochez la case **Afficher les objets existants** en bas de la page à onglet pour diminuer le nombre de lignes affichées. Quand vous décochez cette case, l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] affiche uniquement les objets pour lesquels il a émis une recommandation. Cochez la case **Afficher les objets existants** pour visualiser tous les objets de base de données existant dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Utilisez la barre de défilement à droite de la page à onglet pour visualiser tous les objets.  
  
4.  Cliquez avec le bouton droit dans la grille du volet **Recommandations d’index** . Le menu contextuel qui s'affiche permet de sélectionner et de désélectionner des recommandations. Il permet également de modifier la police du texte de la grille.  
  
5.  Dans le menu **Actions** , cliquez sur **Enregistrer les recommandations** pour enregistrer toutes les recommandations dans un seul script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Attribuez à ce script le nom **MySessionRecommendations.sql**.  
  
    Ouvrez le script MySessionRecommendations.sql dans l'Éditeur de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour en afficher le contenu. Vous pouvez appliquer ces recommandations à la base de données exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en exécutant le script dans l'Éditeur de requête, mais ne le faites pas. Fermez le script dans l'Éditeur de requête sans l'exécuter.  
  
    L’autre méthode pour appliquer les recommandations consiste à cliquer sur **Appliquer les recommandations** dans le menu **Actions** de l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , mais n’appliquez pas ces recommandations maintenant.  
  
6.  Si plusieurs recommandations sont fournies sous l’onglet **Recommandations** , supprimez certaines lignes contenant des objets de base de données dans la grille **Recommandations d’index** .  
  
7.  Dans le menu **Actions** , cliquez sur **Évaluer les recommandations**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] L'Assistant Paramétrage du moteur de base de données crée une nouvelle session de paramétrage qui permet d'évaluer un sous-ensemble des recommandations d'origine de la session MySession.  
  
8.  Tapez **EvaluateMySession (ÉvaluerMaSession)** pour la session dont le nom figure dans **Nom de session**, puis cliquez sur le bouton **Démarrer l’analyse** de la barre d’outils. Vous pouvez répéter les étapes 2 et 3 pour cette nouvelle session de paramétrage afin d'en visualiser les recommandations.  
  
## <a name="summary"></a>Résumé  
Vous avez affiché le contenu de l’onglet **Recommandations** pour la session de paramétrage MySession et vous avez évalué un sous-ensemble de ces recommandations pour la nouvelle session de paramétrage EvaluateMySession.  
  
L'évaluation d'un sous-ensemble de recommandations de paramétrage peut s'avérer nécessaire si vous pensez devoir modifier des options de paramétrage après l'exécution d'une session. Cela peut être le cas, par exemple, si vous configurez l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour qu’il tienne compte des vues indexées quand vous spécifiez des options de paramétrage pour une session, mais décidez après la génération des recommandations de réutiliser les vues indexées. Vous pouvez utiliser ensuite l’option **Évaluer les recommandations** du menu **Actions** pour que l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] réévalue la session sans tenir compte des vues indexées. Quand vous utilisez l’option **Évaluer les recommandations** , les recommandations générées sont appliquées hypothétiquement à la structure de création physique courante pour parvenir à la structure de création physique de la deuxième session de paramétrage.  
  
Il est possible de visualiser davantage d’informations sur les résultats du paramétrage sous l’onglet **Rapports** , décrit dans la tâche suivante de la présente leçon.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Affichage des rapports de paramétrage](../../tools/dta/lesson-1-3-viewing-tuning-reports.md)  
  
  
  
