---
title: Créer et déployer une base de données vide (Analysis Services AMO-TOM) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: afacae6ab8fc6f5a4f592e87758cca3142579c0a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-and-deploy-an-empty-database-analysis-services-amo-tom"></a>Créer et déployer une base de données vide (Analysis Services AMO-TOM)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
Un scénario courant de programmation pour AMO-TOM consiste à générer des bases de données et des modèles à la volée. Cet article vous guide tout au long des étapes de création d’une base de données. 

Pour les solutions tabulaires, il existe une correspondance univoque entre une base de données et un modèle, avec un modèle par base de données. Vous pouvez généralement spécifier un ou l’autre, et le moteur déduit l’objet manquant. 

Création et déploiement d’une base de données sont une tâche en trois parties : 

* Instancier une **base de données** de l’objet et définissez ses propriétés, y compris un nom. 

* Ajouter le **base de données** de l’objet à un **Server.Databases** collection. 

* Appeler le **mise à jour** méthode sur le **base de données** objet à enregistrer sur le **Server**. 

## <a name="code-example-create-empty-database"></a>Exemple de code : créer une base de données vide 

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
                    server.Databases.GetNewName("New Empty Database"); 
 
                // 
                // Instantiate a new  
                // Microsoft.AnalysisServices.Tabular.Database object. 
                // 
                var blankDatabase = new Database() 
                { 
                    Name = newDatabaseName, 
                    ID = newDatabaseName, 
                    CompatibilityLevel = 1200, 
                    StorageEngineUsed = StorageEngineUsed.TabularMetadata, 
                }; 
                // 
                // Add the new database object to the server's  
                // Databases connection and submit the changes 
                // with full expansion to the server. 
                // 
                server.Databases.Add(blankDatabase); 
                blankDatabase.Update(UpdateOptions.ExpandFull); 

                Console.Write("Database "); 
                Console.ForegroundColor = ConsoleColor.Yellow; 
                Console.Write(blankDatabase.Name); 
                Console.ResetColor(); 
                Console.WriteLine(" created successfully."); 
                Console.WriteLine(); 
            } 
            Console.WriteLine("Press Enter to close this console window."); 
            Console.ReadLine(); 
        } 
    } 
} 
```

## <a name="next-steps"></a>Étapes suivantes 

Une fois qu’une base de données est créé, vous pouvez ajouter des objets de modèle : 

- [Ajouter une source de données à un modèle tabulaire](../../analysis-services/tabular-model-programming-compatibility-level-1200/add-a-data-source-to-tabular-model-analysis-services-amo-tom.md)
- [Créer des tables, des partitions et des colonnes dans un modèle tabulaire](../../analysis-services/tabular-model-programming-compatibility-level-1200/create-tables-partitions-and-columns-in-a-tabular-model.md)
 
