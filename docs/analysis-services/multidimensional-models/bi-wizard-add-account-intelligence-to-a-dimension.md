---
title: Ajouter l’Intelligence comptable à une Dimension | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5021d10832028f46d1d0b1a8f33dc01a75df5985
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="bi-wizard---add-account-intelligence-to-a-dimension"></a>Assistant BI - ajouter l’Intelligence comptable à une Dimension
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Ajoutez l'intelligence comptable à un cube ou une dimension pour affecter des classes comptables standard, par exemple recettes et dépenses, aux membres d'un attribut de compte. Cette amélioration identifie également les types de comptes (tels que l'actif et le passif) et affecte l'agrégation appropriée à chaque type de compte. [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut utiliser les classifications pour agréger les comptes au fil du temps.  
  
> [!NOTE]  
>  L'intelligence comptable n'est disponible que pour les dimensions qui sont basées sur des sources de données existantes. Pour les dimensions créées sans utiliser de source de données, vous devez exécuter l'Assistant Génération de schéma pour créer une vue de source de données avant d'ajouter l'intelligence comptable.  
  
 Vous appliquez l'intelligence comptable à une dimension qui spécifie des informations de compte (par exemple le nom, le numéro et le type de compte). Pour ajouter l’intelligence comptable, vous utilisez l’Assistant Business Intelligence et vous sélectionnez l’option **Définir l’intelligence comptable** dans la page **Choisir des améliorations** . L'Assistant vous guide ensuite tout au long des étapes de la procédure consistant à sélectionner la dimension à laquelle vous voulez appliquer l'intelligence comptable, identifier des attributs de compte dans la dimension de comptes sélectionnée, puis mapper les types de comptes de la table de dimension avec les types de comptes reconnus par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="selecting-a-dimension"></a>Sélection d'une dimension  
 Dans la première page **Définir l’intelligence comptable** de l’Assistant, vous spécifiez la dimension à laquelle vous voulez appliquer l’intelligence comptable. L'intelligence comptable ajoutée à la dimension sélectionnée provoquera des changements dans cette dimension. Ces modifications seront héritées par tous les cubes contenant la dimension sélectionnée.  
  
## <a name="specifying-account-attributes"></a>Spécification des attributs de compte  
 Dans la page **Configurer les attributs de la dimension** de l’Assistant, vous spécifiez les attributs de compte dans la dimension de comptes sélectionnée. D’abord, dans la colonne **Inclure** , cochez la case à côté de chaque type d’attribut de compte que vous voulez mapper avec un attribut de dimension dans la dimension. Ensuite, dans la colonne **Attribut de dimension** , développez la liste déroulante et sélectionnez l’attribut dans la dimension qui correspond au type d’attribut sélectionné. La sélection de l’attribut dans la liste définit la propriété **Type** d’attribut pour les attributs de compte.  
  
## <a name="mapping-account-types"></a>Mappage des types de comptes  
 La seconde page **Définir l’intelligence comptable** mappe les valeurs de type de compte issues de la table de dimension avec les types de comptes reconnus par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Cette page apparaît uniquement si vous avez inclus l’attribut de dimension **Type de compte** dans la dimension. Pour inclure la dimension **Type de compte** , dans la page **Définir l’intelligence comptable** de l’Assistant, cochez la case à côté de **Type de compte**, puis sélectionnez l’attribut approprié.  
  
 La seconde page **Définir l’intelligence comptable** comporte deux colonnes :  
  
-   La colonne **Types de comptes de la table source** répertorie les types de comptes que l’Assistant obtient à partir de la table de dimension.  
  
-   La colonne **Types de comptes du serveur** identifie le type de compte correspondant que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] reconnaît. Le tableau suivant répertorie les types de comptes que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] reconnaît et l’agrégation par défaut de chacun de ces types. Les sélections sont effectuées automatiquement si la table de dimension emploie les mêmes noms de type de compte que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
    |Type de compte du serveur|Agrégation|Description|  
    |-------------------------|-----------------|-----------------|  
    |**Statistique**|**Aucun**|Ratio calculé d'un objet quelconque, ou nombre d'un objet qui ne s'agrège pas avec le temps. Ce type de compte d'effectue pas de conversion d'une monnaie à l'autre selon des règles de conversion.|  
    |**Dettes**|**LastNonEmpty**|Prix ou valeur de choses dues à un instant spécifique. Ce type de compte n'effectue pas de cumuls sur le temps écoulé et, de ce fait, ne s'agrège pas naturellement avec le temps. Par exemple, le montant Year est la valeur du dernier mois avec des données. Ce type de compte effectue des conversions entre les monnaies avec le taux End of Period.|  
    |**Actif**|**LastNonEmpty**|Prix ou valeur de choses détenues à un instant spécifique. Ce type de compte s’accumule au fil du temps et, de ce fait, ne s’agrège pas naturellement avec le temps. Par exemple, le montant Year est la valeur du dernier mois avec des données. Ce type de compte effectue des conversions entre les monnaies avec le taux End of Period.|  
    |**Balance**|**LastNonEmpty**|Nombre d'un objet à un instant spécifique. Ce type de compte effectue des cumuls sur le temps écoulé, mais ne s'agrège pas naturellement avec le temps. Par exemple, le montant Year est la valeur du dernier mois avec des données.|  
    |**Flux**|**Sum**|Nombre incrémentiel d'un objet. Ce type de compte s’agrège sous forme de **somme** avec le temps, mais n’effectue pas de conversion entre les monnaies d’après les règles de conversion.|  
    |**Dépenses**|**Sum**|Prix ou valeur de choses dépensées. Ce type de compte s’agrège sous forme de **somme** avec le temps et effectue les conversions entre les monnaies avec un taux moyen.|  
    |**Revenu**|**Sum**|Prix ou valeur de choses reçues. Ce type de compte s’agrège sous forme de **somme** avec le temps et effectue les conversions entre les monnaies avec un taux moyen.|  
  
    > [!NOTE]  
    >  S'il y a lieu, vous pouvez mapper plus d'un type de compte dans la dimension avec un type de compte particulier du serveur.  
  
 Pour modifier les agrégations par défaut mappées avec chaque type de compte pour une base de données, vous pouvez utiliser le Concepteur de bases de données.  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet Analysis Services, puis cliquez sur **Modifier la base de données**.  
  
2.  Dans la zone **Mappage de type de compte** , sélectionnez un type de compte dans la zone **Nom**.  
  
3.  Dans la zone de texte **Alias** , tapez un alias pour ce type de compte.  
  
4.  Dans la zone de liste déroulante **Fonction d’agrégation** , modifiez la fonction d’agrégation par défaut pour ce type de compte.  
  
  
