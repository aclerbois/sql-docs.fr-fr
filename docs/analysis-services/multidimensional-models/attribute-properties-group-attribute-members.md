---
title: Groupe de membres d’attribut (discrétisation) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: edf1f554a4e7829883263198b132f479b0001d5d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="attribute-properties---group-attribute-members"></a>Propriétés - groupe attribut des membres d’attribut
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un groupe de membres est une collection de membres de dimension contigus générée par le système. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], les membres d’un attribut peuvent être regroupés en plusieurs groupes de membres via un processus nommé discrétisation. Un niveau dans une hiérarchie contient soit des groupes de membres, soit des membres, mais pas les deux. Lorsque les utilisateurs professionnels parcourent un niveau qui contient des groupes de membres, ils voient les noms et les valeurs de cellule de ces groupes. Les membres générés par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour prendre en charge les groupes de membres s'appellent membres de regroupement et se présentent comme des membres ordinaires.  
  
 La propriété **DiscretizationMethod** au niveau d’un attribut détermine la façon dont les membres sont regroupés.  
  
|Définition de**DiscretizationMethod** | Description|  
|--------------------------------------|-----------------|  
|**None**|Affiche les membres.|  
|**Automatic**|Sélectionne la méthode qui représente le mieux les données : soit la méthode **EqualAreas** , soit la méthode **Clusters** .|  
|**EqualAreas**|Tente de diviser les membres de l'attribut en groupes contenant un nombre égal de membres.|  
|**Clusters**|Tente de diviser les membres de l'attribut en groupes en échantillonnant les données d'apprentissage, en initialisant à un certain nombre de points aléatoires, puis en exécutant plusieurs itérations de l'algorithme de clustering EM (Expectation-Maximization).<br /><br /> Cette méthode est utile, car elle peut être appliquée à toute courbe de distribution, mais elle demande davantage de temps de traitement.|  
  
 La propriété **DiscretizationNumber** au niveau des attributs spécifie le nombre de groupes à afficher. Si la propriété a la valeur par défaut 0, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] détermine le nombre de groupes en échantillonnant ou en lisant les données, selon la définition de la propriété **DiscretizationMethod** .  
  
 L’ordre de tri des membres dans les groupes de membres est contrôlé à l’aide de la propriété **OrderBy** de l’attribut. Sur la base de cet ordre de tri, les membres d'un groupe de membres sont ordonnés les uns à la suite des autres.  
  
 Les groupes de membres servent généralement à permettre une exploration vers le bas entre un niveau contenant peu de membres et un autre contenant un grand nombre de membres. Pour permettre aux utilisateurs d’explorer vers le bas entre plusieurs niveaux, modifiez la propriété **DiscretizationMethod** de l’attribut du niveau qui contient un grand nombre de membres en remplaçant sa valeur **None** par l’une des méthodes de discrétisation décrites dans le tableau précédent. Par exemple, une dimension Client contient une hiérarchie d'attribut Client Name avec 500 000 membres. Vous pouvez renommer cet attribut Client Groups et attribuer à la propriété **DiscretizationMethod** la valeur **Automatic** pour afficher les groupes de membres au niveau des membres de la hiérarchie d’attribut.  
  
 Pour descendre au niveau des clients individuels dans chaque groupe, vous pouvez créer une autre hiérarchie d'attribut Client Name liée à la même colonne de table. Ensuite, créez une hiérarchie d'utilisateur à partir des deux attributs. Le niveau supérieur est basé sur l'attribut Client Groups et le niveau inférieur est basé sur l'attribut Client Name. La propriété **IsAggregatable** aurait la valeur **True** dans les deux attributs. L'utilisateur peut développer le niveau (Tout) sur la hiérarchie pour afficher les membres du groupe et développer les membres du groupe pour afficher les membres feuilles de la hiérarchie Pour masquer le niveau groupe ou client, vous pouvez affecter à la propriété **AttributeHierarchyVisible** la valeur **True** dans l’attribut correspondant.  
  
## <a name="naming-template"></a>Modèle de nom  
 Le nom des groupes de membres est automatiquement généré lorsqu'ils sont créés. Si vous ne spécifiez pas un modèle de nom, le modèle de nom par défaut est utilisé. Vous pouvez modifier cette méthode de nom en spécifiant un modèle de nom dans l’option **Format** de la propriété **NameColumn** d’un attribut. Il est possible de redéfinir des modèles de nom différents pour chaque langue spécifiée dans la collection **Translations** de la liaison de colonne utilisée pour la propriété **NameColumn** de l’attribut.  
  
 Le paramètre **Format** utilise l’expression de chaîne suivante pour définir le modèle de nom :  
  
 `<Naming template> ::= <First definition> [;<Intermediate definition>;<Last definition>]`  
  
 `<First definition> ::= <Name expression>`  
  
 `<Intermediate defintion> ::= <Name expression>`  
  
 `<Last definition> ::= <Name expression>`  
  
 Le paramètre `<First definition>` s’applique uniquement au premier ou à l’unique groupe de membres généré par la méthode de discrétisation. Si les paramètres facultatifs, `<Intermediate definition>` et `<Last definition>` , ne sont pas fournis, le paramètre `<First definition>` est utilisé pour tous les groupes de mesures générés pour cet attribut.  
  
 Le paramètre `<Last definition>` s'applique uniquement au dernier groupe de membres généré par la méthode de discrétisation.  
  
 Le paramètre `<Intermediate bucket name>` s'applique à chaque groupe de membres autre que le premier ou le dernier groupe de membres généré par la méthode de discrétisation. Si deux ou moins de deux groupes de membres sont générés, ce paramètre est ignoré.  
  
 Le paramètre `<Bucket name>` est une expression de chaîne qui peut inclure un jeu de variables pour représenter des informations de membre ou de groupe de membres dans le nom du groupe de membres :  
  
