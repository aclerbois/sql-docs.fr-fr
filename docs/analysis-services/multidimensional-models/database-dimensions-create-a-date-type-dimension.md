---
title: Créer une Dimension de type Date | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 957f47bbb185f6d9029b9dfbb036aa3738448098
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="database-dimensions---create-a-date-type-dimension"></a>Dimensions de base de données : création d’une Dimension de type Date
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], une dimension de temps est un type de dimension dont les attributs représentent des périodes, telles que des années, des semestres, des trimestres, des mois et des jours. Les périodes d'une dimension de temps fournissent des niveaux de granularité de temps pour l'analyse et les rapports. Les attributs sont organisés en hiérarchies, la granularité de la dimension de temps est largement déterminée par les besoins commerciaux et les besoins en rapports pour les données historiques. Par exemple, la majorité des données financières et de vente dans les applications décisionnelles utilisent la granularité mensuelle ou trimestrielle.  
  
 En règle générale, les cubes [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] contiennent une dimension de temps sous une forme ou une autre. Un cube peut contenir plusieurs dimensions de temps ou plusieurs hiérarchies d'une même dimension de temps, selon la granularité des données et les besoins en rapports. Toutefois, les cubes ne nécessitent pas tous une dimension de temps. Certaines applications OLAP, telles que le calcul des coûts basés sur les activités, ne nécessitent pas une dimension de temps, car le calcul des coûts dans une dimension basée sur l'activité repose sur l'activité, et non sur le temps.  
  
## <a name="dimension-structure"></a>Structure de la dimension  
 La structure d'une dimension de temps dépend de la manière dont la source de données sous-jacente stocke les informations de périodes. Cette différence de stockage produit deux types de dimensions de temps de base :  
  
 Dimension de temps  
 Les dimensions de temps sont similaires aux autres dimensions en ceci qu'une table de dimension fournit les attributs de la dimension. Chaque colonne de la table principale de la dimension définit un attribut d'une période donnée.  
  
 À l'instar des autres dimensions, la table de faits a une relation de clé étrangère avec la table de dimension de la dimension de temps. L'attribut clé d'une dimension de temps est basé sur une clé entière ou sur le plus petit niveau de détail, tel que la date, qui apparaît dans la table principale de la dimension.  
  
 Dimension de temps du serveur  
 Si vous ne disposez pas d'une table de dimension à laquelle lier les attributs relatifs au temps, vous pouvez demander à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de définir une dimension de temps du serveur basée sur des périodes de temps. Pour définir les hiérarchies, les niveaux et les membres représentés par la dimension de temps du serveur, sélectionnez les périodes de temps standard où vous créez la dimension.  
  
 Les attributs d'une dimension de temps du serveur ont une liaison attribut-temps spéciale. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise les types d’attributs liés aux dates, tels que Year, Month ou Day, pour définir les membres des attributs d’une dimension de temps.  
  
 Lorsque vous incluez une dimension de temps du serveur dans un cube, vous définissez la relation entre le groupe de mesures et la dimension de temps du serveur en définissant une relation dans la page **Définir l’utilisation de la dimension** de l’Assistant Cube.  
  
### <a name="calendars"></a>Calendriers  
 Dans une dimension de temps ou une dimension de temps du serveur, les hiérarchies regroupent les attributs de périodes. Ces hiérarchies s'appellent généralement des calendriers.  
  
 Les applications décisionnelles nécessitent fréquemment plusieurs définitions de calendriers. Un service de Ressources Humaines, par exemple, peut suivre les employés à l’aide d’un calendrier *standard* (un calendrier grégorien de 12 mois commence le 1er janvier et se termine le 31 décembre). Toutefois, le service de Ressources Humaines peut suivre les dépenses à l’aide d’un calendrier *fiscal* (calendrier de 12 mois qui définit l’exercice utilisé par l’organisation).  
  
 Vous pouvez créer ces calendriers manuellement dans le Concepteur de dimensions. Toutefois, l'Assistant Dimension fournit des modèles de hiérarchies que vous pouvez utiliser pour générer automatiquement plusieurs types de calendriers lorsque vous créez une dimension de temps ou une dimension de temps du serveur. Le tableau suivant décrit les divers calendriers créés par l'Assistant Dimension.  
  
