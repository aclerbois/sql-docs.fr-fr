---
title: Créer des Tables, des Partitions et des colonnes dans un modèle tabulaire | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c08fa45cbc4b77c151a002c3318dcc3dbeb2ee06
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-tables-partitions-and-columns-in-a-tabular-model"></a>Créer des Tables, des Partitions et des colonnes dans un modèle tabulaire
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
Dans un modèle tabulaire, une table se compose de lignes et de colonnes. Les lignes sont organisées en partitions pour prendre en charge l’actualisation des données incrémentielles. Une solution tabulaire peut prendre en charge plusieurs types de tables, en fonction de d'où proviennent les données :  

* Tables normales, d'où proviennent les données à partir d’une source de données relationnelle, via le fournisseur de données. 

* Tables envoyées, où données sont « (push) à la table par programme. 

* Tables calculées, où les données proviennent d’une expression DAX qui fait référence à un autre objet dans le modèle pour ses données.  

Dans l’exemple de code ci-dessous, nous allons définir une table normale. 

## <a name="required-elements"></a>Éléments requis 

Une table doit avoir au moins une partition. Une table normale doit avoir également au moins une colonne définie. 

Chaque partition doit avoir une Source de la spécification d’origine des données, mais la source peut être définie avec la valeur null. En règle générale, la source est une expression de requête qui définit une tranche de données dans le langage de requête de base de données appropriée. 

## <a name="code-example-create-a-table-column-parition"></a>Exemple de code : créer une table, la colonne, la partition

Les tables sont représentées par la classe de Table (dans l’espace de noms Microsoft.AnalysisServices.Tabular). 

Dans l’exemple ci-dessous, nous définirons une table normale ayant une partition liée à une source de données relationnelles et de quelques colonnes standards. Nous seront également soumettre les modifications au serveur et déclencher une actualisation des données qui met les données dans le modèle. Lorsque vous souhaitez charger des données à partir d’une base de données relationnelle SQL Server dans une solution tabulaire représente le scénario le plus courant. 


```
using System; 
using Microsoft.AnalysisServices; 
using Microsoft.AnalysisServices.Tabular; 
 
namespace TOMSamples 
{ 
    class Program 
    { 
        static void Main(string[] args) 
        { 
            // 
            // Connect to the local default instance of Analysis Services 
            // 
            string ConnectionString = "DataSource=localhost"; 
 
            // 
            // The using syntax ensures the correct use of the 
            // Microsoft.AnalysisServices.Tabular.Server object. 
            // 
            using (Server server = new Server()) 
            { 
                server.Connect(ConnectionString); 
 
                // 
                // Generate a new database name and use GetNewName 
                // to ensure the database name is unique. 
                // 
                string newDatabaseName = 
                    server.Databases.GetNewName("Database with a Table Definition"); 
 
                // 
                // Instantiate a new  
                // Microsoft.AnalysisServices.Tabular.Database object. 
                // 
                var dbWithTable = new Database() 
                { 
                    Name = newDatabaseName, 
                    ID = newDatabaseName, 
                    CompatibilityLevel = 1200, 
                    StorageEngineUsed = StorageEngineUsed.TabularMetadata, 
                }; 
 
                // 
                // Add a Microsoft.AnalysisServices.Tabular.Model object to the 
                // database, which acts as a root for all other Tabular metadata objects. 
                // 
                dbWithTable.Model = new Model() 
                { 
                    Name = "Tabular Data Model", 
                    Description = "A Tabular data model at the 1200 compatibility level." 
                }; 
 
                // 
                // Add a Microsoft.AnalysisServices.Tabular.ProviderDataSource object 
                // to the data Model object created in the previous step. The connection 
                // string of the data source object in this example  
                // points to an instance of the AdventureWorks2014 SQL Server database. 
                // 
                string dataSourceName = "SQL Server Data Source Example"; 
                dbWithTable.Model.DataSources.Add(new ProviderDataSource() 
                { 
                    Name = dataSourceName, 
                    Description = "A data source definition that uses explicit Windows credentials for authentication against SQL Server.", 
                    ConnectionString = "Provider=SQLNCLI11;Data Source=localhost;Initial Catalog=AdventureWorks2014;Integrated Security=SSPI;Persist Security Info=false", 
                    ImpersonationMode = Microsoft.AnalysisServices.Tabular.ImpersonationMode.ImpersonateAccount, 
                    Account = @".\Administrator", 
                    Password = "P@ssw0rd", 
                }); 
 
                //  
                // Add a table called Individual Customers to the data model 
                // with a partition that points to a [Sales].[vIndividualCustomer] view 
                // in the underlying data source. 
                // 
                dbWithTable.Model.Tables.Add(new Table() 
                { 
                    Name = dbWithTable.Model.Tables.GetNewName("Individual Customers"), 
                    Description = "Individual customers (names and email addresses) that purchase Adventure Works Cycles products online.", 
                    Partitions = { 
                        // 
                        // Create a single partition with a QueryPartitionSource for a query 
                        // that imports all customer rows from the underlying data source. 
                        // 
                        new Partition() { 
                            Name = "All Customers", 
                            Source = new QueryPartitionSource() { 
                                DataSource = dbWithTable.Model.DataSources[dataSourceName], 
                                Query = @"SELECT   [FirstName] 
                                                    ,[MiddleName] 
                                                    ,[LastName] 
                                                    ,[PhoneNumber]  
                                                    ,[EmailAddress] 
                                                    ,[City] 
                                        FROM [Sales].[vIndividualCustomer]", 
                            } 
                        } 
                    }, 
                    Columns = 
                    { 
                        // 
                       // DataColumn objects refer to regular table columns.  
                        // Each DataColumn object corresponds to a column in the underlying data source query. 
                        // 
                        new DataColumn() { 
                            Name = "FirstName", 
                            DataType = DataType.String, 
                            SourceColumn = "FirstName", 
                        }, 
                        new DataColumn() { 
                            Name = "MiddleName", 
                            DataType = DataType.String, 
                            SourceColumn = "MiddleName", 
                        }, 
                        new DataColumn() { 
                            Name = "LastName", 
                            DataType = DataType.String, 
                            SourceColumn = "LastName", 
                        }, 
                        new DataColumn() { 
                            Name = "PhoneNumber", 
                            DataType = DataType.String, 
                            SourceColumn = "PhoneNumber", 
                        }, 
                        new DataColumn() { 
                            Name = "EmailAddress", 
                            DataType = DataType.String, 
                            SourceColumn = "EmailAddress", 
                        }, 
                        new DataColumn() { 
                            Name = "City", 
                            DataType = DataType.String, 
                            SourceColumn = "City", 
                        }, 
                    } 
                }); 
 
                // 
                // Add the new database object to the server's  
                // Databases connection and submit the changes 
                // with full expansion to the server. 
                // 
                server.Databases.Add(dbWithTable); 
 
                //  
                // Request a full refresh to import the data from the data source and 
                // and perform any necessary recalculations. 
                // The refresh operation will be performed with the next 
                // invocation of Model.SaveChanges() or Database.Update(UpdateOptions.ExpandFull). 
                dbWithTable.Model.RequestRefresh(Microsoft.AnalysisServices.Tabular.RefreshType.Full); 
                dbWithTable.Update(UpdateOptions.ExpandFull); 
 
 
                Console.Write("Database "); 
                Console.ForegroundColor = ConsoleColor.Yellow; 
                Console.Write(dbWithTable.Name); 
                Console.ResetColor(); 
                Console.WriteLine(" created successfully."); 
 
                Console.WriteLine("The data model includes the following table definitions:"); 
                Console.ForegroundColor = ConsoleColor.Yellow; 
                foreach (Table tbl in dbWithTable.Model.Tables) 
                { 
                    Console.WriteLine("\tTable name:\t\t{0}", tbl.Name); 
                    Console.WriteLine("\ttbl description:\t{0}", tbl.Description); 
                } 
                Console.ResetColor(); 
                Console.WriteLine(); 
            } 
            Console.WriteLine("Press Enter to close this console window."); 
            Console.ReadLine(); 
        } 
    } 
} 
```

