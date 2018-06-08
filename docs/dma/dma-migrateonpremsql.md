---
title: Migration locale SQL Server (Assistant Migration de données) | Documents Microsoft
ms.custom: ''
ms.date: 09/01/2017
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: b535e41b93d337fc2cc1ae3f12699dd841bf91b9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-on-premises-sql-server-using-data-migration-assistant"></a>Migrer à l’aide de l’Assistant Migration de données de SQL Server sur site

Cet article fournit des instructions détaillées pour la migration de SQL Server à l’aide de l’Assistant Migration de données.

L’Assistant Migration de données fournit des évaluations transparentes et les migrations aux plateformes de données SQL Server et SQL Azure VM moderne localement.  

Effectuez les tâches suivantes pour effectuer la migration.

- [Créez un projet de migration](#create-a-new-migration-project)
- [Spécifiez la source et cible](#specify-source-and-target)
- [Ajouter des bases de données](#add-databases)
- [Sélectionnez les connexions](#select-logins)

## <a name="create-a-new-migration-project"></a>Créez un projet de migration

1. Cliquez sur **nouveau** (+) dans le volet gauche, sélectionnez le **Migration** type de projet.

1. Définir le type de serveur source et cible **SQL Server** si vous mettez à niveau un serveur de SQL Server locale à moderne local SQL Server.

1. Cliquez sur **Créer**.

   ![Créer le projet de migration](../dma/media/NewCreate.png)

## <a name="specify-the-source-and-target"></a>Spécifiez la source et cible

1. Pour la source, entrez le nom de l’instance SQL Server dans le **nom du serveur** champ dans le **détails du serveur de Source** section. 

1. Sélectionnez le **type d’authentification** pris en charge par l’instance de SQL Server source.

1. Pour la cible, entrez le nom de l’instance SQL Server dans le **nom du serveur** champ dans le **cibler les détails du serveur** section. 

1. Sélectionnez le **type d’authentification** pris en charge par l’instance de SQL Server cible.

1. Il est recommandé de chiffrer la connexion en sélectionnant **chiffrer la connexion** dans les **propriétés de connexion** section.

1. Cliquez sur **Suivant**.

   ![Spécifier la page source et cible](../dma/media/SourceTarget.png)

## <a name="add-databases"></a>Ajouter des bases de données

1. Sélectionnez les bases de données spécifiques que vous souhaitez migrer en sélectionnant uniquement les bases de données, dans le volet gauche de la **ajouter des bases de données** page.

   Par défaut, toutes les bases de données utilisateur sur l’instance de SQL Server source sont sélectionnés pour la migration

1. Utilisez les paramètres de migration sur le côté droit de la page pour définir les options de migration qui sont appliquées aux bases de données, en procédant comme suit.

   > [!NOTE]
   > Vous pouvez appliquer les paramètres de migration pour toutes les bases de données que vous effectuez une migration, en sélectionnant le serveur dans le volet gauche. Vous pouvez également configurer une base de données individuel avec des paramètres spécifiques en sélectionnant la base de données dans le volet gauche.


 1. Spécifiez le **partagé emplacement accessible par les serveurs SQL source et cible pour l’opération de sauvegarde**. Assurez-vous que le compte de service en cours d’exécution la source SQL Server instance a écriture sur l’emplacement partagé et le compte de service cible a privilèges de lecture sur l’emplacement partagé.

 1. Spécifiez l’emplacement afin de restaurer les données et les fichiers journaux transactionnels sur le serveur cible.

    ![Ajouter une page de bases de données](../dma/media/AddDatabases.png)

1. Entrez un emplacement partagé que les instances de SQL Server source et cible ont accès, dans le **partagent des options de l’emplacement** boîte.

1. Si vous ne pouvez pas fournir un emplacement partagé dont les serveurs SQL source et cible ont accès, sélectionnez **copier les sauvegardes de base de données vers un autre emplacement que le serveur cible peut lire et restaurer à partir de**. Ensuite, entrez une valeur pour le **emplacement pour les sauvegardes pour l’option de restauration** boîte. 

   Assurez-vous que le compte d’utilisateur exécutant l’Assistant Migration de données a accès en lecture à l’emplacement de sauvegarde et en écriture à l’emplacement à partir de laquelle restaure le serveur cible.

   ![Option pour copier les sauvegardes de base de données à un autre emplacement](../dma/media/CopyDatabaseDifferentLocation.png)

1. Cliquez sur **Suivant**.

L’Assistant Migration de données effectue les validations sur les dossiers de sauvegarde, données et le journal des emplacements de fichiers. Si la validation échoue, veuillez corriger les options puis cliquez sur **suivant**.

## <a name="select-logins"></a>Sélectionnez les connexions

1. Sélectionnez des connexions spécifiques pour la migration.

   > [!IMPORTANT]
   > Veillez à sélectionner les connexions qui sont mappées à un ou plusieurs utilisateurs dans les bases de données sélectionnées pour la migration.   

   Par défaut, les connexions de tous les SQL Server et Windows qui sont éligibles pour la migration sont sélectionnées pour la migration.

1. Cliquez sur **démarrer la Migration**.

   ![Sélectionnez les connexions et de démarrer la migration](../dma/media/SelectLogins.png)

## <a name="view-results"></a>Afficher les résultats

Vous pouvez surveiller la progression de la migration sur le **afficher les résultats** page.

![Affichage des résultats de la page](../dma/media/ViewResults.png)

## <a name="export-migration-results"></a>Exporter les résultats de la migration

1. Cliquez sur **exporter le rapport** au bas de la **afficher les résultats** page pour enregistrer les résultats de la migration vers un fichier CSV.

1. Passez en revue le fichier enregistré pour plus d’informations sur la migration de la connexion et vérifiez les modifications.

## <a name="see-also"></a>Voir aussi

[Assistant de Migration de données (DMA)](../dma/dma-overview.md)

[L’Assistant Migration de données : Les paramètres de Configuration](../dma/dma-configurationsettings.md)

[Assistant de Migration de données : Meilleures pratiques](../dma/dma-bestpractices.md)