|Calendrier| Description|  
|--------------|-----------------|  
|Calendrier standard|Calendrier grégorien de 12 mois qui commence le 1er janvier et se termine le 31 décembre.<br /><br /> Que vous utilisiez l'Assistant Dimension pour créer une dimension de temps ou une dimension de temps du serveur, l'Assistant génère une hiérarchie pour un calendrier standard lorsque vous définissez les attributs qui représentent les périodes de la dimension. Si vous utilisez l’Assistant Dimension pour créer une dimension de temps du serveur, vous pouvez faire démarrer le calendrier standard à partir d'un jour autre que le 1er janvier.|  
|Calendrier fiscal|Calendrier fiscal de 12 mois. Lorsque vous sélectionnez ce calendrier, définissez le jour et le mois de début de l'exercice utilisé par l'organisation.<br /><br /> Remarque : ce calendrier n’est disponible que si vous utilisez l’Assistant Dimension pour créer une dimension de temps du serveur.|  
|Calendrier de rapports (calendrier marketing)|Calendrier de rapports de 12 mois qui contient deux mois de quatre semaines et un mois de cinq semaines répétés sur un modèle de trois mois (trimestre). Lorsque vous définissez ce calendrier, définissez le jour et le mois de départ et le modèle de trois mois de 4–4–5, 4–5–4 ou 5–4–4 semaines, où chaque chiffre correspond au nombre de semaines du mois.<br /><br /> Remarque : ce calendrier n’est disponible que si vous utilisez l’Assistant Dimension pour créer une dimension de temps du serveur.|  
|Calendrier de fabrication|Calendrier qui utilise 13 périodes de quatre semaines, divisé en trois trimestres de trois périodes et un trimestre de quatre périodes. Lorsque vous sélectionnez ce calendrier, vous définissez la semaine de début (entre 1 et 4) et le mois de l'année de fabrication utilisée par l'organisation, et vous définissez le trimestre qui contient quatre périodes.<br /><br /> Remarque : ce calendrier n’est disponible que si vous utilisez l’Assistant Dimension pour créer une dimension de temps du serveur.|  
|Calendrier ISO 8601|Calendrier standard de représentation de dates et d'heure ISO (International Organization for Standardization) (8601). Ce calendrier a un nombre intégral de semaines de sept jours. La nouvelle année peut démarrer plusieurs jours avant ou après le début de la nouvelle année du calendrier grégorien. La première semaine de ce calendrier est déterminée par la première semaine du calendrier grégorien qui contient un jeudi. Par conséquent, le premier jour de cette semaine, dimanche, peut tomber l'année précédente.<br /><br /> Remarque : ce calendrier n’est disponible que si vous utilisez l’Assistant Dimension pour créer une dimension de temps du serveur.|  
  
 Lorsque vous créez une dimension de temps du serveur et définissez les périodes et les calendriers à utiliser dans la dimension, l'Assistant Dimension ajoute les attributs des périodes correspondant à chaque calendrier défini. Par exemple, si vous créez une dimension de temps du serveur qui utilise les années comme période et qui contient des calendriers fiscaux et de rapports, l'assistant ajoute ensuite les attributs FiscalYear et ReportingYears ainsi que l'attribut standard Years à la dimension. Une dimension de temps du serveur aura également des attributs pour les combinaisons de périodes sélectionnées, tels que l'attribut DayOfWeek pour une dimension qui contient Days et Weeks. L'Assistant Dimension crée une hiérarchie de calendrier en combinant les attributs qui appartiennent à un seul type de calendrier. Par exemple, une hiérarchie de calendrier fiscal peut contenir les niveaux suivants : Année fiscale, Semestre fiscal, Trimestre fiscal, Mois fiscal et Jour fiscal.  
  
## <a name="adding-time-intelligence-with-the-business-intelligence-wizard"></a>Ajout d'éléments en fonction du temps avec l'Assistant Business Intelligence  
 Après avoir défini une dimension de temps et ajouté la dimension à un cube, vous pouvez utiliser l'Assistant Business Intelligence pour ajouter des fonctions en fonction du temps, telles que des mesures période à date, période à période et moyenne mobile. Pour plus d’informations, consultez [Définir des calculs Time Intelligence à l’aide de l’Assistant Business Intelligence](../../analysis-services/multidimensional-models/define-time-intelligence-calculations-using-the-business-intelligence-wizard.md).  
  
> [!NOTE]  
>  Vous ne pouvez pas utiliser l'Assistant Business Intelligence pour ajouter des éléments en fonction du temps à des dimensions de temps du serveur. L'Assistant Business Intelligence ajoute une hiérarchie pour prendre en charge les éléments en fonction du temps, et cette hiérarchie doit se trouver dans une colonne de la table de la dimension de temps. Les dimensions de temps du serveur n'ont pas de table de dimension de temps correspondante et ne peuvent donc pas prendre en charge cette hiérarchie supplémentaire.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une Dimension de temps en générant une Table de temps](../../analysis-services/multidimensional-models/create-a-time-dimension-by-generating-a-time-table.md)   
 [Aide F1 l’Assistant Business Intelligence](http://msdn.microsoft.com/library/155ac80c-63ae-47aa-9e86-9396e3d920eb)   
 [Types de dimensions](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/database-dimension-properties-types.md)  
  
  
