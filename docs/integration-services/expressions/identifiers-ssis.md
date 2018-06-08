---
title: Identificateurs (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: expressions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- regular identifiers [Integration Services]
- variables [Integration Services], expressions
- identifiers [Integration Services]
- expressions [Integration Services], variables
- lineage identifiers
- columns [Integration Services], identifiers
- names [Integration Services]
- expressions [Integration Services], identifiers
- qualified identifiers [Integration Services]
ms.assetid: 56af984d-88b4-4db8-b6a2-6b07315a699e
caps.latest.revision: 45
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6f62fd9e99c4359af731c0177e443baa4d1edae2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="identifiers-ssis"></a>Identificateurs (SSIS)
  Dans les expressions, les identificateurs sont des colonnes et des variables disponibles pour l'opération. Les expressions peuvent utiliser des identificateurs standard et qualifiés.  
  
## <a name="regular-identifiers"></a>Identificateurs standard  
 Un identificateur standard ne requiert aucun qualificateur supplémentaire. Par exemple, la colonne **MiddleName**de la table **Contacts** de la base de données **AdventureWorks** est un identificateur standard.  
  
 La dénomination des identificateurs standard doit suivre les règles suivantes :  
  
-   Le premier caractère du nom doit être une lettre, comme défini dans le standard Unicode 2.0, ou un trait de soulignement « _ ».  
  
-   Les caractères suivants peuvent être des lettres ou des nombres, comme défini dans le standard Unicode 2.0, le trait de soulignement « _ » et les caractères « @ », « $ » et « # ».  
  
> [!IMPORTANT]  
>  Il n'est pas autorisé d'incorporer dans les identificateurs standard des espaces et des caractères spéciaux, autres que ceux qui sont répertoriés. Pour recourir à des espaces et à des caractères spéciaux, vous devez utiliser un identificateur qualifié au lieu d'un identificateur standard.  
  
## <a name="qualified-identifiers"></a>Identificateurs qualifiés  
 Un identificateur qualifié est un identificateur délimité par des crochets. Un identificateur peut nécessiter un délimiteur, car son nom comprend des espaces ou son premier caractère n'est ni une lettre, ni un trait de soulignement. Par exemple, le nom de colonne **Middle Name** doit être qualifié par des crochets et exprimé sous la forme « [Middle Name] » dans une expression.  
  
 Si le nom de l'identificateur comprend des espaces ou qu'il ne s'agit pas d'un nom d'identificateur standard valide, l'identificateur doit être qualifié. L'évaluateur d'expression utilise les crochets ouvrant et fermant ( [] ) pour qualifier les identificateurs. Les crochets occupent la première et la dernière position de la chaîne. Par exemple, l'identificateur 5$> devient [5$>]. Les crochets peuvent être utilisés avec des noms de colonne, de variable et de fonction.  
  
 Si vous créez des expressions à l'aide des boîtes de dialogue du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , les identificateurs standard sont automatiquement placés entre crochets. Toutefois, les crochets sont requis uniquement si le nom comprend des caractères non valides. Par exemple, la colonne **MiddleName** est valide sans crochets.  
  
 Dans les expressions, vous ne pouvez pas référencer des noms de colonne comprenant des crochets. Par exemple, le nom de colonne **Colonne[1]** ne peut pas être utilisé dans une expression. Pour utiliser la colonne dans une expression, vous devez lui attribuer un nom dépourvu de crochets.  
  
## <a name="lineage-identifiers"></a>identificateur de lignage  
 Les expressions peuvent utiliser des identificateurs de lignage pour faire référence aux colonnes. Les identificateurs de lignage sont automatiquement affectés lorsque vous créez le package. Vous pouvez visualiser l'identificateur de lignage d'une colonne dans l'onglet **Propriétés de la colonne** de la boîte de dialogue **Éditeur avancé** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Si vous faites référence à une colonne à l'aide de son identificateur de lignage, celui-ci doit être préfixé du signe dièse ( # ). Par exemple, une colonne dont l'identificateur de lignage est 147 doit être référencée sous la forme « #147 ».  
  
 Si une expression est correctement analysée, l'évaluateur d'expression remplace les identificateurs de lignage par des noms de colonne dans la boîte de dialogue.  
  
## <a name="unique-column-names"></a>Noms de colonne uniques  
 Plusieurs composants utilisés dans un package peuvent exposer des colonnes de même nom. Si ces colonnes sont utilisées dans des expressions, toute ambiguïté doit être levée afin que celles-ci soient correctement analysées. L'évaluateur d'expression prend en charge la notation ponctuée afin d'identifier la source de la colonne. Par exemple, deux colonnes nommées **Age** sont renommées « **FlatFileSource.Age** » et « **OLEDBSource.Age**», ce qui indique que leurs sources sont FlatFileSource ou OLEDBSource. L'analyseur traite le nom qualifié complet en tant que nom de colonne unique.  
  
 Les noms de colonne et les noms de composant source sont qualifiés séparément. Les exemples suivants illustrent l'utilisation des crochets dans une notation ponctuée :  
  
-   Le nom du composant source comprend des espaces.  
  
    ```  
    [MySo urce].Age  
    ```  
  
-   Le premier caractère du nom de la colonne n'est pas valide.  
  
    ```  
    MySource.[#Age]  
    ```  
  
-   Les noms de la colonne et du composant source contiennent des caractères non valides.  
  
    ```  
    [MySource?].[#Age]  
    ```  
  
> [!IMPORTANT]  
>  Si les deux éléments en notation ponctuée figurent dans une paire de crochets, l'évaluateur d'expression interprète celle-ci comme un identificateur unique, et non pas comme une combinaison de colonnes source.  
  
## <a name="variables-in-expressions"></a>Variables dans des expressions  
 Les variables référencées dans les expressions doivent comprendre le préfixe « @ ». Par exemple, la variable **Counter** est référencée sous la forme @Counter. Le caractère « @ » ne fait pas partie du nom de la variable ; il permet simplement à l'évaluateur d'expression d'identifier la variable. Si vous créez des expressions à l'aide des boîtes de dialogue du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , le caractère « @ » est automatiquement ajouté au nom de la variable. Vous ne pouvez pas insérer des espaces entre le caractère « @ » et le nom de la variable.  
  
 Les noms de variable suivent les mêmes règles que ceux des autres identificateurs standard :  
  
-   Le premier caractère du nom doit être une lettre, comme défini dans le standard Unicode 2.0, ou un trait de soulignement « _ ».  
  
-   Les caractères suivants peuvent être des lettres ou des nombres, comme défini dans le standard Unicode 2.0, le trait de soulignement « _ » et les caractères « @ », « $ » et « # ».  
  
 Si un nom de variable contient des caractères autres que ceux répertoriés, vous devez placer la variable entre crochets. Par exemple, les noms de variable contenant des espaces doivent figurer entre crochets. Le crochet ouvrant suit le caractère « @ ». Par exemple, la variable **Mon nom** est référencée sous la forme « @[Mon nom] ». Vous ne pouvez pas insérer des espaces entre le nom de la variable et les crochets.  
  
> [!NOTE]  
>  Les noms des variables définies par l'utilisateur et des variables système respectent la casse.  
  
## <a name="unique-variable-names"></a>Noms de variable uniques  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend en charge les variables personnalisées et met à votre disposition un ensemble de variables système. Par défaut, les variables personnalisées appartiennent à l’espace de noms **User** , et les variables système à l’espace de noms **System** . Vous pouvez créer des espaces de noms supplémentaires pour les variables personnalisées et mettre à jour les noms d'espace de noms en fonction des besoins de votre application. Le générateur d'expressions répertorie dans tous les espaces de noms les variables qui sont dans leur portée respective.  
  
 Toutes les variables ont une portée et appartiennent à un espace de noms. Une variable a la portée d'un package ou celle d'un conteneur ou d'une tâche appartenant au package. Le générateur d'expressions du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ne répertorie que les variables comprises dans la portée. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  
  
 L'évaluateur d'expression ne peut correctement évaluer les expressions que si les variables utilisées dans celles-ci portent des noms uniques. Si un package utilise plusieurs variables de même nom, leurs espaces de noms doivent être différents. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] a un opérateur de résolution d'espace de noms, constitué de deux points (::), qui permet de qualifier une variable avec son espace de noms. Par exemple, l’expression suivante utilise deux variables nommées **Count**; l’une appartient à l’espace de noms **User** et l’autre à l’espace de noms **MyNamespace** .  
  
```  
@[User::Count] > @[MyNamespace::Count]  
```  
  
> [!IMPORTANT]  
>  Vous devez placer la combinaison de l'espace de noms et du nom de la variable qualifiée entre crochets pour que l'évaluateur d'expression puisse reconnaître la variable.  
  
 Si la variable **Count** de l’espace de noms **User** présente la valeur 10 et que la variable **Count** de l’espace de noms **MyNamespace** est définie sur 2, l’expression renvoie la valeur **true** , car l’évaluateur d’expression reconnaît deux variables différentes.  
  
 Si les noms de variable ne sont pas uniques, aucune erreur ne se produit. Par contre, l'évaluateur d'expression utilise une seule instance de la variable pour évaluer l'expression et renvoie un résultat incorrect. Par exemple, l’expression suivante est conçue pour comparer les valeurs (10 et 2) de deux variables **Count** distinctes, mais retourne la valeur **false** , car l’évaluateur d’expression utilise deux fois la même instance de la variable **Count** .  
  
```  
@Count > @Count  
```  
  
## <a name="related-content"></a>Contenu associé  
 Article technique, [SSIS Expression Cheat Sheet](http://go.microsoft.com/fwlink/?LinkId=746575), sur pragmaticworks.com  
  
  
