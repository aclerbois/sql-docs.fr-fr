---
title: Introduction à l’utilisation de requêtes XPath (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], about XPath queries
- W3C XPath specification
- XPath queries [SQLXML], functionality
ms.assetid: 01050a8e-0ccc-4a02-a4eb-b48be5c3f4f3
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e620b704f7678a9af8510e7b1d81321aec6ba061
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-using-xpath-queries-sqlxml-40"></a>Introduction à l'utilisation des requêtes XPath (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Une requête XPath (XML Path Language) peut être spécifiée dans le cadre d'une URL ou dans un modèle. Le schéma de mappage détermine la structure de ce fragment résultant et les valeurs sont extraites de la base de données. Ce processus est conceptuellement semblable à la création de vues à l'aide de l'instruction CREATE VIEW et à l'écriture de requêtes SQL dans ces vues.  
  
> [!NOTE]  
>  Pour comprendre les requêtes XPath dans SQLXML 4.0, vous devez avoir une bonne connaissance des vues XML et des concepts connexes tels que les modèles et les schémas de mappage. Pour plus d’informations, consultez [Introduction aux schémas de XSD annoté &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)et la norme XPath définie par le World Wide Web Consortium (W3C).  
  
 Un document XML est constitué de nœuds tels qu'un nœud d'élément, un nœud d'attribut, un nœud de texte, et ainsi de suite. Considérons par exemple le document XML suivant :  
  
```  
<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was  
          very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>  
```  
  
 Dans ce document,  **\<client >** est un nœud d’élément, **cid** est un nœud d’attribut, et **« Important »** est un nœud de texte.  
  
 XPath est un langage de navigation graphique utilisé pour sélectionner une collection de nœuds à partir d'un document XML. Chaque opérateur XPath sélectionne un élément node-set sur la base d'un élément node-set sélectionné par un opérateur XPath précédent. Par exemple, étant donné un ensemble de  **\<client >** nœuds, XPath peuvent sélectionner tout  **\<ordre >** nœuds avec le **date** valeur d’attribut **« 7/14/1999 »**. L'élément node-set résultant contient toutes les commandes avec la date de commande 7/14/1999.  
  
 Le langage XPath est défini par le W3C (World Wide Web Consortium) en tant que langage de navigation standard. SQLXML 4.0 implémente un sous-ensemble de la spécification W3C XPath, qui se trouve dans http://www.w3.org/TR/1999/PR-xpath-19991008.html.  
  
 Voici les principales différences entre l'implémentation XPath W3C et l'implémentation SQLXML 4.0.  
  
-   **Requêtes racines**  
  
     SQLXML 4.0 ne prend pas en charge la requête racine (/). Chaque requête XPath doit commencer à un niveau supérieur  **\<ElementType >** dans le schéma.  
  
-   **Rapports d’erreurs**  
  
     La spécification XPath W3C ne définit pas de conditions d'erreur. Les requêtes XPath qui ne sélectionnent aucun nœud retournent un élément node-set vide. Dans SQLXML 4.0, une requête peut retourner de nombreux types de messages d'erreur.  
  
