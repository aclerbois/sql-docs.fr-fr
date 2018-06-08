---
title: Mettre les résultats de requête au format JSON avec FOR JSON (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: json
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- dbe-json
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON
- JSON, exporting
- exporting JSON
ms.assetid: 15b56365-58c2-496c-9d4b-aa2600eab09a
caps.latest.revision: 31
author: jovanpop-msft
ms.author: jovanpop
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ad65f71c46a89f3758cb5102021843f96b3adf1e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="format-query-results-as-json-with-for-json-sql-server"></a>Mettre les résultats de requête au format JSON avec FOR JSON (SQL Server)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Mettez les résultats de la requête au format JSON ou exportez les données à partir de SQL Server au format JSON, en ajoutant la clause **FOR JSON** à une instruction **SELECT**. Utilisez la clause **FOR JSON** pour simplifier les applications clientes en déléguant la mise en forme de la sortie JSON produite par l’application à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
 Quand vous utilisez la clause **FOR JSON**, vous pouvez spécifier de manière explicite la structure de la sortie JSON ou laisser la structure de l’instruction SELECT déterminer la sortie.  
  
-   Pour maintenir un contrôle total sur le format de la sortie JSON, utilisez **FOR JSON PATH**. Vous pouvez créer des objets wrapper et imbriquer des propriétés complexes.  
  
-   Pour mettre en forme la sortie JSON automatiquement en fonction de la structure de l’instruction SELECT, utilisez **FOR JSON AUTO**.  
  
Voici l’exemple d’une instruction **SELECT** avec la clause **FOR JSON** et sa sortie.
  
 ![FOR JSON](../../relational-databases/json/media/jsonslides2forjson.png)
  
## <a name="option-1---you-control-output-with-for-json-path"></a>Option 1 : Vous contrôlez la sortie avec FOR JSON PATH
En mode **PATH** , vous pouvez utiliser la syntaxe à point (par exemple `'Item.Price'` ) pour mettre en forme la sortie imbriquée.  

Voici un exemple de requête qui utilise le mode **PATH** avec la clause **FOR JSON** . L’exemple suivant utilise également l’option **ROOT** pour spécifier un élément racine nommé. 
  
 ![Diagramme du flux de sortie FOR JSON](../../relational-databases/json/media/forjson-example1.png) 

### <a name="more-info-about-for-json-path"></a>Plus d’informations sur FOR JSON PATH
Pour plus d’informations et pour obtenir des exemples détaillés, consultez [Mettre en forme la sortie JSON imbriquée avec le mode PATH &#40;SQL Server&#41;](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md).

Pour plus d’informations sur la syntaxe et l’utilisation, consultez [Clause FOR &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md).  

## <a name="option-2---select-statement-controls-output-with-for-json-auto"></a>Option 2 : L’instruction SELECT contrôle la sortie avec FOR JSON AUTO
Dans le mode **AUTO** , la structure de l'instruction SELECT détermine le format de la sortie JSON.

Par défaut, les valeurs **Null** ne sont pas incluses dans la sortie. Vous pouvez utiliser **INCLUDE_NULL_VALUES** pour modifier ce comportement.  

Voici un exemple de requête qui utilise le mode **AUTO** avec la clause **FOR JSON** .
 
**Requête :**  
  
```sql  
SELECT name, surname  
FROM emp  
FOR JSON AUTO  
```  
  
 **Résultats**  
  
```json  
[{
    "name": "John"
}, {
    "name": "Jane",
    "surname": "Doe"
}]
```
 
### <a name="more-info-about-for-json-auto"></a>Plus d’informations sur FOR JSON AUTO
Pour plus d’informations et pour obtenir des exemples détaillés, consultez [Mettre en forme automatiquement la sortie JSON avec le mode AUTO &#40;SQL Server&#41;](../../relational-databases/json/format-json-output-automatically-with-auto-mode-sql-server.md).

Pour plus d’informations sur la syntaxe et l’utilisation, consultez [Clause FOR &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md).  
  
## <a name="control-other-json-output-options"></a>Contrôler d’autres options de sortie JSON  
Contrôlez la sortie de la clause **FOR JSON** à l’aide des options supplémentaires suivantes.  
  
-   **ROOT**. Pour ajouter un élément de niveau supérieur unique à la sortie JSON, spécifiez l’option **ROOT** . Si vous ne spécifiez pas cette option, la sortie JSON n’a aucun élément racine. Pour plus d’informations, consultez [Ajouter un nœud racine à la sortie JSON avec l’option ROOT &#40;SQL Server&#41;](../../relational-databases/json/add-a-root-node-to-json-output-with-the-root-option-sql-server.md).  
  
-   **INCLUDE_NULL_VALUES**. Pour inclure des valeurs Null dans la sortie JSON, spécifiez l’option **INCLUDE_NULL_VALUES** . Si vous ne spécifiez cette option, la sortie n’inclut pas les propriétés JSON pour les valeurs NULL dans les résultats de la requête. Pour plus d’informations, consultez[Inclure des valeurs Null dans une sortie JSON avec l’option INCLUDE_NULL_VALUES &#40;SQL Server&#41;](../../relational-databases/json/include-null-values-in-json-include-null-values-option.md).   

-   **WITHOUT_ARRAY_WRAPPER**. Pour supprimer les crochets qui entourent par défaut la sortie JSON de la clause **FOR JSON**, spécifiez l’option **WITHOUT_ARRAY_WRAPPER**. Utilisez cette option pour générer un seul objet JSON en tant que sortie d’un résultat de ligne unique. Si vous ne spécifiez pas cette option, la sortie JSON est mise en forme en tant que tableau, ce qui signifie qu’elle est placée entre crochets. Pour plus d’informations, consultez [Supprimer les crochets de la sortie JSON avec l’option WITHOUT_ARRAY_WRAPPER &#40;SQL Server&#41;](../../relational-databases/json/remove-square-brackets-from-json-without-array-wrapper-option.md). 
   
## <a name="output-of-the-for-json-clause"></a>Sortie de la clause FOR JSON  
La sortie de la clause **FOR JSON** présente les caractéristiques suivantes :  
  
1.  Le jeu de résultats contient une seule colonne.
    -   Un jeu de résultats de petite taille peut ne contenir qu’une seule ligne.
    -   Un grand jeu de résultats fractionne la longue chaîne JSON sur plusieurs lignes.
        -   Par défaut, SQL Server Management Studio (SSMS) concatène les résultats en une seule ligne quand la valeur du paramètre de sortie est **Résultats dans des grilles**. La barre d’état SSMS affiche le nombre de lignes réel.
        -   D’autres applications clientes peuvent nécessiter du code qui recombine des résultats longs en une seule chaîne JSON valide en concaténant le contenu de plusieurs lignes. Pour obtenir un exemple de ce code dans une application C#, consultez [Utiliser la sortie de FOR JSON dans une application cliente C#](../../relational-databases/json/use-for-json-output-in-sql-server-and-in-client-apps-sql-server.md#use-for-json-output-in-a-c-client-app).
  
     ![Exemple de sortie FOR JSON](../../relational-databases/json/media/forjson-example2.png)  
  
2.  Les résultats sont présentés sous la forme de tableau d’objets JSON.  
  
    -   Le nombre d’éléments du tableau JSON est égal au nombre de lignes dans les résultats de l’instruction SELECT (avant l’application de la clause FOR JSON). 
  
    -   Chaque ligne des résultats de l’instruction SELECT (avant l’application de la clause FOR JSON) devient un objet JSON distinct dans le tableau.  
  
    -   Chaque colonne des résultats de l’instruction SELECT (avant l’application de la clause FOR JSON) devient une propriété de l’objet JSON.  
  
3.  Le nom et la valeur des colonnes sont échappés selon la syntaxe JSON. Pour plus d’informations, consultez [Comment FOR JSON place dans une séquence d’échappement les caractères spéciaux et les caractères de contrôle &#40;SQL Server&#41;](../../relational-databases/json/how-for-json-escapes-special-characters-and-control-characters-sql-server.md).
  
### <a name="example"></a> Exemple
Voici un exemple qui montre comment la clause **FOR JSON** met en forme la sortie JSON.  
  
**Résultats de la requête**  
  
|||||  
|-|-|-|-|  
|**Objet**|**B**|**C**|**D**|  
|10|11|12|X|  
|20|21|22|O|  
|30|31|32|Z|  
  
 **Sortie JSON**  
  
```json  
[{
    "A": 10,
    "B": 11,
    "C": 12,
    "D": "X"
}, {
    "A": 20,
    "B": 21,
    "C": 22,
    "D": "Y"
}, {
    "A": 30,
    "B": 31,
    "C": 32,
    "D": "Z"
}] 
```  

 Pour obtenir plus d’informations sur ce que vous voyez dans la sortie de la clause **FOR JSON**, consultez les rubriques suivantes.  

-   [Conversion par FOR JSON des types de données SQL Server en types de données JSON &#40;SQL Server&#41;](../../relational-databases/json/how-for-json-converts-sql-server-data-types-to-json-data-types-sql-server.md)  
    La clause **FOR JSON** utilise les règles décrites dans cette rubrique pour convertir les types de données SQL en types JSON dans la sortie JSON.  

-   [Comment FOR JSON place dans une séquence d’échappement les caractères spéciaux et les caractères de contrôle &#40;SQL Server&#41;](../../relational-databases/json/how-for-json-escapes-special-characters-and-control-characters-sql-server.md)  
    La clause **FOR JSON** échappe les caractères spéciaux et représente les caractères de contrôle dans la sortie JSON comme le décrit cette rubrique.  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>En savoir plus sur JSON dans SQL Server et Azure SQL Database  
  
### <a name="microsoft-blog-posts"></a>Billets de blog Microsoft  
  
Pour accéder à un grand nombre de solutions spécifiques, de cas d’usage et de recommandations, consultez ces [billets de blog](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/) sur la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database.  

### <a name="microsoft-videos"></a>Vidéos Microsoft

Pour obtenir une présentation visuelle de la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database, consultez les vidéos suivantes :

-   [SQL Server 2016 et prise en charge de JSON](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Utilisation de JSON dans SQL Server 2016 et Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON : un pont entre les mondes relationnel et NoSQL](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a> Voir aussi  
 [Clause FOR &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md)   
 [Utiliser la sortie de FOR JSON dans SQL Server et les applications clientes &#40;SQL Server&#41;](../../relational-databases/json/use-for-json-output-in-sql-server-and-in-client-apps-sql-server.md)  
  
  
