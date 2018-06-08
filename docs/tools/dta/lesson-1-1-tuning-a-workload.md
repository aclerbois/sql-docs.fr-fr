---
title: Paramétrer une charge de travail | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
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
- workloads [SQL Server], tuning
ms.assetid: 6229bf3f-1182-4bc6-8451-cedc37f4b62e
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: cb21de806444d238c3a2b7d16caa2b6bd709b99a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-1---tuning-a-workload"></a>Leçon 1-1 : Paramétrage d’une charge de travail
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
L'Assistant Paramétrage du moteur de base de données peut servir à trouver la conception de base de données physique qui permet d'obtenir les meilleures performances des requêtes sur les bases de données et les tables que vous avez sélectionnées pour le paramétrage.  
  
Cette tâche utilise l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Pour des raisons de sécurité, les exemples de bases de données ne sont pas installés par défaut. Pour installer les exemples de bases de données, consultez [Installation des exemples SQL Server et des exemples de bases de données](http://sqlserversamples.codeplex.com).  
  
### <a name="tune-a-workload-transact-sql-script-file"></a>Pour paramétrer le fichier de script Transact-SQL d'une charge de travail  
  
1.  Copiez une ou plusieurs instructions SELECT exemple à partir de « A. Utilisation de SELECT pour extraire des lignes et des colonnes » dans [SELECT Examples &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md), puis collez les instructions dans l’Éditeur de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Enregistrez le fichier sous le nom **MyScript.sql** dans un répertoire où il est facile de le retrouver.  
  
2.  Démarrez l'Assistant Paramétrage du moteur de base de données. Voir [Lancement de l’Assistant Paramétrage du moteur de base de données](../../tools/dta/lesson-1-1-launching-database-engine-tuning-advisor.md).  
  
3.  Dans le volet droit de l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données, tapez **MySession** dans la zone **Nom de session**.  
  
4.  Sélectionnez **Fichier** pour votre **Charge de travail**et cliquez sur **Rechercher un fichier de charge de travail** pour localiser le fichier **MyScript.sql** que vous avez enregistré à l’étape 1.  
  
5.  Sélectionnez [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] dans la liste **Base de données pour l’analyse de la charge de travail** , sélectionnez [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] dans la grille **Sélectionnez les bases de données et tables à analyser** et laissez la case **Enregistrer le journal de paramétrage** cochée. **Base de données pour l’analyse de la charge de travail** spécifie la première base de données à laquelle l’Assistant Paramétrage du moteur de base de données se connecte lors du paramétrage d’une charge de travail. Dès que le paramétrage commence, l'Assistant Paramétrage du moteur de base de données se connecte aux bases de données spécifiées par les instructions `USE DATABASE` contenues dans la charge de travail.  
  
6.  Cliquez sur l’onglet **Options de paramétrage** . Dans le cadre de cet exercice, il ne vous est pas demandé de définir des options de paramétrage, mais prenez un moment pour passer en revue les options de paramétrage par défaut. Appuyez sur F1 pour afficher l'aide de cette page à onglets. Cliquez sur **Options avancées** pour afficher des options de paramétrage supplémentaires. Cliquez sur **Aide** dans la boîte de dialogue **Options de paramétrage avancées** pour obtenir des informations sur les options de paramétrage affichées ici. Cliquez sur **Annuler** pour fermer la boîte de dialogue **Options de paramétrage avancées** sans désactiver les options sélectionnées par défaut.  
  
7.  Dans la barre d'outils, cliquez sur le bouton **Démarrer l'analyse** . Pendant que l’Assistant Paramétrage du moteur de base de données analyse la charge de travail, vous pouvez contrôler l’état de l’analyse sous l’onglet **Progression** . Quand le paramétrage est terminé, l’onglet **Recommandations** s’affiche.  
  
    Si vous recevez une erreur relative à l’heure et à la date d’arrêt du paramétrage, vérifiez l’heure précisée pour l’option **Arrêter à** sous l’onglet **Options de paramétrage** principal. Vérifiez que l’heure et la date spécifiées dans **Arrêter à** sont ultérieures à la date et à l’heure actuelles et, si besoin est, modifiez-les.  
  
8.  Une fois l’analyse terminée, enregistrez votre recommandation sous la forme d’un script [!INCLUDE[tsql](../../includes/tsql-md.md)] en cliquant sur **Enregistrer les recommandations** dans le menu **Actions** . Dans la boîte de dialogue **Enregistrer sous** , accédez au répertoire dans lequel vous souhaitez enregistrer le script de recommandations et tapez le nom de fichier **MyRecommendations**.  
  
## <a name="summary"></a>Résumé  
Vous avez correctement paramétré une charge de travail d'instruction SELECT simple sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . L'Assistant Paramétrage du moteur de base de données peut également utiliser les fichiers et les tables de trace [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] comme charges de travail de paramétrage. Au cours de la tâche suivante, vous allez afficher et interpréter les recommandations de paramétrage que vous avez reçues en résultat de l'exercice de paramétrage.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Affichage des recommandations pour le paramétrage](../../tools/dta/lesson-1-2-viewing-tuning-recommendations.md)  
  
  
  
