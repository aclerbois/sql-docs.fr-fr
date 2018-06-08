---
title: Forme la Clause COMPUTE | Documents Microsoft
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
helpviewer_keywords:
- shape commands [ADO]
- compute clause [ADO]
- data shaping [ADO], COMPUTE clause
ms.assetid: 3fdfead2-b5ab-4163-9b1d-3d2143a5db8c
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 25d89db4052234482846dc752e5c0431bb517164
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="shape-compute-clause"></a>Clause COMPUTE de forme
Une clause COMPUTE de forme génère un parent **Recordset**, dont les colonnes sont constitués d’une référence à l’enfant **Recordset**; facultatif dont le contenu est chapitre, nouveau, ou des colonnes calculées, des colonnes ou le résultat de l’exécution des fonctions d’agrégation de l’enfant **Recordset** ou un préalablement mis en forme **Recordset**; et toutes les colonnes à partir de l’enfant **Recordset** répertoriées dans le paramètre facultatif par clause.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SHAPE child-command [AS] child-alias  
   COMPUTE child-alias [[AS] name], [appended-column-list]  
   [BY grp-field-list]  
```  
  
## <a name="description"></a> Description  
 Les parties de cette clause sont les suivantes :  
  
 *commande-enfant*  
 Se compose d’une des opérations suivantes :  
  
-   Une commande de requête entre accolades («{}») qui retourne un enfant **Recordset** objet. La commande est envoyée au fournisseur de données sous-jacent et sa syntaxe dépend des exigences de ce fournisseur. Ce sera généralement le langage SQL, même si ADO ne requiert pas de langage de requête particulier.  
  
-   Le nom d’un objet en forme **Recordset**.  
  
-   Une autre commande shape.  
  
-   Le mot-clé TABLE, suivi du nom d’une table dans le fournisseur de données.  
  
 *alias-enfant*  
 Un alias utilisés pour faire référence à la **Recordset** retourné par la *commande-enfant.* Le *alias-enfant* est requis dans la liste des colonnes dans la clause COMPUTE et définit la relation entre parent et enfant **Recordset** objets.  
  
 *appended-column-list*  
 Une liste dans laquelle chaque élément définit une colonne dans le parent généré. Chaque élément contient une colonne de chapitre, une nouvelle colonne, une colonne calculée ou une valeur obtenue à partir d’une fonction d’agrégation de l’enfant **Recordset**.  
  
 *grp-field-list*  
 Une liste de colonnes dans le parent et enfant **Recordset** objets qui spécifie la façon dont les lignes doivent être groupées dans l’enfant.  
  
 Pour chaque colonne dans la *groupe-field-list,* il existe une colonne correspondante dans l’enfant et parent **Recordset** objets. Pour chaque ligne de la page parente **Recordset**, le *liste de champs de groupe* colonnes ont des valeurs uniques et l’enfant **Recordset** référencé par le parent ligne se compose uniquement d’enfant les lignes dont *liste de champs de groupe* colonnes ont les mêmes valeurs que la ligne parente.  
  
 Si la clause BY est incluse, l’enfant **Recordset**de lignes seront regroupées selon les colonnes dans la clause COMPUTE. Le parent **Recordset** contient une ligne pour chaque groupe de lignes de l’enfant **Recordset**.  
  
 Si la clause BY est omise, l’intégralité de l’enfant **Recordset** est traité comme un seul groupe et le parent **Recordset** contient exactement une ligne. Cette ligne référence tout l’enfant **Recordset**. L’omission de la clause BY vous permet de calculer des agrégats de « total général » sur l’intégralité de l’enfant **Recordset**.  
  
 Par exemple :  
  
```  
SHAPE {select * from Orders} AS orders             COMPUTE orders, SUM(orders.OrderAmount) as TotalSales         
```  
  
 Quelle que soit la façon dont le parent **Recordset** est formé (à l’aide de COMPUTE ou APPEND), elle contient une colonne de chapitre est utilisée pour établir une relation à un enfant **Recordset**. Si vous le souhaitez, le parent **Recordset** peut également contenir des colonnes contenant des agrégats (SUM, MIN, MAX, etc.) sur les lignes enfants. Le parent et l’enfant **Recordset** peuvent contenir des colonnes qui contiennent une expression sur la ligne dans le **Recordset**, ainsi que les colonnes sont de nouveaux et initialement vides.  
  
## <a name="operation"></a>Opération  
 Le *commande enfant* est émis pour le fournisseur, qui retourne un enfant **Recordset**.  
  
 La clause COMPUTE spécifie les colonnes du parent **Recordset**, qui peut être une référence à l’enfant **Recordset**, un ou plusieurs agrégats, une expression calculée ou nouvelles colonnes. Si une clause BY est employée, les colonnes qu’elle définit sont également ajoutées au parent **Recordset**. La clause BY spécifie la façon dont les lignes de l’enfant **Recordset** sont regroupés.  
  
 Par exemple, supposons que la table, nommée données démographiques, qui se compose de champs State, City et Population. (Les chiffres de la table sont fournies uniquement, par exemple).  
  
|État|Ville|Remplissage|  
|-----------|----------|----------------|  
|WA|Seattle|700,000|  
|- ou -|Medford|200,000|  
|- ou -|Portland|400,000|  
|CA|Los Angeles|800,000|  
|CA|San Diego|600,000|  
|WA|Tacoma|500,000|  
|- ou -|Corvallis|300,000|  
  
 Maintenant, exécutez cette commande de la forme :  
  
```  
rst.Open  "SHAPE {select * from demographics} AS rs "  & _  
          "COMPUTE rs, SUM(rs.population) BY state", _  
           objConnection  
