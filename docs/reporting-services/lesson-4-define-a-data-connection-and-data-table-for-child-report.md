---
title: 'Leçon 4 : définir une connexion de données et une table de données pour le rapport enfant | Microsoft Docs'
ms.custom: ''
ms.date: 05/18/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
applies_to:
- SQL Server 2016
ms.assetid: a6aa2c56-227c-43c5-a28e-c7104131ac5e
caps.latest.revision: 7
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d7aa269db14390739e0d9a161819b78793fd8474
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-4-define-a-data-connection-and-data-table-for-child-report"></a>Leçon 4 : définir une connexion de données et une table de données pour le rapport enfant
Après avoir créé le rapport parent, l'étape suivante consiste à créer une connexion de données et une table de données pour le rapport enfant. Dans ce didacticiel, la connexion de données s’établit avec la base de données AdventureWorks2014.  
  
### <a name="to-define-a-data-connection-and-datatable-by-adding-a-dataset-for-child-report"></a>Pour définir une connexion de données et un objet DataTable en ajoutant un DataSet (pour le rapport enfant)  
  
1.  Dans le menu **Site Web** , sélectionnez **Ajouter un nouvel élément**.  
  
2.  Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **DataSet** , puis sélectionnez **Ajouter**. Quand vous y êtes invité, vous devez ajouter l’élément au dossier **App_Code** en sélectionnant **Oui**.  
  
    Vous ajoutez ainsi un nouveau fichier XSD **DataSet2.xsd** au projet et ouvrez le Concepteur de DataSet.  
  
3.  À partir de la fenêtre Boîte à outils, faites glisser un contrôle **TableAdapter** dans l’aire de conception. Cette opération permet de lancer l’Assistant de configuration de **TableAdapter** .  
  
4.  Dans la page **Choisir votre connexion de données** , vous pouvez sélectionner la connexion que vous avez créée à la Leçon 2. Si vous l’avez déjà fait, sélectionnez **Suivant** et passez à l’étape 8. Sinon, sélectionnez **Nouvelle connexion**.  
  
5.  Dans la boîte de dialogue **Ajouter une connexion** , procédez comme suit :  
  
    1.  Dans la zone **Nom du serveur** , entrez le serveur sur lequel se trouve la base de données **AdventureWorks2014** .  
  
        L’instance SQL Server Express par défaut est **(local)\sqlexpress**.  
  
    2.  Dans la section **Connexion au serveur** , sélectionnez l’option qui permet d’accéder aux données. **Utiliser l’authentification Windows** est la valeur par défaut.  
  
    3.  Dans la zone déroulante **Sélectionner ou entrer un nom de base de données** , sélectionnez **AdventureWorks2014**.  
  
    4.  Sélectionnez **OK**, puis **Suivant**.  
  
6.  Si vous avez sélectionné **Utiliser l’authentification SQL Server** à l’étape 5 (b), choisissez s’il faut inclure les données sensibles dans la chaîne ou définir les informations dans votre code d’application.  
  
7.  Dans la page **Enregistrer la chaîne de connexion dans le fichier de configuration de l’application** , tapez le nom de la chaîne de connexion ou acceptez **AdventureWorks2014ConnectionString**par défaut. Sélectionnez **Suivant**.  
  
8.  Dans la page **Choisissez un type de commande** , sélectionnez **Utiliser des instructions SQL**, puis sélectionnez **Suivant**.  
  
9. Dans la page **Entrez une instruction SQL** , entrez la requête Transact-SQL suivante pour récupérer des données de la base de données **AdventureWorks2014** , puis sélectionnez **Suivant**.  
  
    ```  
    SELECT PurchaseOrderID, PurchaseOrderDetailID, OrderQty, ProductID, ReceivedQty, RejectedQty, StockedQty FROM Purchasing.PurchaseOrderDetail  
    ```  
  
    Vous pouvez également créer la requête en sélectionnant **Générateur de requêtes**, puis vérifier la requête en sélectionnant le bouton **Exécuter la requête** . Si la requête ne retourne pas les données attendues, c'est peut-être que vous utilisez une version antérieure d'AdventureWorks. Pour plus d’informations sur la façon d’obtenir l’exemple de base de données **AdventureWorks2014**, consultez [Exemples de bases de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases).  
  
10. Dans la page **Choisir les méthodes à générer** , décochez **Créer des méthodes pour envoyer directement des mises à jour à la base de données (GenerateDBDirectMethods)**, puis sélectionnez **Terminer**.  
  
    > [!WARNING]  
    > Veillez à décocher **Créer des méthodes pour envoyer directement des mises à jour à la base de données (GenerateDBDirectMethods)**  
  
    Vous avez maintenant terminé la configuration de l’objet ADO.NET [DataTable](http://msdn.microsoft.com/library/system.data.datatable.aspx) comme source de données de votre rapport. Dans la page du Concepteur de DataSet dans Visual Studio, vous devez voir l’objet **DataTable** que vous avez ajouté, qui répertorie les colonnes spécifiées dans la requête. DataSet2 contient les données de la table PurchaseOrderDetail, en fonction de la requête.  
  
11. Enregistrez le fichier.  
  
12. Pour afficher un aperçu des données, sélectionnez **Aperçu des données** dans le menu **Données** , puis sélectionnez **Aperçu**.  
  
## <a name="next-task"></a>Tâche suivante  
Vous venez de créer une connexion de données et une table de données pour le rapport enfant. Vous allez à présent concevoir le rapport enfant à l'aide de l'Assistant Rapport. Consultez [Leçon 5 : Concevoir le rapport enfant à l’aide de l’Assistant Rapport](../reporting-services/lesson-5-design-the-child-report-using-the-report-wizard.md).  
  

