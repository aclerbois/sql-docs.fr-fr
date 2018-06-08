---
title: Effectuer une évaluation de la migration de SQL Server (Assistant Migration de données) | Documents Microsoft
ms.custom: ''
ms.date: 10/04/2017
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: 30c44a7aba2a721501996d7a1d53a6a4a83a345e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="perform-a-sql-server-migration-assessment"></a>Effectuer une évaluation de la migration de SQL Server
Les instructions pas à pas suivantes vous aider à effectuer votre première évaluation de la migration vers local SQL Server, SQL Server s’exécutant sur une machine virtuelle Azure, ou la base de données SQL Azure, à l’aide de l’Assistant Migration de données.

## <a name="create-an-assessment"></a>Créer une évaluation

1.  Sélectionnez le **nouveau** (+) icône, puis sélectionnez le **évaluation** type de projet.

2.  Définir le type de serveur source et cible.

    Si vous mettez à niveau votre instance de SQL Server locale à une instance de SQL Server moderne localement ou à SQL Server hébergée sur une machine virtuelle Azure, définissez le type de serveur source et cible **SQL Server**. Si vous effectuez une migration vers la base de données SQL Azure, à la place la valeur du type de serveur cible **base de données SQL Azure**.

3.  Cliquez sur **Créer**.

    ![Créer une évaluation](../dma/media/NewAssessment.png)

## <a name="choose-assessment-options"></a>Choisissez les options d’évaluation

1. Sélectionnez la version de SQL Server cible que vous envisagez de migrer vers.

2. Sélectionnez le type de rapport.

   Lorsque vous évaluez votre instance de SQL Server source pour la migration vers local SQL Server ou SQL Server hébergées sur les cibles de la machine virtuelle Azure, vous pouvez choisir une ou les deux types de rapport d’évaluation suivants :

    -   **Problèmes de compatibilité**

    -   **Recommandation des nouvelles fonctionnalités**

    ![Sélectionnez un type de rapport d’évaluation pour SQL Server cible](../dma/media/AssessmentTypes.png)

   Lorsque vous évaluez votre instance de SQL Server source pour la migration vers la base de données SQL Azure, vous pouvez choisir une ou les deux types de rapport d’évaluation suivants :

    -   **Vérifier la compatibilité de la base de données**

    -   **Vérifiez la parité des fonctionnalités**

    ![Sélectionnez le type de rapport d’évaluation pour la cible de la base de données SQL](../dma/media/AssessmentTypes_Azure.png)

## <a name="add-databases-to-assess"></a>Ajouter des bases de données à évaluer

1.  Sélectionnez **ajouter des Sources** pour ouvrir le menu contextuel de connexion.

2.  Entrez le nom d’instance SQL server, choisissez le type d’authentification, définir les propriétés de connexion correctes, puis sélectionnez **connexion**.

3.  Sélectionnez les bases de données à évaluer, puis sélectionnez **ajouter**.

    > [!NOTE] 
    > Vous pouvez supprimer plusieurs bases de données en les sélectionnant tout en maintenant la touche MAJ ou Ctrl, puis en cliquant sur **supprimer les Sources**. Vous pouvez également ajouter des bases de données à partir de plusieurs instances de SQL Server à l’aide de la **ajouter des Sources** bouton.

4.  Cliquez sur **suivant** pour démarrer l’évaluation.

    ![Ajouter des sources et de démarrer l’évaluation](../dma/media/SelectDatabase.png)

## <a name="view-results"></a>Afficher les résultats

La durée de l’évaluation varie selon le nombre de bases de données ajoutées et la taille du schéma de chaque base de données. Résultats sont affichés pour chaque base de données dès qu’elles sont disponibles.

1.  Sélectionnez la base de données a terminé l’évaluation et ensuite basculer entre les **des problèmes de compatibilité** et **fonctionnalité recommandations** à l’aide du sélecteur.

2.  Passez en revue les problèmes de compatibilité à tous les niveaux de compatibilité pris en charge par la version de SQL Server cible que vous avez sélectionné sur la **Options** page.

Vous pouvez examiner les problèmes de compatibilité en analysant l’objet affecté et ses détails pour chaque problème identifié sous **modifications avec rupture**, **changements de comportement**, et **fonctionnalités déconseillées** .

![Affichage des résultats d’évaluation](../dma/media/ReviewResults.png)

De même, vous pouvez consulter la recommandation de fonctionnalité entre **performances**, **stockage**, et **sécurité** zones.

Recommandations de la fonctionnalité couvrent un large éventail de fonctionnalités telles que l’OLTP et Columnstore, Stretch Database, Always Encrypted, masquage dynamique des données et chiffrement Transparent des données.

![Recommandations de fonctionnalité d’affichage](../dma/media/FeatureRecommendations.png)

Pour la base de données SQL Azure, les évaluations fournissent les problèmes de blocage de migration et les problèmes de parité de fonctionnalités. Passez en revue les résultats pour ces deux catégories en sélectionnant les options spécifiques.

- Le **parité de fonctionnalités SQL Server** catégorie fournit un ensemble complet de recommandations, les autres approches sont disponibles dans Azure et de mesures. Il vous permet de planifier ces efforts dans vos projets de migration.

  ![Afficher les informations de parité de fonctionnalités SQL Server](../dma/media/SQLFeatureParity.png)

- Le **des problèmes de compatibilité** catégorie fournit des fonctionnalités partiellement prises en charge ou non pris en charge qui bloquent la migration bases de données de SQL Server sur site pour les bases de données SQL Azure. Il fournit ensuite des recommandations pour vous aider à résoudre ces problèmes.

  ![Afficher les problèmes de compatibilité](../dma/media/CompatibilityIssues.png)

## <a name="export-results"></a>Exporter les résultats

Après l’évaluation de toutes les bases de données, sélectionnez **exporter le rapport** pour exporter les résultats dans un fichier JSON ou un fichier CSV. Vous pouvez ensuite analyser les données à votre convenance.

Vous pouvez exécuter plusieurs évaluations simultanément et afficher l’état de l’évaluation en ouvrant le **toutes les évaluations** page.
