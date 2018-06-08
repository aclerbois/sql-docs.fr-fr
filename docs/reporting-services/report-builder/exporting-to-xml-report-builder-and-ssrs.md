---
title: Exportation au format XML (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 11d72068-2d97-495e-948f-12d1e8c1957d
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: aef21b126ae81b8821943f70594f04c5118e8446
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="exporting-to-xml-report-builder-and-ssrs"></a>Exportation vers XML (Générateur de rapports et SSRS)
  L’extension de rendu XML retourne un rapport paginé au format XML. Le schéma du rapport XML est spécifique du rapport et contient uniquement des données. Les informations de mise en page ne sont pas rendues et la pagination n'est pas conservée par l'extension de rendu XML. La sortie XML générée par cette extension peut être importée dans une base de données, utilisée en tant que message de données XML ou envoyée à une application personnalisée.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="ReportItems"></a> Éléments de rapport  
 Le tableau suivant décrit la façon dont les éléments de rapport sont rendus.  
  
|Élément|Comportement de rendu|  
|----------|------------------------|  
|Rapport|Rendu sous la forme d'un élément de premier niveau du document XML.|  
|Régions de données|Rendu sous la forme d'un élément à l'intérieur de l'élément de son conteneur. Les régions de données incluent les tables, matrices et listes qui affichent les données sous forme de texte, ainsi que les graphiques, barres de données, graphiques sparkline, jauges et indicateurs qui visualisent les données.|  
|Sections de groupe et de détails|Chaque instance est rendue sous la forme d'un élément à l'intérieur de l'élément de son conteneur.|  
|Zone de texte|Rendu sous la forme d'un attribut ou d'un élément à l'intérieur de son conteneur.|  
|Rectangle|Rendu sous la forme d'un élément à l'intérieur de son conteneur.|  
|Groupes de colonnes de matrices|Rendu sous la forme d'éléments à l'intérieur de groupes de lignes.|  
|Carte|Rendu sous la forme d'un élément à l'intérieur de l'élément de son conteneur. Les couches sont des éléments enfants de la carte et chaque couche comporte des éléments pour ses membres cartographiques et attributs de membre cartographique.|  
|Graphique|Rendu sous la forme d'un élément à l'intérieur de l'élément de son conteneur. Les séries sont des éléments enfants du graphique, et les catégories sont des éléments enfants d'une série. Effectue le rendu de toutes les étiquettes de graphiques pour chaque valeur de graphique. Les étiquettes et les valeurs sont incluses en tant qu'attributs.|  
|Barre de données|Rendu sous la forme d'un élément à l'intérieur de l'élément de son conteneur, comme un graphique. En règle générale, une barre de données n'inclut pas de hiérarchies ou d'étiquettes, seulement des valeurs.|  
|Graphique sparkline|Rendu sous la forme d'un élément à l'intérieur de l'élément de son conteneur, comme un graphique. En règle générale, un graphique sparkline n'inclut pas de hiérarchies ou d'étiquettes, seulement des valeurs.|  
|Jauge|Rendu sous la forme d'un élément à l'intérieur de l'élément de son conteneur. Effectue le rendu en tant qu'élément unique avec les valeurs maximale et minimale de l'échelle, les valeurs de début et de fin de la plage et la valeur du pointeur en tant qu'attributs.|  
|Indicateur|Rendu sous la forme d'un élément à l'intérieur de l'élément de son conteneur, comme une jauge. Effectue le rendu en tant qu'élément unique avec le nom de l'état actif, les états disponibles et la valeur de données en tant qu'attributs.|  
  
 Les rapports qui sont rendus à l'aide de l'extension de rendu XML sont également soumis aux règles suivantes :  
  
-   Les éléments et les attributs XML sont rendus dans leur ordre d'apparition dans la définition de rapport.  
  
-   La pagination est ignorée.  
  
-   Les en-têtes et les pieds de page ne sont pas rendus.  
  
-   Les éléments masqués qui ne peuvent pas être rendus visibles par le biais d'une bascule ne sont pas rendus. Les éléments initialement visibles et les éléments masqués qui peuvent être rendus visibles par le biais d'une bascule sont rendus.  
  
-   **Images, lines, and custom report items** sont ignorés.  
  
  
##  <a name="DataTypes"></a> Types de données  
 L'élément zone de texte ou attribut se voit affecter un type de données XSD sur la base des valeurs affichées par la zone de texte.  
  
|Si toutes les valeurs de zone de texte sont|Le type de données affecté est|  
|--------------------------------|---------------------------|  
|**Int16**, **Int32**, **Int64**, **UInt16**, **UInt32**, **UInt64**, **Byte**, **SByte**|**xsd:integer**|  
|**Decimal** (ou **Decimal** et tout type de données entier ou octet)|**xsd:decimal**|  
|**Float** (ou **Decimal** et tout type de données entier ou octet)|**xsd:float**|  
|**Double** (ou **Decimal** et tout type de données entier ou octet)|**xsd:double**|  
|**DateTime ou DateTime Offset**|**xsd:dateTime**|  
|**Time**|**xsd:string**|  
|**Booléen**|**xsd:boolean**|  
|**String**, **Char**|**xsd:string**|  
|Autres|**xsd:string**|  
  
  
##  <a name="XMLSpecificRenderingRules"></a> Règles de rendu spécifiques à XML  
 Les sections suivantes décrivent comment les extensions de rendu XML interprètent les éléments dans le rapport.  
  
### <a name="report-body"></a>Corps du rapport  
 Un rapport est rendu sous la forme de l'élément racine du document XML. Le nom de l’élément provient de la propriété DataElementName définie dans le volet Propriétés.  
  
 Les définitions d'espaces de noms XML et les attributs de référence de schéma sont également inclus dans l'élément de rapport. Les variables sont notées en gras :  
  
 <**Report** xmlns="**SchemaName**" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:**schemaLocation**="**SchemaNameReportURL**&amp;rc%3aSchema=true" Name="ReportName">  
  
 Les valeurs des variables sont les suivantes :  
  
|Nom   |Valeur|  
|----------|-----------|  
|Rapport|Report.DataElementName|  
|ReportURL|URL absolue URLEncoded d'accès au rapport sur le serveur.|  
|SchemaName|Report.SchemaName. Si Null, alors Report.Name. Si Report.Name est utilisé, il est d'abord encodé avec XmlConvert.EncodeLocalName.|  
|ReportName|Nom du rapport.|  
  
### <a name="text-boxes"></a>Zones de texte  
 Les zones de texte sont rendues sous la forme d’éléments ou d’attributs en fonction de la propriété RDL DataElementStyle. Le nom de l’élément ou de l’attribut provient de la propriété RDL TextBox.DataElementName.  
  
### <a name="charts-data-bars-and-sparklines"></a>Graphiques, barres de données et graphiques sparkline  
 Les graphiques, les barres de données et les graphiques sparkline sont rendus au format XML. Les données sont structurées.  
  
### <a name="gauges-and-indicators"></a>Jauges et indicateurs  
 Les jauges et les indicateurs sont rendus au format XML. Les données sont structurées.  
  
### <a name="subreports"></a>Sous-rapports  
 Un sous-rapport est rendu sous la forme d'un élément. Le nom de l’élément provient de la propriété RDL DataElementName. Le paramètre de propriété TextBoxesAsElements du rapport remplace celui du sous-rapport. Les attributs d'espace de noms et XSLT ne sont pas ajoutés à l'élément de sous-rapport.  
  
### <a name="rectangles"></a>Rectangles  
 Un rectangle est rendu sous la forme d'un élément. Le nom de l’élément provient de la propriété RDL DataElementName.  
  
### <a name="custom-report-items"></a>Éléments de rapport personnalisés  
 Les éléments de rapport personnalisés, CustomReportItems (CRI), ne sont pas visibles à l’extension de rendu. Si le rapport contient un élément de rapport personnalisé, l'extension de rendu le rend sous la forme d'un élément de rapport classique.  
  
### <a name="images"></a>Images  
 Les images ne sont pas rendues.  
  
### <a name="lines"></a>Lignes  
 Les lignes ne sont pas rendues.  
  
  
### <a name="tables-matrices-and-lists"></a>Tables, matrices et listes  
 Les tables, matrices et listes sont rendues sous la forme d'un élément. Le nom de l’élément provient de la propriété RDL DataElementName de tableau matriciel.  
  
#### <a name="rows-and-columns"></a>Lignes et colonnes  
 Les colonnes sont rendues dans des lignes.  
  
#### <a name="tablix-corner"></a>Angle de tableau matriciel  
 L'angle n'est pas rendu. Seul le contenu de l'angle est rendu.  
  
#### <a name="tablix-cells"></a>Cellules de tableau matriciel  
 Les cellules de tableau matriciel sont rendues sous la forme d'éléments. Le nom de l’élément provient de la propriété RDL DataElementName de la cellule.  
  
#### <a name="automatic-subtotals"></a>Sous-totaux automatiques  
 Les sous-totaux automatiques de tableau matriciel ne sont pas rendus.  
  
#### <a name="row-and-column-items-that-do-not-repeat-with-a-group"></a>Éléments de ligne et de colonne qui ne se répètent pas avec un groupe  
 Les éléments qui ne se répètent pas avec un groupe, comme les étiquettes, les sous-totaux et les totaux sont rendus sous la forme d'éléments. Le nom de l’élément provient de la propriété RDL TablixMember.DataElementName.  
  
 La propriété TablixMember.DataElementOutput contrôle si un élément non répétitif est rendu.  
  
 Si la propriété DataElementName du membre de tableau matriciel n’est pas fournie, un nom est généré de façon dynamique pour l’élément non répétitif, sous la forme suivante :  
  
 RowX   Pour les lignes non répétitives, X étant un index de ligne de base zéro dans le parent actuel.  
  
 ColumnY   Pour les colonnes non répétitives, Y étant un index de colonne de base zéro dans le parent actuel.  
  
 Un en-tête non répétitif est rendu sous la forme d'un enfant de la ligne ou la colonne qui ne se répète pas avec un groupe.  
  
 Si un membre non répétitif n'a aucune cellule de tableau matriciel correspondante, il n'est pas rendu. Cela peut se produire dans le cas d'une cellule de tableau matriciel où il s'étend sur plusieurs colonnes.  
  
#### <a name="rows-and-columns-that-repeat-with-a-group"></a>Lignes et colonnes qui se répètent avec un groupe  
 Les lignes et colonnes qui se répètent dans un groupe sont rendues en fonctions des règles Tablix.DataElementOutput. Le nom de l’élément provient de la propriété DataElementName.  
  
 Chaque valeur unique dans un groupe est rendue sous la forme d'un élément enfant du groupe. Le nom de l’élément provient de la propriété Group.DataElementName.  
  
 Si la valeur de la propriété DataElementOutput est égale à Output, l’en-tête d’un élément répétitif est rendu sous la forme d’un enfant de l’élément de détail.  
  
  
##  <a name="CustomFormatsXSLTransformations"></a> Formats personnalisés et XSLT  
 Les fichiers XML générés par l'extension de rendu XML peuvent être convertis dans n'importe quel format à l'aide de XSL Transformations (XSLT). Cette fonctionnalité permet de générer des données dans des formats qui ne sont pas déjà pris en charge par des extensions de rendu existantes. Pensez à utiliser l'extension de rendu XML et XSLT avant de tenter de créer votre propre extension de rendu.  
  
  
##  <a name="DuplicateName"></a> Noms en double  
 S'il existe des noms d'élément de données en double dans la même étendue, le convertisseur affiche un message d'erreur.  
  
  
##  <a name="XSLTTransformations"></a> Transformations XSLT  
 Le convertisseur XML peut appliquer une transformation XSLT côté serveur aux données XML d'origine. Lorsqu'une transformation XSLT est appliquée, le convertisseur génère en sortie le contenu transformé au lieu des données XML d'origine. La transformation a lieu sur le serveur, et non sur le client.  
  
 La transformation XSLT à appliquer à la sortie est définie dans le fichier de définition de rapport avec la propriété DataTransform du rapport ou avec le paramètre XSLT *DeviceInfo* . Si l'une de ces valeurs est définie, la transformation se produit chaque fois que le convertisseur XML est utilisé. Lors de l’utilisation d’abonnements, la transformation XSLT doit être définie dans la propriété RDL DataTransform.  
  
 Si un fichier XSLT est spécifié, à la fois par la propriété de définition DataTransform et par le paramètre d’informations de périphérique, la transformation XSLT spécifiée dans DataTransform a lieu en premier, suivie de celle définie par les paramètres d’informations de périphérique.  
  
  
###  <a name="DeviceInfo"></a> Paramètres d'informations de périphérique  
 Vous pouvez modifier certains paramètres par défaut de ce convertisseur en modifiant les paramètres d'informations de périphérique, y compris ceux ci-dessous :  
  
-   une transformation (XSLT) à appliquer à la sortie XML ;  
  
-   le type MIME du document XML ;  
  
-   s'il faut appliquer des chaînes de format aux données ;  
  
-   s'il faut appliquer un retrait à la sortie XML ;  
  
-   s'il faut inclure le nom de schéma XML ;  
  
-   l'encodage du document XML ;  
  
-   l'extension de fichier du document XML.  
  
 Pour plus d'informations, consultez [XML Device Information Settings](../../reporting-services/xml-device-information-settings.md).  
  
  
## <a name="see-also"></a> Voir aussi  
 [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Fonctionnalités interactives des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Rendu des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
