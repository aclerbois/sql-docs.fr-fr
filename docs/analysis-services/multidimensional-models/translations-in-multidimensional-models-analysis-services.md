---
title: Traductions dans les modèles multidimensionnels (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6ac278f3e254e353d9ab7b6dc6d6fd7850b442c9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="translations-in-multidimensional-models-analysis-services"></a>Traductions dans les modèles multidimensionnels (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez définir des traductions dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en utilisant le concepteur correspondant à l’objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à traduire. La définition d’une traduction crée un objet **Translation** associé à l’objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] approprié qui a les valeurs littérales explicites spécifiées, dans la langue spécifiée, pour les propriétés de l’objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] associé.  
  
## <a name="elements-of-a-multi-lingual-data-model"></a>Éléments d’un modèle de données multilingues  
 Un modèle de données utilisé dans une solution multilingue nécessite davantage que des légendes traduites (noms de champs et descriptions). Il doit également fournir des valeurs de données exprimées dans différents scripts de langue. L'obtention d'une solution multilingue nécessite d'avoir des attributs individuels liés à des colonnes dans une base de données externe qui retournent les données.  
  
 L’exemple de base de données Adventure Works (multidimensionnelle, ainsi que l’entrepôt de données relationnelles) illustre la fonctionnalité de traduction d’Analyses Services. L'exemple de modèle comprend des légendes et des descriptions traduites. L'exemple d'entrepôt de données relationnelles contient des colonnes de valeurs traduites qui fournissent des membres d'attributs localisés dans le modèle.  
  
 Pour afficher les valeurs de données traduites accessibles au modèle  
  
1.  Ouvrez le modèle multidimensionnel Adventure Works dans le concepteur.  
  
2.  Dans l’Explorateur de solutions, ouvrez les vues de sources de données et double-cliquez sur Adventure Works DW\<version > .dsv.  
  
3.  Recherchez dimDate, dimProduct, dimProductCategory ou dimProductSubcateogry. Toutes ces dimensions contiennent des attributs pour les membres traduits pour le mois, le jour de semaine, le nom de produit, le nom de catégorie, et ainsi de suite.  
  
