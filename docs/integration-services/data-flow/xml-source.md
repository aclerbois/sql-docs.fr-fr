---
title: Source XML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.xmlsource.f1
- sql13.dts.designer.xmlsourceadapter.connectionmanager.f1
- sql13.dts.designer.xmlsourceadapter.columns.f1
- sql13.dts.designer.xmlsourceadapter.erroroutput.f1
helpviewer_keywords:
- sources [Integration Services], XML
- XML source [Integration Services]
- XML Source Editor
ms.assetid: 68c27ea5-e93d-4e26-bfb2-d967ca0a5282
caps.latest.revision: 47
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 132690629b3dd9aea99b460952bd17e449ee7e68
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-source"></a>Source XML
  La source XML lit un fichier de données XML et remplit les colonnes de la sortie source avec les données.  
  
 Les données des fichiers XML sont fréquemment organisées de façon hiérarchique. Par exemple, un fichier de données XML peut représenter des catalogues et des éléments dans des catalogues. Avant que les données puissent entrer dans le flux de données, la relation des éléments du fichier de données XML doit être déterminée et une sortie doit être générée pour chaque élément du fichier.  
  
## <a name="schemas"></a>Schémas  
 La source XML utilise un schéma pour interpréter les données XML. La source XML prend en charge l'utilisation d'un fichier XSD (XML Schema Definition) ou de schémas inclus pour convertir les données XML dans un format tabulaire. Si vous configurez la source XML à l’aide de la boîte de dialogue **Éditeur de source XML** , l’interface utilisateur peut générer un fichier XSD à partir du fichier de données XML spécifié.  
  
> [!NOTE]  
>  Les DTD ne sont pas pris en charge.  
  
 Les schémas ne peuvent prendre en charge qu'un seul espace de noms ; ils ne prennent pas en charge les collections de schémas.  
  
> [!NOTE]  
>  La source XML ne valide pas les données du fichier XML par rapport au fichier XSD.  
  
## <a name="xml-source-editor"></a>Éditeur de source XML  
 Les données des fichiers XML sont fréquemment organisées de façon hiérarchique. La boîte de dialogue **Éditeur de source XML** utilise le schéma spécifié pour générer les sorties de la source XML. Vous pouvez spécifier un fichier XSD, utiliser un schéma inclus ou générer un fichier XSD à partir du fichier de données XML spécifié. Le schéma doit être disponible au moment de la conception.  
  
 La source XML génère des structures tabulaires à partir des données XML en créant une sortie pour chaque élément contenant d'autres éléments dans les fichiers XML. Par exemple, si les données XML représentent des catalogues et des éléments dans des catalogues, la source XML crée une sortie pour les catalogues et une sortie pour chaque type d'élément contenu dans ces catalogues. La sortie de chaque élément contient des colonnes de sortie pour les attributs de cet élément.  
  
 Pour fournir des informations sur les relations hiérarchiques des données dans les sorties, la source XML ajoute dans celles-ci une colonne qui identifie l'élément parent de chaque élément enfant. Dans l'exemple des catalogues contenant différents types d'éléments, à chaque élément correspond une valeur de colonne qui identifie le catalogue auquel il appartient.  
  
 La source XML crée une sortie pour chaque élément, mais vous n'êtes pas obligé d'utiliser toutes les sorties. Vous pouvez supprimer toute sortie que vous ne souhaitez pas utiliser ou simplement ne pas la connecter à un composant en aval.  
  
 La source XML génère également les noms de sortie, afin que ceux-ci ne soient pas ambigus. Ces noms peuvent être longs et ne pas identifier les sorties de façon très utilisable pour vous. Vous pouvez renommer les sorties, sous réserve que leurs noms demeurent uniques. Vous pouvez également modifier le type de données et la longueur des colonnes de sortie.  
  
 Pour chaque sortie, la source XML ajoute une sortie d'erreur. Par défaut, les colonnes situées dans les sorties d'erreur ont le type de données chaîne Unicode (DT_WSTR) et une longueur de 255 caractères ; toutefois, vous pouvez reconfigurer leur type de données et leur longueur.  
  
 Si le fichier de données XML contient des éléments qui ne figurent pas dans le fichier XSD, ces éléments sont ignorés et aucune sortie correspondante n'est générée. Par contre, s'il manque dans le fichier de données XML des éléments représentés dans le fichier XSD, la sortie contient des colonnes comportant des valeurs NULL.  
  
 Quand les données sont extraites du fichier de données XML, elles sont converties en un type de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Toutefois, la source XML ne peut pas convertir les données XML en types de données DT_TIME2 ou DT_DBTIMESTAMP2, car elle ne prend pas en charge ces types de données. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Le fichier XSD ou le schéma inclus peuvent spécifier le type de données des éléments mais, s’ils ne le font pas, la boîte de dialogue **Éditeur de source XML** affecte le type de données chaîne Unicode (DT_WSTR) à la colonne de sortie qui contient l’élément et définit pour celle-ci une longueur de 255 caractères.  
  
 Si le schéma spécifie la longueur maximale d'un élément, la longueur de la colonne de sortie prend cette valeur. Si la longueur maximale est supérieure à la longueur prise en charge par le type de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans lequel l’élément est converti, les données sont tronquées à la longueur maximale du type de données. Par exemple, si une chaîne a une longueur de 5000, elle est tronquée au 4000e caractère car la longueur maximale du type de données DT_WSTR est de 4000 caractères ; de même, les données de type octet sont tronquées au 8000e caractère, car la longueur maximale du type de données DT_BYTES est de 4000 caractères. Si le schéma ne spécifie aucune longueur maximale, la longueur par défaut des colonnes, indépendamment de leur type de données, est de 255. La troncation des données dans la source XML est gérée de la même manière que dans les autres composants de flux de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).  
  
 Vous pouvez modifier le type de données et la longueur de la colonne. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="configuration-of-the-xml-source"></a>Configuration de la source XML  
 La source XML prend en charge trois modes différents d'accès aux données. Vous pouvez spécifier l'emplacement du fichier de données XML, la variable qui contient cet emplacement ou celle qui contient les données XML.  
  
 La source XML inclut les propriétés personnalisées **XMLData** et **XMLSchemaDefinition**, qui peuvent être mises à jour par des expressions de propriété lors du chargement du package. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md), [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md) et [Propriétés personnalisées des sources XML](../../integration-services/data-flow/xml-source-custom-properties.md).  
  
 La source XML prend en charge plusieurs sorties standard et plusieurs sorties d'erreur.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] inclut la boîte de dialogue **Éditeur de source XML**pour configurer la source XML. Cette boîte de dialogue est disponible dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des sources XML](../../integration-services/data-flow/xml-source-custom-properties.md)  
  
 Pour plus d’informations sur la définition des propriétés, cliquez sur l’une des rubriques suivantes :  
  
