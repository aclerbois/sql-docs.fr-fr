---
title: Type de règles de conversion dans XQuery | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.component: xquery
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
dev_langs:
- XML
helpviewer_keywords:
- XQuery, type casting
- casting rules [XML in SQL Server]
- explicit casting [SQL Server]
- type casting rules [XML in SQL Server]
- cast as operator
- implicit casting
ms.assetid: f2e91306-2b1b-4e1c-b6d8-a34fb9980057
caps.latest.revision: 35
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 3142794843083c5dcc314b7dc6b0f69cb62f889e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="type-casting-rules-in-xquery"></a>Règles de conversion de types dans XQuery
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Le diagramme des spécifications suivant, retraçant les fonctions et les opérateurs XQuery 1.0 et XPath 2.0 définis par le W3C, illustre les types de données intégrés. Parmi ces types, on retrouve les types intégrés de primitives et de dérivées.  
  
 ![Hiérarchie de type XQuery 1.0](../xquery/media/xquery-typing-rules.gif "hiérarchie de type XQuery 1.0")  
  
 Cette rubrique décrit donc les règles de conversion de types applicables suivant une des deux méthodes suivantes :  
  
-   Conversion explicite vous faire utiliser **castés en tant que** ou les fonctions de constructeur de type (par exemple, `xs:integer("5")`).  
  
-   la conversion implicite qui a lieu pendant la promotion de type à un rang supérieur.  
  
## <a name="explicit-casting"></a>Conversion explicite  
 Le tableau suivant présente la conversion de types autorisée entre les différents types de primitives intégrés.  
  
 ![Décrit les règles de conversion pour XQuery. ] (../xquery/media/casting-builtin-types.gif "Décrit les règles de conversion pour XQuery.")  
  
-   Un type de primitive intégré peut se convertir en un autre type de primitive intégré en respectant les règles en vigueur sur les données de la table.  
  
-   Un type de primitive peut également se convertir en tout autre type dérivé de ce type de primitive. Par exemple, vous pouvez effectuer un cast de **xs : decimal** à **xs : Integer**, ou à partir de **xs : decimal** à **xs : long**.  
  
-   Un type dérivé peut être converti en tout autre type dont il constitue l'ancêtre dans la hiérarchie des types, et ce jusqu'à son type de primitive de base intégré. Par exemple, vous pouvez effectuer un cast de **xs : Token** à **xs : normalizedString** ou **xs : String**.  
  
-   Un type dérivé peut également être converti en type de primitive si l'ancêtre de sa primitive peut aussi être converti dans le type cible. Par exemple, vous pouvez effectuer un cast **xs : Integer**, un type dérivé, en un **xs : String**, la primitive de type, car **xs : decimal**, **xs : Integer**l’ancêtre de primitive, peut être converti en **xs : String**.  
  
-   Un type dérivé peut également être converti en autre type dérivé si l'ancêtre de la primitive du type source peut aussi être converti en ancêtre de primitive du type cible. Par exemple, vous pouvez effectuer un cast de **xs : Integer** à **xs : Token**, car vous pouvez effectuer un cast de **xs : decimal** à **xs : String**.  
  
-   Les règles pour la conversion de types définis par l'utilisateur en types intégrés revient au même que pour la conversion de types intégrés. Par exemple, vous pouvez définir un **myInteger** type dérivé **xs : Integer** type. Ensuite, **myInteger** peut être converti en **xs : Token**, car **xs : decimal** peut être converti en **xs : String**.  
  
 Les types de conversion suivants ne sont pas pris en charge :  
  
-   La conversion mettant en œuvre les types de liste n'est pas autorisée. Cela inclut les types définis par l’utilisateur de liste et les types de listes intégrées telles que **xs : IDREFS**, **xs : Entities**, et **xs : NMTOKENS**.  
  
-   Conversion vers ou depuis **xs : QName** n’est pas pris en charge.  
  
-   **xs : notation** et sous-types de durée entièrement triés **xdt : yearmonthduration** et **xdt : daytimeduration**, ne sont pas pris en charge. C'est pour cela que la conversion mettant en œuvre ces types n'est pas prise en charge.  
  
 Les exemples suivants illustrent la conversion explicite de types.  
  
### <a name="example-a"></a>Exemple A  
 L'exemple suivant interroge une variable de type xml. Elle renvoie ainsi une séquence de valeurs de type simple typé en xs:string.  
  
```  
declare @x xml  
set @x = '<e>1</e><e>2</e>'  
select @x.query('/e[1] cast as xs:string?')  
go  
```  
  
