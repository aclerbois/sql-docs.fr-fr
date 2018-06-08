---
title: Identificateurs (DMX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- DMX
helpviewer_keywords:
- Data Mining Extensions [Analysis Services], identifiers
- delimited identifiers [DMX]
- DMX [Analysis Services], identifiers
- identifiers [DMX]
- regular identifiers [DMX]
- names [DMX]
ms.assetid: fbb487a7-1b89-482a-977e-f079379d44fc
caps.latest.revision: 35
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 4db8e03ca5267941cca1f909e88afa7ceec7e96c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="identifiers-dmx"></a>Identificateurs (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Tous les objets de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] doit avoir un identificateur. Le nom d'un objet constitue son identificateur. Les serveurs, les bases de données et les objets de base de données tels que sources de données, vues de source de données, cubes, dimensions, modèles d'exploration de données, etc., ont tous un identificateur.  
  
 Dans DMX (Data Mining Extensions), il existe deux catégories d'identificateurs :  
  
-   [Identificateurs réguliers](#RegularIdentifiers)  
  
-   [Identificateurs délimités](#DelimitedIdentifiers)  
  
 L'identificateur d'un objet se crée lorsque vous définissez l'objet. Ensuite, vous utilisez l’identificateur pour référencer l’objet. Les identificateurs doivent avoir un maximum de 100 caractères.  
  
##  <a name="RegularIdentifiers"></a> Identificateurs réguliers  
 Les identificateurs réguliers dans DMX respectent les règles [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] relatives au format des identificateurs. Ils ne nécessitent pas de délimiteurs. Voici un exemple d’une instruction DMX qui utilise un identificateur régulier, non délimité par des virgules :  
  
```  
SELECT * FROM Clustering.CONTENT;  
```  
  
### <a name="rules-for-regular-identifiers"></a>Règles pour identificateurs réguliers  
 Voici les règles relatives au format des identificateurs réguliers :  
  
1.  Le premier caractère d'un identificateur régulier doit être l'un des suivants :  
  
    -   Des lettres définies par Unicode Standard 2.0. Cela inclut les caractères latins de a à z et de A à Z, et les lettres d'autres langues.  
  
    -   Un caractère de soulignement (_).  
  
2.  Les caractères suivants peuvent être :  
  
    -   Lettres définies dans Unicode Standard 2.0.  
  
    -   Des nombres décimaux de Basic Latin ou d'autres scripts nationaux.  
  
    -   Un caractère de soulignement (_).  
  
3.  L'identificateur ne doit pas être un mot réservé DMX. Dans DMX, les mots réservés ne respectent pas la casse des caractères. Pour plus d’informations, consultez [mots clés réservés &#40;DMX&#41;](../dmx/reserved-keywords-dmx.md).  
  
4.  L'identificateur ne peut contenir ni espaces insérés ni caractères spéciaux.  
  
 Vous devez placer entre crochets les identificateurs qui ne respectent pas ces règles lorsque vous les utilisez dans des instructions DMX.  
  
##  <a name="DelimitedIdentifiers"></a> Identificateurs délimités  
 Les identificateurs délimités sont placés entre crochets ([ ]).  Voici l'exemple d'une instruction DMX avec un identificateur délimité qui respecte ces règles.  
  
```  
SELECT * FROM [Marketing_Clusters].CONTENT;  
```  
  
 S'il ne respecte pas les règles relatives au format des identificateurs réguliers, il doit toujours être délimité. Voici l'exemple d'une instruction DMX avec un identificateur délimité contenant un espace :  
  
```  
SELECT * FROM [Targeted Mailing].CONTENT;  
```  
  
 Utilisez les identificateurs délimités dans les cas suivants :  
  
-   Lorsque vous utilisez des mots réservés pour des noms d'objet ou des parties de noms d'objet.  
  
     Il est conseillé de ne pas utiliser de mots clés réservés en tant que noms d'objet. Bases de données que vous mettez à niveau des versions antérieures de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] peuvent contenir des identificateurs qui incluent des mots qui n’étaient pas réservés dans la version antérieure de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , mais qui sont des mots réservés pour[!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]. Vous pouvez utiliser un identificateur délimité pour faire référence à ce type d'objet jusqu'à ce que vous puissiez renommer l'objet.  
  
-   Lorsque vous utilisez des caractères non répertoriés comme identificateurs qualifiés.  
  
     [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] permet d'utiliser tout caractère de la page de codes en cours dans un identificateur délimité ; cependant, l'utilisation intempestive de caractères spéciaux dans un nom d'objet peut rendre difficile la lecture et la maintenance des instructions DMX.  
  
### <a name="rules-for-delimited-identifiers"></a>Règles pour identificateurs délimités  
 Voici les règles relatives au format des identificateurs délimités :  
  
1.  Les identificateurs délimités peuvent contenir le même nombre de caractères que les identificateurs réguliers (de 1 à 100 caractères, sans compter les caractères de délimitation).  
  
2.  Le corps de l'identificateur peut contenir n'importe quelle combinaison de caractères dans la page de codes en cours, y compris les caractères de délimitation proprement dits. Si le corps de l'identificateur lui-même contient des caractères de délimitation, un traitement spécial est nécessaire :  
  
    -   Si le corps de l'identificateur contient un crochet gauche ([), aucun traitement supplémentaire n'est nécessaire.  
  
    -   Si le corps de l'identificateur contient un crochet droit (]), vous devez spécifier deux crochets droits (]]) pour le représenter dans la page de codes.  
  
### <a name="delimiting-identifiers-with-multiple-parts"></a>Identificateurs de délimitation en plusieurs parties  
 Lorsque vous utilisez un nom d'objet qualifié, vous pouvez être contraint de délimiter plusieurs des identificateurs qui le composent. Vous devez délimiter chaque identificateur individuellement.  
  
## <a name="see-also"></a>Voir aussi  
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; éléments de syntaxe](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence des opérateurs](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining Extensions &#40;DMX&#41; Conventions de syntaxe](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Structure et utilisation des requêtes de prédiction DMX](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Présentation de l’instruction DMX Select](../dmx/understanding-the-dmx-select-statement.md)  
  
  
