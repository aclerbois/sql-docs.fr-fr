---
title: Propriétés de membre intrinsèques (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 97e1507617d710373dea39b546b74fc77a1d90de
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-member-properties---intrinsic-member-properties"></a>Propriétés de membre MDX - propriétés de membre intrinsèques
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] expose les propriétés intrinsèques sur les membres de dimension que vous pouvez inclure dans une requête afin de retourner des informations supplémentaires ou des métadonnées à utiliser dans une application personnalisée, ou pour faciliter l’analyse ou la construction d’un modèle. Si vous utilisez les outils clients SQL Server, vous pouvez voir les propriétés intrinsèques dans SQL Server Management Studio (SSMS).  
  
 Les propriétés intrinsèques sont **ID**, **KEY**, **KEYx**et **NAME**, lesquelles sont des propriétés exposées par chaque membre, à tout niveau. Vous pouvez également retourner les informations de position, telles que **LEVEL_NUMBER** ou **PARENT_UNIQUE_NAME**, entre autres choses.  
  
 Selon la façon dont vous créez la requête et de l'application cliente que vous utilisez pour exécuter des requêtes, des propriétés de membre peuvent être visibles ou non dans le jeu de résultats. Si vous utilisez SQL Server Management Studio pour tester ou exécuter des requêtes, vous pouvez double-cliquer sur un membre du jeu de résultats pour ouvrir la boîte de dialogue Propriétés de membre, qui affiche les valeurs de chaque propriété de membre intrinsèque.  
  
 Pour obtenir une présentation de l’utilisation et de l’affichage des propriétés de membre de dimension, consultez [Affichage de propriétés de membre SSAS dans une fenêtre de requête MDX dans SSMS](http://go.microsoft.com/fwlink/?LinkId=317362).  
  
> [!NOTE]  
>  En tant que fournisseur compatible avec la section OLAP de la norme OLE DB de mars 1999 (2.6), [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] prend en charge les propriétés de membre intrinsèques répertoriées dans cette rubrique.  
>   
>  Des fournisseurs autres que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] peuvent prendre en charge des propriétés de membre intrinsèques supplémentaires. Pour plus d'informations sur les propriétés de membre intrinsèques prises en charge par d'autres fournisseurs, consultez la documentation qui les accompagne.  
  
## <a name="types-of-member-properties"></a>Types de propriétés de membre  
 Les propriétés de membre intrinsèques prises en charge par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] sont de deux types :  
  
 Propriétés de membre sensibles au contexte  
 Ces propriétés de membre doivent être utilisées dans le contexte d'une hiérarchie ou d'un niveau spécifique ; elles fournissent les valeurs de chaque membre de la dimension ou du niveau en question.  
  
 Notez que l’exemple suivant inclut le chemin d’accès de la propriété **KEY** : `MEMBER [Measures].[Parent Member Key] AS [Product].[Product Categories].CurrentMember.Parent.PROPERTIES("KEY")`.  
  
 Propriétés de membre non sensibles au contexte  
 Ces propriétés de membre ne peuvent pas être utilisées dans le contexte d'une dimension ou d'un niveau spécifique, et retournent des valeurs pour tous les membres situés sur un axe.  
  
 Les propriétés non sensibles au contexte sont autonomes et n'incluent pas d'informations de chemin d'accès. Notez qu’aucune dimension ou aucun niveau n’est spécifié pour **PARENT_UNIQUE_NAME** dans l’exemple suivant : `DIMENSION PROPERTIES PARENT_UNIQUE_NAME ON COLUMNS`  
  
 Que la propriété de membre intrinsèque soit sensible au contexte ou non, les règles d'utilisation suivantes sont d'application :  
  
-   Vous pouvez spécifier uniquement les propriétés de membre intrinsèques associées aux membres de dimension projetés sur l'axe.  
  
-   Vous pouvez combiner dans la même requête des propriétés de membre intrinsèques sensibles au contexte et des propriétés de membre intrinsèques non sensibles au contexte.  
  
