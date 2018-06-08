---
title: Créer des membres calculés | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a0cee9d01fb55ace4d7062f96b5d3ea16c026669
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-calculated-members"></a>Créer des membres calculés
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez créer des mesures ou des membres de dimension personnalisés, nommés « membres calculés », en combinant des données de cube, des opérateurs arithmétiques, des nombres et des fonctions. Par exemple, vous pouvez créer un membre calculé nommé Euros qui convertit les dollars en euros en multipliant une mesure de dollar existante par un taux de conversion. Les euros peuvent ensuite être affichés par l'utilisateur final dans une ligne ou une colonne séparée.  
  
 Les définitions du membre calculé sont stockées, mais leurs valeurs existent uniquement en mémoire. Dans l'exemple précédent, les valeurs en marks sont présentées aux utilisateurs finaux, mais elles ne sont pas stockées comme données du cube.  
  
 Vous créez les membres calculés dans les cubes. Pour créer un membre calculé, sous l’onglet **Calculs** du Concepteur de cube, cliquez sur l’icône **Nouveau membre calculé** de la barre d’outils. Cette commande affiche un formulaire dans lequel vous spécifiez les options suivantes pour le membre calculé :  
  
 **Nom**  
 Sélectionnez le nom du membre calculé. Ce nom apparaît en tant qu'en-tête de colonne ou de ligne pour les valeurs du membre calculé lorsque les utilisateurs finaux parcourent le cube.  
  
 **Hiérarchie parente**  
 Sélectionnez la hiérarchie parente à inclure dans le membre calculé. Les hiérarchies sont les catégories descriptives d'une dimension dans lesquelles les données numériques (c'est-à-dire les mesures) d'un cube peuvent être rangées séparément en vue de leur analyse. Dans les navigateurs tabulaires, les hiérarchies permettent à l'utilisateur final d'afficher les en-têtes de ligne et de colonne lorsqu'il parcourt les données d'un cube. Dans les navigateurs graphiques, elles fournissent d'autres types d'étiquette descriptive, mais elles ont la même fonction que dans les navigateurs tabulaires. Un membre calculé contient un nouvel en-tête (ou étiquette) dans la dimension parente que vous sélectionnez.  
  
 Vous pouvez également inclure le membre calculé dans les mesures au lieu de les inclure dans une dimension. Cette possibilité fournit aussi un nouvel en-tête de colonne ou de ligne, mais le membre calculé est associé aux mesures dans le navigateur.  
  
 **Membre parent**  
 Cliquez sur **Modifier** pour sélectionner un membre parent dans lequel inclure le membre calculé. Cette option n'est pas disponible si vous sélectionnez une hiérarchie à un niveau ou MESURES en tant que dimension parente.  
  
 Les hiérarchies sont divisées en niveaux qui contiennent les membres. Chaque membre produit un en-tête. Lorsqu'il parcourt les données dans un cube, l'utilisateur final peut descendre d'un en-tête sélectionné à des en-têtes subordonnés qui n'étaient pas visibles auparavant. L'en-tête du membre calculé est ajouté dans le niveau situé directement sous le membre parent que vous sélectionnez.  
  
 **Expression**  
 Spécifiez l'expression qui produit les valeurs du membre calculé. Cette expression peut être écrite en MDX L'expression peut contenir l'un des éléments suivants :  
  
-   les expressions de données qui représentent les composants de cube tels que les dimensions, les niveaux, les mesures, et ainsi de suite ;  
  
-   les opérateurs arithmétiques ;  
  
-   Nombres  
  
-   Fonctions  
  
 Vous pouvez faire glisser ou copier les composants de cube de l’onglet **Métadonnées** du volet **Outils de calcul** pour les ajouter rapidement à une expression.  
  
> [!IMPORTANT]  
>  Tout membre calculé devant être utilisé dans l'expression de valeur d'un autre membre calculé doit être créé avant le membre calculé qui l'utilise.  
  
 Chaîne de format  
 Spécifie le format des valeurs de cellules qui se basent sur le membre calculé. Cette propriété accepte les mêmes valeurs que la propriété **Format d’affichage** pour les mesures. Pour plus d’informations sur les formats d’affichage, consultez [Configurer des propriétés de mesure](../../analysis-services/multidimensional-models/configure-measure-properties.md).  
  
 Visible  
 Détermine si le membre calculé est visible ou masqué lors de l'extraction des métadonnées du cube. Même masqué, le membre calculé peut être utilisé dans les expressions MDX, les instructions et les scripts, mais il n'est pas affiché comme un objet pouvant être sélectionné dans les interfaces utilisateur clientes.  
  
 Comportement non vide  
 Stocke les noms des mesures utilisées pour répondre aux requêtes NON EMPTY en MDX. Si cette propriété est vide, le membre calculé doit être évalué à plusieurs reprises pour déterminer si un membre est vide. Si cette propriété contient le nom d'une ou plusieurs mesures, le membre calculé est traité comme vide si toutes les mesures spécifiées sont vides. Cette propriété est un indicateur d'optimisation pour qu'Analysis Services ne retourne que les enregistrements non NULL. Le fait de ne retourner que les enregistrements non NULL améliore les performances des requêtes MDX qui utilisent l'opérateur NON EMPTY ou la fonction NonEmpty, ou qui nécessitent le calcul de valeurs de cellules. Pour obtenir les meilleurs performances avec les calculs de cellules, ne spécifiez qu'un seul membre chaque fois que possible.  
  
 Expressions de couleur  
 Spécifie des expressions MDX qui déterminent dynamiquement les couleurs d'arrière-plan et d'avant-plan des cellules en se basant sur la valeur du membre calculé. Cette propriété est ignorée si elle n'est pas prise en charge par les applications clientes.  
  
 Expressions de police  
 Spécifie des expressions MDX qui déterminent dynamiquement la police, la taille de police et les attributs de police des cellules en se basant sur la valeur du membre calculé. Cette propriété est ignorée si elle n'est pas prise en charge par les applications clientes.  
  
 Vous pouvez faire glisser ou copier les composants de cube de l’onglet **Métadonnées** du volet **Outils de calcul** dans la zone **Expression** du volet des expressions de calcul. Vous pouvez faire glisser ou copier les fonctions de l’onglet **Fonctions** du volet **Outils de calcul** dans la zone **Expression** du volet des expressions de calcul.  
  
## <a name="addressing-calculated-members"></a>Adressage des membres calculés  
 Quand vous créez un membre calculé sous l’onglet **Calculs** du **Concepteur de cube**, vous spécifiez la hiérarchie parente dans laquelle le membre calculé est stocké. La hiérarchie parente détermine l'adressage d'un membre calculé à l'aide des règles suivantes :  
  
-   Si un membre calculé est créé dans la dimension de mesures, il est adressable dans cette dimension.  
  
## <a name="see-also"></a>Voir aussi  
 [Calculs dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md)  
  
  
