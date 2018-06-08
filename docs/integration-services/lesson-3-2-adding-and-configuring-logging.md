---
title: 'Étape 2 : Ajout et configuration de la journalisation | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: 56105f3f-e500-4669-8c8e-acf434527727
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 28f6ab44d3ad27e106a19f6cc9c8f7ac6067af5b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-3-2---adding-and-configuring-logging"></a>Leçon 3-2 : Ajout et configuration de la journalisation
Dans cette tâche, vous allez activer la journalisation pour le flux de données dans le package Lesson 3.dtsx. Vous allez ensuite configurer un module fournisseur d'informations pour les fichiers texte, pour enregistrer les événements PipelineExecutionPlan et PipelineExecuteTrees. Le module fournisseur d'informations pour les fichiers texte crée des journaux faciles à créer et à déplacer. La simplicité de ces fichiers journaux les rend particulièrement utiles pendant la phase de test de base d'un package. Vous pouvez également consulter les entrées du journal dans la fenêtre Journaux d'événements du Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] .  
  
### <a name="to-add-logging-to-the-package"></a>Pour activer le mode d'écriture dans un journal pour le package  
  
1.  Dans le menu **SSIS** , cliquez sur **Enregistrement**.  
  
2.  Dans le volet **Conteneurs** de la boîte de dialogue **Configurer les journaux SSIS** , vérifiez si l’objet situé au niveau le plus élevé, et qui représente le package Lesson 3, est sélectionné.  
  
3.  Sous l’onglet **Fournisseurs et journaux** , dans la zone **Type de fournisseur** , sélectionnez **Module fournisseur d’informations SSIS pour les fichiers texte**, puis cliquez sur **Ajouter**.  
  
    Integration Services ajoute un nouveau module fournisseur d’informations pour les fichiers texte au package avec le nom par défaut : **Module fournisseur d’informations SSIS pour les fichiers texte**. Vous pouvez maintenant configurer le nouveau module fournisseur d'informations.  
  
4.  Dans la colonne **Nom** , tapez **Fichier Journal Leçon 3**.  
  
5.  Modifiez éventuellement les informations figurant dans **Description**.  
  
6.  Dans la colonne **Configuration** , cliquez sur **<New Connection>** pour spécifier la destination dans laquelle enregistrer les informations du journal.  
  
    Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers** , pour **Type d’utilisation**, sélectionnez **Créer un fichier**, puis cliquez sur **Parcourir**. Par défaut, la boîte de dialogue **Sélectionner un fichier** qui s’affiche présente le dossier du projet, mais il est possible d’enregistrer les informations de journal n’importe où ailleurs.  
  
7.  Dans la boîte de dialogue **Sélectionner un fichier** , dans la zone **Nom de fichier** , tapez **TutorialLog.log**, puis cliquez sur **Ouvrir**.  
  
8.  Cliquez sur **OK** pour fermer la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers** .  
  
9. Dans le volet **Conteneurs** , développez tous les nœuds de la hiérarchie des conteneurs du package, puis décochez toutes les cases, notamment la case **Extract Sample Currency Data** . Maintenant, cochez la case **Extract Sample Currency Data** pour extraire uniquement les événements de ce nœud.  
  
    > [!IMPORTANT]  
    > Si la case à cocher **Extract Sample Currency Data** n’apparaît pas sélectionnée mais grisée, la tâche utilise les paramètres du journal du conteneur parent et vous ne pouvez pas activer les journaux d’événements propres à cette tâche.  
  
10. Dans le volet **Détails** , dans la colonne **Événements** , sélectionnez les événements **PipelineExecutionPlan** et **PipelineExecutionTrees** .  
  
11. Cliquez sur **Avancés** pour vérifier les informations détaillées que le module de fournisseur d’informations enregistrera dans le journal pour chaque événement. Par défaut, toutes les catégories d'informations sont automatiquement sélectionnées pour les événements que vous avez spécifiés.  
  
12. Cliquez sur **De base** pour masquer les catégories d’informations.  
  
13. Sous l’onglet **Fournisseurs et journaux** , dans la colonne **Nom** , sélectionnez **Fichier Journal Leçon 3**. Une fois que vous avez créé un module fournisseur d'informations pour votre package, vous pouvez éventuellement le désélectionner temporairement pour désactiver la journalisation, sans avoir à supprimer puis à recréer un module fournisseur d'informations.  
  
14. Cliquez sur **OK**.  
  
## <a name="next-steps"></a>Next Steps  
[Étape 3 : Test de la leçon 3 du package du didacticiel](../integration-services/lesson-3-3-testing-the-lesson-3-tutorial-package.md)  
  
