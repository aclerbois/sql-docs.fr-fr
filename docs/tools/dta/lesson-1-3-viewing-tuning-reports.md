---
title: Affichage des rapports de paramétrage | Microsoft Docs
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
- tuning reports [SQL Server]
ms.assetid: daee6143-269f-428b-8458-9a3e726d586c
caps.latest.revision: 21
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: c65833c170fb8ef258fd97e5e6ad9bd9589ef621
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-3---viewing-tuning-reports"></a>Leçon 1-3 : Affichage des rapports de paramétrage
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Au cours de l'exercice précédent de cette leçon, vous avez affiché les scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] qui permettent de créer ou de supprimer des objets de base de données dans les recommandations de l'Assistant Paramétrage du moteur de base de données générées en résultat de la session de paramétrage MySession. La session de paramétrage MySession a été créée dans [Tuning a Workload](../../tools/dta/lesson-1-1-tuning-a-workload.md).  
  
Bien qu'il soit très utile d'afficher les scripts utilisables pour implémenter les résultats du paramétrage, l'Assistant Paramétrage du moteur de base de données fournit également de nombreux rapports utiles que vous pouvez afficher. Ces rapports fournissent des informations sur les structures de création physiques existantes dans la base de données que vous paramétrez et sur les structures recommandées. Pour afficher les rapports de paramétrage, cliquez sur l'onglet **Rapports** comme décrit dans l'exercice qui suit. Cet exercice utilise les sessions de paramétrage MySession et EvaluateMySession (ÉvaluerMaSession) que vous avez créées dans les sections [Tuning a Workload](../../tools/dta/lesson-1-1-tuning-a-workload.md) et [Viewing Tuning Recommendations](../../tools/dta/lesson-1-2-viewing-tuning-recommendations.md)du didacticiel.  
  
### <a name="view-tuning-reports"></a>Affichage des rapports de paramétrage  
  
1.  Démarrez l'Assistant Paramétrage du moteur de base de données. Consultez [Lancement de l’Assistant Paramétrage du moteur de base de données](../../tools/dta/lesson-1-1-launching-database-engine-tuning-advisor.md). Assurez-vous que vous êtes connecté à la même instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que celle utilisée au cours des exercices précédents de cette leçon.  
  
    Double-cliquez sur **MySession** dans le volet **Moniteur de session** . L'Assistant Paramétrage du moteur de base de données charge les informations de la session à partir de cette session.  
  
2.  Cliquez sur l'onglet **Rapports** .  
  
3.  Dans le volet **Résumé de paramétrage** , vous pouvez visualiser les informations relatives à cette session de paramétrage. Utilisez la barre de défilement pour visualiser tout le contenu du volet. Notez les informations des zones **Pourcentage d'amélioration attendu** et **Espace occupé par la recommandation**. Il est possible de limiter l'espace utilisé par la recommandation lorsque vous définissez les options de paramétrage. Sous l'onglet **Options de paramétrage** , sélectionnez **Options avancées**. Activez **Définir une quantité d'espace max. pour les recommandations** , et spécifiez, en mégaoctets, l'espace maximal qu'une configuration recommandée peut utiliser. Utilisez le bouton **Précédent** dans l'aide de votre navigateur pour revenir au didacticiel.  
  
4.  Dans le volet **Rapports de paramétrage** , sélectionnez **Rapport de coût d'instruction** dans la liste **Sélectionnez un rapport** . Si vous souhaitez disposer de davantage d'espace pour afficher le rapport, faites glisser le bord du volet **Moniteur de session** vers la gauche. Chaque instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui s'exécute sur une table de votre base de données est associée à un coût de performance. Ce coût de performance peut être réduit par la création d'index efficaces sur les colonnes qui font souvent l'objet d'accès dans une table. Ce rapport montre le pourcentage d'amélioration estimé entre le coût de départ pour l'exécution d'une instruction dans la charge de travail et le coût si la recommandation de paramétrage est appliquée. Notez que la quantité d'informations contenues dans le rapport est fonction de la longueur et de la complexité de la charge de travail.  
  
5.  Cliquez avec le bouton droit sur le volet **Rapport de coût d’instruction** dans la grille et cliquez sur **Exporter vers le fichier**. Enregistrez le rapport sous **MyReport**. Une extension .xml est ajoutée automatiquement au nom du fichier. Vous pouvez ouvrir le fichier MyReport.xml dans votre éditeur XML favoris ou dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour afficher le contenu du rapport.  
  
6.  Revenez à l’onglet **Rapports** de l’Assistant Paramétrage du moteur de base de données et recliquez avec le bouton droit sur **Rapport de coût d’instruction** . Passez en revue les autres options disponibles. Notez que vous pouvez modifier la police du rapport affiché. Si vous modifiez la police ici, la police est également modifiée dans les autres pages à onglet.  
  
7.  Sélectionnez d'autres rapports dans la liste **Sélectionnez un rapport** pour vous familiariser avec eux.  
  
## <a name="summary"></a>Résumé  
Vous avez parcouru l'onglet **Rapports** de l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données pour la session de paramétrage MySession. Vous pouvez suivre ces mêmes étapes pour parcourir les rapports générés pour la session de paramétrage EvaluateMySession (ÉvaluerMaSession). Double-cliquez sur **EvaluateMySession** dans le volet **Moniteur de session** pour commencer.  
  
## <a name="next-lesson"></a>Leçon suivante  
[Leçon 3 : Utilisation de l'utilitaire de ligne de commande dta](../../tools/dta/lesson-3-using-the-dta-command-prompt-utility.md)  
  
  
  
