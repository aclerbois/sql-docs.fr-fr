---
title: Requêtes avec paramètres (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-visual-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- parameter queries [SQL Server]
ms.assetid: 4897c41a-324a-47b8-a30b-cbc9e9e19a8b
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 73a075e41e51330d801e977a521c6babdafc86d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parameter-queries-visual-database-tools"></a>Requêtes avec paramètres (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Dans certains cas, il est utile de créer une requête si vous devez souvent y faire appel, mais chaque fois avec une valeur différente. Par exemple, il se peut que vous exécutiez fréquemment une requête pour rechercher tous les `title_ids` écrits par un auteur. Chaque demande pourrait utiliser la même requête, mais l'ID ou le nom de l'auteur serait différent à chaque fois.  
  
Pour que la requête prenne une valeur différente selon le cas, incluez des paramètres dans sa création. Un paramètre est un espace réservé, qui sera rempli par une valeur fournie au moment de l'exécution de la requête. Dans l'instruction SQL de l'exemple suivant, « ? » représente le paramètre de l'ID de l'auteur :  
  
```  
SELECT title_id  
FROM titleauthor  
WHERE (au_id = ?)  
```  
  
## <a name="where-you-can-use-parameters"></a>Où utiliser des paramètres  
Les paramètres sont des espaces réservés à des littéraux — à remplacer par du texte ou des valeurs numériques. La plupart du temps, les paramètres sont utilisés en tant qu'espaces réservés dans des conditions de recherche de lignes individuelles ou de groupes (c'est-à-dire dans les clauses WHERE ou HAVING d'une instruction SQL).  
  
Dans les expressions, vous pouvez utiliser les paramètres comme des espaces réservés. Par exemple, il est possible de calculer des prix avec remise en fournissant une remise différente à chaque exécution de la requête. Dans ce cas, vous pourriez spécifier l'expression suivante :  
  
```  
(price * ?)  
```  
  
## <a name="specifying-unnamed-and-named-parameters"></a>Spécification de paramètres nommés et sans nom  
Vous pouvez spécifier deux types de paramètres : nommés et sans nom. Un paramètre sans nom est un point d'interrogation (?) que vous pouvez placer dans la requête à l'endroit où vous serez invité à indiquer une valeur littérale. Par exemple, si vous utilisez un paramètre sans nom pour rechercher l’ID d’un auteur dans la table `titleauthor` , le [volet SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) peut proposer l’instruction suivante :  
  
```  
SELECT title_id  
FROM titleauthor  
WHERE (au_id = ?)  
```  
  
Quand vous exécutez la requête dans le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md), la boîte de dialogue [Paramètres de la requête](../../ssms/visual-db-tools/query-parameters-dialog-box-visual-database-tools.md) s’affiche avec « ? » comme nom du paramètre.  
  
Une autre méthode consiste à assigner un nom à un paramètre. Cela s'avère particulièrement utile si la requête doit comporter plusieurs paramètres. Par exemple, voici l'instruction proposée dans le volet SQL si vous utilisez des paramètres nommés pour rechercher le prénom et le nom d'un auteur dans la table `authors` :  
  
```  
SELECT au_id  
FROM authors  
WHERE au_fname = %first name% AND  
      au_lname = %last name%  
```  
  
> [!TIP]  
> Vous devez définir les caractères de préfixe et de suffixe avant de créer une requête employant un paramètre nommé.  
  
Quand vous exécutez la requête dans le Concepteur de requêtes et de vues, la boîte de dialogue [Paramètres de la requête](../../ssms/visual-db-tools/query-parameters-dialog-box-visual-database-tools.md) s’affiche avec la liste des paramètres nommés.  
  
## <a name="see-also"></a> Voir aussi  
[Requête avec des paramètres &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-parameters-visual-database-tools.md)  
[Types de requêtes pris en charge &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/supported-query-types-visual-database-tools.md)  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
