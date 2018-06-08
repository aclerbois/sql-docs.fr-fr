---
title: Créer un Cube à partir d’un modèle sans utiliser une vue de Source de données | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 654278ee171666564771a3e620c903e598039a74
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-cube-from-a-template-without-using-a-data-source-view"></a>Créer un cube a partir d'un modèle sans utiliser de vue de source de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Sélectionnez **Construire le cube sans utiliser de source de données** dans la première page de l’Assistant Cube pour créer un cube sans utiliser de vue de source de données. Vous pouvez par la suite utiliser l’Assistant Génération de schéma pour générer le schéma relationnel pour la vue de source de données basée sur la structure du cube et éventuellement d’autres objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations sur la génération d’un schéma, consultez [Assistant Génération de schéma &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/schema-generation-wizard-analysis-services.md).  
  
## <a name="selecting-the-build-method"></a>Sélection de la méthode de construction  
 Dans l’Assistant Cube, dans la page **Sélectionner la méthode de construction** , cliquez sur **Construire le cube sans utiliser de source de données**. Pour construire le cube à l’aide d’un modèle de cube existant, cochez la case **Utiliser un modèle de cube** . . Si vous ne choisissez pas d'utiliser un modèle, vous devez définir les options manuellement.  
  
 Les modèles de cube contiennent des mesures, des groupes de mesures, des dimensions, des hiérarchies et des attributs prédéfinis. Si vous sélectionnez un modèle, l'Assistant utilise les définitions d'objet dans les modèles comme base pour définir des options dans les pages suivantes. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est installé avec plusieurs modèles pour les cubes standard. L'administrateur de serveur peut également ajouter les modèles de cube ou de dimension conçus spécifiquement pour les données de votre organisation.  
  
## <a name="selecting-dimensions"></a>Sélection de dimensions  
 Utilisez la page **Sélectionner des dimensions** de l'Assistant pour ajouter les dimensions existantes au cube. Cette page s'affiche uniquement s'il existe déjà des dimensions partagées sans source de données dans le projet ou la base de données. Elle ne présente pas les dimensions qui ont une source de données.  
  
 Pour ajouter des dimensions existantes, sélectionnez-en une ou plusieurs dans la liste **Dimensions partagées** et cliquez sur le bouton représentant une flèche droite (**>**) pour les placer dans la liste **Dimensions du cube** . Cliquez sur le bouton à deux flèches (**>>**) pour déplacer toutes les dimensions de la liste.  
  
## <a name="defining-new-measures"></a>Définition de nouvelles mesures  
 Utilisez la page **Définir de nouvelles mesures** de l’Assistant pour spécifier les mesures et les groupes de mesures du nouveau cube. Les groupes de mesures que vous spécifiez ici correspondront aux tables de faits dans le schéma généré. Les mesures que vous spécifiez ici correspondront aux colonnes non-clés numériques dans les tables.  
  
 Si vous utilisez un modèle pour créer le cube, les mesures du modèle sont répertoriées sous forme de grille sous **Sélectionnez des mesures dans le modèle**. La case à cocher en regard de chaque mesure dans la liste est initialement sélectionnée. Vous pouvez désactiver la case à cocher en regard de toute mesure que vous ne souhaitez pas inclure dans le cube. Pour ajouter ou supprimer toutes les mesures de la liste, sélectionnez ou désactivez la case à cocher dans la barre de titre de la grille.  
  
 Vous pouvez ajouter des mesures au cube dans la liste située sous **Ajoutez de nouvelles mesures**. Pour ajouter une nouvelle mesure, cliquez sur la première cellule vide dans la colonne **Nom de la mesure** (qui affiche **Ajouter une nouvelle mesure**). Spécifiez un nom de mesure, un groupe de mesures, un type de données et une agrégation pour chaque nouvelle mesure. Pour supprimer une mesure de la liste **Ajoutez de nouvelles mesures** , cliquez sur l’icône de suppression (**X**). Si vous n’utilisez pas de modèle, **Ajoutez de nouvelles mesures** est la seule liste dans cette page de l’Assistant.  
  
 La grille **Sélectionnez des mesures dans le modèle** et la grille **Ajoutez de nouvelles mesures** affichent des valeurs dans les colonnes décrites dans le tableau suivant. Vous pouvez cliquer sur une valeur dans l'une ou l'autre liste pour la modifier.  
  
