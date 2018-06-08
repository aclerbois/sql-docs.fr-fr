---
title: Chemins Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
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
- sql13.dts.designer.patheditor.general.f1
- sql13.dts.designer.patheditor.metadata.f1
- sql13.dts.designer.patheditor.visualizers.f1
helpviewer_keywords:
- paths [Integration Services], about paths
- data flow [Integration Services], paths
- paths [Integration Services]
- destinations [Integration Services], paths
- sources [Integration Services], paths
ms.assetid: 6c4629a9-2ede-4011-9101-3b342249640e
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f825acb27b39a78a7997d34505e99704f0f14b27
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-paths"></a>Chemins d'accès d'Integration Services
  Un chemin d'accès connecte deux composants d'un flux de données en reliant la sortie d'un composant à l'entrée d'un autre composant. Un chemin d'accès comporte une source et une destination. Par exemple, si un chemin d'accès connecte une source OLE DB et une transformation de tri, la source OLE DB est la source du chemin d'accès, tandis que la transformation de tri est la destination du chemin d'accès. La source est le composant où débute le chemin d'accès et la destination, le composant où il se termine.  
  
 Si vous exécutez un package dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vous pouvez afficher les données d'un flux de données en attachant des visionneuses de données à un chemin d'accès. Une visionneuse de données peut être configurée pour afficher des données dans une grille. Une visionneuse de données est un outil de débogage très utile. Pour plus d’informations, consultez [Débogage d’un flux de données](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
## <a name="configure-the-path"></a>Configurer le chemin  
 Le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] contient la boîte de dialogue **Éditeur du chemin d’accès au flux de données** qui permet de définir les propriétés du chemin d’accès, d’afficher les métadonnées des colonnes de données qui empruntent le chemin d’accès et de configurer les visionneuses de données.  
  
 Le nom, la description et l'annotation du chemin d'accès sont des propriétés que vous pouvez configurer. Vous pouvez également configurer les chemins d'accès par programme. Pour plus d’informations, consultez [Connexion de composants de flux de données par programme](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md).  
  
 Une annotation de chemin d’accès affiche le nom de la source du chemin d’accès ou le nom du chemin d’accès sur l’aire de conception de l’onglet **Flux de données** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Les annotations de chemins d'accès sont similaires aux annotations ajoutées aux flux de données, aux flux de contrôles et aux gestionnaires d'événements. La seule différence est qu’une annotation de chemin d’accès est attachée à un chemin d’accès, alors que les autres annotations apparaissent sous les onglets **Flux de données**, **Flux de contrôle**et **Gestionnaires d’événements**du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Les métadonnées indiquent le nom, le type de données, la précision, l'échelle, la longueur, la page de codes et le composant source de chaque colonne dans la sortie du composant précédent. Le composant source est le composant du flux de données ayant créé la colonne. Il peut ou non s'agir du premier composant du flux de données. Par exemple, les transformations d'union totale et de tri créent leurs propres colonnes et sont les sources de leurs colonnes de sortie. À l'inverse, une transformation de copie de colonne peut transmettre des colonnes sans les modifier ou créer des colonnes en copiant les colonnes d'entrée. La transformation de copie de colonne est le composant source des nouvelles colonnes uniquement.  

## <a name="set-the-properties-of-a-path-with-the-data-flow-path-editor"></a>Définir les propriétés d’un chemin à l’aide de l’Éditeur du chemin d’accès au flux de données
Les chemins d'accès connectent deux composants de flux de données. Pour pouvoir définir les propriétés des chemins d'accès, le flux de données doit contenir au moins deux composants de flux de données connectés.
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de données** , puis double-cliquez sur un chemin.  
  
4.  Dans **Éditeur du chemin d’accès au flux de données**, cliquez sur **Général**. Vous pouvez ensuite modifier le nom par défaut du chemin d'accès et fournir une description. Vous pouvez également modifier la propriété PathAnnotation.  
  
5.  Cliquez sur **OK**.  
  
6.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

## <a name="general-page---data-flow-path-editor"></a>Page Général - Éditeur du chemin d’accès au flux de données
Utilisez la boîte de dialogue **Éditeur du chemin d'accès au flux de données** pour définir les propriétés du chemin d'accès, afficher les métadonnées de la colonne et gérer les visionneuses de données liées à ce chemin d'accès.  
  
 Utilisez le nœud **Général** de la boîte de dialogue **Éditeur du chemin d'accès au flux de données** pour nommer et décrire le chemin d'accès et pour spécifier ses options d'annotation.  
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique au chemin d'accès.  
  
 **ID**  
 Identificateur de lignage du chemin d'accès. Cette propriété est en lecture seule.  
  
 **IdentificationString**  
 Chaîne identifiant le chemin d'accès. Généré automatiquement à partir du nom entré précédemment.  
  
 **Description**  
 Décrivez le chemin d'accès.  
  
 **PathAnnotation**  
 Spécifiez le type d'annotation à utiliser. Choisissez **Never** pour désactiver les annotations, **AsNeeded** pour activer l'annotation à la demande, **SourceName** pour annoter automatiquement en utilisant la valeur de l'option **SourceName** et **PathName** pour annoter automatiquement en utilisant la valeur de la propriété **Nom** .  
  
 **DestinationName**  
 Affiche l'entrée qui représente l'issue du chemin d'accès.  
  
 **SourceName**  
 Affiche la sortie qui représente le début du chemin d'accès.  
 
