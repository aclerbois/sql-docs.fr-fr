---
title: Instruction CREATE SESSION CUBE (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 12a450b8184f7a1d6ef8b6068d73f99e17063c5d
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579421"
---
# <a name="mdx-data-definition---create-session-cube"></a>Définition de données MDX - créer un CUBE de SESSION
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Crée et remplit un cube de session à partir d'un cube serveur existant. Le cube de session est visible uniquement dans la session active ; vous ne pouvez pas le parcourir ou l'interroger à partir d'une autre session. Le cube de session est implicitement supprimé à la fermeture de la session.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE SESSION CUBE session_cube_name FROM <cube list> (<param list>)  
  
<cube list>::= source_cube_name [,<cube list>]  
  
<param list>::= <param> ,<param list> | <param>  
  
<param>::= <dims list> | <measures list>  
  
<measures list>::= <measure>[, <measures list>]   
  
<dims list>::= <dim def> [, <dims list>]  
  
<measure>::= MEASURE source_cube_name.measure_name [<visibility qualifier>] [AS measure_name]   
  
<dim def>::= <source dim def> | <derived dim def>  
  
<source dim def>::= DIMENSION source_cube_name.dimension_name [<dim flags>] [<visibility qualifier>] [AS dimension_name>] [FROM <dim from clause> ] [<dim content def>]  
  
<dim flags>::= NOT_RELATED_TO_FACTS   
  
<dim from clause>::= <reg dim from clause>   
  
<dim reg from clause>::= dimension_name  
  
<dim content def>::= ( <level list> [,<grouping list>] [,<member slice list>] [,<default member>] )  
  
<level list>::= <level def> [, <level list>]  
  
<level def>::= LEVEL level_name [<level type> ] [AS level_name] [<level content def>]  
  
<level content def>::= ( <property list> ) | NO_PROPERTIES  
  
<level type>::= GROUPING  
  
<property list>::= <property def> [, <property list>]  
  
<property def>::= PROPERTY property_name   
  
<grouping list>::= <grouping entity> [,<grouping list>]  
  
<grouping entity>::= GROUP group_level_name.group_name (<mixed list>)  
  
<grp mixed list>::= <grp mixed element> [,<grp mixed list>]  
  
<grp mixed element>::= <grouping entity> | <member def>  
  
<member slice list>::= <member list>  
  
<member list>::= <member def> [, <member list>]  
  
<member def>::= MEMBER member_name  
  
<default member>::= DEFAULT_MEMBER AS MDX_expression  
  
<visibility qualifier>::= HIDDEN  
  
```  
  
## <a name="syntax-elements"></a>Éléments de syntaxe  
 session_cube_name  
 Nom du cube de session.  
  
 source_cube_name  
 Nom du cube sur lequel le cube de session est basé.  
  
 source_cube_name.measure_name  
 Nom complet de la mesure source incluse dans le cube de session. Les membres calculés de la dimension de mesures ne sont pas autorisés.  
  
 measure_name  
 Nom de la mesure dans le cube de session.  
  
 source_cube_name.dimension_name  
 Nom complet de la dimension source incluse dans le cube de session.  
  
 dimension_name  
 Nom de la dimension dans le cube de session.  
  
 À partir de \<dim clause from >  
 Élément spécifié uniquement pour la définition de dimension dérivée.  
  
 NOT_RELATED_TO_FACTS  
 Élément spécifié uniquement pour la définition de dimension dérivée.  
  
 \<type de niveau >  
 Élément spécifié uniquement pour la définition de dimension dérivée.  
  
## <a name="remarks"></a>Notes  
 Contrairement aux cubes serveur et locaux, un cube de session n'est pas conservé au-delà de la session qui l'a créé. Un cube de session est défini d'après les mesures et les définitions qui le caractérisent. Il existe deux types de dimensions.  
  
-   Dimensions sources : dimensions qui appartenaient à un ou plusieurs cubes sources.  
  
-   Dimensions dérivées : dimensions offrant de nouvelles fonctionnalités d'analyse. Une dimension dérivée peut être une dimension régulière définie d'après une dimension source découpée verticalement ou horizontalement ou contenant un regroupement personnalisé de membres de dimension. Il peut s'agir également d'une dimension d'exploration de données fondée sur un modèle d'exploration de données.  
  
> [!NOTE]  
>  Le mot clé Dimension peut se rapporter soit à des dimensions, soit à des hiérarchies.  
  
 Les cubes de session servent essentiellement à des applications clientes (par exemple, Microsoft Excel) pour le regroupement dynamique de membres d'attribut dans des groupes de membres personnalisés. Vous pouvez effectuer les tâches suivantes au sein d'un cube de session :  
  
-   Éliminer des dimensions existant dans le cube source.  
  
-   Ajouter ou éliminer des hiérarchies dans une dimension.  
  
-   Éliminer des groupes de mesures ou des mesures spécifiques.  
  
-   Ajouter un nouvel attribut sur la base d'une liaison d'attributs pour créer des groupes par rapport à un attribut existant.  
  
> [!IMPORTANT]  
>  La sécurité des objets de cube de session provient par héritage des objets sources sous-jacents. Le cube de session hérite également d'autres objets, tels que des actions et des scripts de calcul.  
  
 L'instruction CREATE SESSION CUBE respecte les règles suivantes :  
  
-   Vous ne pouvez pas procéder à un regroupement sur des hiérarchies parent-enfant.  
  
-   Vous ne pouvez pas procéder à un regroupement sur des dimensions ROLAP.  
  
-   Vous ne pouvez pas procéder à un regroupement sur des dimensions liées.  
  
-   Vous ne pouvez pas procéder à un regroupement sur des niveaux avec cumuls personnalisés.  
  
-   Vous ne pouvez pas procéder à un regroupement sur des hiérarchies d'attributs discrétisées.  
  
-   Vous ne pouvez pas procéder à un regroupement sur des hiérarchies non naturelles, c'est-à-dire des hiérarchies dotées de relations plusieurs à plusieurs entre les niveaux (par exemple, âge et sexe).  
  
-   Les références explicites à un nom de cube dans un script MDX sont scindées par regroupement puisque le cube de session porte un nom différent. Utilisez le mot clé CURRENTCUBE à la place.  
  
-   Vous ne pouvez pas procéder à un regroupement sur des dimensions dotées de membres par défaut explicites.  
  
-   Lorsque vous procédez à un regroupement, les membres calculés au niveau de la session sur le cube serveur d'origine sont supprimés.  
  
-   Lorsque vous procédez à un regroupement sur une dimension de cube sur un cube serveur, le regroupement affecte toutes les dimensions de cube sur la base de la même dimension.  
  
## <a name="example"></a>Exemple  
 L'exemple suivant illustre la création d'une version au niveau de la session du cube Adventure Works qui contient la mesure Reseller Sales Amount et les dimensions Reseller, Product, Geography et Date. Dans ce cube de session, deux groupes sont créés, un groupe comporte les pays européens et un groupe contient les groupes d'Amérique du Nord. Cet exemple est une version simplifiée de l'instruction CREATE SESSION CUBE émise par Microsoft Excel lorsqu'un utilisateur crée un regroupement personnalisé de membres.  
  
```  
CREATE SESSION CUBE [Adventure Works_XL_GROUPING1]   
   FROM [Adventure Works]   
   ( MEASURE [Adventure Works].[Internet Sales Amount]  
   ,MEASURE [Adventure Works].[Reseller Sales Amount]  
   ,DIMENSION [Adventure Works].[Date].[Calendar]  
   ,DIMENSION [Adventure Works].[Date].[Calendar Year]  
   ,DIMENSION [Adventure Works].[Date].[Calendar Semester]  
   ,DIMENSION [Adventure Works].[Date].[Calendar Quarter]  
   ,DIMENSION [Adventure Works].[Date].[Month Name]  
   ,DIMENSION [Adventure Works].[Date].[Date]  
   ,DIMENSION [Adventure Works].[Geography].[Country]   
      HIDDEN AS _XL_GROUPING81  
   ,DIMENSION [Adventure Works].[Geography].[State-Province]  
   ,DIMENSION [Adventure Works].[Geography].[City]  
   ,DIMENSION [Adventure Works].[Geography].[Postal Code]  
   ,DIMENSION [Adventure Works].[Geography].[Geography]  
   ,DIMENSION [Adventure Works].[Product].[Product Categories]  
   ,DIMENSION [Adventure Works].[Product].[Category]  
   ,DIMENSION [Adventure Works].[Product].[Subcategory]  
   ,DIMENSION [Adventure Works].[Product].[Product]  
   ,DIMENSION [Adventure Works].[Product].[Product Key]  
   ,DIMENSION [Adventure Works].[Reseller].[Reseller]  
   ,DIMENSION [Adventure Works].[Reseller].[Geography Key]  
   ,DIMENSION [Geography].[Country]   
      NOT_RELATED_TO_FACTS FROM _XL_GROUPING81   
          ( LEVEL [(All)]  
         ,LEVEL [Country1] GROUPING  
         ,LEVEL [Country]  
            ,GROUP [Country1].[CountryXl_Grp_1]   
                ( MEMBER [Geography].[Country].&[Canada]  
                  ,MEMBER [Geography].[Country].&[United States] )  
            ,GROUP [Country1].[CountryXl_Grp_2]   
                ( MEMBER [Geography].[Country].&[France]  
                  ,MEMBER [Geography].[Country].&[Germany]  
                  ,MEMBER [Geography].[Country].&[United Kingdom] )   
            )   
   )  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions MDX de définition de données &#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)   
 [Instruction CREATE GLOBAL CUBE &#40;MDX&#41;](../mdx/mdx-data-definition-create-global-cube.md)  
  
  
