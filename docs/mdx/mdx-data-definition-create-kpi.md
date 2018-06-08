---
title: Instruction CREATE KPI (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7c2322d30e4e42bebbe4537f687c349c5179d9f9
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579321"
---
# <a name="mdx-data-definition---create-kpi"></a>Définition de données MDX - créer un KPI
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Crée un indicateur de performance clé (KPI, &lt;legacyItalic&gt;key performance indicator&lt;/legacyItalic&gt;). Un indicateur de performance clé est un ensemble de calculs associés à un groupe de mesures dans un cube et utilisés pour évaluer les performances de l'entreprise ou du scénario.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE KPI CURRENTCUBE | <Cube Name>.KPI_Name AS KPI_Value  
   [,Property_Name = Property_Value, ...n]  
```  
  
## <a name="arguments"></a>Arguments  
 *Nom_icp*  
 Chaîne valide qui fournit le nom d'un indicateur de performance clé.  
  
 *KPI_Value*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une valeur numérique.  
  
 *Property_name*  
 Chaîne valide qui fournit le nom d'une propriété d'indicateur de performance clé.  
  
 *Nom*  
 Expression scalaire valide qui définit la valeur de la propriété d'indicateur de performance clé.  
  
## <a name="remarks"></a>Notes  
 La spécification d'un cube autre que le cube actuellement connecté génère une erreur. C'est pourquoi vous devez utiliser CURRENTCUBE de préférence à un nom de cube, pour désigner le cube actuel.  
  
## <a name="kpi-properties"></a>Propriétés d'indicateur de performance clé  
 Le tableau suivant décrit toutes les propriétés KPI. Aucune de ces propriétés n'a de valeur par défaut. Par conséquent, jusqu'à ce qu'une valeur spécifique soit affectée à une propriété d'indicateur de performance clé, les requêtes sur ces propriétés retourneront une valeur Null.  
  
|Identificateur de propriété|Signification|  
|-------------------------|-------------|  
|GOAL|Expression MDX valide qui retourne une valeur numérique.|  
|STATUS|Expression MDX valide qui retourne une valeur numérique.|  
|STATUS_GRAPHIC|Chaîne qui définit un ensemble d'images graphiques qui sera utilisé par l'application cliente.|  
|TREND|Expression MDX valide qui retourne une valeur numérique.|  
|TREND_GRAPHIC|Chaîne qui définit un ensemble d'images graphiques qui sera utilisé par l'application cliente.|  
|WEIGHT|Expression MDX valide qui retourne une valeur numérique.|  
|CURRENT_TIME_MEMBER|Expression MDX valide qui retourne un membre dans la dimension de temps. CURRENT_TIME_MEMBER définit le point de référence pour toutes les fonctions d'heure relative.|  
|PARENT_KPI|Chaîne qui spécifie le nom de l'indicateur de performance clé parent.|  
|CAPTION|Chaîne que l'application cliente utilise à titre de légende de l'indicateur de performance clé.|  
|DISPLAY_FOLDER|Chaîne qui spécifie le chemin d'accès au dossier d'affichage dans lequel l'application cliente doit afficher l'indicateur de performance clé. Le séparateur de niveau de dossier est défini par l'application cliente. Pour les outils et les clients fournis par [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], la barre oblique inverse (\\) est le séparateur de niveau. Pour fournir plusieurs dossiers d’affichage pour un membre défini, utilisez un point-virgule ( ;) pour séparer les dossiers|  
|ASSOCIATED_MEASURE_GROUP|Chaîne qui spécifie le nom du groupe de mesures auquel tous les calculs MDX doivent faire référence.|  
  
 Les valeurs des propriétés GOAL, STATUS et TREND sont des expressions MDX qui doivent être comprises entre -1 et 1. Toutefois, c'est l'application cliente qui définit la plage de valeurs réelle pour ces propriétés. Lorsque vous utilisez les outils et les clients fournis par [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] pour parcourir les indicateurs de performance clés, les valeurs inférieures à -1 sont assimilées à -1, et les valeurs supérieures à 1 sont assimilées à 1.  
  
 STATUS_GRAPHIC et TREND_GRAPHIC sont des valeurs de chaîne que l'application cliente utilise pour identifier le bon ensemble d'images à afficher. Ces chaînes définissent également le comportement de la fonction d'affichage, à savoir le nombre d'états à afficher (il s'agit généralement d'un nombre impair) et les images à utiliser pour chacun d'eux.  
  
### <a name="kpi-graphics-in-sql-server-data-tools"></a>Graphiques d'indicateur de performance clé dans les outils de données SQL Server  
 Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], les graphiques d'indicateur de performance clé peuvent avoir trois ou cinq états. Le tableau suivant définit les valeurs pour chacun de ces États.  
  
|Nombre d'états du graphique d'indicateur de performance clé|Valeur de ces états|  
|--------------------------------------|---------------------------|  
|3|Mauvais = -1 à -0,5<br /><br /> OK =-0.4999 à 0,4999<br /><br /> Bon = 0,50 à 1|  
|5|Mauvais = -1 à -0,75<br /><br /> Risque = -0,7499 à -0,25<br /><br /> OK = -0,2499 à 0,2499<br /><br /> En hausse = 0,25 à 0,7499<br /><br /> Bon = 0,75 à 1|  
  
> [!NOTE]  
>  Pour certains graphiques, comme la jauge inversée ou la flèche d'état inversée, la plage est inversée. Autrement dit, -1 signifie bon et 1 signifie mauvais.  
  
 Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], le nom du graphique d'indicateur de performance clé détermine si le graphique possède trois ou cinq états. Le tableau suivant répertorie l’utilisation, le nom et le nombre d’états que [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] associe à ses graphiques d’indicateur de performance clé.  
  
|Utilisation du graphique|Nom du graphique d'indicateur de performance clé|Nombre d'états|  
|--------------------|-------------------------|----------------------|  
|État|Formes|3|  
|État|Feu de circulation|3|  
|État|Panneaux de signalisation|3|  
|État|Jauge|3|  
|État|Jauge inversée|5|  
|État|Thermomètre|3|  
|État|Cylindre|3|  
|État|Faces|3|  
|État|Flèche de variance|3|  
|Tendance|Flèche standard|3|  
|Tendance|Flèche d'état|3|  
|Tendance|Flèche d'état inversée|5|  
|Tendance|Faces|3|  
  
## <a name="see-also"></a>Voir aussi  
 [Instruction d’indicateur de performance clé DROP &#40;MDX&#41;](../mdx/mdx-data-definition-drop-kpi.md)   
 [Instructions MDX de définition de données &#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)  
  
  
