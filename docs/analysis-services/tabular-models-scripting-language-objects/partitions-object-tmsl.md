---
title: Objet de partitions (TMSL) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tmsl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5ef6371ab2dca177aa02fa04883017c4c90d2c0b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="partitions-object-tmsl"></a>Objet de partitions (TMSL)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Définit une partition, ou une segmentation logique, de l’ensemble de lignes de table. Une partition se compose d’une requête SQL utilisée pour l’importation de données, pour des exemples de données dans l’environnement de modélisation, ou comme une requête de données complète pour passer de l’exécution des requêtes via DirectQuery.  
  
 Les propriétés sur la partition déterminent la façon dont les données en provenance de la table.  Dans la hiérarchie d’objets, l’objet parent d’une partition est un objet table.  
  
## <a name="object-definition"></a>Définition de l'objet  
 Tous les objets ont un ensemble commun de propriétés, y compris le nom, type, la description, une collection de propriétés et annotations. **Partition** objets ont également les propriétés suivantes.  
  
 Type  
 Le type de partition. Les valeurs valides sont numériques et sont les suivantes :  
  
-   Requête (1) : les données de cette partition est récupérée en exécutant une requête sur un **source de données**. Le **DataSource** doit être une source de données définie dans le fichier model.bim.  
  
-   Calculée (2) : les données de cette partition sont remplies par l’exécution d’une expression calculée.  
  
-   Aucun (3) : données de cette partition est remplie en envoyant un ensemble de lignes de données sur le serveur dans le cadre de l’opération d’actualisation.  
  
 mode  
 Définit le mode de requête de la partition. Les valeurs valides sont **importer**, **DirectQuery**, ou **par défaut** (héritée). Cette valeur est requise.  
  
|||  
|-|-|  
|**Importer**|Indique la requête, les demandes sont effectuées sur le moteur analytique de mémoire dans le stockage des données importées.|  
|**DirectQuery**|Passer en exécutant la requête à une base de données relationnelle externe. Le mode DirectQuery utilise des partitions pour fournir des exemples de données utilisés lors de la conception du modèle. Lors du déploiement sur un serveur de production, vous devez basculer vers la vue complète des données. Rappelez-vous que le mode DirectQuery nécessite une seule partition par table et une source de données par le modèle.|  
|**default**|Définir si vous souhaitez basculer entre les modes plus haut de l’arborescence d’objets au niveau du modèle ou la base de données. Lorsque vous choisissez par défaut, le mode de requête sera import et DirectQuery.|  
  
 source  
 Identifie l’emplacement des données à interroger. Les valeurs valides sont **requête, calculée**, ou **aucun**. Cette valeur est requise.  
  
|||  
|-|-|  
|**Aucun**|Utilisé pour le mode d’importation, où les données sont chargées et stockées en mémoire.|  
|**Requête**|Pour le mode DirectQuery, il s’agit d’une requête SQL exécutée sur la base de données relationnelle spécifiée dans le modèle **source de données**. Consultez [objet de sources de données &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-objects/datasources-object-tmsl.md).|  
|**Calculé**|Tables calculées proviennent d’une expression spécifiée lors de la table est créée. Cette expression est considéré comme la source de la partition créée pour la table calculée.|  
  
 DataView  
 Pour les partitions DirectQuery, une propriété supplémentaire dataView davantage Spécifie si la requête qui Récupère des données est un échantillon ou le jeu de données complet. Les valeurs valides sont **complète**, **exemple**, ou **par défaut** (héritée). Comme indiqué, les exemples sont utilisés uniquement pendant les données de modélisation et de test. Consultez [ajouter des exemples de données à un modèle DirectQuery en Mode Création](../../analysis-services/tabular-models/add-sample-data-to-a-directquery-model-in-design-mode.md) pour plus d’informations.  
  
## <a name="usage"></a>Utilisation  
 Les objets de partition sont utilisées dans [commande Alter &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl.md), [Créer commande &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/create-command-tmsl.md), [la commande CreateOrReplace &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl.md), [Commande delete &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/delete-command-tmsl.md), [commande Refresh &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl.md), et [commande MergePartitions &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/mergepartitions-command-tmsl.md).  
  
 Lors de la création, du remplacement ou de modification d’un objet de la partition, spécifiez toutes les propriétés en lecture-écriture de la définition d’objet. L’omission d’une propriété en lecture-écriture est considérée comme une suppression. Propriétés en lecture-écriture incluent le nom, la description, le mode et source.  
  
