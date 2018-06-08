---
title: Créer une Dimension en générant une Table Non temporelle dans la Source de données | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0f9f84131307282b00fd99c1d5770ac352e31713
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-dimension-by-generating-a-non-time-table-in-the-data-source"></a>Créer une dimension en générant une table non temporelle dans la source de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], l’Assistant Dimension de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] permet de créer une dimension sans utiliser de source de données existante. Pour ce faire, sélectionnez l’option **Générer une table non temporelle dans la source de données** de la page **Sélectionner une méthode de création** de l’Assistant. Pour créer une table de dimension dans la source de données sous-jacente, vous devez avoir l'autorisation de créer des objets dans la source de données sous-jacente. Si vous définissez une dimension sans vue de source de données prédéfinie, vous pouvez la définir entièrement ou utiliser un modèle de dimension.  
  
 L'Assistant Dimension fournit des exemples de modèles de dimension à partir desquels vous pouvez générer un type de dimension fréquemment utilisé. Vous pouvez sélectionner l'un des types de dimensions suivants :  
  
-   Compte  
  
-   Customer  
  
-   Date  
  
-   Service  
  
-   Devise de destination  
  
-   Employee  
  
-   Géographie  
  
-   Détails des commandes client sur Internet  
  
-   Organization  
  
-   Product  
  
-   Promotion  
  
-   Détails des commandes client du revendeur  
  
-   Reseller  
  
-   Canal de vente  
  
-   Motif de vente  
  
-   Détails des commandes récapitulatives client  
  
-   Sales Territory  
  
-   Scénario  
  
-   Devise source  
  
 Chaque modèle standard prend en charge les attributs que vous pouvez choisir d'inclure dans la dimension. Vous pouvez également ajouter vos propres fichiers modèles de dimension pour les dimensions qui sont fréquemment utilisées avec vos données. Les modèles de dimension sont dans le dossier suivant :  
  
 `C:\Program Files\Microsoft SQL Server\100\Tools\Templates\olap\1033\Dimension Templates`  
  
 Vous pouvez utiliser le Concepteur de dimensions après avoir terminé l'Assistant Dimension pour ajouter, supprimer et configurer des attributs et des hiérarchies dans la dimension.  
  
 Lorsque vous créez une dimension non temporelle sans utiliser de source de données, l'Assistant Dimension vous guide à travers les étapes de la spécification du type de dimension et de l'identification de l'attribut de clé et des dimensions à variation lente.  
  
## <a name="specify-dimension-type"></a>Spécification du type de dimension  
 Dans la page **Spécifier le type de dimension** de l’Assistant Dimension, vous pouvez spécifier le type de dimension. Si vous générez la dimension d'après un modèle, le type de dimension est défini automatiquement. Sur cette page, vous pouvez également sélectionner des attributs standard pour le type de dimension spécifié, s'ils sont disponibles.  
  
 Si vous avez sélectionné un modèle qui correspond à un type de dimension, cette page est remplie avec les options de ce type de dimension. Si vous n’avez pas sélectionné de modèle ou qu’il n’existe aucun type de dimension correspondant, le type de dimension par défaut est **Normal**. Si un type de dimension n'est pas déjà sélectionné, choisissez le type le plus approprié à la dimension que vous créez. Si aucun type approprié n’est répertorié pour **Type de dimension**, choisissez **Normal**.  
  
 Quand vous sélectionnez un type de dimension, l’Assistant répertorie les types d’attributs qui s’appliquent à cette dimension sous **Attributs de dimension**. Pour sélectionner un type d’attribut, cochez la case **Inclure** en regard du type d’attribut, puis tapez le nom de l’attribut sous **Attribut de dimension**. Le nom par défaut est le même que dans **Type d’attribut**.  
  
## <a name="identify-key-attribute-and-changing-dimensions"></a>Identifier l'attribut clé et les dimensions variables  
 Dans la page **Spécifier la clé et le type de la dimension** , sélectionnez l’attribut de votre choix comme attribut clé de la dimension. En règle générale, l'attribut correspond à la colonne clé primaire de la table de dimension principale et il indexe les membres feuille de la dimension.  
  
 Si vous avez sélectionné un modèle et qu'un attribut clé est défini dans le modèle, cet attribut est l'attribut clé par défaut. Si vous avez sélectionné un modèle et qu'aucun attribut de clé dans celui-ci, l'attribut de clé par défaut est le premier attribut de la liste. La liste contient tous les attributs que vous avez sélectionnés dans la page **Spécifier le type de dimension** . L’attribut de clé peut être n’importe quel attribut parmi ceux que vous avez sélectionnés dans la page **Spécifier le type de dimension** . Si vous n'avez sélectionné aucun attribut, l'Assistant crée automatiquement un attribut clé et le nomme en concaténant le nom et l'ID de la dimension.  
  
 Enfin, spécifiez si cette dimension est une dimension variable. Dans le temps, les membres d'une dimension variable se déplacent dans la hiérarchie. L'Assistant génère des colonnes supplémentaires et crée des attributs qui correspondent à ces colonnes. Ces colonnes permettront aux utilisateurs d'interroger la dimension de façon à tenir compte de la modification. Tous les packages que vous créerez ultérieurement avec l'Assistant Génération de schéma géreront les clés de substitution à partir des caractéristiques de dimension à variation lente de la dimension.  
  
 Quand vous cochez la case **Il s’agit d’une dimension variable** , l’Assistant Dimension définit les attributs indiqués dans le tableau suivant :  
  
|Attribut|Type|  
|---------------|----------|  
|SCD OriginalID|SCDOriginalID|  
|SCD End Date|SCDEndDate|  
|SCD Start Date|SCDStartDate|  
|SCD Status|SCDStatus|  
  
 La case **Il s’agit d’une dimension variable** est cochée par défaut si vous utilisez un modèle pour lequel ces attributs de dimension à variation lente sont définis. Si vous la désactivez, les attributs de dimension à variation lente sont supprimés de la dimension.  
  
 Vous pouvez utiliser le Concepteur de dimensions pour configurer les propriétés d'une dimension à variation lente.  
  
## <a name="completing-the-dimension-wizard"></a>Fin de l'Assistant Dimension  
 Dans la page **Fin de l’Assistant** , tapez le nom de la nouvelle dimension et affichez sa structure. Cochez la case **Créer le schéma maintenant** pour lancer l’Assistant Génération de schéma une fois que vous avez cliqué sur **Terminer**. En règle générale, vous ne devez pas activer cette case à cocher si vous prévoyez de créer des objets supplémentaires. Si vous n'activez pas cette case à cocher, vous pourrez utiliser le Concepteur de dimensions pour générer le schéma ultérieurement.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une Dimension de temps en générant une Table de temps](../../analysis-services/multidimensional-models/create-a-time-dimension-by-generating-a-time-table.md)   
 [Créer une Dimension de temps en générant une Table de temps](../../analysis-services/multidimensional-models/create-a-time-dimension-by-generating-a-time-table.md)  
  
  
