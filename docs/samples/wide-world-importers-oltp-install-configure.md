---
title: Installer et configurer la base de données exemple WideWorldImporters - SQL | Documents Microsoft
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8d1d00b5d3e5650e628935c840f280b278869b62
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installation-and-configuration"></a>Installation et configuration
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
Wide World Importers OLTP instructions d’installation et configuration de base de données.

## <a name="prerequisites"></a>Configuration requise

- [SQL Server 2016](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016) (ou version ultérieure) ou [base de données SQL Azure](https://azure.microsoft.com/services/sql-database/). Pour obtenir la version complète de l’exemple, utilisez SQL Server Evaluation/développeur/Enterprise Edition.
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Pour obtenir les meilleurs résultats utilisent la version de juin 2016 ou version ultérieure.

## <a name="download"></a>Télécharger

La dernière version de l’exemple :

[wide-world-importers-release](http://go.microsoft.com/fwlink/?LinkID=800630)

Téléchargez l’exemple WideWorldImporters de base de données sauvegarde/bacpac qui correspond à votre édition de SQL Server ou de la base de données SQL Azure.

Code source pour recréer la base de données est disponible à partir de l’emplacement suivant. Notez qu’en recréant l’exemple entraîne légères différences dans les données, car il existe un facteur aléatoire dans la génération de données :

[importateurs de Wide world](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/wwi-database-scripts)

## <a name="install"></a>Installation


### <a name="sql-server"></a>SQL Server

Pour restaurer une sauvegarde à une instance de SQL Server, vous pouvez utiliser Management Studio.

1. Ouvrez SQL Server Management Studio et connectez-vous à l’instance de SQL Server cible.
2. Avec le bouton droit sur le **bases de données** nœud et sélectionnez **restaurer la base de données**.
3. Sélectionnez **périphérique** , puis cliquez sur le bouton **...**
4. Dans la boîte de dialogue **sélectionner les unités de sauvegarde**, cliquez sur **ajouter**, accédez à la sauvegarde de base de données dans le système de fichiers du serveur et sélectionnez la sauvegarde. Cliquez sur **OK**.
5. Si nécessaire, modifiez l’emplacement cible pour les données et fichiers journaux, dans le **fichiers** volet. Notez qu’il est recommandé de placer des données et fichiers journaux sur des lecteurs différents.
6. Cliquez sur **OK**. Ceci lancera la restauration de la base de données. Après avoir terminé, vous devez la base de données WideWorldImporters installés sur votre instance de SQL Server.

### <a name="azure-sql-database"></a>Azure SQL Database

Pour importer un fichier bacpac dans une base de données SQL, vous pouvez utiliser Management Studio.

1. (facultatif) Si vous n’avez pas encore d’un serveur SQL Server dans Azure, accédez à la [portail Azure](https://portal.azure.com/) et créer une base de données SQL. En cours de créer une base de données, vous allez créer un serveur. Prenez note du serveur.
   - Consultez [ce didacticiel](https://azure.microsoft.com/documentation/articles/sql-database-get-started/) pour créer une base de données en quelques minutes
2. Ouvrez SQL Server Management Studio et connectez-vous à votre serveur dans Azure.
3. Avec le bouton droit sur le **bases de données** nœud et sélectionnez **importer l’Application de couche données**.
4. Dans le **importer les paramètres** sélectionnez **importer à partir du disque local** et sélectionnez le fichier bacpac de la base de données à partir de votre système de fichiers.
5. Sous **les paramètres de base de données** modifier le nom de la base de données pour *WideWorldImporters* et sélectionnez l’objectif d’édition et le service cible à utiliser.
6. Cliquez sur **suivant** et **Terminer** pour déclencher le déploiement. Il va de quelques minutes à s’effectuer dans un P1. Si un prix plus bas niveau est souhaité, il est recommandé d’importer dans une base de données P1, puis modifiez le niveau de tarification selon le niveau souhaité.

## <a name="configuration"></a>Configuration

### <a name="full-text-indexing"></a>Indexation de texte intégral

La base de données peut tirer parti de l’indexation de texte intégral. Toutefois, cette fonctionnalité n’est pas installée par défaut avec SQL Server : vous devez la sélectionner lors de l’installation de SQL Server (elle est activée par défaut dans la base de données SQL Azure). Par conséquent, après l’installation est requise.

1. Dans SQL Server Management Studio, connectez-vous à la base de données WideWorldImporters et ouvrir une nouvelle fenêtre de requête.
2. Exécutez la commande T-SQL suivante pour activer l’utilisation de l’indexation de texte intégral dans la base de données :  `EXECUTE Application.Configuration_ApplyFullTextIndexing`


### <a name="sql-server-audit"></a>SQL Server Audit

S’applique à : SQL Server

Activation de l’audit dans SQL Server requiert la configuration du serveur. Pour activer l’audit de SQL Server pour l’exemple de WideWorldImporters, exécutez l’instruction suivante dans la base de données :

    EXECUTE [Application].[Configuration_ApplyAuditing]

Dans la base de données SQL Azure, l’Audit est configuré via la [portail Azure](https://portal.azure.com/).

### <a name="row-level-security"></a>Sécurité au niveau des lignes

S’applique à : base de données SQL Azure

Sécurité au niveau de la ligne n’est pas activée par défaut dans le téléchargement de bacpac de WideWorldImporters. Pour activer la sécurité au niveau de la ligne dans la base de données, exécutez la procédure stockée suivante :

    EXECUTE [Application].[Configuration_ApplyAuditing]

