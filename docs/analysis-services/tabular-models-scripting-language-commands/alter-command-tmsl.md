---
title: Modifier la commande (TMSL) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tmsl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6dc7ba58ce3e5db228046324c17c91719db35d0b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="alter-command-tmsl"></a>Commande ALTER (TMSL)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Modifie un objet existant, mais pas ses enfants, sur une instance d’Analysis Services en mode tabulaire.  Si l’objet n’existe pas, la commande génère une erreur.  
  
 Utilisez **Alter** commande ciblé ou de mise à jour, telles que la définition d’une propriété sur une table sans avoir à spécifier toutes les colonnes ainsi. Cette commande est similaire à **CreateOrReplace**, mais sans le besoin d’avoir à fournir une définition d’objet complet.  
  
 Pour les objets dont les propriétés en lecture-écriture, si vous spécifiez une propriété en lecture-écriture, vous devez spécifier toutes les valeurs existants ou en utilisant le nouveau. Vous pouvez utiliser PowerShell AMO pour obtenir une liste de propriétés. 
  
## <a name="request"></a>Demande  
 **ALTER** n’a pas d’attributs. Entrées incluent l’objet doit être modifié, suivie de la définition de l’objet modifié.  
  
 L’exemple suivant illustre la syntaxe de la modification d’une propriété sur un objet partition. Le chemin d’accès de l’objet établit quelle partition objet doit être modifié via les paires nom-valeur des objets parents. La deuxième entrée est un objet de partition qui spécifie la nouvelle valeur de propriété.  
  
```  
{   
  "alter": {   
    "object": {   
      "database": "\<database-name>",   
      "table": "\<table-name>",   
      "partition": "\<partition-name>"   
    },   
    "partition": {   
      "name": "\<new-partition-name>",   
       . . .  << other properties   
    }   
  }   
}   
```  
  
 La structure de la requête varie en fonction de l’objet. **ALTER** peut être utilisé avec les objets suivants :  
  
 [Objet de base de données &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/database-object-tmsl.md) renommer une base de données.  
  
```  
"alter": {   
    "object": {   
      "database": "\<database-name>"  
    },   
    "database": {   
      "name": "\<new-database-name>",   
    }   
  }   
```  
  
 [Objet de sources de données &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/datasources-object-tmsl.md) renommer une connexion, qui est un objet enfant de la base de données.  
  
```  
{   
   "alter":{   
      "object":{   
         "database":"AdventureWorksTabular1200",  
         "dataSource":"SqlServer localhost AdventureworksDW2016"  
      },  
      "dataSource":{   
         "name":"A new connection name"  
      }  
   }  
}  
```  
  
 [Objet de tables &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/tables-object-tmsl.md) consultez **exemple 1** ci-dessous.  
  
 [Objet de partitions &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/partitions-object-tmsl.md) consultez **exemple 2** ci-dessous.  
  
 [Objet rôles &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl.md) modifier une propriété sur un objet de rôle.  
  
```  
{   
   "alter":{   
      "object":{   
         "database":"AdventureWorksTabular1200",  
         "role":"DataReader"  
      },  
      "role":{   
         "name":"New Name"  
      }  
   }  
}  
```  
  
## <a name="response"></a>Réponse  
 Retourne un résultat vide lorsque la commande aboutit. Sinon, une exception XMLA est retournée.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent le script que vous pouvez exécuter dans une fenêtre XMLA dans Management Studio ou l’utiliser en tant qu’entrée dans [applet de commande Invoke-ASCmd](../../analysis-services/powershell/invoke-ascmd-cmdlet.md) dans PowerShell AMO.  
  
 **Exemple 1** -ce script modifie la propriété de nom sur une table.  
  
```  
{   
  "alter": {   
    "object": {   
      "database": "AdventureWorksDW2016",   
      "table": "DimDate"  
    },   
    "table": {   
      "name": "DimDate2"  
    }   
  }   
}  
```  
  
 En supposant qu’une instance nommée locale (nom de l’instance est « tableau ») et un fichier JSON avec le script alter, cette commande remplace un nom de la table DimDate pour DimDate2 :  
  
 `invoke-ascmd -inputfile '\\my-computer\my-shared-folder\altertablename.json' -server 'localhost\Tabular'`  
  
 **Exemple 2** --le script suivant renomme une partition, par exemple à la fin de l’année lors de l’année en cours devient l’année précédente. Veillez à spécifier toutes les propriétés. Si vous laissez **source** n’est pas spécifié, il peut inhiber toutes vos définitions de partition existant.  
  
 Sauf si vous créez, en remplaçant, ou la modification de l’objet de source de données lui-même, n’importe quelle source de données référencé dans votre script (par exemple, dans le script de partition ci-dessous) doit être un existant **DataSource** objet dans votre modèle. Si vous devez modifier la source de données, cela comme une étape distincte.  
  
```  
{   
  "alter": {   
    "object": {   
      "database": "InternetSales",   
      "table": "DimDate",  
      "partition": "CurrentYear"  
    },   
    "partition": {   
      "name": "Year2009",  
       "source": {  
             "query":  "SELECT [dbo].[DimDate].* FROM [dbo].[DimDate] WHERE [dbo].[DimDate].CalendarYear = 2009",  
              "dataSource": "SqlServer localhost AdventureworksDW2016"  
        }  
    }   
  }   
}  
```  
  
## <a name="usage-endpoints"></a>Utilisation (points de terminaison)  
 Cet élément de commande est utilisé dans une instruction de la [exécuter une méthode &#40;XMLA&#41; ](../../analysis-services/xmla/xml-elements-methods-execute.md) appel via un point de terminaison XMLA exposée comme suit :  
  
-   Comme une fenêtre XMLA dans SQL Server Management Studio (SSMS)  
  
-   En tant que fichier d’entrée dans le **invoke-ascmd** applet de commande PowerShell  
  
-   En tant qu’entrée pour un travail de l’Agent SQL Server ou de tâche SSIS  
  
 Impossible de générer un script prêts à l’emploi pour cette commande à partir de SSMS. Au lieu de cela, vous pouvez commencer par un exemple ou écrire votre propre.  
  
 Le [ \[MS-SSAS-T\]: QL Server tabulaires Analysis Services (SQL Server technique Protocol)](http://go.microsoft.com/fwlink/p/?LinkId=784855) document inclut section 3.1.5.2.2 qui décrit la structure des objets et les commandes de métadonnées sous forme de tableau JSON. Actuellement, ce document traite des commandes et fonctions non encore implémentées dans un script TMSL. Reportez-vous à la rubrique ([Tabular Model Scripting Language &#40;TMSL&#41; référence](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)) pour obtenir des éclaircissements sur ce qui est pris en charge.  

## <a name="see-also"></a>Voir aussi  
 [Tabular Model Scripting Language &#40;TMSL&#41; Reference (Informations de référence sur TMSL &#40;Tabular Model Scripting Language&#41;)](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)  
  
  
