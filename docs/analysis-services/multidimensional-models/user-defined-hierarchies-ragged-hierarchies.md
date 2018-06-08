---
title: Les hiérarchies irrégulières | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4c5eb53a8ae3ff25b7c0b4d390d9c5ffc896e542
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="user-defined-hierarchies---ragged-hierarchies"></a>Hiérarchies définies par l’utilisateur - hiérarchies irrégulières
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une hiérarchie déséquilibrée est une hiérarchie définie par l'utilisateur qui a un nombre de niveaux impair. Voici quelques exemples : un organigramme où un responsable hiérarchique a comme subordonnés directs des cadres du service et des non cadres, ou des hiérarchies avec des attributs Country-Region-City, où certaines villes n'ont pas d'état (State) ou de province (Province) parent, notamment Washington D.C., la Cité du Vatican ou New Dehli.  
  
 Pour la plupart des hiérarchies dans une dimension, chaque niveau a le même nombre de membres au-dessus de lui que tout autre membre au même niveau. Dans une hiérarchie déséquilibrée, en revanche, le membre parent logique d'au moins un membre ne figure pas dans le niveau immédiatement supérieur à ce membre. En pareil cas, la hiérarchie descend à des niveaux différents pour des chemins d'extraction différents. Dans une application cliente, cela peut rendre les chemins d'extraction inutilement compliqués.  
  
 Les applications clientes varient dans la manière de gérer une hiérarchie déséquilibrée. S'il existe des hiérarchies déséquilibrées dans votre modèle, vous devrez effectuer un travail supplémentaire pour obtenir le comportement de rendu attendu.  
  
 Dans un premier temps, vérifiez la façon dont l'application cliente gère le chemin d'extraction. Par exemple, Excel répète les noms parents en tant qu'espaces réservés pour les valeurs manquantes. Pour afficher ce comportement, créez un tableau croisé dynamique à l'aide de la dimension Sales Territory dans le modèle multidimensionnel Adventure Works. Dans un tableau croisé dynamique avec les attributs Group, Country et Region pour Sales Territory, vous constaterez que les pays pour lesquels il manque une valeur de région obtiennent un espace réservé, dans ce cas une répétition du parent au-dessus (le nom du pays). Ce comportement dérive de la propriété de chaîne de connexion MDX Compatibility=1 qui est fixe dans Excel. Si le client ne fournit pas naturellement les comportements d'extraction dont vous avez besoin, vous pouvez définir les propriétés du modèle pour modifier au moins certains de ces comportements.  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Approches pour modifier la navigation d'exploration dans une hiérarchie déséquilibrée](#bkmk_approach)  
  
-   [Définition de HideMemberIf pour masquer les membres dans une hiérarchie normale](#bkmk_Hide)  
  
-   [Définir la compatibilité MDX pour déterminer le mode de représentation des espaces réservés dans les applications clientes](#bkmk_Mdx)  
  
##  <a name="bkmk_approach"></a> Approches pour modifier la navigation d'exploration dans une hiérarchie déséquilibrée  
 La présence d'une hiérarchie déséquilibrée est un problème lorsque la navigation d'extraction ne retourne pas les valeurs attendues ou est perçue comme une utilisation maladroite. Pour résoudre les problèmes de navigation qui résultent de hiérarchies déséquilibrées, considérez ces options :  
  
-   Utilisez une hiérarchie régulière, mais définissez la propriété **HideMemberIf** à chaque niveau pour spécifier si un niveau manquant s'affiche pour l'utilisateur. Lorsque vous définissez **HideMemberIf**, vous devez également définir **MDXCompatibility** dans la chaîne de connexion pour remplacer les comportements de navigation par défaut. Les instructions de définition de ces propriétés sont fournies dans cette rubrique.  
  
-   Créez une hiérarchie parent-enfant qui gère explicitement les membres de niveau. Pour une présentation de la technique, consultez [Hiérarchie déséquilibrée dans SSAS (billet de blog)](http://dwbi1.wordpress.com/2011/03/30/ragged-hierarchy-in-ssas/). Pour plus d’informations dans la documentation en ligne, consultez [Dimensions parent-enfant](../../analysis-services/multidimensional-models/parent-child-dimension.md). Les inconvénients liés à la création d'une hiérarchie parent-enfant résident dans le fait que vous ne pouvez en avoir qu'une seule par dimension, ce qui entraîne généralement une baisse des performances lors du calcul des agrégations pour les membres intermédiaires.  
  
 Si votre dimension contient plusieurs hiérarchies déséquilibrées, vous devez utiliser la première approche, en définissant **HideMemberIf**. Les développeurs BI qui ont une expérience pratique des hiérarchies déséquilibrées vont plus loin et recommandent d'apporter des modifications supplémentaires aux tables de données physiques, en créant des tables distinctes pour chaque niveau. Consultez [Cube financier SSAS de Martin Mason Première partie a : Hiérarchies déséquilibrées (blog)](http://martinmason.wordpress.com/2012/03/03/the-ssas-financial-cubepart-1aragged-hierarchies-cont/) pour plus de détails sur cette technique.  
  
##  <a name="bkmk_Hide"></a> Définition de HideMemberIf pour masquer les membres dans une hiérarchie normale  
 Dans la table d'une dimension déséquilibrée, les membres manquants peuvent être représentés de différentes manières. Les espaces réservés des cellules de table peuvent être des valeurs NULL ou des chaînes vides, ou bien la même valeur que leurs parents. La représentation des espaces réservés est déterminée par l'état d'espace réservé de membres enfants, conformément à la propriété **HideMemberIf** et la propriété de chaîne de connexion **MDX Compatibility** pour l'application cliente.  
  
 Pour les applications clientes qui autorisent l'affichage de hiérarchies déséquilibrées, vous pouvez utiliser ces propriétés de façon à masquer les membres manquants.  
  
1.  Dans SSDT, double-cliquez sur une dimension pour l'ouvrir dans le Concepteur de dimensions. Le premier onglet, Structure de dimension, affiche les hiérarchies d'attribut dans le volet Hiérarchies.  
  
2.  Cliquez avec le bouton droit sur un membre de la hiérarchie et choisissez **Propriétés**. Affectez à la propriété **HideMemberIf** une des valeurs décrites ci-après.  
  
    |Paramètre de HideMemberIf| Description|  
    |--------------------------|-----------------|  
    |**Never**|Les membres de ce niveau ne sont jamais masqués. Ceci est la valeur par défaut.|  
    |**OnlyChildWithNoName**|Un membre de ce niveau est masqué quand il est le seul enfant de son parent et qu'il a pour nom la valeur NULL ou une chaîne vide.|  
    |**OnlyChildWithParentName**|Un membre de ce niveau est masqué quand il est le seul enfant de son parent et qu'il porte le même nom que son parent.|  
    |**NoName**|Un membre de ce niveau est masqué lorsque son nom est vide.|  
    |**ParentName**|Un membre de ce niveau est masqué quand son nom est identique à celui de son parent.|  
  
##  <a name="bkmk_Mdx"></a> Définir la compatibilité MDX pour déterminer le mode de représentation des espaces réservés dans les applications clientes  
 Après avoir défini **HideMemberIf** sur un niveau de hiérarchie, vous devez également définir la propriété **MDX Compatibility** dans la chaîne de connexion envoyée par l'application cliente. La définition de **MDX Compatibility** détermine si **HideMemberIf** est utilisé.  
  
|Paramètre de compatibilité MDX| Description|Utilisation|  
|-------------------------------|-----------------|-----------|  
|**1**|Afficher une valeur d'espace réservé.|Il s'agit de la valeur par défaut utilisée par Excel, SSDT et SSMS. Elle indique au serveur de retourner les valeurs d'espace réservé lors de l'exploration de niveaux vides dans une hiérarchie déséquilibrée. Si vous cliquez sur la valeur d'espace réservé, vous pouvez continuer à explorer le bas de la hiérarchie pour obtenir les nœuds enfants (feuille).<br /><br /> Excel possède la chaîne de connexion à Analysis Services, et affecte toujours la valeur 1 à **MDX Compatibility** pour chaque nouvelle connexion. Ce comportement préserve la compatibilité descendante.|  
|**2**|Masquez une valeur d'espace réservé (valeur Null ou dupliquée du niveau parent), mais affichez d'autres niveaux et nœuds avec des valeurs pertinentes.|**MDX Compatibility**=2 est généralement considéré comme le paramètre privilégié pour les hiérarchies déséquilibrées. Un rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et certaines applications clientes tierces peuvent rendre ce paramètre persistant.|  
  
## <a name="see-also"></a>Voir aussi  
 [Créer des hiérarchies définies par l'utilisateur](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md)   
 [Hiérarchies utilisateur](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies.md)   
 [Dimensions parent-enfant](../../analysis-services/multidimensional-models/parent-child-dimension.md)   
 [Propriétés de chaîne de connexion & #40 ; Analysis Services & #41 ;](../../analysis-services/instances/connection-string-properties-analysis-services.md)  
  
  
