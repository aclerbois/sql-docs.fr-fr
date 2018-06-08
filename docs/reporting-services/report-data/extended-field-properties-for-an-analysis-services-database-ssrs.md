---
title: Propriétés de champ étendues pour une base de données Analysis Services (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1d7d87e2-bf0d-4ebb-a287-80b5a967a3f2
caps.latest.revision: 7
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: bfc5d677da90cd5b88c496b2fb491f1b3b920411
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="extended-field-properties-for-an-analysis-services-database-ssrs"></a>Propriétés de champ étendues pour une base de données Analysis Services (SSRS)
  L’extension pour le traitement des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge les propriétés de champ étendues. Les propriétés de champs étendues sont des propriétés complémentaires aux propriétés **Value** et **IsMissing** qui sont disponibles sur la source de données et prises en charge par l’extension pour le traitement des données. Les propriétés étendues ne figurent pas dans le volet des données de rapport dans le cadre de la collection de champs pour un dataset de rapport. Vous pouvez inclure des valeurs de propriété de champ étendues dans votre rapport en écrivant des expressions qui en spécifient le nom à l’aide de la collection **Fields** intégrée.  
  
 Les propriétés étendues incluent des propriétés prédéfinies et des propriétés personnalisées. Les propriétés prédéfinies sont des propriétés communes à plusieurs sources de données qui sont mappées à des noms de propriétés de champs spécifiques. Elles sont accessibles par nom par l’intermédiaire de la collection **Fields** intégrée. Les propriétés personnalisées sont spécifiques à chaque fournisseur de données et sont accessibles par l’intermédiaire de la collection **Fields** intégrée uniquement par la syntaxe utilisant le nom de la propriété étendue comme chaîne.  
  
 Quand vous utilisez le Concepteur de requêtes MDX [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en mode graphique pour définir votre requête, un ensemble prédéfini de propriétés de cellule et de propriétés de dimension est ajouté automatiquement à la requête MDX. Vous pouvez uniquement utiliser des propriétés étendues qui sont répertoriées spécifiquement dans la requête MDX de votre rapport. Selon votre rapport, vous souhaiterez peut-être modifier le texte de la commande MDX par défaut pour inclure d'autres propriétés personnalisées ou de dimension définies dans le cube. Pour plus d’informations sur les champs étendus disponibles dans les sources de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Création et utilisation de valeurs de propriétés &#40;MDX&#41;](http://msdn.microsoft.com/library/0cafb269-03c8-4183-b6e9-220f071e4ef2).  
  
## <a name="working-with-field-properties-in-a-report"></a>Utilisation des propriétés de champ dans un rapport  
 Les propriétés de champ étendues incluent des propriétés prédéfinies et des propriétés spécifiques au fournisseur de données. Les propriétés de champ n’apparaissent pas avec la liste de champs dans le volet **Données du rapport** , même si elles figurent dans la requête générée pour un dataset ; par conséquent, vous ne pouvez pas faire glisser les propriétés de champ vers l’aire de conception du rapport. À la place, vous devez faire glisser le champ dans le rapport, puis remplacer la propriété **Value** du champ par la propriété que vous voulez utiliser. Par exemple, si les données de cellule d’un cube ont déjà été mises en forme, vous pouvez utiliser la propriété de champ FormattedValue en utilisant l’expression suivante : `=Fields!FieldName.FormattedValue`.  
  
 Pour faire référence à une propriété étendue qui n'est pas prédéfinie, utilisez la syntaxe suivante dans une expression :  
  
-   *Fields!FieldName("PropertyName")*  
  
## <a name="predefined-field-properties"></a>Propriétés de champ prédéfinies  
 Dans la plupart des cas, les propriétés de champ prédéfinies s'appliquent aux mesures, aux niveaux ou aux dimensions. À chaque propriété de champ prédéfinie doit correspondre une valeur stockée dans la source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Si aucune valeur n'existe ou que vous spécifiez une propriété de champ de mesure uniquement sur un niveau (par exemple), la propriété retourne une valeur NULL.  
  
 Pour faire référence à une propriété prédéfinie à partir d'une expression, vous pouvez utiliser l'une des syntaxes suivantes :  
  
-   *Fields!FieldName.PropertyName*  
  
-   *Fields!FieldName("PropertyName")*  
  
 Le tableau suivant dresse la liste des propriétés de champ prédéfinies susceptibles d'être utilisées.  
  
|**Propriété**|**Type**|**Description ou valeur attendue**|  
|------------------|--------------|---------------------------------------|  
|**Value**|**Objet**|Précise la valeur de données du champ.|  
|**IsMissing**|**Booléen**|Indique si le champ figure dans le dataset obtenu.|  
|**UniqueName**|**String**|Retourne le nom complet d'un niveau. Par exemple, la valeur **UniqueName** valeur d’un employé peut être *[Employee].[Employee Department].[Department].&[Sales].&[North American Sales Manager].&[272]*.|  
|**BackgroundColor**|**String**|Retourne la couleur d'arrière-plan définie dans la base de données pour le champ.|  
|**Color**|**String**|Retourne la couleur de premier plan définie dans la base de données pour l'élément.|  
|**FontFamily**|**String**|Retourne le nom de la police définie dans la base de données pour l'élément.|  
|**FontSize**|**String**|Retourne la taille en points de la police définie dans la base de données pour l'élément.|  
|**FontWeight**|**String**|Retourne l'épaisseur de la police définie dans la base de données pour l'élément.|  
|**FontStyle**|**String**|Retourne le style de la police définie dans la base de données pour l'élément.|  
|**TextDecoration**|**String**|Retourne la mise en forme de texte spéciale définie dans la base de données pour l'élément.|  
|**FormattedValue**|**String**|Retourne la valeur mise en forme d'une mesure ou d'un chiffre clé. Par exemple, la propriété **FormattedValue** de **Sales Amount Quota** retourne un format monétaire semblable à $1,124,400.00.|  
|**Clé**|**Objet**|Retourne la clé d'un niveau.|  
|**LevelNumber**|**Integer**|Dans le cas des hiérarchies parent-enfant, cette propriété retourne le nombre de niveaux ou de dimensions.|  
|**ParentUniqueName**|**String**|Dans le cas des hiérarchies parent-enfant, cette propriété retourne le nom complet du niveau parent.|  
  
> [!NOTE]  
>  Ces propriétés de champ étendues ont des valeurs seulement si la source de données (par exemple, le cube [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ) fournit ces valeurs quand votre rapport s’exécute et récupère les données pour ses datasets. Vous pouvez alors faire référence à ces valeurs de propriété de champ à partir de n'importe quelle expression en utilisant la syntaxe décrite dans la section suivante. Cependant, dans la mesure où ces champs sont spécifiques à ce fournisseur de données, les modifications que vous apportez à ces valeurs ne sont pas enregistrées avec la définition du rapport.  
  
### <a name="example-extended-properties"></a>Exemple de propriétés étendues  
 Pour illustrer les propriétés étendues, la requête MDX et l'ensemble de résultats suivants incluent plusieurs propriétés de membre disponibles dans un attribut de dimension défini pour un cube. Les propriétés de membre incluses sont MEMBER_CAPTION, UNIQUENAME, Properties("Day Name"), MEMBER_VALUE, PARENT_UNIQUE_NAME, et MEMBER_KEY.  
  
 Cette requête MDX s’exécute par rapport au cube [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] DW fournie avec les exemples de bases de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] .  
  
```  
WITH MEMBER [Measures].[DateCaption]   
      AS '[Date].[Date].CURRENTMEMBER.MEMBER_CAPTION'   
   MEMBER [Measures].[DateUniqueName]   
      AS '[Date].[Date].CURRENTMEMBER.UNIQUENAME'   
   MEMBER [Measures].[DateDayName]   
      AS '[Date].[Date].Properties("Day Name")'   
   MEMBER [Measures].[DateValueinOriginalDatatype]   
      AS '[Date].[Date].CURRENTMEMBER.MEMBER_VALUE'   
   MEMBER [Measures].[DateParentUniqueName]   
      AS '[Date].[Date].CURRENTMEMBER.PARENT_UNIQUE_NAME'   
   MEMBER [Measures].[DateMemberKeyinOriginalDatatype]   
      AS '[Date].[Date].CURRENTMEMBER.MEMBER_KEY'   
SELECT {  
   [Measures].[DateCaption],   
   [Measures].[DateUniqueName],   
   [Measures].[DateDayName],   
   [Measures].[DateValueinOriginalDatatype],  
   [Measures].[DateParentUniqueName],  
   [Measures].[DateMemberKeyinOriginalDatatype]  
   } ON COLUMNS , [Date].[Date].ALLMEMBERS ON ROWS   
FROM [Adventure Works]  
  
```  
  
 Lorsque vous exécutez cette requête dans un volet de requête MDX, vous obtenez un ensemble de résultats comportant 1158 lignes. Les quatre premières lignes sont affichées dans le tableau suivant.  
  
|DateCaption|DateUniqueName|DateDayName|DateValueinOriginalDatatype|DateParentUniqueName|DateMemberKeyinOriginalDatatype|  
|-----------------|--------------------|-----------------|---------------------------------|--------------------------|-------------------------------------|  
|All Periods|[Date].[Date].[All Periods]|(Null)|(Null)|(Null)|0|  
|1-juil-01|[Date].[Date].&[1]|Dimanche|7/1/2001|[Date].[Date].[All Periods]| 1|  
|2-juil-01|[Date].[Date].&[2]|Lundi|7/2/2001|[Date].[Date].[All Periods]|2|  
|3-juil-01|[Date].[Date].&[3]|Mardi|7/3/2001|[Date].[Date].[All Periods]|3|  
  
 Les requêtes MDX par défaut conçues à l'aide du Concepteur de requêtes MDX en mode graphique n'incluent que MEMBER_CAPTION et UNIQUENAME pour des propriétés de dimension. Par défaut, ces valeurs sont toujours des données de type **String**.  
  
 Si vous avez besoin d'une propriété de membre dans son type de données d'origine, vous pouvez inclure une propriété supplémentaire MEMBER_VALUE en modifiant l'instruction MDX par défaut dans le concepteur de requêtes textuel. Dans l'instruction MDX simple suivante, la propriété MEMBER_VALUE a été ajoutée à la liste des propriétés dimension à extraire.  
  
```  
SELECT NON EMPTY {[Measures].[Order Count]} ON COLUMNS,   
NON EMPTY { ([Date].[Month of Year].[Month of Year] ) }   
DIMENSION PROPERTIES   
   MEMBER_CAPTION, MEMBER_UNIQUE_NAME, MEMBER_VALUE ON ROWS   
FROM [Adventure Works]  
CELL PROPERTIES   
   VALUE, BACK_COLOR, FORE_COLOR,   
   FORMATTED_VALUE, FORMAT_STRING,   
   FONT_NAME, FONT_SIZE, FONT_FLAGS  
```  
  
 Les quatre premières lignes du résultat dans le volet Résultats MDX s'affichent dans le tableau suivant.  
  
|Mois de l'année|Nombre de commandes|  
|-------------------|-----------------|  
|Janvier|2,481|  
|February|2,684|  
|Mars|2,749|  
|avril|2,739|  
  
 Même si les propriétés font partie de l'instruction select MDX, elles n'apparaissent pas dans les colonnes du jeu de résultats. Cependant, les données sont disponibles pour un rapport à l'aide de la fonctionnalité des propriétés étendues. Dans un volet des résultats d’une requête MDX dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], vous pouvez double-cliquer sur la cellule et afficher les valeurs de propriétés de cellules si elles sont définies dans le cube. Si vous double-cliquez sur la première cellule Nombre de commandes qui contient le chiffre 1,379, une fenêtre contextuelle s'affiche avec les propriétés de cellule suivantes :  
  
|Propriété|Valeur|  
|--------------|-----------|  
|CellOrdinal|0|  
|Value|2481|  
|BACK_COLOR|(Null)|  
|FORE_COLOR|(Null)|  
|FORMATTED_VALUE|2,481|  
|FORMAT_STRING|#,#|  
|FONT_NAME|(Null)|  
|FONT_SIZE|(Null)|  
|FONT_FLAGS|(Null)|  
  
 Si vous créez un dataset de rapport avec cette requête et que vous liez celui-ci à une table, vous pouvez consulter la propriété VALUE par défaut pour un champ ; par exemple, `=Fields!Month_of_Year!Value`. Si vous définissez cette expression comme l’expression de tri pour la table, vos résultats trient la table par ordre alphabétique par mois, car le champ Valeur utilise un type de données **String** . Pour trier la table pour afficher les mois dans l'ordre d'apparition au cours de l'année (janvier au début, décembre à la fin), utilisez l'expression suivante :  
  
```  
=Fields!Month_of_Year("MEMBER_VALUE")  
```  
  
 Cette expression trie la valeur du champ dans son type de données entier d'origine à partir de la source de données.  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Collections intégrées dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md)   
 [Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
  
  
