---
title: Actualiser la commande (TMSL) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tmsl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: db07b1a5b4dc98b516e2f15e29aced3c74a57a16
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="refresh-command-tmsl"></a>Actualiser la commande (TMSL)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Traite les objets dans la base de données actuelle.   
**Actualiser** s’exécute toujours en parallèle, sauf si vous limiter à [de séquence de commandes &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/sequence-command-tmsl.md).  
  
 Vous pouvez remplacer certaines propriétés de certains objets pendant une opération d’actualisation des données :  
  
-   Modifier la **QueryDefinition** propriété d’un **Partition** objet pour importer des données à l’aide d’une expression de filtre d’à la volée.  
  
-   Fournissent des informations d’identification de source de données dans le cadre d’un **Actualiser** commande, dans le **ConnectionString** propriété d’un **source de données** objet. Cette approche peut être considéré comme plus fiable, comme les informations d’identification sont fournies et utilisées temporairement pour la durée de l’opération, plutôt que stockée.  
  
 Consultez les exemples dans cette rubrique pour obtenir une illustration de ces substitutions de propriété.  
  
> [!NOTE]  
>  Contrairement au traitement multidimensionnel, il n’existe aucune gestion spéciale des erreurs pour le traitement tabulaire de traitement.  

  
## <a name="request"></a>Demande  
 **Actualiser** prend une définition d’objet et le paramètre de type.  
  
```  
    {  
        "refresh": {  
            "description": "Parameters of Refresh command of Analysis Services JSON API",  
            "properties": {  
            "type": {  
                "enum": [  
                "full",  
                "clearValues",  
                "calculate",  
                "dataOnly",  
                "automatic",  
                "add",  
                "defragment"  
                ]  
            },  
            "objects": {  
. . .   
```  
  
 Le **type** paramètre définit une étendue sur l’opération de traitement.  
  
||||  
|-|-|-|  
|**Type d’actualisation**|**S’applique à**|**Description**|  
|complète|Base de données,<br />Table,<br />Partition|Pour toutes les partitions dans la partition, la table ou la base de données spécifiée, actualiser les données et recalculer toutes les éléments dépendants. Pour une partition de calcul, recalculer la partition et tous ses éléments dépendants.|  
|clearValues|Base de données,<br />Table,<br />Partition|Effacez les valeurs de cet objet et tous ses dépendants.|  
|calculer|Base de données,<br />Table,<br />Partition|Recalculer cet objet et tous ses éléments dépendants, mais seulement si nécessaire. Cette valeur ne force pas le recalcul, excepté pour les formules volatiles.|  
|dataOnly|Base de données,<br />Table,<br />Partition|Actualisez les données dans cet objet et effacez tous les dépendants.|  
|automatic|Base de données,<br />Table,<br />Partition|Si l’objet doit être actualisé et recalculé, actualiser et recalculer l’objet et tous ses éléments dépendants. S’applique si la partition est dans un état autre que Prêt.|  
|add|Partition|Ajoutez des données à cette partition et recalculez tous les dépendants. Cette commande est valide seulement pour les partitions régulières, et non pas pour les partitions de calcul.|  
|défragmenter|Base de données,<br />Table|Défragmenter les données dans la table spécifiée. Comme les données sont ajoutées ou supprimées d’une table, les dictionnaires de chaque colonne peuvent devenir pollués par des valeurs qui n’existent plus dans les valeurs actuelles des colonnes. L’option de défragmentation nettoie les valeurs dans les dictionnaires qui ne sont plus utilisées.|  
  
 Vous pouvez actualiser les objets suivants :  
  
 [Objet de base de données &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/database-object-tmsl.md) traiter une base de données.  
  
```  
{  
  "refresh": {  
    "type": "automatic",  
    "objects": [  
      {  
        "database": "AdventureWorksTabular1200"  
      }  
    ]  
  }  
}  
```  
  
 [Objet de tables &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/tables-object-tmsl.md) traiter une seule table.  
  
```  
{  
  "refresh": {  
    "type": "automatic",  
    "objects": [  
      {  
        "database": "AdventureWorksTabular1200",  
        "table": "Date"  
      }  
    ]  
  }  
}  
```  
  
 [Objet de partitions &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/partitions-object-tmsl.md) traiter une partition unique dans une table.  
  
