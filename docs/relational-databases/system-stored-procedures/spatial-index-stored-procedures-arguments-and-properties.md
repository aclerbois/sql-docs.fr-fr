---
title: Propriétés d’Index Spatial et les arguments de procédures stockées | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], stored procedures
ms.assetid: ee26082b-c0ed-40ff-b5ad-f5f6b00f0475
caps.latest.revision: 10
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 20c747b51bd4fc20e21bedfed1f826005b1037a1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spatial-index-stored-procedures---arguments-and-properties"></a>Procédures - Arguments et les propriétés stockées d’Index spatial
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Cette rubrique documente les arguments et propriétés pour les procédures stockées d'index spatial.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
 Pour la syntaxe de procédures stockées d'index spatial spécifiques, consultez les rubriques suivantes :  
  
-   [sp_help_spatial_geometry_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)  
  
-   [sp_help_spatial_geometry_index_xml &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)  
  
-   [sp_help_spatial_geography_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)  
  
-   [sp_help_spatial_geography_index_xml &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-xml-transact-sql.md)  
  
## <a name="arguments"></a>Arguments  
 [  **@tabname =**] **'***tabname***'**  
 Spécifie le nom qualifié ou non qualifié de la table pour laquelle l'index spatial a été défini.  
  
 Les guillemets ne sont nécessaires que si une une table qualifiée est spécifiée. Si un nom qualifié complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données active. *tabname* est **nvarchar**(776), sans valeur par défaut.  
  
 [  **@indexname =** ] **'***indexname***'**  
 Nom de l'index spatial spécifié. *IndexName* est **sysname** sans valeur par défaut.  
  
 [  **@verboseoutput =** ] **'***verboseoutput***'**  
 Spécifie la plage des noms de propriétés et les valeurs à retourner.  
  
 0 = propriétés principales  
  
 \>0 = toutes les propriétés  
  
 *verboseoutput* est **tinyint** sans valeur par défaut.  
  
 [  **@query_sample =** ] **'***query_sample***'**  
 Spécifie un exemple de requête représentatif qui peut être utilisé pour tester l'utilité de l'index. Il peut s'agir d'un objet représentatif ou d'une fenêtre de requête. *query_sample* est **geometry** sans valeur par défaut.  
  
 [  **@xml_output =** ] **'***xml_output***'**  
 Spécifie un paramètre de sortie qui retourne le jeu de résultats dans un fragment XML. *xml_output* est **xml** sans valeur par défaut.  
  
