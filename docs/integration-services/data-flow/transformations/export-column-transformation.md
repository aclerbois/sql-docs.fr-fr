---
title: Exportation de colonne, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- sql13.dts.designer.exportcolumntrans.f1
- sql13.dts.designer.fileextractortransformation.columns.f1
- sql13.dts.designer.fileextractortransformation.errorhandling.f1
helpviewer_keywords:
- exporting data
- append options [Integration Services]
- Export Column transformation [Integration Services]
- columns [Integration Services], exporting
- inserting data
- truncate options [Integration Services]
ms.assetid: 678d2dfc-e40c-4fbb-b2cc-42fffc44478a
caps.latest.revision: 45
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e51d79c8d365bb1ba5b28feec4ab19ce445e234a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="export-column-transformation"></a>Transformation d'exportation de colonne
  La transformation d'exportation de colonne lit des données dans un flux de données puis les insère dans un fichier. Par exemple, si le flux de données contient des informations sur les produits, telles qu'une image de chaque produit, vous pouvez utiliser la transformation d'exportation de colonne pour enregistrer les images dans des fichiers.  
  
## <a name="append-and-truncate-options"></a>Options d'ajout et de troncation  
 Le tableau suivant décrit l'impact des paramètres des options d'ajout et de troncation sur les résultats.  
  
|Ajouter|Tronqué|Le fichier existe|Résultats|  
|------------|--------------|-----------------|-------------|  
|False|False|non|La transformation crée un nouveau fichier et y écrit les données.|  
|True|False|non|La transformation crée un nouveau fichier et y écrit les données.|  
|False|True|non|La transformation crée un nouveau fichier et y écrit les données.|  
|True|True|non|La validation de la transformation au moment de la conception a échoué. Vous ne pouvez pas attribuer aux deux propriétés la valeur **true**.|  
|False|False|Oui|Une erreur d'exécution se produit. Le fichier existe, mais la transformation ne peut pas y écrire.|  
|False|True|Oui|La transformation supprime et recrée le fichier, puis y écrit les données.|  
|True|False|Oui|La transformation ouvre le fichier, à la fin duquel elle ajoute les données.|  
|True|True|Oui|La validation de la transformation au moment de la conception a échoué. Vous ne pouvez pas attribuer aux deux propriétés la valeur **true**.|  
  
## <a name="configuration-of-the-export-column-transformation"></a>Configuration de la transformation d'exportation de colonne  
 Vous pouvez configurer la transformation d'exportation de colonne comme suit :  
  
-   Spécifier les colonnes de données et les colonnes qui contiennent le chemin d'accès aux fichiers dans lesquels les données doivent être écrites.  
  
-   Indiquer si l'opération d'insertion de données tronque des fichiers existants ou y ajoute les données.  
  
-   Indiquer si une marque d'ordre d'octet (BOM, Byte-Order Mark) est écrite dans le fichier.  
  
    > [!NOTE]  
    >  Une marque d'ordre d'octet n'est écrite que lorsque les données ne sont pas ajoutées à un fichier existant et qu'elles sont du type de données DT_NTEXT.  
  
 La transformation utilise des paires de colonnes d'entrée : une colonne contient un nom de fichier, l'autre comporte des données. Chaque ligne du jeu de données peut spécifier un fichier différent. À mesure que la transformation traite des lignes, les données sont insérées dans le fichier spécifié. Au moment de l'exécution, la transformation crée les fichiers, s'ils n'existent pas déjà, puis y écrit les données. Les données à écrire doivent être du type de données DT_TEXT, DT_NTEXT ou DT_IMAGE. Pour plus d’informations, consultez [Types de données Integration Services](../../../integration-services/data-flow/integration-services-data-types.md).  
  
 Cette transformation a une entrée, une sortie et une sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d’informations sur la définition des propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="export-column-transformation-editor-columns-page"></a>Éditeur de transformation d'exportation de colonne (page Colonnes)
  Utilisez la page **Colonnes** de la boîte de dialogue **Éditeur de transformation d'exportation de colonne** pour spécifier les colonnes du flux de données à extraire dans des fichiers. Vous pouvez préciser si la transformation d'exportation de colonne ajoute des données à la fin d'un fichier ou écrase le fichier existant.  
  
### <a name="options"></a>Options  
 **Colonne d'extraction**  
 Permet de sélectionner à partir de la liste d'entrée des colonnes contenant des données texte ou image. Toutes les lignes doivent avoir des définitions pour les options **Colonne d'extraction** et **Colonne du chemin d'accès**.  
  
 **Colonne du chemin d'accès**  
 Permet de sélectionner à partir de la liste d'entrée des colonnes contenant les chemins d'accès aux fichiers ainsi que les noms de fichiers. Toutes les lignes doivent avoir des définitions pour les options **Colonne d'extraction** et **Colonne du chemin d'accès**.  
  
 **Autoriser l'ajout**  
 Permet de préciser si la transformation ajoute ou non des données à la fin des fichiers existants. La valeur par défaut est **false**.  
  
 **Forcer la troncation**  
 Permet de préciser si la transformation supprime le contenu des fichiers existants avant d'écrire des données. La valeur par défaut est **false**.  
  
 **Écrire la marque d'ordre d'octet**  
 Indique s'il est nécessaire d'écrire une marque d'ordre d'octet (BOM, Byte-Order Mark) dans le fichier. Une BOM est écrite uniquement quand les données sont de type **DT_NTEXT** ou DT_WSTR et qu’elles ne sont pas ajoutées à un fichier de données existant.  
  
## <a name="export-column-transformation-editor-error-output-page"></a>Éditeur de transformation d'exportation de colonne (page Sortie d'erreur)
  La page **Sortie d'erreur** de la boîte de dialogue **Éditeur de transformation d'exportation de colonne** permet de spécifier la manière dont les erreurs doivent être traitées.  
  
### <a name="options"></a>Options  
 **Entrée/sortie**  
 Affiche le nom de la sortie. Cliquez sur le nom pour développer la vue de sorte à afficher les colonnes.  
  
 **Colonne**  
 Affiche les colonnes de sortie sélectionnées à la page **Colonnes** de la boîte de dialogue **Éditeur de transformation d’exportation de colonne** .  
  
 **Erreur**  
 Indiquez ce qui doit se produire en cas d'erreur : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Troncation**  
 Indiquez ce qui doit se produire en cas de troncation : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Description**  
 Affichez la description de l'opération.  
  
 **Définir cette valeur sur les cellules sélectionnées**  
 Indiquez ce qui doit se produire pour l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Appliquer**  
 Appliquez l'option de gestion des erreurs aux cellules sélectionnées.  
  
  