### <a name="example-b"></a>Exemple B  
 L'exemple suivant interroge une variable xml typé. Nous procédons ainsi dans un premier temps à la création d'une collection de schémas XML. Nous utilisons ensuite cette collection afin de créer à son tour une variable XML typé. Le schéma fournit alors les informations de typage pour l'instance XML affectée à la variable. Des requêtes sont ensuite lancées sur la variable.  
  
```  
create xml schema collection myCollection as N'  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  
      <xs:element name="root">  
            <xs:complexType>  
                  <xs:sequence>  
                        <xs:element name="A" type="xs:string"/>  
                        <xs:element name="B" type="xs:string"/>  
                        <xs:element name="C" type="xs:string"/>  
                  </xs:sequence>  
            </xs:complexType>  
      </xs:element>  
</xs:schema>'  
go  
```  
  
 La requête suivante renvoie une erreur statique car l'on ne connaît pas le nombre d'éléments de niveau racine (<`root`>) présents dans l'instance du document.  
  
```  
declare @x xml(myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
          <root><A>4</A><B>5</B><C>6</baz></C>'  
select @x.query('/root/A cast as xs:string?')  
go  
```  
  
 Si vous indiquez un élément singleton <`root`> dans l'expression, la requête fonctionne. Elle renvoie ainsi une séquence de valeurs de type simple typé en xs:string.  
  
```  
declare @x xml(myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
              <root><A>4</A><B>5</B><C>6</C></root>'  
select @x.query('/root[1]/A cast as xs:string?')  
go  
```  
  
 Dans l'exemple suivant, la variable de type xml inclut un mot clé document précisant la collection de schémas XML. Cela indique que l'instance XML doit être un document ne possédant qu'un seul élément de premier niveau. Dans ce cas, si vous créez deux éléments <`root`> dans l'instance XML, une erreur est alors renvoyée.  
  
```  
declare @x xml(document myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
              <root><A>4</A><B>5</B><C>6</C></root>'  
go  
```  
  
 Vous pouvez modifier l'instance afin de n'inclure qu'un seul élément de premier niveau et faire fonctionner ainsi la requête. Elle renvoie donc une séquence de valeurs de type simple typé en xs:string.  
  
```  
declare @x xml(document myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>'  
select @x.query('/root/A cast as xs:string?')  
go  
```  
  
## <a name="implicit-casting"></a>Conversion implicite  
 La conversion implicite n'est autorisée que pour les types numériques et les types atomiques non typés. Par exemple, **min()** fonction retourne la valeur minimale de deux valeurs :  
  
```  
min(xs:integer("1"), xs:double("1.1"))  
```  
  
 Dans cet exemple, les deux valeurs transmise à la fonction XQuery **min()** (fonction) sont de types différents. Par conséquent, la conversion implicite est effectuée où **entier** type est promu au rang **double** et les deux **double** valeurs sont comparées.  
  
 La promotion de type telle qu'elle est décrite dans cet exemple suit les règles suivantes :  
  
-   Un type numérique dérivé peut être promu en son type de base. Par exemple, **entier** peut être promu en **décimal**.  
  
-   A **décimal** peut être promu en **float,** et un **float** peut être promu en **double**.  
  
 La conversion implicite n'étant autorisée que pour les types numériques, ce qui suit n'est pas possible :  
  
-   La conversion implicite mettant en œuvre les types de chaîne (string) n'est pas autorisée. Par exemple, si deux **chaîne** types sont attendus mais que vous transmettez un **chaîne** et un **jeton**, aucune conversion implicite se produit et une erreur est renvoyée.  
  
-   La conversion implicite des types numériques en types de chaînes n'est pas non plus autorisée. Ainsi, si vous transmettez une valeur de type integer à une fonction s'attendant à un paramètre de type string (chaîne), là encore la conversion implicite ne peut pas se produire et entraîne une erreur.  
  
## <a name="casting-values"></a>Conversion de valeurs  
 Lors de la conversion d'un type vers un autre, les valeurs sont en fait transformées de l'espace de valeurs du type source à celui du type cible. Par exemple, la conversion de xs:decimal en xs:double transforme la valeur décimale en valeur double.  
  
 Voici quelques-unes des règles qui s'appliquent lors de la transformation.  
  