## <a name="partitions-in-a-table"></a>Partitions dans une table 

Les partitions sont représentées par un **Partition** classe (dans l’espace de noms Microsoft.AnalysisServices.Tabular). Le **Partition** classe expose un **Source** propriété p**artitionSource** type, qui fournit une abstraction sur les différentes approches pour la réception des données dans la partition. A **Partition** instance peut avoir un **Source** propriété comme null, indiquant que les données seront adressées dans la partition en envoyant des segments de données sur le serveur dans le cadre de pousser les API de données exposée par Analysis Services. Dans SQL Server 2016, **PartitionSource** classe possède deux classes dérivées qui représentent des méthodes pour lier des données à une partition : **QueryPartitionSource** et **CalculatedPartitionSource**. 

## <a name="columns-in-a-table"></a>Colonnes dans une table 

Les colonnes sont représentées par plusieurs classes dérivées de la base de **colonne** classe (dans l’espace de noms Microsoft.AnalysisServices.Tabular) : 

* **DataColumn** (pour les colonnes régulières de tables normales)
* **CalculatedColumn** (pour les colonnes soutenus par une expression DAX)
* **CalculatedTableColumn** (pour les colonnes standards dans les tables calculées)
* **RowNumberColumn** (type spécial de colonne créée par SSAS pour chaque table). 

## <a name="row-numbers-in-a-table"></a>Numéros de ligne dans une table 

Chaque **Table** objet sur un serveur a un **RowNumberColumn** utilisé pour l’indexation. Impossible de créer ou d’ajouter de manière explicite. La colonne est créée automatiquement lorsque vous enregistrez ou mettre à jour l’objet : 

* **base de données. SaveChanges** 

* **base de données. Update(ExpandFull)** 

Lorsque vous appelez une méthode, le serveur crée colonne de numéro de ligne automatiquement, qui sera visible en tant que **RowNumberColumn** collection de colonnes de la table. 

## <a name="calculated-tables"></a>Tables calculées 

Tables calculées proviennent d’une expression DAX qui utilise des données à partir des structures de données existantes dans le modèle ou à partir des liaisons hors ligne. Pour créer une table calculée par programme, procédez comme suit : 

* Créer un type générique **Table**. 

* Ajouter une partition avec une Source de type **CalculatedPartitionSource**, où la source est une expression DAX. La source de partition est ce qui différencie notamment une table normale à partir d’une table calculée. 

Lorsque vous enregistrez les modifications sur le serveur, le serveur retourne la liste déduite de **CalculatedTableColumns** (calculé tables sont composées de colonnes de table calculée), visibles via la collection de colonnes de la table. 

## <a name="next-steps"></a>Étapes suivantes

Passez en revue les classes utilisées pour la gestion des exceptions dans TOM : [gestion des erreurs dans TOM](../../analysis-services/tabular-model-programming-compatibility-level-1200/handling-errors-in-the-tom-api-analysis-services-amo-tom.md)
  
