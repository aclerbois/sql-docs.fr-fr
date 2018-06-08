---
title: Instruction FLWOR et itération (XQuery) | Documents Microsoft
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
- return clause
- FLWOR statement
- effective Boolean value [XQuery]
- multiple variable binding
- order by clause [XQuery]
- for clause [XQuery]
- where clause [XQuery]
- iterations [XQuery]
- XQuery, FLWOR statement
- EBV
ms.assetid: d7cd0ec9-334a-4564-bda9-83487b6865cb
caps.latest.revision: 44
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: c9ca8e71d1f71ba6416c08586e5613d129ad35d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="flwor-statement-and-iteration-xquery"></a>Instruction et itération FLWOR (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  XQuery définit la syntaxe d'itération FLWOR. FLWOR est l'acronyme de `for`, `let`, `where`, `order by` et `return`.  
  
 Une instruction FLWOR est constituée des parties suivantes :  
  
-   Une ou plusieurs clauses FOR qui relient une ou plusieurs variables d'itération aux séquences en entrée.  
  
     Les séquences en entrée peuvent être d'autres expressions XQuery telles que des expressions XPath. Il peut s'agir de séquences de nœuds ou de séquences de valeurs atomiques. Séquences de valeurs atomiques peuvent être construites à l’aide des littéraux ou des fonctions de constructeur. Les nœuds XML construits ne sont pas autorisés comme séquences en entrée dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-   Une clause `let` facultative. Cette clause attribue une valeur à la variable donnée pour une itération spécifique. L'expression attribuée peut être une expression XQuery, telle qu'une expression XPath, et peut retourner une séquence de nœuds ou une séquence de valeurs atomiques. Les séquences de valeurs atomiques peuvent être construites à l'aide de littéraux ou de fonctions constructeur. Les nœuds XML construits ne sont pas autorisés comme séquences en entrée dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-   Une variable d'itération. Il est possible d'associer une assertion de type à cette variable à l'aide du mot clé `as`.  
  
-   Une clause `where` facultative. Cette clause applique un prédicat de filtre sur l'itération.  
  
-   Une clause `order by` facultative.  
  
-   Une expression `return`. L'expression de la clause `return` construit le résultat de l'instruction FLWOR.  
  
 Par exemple, la requête suivante effectue une itération au sein des éléments <`Step`> au niveau du premier site de fabrication, et renvoie la valeur de chaîne des nœuds <`Step`> :  
  
```  
declare @x xml  
set @x='<ManuInstructions ProductModelID="1" ProductModelName="SomeBike" >  
<Location LocationID="L1" >  
  <Step>Manu step 1 at Loc 1</Step>  
  <Step>Manu step 2 at Loc 1</Step>  
  <Step>Manu step 3 at Loc 1</Step>  
</Location>  
<Location LocationID="L2" >  
  <Step>Manu step 1 at Loc 2</Step>  
  <Step>Manu step 2 at Loc 2</Step>  
  <Step>Manu step 3 at Loc 2</Step>  
</Location>  
</ManuInstructions>'  
SELECT @x.query('  
   for $step in /ManuInstructions/Location[1]/Step  
   return string($step)  
')  
```  
  
 Voici le résultat obtenu :  
  
```  
Manu step 1 at Loc 1 Manu step 2 at Loc 1 Manu step 3 at Loc 1  
```  
  
 La requête suivante est très similaire à la précédente, à une exception près : elle porte sur la colonne Instructions, une colonne typée xml, de la table ProductModel. La requête effectue une itération sur toutes les étapes de fabrication, éléments <`step`>, au niveau du premier poste de travail, pour un produit spécifique.  
  
```  
SELECT Instructions.query('  
   declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $Step in //AWMI:root/AWMI:Location[1]/AWMI:step  
      return  
           string($Step)   
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   `$Step` est la variable d'itération.  
  
-   Le [expression de chemin d’accès](../xquery/path-expressions-xquery.md), `//AWMI:root/AWMI:Location[1]/AWMI:step`, génère la séquence d’entrée. Cette séquence est la séquence des enfants du nœud d'élément <`step`> du premier nœud d'élément <`Location`>.  
  
-   La clause de prédicat facultative, `where`, n'est pas utilisée.  
  
-   L'expression `return` renvoie une valeur de chaîne à partir de l'élément <`step`>.  
  
 Le [fonction string (XQuery)](../xquery/data-accessor-functions-string-xquery.md) est utilisé pour récupérer la valeur de chaîne de la <`step`> nœud.  
  
 Voici le résultat partiel :  
  
```  
Insert aluminum sheet MS-2341 into the T-85A framing tool.   
Attach Trim Jig TJ-26 to the upper and lower right corners of   
the aluminum sheet. ....         
```  
  
 Voici des exemples d'autres séquences en entrée autorisées :  
  
```  
declare @x xml  
set @x=''  
SELECT @x.query('  
for $a in (1, 2, 3)  
  return $a')  
-- result = 1 2 3   
  
declare @x xml  
set @x=''  
SELECT @x.query('  
for $a in   
   for $b in (1, 2, 3)  
      return $b  
return $a')  
-- result = 1 2 3  
  
declare @x xml  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('  
  for $a in (xs:string( "test"), xs:double( "12" ), data(/ROOT/a ))  
  return $a')  
-- result test 12 111  
```  
  
 Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], les séquences hétérogènes ne sont pas autorisées. En particulier, les séquences contenant un mélange de valeurs atomiques et de nœuds sont interdites.  
  
 Itération est souvent utilisée avec la [Construction XML](../xquery/xml-construction-xquery.md) formats de syntaxe dans la transformation de code XML, comme indiqué dans la requête suivante.  
  
 Dans la base de données AdventureWorks, les instructions de fabrication stockées dans le **Instructions** colonne de la **Production.ProductModel** table ont la forme suivante :  
  
