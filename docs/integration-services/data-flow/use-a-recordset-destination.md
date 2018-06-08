---
title: Utiliser une destination de recordset | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset destination
ms.assetid: a7b143dc-8008-404f-83b0-b45ffbca6029
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 508deeca800867cfb24afc636d035f34f00ee98e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-a-recordset-destination"></a>Utiliser une destination de jeu d'enregistrements
  La destination d'ensemble d'enregistrements n'enregistre pas les données sur une source de données externe. Elle enregistre les données en mémoire dans un ensemble d’enregistrements stocké dans une variable de package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de type **Object** . Une fois que la destination d'ensemble d'enregistrements a sauvegardé les données, vous devez en général utiliser un conteneur de boucles Foreach avec l'énumérateur ADO Foreach pour traiter une par une les lignes de l'ensemble d'enregistrements. L'énumérateur ADO Foreach enregistre la valeur de chaque colonne de la ligne actuelle dans une variable de package distincte. Ensuite, les tâches que vous configurez à l'intérieur du conteneur de boucles Foreach lisent les valeurs contenues dans ces variables et effectuent certaines actions sur ces valeurs.  
  
 Vous pouvez utiliser la destination d'ensemble d'enregistrements dans de nombreux scénarios différents. Voici quelques exemples :  
  
-   Vous pouvez utiliser une tâche Envoyer un message et le langage d’expression [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour envoyer un message électronique personnalisé pour chaque ligne de l’ensemble d’enregistrements.  
  
-   Vous pouvez utiliser un composant Script configuré en tant que source à l'intérieur d'une tâche de flux de données pour lire les valeurs de colonne dans les colonnes du flux de données. Vous pouvez ensuite utiliser des transformations et des destinations pour transformer et enregistrer la ligne. Dans cet exemple, la tâche de flux de données s'exécute une fois pour chaque ligne.  
  
 Les sections suivantes décrivent tout d'abord le processus général d'utilisation de la destination d'ensemble d'enregistrements et présentent ensuite un exemple spécifique d'utilisation de la destination.  
  
## <a name="general-steps-to-using-a-recordset-destination"></a>Étapes générales d'utilisation d'une destination d'ensemble d'enregistrements  
 La procédure suivante résume les étapes requises pour enregistrer des données dans une destination d'ensemble d'enregistrements, puis l'utilisation du conteneur de boucles Foreach pour traiter chaque ligne.  
  
#### <a name="to-save-data-to-a-recordset-destination-and-process-each-row-by-using-the-foreach-loop-container"></a>Pour enregistrer des données dans une destination d'ensemble d'enregistrements et traiter chaque ligne à l'aide du conteneur de boucles Foreach  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], créez ou ouvrez un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Créez une variable qui contiendra l'ensemble d'enregistrements enregistré en mémoire par la destination d'ensemble d'enregistrements et définissez le type de la variable sur **Object**.  
  
3.  Créez des variables supplémentaires avec les types appropriés pour contenir les valeurs de chaque colonne de l'ensemble d'enregistrements que vous souhaitez utiliser.  
  
4.  Ajoutez et configurez le gestionnaire de connexions requis par la source de données que vous envisagez d'utiliser dans votre flux de données.  
  
5.  Ajoutez une tâche de flux de données au package et, sous l'onglet Flux de données du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , configurez des sources et des transformations pour le chargement et la transformation des données.  
  
6.  Ajoutez une destination d'ensemble d'enregistrements au flux de données et connectez-la aux transformations. Pour la propriété **VariableName** de la destination d'ensemble d'enregistrements, entrez le nom de la variable que vous avez créée pour contenir l'ensemble d'enregistrements.  
  
7.  Sous l'onglet Flux de contrôle du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , ajoutez un conteneur de boucles Foreach et connectez ce conteneur à la tâche de flux de données. Ouvrez ensuite l' **Éditeur de boucle Foreach** pour configurer le conteneur avec les paramètres suivants :  
  
    1.  Dans la page **Collection** , sélectionnez l'énumérateur ADO Foreach. Puis, pour **Variable source de l'objet ADO**, sélectionnez la variable qui contient l'ensemble d'enregistrements.  
  
    2.  Dans la page **Mappage de variables** , mappez l’index de base zéro de chaque colonne que vous souhaitez utiliser à la variable appropriée.  
  
         Sur chaque itération de la boucle, l'énumérateur remplit les variables avec les valeurs de colonne de la ligne actuelle.  
  
8.  À l'intérieur du conteneur de boucles Foreach, ajoutez et configurez des tâches destinées à traiter une par une les lignes de l'ensemble d'enregistrements en lisant les valeurs des variables.  
  
## <a name="example-of-using-the-recordset-destination"></a>Exemple d'utilisation de la destination d'ensemble d'enregistrements  
 Dans l'exemple suivant, la tâche de flux de données charge des informations relatives aux employés [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] de la table Sales.SalesPerson dans une destination d'ensemble d'enregistrements. Un conteneur de boucles Foreach lit ensuite les lignes de données une par une et appelle une tâche Envoyer un message. Cette tâche utilise des expressions pour envoyer un message électronique personnalisé à chaque vendeur au sujet du montant de sa prime.  
  
#### <a name="to-create-the-project-and-configure-the-variables"></a>Pour créer le projet et configurer les variables  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], créez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Dans le menu **SSIS** , sélectionnez **Variables**.  
  
3.  Dans la fenêtre **Variables** , créez les variables qui contiendront l'ensemble d'enregistrements et les valeurs de colonne de la ligne actuelle :  
  
    1.  Créez une variable nommée **BonusRecordset**et définissez son type sur **Object**.  
  
         La variable **BonusRecordset** contient le jeu d’enregistrements.  
  
    2.  Créez une variable nommée **EmailAddress**et définissez son type sur **String**.  
  
         La variable **EmailAddress** contient l’adresse e-mail du vendeur.  
  
    3.  Créez une variable nommée **FirstName**et définissez son type sur **String**.  
  
         La variable **FirstName** contient le prénom du vendeur.  
  
    4.  Créez une variable nommée **Bonus**et définissez son type sur **Double**.  
  
         La variable **Bonus** contient le montant de la prime du vendeur.  
  
#### <a name="to-configure-the-connection-managers"></a>Pour configurer les gestionnaires de connexions  
  
1.  Dans la zone Gestionnaires de connexions du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , ajoutez et configurez un nouveau gestionnaire de connexions OLE DB qui se connecte à l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] .  
  
     La source OLE DB dans la tâche de flux de données utilisera ce gestionnaire de connexions pour extraire les données.  
  
