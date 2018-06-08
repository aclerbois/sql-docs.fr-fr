---
title: 'Didacticiel : Utiliser l’éditeur SQL opérations Studio (version préliminaire) Transact-SQL pour créer des objets de base de données | Documents Microsoft'
description: Ce didacticiel présente les principales fonctionnalités qui simplifient l’utilisation de T-SQL dans Studio des opérations SQL (version préliminaire).
ms.custom: tools|sos
ms.date: 03/13/2018
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5ea03ea9ee0d45e15ec81dda9be95d38ad99c6d6
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="tutorial-use-the-transact-sql-editor-to-create-database-objects---includename-sosincludesname-sos-shortmd"></a>Didacticiel : Utiliser l’éditeur Transact-SQL pour créer des objets de base de données- [!INCLUDE[name-sos](../includes/name-sos-short.md)]

Création et exécution de requêtes, procédures stockées, scripts, etc. sont les principales tâches de professionnels de la base de données. Ce didacticiel illustre les fonctionnalités clés dans l’éditeur T-SQL pour créer des objets de base de données.

Dans ce didacticiel, vous apprenez à utiliser [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] pour :
> [!div class="checklist"]
> * Objets de base de données de recherche
> * Modifier les données de la table 
> * Utiliser des extraits de code pour écrire rapidement T-SQL
> * Détails des objets de base de données à l’aide de *aperçu de définition* et *atteindre la définition*


## <a name="prerequisites"></a>Configuration requise

Ce didacticiel nécessite SQL Server ou la base de données SQL Azure *TutorialDB*. Pour créer le *TutorialDB* de base de données, effectuez l’une des Démarrages rapides suivants :

