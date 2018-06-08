---
title: 'Leçon 8 : créer un filtre de données | Microsoft Docs'
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
ms.assetid: 19ccbdba-e3da-40a4-b652-32c628cf32e5
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 5e6e2976cb21c2c6b1bd559282dcbfb1e403a9b3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-8-create-a-data-filter"></a>Leçon 8 : créer un filtre de données
Après avoir ajouté une action d'extraction dans le rapport parent, l'étape suivante consiste à créer un filtre de données pour la table de données que vous avez définie pour le rapport enfant.  
  
Vous pouvez créer un filtre de table **ou** un filtre de requête pour le rapport d’extraction. Cette leçon contient des instructions pour ces deux options.  
  
## <a name="table-based-filter"></a>Filtre de table  
Vous devez effectuer les tâches suivantes pour implémenter un filtre de table.  
  
-   Ajoutez une expression de filtre dans le tableau matriciel du rapport enfant.  
  
-   Créez une fonction qui sélectionne les données non filtrées dans la table **PurchaseOrderDetail** .  
  
-   Ajoutez un gestionnaire d’événements qui lie l’objet DataTable **PurchaseOrderDetail** au rapport enfant.  
  
### <a name="to-add-a-filter-expression-to-the-tablix-in-the-child-report"></a>Pour ajouter une expression de filtre dans le tableau matriciel du rapport enfant  
  
1.  Ouvrez le rapport enfant.  
  
2.  Sélectionnez un en-tête de colonne dans le tableau matriciel, cliquez avec le bouton droit sur la cellule grise qui apparaît au-dessus de l’en-tête de colonne, puis sélectionnez **Propriétés du tableau matriciel**.  
  
3.  Sélectionnez la page **Filtres** , puis **Ajouter**.  
  
4.  Dans le champ **Expression** , sélectionnez **ProductID** dans la liste déroulante. Il s'agit de la colonne à laquelle vous appliquez le filtre.  
  
5.  Sélectionnez l’opérateur égal (**=**) dans la liste déroulante **Opérateur** .  
  
6.  Sélectionnez le bouton d’expression situé en regard du champ **Valeur** , sélectionnez **Paramètres** dans la zone **Catégorie** , puis double-cliquez sur **productid** dans la zone **Valeurs** . Le champ **Définir l’expression pour : Valeur** doit maintenant contenir une expression semblable à **=Parameters!productid.Value**.  
  
7.  Sélectionnez **OK** , puis à nouveau **OK** dans la boîte de dialogue **Propriétés du tableau matriciel** .  
  
8.  Enregistrez le fichier .rdlc.  
  
### <a name="to-create-a-function-that-selects-unfiltered-data-from-the-purchaseordedetail-table"></a>Pour créer une fonction qui sélectionne les données non filtrées dans la table PurchaseOrdeDetail  
  
1.  Dans l'Explorateur de solutions, développez Default.aspx, puis double-cliquez sur Default.aspx.cs.  
  
2.  Créez une fonction qui accepte un paramètre, **productid**, de type entier et retourne un objet **datatable** , puis procédez comme suit.  
  
    1.  Crée une instance du dataset, **DataSet2**, qui a été créé à l’étape 2 de la [Leçon 4 : Définir une connexion de données et une table de données pour le rapport enfant](../reporting-services/lesson-4-define-a-data-connection-and-data-table-for-child-report.md).  
  
    2.  Crée une connexion à la base de données SqlServer pour exécuter la requête définie dans la **Leçon 4 : Définir une connexion de données et une table de données pour le rapport enfant**.  
  
    3.  La requête retourne des données non filtrées.  
  
    4.  Remplissez l'instance de DataSet avec des données non filtrées en exécutant la requête.  
  
    5.  Retourne la table de données **PurchaseOrderDetail** .  
  
        La fonction doit ressembler à celle ci-dessous. (Pour référence uniquement. Vous pouvez suivre le modèle de votre choix pour extraire les données nécessaires pour le rapport enfant.)  
  
        ```  
        /// <summary>  
            /// Function to query PurchaseOrderDetail table, fetch the  
            /// unfiltered data and bind it with the Child report  
            /// </summary>  
            /// <returns>A dataTable of type PurchaseOrderDetail</returns>  
            private DataTable GetPurchaseOrderDetail()  
            {  
                try  
                {  
                    //Create the instance for the typed dataset, DataSet2 which will   
                    //hold the [PurchaseOrderDetail] table details.  
                    //The dataset was created as part of the tutorial in Step 4.  
                    DataSet2 ds = new DataSet2();  
  
                    //Create a SQL Connection to the AdventureWorks2008 database using Windows Authentication.  
                    using (SqlConnection sqlconn = new SqlConnection("Data Source=.;Initial Catalog=Adventureworks2014;Integrated Security=SSPI"))  
                    {  
                        //Building the dynamic query with the parameter ProductID.  
                        SqlDataAdapter adap = new SqlDataAdapter("SELECT PurchaseOrderID, PurchaseOrderDetailID, OrderQty, ProductID, ReceivedQty, RejectedQty, StockedQty FROM Purchasing.PurchaseOrderDetail ", sqlconn);  
                        //Executing the QUERY and fill the dataset with the PurchaseOrderDetail table data.  
                        adap.Fill(ds, "PurchaseOrderDetail");  
                    }  
  
                    //Return the PurchaseOrderDetail table for the Report Data Source.  
                    return ds.PurchaseOrderDetail;  
                }  
                catch  
                {  
                    throw;  
                }  
            }  
        ```  
  