## <a name="properties"></a>Propriétés  
 Définissez **@verboseoutput** = 0 pour retourner des propriétés principales comme indiqué dans le tableau ci-dessous ; **@verboseoutput** > 0 pour retourner toutes les propriétés de l’index spatial.  
  
 **Base_Table_Rows**  
 Nombre de lignes dans la table de base. La valeur est **bigint**.  
  
 **Colonnes Bounding_Box_xmin**  
 Délimitation des propriétés de la zone de l’index spatial pour X-minimum **geometry** type. Valeur de cette propriété est NULL pour **geography**type. La valeur est **float**.  
  
 **Bounding_Box_ymin**  
 Délimitation des propriétés de la zone de l’index spatial pour Y-minimum **geometry** type. Valeur de cette propriété est NULL pour **geography** type. La valeur est **float**.  
  
 **Bounding_Box_xmax**  
 Délimitation des propriétés de la zone de l’index spatial pour X-maximum **geometry** type. Valeur de cette propriété est NULL pour **geography** type. La valeur est **float**.  
  
 **Bounding_Box_ymax**  
 Délimitation des propriétés de la zone de l’index spatial pour Y-maximum **geometry** type. Valeur de cette propriété est NULL pour **geography** type. La valeur est **float**.  
  
 **Grid_Size_Level_1**  
 Densité de grille de niveau 1 de l’index spatial :  
  
 16 pour LOW  
  
 64 pour MEDIUM  
  
 256 pour HIGH  
  
 La valeur est **int**.  
  
 **Grid_Size_Level_2**  
 Densité de grille de niveau 2 de l’index spatial :  
  
 16 pour LOW  
  
 64 pour MEDIUM  
  
 256 pour HIGH  
  
 La valeur est **int**.  
  
 **Grid_Size_Level_3**  
 Densité de la grille de niveau 3 de l’index spatial :  
  
 16 pour LOW  
  
 64 pour MEDIUM  
  
 256 pour HIGH  
  
 La valeur est **int**.  
  
 **Grid_Size_Level_4**  
 Densité de la grille de niveau 4 de l'index spatial :  
  
 16 pour LOW  
  
 64 pour MEDIUM  
  
 256 pour HIGH  
  
 La valeur est **int**.  
  
 **Cells_Per_Object**  
 Nombre de cellules par objet spatial (propriété d'index). La valeur est **int**.  
  
 **Total_Primary_Index_Rows**  
 Nombre de lignes dans l'index. La valeur est **bigint**.  
  
 **Total_Primary_Index_Pages**  
 Nombre de pages dans l'index. La valeur est **bigint**.  
  
 **Average_Number_Of_Index_Rows_Per_Base_Row**  
 Nombre de lignes d'index / nombre de lignes de table de base. La valeur est **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level0_For_QuerySample**  
 Indique si l’exemple de requête représentatif se situe en dehors de la zone englobante de la **geometry** index et dans la cellule racine (cellule niveau 0). Il s'agit de 0 (pas dans la cellule de niveau 0) ou de 1. Si c'est dans la cellule de niveau 0, l'index exploré n'est pas un index approprié pour l'exemple de requête. Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level0_In_Index**  
 Nombre d’instances de cellule d’objets indexés qui sont tesselées au niveau 0 (cellule racine, en dehors de la zone englobante pour **geometry**). Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 Pour **geometry** index, cela se produit si la zone englobante de l’index est plus petite que le domaine de données. Un grand nombre d’objets de niveau 0 peut nécessiter des filtres secondaires si la fenêtre de requête se situe partiellement à l’extérieur du cadre englobant et diminueront la performance d’index (par exemple, **Total_Number_Of_ObjectCells_In_Level0_For_QuerySample** est 1). Si la fenêtre de requête se situe à l'intérieur du cadre englobant, un nombre élevé d'objets au niveau 0 peut améliorer réellement la performance de l'index.  
  
 Les instances NULL et vides sont comptées au niveau 0 mais n'ont pas d'incidence sur les performances. Le niveau 0 aura autant de cellules que les instances NULL et vides à la table de base. Pour **geography** index, le niveau 0 aura autant de cellules en tant que valeur NULL et une cellule vide instances + 1, car l’exemple de requête est compté comme 1.  
  
 **Total_Number_Of_ObjectCells_In_Level1_In_Index**  
 Nombre d’instances de cellule d’objets indexés qui sont pavées avec une précision de niveau 1. Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level2_In_Index**  
 Nombre d’instances de cellule d’objets indexés qui sont pavées avec une précision de niveau 2. Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level3_In_Index**  
 Nombre d’instances de cellule d’objets indexés qui sont pavées avec une précision de niveau 3. Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_ObjectCells_In_Level4_In_Index**  
 Nombre d'instances de cellule d'objets indexés qui sont pavées avec la précision de niveau 4. Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level1_In_Index**  
 Nombre de cellules qui sont complètement couvertes par un objet au pavage niveau 1 et est donc intérieur à l’objet. (Cell_attributevalue est 2.) Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level2_In_Index**  
 Nombre de cellules qui sont complètement couvertes par un objet au pavage niveau 2 et est donc intérieur à l’objet. (La valeur Cell_attribute est 2.) Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level3_In_Index**  
 Nombre de cellules qui sont complètement couvertes par un objet au pavage niveau 3 et est donc intérieur à l’objet. (La valeur Cell_attribute est 2.) Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_interior_ObjectCells_In_Level4_In_Index**  
 Nombre de cellules qui sont complètement couvertes par un objet au pavage niveau 4 et se situent donc à l'intérieur de l'objet. (La valeur Cell_attribute est 2.) Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level1_In_Index**  
 Nombre de cellules qui sont croisées par un objet au pavage niveau 1. (La valeur Cell_attribute est 1). Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level2_In_Index**  
 Nombre de cellules qui sont croisées par un objet au pavage niveau 2. (La valeur Cell_attribute est 1). Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level3_In_Index**  
 Nombre de cellules qui sont croisées par un objet au pavage niveau 3. (La valeur Cell_attribute est 1). Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level4_In_Index**  
 Nombre de cellules qui sont croisées par un objet au pavage niveau 4. (La valeur Cell_attribute est 1). Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level0_For_QuerySample**  
 Indique si l'exemple de requête est dans la cellule racine 0 à l'extérieur du cadre englobant, mais le touche. Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
> [!NOTE]  
>  Cette information est uniquement utile pour déterminer s'il y a des objets que le cadre englobant a pu manquer de peu.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level0_In_Index**  
 Nombre d'objets au niveau 0 qui touchent le cadre englobant. (La valeur Cell_attribute est 0.)  La valeur est **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level1_In_Index**  
 Nombre de cellules d’objet qui touchent une limite de cellule de grille au pavage niveau 1. (La valeur Cell_attribute est 0.) Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level2_In_Index**  
 Nombre de cellules d’objet qui touchent une limite de cellule de grille au pavage niveau 2. (La valeur Cell_attribute est 0.) Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level3_In_Index**  
 Nombre de cellules d’objet qui touchent une limite de cellule de grille au pavage niveau 3. (La valeur Cell_attribute est 0.) Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Total_Number_Of_Border_ObjectCells_In_Level4_In_Index**  
 Nombre de cellules d'objet qui touchent une limite de cellule de grille au pavage niveau 4. (La valeur Cell_attribute est 0.) Il s'agit d'une propriété principale. La valeur est **bigint**.  
  
 **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Pourcentage de la zone totale (cellules feuilles totales) de la grille qui contient des cellules feuilles couvertes par un objet.  
  
 Par exemple, un objet est pavé dans 10 cellules à 4 niveaux de grille différents couvrant une zone qui est équivalente à 100 cellules feuilles au total. Supposons l'existence de 3 cellules intérieures qui sont complètement couvertes par l'objet. La zone couverte par les 3 cellules intérieures est équivalente à 42 cellules feuilles. Le pourcentage de zone couverte est donc de 42 pour cent. Il s'agit d'une bonne mesure de la manière dont les objets dans l'index sont déchiquetés.  
  
 La valeur est **float**.  
  
 **Intersecting_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Identique à **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**, à ceci près que ces cellules partiellement couvertes. La valeur est **float**.  
  
 **Border_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 Identique à **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage** , sauf qu’il s’agit des cellules de bordure. La valeur est **float**.  
  
 **Average_Cells_Per_Object_Normalized_To_Leaf_Grid**  
 Cellules moyennes par objet normalisées selon la grille feuilles. Cela nous donne une indication de la taille spatiale de l'objet, ou le volume des objets. La valeur est **float**.  
  
 **Average_Objects_PerLeaf_GridCell**  
 Fragmentation de l'index Nombre moyen d'objets par cellule feuille. La valeur est **float**.  
  
 **Number_Of_SRIDs_Found**  
 Nombre de SRID uniques dans l'index et la colonne. La valeur est **int**.  
  
 Dans la mesure où une colonne peut contenir plusieurs SRID et que les objets de SRID différent ne se croisent jamais, le nombre de SRID indique la sélectivité de l'index.  
  
 **Width_Of_Cell_In_Level1**  
 Propriété de largeur (Width) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Width_Of_Cell_In_Level2**  
 Propriété de largeur (Width) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Width_Of_Cell_In_Level3**  
 Propriété de largeur (Width) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Width_Of_Cell_In_Level4**  
 Propriété de largeur (Width) de cellule dans la grille d'indexation. L'unité de mesure est fournie par l'index et dépend du SRID des données indexées. La valeur est **float**.  
  
 **Height_Of_Cell_In_Level1**  
 Propriété de hauteur (Height) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Height_Of_Cell_In_Level2**  
 Propriété de hauteur (Height) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Height_Of_Cell_In_Level3**  
 Propriété de hauteur (Height) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Height_Of_Cell_In_Level4**  
 Propriété de hauteur (Height) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Area_Of_Cell_In_Level1**  
 Propriété de zone (Area) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Area_Of_Cell_In_Level2**  
 Propriété de zone (Area) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Area_Of_Cell_In_Level3**  
 Propriété de zone (Area) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **Area_Of_Cell_In_Level4**  
 Propriété de zone (Area) de cellule dans la grille d'indexation. L’unité de mesure est fournie par l’index et varie selon le SRID des données indexées. La valeur est **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level1**  
 Le pourcentage de couverture du cadre englobant par une cellule de niveau 1. La valeur est **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level2**  
 Le pourcentage de couverture du cadre englobant par une cellule de niveau 2. La valeur est **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level3**  
 Le pourcentage de couverture du cadre englobant par une cellule de niveau 3. La valeur est **float**.  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level4**  
 Le pourcentage de couverture du cadre englobant par une cellule de niveau 4. La valeur est **float**.  
  
 **Number_Of_Rows_Selected_By_Primary_Filter**  
 Nombre de lignes sélectionnées par le filtre principal. Il s'agit d'une propriété principale. La valeur est **bigint.**  
  
 **Number_Of_Rows_Selected_By_Internal_Filter**  
 Nombre de lignes sélectionnées par le filtre interne. Le filtre secondaire n'est pas appelé pour ces lignes. Il s'agit d'une propriété principale. La valeur est **bigint.**  
  
 Le nombre retourné concerne uniquement **STintersects**.  
  
 **Number_Of_Times_Secondary_Filter_Is_Called**  
 Nombre de fois que le filtre secondaire est appelé. Il s'agit d'une propriété principale. La valeur est **bigint.**  
  
 **Percentage_Of_Rows_NotSelected_By_Primary_Filter**  
 S'il y a N lignes dans la table de base, et si P est sélectionné par le filtre principal, cela retourne (N-P)/N comme pourcentage. Il s'agit d'une propriété principale. La valeur est **float.**  
  
 **Percentage_Of_Primary_Filter_Rows_Selected_By_internal_Filter**  
 Si les lignes P sont sélectionnées par les filtre principal et si les lignes S sont sélectionnées par le filtre interne, cela retourne S/P comme pourcentage. Plus le pourcentage est élevé, mieux l'index se comporte pour éviter le filtre secondaire qui est plus pénalisant en termes de performances. Il s'agit d'une propriété principale. La valeur est **float.**  
  
 **Number_Of_Rows_Output**  
 Nombre réel de lignes générées par la requête. Il s'agit d'une propriété principale. La valeur est **bigint.**  
  
 **Internal_Filter_Efficiency**  
 Si O est le nombre de lignes générées, cela retourne S/O comme pourcentage. Il s'agit d'une propriété principale. La valeur est **float.**  
  
 **Primary_Filter_Efficiency**  
 Si les lignes P sont sélectionnées par le filtre primaire et O est le nombre de lignes de sortie, cette returnsO/P comme pourcentage. Plus le rendement du filtre principal est principal, moins il y a de faux positifs que le filtre secondaire doit traiter. Il s'agit d'une propriété principale. La valeur est **float.**  
  
## <a name="permissions"></a>Autorisations  
 Utilisateur doit être un membre de la **public** rôle. Nécessite une autorisation READ ACCESS sur le serveur et l'objet. Cela s'applique à toutes les procédures stockées de l'index spatial.  
  
## <a name="remarks"></a>Notes  
 Les propriétés qui contiennent des valeurs NULL ne sont pas incluses dans le jeu de retour.  
  
## <a name="examples"></a>Exemples  
 Pour les exemples, consultez les rubriques suivantes :  
  
-   [sp_help_spatial_geometry_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)  
  
-   [sp_help_spatial_geometry_index_xml &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)  
  
-   [sp_help_spatial_geography_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)  
  
-   [sp_help_spatial_geography_index_xml &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-xml-transact-sql.md)  
  
## <a name="requirements"></a>Spécifications  
  
## <a name="see-also"></a>Voir aussi  
 [Les procédures stockées d’Index spatial &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/1be0f34e-3d5a-4a1f-9299-bd482362ec7a)   
 [sp_help_spatial_geometry_index &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)   
 [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Principes fondamentaux de XQuery](../../xquery/xquery-basics.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md)  
  
  
