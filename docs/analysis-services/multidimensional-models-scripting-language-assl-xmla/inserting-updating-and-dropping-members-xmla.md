---
title: Insertion, mise à jour et suppression de membres (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: aafd5a15873b5f41a6c783e98581411da6538e37
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="inserting-updating-and-dropping-members-xmla"></a>Insertion, mise à jour et suppression de membres (XMLA)
  Vous pouvez utiliser la [insérer](../../analysis-services/xmla/xml-elements-commands/insert-element-xmla.md), [mettre à jour](../../analysis-services/xmla/xml-elements-commands/update-element-xmla.md), et [supprimer](../../analysis-services/xmla/xml-elements-commands/drop-element-xmla.md) commandes XML for Analysis (XMLA) pour respectivement, insérer, mettre à jour ou supprimer des membres dans une dimension activée en écriture. Pour plus d’informations sur les dimensions activées en écriture, consultez [Write Dimensions](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/write-enabled-dimensions.md).  
  
## <a name="inserting-new-members"></a>Insertion de nouveaux membres  
 Le **insérer** commande insère de nouveaux membres dans les attributs spécifiés dans une dimension activée en écriture.  
  
 Avant de construire le **insérer** de commande, vous devez disposer des informations suivantes pour les nouveaux membres à insérer :  
  
-   la dimension dans laquelle les nouveaux membres doivent être insérés ;  
  
-   l'attribut de dimension dans lequel insérer les nouveaux membres ;  
  
-   les noms des nouveaux membres, y compris les éventuelles traductions applicables au nom ;  
  
-   les clés des nouveaux membres. Si un attribut utilise une clé composite, la clé peut nécessiter plusieurs valeurs.  
  
-   les valeurs des propriétés d'attribut applicables qui ne sont pas implémentées en tant qu'autres attributs au sein de la dimension. Ces propriétés d'attribut peuvent consister en des opérations unaires, des traductions, des cumuls personnalisés, des propriétés de cumul personnalisé et des niveaux ignorés.  
  
 Le **insérer** commande accepte uniquement deux propriétés :  
  
-   Le [objet](../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md) propriété, qui contient une référence d’objet pour la dimension dans laquelle les membres doivent être insérées. La référence d'objet contient l'identificateur de base de données, l'identificateur de cube et l'identificateur de la dimension.  
  
-   Le [attributs](../../analysis-services/xmla/xml-elements-properties/attributes-element-xmla.md) propriété, qui contient un ou plusieurs [attribut](../../analysis-services/xmla/xml-elements-properties/attribute-element-xmla.md) éléments pour identifier les attributs dans lequel les membres doivent être insérés. Chaque **attribut** élément identifie un attribut et fournit le nom, valeur, traductions, opérateur unaire, cumul personnalisé, propriétés de cumul personnalisé et des niveaux pour un seul membre à ajouter à l’attribut identifié ignorés.  
  
    > [!NOTE]  
    >  Toutes les propriétés pour le **attribut** élément doit être inclus. Sinon, une erreur risque de se produire.  
  
## <a name="updating-existing-members"></a>Mise à jour de membres existants  
 Le **mise à jour** commande met à jour les membres existants dans les attributs spécifiés, en fonction des relations avec d’autres membres dans d’autres attributs, dans une dimension activée en écriture. Le **mise à jour** commande peut déplacer les membres vers d’autres niveaux dans les hiérarchies contient dans la dimension et peuvent être utilisés pour restructurer les hiérarchies parent-enfant définies par les attributs parents.  
  
 Avant de construire le **mise à jour** de commande, vous devez disposer des informations suivantes pour les membres à mettre à jour :  
  
-   la dimension dans laquelle les membres existants doivent être mis à jour ;  
  
-   les attributs de la dimension dans lesquels les membres existants doivent être mis à jour ;  
  
-   les clés des membres existants. Si un attribut utilise une clé composite, la clé peut nécessiter plusieurs valeurs.  
  
-   les valeurs des propriétés d'attribut applicables qui ne sont pas implémentées en tant qu'autres attributs au sein de la dimension. Ces propriétés d'attribut peuvent consister en des opérations unaires, des traductions, des cumuls personnalisés, des propriétés de cumul personnalisé et des niveaux ignorés.  
  
 Le **mise à jour** commande accepte uniquement trois propriétés obligatoires :  
  
-   Le **objet** propriété, qui contient une référence d’objet pour la dimension dans laquelle les membres doivent être mis à jour. La référence d'objet contient l'identificateur de base de données, l'identificateur de cube et l'identificateur de la dimension.  
  
-   Le **attributs** propriété, qui contient un ou plusieurs **attribut** éléments pour identifier les attributs dans lequel les membres doivent être mis à jour. Le **attribut** élément identifie un attribut et fournit le nom, valeur, traductions, opérateur unaire, cumul personnalisé, propriétés de cumul personnalisé et des niveaux ignorés pour un seul membre mis à jour pour l’attribut identifié.  
  
    > [!NOTE]  
    >  Toutes les propriétés pour le **attribut** élément doit être inclus. Sinon, une erreur risque de se produire.  
  
-   Le [où](../../analysis-services/xmla/xml-elements-properties/where-element-xmla.md) propriété, qui contient un ou plusieurs **attribut** éléments qui contraignent les attributs dans lequel les membres doivent être mis à jour. Le **où** propriété s’avère indispensable pour limiter une **mise à jour** commande à des instances spécifiques d’un membre. Si le **où** propriété n’est pas spécifiée, toutes les instances d’un membre donné sont mises à jour. Par exemple, supposons que vous voulez modifier le nom de la ville de trois clients (Bellevue à la place de Redmond). Pour modifier le nom de ville, vous devez fournir un **où** attribut de propriété qui identifie les trois membres de client pour lequel les membres de l’attribut Ville doivent être modifiées. Si vous ne fournissez pas cette **où** propriété, tous les clients dont le nom ville est Redmond aurait le nom de ville Bellevue après le **mise à jour** commande s’exécute.  
  
    > [!NOTE]  
    >  À l’exception des nouveaux membres, le **mettre à jour** commande peut uniquement mettre à jour les valeurs de clé des attributs non inclus dans le **où** clause. Par exemple, le nom de ville ne peut pas être mis à jour lors de la mise à jour d'un client ; sinon, le nom de ville est modifié pour tous les clients.  
  
### <a name="updating-members-in-parent-attributes"></a>Mise à jour des membres d'attributs parents  
 Pour prendre en charge les attributs parents, le **mise à jour** commande facultatif [MoveWithDescendants](../../analysis-services/xmla/xml-elements-properties/movewithdescendants-element-xmla.md)propriété. Définition de la **MoveWithDescendants** propriété la valeur true indique que les descendants du membre parent doivent également être déplacés avec le membre parent lorsque l’identificateur de ce membre parent change. Si cette valeur est définie à false, le déplacement d'un membre parent entraîne la promotion des descendants immédiats de ce membre parent au niveau précédent du membre parent.  
  
 Lors de la mise à jour des membres d’un attribut parent, le **mettre à jour** commande ne peut pas mettre à jour les membres des autres attributs.  
  
## <a name="dropping-existing-members"></a>Suppression de membres existants  
 Avant de construire le **supprimer** de commande, vous devez disposer des informations suivantes pour les membres à supprimer :  
  
-   La dimension dans laquelle les membres existants doivent être supprimés.  
  
-   Les attributs de la dimension dans lesquels les membres existants doivent être supprimés.  
  
-   Les clés des membres existants à supprimer. Si un attribut utilise une clé composite, la clé peut nécessiter plusieurs valeurs.  
  
 Le **supprimer** commande accepte uniquement deux propriétés obligatoires :  
  
-   Le **objet** propriété, qui contient une référence d’objet pour la dimension dans laquelle les membres doivent être ignorés. La référence d'objet contient l'identificateur de base de données, l'identificateur de cube et l'identificateur de la dimension.  
  
-   Le **où** propriété, qui contient un ou plusieurs **attribut** éléments pour limiter les attributs dans lequel les membres doivent être supprimées. Le **où** propriété s’avère indispensable pour limiter une **supprimer** commande à des instances spécifiques d’un membre. Si le **où** commande n’est pas spécifiée, toutes les instances d’un membre donné sont supprimés. Par exemple, supposons que vous voulez supprimer tois clients de la ville de Redmond. Pour supprimer ces clients, vous devez fournir un **où** propriété qui identifie les trois membres de l’attribut Customer à supprimer et le membre Redmond de l’attribut City à partir de laquelle les trois clients doivent être supprimés. Si le **où** propriété spécifie uniquement le membre Redmond de l’attribut City, tous les clients associés à Redmond sont supprimés par le **supprimer** commande. Si le **où** propriété spécifie uniquement les trois membres de l’attribut Customer, les trois clients seront supprimées entièrement par le **supprimer** commande.  
  
    > [!NOTE]  
    >  Le **attribut** éléments inclus dans un **supprimer** commande doit contenir uniquement le **AttributeName** et **clés** propriétés. Sinon, une erreur risque de se produire.  
  
### <a name="dropping-members-in-parent-attributes"></a>Suppression de membres d'attributs parents  
 Définition de la [DeleteWithDescendants](../../analysis-services/xmla/xml-elements-properties/deletewithdescendants-element-xmla.md) propriété indique que les descendants d’un membre parent doivent également être supprimés avec le membre parent. Si cette valeur est définie à false, les descendants immédiats du membre parent sont alors promus au niveau antérieur du membre parent.  
  
> [!IMPORTANT]  
>  Un utilisateur n'a besoin que d'autorisations de suppression du membre parent pour pouvoir supprimer à la fois le membre parent et ses descendants. Il n'a pas besoin d'autorisations de suppression des descendants.  
  
## <a name="see-also"></a>Voir aussi  
 [Supprimer l’élément & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/drop-element-xmla.md)   
 [Insérer un élément & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/insert-element-xmla.md)   
 [Mettre à jour, élément & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/update-element-xmla.md)   
 [Définition et identification d’objets &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/defining-and-identifying-objects-xmla.md)   
 [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
