---
title: Objet de tables (TMSL) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tmsl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: eb8948ca9c51bebc39bb93fbe44bed1afc5be38b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tables-object-tmsl"></a>Objet de tables (TMSL)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Définit les tables contenues dans un modèle. Tables d’un modèle sont soit liés à des tables dans une base de données externe à partir de laquelle les données sont importées ou interrogées ou une table calculée est construit à partir d’une expression DAX. Dans une table, un ou plusieurs **Partition** objets décrivent la source de données.  Entre les tables, un **relation** objet spécifie la cardinalité, direction du filtrage et autres propriétés de la relation.  
  
## <a name="object-definition"></a>Définition d’objet  
 Tous les objets ont un ensemble commun de propriétés, y compris le nom, type, la description, une collection de propriétés et annotations. **Table** objets ont également les propriétés suivantes.  
  
 dataCategory  
 Spécifie le type de Table, généralement gauche n’est pas spécifié. Les valeurs valides sont 0 - inconnu, 1 - heure, 2 - mesure, 3 - autres, 5 - QUANTITATIVE, 6-comptes, 7 - clients, produits-8, 9 - scénario, l’utilitaire-10, 11 -, 12 - taux de devise, canal 13 -, 4 - PROMOTION, 15 - organisation, 16 - nomenclature, 17 – GEOGRAPHY.  
  
 isHidden  
 Valeur booléenne qui indique si la table est traitée comme masqué par les outils de visualisation de clients.  
True si la Table est traitée comme masquée ; sinon, false.  
  
 colonnes  
 Représente une colonne dans une Table. C’est un enfant d’un objet Table. Chaque colonne a un nombre de propriétés définies sur ce dernier qui influencent la manière dont les applications clientes visualiser les données dans la colonne.  
  
 mesures  
 Représente une valeur qui est calculée en fonction d’une expression. C’est un enfant d’un objet Table.  
  
 hiérarchies  
 Représente une collection de niveaux qui fournissent un chemin d’exploration hiérarchique logique pour les applications clientes. C’est un enfant d’un objet Table.  
  
## <a name="usage"></a>Utilisation  
 Objets de table sont utilisés dans [commande Alter &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl.md), [Créer commande &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/create-command-tmsl.md), [la commande CreateOrReplace &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl.md), [Commande delete &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/delete-command-tmsl.md), [commande Refresh &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl.md), et [commande MergePartitions &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/mergepartitions-command-tmsl.md).  
  
 Lors de la création, du remplacement ou de modification d’un objet table, spécifiez toutes les propriétés en lecture-écriture de la définition d’objet. L’omission d’une propriété en lecture-écriture est considérée comme une suppression.  
  
## <a name="condensed-syntax"></a>Syntaxe condensée  
 Définitions d’objets de table sont complexes. Cette syntaxe réduit les propriétés et les objets pour vous donner un aperçu général les parties principales.  
  
```  
"tables": {  
          "type": "array",  
          "items": {  
            "description": "Table object of Tabular Object Model (TOM)",  
            "type": "object",  
            "properties": {    
              "dataCategory": {  },  
              "description": {  },  
              "isHidden": {  },  
              "partitions": {  },  
              "annotations": {  },  
              "columns": {  },  
              "measures": {   },  
              "hierarchies": {  },  
```  
  
## <a name="full-syntax"></a>Syntaxe complète  
 Voici la représentation sous forme de schéma d’un objet de tables d’un modèle. Pour réduire la taille de cette définition, les objets de Partition sont décrites ailleurs. Consultez [objet de Partitions &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-objects/partitions-object-tmsl.md).  
  
