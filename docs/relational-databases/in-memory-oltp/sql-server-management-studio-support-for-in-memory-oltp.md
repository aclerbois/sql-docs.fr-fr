---
title: Prise en charge d’OLTP en mémoire par SQL Server Management Studio | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ee847b5f-6a1a-448e-a746-d61a023881ff
caps.latest.revision: 31
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3ddd883651796b31d1cc8745df47a9e0eb44bc7b
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-management-studio-support-for-in-memory-oltp"></a>Prise en charge de SQL Server Management Studio pour l'OLTP en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un environnement intégré pour la gestion de votre infrastructure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fournit des outils permettant de configurer, de surveiller et d’administrer les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SQL Server Management Studio](http://msdn.microsoft.com/library/66a6b7b1-de6a-4161-82bd-98ded486947b).  
  
 Les tâches de cette rubrique décrivent comment utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour gérer les tables mémoire optimisées ; les index sur les tables mémoire optimisées ; les procédures stockées compilées en mode natif ; et les types de tables mémoire optimisées, définis par l'utilisateur.  
  
 Pour plus d’informations sur la façon de créer par programmation des tables optimisées en mémoire, consultez [Création d’une table optimisée en mémoire et d’une procédure stockée compilée en mode natif](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).  
  
### <a name="to-create-a-database-with-a-memory-optimized-data-filegroup"></a>Pour créer une base de données avec un groupe de fichiers de données optimisé en mémoire  
  
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et développez-la.  
  
2.  Cliquez avec le bouton droit sur **Bases de données**, puis cliquez sur **Nouvelle base de données**.  
  
3.  Pour ajouter un nouveau groupe de fichiers de données optimisé en mémoire, cliquez sur la page **Groupes de fichiers** . Sous **MEMORY OPTIMIZED DATA**, cliquez sur **Ajouter un groupe de fichiers** , puis entrez le nom du groupe de fichiers de données optimisé en mémoire.  La colonne correspondant aux **fichiers FILESTREAM** représente le nombre de conteneurs dans le groupe de fichiers. Les conteneurs sont ajoutés à la page **Général** .  
  
4.  Pour ajouter un fichier (conteneur) au groupe de fichiers, cliquez sur la page **Général** . Sous **Fichiers de la base de données**, cliquez sur **Ajouter**. Sélectionnez **Type de fichier** comme **Données FILESTREAM**, spécifiez le nom logique du conteneur, sélectionnez le groupe de fichiers optimisé en mémoire, puis assurez-vous que l’option **Croissance automatique/Taille maximale** est définie sur **Illimité**.  
  
     Pour plus d’informations sur la création d’une base de données à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], consultez [Créer une base de données](../../relational-databases/databases/create-a-database.md).  
  
### <a name="to-create-a-memory-optimized-table"></a>Pour créer une table optimisée en mémoire  
  
1.  Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur le nœud **Tables** de votre base de données, cliquez sur **Nouveau**, puis cliquez sur **Table optimisée en mémoire**.  
  
     Un modèle pour créer des tables optimisées en mémoire s'affiche.  
  
