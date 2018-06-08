---
title: Requêtes Integration Services (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.querybuilder.f1
helpviewer_keywords:
- Query Builder [Integration Services]
- queries [Integration Services]
- statements [Integration Services]
- queries [Integration Services], about queries in packages
ms.assetid: 8822bd29-4575-46c8-92a0-1a39bc2604c1
caps.latest.revision: 58
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4e38b45781566ac04acdfb7584f8f10ed249d44c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-ssis-queries"></a>Requêtes Integration Services (SSIS)
  La tâche Exécution SQL, la source OLE DB, la destination OLE DB et la transformation de recherche peuvent utiliser des requêtes SQL. Dans la tâche d'exécution SQL, les instructions SQL peuvent créer, mettre à jour et supprimer des données et des objets de base de données, exécuter des procédures stockées et des instructions SELECT. Dans la source OLE DB et la transformation de recherche, les instructions SQL sont généralement des instructions SELECT ou EXEC. Cette dernière exécute le plus souvent des procédures stockées qui retournent des jeux de résultats.  
  
 Un requête peut être analysée pour déterminer si elle est valide. Pendant l’analyse d’une requête utilisant une connexion vers [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], la requête est analysée, exécutée et le résultat de l’exécution (succès ou échec) est affecté au résultat de l’analyse. Si la requête utilise une connexion à des données qui ne sont pas [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], l'instruction est seulement analysée.  
  
Vous pouvez fournir l’instruction SQL de plusieurs manières :
1.   En l’entrant directement dans le concepteur
2.   En spécifiant une connexion à un fichier qui contient l’instruction
3.   En spécifiant une variable qui contient l’instruction  
  
## <a name="direct-input-sql"></a>SQL à entrée directe  
 Le Générateur de requêtes est disponible dans l'interface utilisateur pour la tâche d'exécution SQL, la source OLE DB, la destination OLE DB et la transformation de recherche. Le générateur de requêtes présente les avantages suivants :  
  
-   Travailler visuellement ou avec des commandes SQL.  
  
     Le Générateur de requêtes comprend des volets graphiques qui composent visuellement votre requête et un volet de texte qui contient le texte SQL de votre requête. Vous pouvez travailler dans le volet graphique ou le volet de texte. Le Générateur de requêtes synchronise les vues afin de toujours faire correspondre le texte de la requête et la représentation graphique.  
  
-   Joindre des tables liées.  
  
     Si vous ajoutez plusieurs tables à votre requête, le générateur de requêtes détermine automatiquement la manière dont les tables sont associées entre elles et construit la commande de jointure appropriée.  
  
-   Interroger ou mettre à jour les bases de données.  
  
     Vous pouvez utiliser le générateur de requêtes pour renvoyer des données à l'aide d'instructions Transact-SQL SELECT, ou pour créer des requêtes qui mettent à jour, ajoutent ou suppriment des enregistrements dans une base de données.  
  
-   Afficher et modifier les résultats immédiatement.  
  
     Vous pouvez exécuter votre requête et travailler avec un jeu d'enregistrements dans une grille qui vous permet de faire défiler et de modifier les enregistrements de la base de données.  
  
 Bien que le générateur de requêtes soit limité visuellement à la création de requêtes SELECT, vous pouvez tapez le SQL pour d'autres types d'instructions, par exemple des instructions DELETE et UPDATE, dans le volet de texte. Le volet graphique est automatiquement mis à jour pour prendre en compte l'instruction SQL que vous avez tapée.  
  
 Vous pouvez également fournir une entrée directe en tapant la requête dans la boîte de dialogue de la tâche ou du composant de flux de données ou dans la fenêtre Propriétés.  
  
 Pour plus d’informations, consultez [Générateur de requêtes](http://msdn.microsoft.com/library/780752c9-6e3c-4f44-aaff-4f4d5e5a45c5).  
  
## <a name="sql-in-files"></a>SQL dans des fichiers  
 L'instruction SQL pour la tâche d'exécution SQL peut également se trouver dans un fichier distinct. Par exemple, vous pouvez écrire des requêtes à l'aide d'outils tels que l'éditeur de requêtes dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], enregistrer la requête dans un fichier, puis lire la requête à partir du fichier lors de l'exécution d'un package. Le fichier ne peut contenir que les instructions SQL à exécuter et des commentaires. Pour utiliser une instruction SQL stockée dans un fichier, vous devez fournir une connexion de fichiers qui spécifie le nom et l'emplacement du fichier. Pour plus d’informations, consultez [File Connection Manager](../integration-services/connection-manager/file-connection-manager.md).  
  
## <a name="sql-in-variables"></a>SQL dans des variables  
 Si la source de l'instruction SQL dans la tâche d'exécution SQL est une variable, vous fournissez le nom de la variable qui contient la requête. La propriété Value de la variable contient le texte de la requête. Vous définissez la propriété ValueType de la variable en tant que type de données String, puis vous tapez ou copiez l’instruction SQL dans la propriété Value. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  

## <a name="query-builder-dialog-box"></a>Générateur de requêtes, boîte de dialogue
Utilisez la boîte de dialogue **Générateur de requêtes** pour créer une requête à utiliser dans la tâche Exécution SQL, la source OLE DB et la destination OLE DB, ainsi que la transformation de recherche.  
  
 Vous pouvez utiliser le générateur de requêtes pour réaliser les tâches suivantes :  
  
-   **Utilisation d'une représentation graphique d'une requête ou des commandes SQL** Le générateur de requêtes inclut un volet qui affiche votre requête graphiquement et un volet qui affiche le texte SQL de votre requête. Vous pouvez travailler dans le volet graphique ou dans le volet texte. Le générateur de requêtes synchronise les vues de manière à ce qu'elles soient toujours actualisées.  
  
-   **Jointure de tables associées** Si vous ajoutez plusieurs tables à votre requête, le générateur de requêtes détermine automatiquement la manière dont les tables sont associées et construit la commande de jointure appropriée.  
  
-   **Requête ou mise à jour de bases de données** Vous pouvez utiliser le générateur de requêtes pour renvoyer des données à l’aide d’instructions Transact-SQL SELECT et pour créer des requêtes qui mettent à jour, ajoutent ou suppriment des enregistrements dans une base de données.  
  
-   **Affichage et modification immédiate des résultats** Vous pouvez exécuter votre requête et travailler avec un jeu d'enregistrements dans une grille qui vous permet de faire défiler et de modifier les enregistrements de la base de données.  
  
 Les outils graphiques de la boîte de dialogue **Générateur de requêtes** permettent de créer des requêtes par glisser-déplacer. Par défaut, la boîte de dialogue Générateur de requêtes crée des requêtes SELECT, mais vous pouvez également générer des requêtes INSERT, UPDATE ou DELETE. Tous les types d'instructions SQL peuvent être analysés et exécutés dans la boîte de dialogue **Générateur de requêtes** . Pour plus d’informations sur les instructions SQL dans les packages, consultez [Requêtes Integration Services &#40;SSIS&#41;](../integration-services/integration-services-ssis-queries.md).  
  
 Pour en savoir plus sur le langage Transact-SQL et sa syntaxe, consultez [Référence Transact-SQL &#40;moteur de base de données&#41;](../t-sql/transact-sql-reference-database-engine.md).  
  
 Vous pouvez aussi utiliser des variables dans une requête pour fournir des valeurs à un paramètre d'entrée, pour capturer les valeurs des paramètres de sortie et pour stocker des codes de retour. Pour en savoir plus sur l’utilisation de variables dans les requêtes utilisées par les packages, consultez [Tâche d’exécution de requêtes SQL](../integration-services/control-flow/execute-sql-task.md), [Source OLE DB](../integration-services/data-flow/ole-db-source.md)et [Integration Services &#40;SSIS&#41; Queries](../integration-services/integration-services-ssis-queries.md). Pour en savoir plus sur l’utilisation de variables dans la tâche d’exécution de requêtes SQL, consultez [Paramètres et codes de retour dans la tâche d’exécution SQL](http://msdn.microsoft.com/library/a3ca65e8-65cf-4272-9a81-765a706b8663) et [Ensembles de résultats dans la tâche d’exécution SQL](http://msdn.microsoft.com/library/62605b63-d43b-49e8-a863-e154011e6109).  
  
 Les transformations de recherche et de recherche floue peuvent aussi utiliser des variables avec des paramètres et des codes de retour. Les informations relatives à la source OLE DB s'appliquent également à ces deux transformations.  
  
### <a name="options"></a>Options  
 **Barre d'outils**  
 Utilisez la barre d'outils pour gérer les datasets, sélectionner les volets à afficher et contrôler les fonctions de requête.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Afficher/Masquer le volet Diagramme**|Affiche ou masque le volet **Diagramme** .|  
|**Afficher/Masquer le volet Grille**|Affiche ou masque le volet **Grille** .|  
|**Afficher/Masquer le volet SQL**|Affiche ou masque le volet **SQL** .|  
|**Afficher/Masquer le volet Résultats**|Affiche ou masque le volet **Résultats** .|  
|**Exécuter**|Exécute la requête. Les résultats s'affichent dans le volet Résultats.|  
|**Vérifier SQL**|Vérifie que l'instruction SQL est valide.|  
|**Tri croissant**|Trie dans l'ordre croissant les lignes de sortie sur la colonne sélectionnée du volet de la grille.|  
|**Tri décroissant**|Trie dans l'ordre décroissant les lignes de sortie sur la colonne sélectionnée du volet de la grille.|  
|**Supprimer le filtre**|Sélectionnez le nom d'une colonne dans le volet de la grille, puis choisissez **Supprimer le filtre** pour supprimer les critères de tri de la colonne.|  
|**Utiliser GROUP BY**|Ajoute la fonctionnalité GROUP BY à la requête.|  
|**Ajouter une table**|Ajoute une table à la requête.|  
  
 **Définition de la requête**  
 La définition de la requête fournit une barre d'outils et des volets dans lesquels définir et tester la requête.  
  
|Volet|Description|  
|----------|-----------------|  
|Volet**Diagramme** .|Affiche la requête dans un diagramme. Le diagramme illustre les tables contenues dans la requête et leur mode de jointure. Activez ou désactivez la case à cocher correspondant à une colonne de la table pour l'ajouter ou la supprimer du résultat de la requête.<br /><br /> Lorsque vous ajoutez des tables à la requête, le Générateur de requêtes crée des jointures entre les tables basées sur les tables, en fonction des clés de la table. Pour ajouter une jointure, faites glisser le champ d'une table vers un champ situé dans une autre table. Pour gérer une jointure, cliquez dessus avec le bouton droit, puis sélectionnez une option du menu.<br /><br /> Cliquez avec le bouton droit sur le volet **Diagramme** pour ajouter ou supprimer des tables, sélectionner toutes les tables et afficher ou masquer des volets.|  
|Volet**Grille** |Affiche la requête dans une grille. Vous pouvez utiliser ce volet pour ajouter et supprimer des colonnes dans la requête et modifier les paramètres de chaque colonne.|  
|Volet**SQL** |Affiche la requête sous forme de texte SQL. Les modifications effectuées dans le volet **Diagramme** et le volet **Grille** sont affichées ici et les modifications apportées ici sont affichées dans les volets **Diagramme** et **Grille** .|  
|Volet**Résultats** |Affiche les résultats de la requête lorsque vous cliquez sur **Exécuter** dans la barre d'outils.| 

  
