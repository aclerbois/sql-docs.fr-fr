---
title: XQuery et statique en tapant | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
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
- XQuery, static typing
- static typing
- checking static types
- inference [XQuery]
ms.assetid: d599c791-200d-46f8-b758-97e761a1a5c0
caps.latest.revision: 38
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 03958536a329b59a15ceade13cac1598d97dd346
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xquery-and-static-typing"></a>XQuery et le typage statique
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  XQuery dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est un langage typé statiquement. Autrement dit, il génère des erreurs de type lors de la compilation des requêtes dès qu'une expression renvoie une valeur dotée d'un type ou d'une cardinalité qu'une fonction ou un opérateur spécifique n'accepte pas. De plus, la vérification des types statiques peut également déceler si une expression de chemin d'accès faisant référence à un document XML typé a été mal typée. Le compilateur XQuery commence par une phase de normalisation au cours de laquelle s'ajoutent les opérations implicites (atomisation, par exemple), puis effectue l'inférence de type statique et la vérification des types statiques.  
  
## <a name="static-type-inference"></a>Inférence de type statique  
 L'inférence de type statique consiste à déterminer le type de retour d'une expression. Pour cela, il faut prendre en compte les types statiques des paramètres d'entrée et la sémantique statique de l'opération, puis inférer le type statique du résultat. Par exemple, le type statique de l'expression 1 + 2,3 est déterminé de la manière suivante :  
  
-   Le type statique de 1 est **xs : Integer** et le type statique de 2,3 est **xs : decimal**. Selon la sémantique dynamique, la sémantique statique de la **+** opération convertit l’entier décimal et retourne une valeur décimale. Le type statique inféré serait alors **xs : decimal**.  
  
 En cas d'instances XML non typées, il existe des types particuliers pour indiquer que les données ne sont pas typées. Ces informations servent à vérifier le type statique et à effectuer certaines conversions implicites.  
  
 En cas de données typées, le type d'entrée est inféré à partir de la collection de schémas XML qui contraint l'instance du type de données XML. Par exemple, si le schéma autorise uniquement les éléments de type **xs : Integer**, les résultats d’une expression de chemin d’accès à l’aide de cet élément seront zéro ou plusieurs éléments de type **xs : Integer**. Cela s’exprime actuellement à l’aide d’une expression comme `element(age,xs:integer)*` où l’astérisque (\*) indique la cardinalité du type résultant. Dans cet exemple, l’expression peut entraîner zéro ou plusieurs éléments de nom « age » et le type **xs : Integer**. Autres cardinalités correspondent exactement à l’un et sont exprimées en utilisant le nom de type seulement, zéro ou un et exprimée à l’aide d’un point d’interrogation (**?**) et 1 ou plus et à l’aide d’un signe plus (**+**).  
  
 Il arrive parfois que l'inférence de type statique induise le renvoi d'une séquence vide par une expression. Par exemple, si une expression de chemin d’accès sur un type de données XML typé recherche un \<nom > élément à l’intérieur d’un \<client > élément (/ customer/name), mais le schéma n’autorise pas un \<nom > à l’intérieur d’un \<client >, l’inférence de type statique déduit que le résultat sera vide. Cela permet de détecter des requêtes incorrectes et est signalée comme une erreur statique, sauf si l’expression a été () ou **(()) de données**.  
  
 Des règles détaillées d'inférence sont fournies dans la sémantique formelle de la spécification XQuery. Microsoft les a légèrement modifiées pour les rendre compatibles avec les instances du type de données XML typé. Suite à la modification la plus importante apportée à cette norme, le nœud de document implicite connaît le type de l'instance du type de données XML. Par conséquent, une expression de chemin d'accès de la forme /age sera typée précisément en fonction de cette information.  
  
 À l’aide de [modèles du Générateur de profils SQL Server et les autorisations](../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md), vous pouvez voir les types statiques retournés en tant que partie de compilations de requêtes. Pour les consulter, votre trace doit inclure l'événement XQuery Static Type dans la catégorie d'événement TSQL.  
  
## <a name="static-type-checking"></a>Vérification des types statiques  
 La vérification des types statiques permet de s'assurer que, lors de l'exécution, seules des valeurs de type compatible avec l'opération seront reçues. Dans la mesure où les types n'ont pas à être vérifiés lors de l'exécution, des erreurs potentielles peuvent être détectées à l'avance au moment de la compilation. Les performances s'en trouvent donc améliorées. En revanche, le typage statique demande à l'auteur de la requête un soin supplémentaire dans la formulation de la requête.  
  
 Voici les types appropriés qu'il est possible d'utiliser :  
  
-   Types explicitement autorisés par une fonction ou une opération.  
  