|Variable|Description|  
|--------------|-----------------|  
|%{First bucket member}|Nom de membre du premier membre à inclure dans le groupe de membres actif.|  
|%{Last bucket member}|Nom de membre du dernier membre à inclure dans le groupe de membres actif.|  
|%{Previous bucket last member}|Nom du membre du dernier membre assigné au groupe de membres précédent.|  
|%{Next bucket first member}|Nom de membre du premier membre à assigner au groupe de membres suivant.|  
|%{Bucket Min}|Valeur minimale des membres à assigner au groupe de membres actif.|  
|%{Bucket Max}|Valeur maximale des membres à assigner au groupe de membres actif.|  
|%{Previous Bucket Max}|Valeur maximale des membres à assigner au groupe de membres précédent.|  
|%{Next Bucket Min}|Valeur minimale des membres à assigner au groupe de membres suivant.|  
  
 Le modèle de nom par défaut est `"%{First bucket member} - %{Last bucket member}"`, pour assurer la compatibilité avec les versions antérieures de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
> [!NOTE]  
>  Pour inclure un point-virgule (;) comme caractère littéral dans le modèle de nom, faites-le précéder du signe de pourcentage (%).  
  
### <a name="example"></a>Exemple  
 L’expression de chaîne suivante peut être utilisée pour classer l’attribut Yearly Income de la dimension Customer de l’exemple de base de données [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , où cet attribut fait appel au regroupement de membres :  
  
 "Less than %{Next Bucket Min};Between %{First bucket member} and %{Last bucket member};Greater than %{Previous Bucket Max}"  
  
## <a name="adding-new-members-to-existing-member-groups"></a>Ajout de membres aux groupes de membres existants  
 Si de nouveaux membres sont ajoutés à la dimension, ils sont assignés aux groupes de membres appropriés en comparant la valeur du membre à la disposition du groupe de membres actif.  
  
 Si un membre est inséré entre le dernier membre du groupe de membres précédent et le premier membre du groupe de membres suivant, il devient le dernier membre du groupe de membres précédent.  
  
## <a name="updating-a-dimension-with-discretized-attributes"></a>Mise à jour d'une dimension avec des attributs discrétisés  
 Lorsque vous traitez une dimension, un attribut discrétisé n'est rediscrétisé que pendant une mise à jour complète (ProcessFull). Pour rediscrétiser un attribut, vous devez effectuer une mise à jour complète de la dimension. Si la table de dimension d'un attribut discrétisé est mise à jour et que vous effectuez une mise à jour incrémentielle (ProcessAdd) de la dimension, l'attribut discrétisé n'est pas rediscrétisé. Les noms et les enfants des nouveaux compartiments restent les mêmes. Pour plus d’informations sur le traitement des dimensions, consultez [Traitement des objets Analysis Services](../../analysis-services/multidimensional-models/processing-analysis-services-objects.md).  
  
## <a name="usage-limitations"></a>Limites d'utilisation  
  
-   Vous ne pouvez pas créer de groupes de membres dans le premier ou le dernier niveau d'une hiérarchie. En revanche, si cette nécessité se présente, vous pouvez ajouter un niveau de façon à ce que le niveau dans lequel vous souhaitez créer des groupes de membres ne soit plus le premier ou le dernier niveau. Vous pouvez masquer le niveau ajouté en affectant à la propriété **Visible** la valeur **False**.  
  
-   Vous ne pouvez pas créer de groupes de membres dans deux niveaux consécutifs d'une hiérarchie.  
  
-   Les groupes de membres ne sont pas pris en charge pour les dimensions qui utilisent le mode de stockage ROLAP.  
  
-   Si la table de dimension d'une dimension qui contient des groupes de membres est mise à jour et que la dimension fait ensuite l'objet d'un traitement complet, un nouveau jeu de groupes de membres est généré. Les noms et les enfants des nouveaux groupes de membres peuvent être différents de ceux des anciens groupes de membres.  
  
## <a name="see-also"></a>Voir aussi  
 [Attributs et hiérarchies d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md)  
  
  