```  
  
 Cette commande ouvre une forme **Recordset** à deux niveaux. Le niveau parent est un généré **Recordset** avec une colonne d’agrégation (`SUM(rs.population)`), une colonne référençant l’enfant **Recordset** (`rs`) et une colonne de regroupement des enfants **Recordset** (`state`). Le niveau enfant est la **Recordset** retourné par la commande de requête (`select * from demographics`).  
  
 L’enfant **Recordset** les lignes de détails sont groupés par état, mais dans aucun ordre particulier. Autrement dit, les groupes ne seront pas dans l’ordre alphabétique ou numérique. Si vous souhaitez que le parent **Recordset** pour être classés, vous pouvez utiliser la **Recordset Sort** méthode pour commander le parent **Recordset**.  
  
 Vous pouvez désormais accéder le parent **Recordset** et accéder aux détails de l’enfant **Recordset** objets. Pour plus d’informations, consultez [accès aux lignes d’un objet Recordset hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
## <a name="resultant-parent-and-child-detail-recordsets"></a>Parent résultant et jeux d’enregistrements de détail enfants  
  
### <a name="parent"></a>Parent  
  
|SUM (rs. Remplissage)|rs|État|  
|---------------------------|--------|-----------|  
|1,300,000|Référence à enfant 1|CA|  
|1,200,000|Référence à enfant 2|WA|  
|1,100,000|Référence à enfant 3|- ou -|  
  
## <a name="child1"></a>Child1  
  
|État|Ville|Remplissage|  
|-----------|----------|----------------|  
|CA|Los Angeles|800,000|  
|CA|San Diego|600,000|  
  
## <a name="child2"></a>Enfant2  
  
|État|Ville|Remplissage|  
|-----------|----------|----------------|  
|WA|Seattle|700,000|  
|WA|Tacoma|500,000|  
  
## <a name="child3"></a>Child3  
  
|État|Ville|Remplissage|  
|-----------|----------|----------------|  
|- ou -|Medford|200,000|  
|- ou -|Portland|400,000|  
|- ou -|Corvallis|300,000|  
  
## <a name="see-also"></a>Voir aussi  
 [L’accès aux lignes dans un jeu d’enregistrements hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)   
 [Vue d’ensemble de la mise en forme des données](../../../ado/guide/data/data-shaping-overview.md)   
 [Objet Field](../../../ado/reference/ado-api/field-object.md)   
 [Grammaire de mise en forme formelle](../../../ado/guide/data/formal-shape-grammar.md)   
 [Objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Fournisseurs requis pour la mise en forme des données](../../../ado/guide/data/required-providers-for-data-shaping.md)   
 [Clause APPEND de forme](../../../ado/guide/data/shape-append-clause.md)   
 [En général, les commandes de forme](../../../ado/guide/data/shape-commands-in-general.md)   
 [Value (propriété) (ADO)](../../../ado/reference/ado-api/value-property-ado.md)   
 [Fonctions Visual Basic pour Applications](../../../ado/guide/data/visual-basic-for-applications-functions.md)