|Colonne|Description|  
|------------|-----------------|  
|**Nom de la mesure**|Une valeur de cette colonne définit le nom d'une mesure dans le cube. Cliquez sur une valeur dans cette colonne pour taper un nom. Cliquez sur **Ajouter une nouvelle mesure** dans cette colonne pour créer une nouvelle mesure. Cette colonne définit la propriété **Name** de l’objet de mesure.|  
|**Groupe de mesures**|Nom du groupe de mesures qui contient la mesure. Cliquez sur cette valeur pour effectuer votre choix ou tapez un nom. Si vous supprimez toutes les mesures qui appartiennent à un groupe de mesures particulier, le groupe de mesures est également supprimé. Cette colonne définit la propriété **Name** du groupe de mesures.|  
|**Type de données**|Type de données de la mesure. Cliquez sur cette valeur pour modifier le type de données. La valeur par défaut au moment de créer une mesure est **Unique**. Cette colonne définit la propriété **DataType** de l’objet de mesure.|  
|**Agrégation**|Agrégation standard de la mesure. Cliquez sur cette cellule pour spécifier l’une des agrégations standard de la mesure (ou **Aucune**). La valeur par défaut au moment de créer une mesure est **Somme**. Cette colonne définit la propriété **AggregationFunction** de l’objet de mesure.|  
  
## <a name="defining-new-dimensions"></a>Définition de nouvelles dimensions  
 Utilisez la page **Définir de nouvelles dimensions** de l’Assistant pour spécifier les dimensions du nouveau cube.  
  
 Si vous utilisez un modèle pour créer le cube, la grille située sous **Sélectionnez des dimensions dans le modèle** affiche les dimensions du modèle. Vous pouvez désactiver la case à cocher en regard de n'importe quelle dimension pour la supprimer du cube. Désactivez la case à cocher dans la barre de titre de la grille pour supprimer toutes les dimensions répertoriées. Si vous n'utilisez pas de modèle, cette grille répertorie uniquement la dimension de temps.  
  
 Vous pouvez ajouter des dimensions au cube dans la grille située sous **Ajoutez de nouvelles dimensions**. Pour ajouter une dimension, cliquez dans la cellule de la colonne **Nom** qui contient le texte **Ajouter une nouvelle dimension**, puis tapez un nom pour la dimension. Pour supprimer une ligne de la liste, cliquez sur l’icône de suppression (**X**).  
  
 La grille **Sélectionnez des dimensions dans le modèle** et la grille **Ajoutez de nouvelles dimensions** affichent des valeurs dans les colonnes décrites dans le tableau suivant. Vous pouvez cliquer sur une valeur dans l'une ou l'autre liste pour la modifier.  
  
|Colonne|Description|  
|------------|-----------------|  
|**Type**|Affiche le type de dimension pour une dimension de modèle. Cliquez sur cette cellule pour modifier le type d'une dimension. Cette colonne définit la propriété **Type** de l’objet de dimension.|  
|**Nom**|Affiche le nom de la dimension. Cliquez sur cette cellule pour taper un nom différent. Cette valeur définit la propriété **Name** de l’objet de dimension.|  
|**SCD**|Spécifie qu'il s'agit d'une dimension à variation lente (SCD). Cette case à cocher ajoute l'ID d'origine de date de début et de date de fin SCD, ainsi que les attributs d'état à la dimension. **SCD** est activé par défaut si vous utilisez un modèle pour créer le cube et que l’Assistant détecte ces quatre types d’attribut dans une dimension de modèle.|  
|**Attributs**|Affiche les attributs qui doivent être créés pour la dimension. Chaque nom d'attribut dans la liste est précédé du nom de la dimension. Cette liste est en lecture seule. Vous pouvez modifier les attributs à l'aide du Concepteur de dimensions une fois l'exécution de l'Assistant terminée.|  
  
## <a name="defining-time-periods"></a>Définition de périodes  
 Utilisez la page **Définir des périodes** de l’Assistant pour spécifier la plage de dates que vous voulez inclure dans la dimension. Par exemple, vous pouvez choisir une plage commençant le 1er janvier de la première année de vos données et se prolongeant des années après votre transaction la plus actuelle. Les transactions qui sont en dehors de la plage n’apparaissent pas ou apparaissent en tant que membres inconnus dans la dimension, selon la valeur de la propriété **UnknownMemberVisible** de la dimension. La propriété **UnknownMemberName** spécifie la légende du membre inconnu. Vous pouvez également modifier le premier jour de la semaine utilisé par vos données (le jour par défaut est le dimanche).  
  
> [!NOTE]  
>  La page **Définir des périodes** s’affiche uniquement si vous incluez une dimension de temps dans votre cube dans la page **Définir de nouvelles dimensions** de l’Assistant.  
  
 Sélectionnez les périodes (**Année**, **Semestre**, **Trimestre**, **Quadrimestre**, **Mois**, **Décade**, **Semaine**et **Date**) que vous souhaitez inclure dans votre schéma. Vous devez sélectionner la période (date/heure) ; l'attribut Date étant l'attribut clé de la dimension, cette dernière ne peut pas fonctionner sans lui. Vous pouvez également modifier la langue utilisée pour étiqueter les membres de la dimension.  
  
 Les périodes que vous sélectionnez créent des attributs de temps correspondants dans la nouvelle dimension de temps. L'Assistant ajoute également les attributs associés qui n'apparaissent pas dans la liste. Par exemple, quand vous sélectionnez les intervalles de temps **Année** et **Semestre** , l’Assistant crée les attributs Jour de l’année, Jour du semestre et Semestres de l’année, en plus des attributs Année et Semestre.  
  
 Une fois le cube créé, vous pouvez utiliser le concepteur de dimensions pour ajouter ou supprimer des attributs de temps. L'attribut Date étant l'attribut clé de la dimension, vous ne pouvez pas le supprimer. Pour masquer l’attribut Date aux utilisateurs, vous pouvez remplacer la valeur de la propriété **AttributeHierarchyVisible** par **False**.  
  
 Toutes les périodes disponibles apparaissent dans le volet de périodes du concepteur de dimensions. (Ce volet remplace le volet **Vue de source de données** pour les dimensions basées sur les tables de dimension.) Vous pouvez modifier la plage de dates d’une dimension en modifiant le paramètre de propriété **Source** (liaison de temps) de la dimension. Comme il s'agit d'une modification structurelle, vous devez retraiter la dimension et les cubes qui l'utilisent avant de parcourir les données.  
  