4.  Cliquez avec le bouton droit sur un champ et sélectionnez **Explorer les données**. Vous verrez des traductions en anglais, espagnol et français de chaque membre.  
  
 Les formats de date, d'heure et de devise ne sont pas implémentés dans les traductions. Pour fournir de manière dynamique des formats spécifiques à une culture en fonction des paramètres régionaux du client, utilisez l'Assistant Conversion monétaire et la propriété **FormatString** . Pour plus d’informations, consultez [Conversions monétaires &#40;Analysis Services&#41;](../../analysis-services/currency-conversions-analysis-services.md) et [Élément FormatString &#40;ASSL&#41;](../../analysis-services/scripting/properties/formatstring-element-assl.md).  
  
 [Lesson 9: Defining Perspectives and Translations](../../analysis-services/lesson-9-defining-perspectives-and-translations.md) d ans le didacticiel Analysis Services décrit les étapes de création et de test des traductions.  
  
## <a name="defining-translations"></a>Définition des traductions  
  
### <a name="add-translations-to-a-cube"></a>Ajouter des traductions à un cube  
 Vous pouvez ajouter des traductions au cube, aux groupes de mesures, aux mesures, à la dimension de cube, aux perspectives, aux indicateurs de performance clés, aux actions, aux jeux nommés et aux membres calculés.  
  
1.  Dans l'Explorateur de solutions, double-cliquez sur le nom du cube pour ouvrir le Concepteur de cube.  
  
2.  Cliquez sur l'onglet **Traductions** . Tous les objets qui prennent en charge les traductions sont répertoriés dans cette page.  
  
3.  Pour chaque objet, spécifiez la langue cible (résolue en interne en un LCID), la légende traduite et la description traduite. La liste des langues est cohérente dans Analysis Services, que vous définissiez la langue du serveur dans Management Studio ou que vous ajoutiez une traduction de remplacement sur un attribut unique.  
  
     N'oubliez pas que vous ne pouvez pas modifier le classement. Un cube utilise essentiellement un seul classement, même si vous prenez en charge plusieurs langues via des légendes traduites (il existe une exception pour les attributs de dimension, décrite ci-dessous). Si les langues ne sont pas triées correctement sous le classement partagé, vous devez effectuer des copies du cube simplement pour répondre à vos spécifications de classement.  
  
4.  Générez et déployez le projet.  
  
5.  Connectez-vous à la base de données à l'aide d'une application cliente, comme Excel, en modifiant la chaîne de connexion pour utiliser l'identificateur de paramètres régionaux. Pour plus d’informations, consultez [Conseils et meilleures pratiques en matière de globalisation &#40;Analysis Services&#41;](../../analysis-services/globalization-tips-and-best-practices-analysis-services.md).  
  
### <a name="add-translations-to-a-dimension-and-attributes"></a>Ajouter des traductions à une dimension et à des attributs  
 Vous pouvez ajouter des traductions à des dimensions de base de données, à des attributs, à des hiérarchies et à des niveaux au sein d'une hiérarchie.  
  
 Vous pouvez ajouter manuellement des légendes traduites au modèle à l'aide de votre clavier ou d'une opération copier-coller, mais pour les membres d'attributs de dimension, vous pouvez obtenir les valeurs traduites à partir d'une base de données externe. Plus précisément, la propriété **CaptionColumn** d'un attribut peut être liée à une colonne dans une vue de source de données.  
  
 Au niveau de l'attribut, vous pouvez remplacer les paramètres de classement. Par exemple, vous souhaiterez peut-être ajuster le respect de la largeur ou utiliser un tri binaire pour un attribut spécifique. Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], le classement est exposé là où les liaisons de données sont définies. Étant donné que vous liez une traduction d'attribut de dimension à une colonne source différente dans la vue de source de données, un paramètre de classement est disponible pour que vous puissiez spécifier le classement utilisé par la colonne source. Pour plus d'informations sur le classement de colonne dans la base de données relationnelle, voir [Set or Change the Column Collation](../../relational-databases/collations/set-or-change-the-column-collation.md) .  
  
1.  Dans l'Explorateur de solutions, double-cliquez sur le nom de la dimension pour ouvrir le Concepteur de dimensions.  
  
2.  Cliquez sur l'onglet **Traductions** . Tous les objets de dimension qui prennent en charge les traductions sont répertoriés dans cette page.  
  
     Pour chaque objet, spécifiez la langue cible (résolue en un LCID), la légende traduite et la description traduite. La liste des langues est cohérente dans Analysis Services, que vous définissiez la langue du serveur dans Management Studio ou que vous ajoutiez une traduction de remplacement sur un attribut unique.  
  
3.  Pour lier un attribut à une colonne fournissant des valeurs traduites  
  
    1.  Toujours dans le Concepteur de dimensions | **Traductions**, ajoutez une nouvelle traduction. Choisissez la langue. Une nouvelle colonne apparaît dans la page pour accepter les valeurs traduites.  
  
    2.  Placez le curseur dans une cellule vide adjacente à l'un des attributs. L'attribut ne peut pas être la clé, mais tous les autres attributs sont des options possibles. Vous devriez voir un petit bouton avec un point à l'intérieur. Cliquez sur le bouton pour ouvrir la boîte de dialogue **Traduction de données d'attribut**.  
  
    3.  Entrez une traduction de la légende. Elle sert d'étiquette de données dans le langage cible, par exemple comme nom de champ dans une liste de champs de tableau croisé dynamique.  
  
    4.  Choisissez la colonne source qui fournit les valeurs traduites des membres d'attribut. Seules les colonnes pré-existantes dans la table ou la requête liée à la dimension sont disponibles. Si la colonne n'existe pas, vous devez modifier la vue de source de données, la dimension et le cube pour sélectionner la colonne.  
  
    5.  Choisissez le classement et l'ordre de tri, le cas échéant.  
  
4.  Générez et déployez le projet.  
  
5.  Connectez-vous à la base de données à l'aide d'une application cliente, comme Excel, en modifiant la chaîne de connexion pour utiliser l'identificateur de paramètres régionaux. Pour plus d’informations, consultez [Conseils et meilleures pratiques en matière de globalisation &#40;Analysis Services&#41;](../../analysis-services/globalization-tips-and-best-practices-analysis-services.md).  
  
### <a name="add-a-translation-of-the-database-name"></a>Ajouter une traduction du nom de base de données  
 Au niveau de la base de données, vous pouvez ajouter des traductions pour le nom et la description de la base de données. Le nom traduit de la base de données peut être visible sur les connexions clientes qui spécifient le LCID de la langue, mais cela dépend de l'outil. Par exemple, l'affichage de la base de données dans Management Studio ne montre pas le nom traduit, même si vous spécifiez l'identificateur de paramètres régionaux sur la connexion. L'API utilisée par Management Studio pour se connecter à Analysis Services ne lit pas la propriété **Language** .  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet | **Modifier la base de données** pour ouvrir le Concepteur de base de données.  
  
2.  Dans Traductions, spécifiez la langue cible (résolue en un LCID), la légende traduite et la description traduite. La liste des langues est cohérente dans Analysis Services, que vous définissiez la langue du serveur dans Management Studio ou que vous ajoutiez une traduction de remplacement sur un attribut unique.  
  
3.  Dans la page Propriétés de la base de données, affectez à la propriété **Language** le même LCID que celui que vous avez spécifié pour la traduction. Si vous le souhaitez, vous pouvez aussi définir la propriété **Collation** si la valeur par défaut n'est plus justifiée.  
  
4.  Générez et déployez la base de données.  
  
## <a name="deleting-translation-objects"></a>Suppression des objets Translation  
 Vous pouvez cliquer avec le bouton droit sur un objet Translation dans le concepteur de cube ou de dimension afin de le supprimer définitivement. Vous ne pouvez pas restaurer ou recycler un objet supprimé. Par conséquent, veillez à vérifier la liste des objets concernés avant de continuer.  
  
## <a name="resolving-translations"></a>Résolution des traductions  
 Si une application cliente demande des informations dans un identificateur de langue spécifié, l’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tente de convertir les données et les métadonnées des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans l’identificateur de langue le plus proche possible. Si l’application cliente ne spécifie pas de langue par défaut ou si elle spécifie l’identificateur local neutre (0) ou l’identificateur de langue de traitement par défaut (1024), alors [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise la langue par défaut de l’instance pour renvoyer les données et les métadonnées des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 Si l'application cliente spécifie un identificateur de langue différent de l'identificateur de langue par défaut, l'instance parcourt toutes les traductions disponibles pour tous les objets disponibles. Si l’identificateur de langue spécifié correspond à l’identificateur de langue d’une traduction, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] renvoie cette traduction. Si aucune correspondance n'est trouvée, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tente d'utiliser l'une des méthodes suivantes pour renvoyer les traductions d'un identificateur de langue qui est le plus proche possible de l'identificateur de langue spécifié :  
  
-   Pour les identificateurs de langue suivants, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tente d’utiliser un identificateur de langue de remplacement si aucune traduction n’est définie pour l’identificateur de langue spécifié :  
  
    |Identificateur de langue spécifié|Identificateur de langue de remplacement|  
    |-----------------------------------|-----------------------------------|  
    |3076 - Chinois (RAS de Hong Kong, RPC)|1028 - Chinois (Taïwan)|  
    |5124 - Chinois (RAS de Macao)|1028 - Chinois (Taïwan)|  
    |1028 - Chinois (Taïwan)|Langue par défaut|  
    |4100 - Chinois (Singapour)|2052 - Chinois (RPC)|  
    |2074 - Croate|Langue par défaut|  
    |3098 - Croate (Cyrillique)|Langue par défaut|  
  
-   Pour tous les autres identificateurs de langue spécifiés, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] extrait la langue primaire de l’identificateur de langue spécifié et récupère l’identificateur de langue que Windows considère comme la meilleure correspondance pour la langue primaire. Si aucune traduction n'est trouvée pour la meilleure correspondance indiquée par Windows ou si l'identificateur de langue spécifié est la meilleure correspondance pour la langue primaire, la langue par défaut est utilisée.  
  
## <a name="see-also"></a>Voir aussi  
 [Scénarios de globalisation pour Analysis Services](../../analysis-services/globalization-scenarios-for-analysis-services.md)   
 [Langues et classements &#40;Analysis Services&#41;](../../analysis-services/languages-and-collations-analysis-services.md)  
  
  