-   [Définir les propriétés d'un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="xml-source-editor-connection-manager-page"></a>Éditeur de source XML (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de l' **Éditeur de source XML** pour spécifier un fichier XML et le schéma XSD qui transforme les données XML.  
  
### <a name="static-options"></a>Options statiques  
 **Mode d'accès aux données**  
 Spécifiez la méthode de sélection des données dans la source.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Emplacement du fichier XML|Récupère des données dans un fichier XML.|  
|Fichier XML à partir d'une variable|Spécifiez le nom de fichier XML dans une variable.<br /><br /> **Informations connexes**: [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787)|  
|Données XML à partir d'une variable|Récupère des données XML à partir d'une variable.|  
  
 **Utiliser le schéma inclus**  
 Indique si les données de la source XML contiennent le schéma XSD définissant et validant sa structure et ses données.  
  
 **Emplacement XSD**  
 Tapez le chemin et le nom de fichier du schéma XSD, ou recherchez le fichier en cliquant sur **Parcourir**.  
  
 **Parcourir**  
 Dans la boîte de dialogue **Ouvrir** , recherchez le fichier de schéma XSD.  
  
 **Créer XSD**  
 Utilisez la boîte de dialogue **Enregistrer sous** pour sélectionner l’emplacement du fichier de schéma XSD généré automatiquement. L'éditeur détermine le schéma en fonction de la structure des données XML.  
  
### <a name="data-access-mode-dynamic-options"></a>Options dynamiques du mode d'accès aux données  
  
#### <a name="data-access-mode--xml-file-location"></a>Mode d'accès aux données = emplacement du fichier XML  
 **Emplacement XML**  
 Tapez le chemin et le nom du fichier de données XML, ou recherchez le fichier en cliquant sur **Parcourir**.  
  
 **Parcourir**  
 Dans la boîte de dialogue **Ouvrir** , recherchez le fichier de données XML.  
  
#### <a name="data-access-mode--xml-file-from-variable"></a>Mode d'accès aux données = fichier XML à partir d'une variable  
 **Nom de la variable**  
 Sélectionnez la variable contenant le chemin d'accès et le nom du fichier XML.  
  
#### <a name="data-access-mode--xml-data-from-variable"></a>Mode d'accès aux données = données XML à partir d'une variable  
 **Nom de la variable**  
 Sélectionnez la variable qui contient les données XML.  
  
## <a name="xml-source-editor-columns-page"></a>Éditeur de source XML (page Colonnes)
  Le nœud **Colonnes** de la boîte de dialogue **Éditeur de source XML** vous permet de mapper une colonne de sortie à une colonne externe (source).  
  
### <a name="options"></a>Options  
 **Colonnes externes disponibles**  
 Affiche la liste des colonnes externes disponibles dans la source de données. Vous ne pouvez pas ajouter ou supprimer des colonnes à l'aide de cette table.  
  
 **Colonne externe**  
 Affiche les colonnes externes (sources) dans l'ordre de lecture de la tâche. Vous pouvez modifier cet ordre en désactivant les colonnes sélectionnées dans la table affichée dans l'éditeur, puis en sélectionnant les colonnes externes dans la liste, dans un ordre différent.  
  
 **Colonne de sortie**  
 Spécifiez un nom unique pour chaque colonne de sortie. Le nom par défaut est celui de la colonne externe (source) sélectionnée ; vous pouvez néanmoins choisir n'importe quel nom unique et significatif. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
## <a name="xml-source-editor-error-output-page"></a>Éditeur de source XML (page Sortie d'erreur)
  Utilisez la page **Sortie d’erreur** de la boîte de dialogue **Éditeur de source XML** pour sélectionner les options de gestion des erreurs et pour définir les propriétés des colonnes de sortie d’erreur.  
  
### <a name="options"></a>Options  
 **Entrée/sortie**  
 Affichez le nom de la source de données.  
  
 **Colonne**  
 Affichez les colonnes externes (sources) que vous avez sélectionnées dans la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source XML**.  
  
 **Erreur**  
 Indiquez ce qui doit se produire lorsqu'une erreur se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Rubriques connexes :** [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **Troncation**  
 Indiquez ce qui doit se produire lorsqu'une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Description**  
 Affiche la description de l'erreur.  
  
 **Définir cette valeur sur les cellules sélectionnées**  
 Indiquez ce qui doit se produire pour l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Appliquer**  
 Appliquez l'option de gestion des erreurs aux cellules sélectionnées.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Extraire des données à l’aide de la source XML](../../integration-services/data-flow/extract-data-by-using-the-xml-source.md)  
