---
title: Instruction CREATE GLOBAL CUBE (MDX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: ''
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- GLOBAL CUBE
- CUBE
- GLOBAL
- CREATE
- CREATE GLOBAL
- CREATE GLOBAL CUBE
dev_langs:
- kbMDX
helpviewer_keywords:
- statements [MDX], CREATE GLOBAL CUBE
- CREATE GLOBAL CUBE
ms.assetid: b46f3c98-a4f1-4ebb-915f-a3333f4054dc
caps.latest.revision: 17
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: dc74fe731c98609a0840c12f0cd216b89d53e0a9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mdx-data-definition---create-global-cube"></a>Définition de données MDX - CREATE GLOBAL CUBE
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Crée et remplit un cube persistant localement, en fonction d'un sous-cube issu d'un cube sur le serveur. Aucune connexion au serveur n'est nécessaire pour se connecter au cube conservé localement. Pour plus d’informations sur les cubes locaux, consultez [Cubes locaux &#40;Analysis Services - données multidimensionnelles&#41;](../analysis-services/multidimensional-models/olap-physical/local-cubes-analysis-services-multidimensional-data.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE GLOBAL CUBE local_cube_name STORAGE 'Cube_Location'   
FROM source_cube_name (<param list>)  
  
<param list>::= <param> ,<param list> | <param>  
  
<param>::= <dims list> | <measures list>  
  
<measures list>::= <measure>[, <measures list>]   
  
<dims list>::= <dim def> [, <dims list>]  
  
<measure>::= MEASURE source_cube_name.measure_name [<visibility qualifier>] [AS measure_name]   
  
<dim def>::= <source dim def> | <derived dim def>  
  
<source dim def>::= DIMENSION source_cube_name.dimension_name [<dim flags>] [<visibility qualifier>] [AS dimension_name>] [FROM <dim from clause> ] [<dim content def>]  
  
<dim flags>::= NOT_RELATED_TO_FACTS   
  
<dim from clause>::= < dim DM from clause> | <reg dim from clause>   
  
<dim DM from clause>::= dm_model_name> COLUMN column_name   
  
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
 local_cube_name  
 Nom du cube local.  
  
 'Cube_Location'  
 Nom et chemin d'accès du cube conservé localement.  
  
 source_cube_name  
 Nom du cube sur lequel le cube local est basé.  
  
 source_cube_name.measure_name  
 Nom complet de la mesure source incluse dans le cube local. Les membres calculés de la dimension de mesures ne sont pas autorisés.  
  
 measure_name  
 Nom de la mesure au sein du cube local.  
  
 source_cube_name.dimension_name  
 Nom complet de la dimension source incluse dans le cube local.  
  
 dimension_name  
 Nom de la dimension dans le cube local.  
  
 À partir de \<dim clause from >  
 Élément spécifié uniquement pour la définition de dimension dérivée.  
  
 NOT_RELATED_TO_FACTS  
 Élément spécifié uniquement pour la définition de dimension dérivée.  
  
 \<type de niveau >  
 Élément spécifié uniquement pour la définition de dimension dérivée.  
  
## <a name="remarks"></a>Notes  
 Un cube local est termes definedin des mesures et des définitions qui le caractérisent. Il existe deux types de dimensions.  
  
-   Dimensions sources : ce sont des dimensions qui faisaient partie de l’un ou plusieurs cubes sources  
  
-   Dimensions dérivées : dimensions offrant de nouvelles fonctionnalités d'analyse. Une dimension dérivée peut être une dimension régulière définie d'après une dimension source découpée verticalement ou horizontalement ou contenant un regroupement personnalisé de membres de dimension. Il peut s'agir également d'une dimension d'exploration de données fondée sur un modèle d'exploration de données.  
  
> [!NOTE]  
>  Le mot clé Dimension peut se rapporter soit à des dimensions, soit à des hiérarchies.  
  
 Vous pouvez effectuer les tâches suivantes au sein d'un cube local :  
  
-   Éliminer des dimensions qui existent dans le cube source  
  
-   Ajouter ou éliminer des hiérarchies dans une dimension.  
  
-   Supprimer les groupes de mesures ou des mesures spécifiques  
  
 L'instruction CREATE GLOBAL CUBE respecte les règles suivantes :  
  
-   L'instruction CREATE GLOBAL CUBE copie automatiquement toutes les commandes, telles que les actions ou mesures calculées, dans le cube local. Si une commande contient une expression MDX (Multidimensional Expression) qui fait référence au cube parent de manière explicite, le cube local ne peut pas exécuter cette commande. Pour éviter ce problème, utilisez le **CURRENTCUBE** mot clé lors de la définition des expressions MDX pour les commandes. Le **CURRENTCUBE** (mot clé) utilise le contexte de cube actuel lors du référencement d’un cube dans une expression MDX.  
  
-   Un cube global créé à partir d'un cube global existant dans un fichier de cube local ne peut pas être enregistré dans le même fichier de cube local. Par exemple, vous créez un cube global nommé SalesLocal1 et vous l'enregistrez dans le fichier C:\SalesLocal.cub. Vous vous connectez ensuite au fichier C:\SalesLocal.cub et vous créez un deuxième cube global nommé SalesLocal2. Si vous tentez maintenant d'enregistrer le cube global SalesLocal2 dans le fichier C:\SalesLocal.cub, vous recevez une erreur. Toutefois, vous pouvez enregistrer le cube global SalesLocal2 dans un fichier de cube local différent.  
  
-   Les cubes globaux ne prennent pas en charge les mesures de comptage de valeurs. Les cubes qui incluent des mesures de comptage de valeurs étant non additifs, l'instruction CREATE GLOBAL CUBE ne peut pas prendre en charge la création ni l'utilisation de mesures de comptage de valeurs.  
  
-   Lorsque vous ajoutez une mesure à un cube local, vous devez également inclure au moins une dimension associée à la mesure ajoutée.  
  
-   Si vous ajoutez une hiérarchie parent-enfant à un cube local, les niveaux et les filtres de cette hiérarchie sont ignorés et la hiérarchie parent-enfant tout entière est intégrée.  
  
-   Les propriétés de membre ne sont pas prises en charge dans les cubes locaux.  
  
-   Vous ne pouvez pas créer un cube local à partir d'une perspective.  
  
-   Lorsque vous ajoutez une mesure semi-additive à un cube local, les règles suivantes s'appliquent :  
  
    -   Vous devez inclure la dimension de compte dans la propriété AggregateFunction de la mesure ajoutée par ByAccount.  
  
    -   Vous devez inclure la dimension de temps tout entière si la mesure de propriété AggregateFunction ajoutée est FirstChild, LastChild, FirstNonEmpty, LastNonEmpty ou AverageOfChildren.  
  
-   Les dimensions d'exploration de données ne peuvent pas être ajoutées à un cube local.  
  
-   Les dimensions de référence sont matérialisées et ajoutées comme des dimensions régulières.  
  
-   Lorsque vous insérez une dimension plusieurs à plusieurs, les règles suivantes s'appliquent :  
  
    -   Vous devez ajouter la dimension plusieurs à plusieurs tout entière.  
  
    -   Vous devez ajouter le groupe de mesures intermédiaire.  
  
    -   Vous devez ajouter l'intégralité de toutes les dimensions communes aux deux groupes de mesures impliqués dans la relation plusieurs à plusieurs.  
  
 L'exemple suivant illustre la création d'une version locale et persistante du cube Adventure Works qui contient uniquement la mesure Reseller Sales Amount, la dimension Reseller et la dimension Date.  
  
```  
CREATE GLOBAL CUBE [LocalReseller]  
   Storage 'C:\LocalAWReseller1.cub'  
   FROM [Adventure Works]  
   (  
      MEASURE  [Adventure Works].[Reseller Sales Amount],  
      DIMENSION [Adventure Works].[Reseller],  
      DIMENSION [Adventure Works].[Date]  
   )  
```  
  
 L'exemple suivant illustre le découpage lorsque vous créez un cube local. Le cube global créé est basé sur le cube Adventure Works découpé verticalement par le membre 2005 du niveau Fiscal Year et horizontalement par les niveaux Fiscal Year et Month.  
  
```  
CREATE GLOBAL CUBE [LocalReseller]  
   Storage 'C:\LocalAWReseller2.cub'  
   FROM [Adventure Works]  
   (  
      MEASURE  [Adventure Works].[Reseller Sales Amount],  
      DIMENSION [Adventure Works].[Reseller],  
      DIMENSION [Adventure Works].[Date]  
      (  
LEVEL [Fiscal Year],  
LEVEL [Month],  
MEMBER [Date].[Fiscal].[Fiscal Year].&[2005]  
      )  
   )  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions MDX de définition de données &#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)   
 [Instruction CREATE SESSION CUBE &#40;MDX&#41;](../mdx/mdx-data-definition-create-session-cube.md)  
  
  