2.  Dans la zone Gestionnaires de connexions, ajoutez et configurez un nouveau gestionnaire de connexions SMTP qui se connecte à un serveur SMTP disponible.  
  
     La tâche Envoyer un message à l'intérieur du conteneur de boucles Foreach utilisera ce gestionnaire de connexions pour envoyer des courriers électroniques.  
  
#### <a name="to-configure-the-data-flow-and-the-recordset-destination"></a>Pour configurer le flux de données et la destination d'ensemble d'enregistrements  
  
1.  Sous l'onglet **Flux de contrôle** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , ajoutez une tâche de flux de données à l'aire de conception.  
  
2.  Sous l'onglet **Flux de données** tab, add an OLE DB source to the Flux de données task, and then open the **Éditeur de source OLE DB.**  
  
3.  Dans la page **Gestionnaire de connexions** de l'éditeur, configurez la source avec les paramètres suivants :  
  
    1.  Pour **Gestionnaire de connexions OLE DB**, sélectionnez le gestionnaire de connexions OLE DB que vous venez de créer.  
  
    2.  Pour **Mode d'accès aux données**, sélectionnez **Commande SQL**.  
  
    3.  Pour **Texte de la commande SQL**, entrez la requête suivante :  
  
        ```sql 
        SELECT     Person.Contact.EmailAddress, Person.Contact.FirstName, CONVERT(float, Sales.SalesPerson.Bonus) AS Bonus  
        FROM         Sales.SalesPerson INNER JOIN  
                              Person.Contact ON Sales.SalesPerson.SalesPersonID = Person.Contact.ContactID  
        ```  
  
        > [!NOTE]  
        >  Vous devez remplacer la valeur **currency** de la colonne Bonus par **float** pour pouvoir charger cette valeur dans une variable de package de type **Double**.  
  
