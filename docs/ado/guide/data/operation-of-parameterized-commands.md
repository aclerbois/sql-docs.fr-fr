---
title: Fonctionnement des commandes paramétrées | Documents Microsoft
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
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
ms.assetid: 4fae0d54-83b6-4ead-99cc-bcf532daa121
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ce4d9977628e4024539a2e3e9fe8950513100620
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="operation-of-parameterized-commands"></a>Fonctionnement des commandes paramétrées
Si vous travaillez avec un enfant **Recordset**, en particulier par rapport à la taille du parent **Recordset**, mais avez besoin d’accéder uniquement quelques chapitres enfant, il peut s’avérer plus efficace d’utiliser un commande paramétrable.  
  
 A *commande non paramétrée* extrait l’intégralité du parent et enfant **jeux d’enregistrements**, ajoute une colonne de chapitre au parent, puis affecte une référence au chapitre enfant associé pour chaque ligne parente .  
  
 A *commande paramétrable* récupère l’intégralité du parent **Recordset**, mais n’extrait le chapitre **Recordset** lorsque vous accédez à la colonne de chapitre. Cette différence de la stratégie de récupération peut produire des gains de performance significatifs.  
  
 Par exemple, vous pouvez spécifier les éléments suivants :  
  
```  
SHAPE {SELECT * FROM customer}   
   APPEND ({SELECT * FROM orders WHERE cust_id = ?}   
   RELATE cust_id TO PARAMETER 0)  
```  
  
 Les tables parent et enfant partagent un nom de colonne commun, cust_id *.* Le *commande enfant* a un « ? » espace réservé, auquel la clause RELATE fait référence (autrement dit, «... » PARAMÈTRE 0 »).  
  
> [!NOTE]
>  La clause de paramètre se rapporte uniquement à la syntaxe de commande de forme. Il n’est pas associé à un ADO [paramètre](../../../ado/reference/ado-api/parameter-object.md) objet ou le [paramètres](../../../ado/reference/ado-api/parameters-collection-ado.md) collection.  
  
 Lorsque la commande de mise en forme paramétrée est exécutée, les événements suivants se produisent :  
  
1.  Le *parent-command* est exécutée et renvoie un parent **Recordset** à partir de la table Customers.  
  
2.  Une colonne de chapitre est ajoutée au parent **Recordset**.  
  
3.  Lorsque vous accédez à la colonne de chapitre d’une ligne parente, le *commande enfant* est exécutée à l’aide de la valeur du champ customer.cust_id comme valeur du paramètre.  
  
4.  Toutes les lignes dans l’ensemble de lignes de fournisseur de données créé à l’étape 3 sont utilisés pour remplir l’enfant **Recordset**. Dans cet exemple, c'est-à-dire toutes les lignes de la table Orders dans lequel le cust_id est égal à la valeur du champ customer.cust_id. Par défaut, l’enfant **Recordset**s sont mises en cache sur le client jusqu'à ce que toutes les références au parent **Recordset** sont libérés. Pour modifier ce comportement, définissez la **Recordset** [propriété dynamique](../../../ado/reference/ado-api/ado-dynamic-property-index.md) **Cache Child Rows** à **False**.  
  
5.  Une référence aux lignes enfant extraites (autrement dit, le chapitre de l’enfant **Recordset**) est placé dans la colonne de chapitre de la ligne actuelle du parent **Recordset**.  
  
6.  Étapes 3 à 5 sont répétées lors de l’accès à la colonne de chapitre d’une autre ligne.  
  
 Le **Cache Child Rows** dynamique est définie sur **True** par défaut. Le comportement de mise en cache varie en fonction des valeurs de paramètre de la requête. Dans une requête avec un seul paramètre, l’enfant **Recordset** pour un paramètre donné valeur est mises en cache entre les requêtes pour un enfant avec la même valeur. Le code suivant illustre cela :  
  
```  
SCmd = "SHAPE {select * from customer} " & _  
         "APPEND({select * from orders where cust_id = ?} " & _  
         "RELATE cust_id TO PARAMETER 0) AS chpCustOrder"  
Rst1.Open sCmd, Cnn1  
Set RstChild = Rst1("chpCustOrder").Value  
Rst1.MoveNext      ' Next cust_id passed to Param 0, & new rs fetched   
                   ' into RstChild.  
Rst1.MovePrevious  ' RstChild now holds cached rs, saving round trip.  
```  
  
 Dans une requête avec deux ou plusieurs paramètres, un enfant mis en cache est utilisé uniquement si toutes les valeurs de paramètre correspondent aux valeurs mises en cache.  
  
## <a name="parameterized-commands-and-complex-parent-child-relations"></a>Commandes paramétrées et Relations enfant-Parent complexes  
 Outre l’utilisation des commandes paramétrées pour améliorer les performances d’une hiérarchie de type équijointure, les commandes paramétrables peuvent être utilisées pour prendre en charge les relations parent-enfant plus complexes. Par exemple, considérons une base de données et la ligue peu avec deux tables : une comprenant les équipes (team_id, nom_équipe) et l’autre des jeux (date, home_team, visiting_team).  
  
 À l’aide d’une hiérarchie non paramétrée, il n’existe aucun moyen pour relier les tables de jeux et les équipes de sorte que l’enfant **Recordset** pour chaque équipe contienne sa planification complète. Vous pouvez créer des chapitres contenant le calendrier à domicile ou la planification de la feuille de route, mais pas les deux. Il s’agit, car la clause RELATE vous limite à des relations parent-enfant de l’écran (pc1 = cc1) AND (pc2 = pc2). Par conséquent, si votre commande inclut « RELATE team_id TO home_team, team_id TO visiting_team », vous obtiendrez uniquement les jeux où une équipe lu lui-même. Vous souhaitez "(team_id=home_team) ou (team_id = visiting_team) », mais le fournisseur Shape ne prend pas en charge la clause OR.  
  
 Pour obtenir le résultat souhaité, vous pouvez utiliser une commande paramétrée. Par exemple :  
  
```  
SHAPE {SELECT * FROM teams}   
APPEND ({SELECT * FROM games WHERE home_team = ? OR visiting_team = ?}   
        RELATE team_id TO PARAMETER 0,   
               team_id TO PARAMETER 1)   
```  
  
 Cet exemple exploite la plus grande souplesse de la clause SQL WHERE pour obtenir le résultat que vous avez besoin.  
  
> [!NOTE]
>  Lorsque à l’aide des clauses WHERE, paramètres ne peuvent pas utiliser les types de données SQL pour text, ntext et image ou une erreur se produit, qui contient la description suivante : `Invalid operator for data type`.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](../../../ado/guide/data/data-shaping-example.md)   
 [Grammaire de mise en forme formelle](../../../ado/guide/data/formal-shape-grammar.md)   
 [Généralités sur les commandes SHAPE](../../../ado/guide/data/shape-commands-in-general.md)
