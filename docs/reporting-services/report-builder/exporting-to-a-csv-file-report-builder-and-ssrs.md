---
title: Exportation vers un fichier CSV (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 68ec746e-8c82-47f5-8c3d-dbe403a441e5
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: c92074304996729e0dd22ae218a0bafa884519d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="exporting-to-a-csv-file-report-builder-and-ssrs"></a>Exportation vers un fichier CSV (Générateur de rapports et SSRS)
  L’extension de rendu CSV (valeurs séparées par des virgules) permet de rendre les rapports paginés sous la forme d’une représentation aplatie des données d’un rapport dans un format standardisé, texte brut qui peut être facilement lu et échangé avec de nombreuses applications.  
  
 L'extension de rendu CSV utilise un caractère en tant que délimiteur de chaîne pour dissocier les champs et les lignes. Le délimiteur peut être configuré pour être un caractère autre que la virgule. Le fichier résultant peut être ouvert dans un tableur comme [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] ou utilisé comme format d'importation pour d'autres programmes. Le rapport exporté devient un fichier .csv et retourne un type MIME **texte/csv**.  
  
 Si vous souhaitez utiliser des données liées aux graphiques, barres de données, graphiques sparkline, jauges et indicateurs dans [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)], exportez le rapport vers un fichier CSV, puis ouvrez le fichier dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel.  
  
 Pour plus d’informations sur l’exportation au format CSV, consultez [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="CSVRendering"></a> Rendu CSV  
 Si un rapport CSV est rendu avec les paramètres par défaut, il présente les caractéristiques suivantes :  
  
-   La chaîne séparateur de champs par défaut est la virgule (,).  
  
    > [!NOTE]  
    >  Vous pouvez changer de séparateur de champs et utiliser n'importe quel caractère voulu, y compris TABULATION, ce en modifiant les paramètres d'informations de périphérique. Pour plus d'informations, consultez [CSV Device Information Settings](../../reporting-services/csv-device-information-settings.md).  
  
-   La chaîne séparateur des enregistrements est le retour chariot suivi du saut de ligne (\<cr>\<lf>).  
  
-   La chaîne d'identificateur de texte est le guillemet double (").  
  
     Le convertisseur CSV n'ajoute pas d'identificateurs autour de toutes les chaînes de texte. Les identificateurs de texte sont ajoutés uniquement lorsque la valeur contient le caractère délimiteur ou lorsque la valeur comporte un saut de ligne.  
  
-   Si le texte contient une chaîne de séparateur ou d'identificateur incorporée, l'identificateur est placé autour du texte et les chaînes d'identificateur incorporées sont doublées.  
  
-   La mise en forme et la mise en page sont ignorées.  
  
 Les éléments suivants sont ignorés lors du rendu :  
  
-   En-tête de page  
  
-   Pied de page  
  
-   Éléments de rapport personnalisés  
  
-   Ligne  
  
-   image  
  
-   Rectangle  
  
-   Sous-totaux automatiques  
  
 Les autres éléments de rapport sont triés, de haut en bas, puis de gauche à droite. Chaque élément est ensuite rendu dans une colonne. Si le rapport comporte des éléments de données imbriqués comme des listes ou des tableaux, les éléments parents sont répétés dans chaque enregistrement.  
  
 Le tableau suivant indique l'apparence des éléments de rapport lors du rendu :  
  
|Élément|Comportement de rendu|  
|----------|------------------------|  
|Zone de texte|Effectue le rendu du contenu de la zone de texte. En mode par défaut, les éléments sont mis en forme en fonction des propriétés de mise en forme de l'élément. En mode conforme, la mise en forme peut être modifiée par les paramètres d'informations de périphérique. Pour plus d'informations sur les modes de rendu CSV, voir ci-dessous.|  
|Table|Effectue le rendu en développant la table et en créant une ligne et une colonne pour chaque ligne et colonne au niveau de détails le plus bas. Les colonnes et les lignes de sous-total ne comprennent pas de titres de colonne ou de ligne. Les rapports d'extraction ne sont pas pris en charge.|  
|Matrice|Effectue le rendu en développant la matrice et en créant une ligne et une colonne pour chaque ligne et colonne au niveau de détails le plus bas. Les colonnes et les lignes de sous-total ne comprennent pas de titres de colonne ou de ligne.|  
|Liste|Effectue le rendu d'un enregistrement pour chaque instance ou ligne de détails dans la liste.|  
|Sous-rapport|L'élément parent est répété pour chaque instance du contenu.|  
|Graphique|Effectue le rendu en créant une ligne pour chaque valeur de graphique et étiquette de membre. Les étiquettes de séries et de catégories dans les hiérarchies sont aplaties et incluses dans la ligne pour une valeur de graphique.|  
|Barre de données|Effectue le rendu comme un graphique. En règle générale, une barre de données n'inclut pas de hiérarchies ou d'étiquettes.|  
|Graphique sparkline|Effectue le rendu comme un graphique. En règle générale, un graphique sparkline n'inclut pas de hiérarchies ou d'étiquettes.|  
|Jauge|Effectue le rendu en tant qu'enregistrement unique avec les valeurs maximale et minimale de l'échelle linéaire, les valeurs de début et de fin de la plage et la valeur du pointeur.|  
|Indicateur|Effectue le rendu en tant qu'enregistrement unique avec le nom de l'état actif, les états disponibles et la valeur de données.|  
|Carte|Restitue une ligne avec les étiquettes et valeurs de chaque membre cartographique d'une couche.<br /><br /> Si la carte comporte plusieurs couches, les valeurs dans les lignes varient selon que les couches utilisent des régions de données cartographiques identiques ou différentes. Si plusieurs couches utilisent la même région de données, les lignes contiennent des données de toutes les couches.|  
  
### <a name="hierarchical-and-grouped-data"></a>Données hiérarchiques et groupées  
 Les données hiérarchiques et groupées doivent être aplaties pour être représentées au format CSV.  
  
 L'extension de rendu aplatit le rapport en arborescence qui représente les groupes imbriqués dans la région de données. Pour aplatir le rapport :  
  
-   Une hiérarchie de ligne est aplatie avant une hiérarchie de colonne.  
  
-   Les colonnes sont ordonnées comme suit : zones de texte dans le corps ordonnées de gauche à droite et de haut en bas, suivies des régions de données ordonnées de gauche à droite et de haut en bas.  
  
-   Dans une région de données, les colonnes sont ordonnées comme suit : membres d'angle, membres de hiérarchie de ligne, membres de hiérarchie de colonne et enfin cellules.  
  
-   Les régions de données d'homologue sont des régions de données ou des groupes dynamiques qui partagent une région de données commune ou un ancêtre dynamique commun. Les données d'homologue sont identifiées par une création de branche au niveau de l'arborescence aplatie.  
  
 Pour plus d'informations, consultez [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md).  
  
  
##  <a name="RenderingModes"></a> Modes du convertisseur  
 L'extension de rendu CSV peut fonctionner dans deux modes : l'un est optimisé pour Excel, l'autre pour les applications tierces qui requièrent une conformité stricte à la spécification CSV décrite dans le document RFC 4180. Selon le mode que vous utilisez, les régions de données d'homologue sont gérées différemment.  
  
### <a name="default-mode"></a>Mode par défaut  
 Le mode par défaut est optimisé pour Excel. En cas de rendu en mode par défaut, le rapport est rendu sous la forme d'un fichier CSV contenant plusieurs sections de données avec rendu CSV. Chaque région de données d'homologue est délimitée par une ligne vide. Les régions de données d'homologue situées dans le corps du rapport sont rendues sous la forme de blocs de données distincts dans le fichier CSV. Le résultat est un fichier CSV dans lequel :  
  
-   Les zones de texte individuelles situées dans le corps du rapport sont rendues une fois en tant que premier bloc de données dans le fichier CSV.  
  
-   Chaque région de données d'homologue de niveau supérieur dans le corps du rapport est rendue dans son propre bloc de données.  
  
-   Les régions de données imbriquées sont rendues en diagonale dans le même bloc de données.  
  
#### <a name="formatting"></a>Mise en forme  
 Les valeurs numériques sont rendues dans leur état mis en forme. Excel peut reconnaître les valeurs numériques mises en forme, telles que la devise, le pourcentage et la date, et met en forme les cellules de façon appropriée lors de l'importation du fichier CSV.  
  
### <a name="compliant-mode"></a>Mode conforme  
 Le mode conforme est optimisé pour les applications tierces.  
  
#### <a name="data-regions"></a>Régions de données  
 Seule la première ligne du fichier contient les en-têtes de colonne et chaque ligne contient le même nombre de colonnes.  
  
#### <a name="formatting"></a>Mise en forme  
 Les valeurs sont dépourvues de mise en forme.  
  
##  <a name="Interactivity"></a> Interactivité  
 L'interactivité n'est pas prise en charge par les formats CSV générés par ce convertisseur. Les éléments interactifs suivants ne sont pas rendus :  
  
-   Liens hypertexte  
  
-   Afficher ou masquer  
  
-   Explorateur de documents  
  
-   Liens d'extraction ou interactifs  
  
-   Tri d'utilisateur final  
  
-   En-têtes fixes  
  
-   Signets  
  
  
##  <a name="DeviceInfo"></a> Paramètres d'informations de périphérique  
 Vous pouvez modifier certains paramètres par défaut de ce convertisseur, notamment le mode de rendu, les caractères à utiliser comme séparateurs et ceux à utiliser comme chaîne par défaut d'identificateur de texte, ce en modifiant les paramètres d'informations de périphérique. Pour plus d'informations, consultez [CSV Device Information Settings](../../reporting-services/csv-device-information-settings.md).  
  
  
## <a name="see-also"></a> Voir aussi  
 [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Fonctionnalités interactives des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Rendu des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
