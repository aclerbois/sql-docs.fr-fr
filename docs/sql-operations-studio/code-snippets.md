---
title: Créer des extraits de code dans les opérations de SQL Studio (version préliminaire) | Documents Microsoft
description: Découvrez comment créer et utiliser des extraits de code SQL dans SQL opérations Studio (version préliminaire)
ms.custom: tools|sos
ms.date: 11/15/2017
ms.reviewer: alayu; sstein
ms.prod: sql
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e8cd2dc80f7a719f82bd4ff09729cbbd5dfb4186
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="create-and-use-code-snippets-to-quickly-create-transact-sql-t-sql-scripts-in-includename-sosincludesname-sos-shortmd"></a>Créer et utiliser des extraits de code pour créer rapidement des scripts Transact-SQL (T-SQL) dans [!INCLUDE[name-sos](../includes/name-sos-short.md)]

Extraits de code dans le code [!INCLUDE[name-sos](../includes/name-sos-short.md)] sont des modèles qui le rendent plus facile de créent des bases de données et les objets de base de données. 

[!INCLUDE[name-sos](../includes/name-sos-short.md)] fournit plusieurs extraits de code T-SQL pour vous aider à générer rapidement la syntaxe appropriée. 

Extraits de code défini par l’utilisateur peuvent également être créées.

## <a name="using-built-in-t-sql-code-snippets"></a>À l’aide d’extraits de code T-SQL intégrés

1. Pour accéder aux extraits de code disponibles, tapez *sql* dans l’éditeur de requête pour ouvrir la liste :

   ![extraits de code](media/code-snippets/sql-snippets.png)

1. Sélectionnez l’extrait de code que vous souhaitez utiliser, et génère le script T-SQL. Par exemple, sélectionnez *sqlCreateTable*:

   ![créer des extraits de la table](media/code-snippets/create-table.png)

1. Mettre à jour les champs en surbrillance avec les valeurs spécifiques. Par exemple, remplacez *TableName* et *schéma* avec les valeurs de votre base de données :

   ![Remplacez le champ de modèle](media/code-snippets/table-from-snippet.png)

   Si le champ que vous souhaitez modifier n’est plus balisée (cela se produit lorsque vous déplacez le curseur autour de l’éditeur), cliquez sur le mot que vous souhaitez modifier, puis sélectionnez **modifier toutes les occurrences**:

   ![Remplacez le champ de modèle](media/code-snippets/change-all.png)

1. Mettre à jour ou ajouter n’importe quel T-SQL supplémentaires vous avez besoin pour l’extrait de code sélectionné. Par exemple, mettre à jour *Column1*, *Column2*et ajouter d’autres colonnes.


 
## <a name="creating-sql-code-snippets"></a>Création d’extraits de code SQL 

Vous pouvez définir vos propres extraits de code. Pour ouvrir le fichier d’extrait de code SQL pour la modification :

1. Ouvrir le *Palette de commandes* (**Maj + Ctrl + P**) et le type *capture*, puis sélectionnez **préférences : ouvrir des extraits de code utilisateur**:

   ![Remplacez le champ de modèle](media/code-snippets/user-snippets.png)

1. Sélectionnez **SQL**:

   > [!NOTE]
   > [!INCLUDE[name-sos](../includes/name-sos-short.md)] Cet article traite plus précisément à l’aide d’extraits de code SQL hérite sa fonctionnalité d’extrait de code à partir de Code Visual Studio. Pour plus d’informations, consultez [créer vos propres extraits de code](https://code.visualstudio.com/docs/editor/userdefinedsnippets) dans la documentation de Visual Studio Code. 

   ![Remplacez le champ de modèle](media/code-snippets/select-sql.png)

1. Collez le code suivant dans *sql.json*:

   ```sql
   "Select top 5": {
    "prefix": "sqlSelectTop5",
    "body": "SELECT TOP 5 * FROM ${1:TableName}",
    "description": "User-defined snippet example 1"
    },
    "Create Table snippet":{
    "prefix": "sqlCreateTable2",
    "body": [
    "-- Create a new table called '${1:TableName}' in schema '${2:SchemaName}'",
    "-- Drop the table if it already exists",
    "IF OBJECT_ID('$2.$1', 'U') IS NOT NULL",
    "DROP TABLE $2.$1",
    "GO",
    "-- Create the table in the specified schema",
    "CREATE TABLE $2.$1",
    "(",
    "   $1Id INT NOT NULL PRIMARY KEY, -- primary key column",
    "   Column1 [NVARCHAR](50) NOT NULL,",
    "   Column2 [NVARCHAR](50) NOT NULL",
    "   -- specify more columns here",
    ");",
    "GO"
    ],
   "description": "User-defined snippet example 2"
   }
   ```

1. Enregistrez le fichier sql.json.
1. Ouvrez une nouvelle fenêtre d’éditeur de requête en cliquant sur **Ctrl + N**.
2. Type **sql**, et vous voyez les extraits de code utilisateur en deux que vous venez d’ajouter ; *sqlCreateTable2* et *sqlSelectTop5*.

Sélectionnez un des nouveaux extraits de code et lui donner une série de tests.


## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations sur l’éditeur SQL, consultez [didacticiel de l’éditeur de Code](tutorial-sql-editor.md).