### <a name="to-add-an-event-handler-that-binds-the-purchaseorderdetail-datatable-to-the-child-report"></a>Pour ajouter un gestionnaire d'événements qui lie l'objet DataTable PurchaseOrderDetail au rapport enfant  
  
1.  Ouvrez Default.aspx en mode concepteur.  
  
2.  Cliquez avec le bouton droit sur le contrôle ReportViewer et sélectionnez **Propriétés**.  
  
3.  Dans la page **Propriétés** , sélectionnez l’icône **Événements** .  
  
4.  Double-cliquez sur l’événement **Extraction** .  
  
    Cette opération permet d'ajouter une section du gestionnaire d'événements dans le code, qui doit ressembler au bloc ci-dessous.  
  
    ```  
    protected void ReportViewer1_Drillthrough(object sender, Microsoft.Reporting.WebForms.DrillthroughEventArgs e)  
    {  
    }  
    ```  
  
5.  Terminez l'exécution du gestionnaire d'événements. Il doit inclure les fonctionnalités suivantes.  
  
    1.  Extraire la référence d’objet de rapport enfant du paramètre *DrillthroughEventArgs* .  
  
    2.  Appeler la fonction **GetPurchaseOrderDetail**.  
  
    3.  Lier l’objet DataTable **PurchaseOrderDetail** à la source de données correspondante du rapport.  
  
        Le code exécuté du gestionnaire d'événements doit ressembler à ce qui suit.  
  
        ```  
        protected void ReportViewer1_Drillthrough(object sender, Microsoft.Reporting.WebForms.DrillthroughEventArgs e)  
            {  
                try  
                {  
                     //Get the instance of the Target report.  
                    LocalReport report = (LocalReport)e.Report;  
  
                     //Binding the DataTable to the Child report dataset.  
                    //The name DataSet1 which can be located from,   
                    //Go to Design view of Child.rdlc, Click View menu -> Report Data  
                    //You'll see this name under DataSet2.  
                    report.DataSources.Add(new ReportDataSource("DataSet1", GetPurchaseOrderDetail()));  
                }  
                catch (Exception ex)  
                {  
                    Response.Write(ex.Message);  
                }  
            }  
        ```  
  
6.  Enregistrez le fichier.  
  
## <a name="query-filter"></a>Filtre de requête  
Vous devez effectuer les tâches suivantes pour implémenter un filtre de requête.  
  
-   Créez une fonction qui sélectionne les données filtrées dans la table **PurchaseOrderDetail** .  
  
-   Ajoutez un gestionnaire d’événements qui extrait les valeurs de paramètre et lie l’objet DataTable **PurchaseOrdeDetail** au rapport enfant.  
  
### <a name="to-create-a-function-that-selects-filtered-data-from-the-purchaseorderdetail-table"></a>Pour créer une fonction qui sélectionne les données filtrées dans la table PurchaseOrderDetail  
  
1.  Dans l'Explorateur de solutions, développez Default.aspx, puis double-cliquez sur Default.aspx.cs.  
  
2.  Créez une fonction qui accepte un paramètre, **productid**, de type entier et retourne un objet **datatable** , puis procédez comme suit.  
  
    1.  Crée une instance du dataset, **DataSet2**, qui a été créé à l’étape 2 de la [Leçon 4 : Définir une connexion de données et une table de données pour le rapport enfant](../reporting-services/lesson-4-define-a-data-connection-and-data-table-for-child-report.md).  
  
    2.  Créez une connexion à la base de données SqlServer pour exécuter la requête définie dans la **Leçon 4 : Définir une connexion de données et une table de données pour le rapport enfant**.  
  
    3.  La requête inclut un paramètre, **productid**, pour vérifier que les données retournées sont filtrées en fonction de l’élément **ProductID** sélectionné dans le rapport parent.  
  
    4.  Remplissez l'instance de DataSet avec les données filtrées en exécutant la requête.  
  
    5.  Retourne la table de données **PurchaseOrderDetail** .  
  
        La fonction doit ressembler à celle ci-dessous. (Pour référence uniquement. Vous pouvez suivre le modèle de votre choix pour extraire les données nécessaires pour le rapport enfant.)  
  
        ```  
        /// <summary>  
            /// Function to query PurchaseOrderDetail table and filter the  
            /// data for a specific ProductID selected in the Parent report.  
            /// </summary>  
            /// <param name="productid">Parameter passed from the Parent report to filter data.</param>  
            /// <returns>A dataTable of type PurchaseOrderDetail</returns>  
            private DataTable GetPurchaseOrderDetail(int productid)  
            {  
                try  
                {  
                    //Create the instance for the typed dataset, DataSet2 which will   
                    //hold the [PurchaseOrderDetail] table details.  
                    //The dataset was created as part of the tutorial in Step 4.  
                    DataSet2 ds = new DataSet2();  
  
                    //Create a SQL Connection to the AdventureWorks2008 database using Windows Authentication.  
                    using (SqlConnection sqlconn = new SqlConnection("Data Source=.;Initial Catalog=Adventureworks2014;Integrated Security=SSPI"))  
                    {  
                        //Building the dynamic query with the parameter ProductID.  
                        SqlCommand cmd = new SqlCommand("SELECT PurchaseOrderID, PurchaseOrderDetailID, OrderQty, ProductID, ReceivedQty, RejectedQty, StockedQty FROM Purchasing.PurchaseOrderDetail where ProductID = @ProductID", sqlconn);  
                  
                        // Sets the productid parameter.  
                        cmd.Parameters.Add((new SqlParameter("@ProductID", SqlDbType.Int)).Value = productid);  
  
                        SqlDataAdapter adap = new SqlDataAdapter(cmd);  
                        //Executing the QUERY and fill the dataset with the PurchaseOrderDetail table data.  
                        adap.Fill(ds, "PurchaseOrderDetail");  
                    }  
  
                    //Return the PurchaseOrderDetail table for the Report Data Source.  
                    return ds.PurchaseOrderDetail;  
                }  
                catch  
                {  
                    throw;  
                }  
            }  
        ```  
  