```  
<Location LocationID="10" LaborHours="1.2"   
            SetupHours=".2" MachineHours=".1">  
  <step>describes 1st manu step</step>  
   <step>describes 2nd manu step</step>  
   ...  
</Location>  
...  
```  
  
 La requête suivante construit un nouveau code XML où figurent des éléments <`Location`> et où les attributs des postes de travail sont renvoyés comme des éléments enfants :  
  
```  
<Location>  
   <LocationID>10</LocationID>  
   <LaborHours>1.2</LaborHours>  
   <SetupHours>.2</SteupHours>  
   <MachineHours>.1</MachineHours>  
</Location>  
...  
```  
  
 Voici la requête :  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        for $WC in /AWMI:root/AWMI:Location  
        return  
          <Location>  
            <LocationID> { data($WC/@LocationID) } </LocationID>  
            <LaborHours>   { data($WC/@LaborHours) }   </LaborHours>  
            <SetupHours>   { data($WC/@SetupHours) }   </SetupHours>  
            <MachineHours> { data($WC/@MachineHours) } </MachineHours>  
          </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   L'instruction FLWOR récupère une séquence d'éléments <`Location`> pour un produit spécifique.  
  
-   Le [fonction data (XQuery)](../xquery/data-accessor-functions-data-xquery.md) est utilisé pour extraire la valeur de chaque attribut, donc ils seront ajoutés au document XML obtenu en tant que nœuds de texte et non en tant qu’attributs.  
  
-   L'expression de la clause RETURN construit le code XML dont vous avez besoin.  
  
 Voici un extrait du résultat :  
  
```  
<Location>  
  <LocationID>10</LocationID>  
  <LaborHours>2.5</LaborHours>  
  <SetupHours>0.5</SetupHours>  
  <MachineHours>3</MachineHours>  
</Location>  
<Location>  
   ...  
<Location>  
...  
```  
  
## <a name="using-the-let-clause"></a>Utilisation de la clause let  
 Vous pouvez utiliser la clause `let` pour nommer des expressions récurrentes auxquelles vous pouvez faire référence en faisant référence à la variable. L'expression attribuée à une variable `let` est insérée dans la requête chaque fois que la variable est référencée dans la requête. Cela signifie que l'instruction est exécutée autant de fois que l'expression est référencée.  
  
 Dans la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)], les instructions de fabrication contiennent des informations sur les outils requis et les emplacements où les outils sont utilisés. La requête suivante utilise la clause `let` pour répertorier les outils requis pour construire un modèle de production, ainsi que les emplacements où chaque outil est requis.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        for $T in //AWMI:tool  
            let $L := //AWMI:Location[.//AWMI:tool[.=data($T)]]  
        return  
          <tool desc="{data($T)}" Locations="{data($L/@LocationID)}"/>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