## <a name="examples"></a>Exemples  
 **Exemple 1** -une structure simple de partition d’une table (pas une table de faits).  
  
```  
"partitions": [  
          {  
            "name": "Customer",  
            "source": {  
              "query": "SELECT [dbo].[Customer].* FROM [dbo].[Customer]",  
              "dataSource": "SqlServer localhost FoodmartDB"  
            }  
]  
```  
  
 **Exemple 2** - partitionné les données de faits sont généralement basées sur une clause WHERE qui crée sans chevauchement de partitions pour les données de la même table.  
  
```  
"partitions": [  
          {  
            "name": "sales_fact_2015",  
            "source": {  
              "query": "SELECT [dbo].[sales_fact_2015].* FROM [dbo].[sales_fact_2015] WHERE [dbo].[sales_fact_2015].[Quarter]=4",                                                                                          
              "dataSource": "SqlServer localhost FoodmartDB"  
            },  
          }  
        ]  
```  
  
 **Exemple 3** -une table calculée selon une expression DAX.  
  
```  
"partitions": [  
          {  
            "name": "CalcTable1",  
            "source": {  
              "type": "calculated",  
              "expression": "'Product Class'"  
            },  
            }  
]  
```  
  
## <a name="full-syntax"></a>Syntaxe complète  
 Voici la représentation sous forme de schéma d’un objet partition.  
  
```  
"partitions": {  
                "type": "array",  
                "items": {  
                  "description": "Partition object of Tabular Object Model (TOM)",  
                  "type": "object",  
                  "properties": {  
                    "name": {  
                      "type": "string"  
                    },  
                    "description": {  
                      "anyOf": [  
                        {  
                          "type": "string"  
                        },  
                        {  
                          "type": "array",  
                          "items": {  
                            "type": "string"  
                          }  
                        }  
                      ]  
                    },  
                    "mode": {  
                      "enum": [  
                        "import",  
                        "directQuery",  
                        "default"  
                      ]  
                    },  
                    "dataView": {  
                      "enum": [  
                        "full",  
                        "sample",  
                        "default"  
                      ]  
                    },  
                    "source": {  
                      "anyOf": [  
                        {  
                          "description": "QueryPartitionSource object of Tabular Object Model (TOM)",  
                          "type": "object",  
                          "properties": {  
                            "type": {  
                              "enum": [  
                                "query",  
                                "calculated",  
                                "none"  
                              ]  
                            },  
                            "query": {  
                              "anyOf": [  
                                {  
                                  "type": "string"  
                                },  
                                {  
                                  "type": "array",  
                                  "items": {  
                                    "type": "string"  
                                  }  
                                }  
                              ]  
                            },  
                            "dataSource": {  
                              "type": "string"  
                            }  
                          },  
                          "additionalProperties": false  
                        },  
                        {  
                          "description": "CalculatedPartitionSource object of Tabular Object Model (TOM)",  
                          "type": "object",  
                          "properties": {  
                            "type": {  
                              "enum": [  
                                "query",  
                                "calculated",  
                                "none"  
                              ]  
                            },  
                            "expression": {  
                              "anyOf": [  
                                {  
                                  "type": "string"  
                                },  
                                {  
                                  "type": "array",  
                                  "items": {  
                                    "type": "string"  
                                  }  
                                }  
                              ]  
                            }  
                          },  
                          "additionalProperties": false  
                        }  
                      ]  
                    },  
                    "annotations": {  
                      "type": "array",  
                      "items": {  
                        "description": "Annotation object of Tabular Object Model (TOM)",  
                        "type": "object",  
                        "properties": {  
                          "name": {  
                            "type": "string"  
                          },  
                          "value": {  
                            "anyOf": [  
                              {  
                                "type": "string"  
                              },  
                              {  
                                "type": "array",  
                                "items": {  
                                  "type": "string"  
                                }  
                              }  
                            ]  
                          }  
                        },  
                        "additionalProperties": false  
                      }  
                    }  
                  },  
                  "additionalProperties": false  
                }  
              },  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Tabular Model Scripting Language &#40;TMSL&#41; Reference (Informations de référence sur TMSL &#40;Tabular Model Scripting Language&#41;)](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)  
  
  
