---
title: Objet de sources de données (TMSL) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tmsl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1dd73918ca2d52cf38dba74455cf225a8c15ac3e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="datasources-object-tmsl"></a>Objet de sources de données (TMSL)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Définit une connexion à une source de données utilisée par le modèle, soit pendant l’importation pour ajouter des données au modèle, ou dans les requêtes directes via le mode DirectQuery.  Modèles en mode DirectQuery peuvent posséder une seule **DataSource** objet.  
  
 Sauf si vous créez, en remplaçant, ou la modification de l’objet de source de données lui-même, n’importe quelle source de données référencé dans votre script (comme dans le script de partition) doit être un existant **DataSource** objet dans votre modèle.  
  
## <a name="object-definition"></a>Définition de l'objet  
 Tous les objets ont un ensemble commun de propriétés, y compris le nom, type, la description, une collection de propriétés et annotations. **Source de données** objets ont également les propriétés suivantes.  
  
 Type  
 Type de DataSource. À l’heure actuelle, la seule valeur valide est le fournisseur (1) - chaîne de connexion normale.  
  
 connectionString  
 La chaîne de connexion qui spécifie le serveur et la base de données au minimum, mais peut également inclure d’autres propriétés prises en charge par le système SGBDR externe, tel qu’un compte d’utilisateur ou le fournisseur de données. Cette valeur est requise. Consultez [classe SqlConnectionStringBuilder](https://msdn.microsoft.com/en-us/library/ms254500\(v=vs.110\).aspx) pour plus d’informations sur SQL Server les propriétés de chaîne de connexion de base de données.  
  
 impersonationMode  
 Spécifie si les Analysis Services doivent emprunter l’identité de l’utilisateur qui demande la requête. Cette propriété est une valeur numérique qui spécifie les informations d’identification à utiliser pour l’emprunt d’identité. Les valeurs d'énumération sont les suivantes :  
  
-   Par défaut (1) - le serveur utilise la méthode d’emprunt d’identité qu’elle juge appropriée pour le contexte dans lequel l’emprunt d’identité est utilisé.  
  
-   ImpersonateAccount (2) : le serveur utilise le compte d’utilisateur spécifié.  
  
-   ImpersonateAnonymous (3) - le serveur utilise le compte d’utilisateur anonyme.  Cette option n’est pas recommandée, mais il est parfois utilisée dans les scénarios d’accès HTTP par les applications personnalisées qui gèrent l’authentification.  
  
-   ImpersonateCurrentUser (4) - le serveur utilise le compte d’utilisateur que le client se connecte en tant que.  
  
-   ImpersonateServiceAccount (5) - le serveur utilise le compte d’utilisateur que le serveur est en cours d’exécution en tant que.  
  
-   ImpersonateUnattendedAccount (6), le serveur utilise un compte d’utilisateur sans assistance. Il est utilisé pour les modèles Power Pivot ou tabulaire qui s’exécutent dans un environnement SharePoint.  
  
 Le mode DirectQuery peut utiliser impersonateCurrentuser si Analysis Services est configuré pour la délégation approuvée, ou  
                      impersonateServiceAccount si la demande de requête est effectuée dans le contexte de sécurité du compte de service Analysis Services. Consultez [configurer Analysis Services for Kerberos la délégation contrainte](../../analysis-services/instances/configure-analysis-services-for-kerberos-constrained-delegation.md).  
  
 account  
 Utilisé pour l’emprunt d’identité. Un compte Windows ou de la base de données qui dispose d’une connexion valide avec des autorisations de lecture sur la base de données externe.  
  
 password  
 Une chaîne chiffrée en fournissant le mot de passe du compte.  
  
 maxConnections  
 Nombre maximal de connexions ouvertes simultanément sur la source de données.  
  
 isolation  
 Le type d’isolation qui est utilisé lors de l’exécution des commandes sur la source de données. Les valeurs valides sont ReadCommitted (1) ou instantané (2).  
  
 délai d'expiration  
 Entier qui spécifie le délai d’attente en secondes pour les commandes exécutées sur la source de données.  
  
 fournisseur  
 Chaîne facultative qui identifie le nom du fournisseur de données managé utilisé pour la connexion à la base de données relationnelle, si n’est pas spécifiée sur la chaîne de connexion.  
  
## <a name="usage"></a>Utilisation  
 **Source de données** objets sont utilisés dans [commande Alter &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl.md), [Créer commande &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/create-command-tmsl.md), [la commande CreateOrReplace &#40; TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl.md), [commande Delete &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/delete-command-tmsl.md), [commande Refresh &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl.md), et [commande MergePartitions &#40;TMSL&#41;](../../analysis-services/tabular-models-scripting-language-commands/mergepartitions-command-tmsl.md).  
  
 A **DataSource** objet est une propriété d’un modèle, mais peut également être spécifié en tant que propriété d’un objet de base de données étant donné le mappage entre le modèle et la base de données.  Les partitions basées sur des requêtes SQL également spécifient un **source de données**, uniquement avec un ensemble réduit de propriétés.  
  
 Lors de la création, du remplacement ou de modification d’un objet de source de données, spécifiez toutes les propriétés en lecture-écriture de la définition d’objet. L’omission d’une propriété en lecture-écriture est considérée comme une suppression.  
  
## <a name="examples"></a>Exemples  
 **Exemple 1** -une connexion à un *FoodMart* base de données sur un ordinateur à une instance nommé de *Sales* sur un serveur réseau nommé *Server01*.  
  
```  
"dataSources": [  
      {  
        "name": "SqlServer Server01 FoodMart",  
        "connectionString": "Provider=SQLNCLI11;Data Source=Server01\Sales;Initial Catalog=Foodmart;Integrated Security=SSPI;Persist Security Info=false",  
        "impersonationMode": "impersonateServiceAccount",  
      }  
]  
```  
  
## <a name="full-syntax"></a>Syntaxe complète  
 Voici la représentation sous forme de schéma d’un objet de source de données d’un modèle.  
  
```  
"dataSources": {  
          "type": "array",  
          "items": {  
            "anyOf": [  
              {  
                "description": "ProviderDataSource object of Tabular Object Model (TOM)",  
                "type": "object",  
                "properties": {  
                  "name": {  
                    "type": "string"  
                  },  
                  "description": {  
                    "type": "string"  
                  },  
                  "type": {  
                    "enum": [  
                      "provider"  
                    ]  
                  },  
                  "connectionString": {  
                    "type": "string"  
                  },  
                  "impersonationMode": {  
                    "enum": [  
                      "default",  
                      "impersonateAccount",  
                      "impersonateAnonymous",  
                      "impersonateCurrentUser",  
                      "impersonateServiceAccount",  
                      "impersonateUnattendedAccount"  
                    ]  
                  },  
                  "account": {  
                    "type": "string"  
                  },  
                  "password": {  
                    "type": "string"  
                  },  
                  "maxConnections": {  
                    "type": "integer"  
                  },  
                  "isolation": {  
                    "enum": [  
                      "readCommitted",  
                      "snapshot"  
                    ]  
                  },  
                  "timeout": {  
                    "type": "integer"  
                  },  
                  "provider": {  
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
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Tabular Model Scripting Language &#40;TMSL&#41; Reference [Informations de référence sur TMSL &#40;Tabular Model Scripting Language&#41;]](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)   
 [Mode DirectQuery](../../analysis-services/tabular-models/directquery-mode-ssas-tabular.md)   
 [Configurer l’accès HTTP à Analysis Services sur Internet Information Services & #40 ; IIS & #41 ; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md)  
  
  