-   Vous devez utiliser le mot clé **PROPERTIES** pour rechercher les propriétés.  
  
 Les sections suivantes décrivent à la fois les différentes propriétés de membre intrinsèques sensibles au contexte et non sensibles au contexte disponibles dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], ainsi que l’utilisation du mot clé **PROPERTIES** avec chaque type de propriété.  
  
## <a name="context-sensitive-member-properties"></a>Propriétés de membre sensibles au contexte  
 Tous les membres de dimension ou de niveau prennent en charge une série de propriétés de membre intrinsèques sensibles au contexte. Le tableau suivant en dresse la liste.  
  
|Propriété| Description|  
|--------------|-----------------|  
|**ID**|Identificateur géré en interne pour le membre.|  
|**Key**|Valeur de la clé du membre dans le type de données d'origine. MEMBER_KEY est fournie à des fins de compatibilité descendante.  MEMBER_KEY a la même valeur que KEY0 pour les clés non composites et la propriété MEMBER_KEY est Null pour les clés composites.|  
|**KEYx**|Clé du membre, où x est la valeur ordinale de base zéro de la clé. KEY0 est disponible pour les clés composites et non composites, mais utilisé principalement pour les clés composites.<br /><br /> Pour les clés composites, KEY0, KEY1, KEY2, etc., forment collectivement la clé composite. Vous pouvez utiliser chacune indépendamment dans une requête pour retourner la partie en question de la clé composite. Par exemple, la spécification de KEY0 retourne la première partie de la clé composite, la spécification de KEY1 retourne la partie suivante de la clé composite, et ainsi de suite.<br /><br /> S’il s’agit d’une clé non composite, KEY0 équivaut alors à **Key**.<br /><br /> Notez que **KEYx** peut être utilisé en contexte, ainsi que sans contexte. Pour cette raison, il apparaît dans les deux listes.<br /><br /> Pour obtenir un exemple d’utilisation de cette propriété de membre, consultez [A Simple MDX Tidbit : Key0, Key1, Key2](http://go.microsoft.com/fwlink/?LinkId=317364)(Une astuce MDX toute simple : Key0, Key1, Key2).|  
|**Name**|Nom du membre.|  
  
### <a name="properties-syntax-for-context-sensitive-properties"></a>Syntaxe PROPERTIES pour les propriétés sensibles au contexte  
 Vous utilisez ces propriétés de membre dans le contexte d’une dimension ou d’un niveau spécifique, et fournissez les valeurs de chaque membre de la dimension ou du niveau en question.  
  
 Pour les propriétés de membre de dimension, vous devez faire précéder le nom de la propriété du nom de la dimension à laquelle elle s'applique. L'exemple suivant illustre la syntaxe appropriée :  
  
 `DIMENSION PROPERTIES Dimension.Property_name`  
  
 Pour une propriété de membre de niveau, vous pouvez faire précéder le nom de la propriété du nom du niveau uniquement, ou des noms de la dimension et du niveau pour une spécification plus détaillée. L'exemple suivant illustre la syntaxe appropriée :  
  
 `DIMENSION PROPERTIES [Dimension.]Level.Property_name`  
  
 Pour illustrer cela, vous pouvez retourner les noms de chaque membre référencé dans la dimension `[Sales]` . Pour retourner ces noms, vous devez utiliser l'instruction suivante dans une requête MDX (Multidimensional Expressions) :  
  
 `DIMENSION PROPERTIES [Sales].Name`  
  
## <a name="non-context-sensitive-member-properties"></a>Propriétés de membre non sensibles au contexte  
 Tous les membres prennent également en charge une liste de propriétés de membre intrinsèques qui sont identiques quel que soit le contexte. Ces propriétés fournissent des informations supplémentaires pouvant être utilisées par des applications pour améliorer l'expérience de l'utilisateur.  
  
 Le tableau suivant répertorie les propriétés intrinsèques non sensibles au contexte prises en charge par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
> [!NOTE]  
>  Les colonnes du jeu de lignes du schéma MEMBERS prennent en charge les propriétés de membre intrinsèques répertoriées dans le tableau suivant. Pour plus d’informations sur l’ensemble de lignes du schéma **MEMBERS** , consultez [Ensemble de lignes MDSCHEMA_MEMBERS](../../../analysis-services/schema-rowsets/ole-db-olap/mdschema-members-rowset.md).  
  
|Propriété| Description|  
|--------------|-----------------|  
|**CATALOG_NAME**|Nom du cube auquel ce membre appartient.|  
|**CHILDREN_CARDINALITY**|Nombre d'enfants de ce membre. Ce nombre peut être une estimation, et ne doit pas être considéré comme le compte exact. Les fournisseurs doivent renvoyer la meilleure estimation possible.|  
|**CUSTOM_ROLLUP**|Expression de membre personnalisée.|  
|**CUSTOM_ROLLUP_PROPERTIES**|Propriétés de membre personnalisées.|  
|**DESCRIPTION**|Description du membre à l'intention des utilisateurs.|  
|**DIMENSION_UNIQUE_NAME**|Nom unique de la dimension à laquelle ce membre appartient. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**HIERARCHY_UNIQUE_NAME**|Nom unique de la hiérarchie. Si le membre appartient à plusieurs hiérarchies, il existe une ligne pour chaque hiérarchie dont il fait partie. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**IS_DATAMEMBER**|Valeur booléenne indiquant si le membre est un membre de données.|  
|**IS_PLACEHOLDERMEMBER**|Valeur booléenne indiquant si le membre est un espace réservé.|  
|**KEYx**|Clé du membre, où x est la valeur ordinale de base zéro de la clé. KEY0 est disponible pour les clés composites et non composites.<br /><br /> S’il s’agit d’une clé non composite, KEY0 équivaut alors à **Key**.<br /><br /> Pour les clés composites, KEY0, KEY1, KEY2, etc., forment collectivement la clé composite. Vous pouvez faire référence à chacune indépendamment dans une requête pour retourner la partie en question de la clé composite. Par exemple, la spécification de KEY0 retourne la première partie de la clé composite, la spécification de KEY1 retourne la partie suivante de la clé composite, et ainsi de suite.<br /><br /> Notez que **KEYx** peut être utilisé en contexte, ainsi que sans contexte. Pour cette raison, il apparaît dans les deux listes.<br /><br /> Pour obtenir un exemple d’utilisation de cette propriété de membre, consultez [A Simple MDX Tidbit : Key0, Key1, Key2](http://go.microsoft.com/fwlink/?LinkId=317364)(Une astuce MDX toute simple : Key0, Key1, Key2).|  
|**LCID** *x*|Traduction de la légende du membre dans la valeur hexadécimale de l’ID des paramètres régionaux, où *x* correspond à la valeur décimale de l’ID des paramètres régionaux (par exemple, LCID1009 pour Anglais - Canada). Uniquement disponible si la colonne de légende de la traduction est liée à la source de données.|  
|**LEVEL_NUMBER**|Distance du membre par rapport à la racine de la hiérarchie. Le niveau de la racine est égal à zéro.|  
|**LEVEL_UNIQUE_NAME**|Nom unique du niveau auquel le membre appartient. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**MEMBER_CAPTION**|Étiquette ou légende associée au membre. La légende est essentiellement utilisée à des fins d'affichage. En l’absence de légende, la requête retourne **MEMBER_NAME**.|  
|**MEMBER_KEY**|Valeur de la clé du membre dans le type de données d'origine. MEMBER_KEY est fournie à des fins de compatibilité descendante.  MEMBER_KEY a la même valeur que KEY0 pour les clés non composites et la propriété MEMBER_KEY est Null pour les clés composites.|  
|**MEMBER_NAME**|Nom du membre.|  
|**MEMBER_TYPE**|Type du membre. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **MDMEMBER_TYPE_REGULAR**<br /><br /> **MDMEMBER_TYPE_ALL**<br /><br /> **MDMEMBER_TYPE_FORMULA**<br /><br /> **MDMEMBER_TYPE_MEASURE**<br /><br /> **MDMEMBER_TYPE_UNKNOWN**<br /><br /> <br /><br /> Remarque : MDMEMBER_TYPE_FORMULA est prioritaire sur MDMEMBER_TYPE_MEASURE. Ainsi, si la dimension Measures comprend un membre de formule (calculé), la propriété **MEMBER_TYPE** du membre calculé est MDMEMBER_TYPE_FORMULA.|  
|**MEMBER_UNIQUE_NAME**|Nom unique du membre. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**MEMBER_VALUE**|Valeur du membre dans le type d'origine.|  
|**PARENT_COUNT**|Nombre de parents de ce membre.|  
|**PARENT_LEVEL**|Distance du parent du membre par rapport au niveau racine de la hiérarchie. Le niveau de la racine est égal à zéro.|  
|**PARENT_UNIQUE_NAME**|Nom unique du parent du membre. **NULL** est retournée pour tout membre situé au niveau de la racine. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**SKIPPED_LEVELS**|Nombre de niveaux ignorés pour le membre.|  
|**UNARY_OPERATOR**|Opérateur unaire du membre.|  
|**UNIQUE_NAME**|Le nom complet du membre, au format : [dimension].[niveau].[key6].|  
  
### <a name="properties-syntax-for-non-context-sensitive-properties"></a>Syntaxe PROPERTIES pour les propriétés non sensibles au contexte  
 Utilisez la syntaxe suivante pour spécifier une propriété de membre intrinsèque non sensible au contexte à l’aide du mot clé **PROPERTIES** :  
  
 `DIMENSION PROPERTIES Property`  
  
 Remarquez que cette syntaxe n'autorise pas la qualification de la propriété par une dimension ou un niveau. La propriété ne peut pas être qualifiée, car une propriété de membre intrinsèque non sensible au contexte s'applique à tous les membres d'un axe.  
  
 Par exemple, une instruction MDX spécifiant la propriété de membre intrinsèque **DESCRIPTION** posséderait la syntaxe suivante :  
  
 `DIMENSION PROPERTIES DESCRIPTION`  
  
 Cette instruction retourne la description de chaque membre de la dimension de l'axe. Si vous avez tenté de qualifier la propriété avec une dimension ou un niveau, comme dans *Dimension*`.DESCRIPTION` ou dans *Level*`.DESCRIPTION`, l’instruction n’est pas validée.  
  
### <a name="example"></a>Exemple  
 Les exemples suivants montrent des requêtes MDX qui retournent les propriétés intrinsèques.  
  
 **Exemple 1 : utiliser les propriétés intrinsèques contextuelles dans une requête**  
  
 L'exemple suivant retourne l'ID parent, la clé et le nom de chaque catégorie de produits. Notez que les propriétés sont exposées en tant que mesures. Cela vous permet d'afficher les propriétés d'un ensemble de cellules lorsque vous exécutez la requête, plutôt que la boîte de dialogue Propriétés de membre dans SSMS. Vous pouvez exécuter une requête comme celle qui suit pour récupérer des métadonnées de membre à partir d'un cube déjà déployé.  
  
```  
WITH  
MEMBER [Measures].[Parent Member ID] AS  
 [Product].[Product Categories].CurrentMember.Parent.PROPERTIES("ID")  
MEMBER [Measures].[Parent Member Key] AS  
 [Product].[Product Categories].CurrentMember.Parent.PROPERTIES("KEY")  
MEMBER [Measures].[Parent Member Name] AS  
 [Product].[Product Categories].CurrentMember.Parent.PROPERTIES("Name")  
SELECT  
 {[Measures].[Parent Member ID], [Measures].[Parent Member Key], [Measures].[Parent Member Name] } on COLUMNS,   
 [Product].[Product Categories].AllMembers on ROWS  
FROM [Adventure Works]  
```  
  
 **Exemple 2 : propriétés intrinsèques non sensibles au contexte**  
  
 L'exemple suivant correspond à la liste exhaustive des propriétés intrinsèques non sensibles au contexte. Après avoir exécuté la requête dans SSMS, cliquez sur les membres individuels pour afficher les propriétés dans la boîte de dialogue Propriétés de membre.  
  
```  
SELECT [Measures].[Sales Amount Quota] on COLUMNS,  
[Employee].[Employees].members  
DIMENSION PROPERTIES  
 CATALOG_NAME ,   
 CHILDREN_CARDINALITY ,  
 CUSTOM_ROLLUP ,   
 CUSTOM_ROLLUP_PROPERTIES ,   
 DESCRIPTION ,   
 DIMENSION_UNIQUE_NAME ,   
 HIERARCHY_UNIQUE_NAME ,  
 IS_DATAMEMBER ,   
 IS_PLACEHOLDERMEMBER ,   
 KEY0 ,  
 LCID ,  
 LEVEL_NUMBER ,  
 LEVEL_UNIQUE_NAME ,  
 MEMBER_CAPTION ,   
 MEMBER_KEY ,   
 MEMBER_NAME ,   
 MEMBER_TYPE ,  
 MEMBER_UNIQUE_NAME ,   
 MEMBER_VALUE ,   
 PARENT_COUNT ,   
 PARENT_LEVEL ,   
 PARENT_UNIQUE_NAME ,  
 SKIPPED_LEVELS ,   
 UNARY_OPERATOR ,   
 UNIQUE_NAME    
 ON ROWS  
FROM [Adventure Works]  
WHERE [Employee].[Employee Department].[Department].&[Sales]  
```  
  
 **Exemple 3 : retourner les propriétés de membre sous forme de données dans un jeu de résultats**  
  
 L'exemple suivant retourne la légende traduite pour le membre de la catégorie de produit dans la dimension Product du cube Adventure Works pour les paramètres régionaux spécifiés.  
  
```  
WITH   
MEMBER Measures.CategoryCaption AS Product.Category.CurrentMember.MEMBER_CAPTION  
MEMBER Measures.SpanishCategoryCaption AS Product.Category.CurrentMember.Properties("LCID3082")  
MEMBER Measures.FrenchCategoryCaption AS Product.Category.CurrentMember.Properties("LCID1036")  
SELECT   
{ Measures.CategoryCaption, Measures.SpanishCategoryCaption, Measures.FrenchCategoryCaption } ON 0  
,[Product].[Category].MEMBERS ON 1  
FROM [Adventure Works]  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [PeriodsToDate &#40;MDX&#41;](../../../mdx/periodstodate-mdx.md)   
 [Enfants &#40;MDX&#41;](../../../mdx/children-mdx.md)   
 [Hierarchize & #40 ; MDX & #41 ;](../../../mdx/hierarchize-mdx.md)   
 [Nombre & #40 ; Définir le & #41 ; & #40 ; MDX & #41 ;](../../../mdx/count-set-mdx.md)   
 [Filtre & #40 ; MDX & #41 ;](../../../mdx/filter-mdx.md)   
 [AddCalculatedMembers & #40 ; MDX & #41 ;](../../../mdx/addcalculatedmembers-mdx.md)   
 [DrilldownLevel & #40 ; MDX & #41 ;](../../../mdx/drilldownlevel-mdx.md)   
 [Propriétés & #40 ; MDX & #41 ;](../../../mdx/properties-mdx.md)   
 [PrevMember &#40;MDX&#41;](../../../mdx/prevmember-mdx.md)   
 [À l’aide des propriétés de membre & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-member-properties.md)   
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../../../mdx/mdx-function-reference-mdx.md)  
  
  