```  
{  
  "refresh": {  
    "type": "automatic",  
    "objects": [  
      {  
        "database": "AdventureWorksTabular1200",  
        "table": "FactSalesQuota",  
        "partition": "FactSalesQuota"  
      },  
      {  
        "database": "AdventureWorksTabular1200",  
        "table": "FactSalesQuota",  
        "partition": "FactSalesQuota - 2011"  
      }  
    ]  
  }  
}  
```  
  
## <a name="response"></a>Réponse  
 Retourne un résultat vide lorsque la commande aboutit. Sinon, une exception XMLA est retournée.  
  
## <a name="examples"></a>Exemples  
 Substituer les deux le **ConnectionString** et définition d’une partition de la requête.  
  
```  
{   
    "refresh": {   
        "type": "data",   
        "objects": [   
            {   
                "database": "tmtestdb",   
                "table": "sales"   
            },   
            {   
                "database": "tmtestdb",   
                "table": "customer"   
            }   
        ],   
        "overrides": [   
            {   
                "dataSources": [ // Bindings for Data Sources   
                    {   
                        "object": {   
                            "database": "tmtestdb",   
                            "dataSource": "contoso"   
                        },   
                        "dataSource": {   
                        "connectionString": "Provider=SQLNCLI11;Data Source=localhost;Initial Catalog=contoso;Integrated Security=SSPI;Persist Security Info=false"   
"   
                        }   
                    }   
                ],   
                "partitions": [ // Bindings for Partitions   
                    {   
                        "object": {   
                            "database": "tmtestdb",   
                            "table": "sales",   
                            "partition": "2015"   
                        },   
                        "partition": {   
                            "source": {   
                                "query": [  
                                     "SELECT sales.salesamount, customer.customername FROM sales",  
                                     "JOIN customer on custKey = sales.custkey",  
                                     "JOIN date on date.DateKey = customer.OrderDate",  
                                     "WHERE date.CalendarYear='2015'"  
                                  ],  
                            }   
                        }   
                    }   
                ]   
            }   
        ]   
    }   
}   
```  
  
 Étendue particulière remplace en définissant le paramètre de type un **dataOnly** actualisation des métadonnées restent intacte.  
  
```  
{   
        "refresh" : {   
            "type" : "dataOnly",   
            "objects" : [   
                {   
                    "database" : "TMTestDB",   
                    "table" : "Customer"   
                },   
                {   
                    "database" : "TMTestDB",   
                    "table" : "Sales"   
                }   
            ],   
            "overrides" : [{   
                "scope" : {   
                    "database" : "TMTestDB",   
                    "table" : "Sales"   
                },   
                "dataSources" : [{   
                    "originalObject" : {   
                        "dataSource" : "SqlServer sqlcldb2 AS_foodmart_2000"   
                    },   
                    "connectionString" : "Provider=SQLNCLI11;Data Source=sqlcldb2;Initial Catalog=AS_foodmart_2000;Integrated Security=SSPI;Persist Security Info=false"   
                }]   
            }]   
        }   
    }   
```  
  
## <a name="usage-endpoints"></a>Utilisation (points de terminaison)  
 Cet élément de commande est utilisé dans une instruction de la [exécuter une méthode &#40;XMLA&#41; ](../../analysis-services/xmla/xml-elements-methods-execute.md) appel via un point de terminaison XMLA exposée comme suit :  
  
-   Comme une fenêtre XMLA dans SQL Server Management Studio (SSMS)  
  
-   En tant que fichier d’entrée dans le **invoke-ascmd** applet de commande PowerShell  
  
-   En tant qu’entrée pour un travail de l’Agent SQL Server ou de tâche SSIS  
  
 Vous pouvez générer un script prêts à l’emploi pour cette commande à partir de SSMS.  Par exemple, vous pouvez cliquer sur le **Script** dans une boîte de dialogue de traitement.  
  
 Le [ \[MS-SSAS-T\]: QL Server tabulaires Analysis Services (SQL Server technique Protocol)](http://go.microsoft.com/fwlink/p/?LinkId=784855) document inclut section 3.1.5.2.2 qui décrit la structure des objets et les commandes de métadonnées sous forme de tableau JSON. Actuellement, ce document traite des commandes et fonctions non encore implémentées dans un script TMSL. Reportez-vous à la rubrique ([Tabular Model Scripting Language &#40;TMSL&#41; référence](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)) pour obtenir des éclaircissements sur ce qui est pris en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Tabular Model Scripting Language &#40;TMSL&#41; Reference [Informations de référence sur TMSL &#40;Tabular Model Scripting Language&#41;]](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)   
 [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md)  
  
  
