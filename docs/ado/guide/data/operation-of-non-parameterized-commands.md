---
title: Fonctionnement des commandes Non paramétrées | Documents Microsoft
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
- non-parameterized commands [ADO]
- data shaping [ADO], non-parameterized commands
ms.assetid: 9700e50a-9f17-4ba3-8afb-f750741dc6ca
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b468d55f9085c5201c1720887087b3ace94a6069
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="operation-of-non-parameterized-commands"></a>Fonctionnement des commandes Non paramétrées
Pour les commandes non paramétrées, toutes les commandes fournisseur sont exécutées et la **jeux d’enregistrements** sont créés lors de l’exécution de commande. Si la commande est exécutée de façon synchrone, toutes les **jeux d’enregistrements** sera entièrement remplie. Si un mode de remplissage asynchrone a été sélectionné, le degré de remplissage de la **jeux d’enregistrements** dépend du mode de remplissage et de la taille de la **jeux d’enregistrements**.  
  
 Par exemple, le *parent-command* peut retourner un **Recordset** des clients d’une société à partir d’une table de clients et le *commande enfant* pourrait retourner un **Recordset** des commandes de tous les clients à partir d’une table Orders.  
  
```  
SHAPE {SELECT * FROM Customers}   
   APPEND ({SELECT * FROM Orders} AS chapOrders   
   RELATE customerID TO customerID)  
```  
  
 Pour les relations parent-enfant non paramétrées, chaque parent et l’enfant **Recordset** objet doit avoir une colonne en commun pour les associer. Les colonnes sont nommées dans la clause RELATE, *colonne-parent* premier, puis *-colonne enfant*. Les colonnes peuvent avoir des noms différents dans leurs respectifs **Recordset** objets mais il doit faire référence aux mêmes informations afin de spécifier une relation explicite. Par exemple, le **clients** et **commandes Recordset** objets peut les deux avoir un champ customerID. Étant donné que l’appartenance de l’enfant **Recordset** est déterminée par la commande fournisseur, l’enfant **Recordset** peut contenir des lignes orphelines. Ces lignes sont inaccessibles sans la mise en forme supplémentaires.  
  
 Mise en forme des données ajoute une colonne de chapitre au parent **Recordset**. Les valeurs dans la colonne de chapitre sont des références aux lignes de l’enfant **Recordset**, qui répondent à la clause RELATE. Autrement dit, la même valeur est dans le *colonne-parent* d’une ligne parent donné est dans le *-colonne enfant* de toutes les lignes de l’enfant de chapitre. Lorsque plusieurs clauses TO sont utilisées dans la même clause RELATE, ils sont implicitement associées à l’aide d’un opérateur AND. Si les colonnes parent de la clause RELATE ne constituent pas une clé pour le parent **Recordset**, une seule ligne enfant peut avoir plusieurs lignes parent.  
  
 Lorsque vous accédez à la référence dans la colonne de chapitre, ADO extrait automatiquement le **Recordset** représenté par la référence. Notez que dans une commande non paramétrée, bien que l’intégralité de l’enfant **Recordset** a été récupéré, le chapitre ne présente qu’un sous-ensemble de lignes.  
  
 Si la colonne ajoutée n’a pas *alias-chapitre*, un nom sera généré automatiquement pour celui-ci. A [champ](../../../ado/reference/ado-api/field-object.md) de l’objet pour la colonne est ajoutée à la **Recordset** l’objet [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection et son type de données seront **adChapter**.  
  
 Pour plus d’informations sur la navigation dans une liste hiérarchique **Recordset**, consultez [accès aux lignes d’un objet Recordset hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](../../../ado/guide/data/data-shaping-example.md)   
 [Grammaire de mise en forme formelle](../../../ado/guide/data/formal-shape-grammar.md)   
 [Généralités sur les commandes SHAPE](../../../ado/guide/data/shape-commands-in-general.md)