- [Se connecter et interroger à l’aide de SQL Server [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
- [Se connecter et interroger à l’aide de la base de données SQL Azure [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)


## <a name="quickly-locate-a-database-object-and-perform-a-common-task"></a>Rapidement localiser un objet de base de données et effectuer une tâche courante

[!INCLUDE[name-sos-short](../includes/name-sos-short.md)] fournit un widget de recherche pour trouver rapidement les objets de base de données. La liste des résultats fournit un menu contextuel pour les tâches courantes relatives à l’objet sélectionné, tel que *modifier les données* pour une table.

1. Ouvrez la barre latérale de serveurs (**Ctrl + G**), développez **bases de données**, puis sélectionnez **TutorialDB**. 

1. Ouvrez le *TutorialDB le tableau de bord* en double-cliquant sur **TutorialDB** et en sélectionnant **gérer** dans le menu contextuel :

   ![menu contextuel - gérer](./media/tutorial-sql-editor/insight-open-dashboard.png)

1. Cliquez sur le tableau de bord, **dbo. Clients** (dans le widget de recherche), puis sélectionnez **modifier les données**.
   
   > [!TIP]
   > Pour les bases de données avec de nombreux objets, utiliser le widget de recherche pour localiser rapidement la table, vue, etc. que vous recherchez.

   ![widget de la recherche rapide](./media/tutorial-sql-editor/quick-search-widget.png)

1. Modifier la **messagerie** colonne dans la première ligne, type *orlando0@adventure-works.com*et appuyez sur **entrée** pour enregistrer les modifications.

   ![modifier des données](./media/tutorial-sql-editor/edit-data.png)

## <a name="use-t-sql-snippets-to-create-stored-procedures"></a>Utiliser des extraits de code T-SQL pour créer des procédures stockées

Opérations de SQL Studio fournit plusieurs extraits de code T-SQL intégrés pour créer rapidement des instructions.


1. Ouvrez un nouvel éditeur de requête en appuyant sur **Ctrl + N**.

2. Type **sql** dans l’éditeur, la flèche vers le bas pour **sqlCreateStoredProcedure**, puis appuyez sur la *onglet* clé (ou *entrée*) pour charger la créer stockée extrait de la procédure.

   ![liste de l’extrait de code](./media/tutorial-sql-editor/snippet-list.png)

3. L’extrait de code de procédure stockée créer comporte deux champs défini pour la modification rapide, *StoredProcedureName* et *SchemaName*. Sélectionnez *StoredProcedureName*, avec le bouton droit, puis sélectionnez **modifier toutes les Occurrences**. Tapez maintenant *getCustomer* et tous les *StoredProcedureName* pour modifier les entrées *getCustomer*.

   ![Extrait de code](./media/tutorial-sql-editor/snippet.png)

5. Remplacez toutes les occurrences de *SchemaName* à *dbo*. 
6. L’extrait de code contient les paramètres de l’espace réservé et le texte à mettre à jour. Le *EXECUTE* instruction contient également le texte d’espace réservé, car il ne connaît pas le nombre de paramètres aura par la procédure. Pour ce didacticiel mise à jour de l’extrait de code il ressemble le code suivant :

    ```sql
    -- Create a new stored procedure called 'getCustomer' in schema 'dbo'
    -- Drop the stored procedure if it already exists
    IF EXISTS (
    SELECT *
    FROM INFORMATION_SCHEMA.ROUTINES
    WHERE SPECIFIC_SCHEMA = N'dbo'
    AND SPECIFIC_NAME = N'getCustomer'
    )
    DROP PROCEDURE dbo.getCustomer
    GO
    -- Create the stored procedure in the specified schema
    CREATE PROCEDURE dbo.getCustomer
    @ID int
    -- add more stored procedure parameters here
    AS
    -- body of the stored procedure
    SELECT  c.CustomerId, 
    c.Name, 
    c.Location, 
    c.Email
    FROM dbo.Customers c
    WHERE c.CustomerId = @ID
    FOR JSON PATH

    GO
    -- example to execute the stored procedure we just created
    EXECUTE dbo.getCustomer 1
    GO
    ```
    
5. Pour créer la procédure stockée et lui donner une série de tests, appuyez sur **F5**.

La procédure stockée est maintenant créée et le **résultats** volet affiche le client retourné au format JSON. Pour afficher au format JSON, cliquez sur l’enregistrement retourné. 


## <a name="use-peek-definition"></a>Utilisez l’aperçu de définition 

Opérations de SQL Studio offre la possibilité pour afficher une définition d’objets à l’aide de la fonctionnalité de définition d’aperçu. Cette section crée une seconde procédure stockée et utilise un aperçu de définition pour voir quels sont les colonnes dans une table pour créer rapidement le corps de la procédure stockée.

1. Ouvrez un nouvel éditeur en appuyant sur **Ctrl + N**. 

2. Type *sql* dans l’éditeur, la flèche vers le bas pour *sqlCreateStoredProcedure*, puis appuyez sur la *onglet* clé (ou *entrée*) pour charger la créer stockée extrait de la procédure.
3. Tapez dans *setCustomer* pour *StoredProcedureName* et *dbo* pour *SchemaName*

3. Remplacez le @param des espaces réservés avec la définition des paramètres suivants :

   ```sql
   @json_val nvarchar(max)
   ```

4. Remplacez le corps de la procédure stockée par le code suivant :
   ```sql
   INSERT INTO dbo.Customers
   ```

5. Dans le *insérer* vient d’être ajouté, avec le bouton droit de la ligne vous **dbo. Clients** et sélectionnez **aperçu de définition**.

   ![Aperçu de définition](./media/tutorial-sql-editor/peek-definition.png)

6. La définition de table s’affiche pour vous pouvez de voir rapidement les colonnes qui figurent dans la table. Reportez-vous à la liste des colonnes facilement compléter les instructions de votre procédure stockée. Terminer la création de l’instruction INSERT que vous avez ajouté précédemment pour terminer le corps de la procédure stockée et fermer la fenêtre de définition de lecture :

   ```sql
   INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
    )
   ```
7. Supprimer (ou mettez en commentaire) la *EXECUTE* commande en bas de la requête.
8. L’instruction entière doit se présenter comme le code suivant :

   ```sql
   -- Create a new stored procedure called 'setCustomer' in schema 'dbo'
   -- Drop the stored procedure if it already exists
   IF EXISTS (
   SELECT *
       FROM INFORMATION_SCHEMA.ROUTINES
       WHERE SPECIFIC_SCHEMA = N'dbo'
       AND SPECIFIC_NAME = N'setCustomer'
   )
   DROP PROCEDURE dbo.setCustomer
   GO
   -- Create the stored procedure in the specified schema
   CREATE PROCEDURE dbo.setCustomer
       @json_val nvarchar(max) 
   AS
       -- body of the stored procedure
       INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
       )
   GO
   ```

8. Pour créer le *setCustomer* procédure stockée, appuyez sur **F5**.

## <a name="use-save-query-results-as-json-to-test-the-setcustomer-stored-procedure"></a>Utilisez Enregistrer les résultats de la requête au format JSON pour tester la procédure stockée de setCustomer

Le *setCustomer* procédure stockée créée dans la section précédente requiert JSON données être passées dans le *@json_val* paramètre. Cette section montre comment obtenir un bit correctement mis en forme de JSON à passer dans le paramètre, vous pouvez tester la procédure stockée.

1. Dans le **serveurs** droit de la barre latérale du *dbo. Clients* de table et cliquez sur **sélectionnez 1000 lignes du haut**.

2. Sélectionnez la première ligne dans l’affichage des résultats, vérifiez que la ligne entière est sélectionnée (cliquez sur le numéro 1 dans la colonne la plus à gauche), puis sélectionnez **enregistrer en tant que JSON**.  
3. Accédez à un emplacement, vous devez vous rappeler afin de pouvoir supprimer le fichier ultérieures (pour le bureau de l’exemple) et cliquez sur **enregistrer**. Le JSON au format de fichier s’ouvre.

   ![Enregistrer en tant que JSON](./media/tutorial-sql-editor/save-as-json.png)

4. Sélectionnez les données JSON dans l’éditeur et le copier.
5. Ouvrez un nouvel éditeur en appuyant sur **Ctrl + N**.
6. Les étapes précédentes montrent comment vous pouvez obtenir facilement les données au format correct pour terminer l’appel à la *setCustomer* procédure. Vous pouvez voir le code suivant utilise le même format JSON avec de nouvelles informations client afin que nous puissions tester le *setCustomer* procédure. L’instruction inclut la syntaxe pour déclarer le paramètre et exécutez de nouveau get et procédures. Vous pouvez coller les données copiées à partir de la section précédente et modifiez-le afin qu’il est identique à l’exemple suivant ou simplement Coller l’instruction suivante dans l’éditeur de requête.

   ```sql
   -- example to execute the stored procedure we just created
   declare @json nvarchar(max) =
   N'[
       {
           "CustomerId": 5,
           "Name": "Lucy",
           "Location": "Canada",
           "Email": "lucy0@adventure-works.com"
       }
   ]'

   EXECUTE dbo.setCustomer @json_val = @json
   GO

   EXECUTE dbo.getCustomer @ID = 5
   ```

7. Exécutez le script en appuyant sur **F5**. Le script insère un nouveau client et retourne les nouvelles informations du client au format JSON. Cliquez sur le résultat pour ouvrir une vue mise en forme.

   ![Résultat de test](./media/tutorial-sql-editor/test-result.png)

## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez appris comment :
> [!div class="checklist"]
> * Objets de schéma de recherche rapide
> * Modifier les données de la table 
> * L’écriture de script T-SQL à l’aide d’extraits de code
> * En savoir plus sur les détails de l’objet de base de données à l’aide d’aperçu de définition et d’atteindre la définition


Pour savoir comment activer la **cinq premières requêtes** widget, effectuer le didacticiel suivant :

> [!div class="nextstepaction"]
> [Activer le widget d’insight exemple requêtes lentes](tutorial-qds-sql-server.md)
