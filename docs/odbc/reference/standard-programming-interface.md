---
title: Interface de programmation standard | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ODBC [ODBC], database access
- SQL [ODBC], database access
- database access [ODBC]
- standardizing database access [ODBC], programming interface
- programming interface standardization [ODBC]
ms.assetid: a2fa727e-51f2-4123-ae25-0ee28e611231
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e820b626ce8e4f207885b8c7e5dd5cdc7050f960
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="standard-programming-interface"></a>Interface de programmation standard
L’interface de programmation est peut-être le plus évident candidat pour la normalisation. En fait, lorsque ODBC a été développé, ANSI et ISO déjà fournies aux normes pour embedded SQL et SQL modules. Bien qu’aucun niveau n’existe pour une base de données CLI, le groupe d’accès SQL : un consortium industriel de fournisseurs de base de données, a été vous envisagez de créer un. parties d’ODBC ultérieurement est devenue la base de leur travail.  
  
 L’une des exigences pour ODBC était qu’une seule application binaire devait travailler avec plusieurs SGBD. C’est pour cette raison que ODBC n’utilise pas les langues SQL ou le module incorporés. Bien que la langue dans les langues SQL et le module incorporés est normalisée, chacun est lié à precompilers des SGBD spécifiques. Par conséquent, les applications doivent être recompilées chaque SGBD et les fichiers binaires qui en résulte fonctionnent uniquement avec un SGBD unique. Bien que cela soit acceptable pour les applications dans des environnements de macroordinateur et de ses faible volume, il est interdit dans le monde de l’ordinateur personnel. Tout d’abord, il s’agit d’un cauchemar logistique pour fournir plusieurs versions de logiciels volumineux, emballées à des clients ; en second lieu, les applications doivent souvent accéder simultanément à plusieurs systèmes SGBD.  
  
 En revanche, une interface de niveau d’appel peut être implémentée par le biais des bibliothèques ou les pilotes de base de données qui résident sur chaque ordinateur local ; un autre pilote est requis pour chaque SGBD. Étant donné que les systèmes d’exploitation modernes peut charger ces bibliothèques (tels que les bibliothèques de liens dynamiques sur le système d’exploitation Microsoft® Windows®) au moment de l’exécution, une seule application peut accéder aux données à partir de différents SGBD sans recompilation et permettre également accéder aux données à partir de plusieurs bases de données simultanément. Comme les nouveaux pilotes de base de données deviennent disponibles, les utilisateurs peuvent installer uniquement ces sur leurs ordinateurs sans avoir à modifier, de recompiler ou de rétablir les liens de leurs applications de base de données. En outre, une interface de niveau d’appel est un bon candidat pour ODBC, car Windows : la plateforme pour laquelle ODBC a été initialement développé — déjà effectuées utilisent beaucoup de ces bibliothèques.