##### <a name="casting-a-value-from-a-string-or-untypedatomic-type"></a>Conversion d'une valeur de type string ou de type untypedAtomic  
 Une valeur convertie en un type string (chaîne) ou untypedAtomic (atomique non typé) est transformée comme si elle faisait l'objet d'une validation suivant les règles du type cible. Cela inclut les règles éventuelles de traitement des modèles et des espaces. Par exemple, l'instruction suivante fonctionne et génère ainsi une valeur de type double (1.1e0) :  
  
 `xs:double("1.1")`  
  
 Lors de la conversion vers des types binaires tels que xs:base64Binary ou xs:hexBinary à partir d'un type string (chaîne) ou untypedAtomic (atomique non typé), la valeur d'entrée doit être encodée respectivement au format base64 ou HEX.  
  
##### <a name="casting-a-value-to-a-string-or-untypedatomic-type"></a>Conversion d'une valeur en valeur de type string ou untypedAtomic  
 La conversion vers un type string (chaîne) ou untypedAtomic (atomique non typé) transforme la valeur en sa représentation lexicale canonique XQuery. Plus précisément, on peut dire qu'une valeur ayant répondu à un modèle précis ou à une autre contrainte lors de sa saisie pourrait ne pas être représentée en fonction de ladite contrainte.  Pour informer les utilisateurs, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] indique les types où la contrainte de type peut être un problème en fournissant un avertissement lorsque ces types sont chargés dans la collection de schémas.  
  
 Lors de la conversion d'une valeur de type xs:float ou xs:double, ou encore de l'un de leurs sous-types, en une valeur de type string ou untypedAtomic, ladite valeur est alors représentée sous sa forme scientifique. Cette opération n'est effectuée que si la valeur absolue de la donnée est inférieure à 1,0E-6 ou supérieure ou égale à 1,0E6. Dans ce cas, 0 est sérialisé sous sa forme scientifique de 0,0E0.  
  
 Par exemple, `xs:string(1.11e1)` renvoie la valeur de type chaîne `"11.1"`, alors que `xs:string(-0.00000000002e0)` renvoie la valeur de type chaîne `"-2.0E-11"`.  
  
 Lors de la conversion de types binaires tels que xs:base64Binary ou xs:hexBinary en un type string (chaîne) ou untypedAtomic (atomique non typé), les valeurs binaires sont alors respectivement encodées au format base64 ou HEX.  
  
##### <a name="casting-a-value-to-a-numeric-type"></a>Conversion d'une valeur en valeur de type numérique  
 Lors de la conversion d'une valeur d'un des types numériques en une valeur d'un autre type numérique, la valeur est alors mappée d'un espace de valeurs à l'autre sans passer par la sérialisation par chaîne. Si elle ne satisfait pas la contrainte relative au type cible, les règles suivantes s'appliquent alors :  
  
-   Si la valeur source est déjà de type numérique et le type cible est xs:float ou un de ses sous-types acceptant les valeurs -INF ou INF, et si la conversion de la valeur numérique source provoque un dépassement de capacité, la valeur est alors mappée à INF si elle est positive ou à -INF si elle est négative. Si le type cible n'accepte pas ces valeurs INF ou -INF entraînant ainsi un dépassement de capacité, la conversion échoue et le résultat (dans le cas de cette version de SQL Server) correspond ainsi à la séquence vide.  
  
-   Si la valeur source est déjà de type numérique et le type cible est un type numérique acceptant les valeurs 0, -0e0 ou 0e0 dans sa plage de valeurs, si la conversion de la valeur numérique source provoque un dépassement de capacité, la valeur est mappée selon les cas suivants :  
  
    -   La valeur est mappée à 0 pour un type cible décimal.  
  
    -   La valeur est mappée à -0e0 si la valeur correspond à un dépassement de capacité négatif.  
  
    -   La valeur est mappée sur 0e0 si la valeur correspond à un dépassement de capacité positif pour un type float ou double.  
  
     Si le type cible n'inclut pas le zéro dans son espace de valeurs, la conversion échoue et le résultat est alors la séquence vide.  
  
     Remarque : il se peut que la conversion d'une valeur en un type à virgule flottante binaire, tel que xs:float, xs:double ou un de leurs sous-types, entraîne une perte de précision.  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La valeur à virgule flottante NaN n'est pas prise en charge.  
  
-   Les valeurs convertibles sont limitées par les restrictions d'implémentation des types de cibles. Par exemple, vous ne peut pas convertir une chaîne de date avec une année négative en **xs : date**. De telles conversions génèreront la séquence vide si la valeur est fournie au moment de l'exécution (au lieu de déclencher une erreur d'exécution).  
  
## <a name="see-also"></a>Voir aussi  
 [Définir la sérialisation des données XML](../relational-databases/xml/define-the-serialization-of-xml-data.md)  
  
  
