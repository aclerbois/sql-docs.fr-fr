---
title: Manipulation de données UDT | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- data deletions [CLR integration]
- data insertions [CLR integration]
- CONVERT function
- data updates [CLR integration]
- comparing data
- updating data [CLR integration]
- removing data
- IsByteOrdered property
- variables [CLR integration]
- data manipulation [CLR integration]
- user-defined types [CLR integration], data manipulation
- deleting data
- UDTs [CLR integration], data manipulation
- invoking UDT methods
- inserting data
ms.assetid: 51b1a5f2-7591-4e11-bfe2-d88e0836403f
caps.latest.revision: 14
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 754687e8fd100950b851f50d34e14aa968da37dc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-user-defined-types---manipulating-udt-data"></a>Utilisation des Types définis par l’utilisateur - de manipulation de données UDT
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] ne fournit aucune syntaxe spécialisée pour les instructions INSERT, UPDATE ou DELETE lors de la modification de données dans des colonnes de type défini par l'utilisateur (UDT). Les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] CAST ou CONVERT sont utilisées pour convertir des types de données natives en type UDT.  
  
## <a name="inserting-data-in-a-udt-column"></a>Insertion de données dans une colonne UDT  
 Les éléments suivants [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions insèrent trois lignes d’exemples de données dans le **Points** table. Le **Point** type de données se compose de X et Y entier des valeurs qui sont exposées comme propriétés de l’UDT. Vous devez utiliser soit la fonction CAST ou CONVERT pour effectuer un cast de la virgule délimitée par des valeurs X et Y le **Point** type. Les deux premières instructions utilisent la fonction CONVERT pour convertir une valeur de chaîne pour le **Point** type et la troisième instruction utilise la fonction CAST :  
  
```  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
## <a name="selecting-data"></a>Sélection de données  
 L'instruction SELECT suivante sélectionne la valeur binaire du type UDT.  
  
```  
SELECT ID, PointValue FROM dbo.Points  
```  
  
 Pour afficher la sortie affichée dans un format lisible, appelez le **ToString** méthode de la **Point** UDT, qui convertit la valeur en sa représentation sous forme de chaîne.  
  
```  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points;  
```  
  
 Ce code produit les résultats suivants.  
  
```  
IDPointValue  
----------  
13,4  
21,5  
31,99  
```  
  
 Vous pouvez également utiliser les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] CAST et CONVERT pour obtenir les mêmes résultats.  
  
```  
SELECT ID, CAST(PointValue AS varchar)   
FROM dbo.Points;  
  
SELECT ID, CONVERT(varchar, PointValue)   
FROM dbo.Points;  
```  
  
 Le **Point** UDT expose ses coordonnées X et Y comme propriétés que vous pouvez ensuite sélectionner individuellement. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante sélectionne les coordonnées X et Y séparément :  
  
```  
SELECT ID, PointValue.X AS xVal, PointValue.Y AS yVal   
FROM dbo.Points;  
```  
  
 Les propriétés X et Y retournent une valeur entière qui est affichée dans le jeu de résultats.  
  
```  
IDxValyVal  
----------  
134  
215  
3199  
```  
  
## <a name="working-with-variables"></a>Utilisation de Variables  
 Vous pouvez utiliser des variables à l'aide de l'instruction DECLARE pour attribuer une variable à un type UDT. Les instructions suivantes attribuent une valeur à l’aide de la [!INCLUDE[tsql](../../includes/tsql-md.md)] définir l’instruction et afficher les résultats en appelant de l’UDT **ToString** méthode sur la variable :  
  
```  
DECLARE @PointValue Point;  
SET @PointValue = (SELECT PointValue FROM dbo.Points  
    WHERE ID = 2);  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 Le jeu de résultats affiche la valeur de la variable :  
  
```  
PointValue  
----------  
-1,5  
```  
  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes génèrent le même résultat en remplaçant SET par SELECT pour l'attribution de la variable :  
  
```  
DECLARE @PointValue Point;  
SELECT @PointValue = PointValue FROM dbo.Points  
    WHERE ID = 2;  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 La différence entre SELECT et SET pour l'attribution de la variable réside dans le fait que SELECT vous permet d'attribuer plusieurs variables dans une instruction SELECT, tandis que la syntaxe SET nécessite que chaque attribution de variable possède sa propre instruction SET.  
  
## <a name="comparing-data"></a>Comparaison de données  
 Vous pouvez utiliser des opérateurs de comparaison pour comparer des valeurs dans votre UDT si vous avez défini le **IsByteOrdered** propriété **true** lors de la définition de la classe. Pour plus d’informations, consultez [création d’un Type défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types.md).  
  
```  
SELECT ID, PointValue.ToString() AS Points   
FROM dbo.Points  
WHERE PointValue > CONVERT(Point, '2,2');  
```  
  
 Vous pouvez comparer des valeurs internes de l’UDT indépendamment de la **IsByteOrdered** paramètre si les valeurs elles-mêmes sont comparables. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante sélectionne des lignes où X est supérieur à Y :  
  
```  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points  
WHERE PointValue.X < PointValue.Y;  
```  
  
 Vous pouvez également utiliser des opérateurs de comparaison avec des variables, comme indiqué dans cette requête que recherche un PointValue correspondant.  
  
```  
DECLARE @ComparePoint Point;  
SET @ComparePoint = CONVERT(Point, '3,4');  
SELECT ID, PointValue.ToString() AS MatchingPoint   
FROM dbo.Points  
WHERE PointValue = @ComparePoint;  
```  
  
## <a name="invoking-udt-methods"></a>Appel de méthodes UDT  
 Vous pouvez également appeler des méthodes qui sont définies dans votre type UDT dans [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le **Point** classe contient trois méthodes, **Distance**, **DistanceFrom**, et **DistanceFromXY**. Pour les listes de code définissant ces trois méthodes, consultez [Coding User-Defined Types](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
 Les éléments suivants [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction appelle la **PointValue.Distance** méthode :  
  
```  
SELECT ID, PointValue.X AS [Point.X],   
    PointValue.Y AS [Point.Y],  
    PointValue.Distance() AS DistanceFromZero   
FROM dbo.Points;  
```  
  
 Les résultats sont affichés dans le **Distance** colonne :  
  
```  
IDXYDistance  
------------------------  
1345  
2155.09901951359278  
319999.0050503762308  
```  
  
 Le **DistanceFrom** méthode prend un argument de **Point** type de données et affiche la distance entre le point spécifié au PointValue :  
  
```  
SELECT ID, PointValue.ToString() AS Pnt,  
   PointValue.DistanceFrom(CONVERT(Point, '1,99')) AS DistanceFromPoint  
FROM dbo.Points;  
```  
  
 Les résultats affichent les résultats de la **DistanceFrom** pour chaque ligne dans la table :  
  
```  
ID PntDistanceFromPoint  
---------------------  
13,495.0210502993942  
21,594  
31,990  
```  
  
 Le **DistanceFromXY** méthode accepte les points individuellement en tant qu’arguments :  
  
```  
SELECT ID, PointValue.X as X, PointValue.Y as Y,   
PointValue.DistanceFromXY(1, 99) AS DistanceFromXY   
FROM dbo.Points  
```  
  
 Le jeu de résultats est le même que le **DistanceFrom** (méthode).  
  
## <a name="updating-data-in-a-udt-column"></a>Mise à jour de données dans une colonne UDT  
 Pour mettre à jour des données dans une colonne UDT, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] UPDATE. Vous pouvez également utiliser une méthode du type UDT pour mettre à jour l'état de l'objet. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante met à jour une ligne unique dans la table :  
  
```  
UPDATE dbo.Points  
SET PointValue = CAST('1,88' AS Point)  
WHERE ID = 3  
```  
  
 Vous pouvez également mettre à jour des éléments UDT séparément. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante met à jour uniquement la coordonnée Y :  
  
```  
UPDATE dbo.Points  
SET PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Si l’UDT a été défini avec la valeur d’ordre des octets **true**, [!INCLUDE[tsql](../../includes/tsql-md.md)] peut évaluer la colonne UDT dans une clause WHERE.  
  
```  
UPDATE dbo.Points  
SET PointValue = '4,5'  
WHERE PointValue = '3,4';  
```  
  
### <a name="updating-limitations"></a>Mise à jour de limitations  
 Vous ne pouvez pas mettre à jour plusieurs propriétés à la fois à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Par exemple, l'instruction UPDATE suivante échoue avec une erreur parce que vous ne pouvez pas utiliser le même nom de colonne à deux reprises dans une instruction UDATE.  
  
```  
UPDATE dbo.Points  
SET PointValue.X = 5, PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Pour mettre à jour chaque point individuellement, vous devez créer une méthode mutateur dans l'assembly UDT Point. Vous pouvez ensuite appeler la méthode mutateur pour mettre à jour l'objet dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] UPDATE, comme suit :  
  
```  
UPDATE dbo.Points  
SET PointValue.SetXY(5, 99)  
WHERE ID = 3  
```  
  
## <a name="deleting-data-in-a-udt-column"></a>Suppression de données dans une colonne UDT  
 Pour supprimer des données dans un type UDT, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] DELETE. L'instruction suivante supprime toutes les lignes dans la table qui correspondent aux critères spécifiés dans la clause WHERE. Si vous omettez la clause WHERE dans une instruction DELETE, toutes les lignes dans la table seront supprimées.  
  
```  
DELETE FROM dbo.Points  
WHERE PointValue = CAST('1,99' AS Point)  
```  
  
 Utilisez l'instruction UPDATE si vous souhaitez supprimer les valeurs dans une colonne UDT tout en laissant d'autres valeurs de lignes intactes. Cet exemple attribut la valeur null à PointValue.  
  
```  
UPDATE dbo.Points  
SET PointValue = null  
WHERE ID = 2  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation des Types définis par l’utilisateur dans SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)  
  
  