4.  Sous l'onglet **Flux de données** , ajoutez une destination d'ensemble d'enregistrements et connectez-la à la source OLE DB.  
  
5.  Ouvrez l' **Éditeur de destination d'ensemble d'enregistrements**et configurez la destination avec les paramètres suivants :  
  
    1.  Dans l’onglet **Propriétés du composant** , pour la propriété **VariableName** , sélectionnez **User::BonusRecordset**.  
  
    2.  Sous l'onglet **Colonnes d'entrée** , sélectionnez les trois colonnes disponibles.  
  
#### <a name="to-configure-the-foreach-loop-container-and-run-the-package"></a>Pour configurer le conteneur de boucles Foreach et exécuter le package  
  
1.  Sous l'onglet **Flux de contrôle** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , ajoutez un conteneur de boucles Foreach et connectez-le à la tâche de flux de données.  
  
2.  Ouvrez l' **Éditeur de boucle Foreach**et configurez le conteneur avec les paramètres suivants :  
  
    1.  Sur la page **Collection** , pour **Énumérateur**, sélectionnez **Énumérateur ADO Foreach**, et pour **Variable source de l’objet ADO**, sélectionnez **User::BonusRecordset**.  
  
    2.  Sur la page **Mappage de variables** , mappez la variable **User::EmailAddress** à l’index 0, la variable **User::FirstName** à l’index 1 et la variable **User::Bonus** à l’index 2.  
  
3.  Sous l'onglet **Flux de contrôle** , à l'intérieur du conteneur de boucles Foreach, ajoutez une tâche Envoyer un message.  
  
4.  Ouvrez l' **Éditeur de tâche Envoyer un message**, puis dans la page **Courrier** , configurez la tâche avec les paramètres suivants :  
  
    1.  Pour **SmtpConnection**, sélectionnez le gestionnaire de connexions SMTP qui a été préalablement configuré.  
  
    2.  Pour **De**, entrez une adresse e-mail appropriée.  
  
         Si vous utilisez votre propre adresse de messagerie, vous pourrez vérifier que le package s'est exécuté avec succès. Vous recevrez des accusés de non remise pour les messages envoyés par la tâche Envoyer un message aux vendeurs fictifs de [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
    3.  Pour **À**, entrez une adresse e-mail par défaut.  
  
         Cette valeur ne sera pas utilisée, mais sera remplacée au moment de l'exécution par l'adresse de messagerie de chaque vendeur.  
  
    4.  Pour **Objet**, entrez « Votre prime annuelle ».  
  
    5.  Pour **MessageSourceType**, sélectionnez **Entrée directe**.  
  
5.  Dans la page **Expressions** de **l’Éditeur de tâche Envoyer un message**, cliquez sur le bouton de sélection (**...**) pour ouvrir **l’Éditeur d’expressions de la propriété**.  
  
6.  Dans l' **Éditeur d'expressions de la propriété**, entrez les informations suivantes :  
  
    1.  Pour **ToLine**, ajoutez l'expression suivante :  
  
        ```  
        @[User::EmailAddress]  
        ```  
  
    2.  Pour la propriété **MessageSource** , ajoutez l'expression suivante :  
  
        ```  
        "Dear " +  @[User::FirstName] + ": The amount of your bonus for this year is $" +  (DT_WSTR, 12) @[User::Bonus] + ". Thank you!"  
        ```  
  
7.  Exécutez le package.  
  
     Si vous avez spécifié un serveur SMTP valide et avez fourni votre propre adresse de messagerie, vous recevrez des accusés de non remise pour les messages envoyés par la tâche Envoyer un message aux vendeurs fictifs de [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
  