## <a name="metadata-page---data-flow-path-editor"></a>Page Métadonnées - Éditeur du chemin d’accès au flux de données
Utilisez la page **Métadonnées** de la boîte de dialogue **Éditeur du chemin d'accès au flux de données** pour afficher les métadonnées des colonnes du chemin d'accès.  
  
### <a name="options"></a>Options  
 **Path metadata**  
 Affiche la liste des métadonnées des colonnes. Cliquez sur les en-têtes de colonne pour trier les données des colonnes.  
  
 **Nom**  
 Donne le nom de la colonne.  
  
 **Type de données**  
 Affiche le type de données de la colonne.  
  
 **Précision**  
 Affiche le nombre de chiffres d'une valeur numérique.  
  
 **Échelle**  
 Affiche le nombre de chiffres à droite du séparateur décimal d'une valeur numérique.  
  
 **Longueur**  
 Affiche la longueur actuelle de la colonne.  
  
 **Page de codes**  
 Affiche la page de codes de la colonne. La valeur **0** indique que la colonne d'utilise pas de page de codes. Cette situation se produit lorsque les données sont au format Unicode ou de type numérique, date ou heure.  
  
 **Position de la clé de tri**  
 Affiche la position de la clé de tri de la colonne. La valeur **0** indique que la colonne n'est pas triée.  
  
> [!NOTE]  
>  Un signe moins (-) comme préfixe indique que la colonne est triée dans l'ordre décroissant.  
  
 **Indicateurs de comparaison**  
 Affiche la liste des indicateurs de comparaison qui s'appliquent à la colonne.  
  
 **Composant source**  
 Affiche le composant du flux de données qui représente la source de la colonne.  
  
 **Copier dans le Presse-papiers**  
 Copie les données de la colonne dans le Presse-papiers. Par défaut, toutes les lignes de métadonnées sont copiées dans l'ordre de tri actuellement affiché.  
 
## <a name="data-viewers-page---data-flow-path-editor"></a>Page Visionneuses de données - Éditeur du chemin d’accès au flux de données
Utilisez la page **Visionneuses de données** de la boîte de dialogue **Éditeur du chemin d'accès au flux de données** pour gérer les visionneuses de données liées au chemin d'accès.  
  
### <a name="options"></a>Options  
 **Nom**  
 Affiche la liste des visionneuses de données.  
  
 **Type de Visionneuse de données**  
 Affiche le type de la visionneuse de données.  
  
 **Ajouter**  
 Cliquez sur ce bouton pour ajouter une visionneuse de données à l’aide de la boîte de dialogue **Configurer la Visionneuse de données** .  
  
 **Supprimer**  
 Cliquez sur cette option pour supprimer la Visionneuse de données sélectionnée.  
  
 **Configurer**  
 Cliquez sur ce bouton pour configurer la visionneuse de données sélectionnée à l’aide de la boîte de dialogue **Configurer la Visionneuse de données** .  
 
## <a name="path-properties"></a>Propriétés du chemin d’accès
Les objets de flux de données dans le modèle d’objet [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ont des propriétés communes et des propriétés personnalisées au niveau du composant, des entrées et sorties, et des colonnes d’entrée et colonnes de sortie. De nombreuses propriétés ont des valeurs en lecture seule qui sont assignées au moment de l'exécution par le moteur de flux de données.  
  
 Cette rubrique répertorie et décrit les propriétés personnalisées des chemins d'accès qui connectent des objets de flux de données.  
  
### <a name="custom-properties-of-a-path"></a>Propriétés personnalisées d’un chemin  
 Dans le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], un chemin d'accès qui connecte des composants dans le flux de données implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100>.  
  
 Le tableau suivant décrit les propriétés configurables des chemins d'accès dans un flux de données. Le moteur de flux de données assigne également des valeurs à d'autres propriétés en lecture seule qui ne sont pas répertoriées ici.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|PathAnnotation|Integer (énumération)|Valeur qui indique si une annotation doit être affichée avec le chemin d'accès sur l'aire du concepteur. Les valeurs possibles sont **AsNeeded**, **SourceName**, **PathName**et **Never**. La valeur par défaut est **AsNeeded**.|  
|DestinationName|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100>|Entrée associée au chemin d'accès.|  
|SourceName|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100>|Sortie associée au chemin d'accès.|  