-   **Ordre du document**  
  
     Dans SQLXML 4.0, l'ordre du document n'est pas toujours déterminé. Par conséquent et les prédicats numériques axes cet ordre de document Utilisez (telles que **suivant**) ne sont pas implémentées.  
  
     L'absence d'ordre de document signifie également que la valeur de chaîne d'un nœud peut être évaluée uniquement lorsque ce nœud mappe à une colonne unique dans une ligne unique. Un élément avec des éléments enfants ou un nœud IDREFS ou NMTOKENS ne peut pas être converti en chaîne.  
  
    > [!NOTE]  
    >  Dans certains cas, le **key-fields** annotation ou des clés de la **relation** annotation peut générer un ordre de document déterministe. Toutefois, cela n’est pas la principale utilisation de ces annotations pour plus d’informations, consultez [colonnes clé identifiant à l’aide de SQL : Key-champs &#40;SQLXML 4.0&#41; ](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md) et [spécification de relations à l’aide de sql : relation &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md).  
  
-   **Types de données**  
  
     SQLXML 4.0 présente des limitations de l’implémentation de XPath **chaîne**, **nombre**, et **booléenne** des types de données. Pour plus d’informations, consultez [les Types de données XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/xpath-data-types-sqlxml-4-0.md).  
  
-   **Requêtes entre les produits**  
  
     SQLXML 4.0 ne prend pas en charge les requêtes XPath entre les produits, telles que `Customers[Order/@OrderDate=Order/@ShipDate]`. Cette requête sélectionne tous les éléments Customers avec tout élément Order pour lequel OrderDate est égal à l'élément ShipDate de tout élément Order.  
  
     En revanche, SQLXML 4.0 prend en charge les requêtes telles que `Customer[Order[@OrderDate=@ShippedDate]]`, qui sélectionne les éléments Customers avec tout élément Order pour lequel OrderDate est égal à son élément ShipDate.  
  
-   **Sécurité et gestion des erreurs**  
  
     Selon le schéma et l'expression de requête XPath utilisés, les erreurs [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent être exposées aux utilisateurs sous certaines conditions.  
  
 Les tableaux des sections suivantes fournissent des détails sur la manière dont l'implémentation des requêtes XPath dans SQLXML 4.0 diffère de la spécification W3C dans ces domaines.  
  
## <a name="supported-functionality"></a>Fonctionnalités prises en charge  
 Le tableau suivant indique les fonctionnalités du langage XPath implémentées dans SQLXML 4.0.  
  
|Fonctionnalité|Élément|Lien aux exemples de requêtes|  
|-------------|----------|----------------------------|  
|Axes|**attribut**, **enfant**, **parent**, et **self** axes|[Définition des Axes dans les requêtes XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-axes-in-xpath-queries-sqlxml-4-0.md)|  
|Prédicats à valeurs booléennes, y compris les prédicats consécutifs et imbriqués||[Spécification d’opérateurs arithmétiques dans des requêtes XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-arithmetic-operators-in-xpath-queries-sqlxml-4-0.md)|  
|Tous les opérateurs relationnels|=, !=, <, \<=, >, >=|[Spécification d’opérateurs relationnels dans des requêtes XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-relational-operators-in-xpath-queries-sqlxml-4-0.md)|  
|les opérateurs arithmétiques ;|+, -, *, div|[Spécification d’opérateurs arithmétiques dans des requêtes XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-arithmetic-operators-in-xpath-queries-sqlxml-4-0.md)|  
|Fonctions de conversion explicite|**number()**, **string()**, **Boolean()**|[Spécification de fonctions de Conversion explicite dans des requêtes XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-explicit-conversion-functions-in-xpath-queries-sqlxml-4-0.md)|  
|opérateurs booléens|AND, OR|[Spécification d’opérateurs booléens dans les requêtes XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-boolean-operators-in-xpath-queries-sqlxml-4-0.md)|  
|fonctions booléennes|**true()**, **false()**, **not()**|[Spécification de fonctions booléennes dans des requêtes XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-boolean-functions-in-xpath-queries-sqlxml-4-0.md)|  
|variables XPath||[Spécification de Variables XPath dans les requêtes XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-xpath-variables-in-xpath-queries-sqlxml-4-0.md)|  
  
## <a name="unsupported-functionality"></a>Fonctionnalités non prises en charge  
 Le tableau suivant indique les fonctionnalités du langage XPath non implémentées dans SQLXML 4.0.  
  
|Fonctionnalité|Élément|  
|-------------|----------|  
|Axes|**ancêtre**, **ancêtre-or-self**, **descendants**, **descendant-or-self (/ /)**, **suivant**,  **frère suivant**, **espace de noms**, **précédent**, **frère précédent**|  
|Prédicats à valeurs numériques||  
|les opérateurs arithmétiques ;|mod|  
|Fonctions de nœuds|**ancêtre**, **ancêtre-or-self**, **descendants**, **descendant-or-self (/ /)**, **suivant**,  **frère suivant**, **espace de noms**, **précédent**, **frère précédent**|  
|Fonctions de chaînes|**String()**, **concat()**, **starts-with()**, **contains()**, **substring-before()**,  **SUBSTRING-After()**, **substring()**, **Length**, **normalize()**, **translate()**|  
|fonctions booléennes|**lang()**|  
|Fonctions Numériques|**sum()**, **floor()**, **ceiling()**, **round()**|  
|Opérateur d'union|&#124;|  
  
 Lorsque vous spécifiez des requêtes XPath dans un modèle, notez le comportement suivant :  
  
-   XPath peut contenir des caractères tels que < ou & qui ont des significations spéciales en XML (et le modèle est un document XML). Vous devez échapper ces caractères à l'aide de l'encodage & XML ou spécifier le code XPath dans l'URL.  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation des requêtes XPath dans SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/using-xpath-queries-in-sqlxml-4-0.md)  
  
  