## <a name="using-the-where-clause"></a>Utilisation de la clause where  
 Vous pouvez utiliser la clause `where` pour filtrer les résultats d'une itération. L'exemple suivant illustre ce concept.  
  
 Dans le cadre de la fabrication d'une bicyclette, le processus de fabrication passe par une série de postes de travail. Chaque poste de travail définit une séquence d'étapes de fabrication. La requête suivante récupère uniquement les postes de travail qui fabriquent un modèle de bicyclette et comportent moins de trois étapes de fabrication, soit moins de trois éléments <`step`>.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /AWMI:root/AWMI:Location  
      where count($WC/AWMI:step) < 3  
      return  
          <Location >  
           { $WC/@LocationID }   
          </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Notez les points suivants par rapport à la requête ci-dessus :  
  
-   Le `where` (mot clé) utilise le **count()** fonction pour compter le nombre de <`step`> emplacement du centre des éléments enfants dans chaque travail.  
  
-   L'expression `return` construit le code XML qu'il vous faut d'après les résultats de l'itération.  
  
 Voici le résultat obtenu :  
  
```  
<Location LocationID="30"/>   
```  
  
 Le résultat de l'expression de la clause `where` est converti en une valeur booléenne à l'aide des règles suivantes, dans l'ordre précisé. Il s'agit des mêmes règles que celles utilisées pour les prédicats dans les expressions de chemin, mis à part que les entiers ne sont pas autorisés :  
  
1.  Si l'expression `where` renvoie une séquence vide, sa valeur booléenne effective est False.  
  
2.  Si l'expression `where` renvoie une valeur booléenne simple, cette valeur est la valeur booléenne effective.  
  
3.  Si l'expression `where` renvoie une séquence contenant au moins un nœud, la valeur booléenne effective est True.  
  
4.  Sinon, une erreur statique est générée.  
  
## <a name="multiple-variable-binding-in-flwor"></a>Liaison de plusieurs variables dans FLWOR  
 Une seule expression FLWOR peut servir à lier plusieurs variables aux séquences en entrée. Dans l'exemple suivant, la requête est spécifiée sur une variable de type xml non typé. L'expression FLOWR renvoie le premier enfant de l'élément <`Step`> de chaque élément <`Location`>.  
  
