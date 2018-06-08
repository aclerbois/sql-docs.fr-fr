---
title: Extensions Visual C++ pour ADO | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- ADO, Visual C++
- Visual C++ [ADO], VC++ extensions for ADO
ms.assetid: 2952ece0-7217-4448-bb09-f6b64f43b7e2
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d27cc7776c59364ebc0b69c4872dc8b78ee51116
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="visual-c-extensions"></a>Extensions Visual C++
À l’aide de la méthode préférée de programmation ADO avec Visual C++ est la **#import** directive, comme indiqué dans [programmation ADO Visual C++](../../../ado/guide/appendixes/visual-c-ado-programming.md). Toutefois, les versions antérieures de l’objet ADO inclus dans une autre méthode de programmation à l’aide de Visual C++ : les Extensions Visual C++. Cette section décrit cette fonctionnalité pour les utilisateurs qui doivent conserver les Extensions Visual C++, mais le nouveau code ADO doit être écrit à l’aide de #**importer**.

 Parmi les plus fastidieux travaux Visual C++ les programmeurs sont confrontés lors de la récupération des données avec ADO sont conversion des données retournées comme un type de données VARIANT en un type de données C++ et puis de stocker les données converties dans une classe ou structure. De plus, la récupération de données C++ via un type de données VARIANT diminue les performances.

 ADO fournit une interface qui prend en charge la récupération des données dans des types de données C/C++ natifs sans passer par une variante et fournit également des macros de préprocesseur qui simplifient l’utilisation de l’interface. Le résultat est un outil flexible qui est plus facile à utiliser et hautes performances.

 Un scénario de client C/C++ classique consiste à lier un enregistrement dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) à une struct C/C++ ou d’une classe contenant des types C/C++ natifs. Lorsque vous passez par variantes, cela implique l’écriture de code de la conversion de type VARIANT en types C/C++ natifs. Les Extensions Visual C++ pour ADO sont ciblées sur facilite ce scénario pour les programmeurs Visual C++.

 Consultez les rubriques suivantes pour en savoir plus sur les Extensions Visual C++ pour ADO.

-   [À l’aide des Extensions Visual C++ pour ADO](../../../ado/guide/appendixes/using-visual-c-extensions.md)

-   [En-tête d’extensions Visual C++](../../../ado/guide/appendixes/visual-c-extensions-header.md)

-   [ADO avec exemple d’Extensions Visual C++](../../../ado/guide/appendixes/visual-c-extensions-example.md)

## <a name="see-also"></a>Voir aussi
 [INDEX de la syntaxe Visual C++ pour COM](../../../ado/reference/ado-api/ado-for-visual-c-syntax-index-for-com.md) [exemple d’Extensions Visual C++](../../../ado/guide/appendixes/visual-c-extensions-example.md) [à l’aide des Extensions Visual C++](../../../ado/guide/appendixes/using-visual-c-extensions.md) [en-tête d’Extensions Visual C++](../../../ado/guide/appendixes/visual-c-extensions-header.md)