```  
"tables": {  
          "type": "array",  
          "items": {  
            "description": "Table object of Tabular Object Model (TOM)",  
            "type": "object",  
            "properties": {  
              "name": {  
                "type": "string"  
              },  
              "dataCategory": {  
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
              "isHidden": {  
                "type": "boolean"  
              },  
              "partitions":  {   
                 },  
              "columns": {  
                "type": "array",  
                "items": {  
                  "anyOf": [  
                    {  
                      "description": "DataColumn object of Tabular Object Model (TOM)",  
                      "type": "object",  
                      "properties": {  
                        "name": {  
                          "type": "string"  
                        },  
                        "dataType": {  
                          "enum": [  
                            "automatic",  
                            "string",  
                            "int64",  
                            "double",  
                            "dateTime",  
                            "decimal",  
                            "boolean",  
                            "binary",  
                            "unknown",  
                            "variant"  
                          ]  
                        },  
                        "dataCategory": {  
                          "type": "string"  
                        },  
                        "description": {  
                          "type": "string"  
                        },  
                        "isHidden": {  
                          "type": "boolean"  
                        },  
                        "isUnique": {  
                          "type": "boolean"  
                        },  
                        "isKey": {  
                          "type": "boolean"  
                        },  
                        "isNullable": {  
                          "type": "boolean"  
                        },  
                        "alignment": {  
                          "enum": [  
                            "default",  
                            "left",  
                            "right",  
                            "center"  
                          ]  
                        },  
                        "tableDetailPosition": {  
                          "type": "integer"  
                        },  
                        "isDefaultLabel": {  
                          "type": "boolean"  
                        },  
                        "isDefaultImage": {  
                          "type": "boolean"  
                        },  
                        "summarizeBy": {  
                          "enum": [  
                            "default",  
                            "none",  
                            "sum",  
                            "min",  
                            "max",  
                            "count",  
                            "average",  
                            "distinctCount"  
                          ]  
                        },  
                        "type": {  
                          "enum": [  
                            "data",  
                            "calculated",  
                            "rowNumber",  
                            "calculatedTableColumn"  
                          ]  
                        },  
                        "formatString": {  
                          "type": "string"  
                        },  
                        "isAvailableInMdx": {  
                          "type": "boolean"  
                        },  
                        "keepUniqueRows": {  
                          "type": "boolean"  
                        },  
                        "displayOrdinal": {  
                          "type": "integer"  
                        },  
                        "sourceProviderType": {  
                          "type": "string"  
                        },  
                        "displayFolder": {  
                          "type": "string"  
                        },  
                        "sourceColumn": {  
                          "type": "string"  
                        },  
                        "sortByColumn": {  
                          "type": "string"  
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
                    },  
                    {  
                      "description": "CalculatedTableColumn object of Tabular Object Model (TOM)",  
                      "type": "object",  
                      "properties": {  
                        "name": {  
                          "type": "string"  
                        },  
                        "dataType": {  
                          "enum": [  
                            "automatic",  
                            "string",  
                            "int64",  
                            "double",  
                            "dateTime",  
                            "decimal",  
                            "boolean",  
                            "binary",  
                            "unknown",  
                            "variant"  
                          ]  
                        },  
                        "dataCategory": {  
                          "type": "string"  
                        },  
                        "description": {  
                          "type": "string"  
                        },  
                        "isHidden": {  
                          "type": "boolean"  
                        },  
                        "isUnique": {  
                          "type": "boolean"  
                        },  
                        "isKey": {  
                          "type": "boolean"  
                        },  
                        "isNullable": {  
                          "type": "boolean"  
                        },  
                        "alignment": {  
                          "enum": [  
                            "default",  
                            "left",  
                            "right",  
                            "center"  
                          ]  
                        },  
                        "tableDetailPosition": {  
                          "type": "integer"  
                        },  
                        "isDefaultLabel": {  
                          "type": "boolean"  
                        },  
                        "isDefaultImage": {  
                          "type": "boolean"  
                        },  
                        "summarizeBy": {  
                          "enum": [  
                            "default",  
                            "none",  
                            "sum",  
                            "min",  
                            "max",  
                            "count",  
                            "average",  
                            "distinctCount"  
                          ]  
                        },  
                        "type": {  
                          "enum": [  
                            "data",  
                            "calculated",  
                            "rowNumber",  
                            "calculatedTableColumn"  
                          ]  
                        },  
                        "formatString": {  
                          "type": "string"  
                        },  
                        "isAvailableInMdx": {  
                          "type": "boolean"  
                        },  
                        "keepUniqueRows": {  
                          "type": "boolean"  
                        },  
                        "displayOrdinal": {  
                          "type": "integer"  
                        },  
                        "sourceProviderType": {  
                          "type": "string"  
                        },  
                        "displayFolder": {  
                          "type": "string"  
                        },  
                        "isNameInferred": {  
                          "type": "boolean"  
                        },  
                        "isDataTypeInferred": {  
                          "type": "boolean"  
                        },  
                        "sourceColumn": {  
                          "type": "string"  
                        },  
                        "sortByColumn": {  
                          "type": "string"  
                        },  
                        "columnOriginTable": {  
                          "type": "string"  
                        },  
                        "columnOriginColumn": {  
                          "type": "string"  
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
                    },  
                    {  
                      "description": "CalculatedColumn object of Tabular Object Model (TOM)",  
                      "type": "object",  
                      "properties": {  
                        "name": {  
                          "type": "string"  
                        },  
                        "dataType": {  
                          "enum": [  
                            "automatic",  
                            "string",  
                            "int64",  
                            "double",  
                            "dateTime",  
                            "decimal",  
                            "boolean",  
                            "binary",  
                            "unknown",  
                            "variant"  
                          ]  
                        },  
                        "dataCategory": {  
                          "type": "string"  
                        },  
                        "description": {  
                          "type": "string"  
                        },  
                        "isHidden": {  
                          "type": "boolean"  
                        },  
                        "isUnique": {  
                          "type": "boolean"  
                        },  
                        "isKey": {  
                          "type": "boolean"  
                        },  
                        "isNullable": {  
                          "type": "boolean"  
                        },  
                        "alignment": {  
                          "enum": [  
                            "default",  
                            "left",  
                            "right",  
                            "center"  
                          ]  
                        },  
                        "tableDetailPosition": {  
                          "type": "integer"  
                        },  
                        "isDefaultLabel": {  
                          "type": "boolean"  
                        },  
                        "isDefaultImage": {  
                          "type": "boolean"  
                        },  
                        "summarizeBy": {  
                          "enum": [  
                            "default",  
                            "none",  
                            "sum",  
                            "min",  
                            "max",  
                            "count",  
                            "average",  
                            "distinctCount"  
                          ]  
                        },  
                        "type": {  
                          "enum": [  
                            "data",  
                            "calculated",  
                            "rowNumber",  
                            "calculatedTableColumn"  
                          ]  
                        },  
                        "formatString": {  
                          "type": "string"  
                        },  
                        "isAvailableInMdx": {  
                          "type": "boolean"  
                        },  
                        "keepUniqueRows": {  
                          "type": "boolean"  
                        },  
                        "displayOrdinal": {  
                          "type": "integer"  
                        },  
                        "sourceProviderType": {  
                          "type": "string"  
                        },  
                        "displayFolder": {  
                          "type": "string"  
                        },  
                        "isDataTypeInferred": {  
                          "type": "boolean"  
                        },  
                        "expression": {  
                          "type": "string"  
                        },  
                        "sortByColumn": {  
                          "type": "string"  
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
                  ]  
                }  
              },  
              "measures": {  
                "type": "array",  
                "items": {  
                  "description": "Measure object of Tabular Object Model (TOM)",  
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
                    },  
                    "formatString": {  
                      "type": "string"  
                    },  
                    "isHidden": {  
                      "type": "boolean"  
                    },  
                    "isSimpleMeasure": {  
                      "type": "boolean"  
                    },  
                    "displayFolder": {  
                      "type": "string"  
                    },  
                    "kpi": {  
                      "description": "KPI object of Tabular Object Model (TOM)",  
                      "type": "object",  
                      "properties": {  
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
                        "targetDescription": {  
                          "type": "string"  
                        },  
                        "targetExpression": {  
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
                        "targetFormatString": {  
                          "type": "string"  
                        },  
                        "statusGraphic": {  
                          "type": "string"  
                        },  
                        "statusDescription": {  
                          "type": "string"  
                        },  
                        "statusExpression": {  
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
                        "trendGraphic": {  
                          "type": "string"  
                        },  
                        "trendDescription": {  
                          "type": "string"  
                        },  
                        "trendExpression": {  
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
              "hierarchies": {  
                "type": "array",  
                "items": {  
                  "description": "Hierarchy object of Tabular Object Model (TOM)",  
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
                    "isHidden": {  
                      "type": "boolean"  
                    },  
                    "displayFolder": {  
                      "type": "string"  
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
                    },  
                    "levels": {  
                      "type": "array",  
                      "items": {  
                        "description": "Level object of Tabular Object Model (TOM)",  
                        "type": "object",  
                        "properties": {  
                          "ordinal": {  
                            "type": "integer"  
                          },  
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
                          "column": {  
                            "type": "string"  
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
                    }  
                  },  
                  "additionalProperties": false  
                }  
              }  
            },  
            "additionalProperties": false  
          }  
        }  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Tabular Model Scripting Language &#40;TMSL&#41; Reference (Informations de référence sur TMSL &#40;Tabular Model Scripting Language&#41;)](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)  
  
  
