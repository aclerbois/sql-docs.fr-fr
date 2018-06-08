---
title: Dimensions parent-enfant | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a9f9cda883822d093db624a4580a94093120ba41
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="parent-child-dimension"></a>Dimension parent-enfant
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une hiérarchie parent-enfant est une hiérarchie dans une dimension standard qui contient un attribut parent. Un attribut parent décrit une *relation d’auto-référencement*, ou *jointure réflexive*, dans une table de dimension principale. Les hiérarchies parent-enfant sont construites à partir d'un seul attribut parent. Un seul niveau est affecté à une hiérarchie parent-enfant parce que les niveaux présents dans la hiérarchie sont constitués à partir des relations parent-enfant entre les membres associés à l'attribut parent. La position d’un membre au sein d’une hiérarchie parent-enfant est déterminée par les propriétés **KeyColumns** et **RootMemberIf** de l’attribut parent, tandis que la position d’un membre au sein d’un niveau est déterminée par la propriété **OrderBy** de l’attribut parent. Pour plus d’informations sur les propriétés d’attributs, consultez [Attributs et hiérarchies d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md).  
  
 À cause des relations parent-enfant existant entre les niveaux d'une hiérarchie parent-enfant, certains membres non-feuilles peuvent également avoir des données dérivées des sources de données sous-jacentes en plus des données agrégées des membres enfants.  
  
## <a name="dimension-schema"></a>Schéma de dimension  
 Le schéma de dimension d'une hiérarchie parent-enfant dépend de la présence d'une relation d'auto-référencement dans la table principale de la dimension. Par exemple, le diagramme suivant montre la table de dimension principale **DimOrganization** de l’exemple de base de données [!INCLUDE[ssSampleDBDWobject](../../includes/sssampledbdwobject-md.md)] .  
  
 ![Jointure circulaires dans la table DimOrganization](../../analysis-services/multidimensional-models/media/dimorganization.gif "jointure circulaires dans la table DimOrganization")  
  
 Dans cette table de dimension, la colonne **ParentOrganizationKey** a une relation de clé étrangère avec la colonne clé primaire **OrganizationKey** . En d'autres termes, chaque enregistrement dans cette table peut être associé à un autre enregistrement de la table par une relation parent-enfant. Ce type de jointure réflexive est généralement utilisé pour représenter les données d'une entité d'une organisation, telles que la structure de gestion des employés dans un service.  
  
## <a name="hierarchies-and-levels"></a>Hiérarchies et niveaux  
 Les dimensions qui n'ont pas de relations de type parent-enfant construisent les hiérarchies en regroupant et en organisant les attributs. Les noms de niveaux des hiérarchies de ces dimensions sont issus des noms d'attributs.  
  
 Toutefois, les dimensions de type parent-enfant construisent les hiérarchies de type parent-enfant en analysant les données de la table de dimension principale et en évaluant les relations de type parent-enfant entre les enregistrements dans la table. Pour plus d’informations sur les hiérarchies parent-enfant, consultez [Hiérarchies utilisateur](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies.md).  
  
 Les hiérarchies parent-enfant ne dérivent pas les noms de leurs niveaux des attributs utilisés pour créer la hiérarchie. En effet, ces dimensions créent les noms de niveaux automatiquement en utilisant un modèle de nom, une expression de chaîne que vous pouvez spécifier au niveau de l'attribut parent qui contrôle la manière dont l'attribut génère la hiérarchie d'attribut. Pour plus d’informations sur la façon de définir le modèle de nom d’un attribut parent, consultez [Attributs et hiérarchies d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md).  
  
## <a name="data-members"></a>Données membres  
 En général, les membres feuilles d'une dimension contiennent des données directement dérivées des sources de données sous-jacentes, tandis que les membres non-feuilles contiennent des données dérivées d'agrégations effectuées sur les membres enfants.  
  
 Toutefois, les hiérarchies parent-enfant peuvent avoir certains membres non-feuilles dont les données sont dérivées des sources de données sous-jacentes, en plus des données agrégées des membres enfants. Pour ces membres non-feuilles d'une hiérarchie parent-enfant, il est possible de créer des membres enfants spéciaux générés par le système qui contiennent les données de la table de faits sous-jacente. Appelés *données membres*, ces membres enfants spéciaux contiennent une valeur directement associée à un membre non feuille et indépendante de la valeur résumée calculée à partir des descendants du membre non feuille. Pour plus d’informations sur les membres de données, consultez [Attributs dans des hiérarchies de type parent-enfant](../../analysis-services/multidimensional-models/parent-child-dimension-attributes.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Attributs dans les hiérarchies Parent-enfant](../../analysis-services/multidimensional-models/parent-child-dimension-attributes.md)   
 [Propriétés de Dimension de base de données](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/database-dimension-properties.md)  
  
  