```  
declare @x xml  
set @x='<ManuInstructions ProductModelID="1" ProductModelName="SomeBike" >  
<Location LocationID="L1" >  
  <Step>Manu step 1 at Loc 1</Step>  
  <Step>Manu step 2 at Loc 1</Step>  
  <Step>Manu step 3 at Loc 1</Step>  
</Location>  
<Location LocationID="L2" >  
  <Step>Manu step 1 at Loc 2</Step>  
  <Step>Manu step 2 at Loc 2</Step>  
  <Step>Manu step 3 at Loc 2</Step>  
</Location>  
</ManuInstructions>'  
SELECT @x.query('  
   for $Loc in /ManuInstructions/Location,  
       $FirstStep in $Loc/Step[1]  
   return   
       string($FirstStep)  
')  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le `for` expression définit `$Loc` et $`FirstStep` variables.  
  
-   Les `two` expressions, `/ManuInstructions/Location` and `$FirstStep in $Loc/Step[1]`, sont corrélées en ce sens que les valeurs de `$FirstStep` dépendent des valeurs de `$Loc`.  
  
-   L'expression associée à `$Loc` génère une séquence d'éléments <`Location`>. Pour chaque élément <`Location`>, `$FirstStep` génère une séquence à un seul élément <`Step`>, soit un singleton.  
  
-   `$Loc` est spécifié dans l'expression associée à la variable `$FirstStep`.  
  
 Voici le résultat obtenu :  
  
```  
Manu step 1 at Loc 1   
Manu step 1 at Loc 2  
```  
  
 La requête suivante est similaire, sauf qu’elle est spécifiée sur la colonne Instructions, tapée **xml** colonne, de la **ProductModel** table. [Construction XML (XQuery)](../xquery/xml-construction-xquery.md) est utilisé pour générer le document XML souhaité.  
  
```  
SELECT Instructions.query('  
     declare default element namespace "http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /root/Location,  
            $S  in $WC/step  
      return  
          <Step LocationID= "{$WC/@LocationID }" >  
            { $S/node() }  
          </Step>  
') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants par rapport à la requête ci-dessus :  
  
-   La clause `for` définit deux variables, `$WC` et `$S`. L'expression associée à `$WC` génère une séquence de postes de travail impliqués dans la fabrication d'une bicyclette (modèle de produit). L'expression de chemin affectée à la variable `$S` génère une séquence d'étapes pour chaque séquence de postes de travail de `$WC`.  
  
-   L’instruction return construit le document XML qui a une <`Step`> élément qui contient l’étape de fabrication et la **LocationID** comme son attribut.  
  
-   Le **déclarer l’espace de noms par défaut élément** est utilisée dans le prologue XQuery de sorte que toutes les déclarations d’espace de noms dans le XML résultant apparaissent au niveau de l’élément de niveau supérieur. Le résultat est ainsi beaucoup plus facile à lire. Pour plus d’informations sur les espaces de noms par défaut, consultez [la gestion des espaces de noms XQuery](../xquery/handling-namespaces-in-xquery.md).  
  
 Voici le résultat partiel :  
  
```  
<Step xmlns=  
    "http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"     
  LocationID="10">  
     Insert <material>aluminum sheet MS-2341</material> into the <tool>T-   
     85A framing tool</tool>.   
</Step>  
...  
<Step xmlns=  
      "http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"     
    LocationID="20">  
        Assemble all frame components following blueprint   
        <blueprint>1299</blueprint>.  
</Step>  
...  
```  
  
## <a name="using-the-order-by-clause"></a>Utilisation de la clause order by  
 Dans XQuery, le tri est réalisé à l'aide de la clause `order by` dans l'expression FLWOR. Les expressions de tri est passé à la `order by` clause doit retourner des valeurs dont les types sont valides pour le **gt** opérateur. Chaque expression de tri doit avoir pour résultat un singleton (une séquence composée d'un seul et unique élément). Par défaut, le tri s'effectue en ordre croissant, mais vous pouvez spécifier l'ordre croissant ou décroissant pour chaque expression de tri.  
  
> [!NOTE]  
>  Les comparaisons de tri sur les valeurs de chaîne effectuées par la mise en œuvre de XQuery dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sont toujours effectuées selon le classement des points de code Unicode binaires.  
  
 La requête suivante récupère tous les numéros de téléphone existants pour un client spécifique à partir de la colonne AdditionalContactInfo. Les résultats sont triés par numéro de téléphone.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT AdditionalContactInfo.query('  
   declare namespace act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
   declare namespace aci="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
   for $a in /aci:AdditionalContactInfo//act:telephoneNumber   
   order by $a/act:number[1] descending  
   return $a  
') As Result  
FROM Person.Person  
WHERE BusinessEntityID=291;  
```  
  
 Notez que la [atomisation (XQuery)](../xquery/atomization-xquery.md) processus récupère la valeur atomique de la <`number`> éléments avant leur transmission à `order by`. Vous pouvez écrire l’expression à l’aide de la **data()** (fonction), mais qui n’est pas obligatoire.  
  
```  
order by data($a/act:number[1]) descending  
```  
  
 Voici le résultat obtenu :  
  
```  
<act:telephoneNumber xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3334</act:number>  
</act:telephoneNumber>  
<act:telephoneNumber xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3333</act:number>  
</act:telephoneNumber>  
```  
  
 Au lieu de déclarer les espaces de noms dans le prologue de la requête, vous pouvez les déclarer à l'aide de WITH XMLNAMESPACES.  
  
```  
WITH XMLNAMESPACES (  
   'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
   'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo'  AS aci)  
  
SELECT AdditionalContactInfo.query('  
   for $a in /aci:AdditionalContactInfo//act:telephoneNumber   
   order by $a/act:number[1] descending  
   return $a  
') As Result  
FROM Person.Person  
WHERE BusinessEntityID=291;  
```  
  
 Vous pouvez aussi réaliser le tri en fonction de la valeur d'attribut. Par exemple, la requête suivante récupère les éléments <`Location`> nouvellement créés dont les attributs LocationID et LaborHours sont triés dans l'ordre décroissant par l'attribut LaborHours. Par conséquent, les postes de travail enregistrant le plus d'heures de main-d'œuvre sont renvoyés en premier.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /AWMI:root/AWMI:Location   
order by $WC/@LaborHours descending  
        return  
          <Location>  
             { $WC/@LocationID }   
             { $WC/@LaborHours }   
          </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 Voici le résultat obtenu :  
  
```  
<Location LocationID="60" LaborHours="4"/>  
<Location LocationID="50" LaborHours="3"/>  
<Location LocationID="10" LaborHours="2.5"/>  
<Location LocationID="20" LaborHours="1.75"/>  
<Location LocationID="30" LaborHours="1"/>  
<Location LocationID="45" LaborHours=".5"/>  
```  
  
 Dans la requête suivante, les résultats sont triés par le nom d'élément. La requête récupère les spécifications d'un produit particulier dans le catalogue de produits. Les spécifications sont les enfants de l'élément <`Specifications`>.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace  
 pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $a in /pd:ProductDescription/pd:Specifications/*   
     order by local-name($a)  
      return $a  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19;  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   L'expression `/p1:ProductDescription/p1:Specifications/*` renvoie les enfants de <`Specifications`>.  
  
-   L'expression `order by (local-name($a))` trie la séquence par la partie locale du nom d'élément.  
  
 Voici le résultat obtenu :  
  
```  
<Color>Available in most colors</Color>  
<Material>Almuminum Alloy</Material>  
<ProductLine>Mountain bike</ProductLine>  
<RiderExperience>Advanced to Professional riders</RiderExperience>  
<Style>Unisex</Style>    
```  
  
 Les nœuds dans lesquels l'expression de tri renvoie une valeur vide sont placés au début de la séquence, comme le montre l'exemple suivant :  
  
```  
declare @x xml  
set @x='<root>  
  <Person Name="A" />  
  <Person />  
  <Person Name="B" />  
</root>  
'  
select @x.query('  
  for $person in //Person  
  order by $person/@Name  
  return   $person  
')  
```  
  
 Voici le résultat obtenu :  
  
```  
<Person />  
<Person Name="A" />  
<Person Name="B" />  
```  
  
 Vous pouvez spécifier plusieurs critères de tri, comme le montre l'exemple suivant. Dans cet exemple, la requête trie les éléments <`Employee`> d'abord par Title, puis par les valeurs d'attribut Administrator.  
  
```  
declare @x xml  
set @x='<root>  
  <Employee ID="10" Title="Teacher"        Gender="M" />  
  <Employee ID="15" Title="Teacher"  Gender="F" />  
  <Employee ID="5" Title="Teacher"         Gender="M" />  
  <Employee ID="11" Title="Teacher"        Gender="F" />  
  <Employee ID="8" Title="Administrator"   Gender="M" />  
  <Employee ID="4" Title="Administrator"   Gender="F" />  
  <Employee ID="3" Title="Teacher"         Gender="F" />  
  <Employee ID="125" Title="Administrator" Gender="F" /></root>'  
SELECT @x.query('for $e in /root/Employee  
order by $e/@Title ascending, $e/@Gender descending  
  
  return  
     $e  
')  
```  
  
 Voici le résultat obtenu :  
  
```  
<Employee ID="8" Title="Administrator" Gender="M" />  
<Employee ID="4" Title="Administrator" Gender="F" />  
<Employee ID="125" Title="Administrator" Gender="F" />  
<Employee ID="10" Title="Teacher" Gender="M" />  
<Employee ID="5" Title="Teacher" Gender="M" />  
<Employee ID="11" Title="Teacher" Gender="F" />  
<Employee ID="15" Title="Teacher" Gender="F" />  
<Employee ID="3" Title="Teacher" Gender="F" />  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Les expressions de tri doivent être homogènes en ce qui concerne le type. Le contrôle s'effectue de façon statique.  
  
-   Le tri des séquences vides ne peut pas être contrôlé.  
  
-   Les mots clés empty least, empty greatest et collation de `order by` ne sont pas pris en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