2.  Pour remplacer les paramètres du modèle, cliquez sur **Spécifier les valeurs des paramètres du modèle** dans le menu **Requête** .  
  
     Pour plus d’informations sur la manière d’utiliser les modèles, consultez [Explorateur de modèles](http://msdn.microsoft.com/library/b9ee55c5-bb44-4f76-90ac-792d8d83b4c8).  
  
3.  Dans l’ **Explorateur d’objets**, les tables sont d’abord triées en fonction des tables sur disque suivies des tables optimisées en mémoire. Utilisez les **Détails de l’Explorateur d’objets** pour afficher toutes les tables classées par nom.  
  
### <a name="to-create-a-natively-compiled-stored-procedure"></a>Pour créer une procédure stockée compilée en mode natif  
  
1.  Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur le nœud **Procédures stockées** de votre base de données, cliquez sur **Nouveau**, puis cliquez sur **Procédure stockée compilée en mode natif**.  
  
     Un modèle pour créer des procédures stockées compilées en mode natif s'affiche.  
  
2.  Pour remplacer les paramètres du modèle, cliquez sur **Spécifier les valeurs des paramètres du modèle** dans le menu **Requête**.  
  
     Pour plus d’informations sur la création de procédures stockées, consultez [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md).  
  
### <a name="to-create-a-user-defined-memory-optimized-table-type"></a>Pour créer un type de table optimisée en mémoire, défini par l'utilisateur  
  
1.  Dans l’ **Explorateur d’objets**, développez le nœud **Types** de votre base de données, cliquez avec le bouton droit sur le nœud **Types de tables définis par l’utilisateur** , cliquez sur **Nouveau**, puis cliquez sur **Type de table optimisée en mémoire défini par l’utilisateur**.  
  
     Un modèle pour créer un type de table optimisée en mémoire défini par l'utilisateur s'affiche.  
  
2.  Pour remplacer les paramètres du modèle, cliquez sur **Spécifier les valeurs des paramètres du modèle** dans le menu **Requête**.  
  
     Pour plus d’informations sur la création de procédures stockées, consultez [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md).  
  
## <a name="memory-monitoring"></a>Surveillance de la mémoire  
  
#### <a name="view-memory-usage-by-memory-optimized-objects-report"></a>Consulter l'utilisation de la mémoire à l'aide du rapport des objets optimisés en mémoire  
  
-   Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la base de données, cliquez sur **Rapports**, cliquez sur **Rapports standard**, puis cliquez sur **Utilisation de la mémoire par les objets mémoire optimisés**.  
  
     Ce rapport fournit des informations détaillées sur l'utilisation de l'espace mémoire par des objets optimisés en mémoire au sein de la base de données.  
  
#### <a name="view-properties-for-allocated-and-used-memory-for-a-table-database"></a>Afficher les propriétés de la mémoire allouée et utilisée pour une table ou une base de données  
  
1.  Pour obtenir des informations sur l'utilisation de la mémoire :  
  
    -   Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la table optimisée en mémoire, cliquez sur **Propriétés**, puis cliquez sur la page **Stockage** . La valeur de la propriété **Espace de données** indique la mémoire utilisée par les données dans la table. La valeur de la propriété **Espace d’index** indique la mémoire utilisée par les index dans la table.  
  
    -   Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la base de données, cliquez sur **Propriétés**, puis cliquez sur la page **Général** . La valeur de la propriété **Mémoire allouée aux objets mémoire optimisés** indique la mémoire allouée aux objets optimisés en mémoire dans la base de données. La valeur de la propriété **Mémoire utilisée par les objets mémoire optimisés** indique la mémoire utilisée par les objets optimisés en mémoire dans la base de données.  
  
## <a name="supported-features-in-includessmanstudiofullincludesssmanstudiofull-mdmd"></a>Fonctionnalités prises en charge dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] prend en charge les fonctionnalités et les opérations prises en charge par le moteur de base de données sur les bases de données contenant un groupe de fichiers de données optimisé en mémoire, des tables optimisées en mémoire, des index et des procédures stockées compilées en mode natif.  
  
 Pour la base de données, la table, la procédure stockée, le type de table défini par l'utilisateur ou les objets Index, les fonctionnalités de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] suivantes ont été mises à jour ou étendues pour prendre en charge OLTP en mémoire.  
  
-   Explorateur d'objets  
  
    -   Menu contextuels  
  
    -   Paramètres du filtre  
  
    -   Script en tant que  
  
    -   Tâches  
  
    -   Rapports  
  
    -   Propriétés  
  
    -   Tâche de base de données :  
  
        -   Attachez et détachez une base de données qui contient des tables optimisées en mémoire.  
  
             L’interface utilisateur **Attacher des bases de données** n’affiche pas le groupe de fichiers de données optimisés en mémoire. Toutefois, vous pouvez poursuivre l'attachement de la base de données et celle-ci sera attachée correctement.  
  
            > [!NOTE]  
            >  Si vous voulez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour attacher une base de données qui contient un conteneur de groupe de fichiers de données optimisés en mémoire et, si celui-ci a été créé sur un autre ordinateur, l'emplacement de ce conteneur doit être le même sur les deux ordinateurs. Si vous voulez que l'emplacement du conteneur de groupe de fichiers de données optimisés en mémoire de la base de données soit différent sur le nouvel ordinateur, utilisez [!INCLUDE[tsql](../../includes/tsql-md.md)] pour attacher la base de données. Dans l'exemple suivant, l'emplacement du conteneur de groupe de fichiers de données optimisés en mémoire sur le nouvel ordinateur est C:\Folder2. Cependant, lors de la création du conteneur de groupe de fichier de données optimisés en mémoire sur le premier ordinateur, l'emplacement était C:\Folder1.  
            >   
            >  `CREATE DATABASE[imoltp] ON`  
            >   
            >  `(NAME =N'imoltp',FILENAME=N'C:\Folder2\imoltp.mdf'),`  
            >   
            >  `(NAME =N'imoltp_mod1',FILENAME=N'C:\Folder2\imoltp_mod1'),`  
            >   
            >  `(NAME =N'imoltp_log',FILENAME=N'C:\Folder2\imoltp_log.ldf')`  
            >   
            >  `FOR ATTACH`  
            >   
            >  `GO`  
  
        -   Générez des &amp;scripts.  
  
             Dans l’**Assistant Générer et publier des scripts**, la valeur par défaut de l’option de script **Vérifier l’existence de l’objet** est FALSE. Si la valeur de l’option de script **Vérifier l’existence de l’objet** est TRUE dans l’écran **Définir les options de script** de l’Assistant, le script généré contient « CREATE PROCEDURE <nom_procédure> AS » et « ALTER PROCEDURE <nom_procédure> <définition_procédure> ». Lorsqu'il est exécuté, le script généré retourne une erreur, car ALTER PROCEDURE n'est pas pris en charge sur les procédures stockées compilées en mode natif.  
  
             Pour modifier le script généré pour chaque procédure stockée compilée en mode natif :  
  
            1.  Dans « CREATE PROCEDURE <nom_procédure> AS », remplacez « AS » par « <définition_procédure> ».  
  
            2.  Supprimez « ALTER PROCEDURE <nom_procédure> <définition_procédure> ».  
  
        -   Copie de bases de données. Pour les bases de données contenant des objets optimisés en mémoire, la création de la base de données sur le serveur de destination et le transfert des données ne seront pas exécutés dans une transaction.  
  
        -   Importez et exportez des données. Utilisez l’ **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et l’option Copier les données à partir d’une ou plusieurs tables ou vues** . Si la table de destination est une table optimisée en mémoire qui n'existe pas dans la base de données de destination :  
  
            1.  Dans l’**Assistant Importation et Exportation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**, dans l’écran **Spécifier la copie ou l’interrogation de table**, sélectionnez **Copier les données à partir d’une ou plusieurs tables ou vues**. Ensuite, cliquez sur **Suivant**.  
  
            2.  Cliquez sur **Modifier les mappages**. Sélectionnez ensuite **Créer la table de destination** puis cliquez sur **Modifier SQL**. Entrez la syntaxe CREATE TABLE pour créer une table optimisée en mémoire sur la base de données de destination. Cliquez sur **OK** et suivez les étapes restantes de l’Assistant.  
  
        -   Plans de maintenance. Les tâches de maintenance de réorganisation et de reconstruction d'index ne sont pas prises en charge sur les tables optimisées en mémoire et leurs index. Par conséquent, lorsqu'un plan de maintenance pour reconstruire et réorganiser l'index est exécuté, les tables optimisées en mémoire et leurs index dans les bases de données sélectionnées sont omis.  
  
             Les statistiques de mise à jour des tâches de maintenance ne sont pas prises en charge avec une analyse d'échantillons sur les tables optimisées en mémoire et leurs index. Ainsi, quand un plan de maintenance des statistiques de mise à jour est exécuté, les statistiques des tables optimisées en mémoire et leurs index sont toujours mis à jour avec **WITH FULLSCAN, NORECOMPUTE**.  
  
-   Volet Détails de l'Explorateur d'objets  
  
-   Explorateur de modèles  
  
## <a name="unsupported-features-in-includessmanstudiofullincludesssmanstudiofull-mdmd"></a>Fonctionnalités non prises en charge dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
 Pour les objets OLTP en mémoire, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ne prend pas en charge les fonctionnalités et les opérations qui ne sont pas non plus prises en charge par le moteur de base de données.  
  
 Pour plus d’informations sur les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non prises en charge, consultez [Fonctionnalités SQL Server non prises en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Prise en charge d'OLTP en mémoire par SQL Server](../../relational-databases/in-memory-oltp/sql-server-support-for-in-memory-oltp.md)  
  
  