### <a name="to-add-an-event-handler-that-retrieves-parameter-values-and-binds-the-purchaseordedetail-datatable-to-the-child-report"></a>Pour ajouter un gestionnaire d'événements qui extrait les valeurs de paramètre et lie l'objet DataTable PurchaseOrderDetail au rapport enfant  
  
1.  Ouvrez Default.aspx en mode concepteur.  
  
2.  Cliquez avec le bouton droit sur le contrôle ReportViewer et sélectionnez **Propriétés**.  
  
3.  Dans le volet **Propriétés** , sélectionnez l’icône **Événements** .  
  
4.  Double-cliquez sur l’événement **Extraction** .  
  
    Cette opération permet d'ajouter une section du gestionnaire d'événements dans le code, qui doit ressembler à ce qui suit.  
  
    ```  
    protected void ReportViewer1_Drillthrough(object sender, Microsoft.Reporting.WebForms.DrillthroughEventArgs e)  
    {  
    }  
    ```  
  
5.  Terminez l'exécution du gestionnaire d'événements. Il doit inclure la fonctionnalité suivante.  
  
    1.  Extraire la référence d’objet de rapport enfant du paramètre *DrillthroughEventArgs* .  
  
    2.  Obtenez la liste des paramètres de rapport enfant de l'objet de rapport enfant extrait.  
  
    3.  Parcourir la collection du paramètre et récupérer la valeur du paramètre, **ProductID**, passée depuis le rapport parent.  
  
    4.  Appeler la fonction, **GetPurchaseOrderDetail**, et passer la valeur du paramètre **ProductID**.  
  
    5.  Lier l’objet DataTable **PurchaseOrderDetail** à la source de données correspondante du rapport.  
  
        Le code exécuté du gestionnaire d'événements doit ressembler à ce qui suit.  
  
        ```  
        protected void ReportViewer1_Drillthrough(object sender, Microsoft.Reporting.WebForms.DrillthroughEventArgs e)  
            {  
                try  
                {  
                    //Variable to store the parameter value passed from the MainReport.  
                    int productid = 0;  
  
                    //Get the instance of the Target report.  
                    LocalReport report = (LocalReport)e.Report;  
  
                    //Get all the parameters passed from the main report to the target report.  
                    //OriginalParametersToDrillthrough actually returns a Generic list of   
                    //type ReportParameter.  
                    IList<ReportParameter> list = report.OriginalParametersToDrillthrough;  
  
                    //Parse through each parameters to fetch the values passed along with them.  
                    foreach (ReportParameter param in list)  
                    {  
                        //Since we know the report has only one parameter and it is not a multivalued,   
                        //we can directly fetch the first value from the Values array.  
                        productid = Convert.ToInt32(param.Values[0].ToString());  
                    }  
  
                    //Binding the DataTable to the Child report dataset.  
                    //The name DataSet1 which can be located from,   
                    //Go to Design view of Child.rdlc, Click View menu -> Report Data  
                    //You'll see this name under DataSet2.  
                    report.DataSources.Add(new ReportDataSource("DataSet1", GetPurchaseOrderDetail(productid)));  
                }  
                catch (Exception ex)  
                {  
                    Response.Write(ex.Message);  
                }  
            }  
        ```  
  
6.  Enregistrez le fichier.  
  
## <a name="next-task"></a>Tâche suivante  
Vous venez de créer un filtre de données pour la table de données que vous avez définie pour le rapport enfant. Vous allez à présent générer et exécuter l'application de site Web. Consultez [Leçon 9 : Générer et exécuter l’application](../reporting-services/lesson-9-build-and-run-the-application.md).  
  
  
  