## <a name="specifying-additional-calendars"></a>Spécification de calendriers supplémentaires  
 Dans la page **Spécifier des calendriers supplémentaires** de l’Assistant, sélectionnez les calendriers sur lesquels reposeront les hiérarchies de la dimension. Vous pouvez sélectionner l'un des calendriers suivants.  
  
|Calendrier|Description|  
|--------------|-----------------|  
|Calendrier fiscal|Calendrier fiscal de 12 mois. Si vous sélectionnez ce calendrier, définissez le jour et le mois de début de l'exercice fiscal utilisé par votre organisation.|  
|Calendrier de rapports (ou du marketing)|Calendrier de rapports de 12 mois qui contient deux mois de quatre semaines et un mois de cinq semaines sur un modèle récurrent de trois mois (trimestre). Si vous définissez ce calendrier, définissez le jour et le mois de départ et le modèle de trois mois de 4–4–5, 4–5–4 ou 5–4–4 semaines, où chaque chiffre correspond au nombre de semaines du mois.|  
|Calendrier de fabrication|Calendrier qui utilise 13 périodes de quatre semaines, divisé en trois trimestres de quatre périodes et un trimestre de cinq périodes. Si vous sélectionnez ce calendrier, définissez la semaine initiale (entre 1 et 4), le mois de l'année de fabrication et le trimestre avec des périodes supplémentaires.|  
|Calendrier ISO 8601|Calendrier standard de représentation de dates et d'heure ISO (International Organization for Standardization) (8601). Ce calendrier a un nombre intégral de semaines de sept jours. Pour éviter de fractionner une semaine, ce calendrier démarre une nouvelle année jusqu'à plusieurs jours avant ou après le 1er janvier.|  
  
 Le calendrier et les paramètres que vous sélectionnez déterminent les attributs qui sont créés dans la dimension. Par exemple, si vous sélectionnez les périodes **Année** et **Trimestre** dans la page **Définir des périodes** de l’Assistant, ainsi que **Calendrier fiscal** dans cette page, les attributs FiscalYear, FiscalQuarter et FiscalQuarterOfYear sont créés pour le calendrier fiscal.  
  
 L'Assistant crée également des hiérarchies spécifiques aux calendriers qui se composent des attributs créés pour le calendrier. Pour chaque calendrier, chaque niveau de chaque hiérarchie est reporté dans le niveau immédiatement supérieur. Par exemple, dans le calendrier standard de 12 mois, l'Assistant crée une hiérarchie d'Années et de Semaines ou d'Années et de Mois. Toutefois, les mois d'un calendrier standard ne contenant pas un nombre égal de semaines, il n'y a pas de hiérarchie d'Années, de Mois et de Semaines. À l'inverse, les mois d'un calendrier de fabrication ou de rapports contenant un nombre égal de semaines, les semaines dans ce calendrier sont regroupées dans les mois.  
  
## <a name="defining-dimension-usage"></a>Définition de l'utilisation de la dimension  
 Utilisez la page **Définir l’utilisation de la dimension** de l’Assistant pour spécifier les mesures de cube qui sont agrégées par chaque dimension de l’Assistant. La grille **Utilisation de la dimension** de cette page répertorie les dimensions sous forme de lignes et les groupes de mesures sous forme de colonnes. Activez la case à cocher pour n'importe quelle combinaison de dimensions et de groupes de mesures dans laquelle la dimension agrège les mesures de ce groupe de mesures.  
  
## <a name="completing-the-cube-wizard"></a>Fin de l'Assistant Cube  
 Dans la page **Fin de l’Assistant** , examinez la structure du nouveau cube et dans la zone **Nom du cube** , tapez son nom. Cochez éventuellement la case **Créer le schéma maintenant** pour lancer l’Assistant Génération de schéma. En règle générale, vous ne devez pas activer cette case à cocher si vous prévoyez de créer des objets supplémentaires. Vous pouvez également utiliser le Concepteur de cube pour générer le schéma ultérieurement.  
  
  