-   Sous-type d'un type explicitement autorisé.  
  
 Les sous-types sont définis en fonction des règles de sous-typage permettant de les dériver par restriction ou extension du schéma XML. Par exemple, un type S est un sous-type du type T si toutes les valeurs dotées du type S sont aussi des instances du type T.  
  
 De plus, toutes les valeurs entières sont aussi des valeurs décimales, conformément à la hiérarchie du type de schéma XML. En revanche, toutes les valeurs décimales ne sont pas des entiers. Si un entier est un sous-type de décimal, la réciproque est fausse. Par exemple, le **+** opération autorise uniquement les valeurs de certains types, tels que les types numériques **xs : Integer**, **xs : decimal**, **xs : float**, et **xs : double**. Si les valeurs des autres types, tels que **xs : String**, sont passés, l’opération génère une erreur de type. Cela s'appelle le typage fort. Des valeurs d'autres types, comme le type atomique utilisé pour indiquer la présence de XML non typé, peuvent être converties implicitement en une valeur d'un type accepté par l'opération. Cela s'appelle le typage faible.  
  
 Si elle est obligatoire après une conversion implicite, la vérification des types statiques permet de s'assurer que seules les valeurs dotées de types autorisés et d'une cardinalité correcte sont transmises à une opération. Pour « chaîne » + 1, il reconnaît que le type statique de « chaîne » est **xs : String**. Étant donné que cela n’est pas un type autorisé pour le **+** opération, une erreur de type est déclenchée.  
  
 En cas d'ajout du résultat d'une expression arbitraire E1 à une expression arbitraire E2 (E1 + E2), l'inférence de type statique détermine tout d'abord les types statiques de E1 et de E2, puis vérifie que ces types statiques sont autorisés avec l'opération. Par exemple, si le type statique de E1 peut être soit un **xs : String** ou **xs : Integer**, la vérification des types statiques génère une erreur de type, bien que certaines valeurs au moment de l’exécution peut-être être des entiers. Ce serait le cas si le type statique de E1 était **xs : Integer\***. Étant donné que la **+** opération accepte uniquement un seul nombre entier et E1 peut en renvoyer zéro ou supérieur à 1, la vérification des types statiques génère une erreur.  
  
 Comme nous l'avons déjà mentionné, il arrive fréquemment que l'inférence de type déduise un type plus large que ce que sait l'utilisateur à l'égard des données transmises. Dans ces cas-là, l'utilisateur doit réécrire la requête. Certains cas par défaut sont les suivantes :  
  
-   Le type infère un type plus général tel qu'un supertype ou une union de types. Si le type est un type atomique, vous devez utiliser l'expression cast ou la fonction constructor pour indiquer le type statique réel. Par exemple, si le type inféré de l’expression E1 est un choix entre **xs : String** ou **xs : Integer** et l’addition requiert **xs : Integer**, vous devez écrire `xs:integer(E1) + E2` au lieu de `E1+E2`. Cette expression peut échouer au moment de l’exécution si une valeur de chaîne qui ne peut pas être casté en **xs : Integer**. En revanche, l'expression passera désormais la vérification des types statiques. Cette expression est mappée avec la séquence vide.  
  
-   Le type infère une cardinalité supérieure à celle que les données contiennent réellement. Cela se produit fréquemment, car la **xml** type de données peut contenir plusieurs éléments de niveau supérieur, et une collection de schémas XML ne peut pas contraindre cela. Afin de réduire le type statique et de garantir qu'il n'y a en fait qu'une seule et unique valeur transmise, vous devez utiliser le prédicat positionnel `[1]`. Par exemple, pour ajouter 1 à la valeur de l'attribut `c` de l'élément `b` situé sous l'élément a de niveau supérieur, vous devez `write (/a/b/@c)[1]+1`. En outre, le mot clé DOCUMENT peut être utilisé avec une collection de schémas XML.  
  
-   Certaines opérations perdent les informations de type lors de l'inférence. Par exemple, si le type d’un nœud ne peut pas être déterminé, il devient **anyType**. Il n'est pas implicitement converti en un autre type. Ces conversions se produisent le plus souvent pendant la navigation à l'aide de l'axe parent. Vous devez éviter d'utiliser ce type d'opérations et réécrire la requête si l'expression crée une erreur de type statique.  
  
## <a name="type-checking-of-union-types"></a>Contrôle des types union  
 La manipulation des types union demande un soin particulier du fait du contrôle du type. Deux des problèmes rencontrés sont expliqués dans les exemples suivants.  
  
### <a name="example-function-over-union-type"></a>Exemple : fonction sur un type union  
 Considérez une définition d'élément pour <`r`> d'un type union :  
  
```  
<xs:element name="r">  
<xs:simpleType>  
   <xs:union memberTypes="xs:int xs:float xs:double"/>  
</xs:simpleType>  
</xs:element>  
```  
  
 Dans le contexte XQuery, la fonction « moyenne » `fn:avg (//r)` retourne une erreur statique, car le compilateur XQuery ne peut pas ajouter des valeurs de types différents (**xs : int**, **xs : float** ou **xs : double**) pour le <`r`> éléments dans l’argument de **fn:avg()**. Pour résoudre ce problème, réécrivez l'appel de fonction sous la forme `fn:avg(for $r in //r return $r cast as xs:double ?)`.  
  
### <a name="example-operator-over-union-type"></a>Exemple : opérateur sur un type union  
 L'opération addition (« + ») requiert les types exacts des opérandes. Par conséquent, l'expression `(//r)[1] + 1` retourne une erreur statique qui a la définition de type décrite précédemment pour l'élément <`r`>. Une solution consiste à la réécrire sous la forme `(//r)[1] cast as xs:int? +1`, où « ?  » indique 0 ou 1 occurrence. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] requiert « cast as » avec « ? », parce qu'une conversion peut générer la séquence vide comme résultat des erreurs d'exécution.  
  
## <a name="see-also"></a>Voir aussi  
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
