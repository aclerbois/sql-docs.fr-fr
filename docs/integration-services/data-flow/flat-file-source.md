---
title: Source de fichier plat | Microsoft Docs
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
- sql13.dts.designer.flatfilesource.f1
- sql13.dts.designer.flatfilesourceadapter.connection.f1
- sql13.dts.designer.flatfilesourceadapter.columns.f1
- sql13.dts.designer.flatfilesourceadapter.errorhandling.f1
helpviewer_keywords:
- sources [Integration Services], Flat File
- text file reading [Integration Services]
- flat files
- Flat File source
ms.assetid: 4a64f7f3-f25d-4db0-93b3-a29496030e58
caps.latest.revision: 50
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1accfd1edea180ea20ae09f1ef8fbf8d0c54ad40
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="flat-file-source"></a>Source de fichier plat
  La source de fichier plat lit des données figurant dans un fichier texte. Le fichier texte peut être d'un format délimité, à largeur fixe ou mixte.  
  
-   Le format délimité utilise des séparateurs de colonnes et de lignes pour définir les colonnes et les lignes.  
  
-   Le format de largeur fixe utilise la largeur pour définir les colonnes et les lignes. Ce format comprend également un caractère qui permet de remplir les champs dans la limite de leur largeur maximale.  
  
-   Le format non justifié à droite utilise la largeur pour définir toutes les colonnes, à l'exception de la dernière colonne, qui est délimitée par le séparateur de lignes.  
  
 Vous pouvez configurer la source de fichier plat comme suit :  
  
-   Ajoutez une colonne à la sortie de transformation qui contient le nom du fichier texte dont la source de fichier plat extrait les données.  
  
-   Spécifiez si la source de fichier plat interprète les chaînes nulles dans les colonnes en tant que valeurs NULL.  
  
    > [!NOTE]  
    >  Le gestionnaire de connexions de fichiers plats utilisé par la source de fichier plat doit être configuré de manière à utiliser un format délimité, afin qu'il puisse interpréter les chaînes de longueur nulle en tant que valeurs NULL. Si le gestionnaire de connexions utilise le format à largeur fixe ou non justifié à droite, les données composées d'espaces ne peuvent pas être interprétées comme des valeurs NULL.  
  
 Les colonnes de sortie dans la sortie de la source de fichier plat comportent la propriété FastParse. FastParse indique si la colonne utilise les routines d’analyse de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui sont rapides, mais ne tiennent pas compte des paramètres régionaux, ou les routines d’analyse standard qui tiennent compte des paramètres régionaux. Pour plus d'informations, consultez [Fast Parse](http://msdn.microsoft.com/library/6688707d-3c5b-404e-aa2f-e13092ac8d95) et [Standard Parse](http://msdn.microsoft.com/library/dfe835b1-ea52-4e18-a23a-5188c5b6f013).  
  
 Les colonnes de sortie incluent également la propriété UseBinaryFormat. Cette propriété vous permet d'implémenter la prise en charge de données binaires, par exemple des données au format décimal compressé, dans des fichiers. Par défaut, UseBinaryFormat a la valeur **false**. Si vous souhaitez utiliser un format binaire, affectez à UseBinaryFormat la valeur **true** et au type de données sur la colonne de sortie la valeur **DT_BYTES**. De cette manière, la source de fichier plat ignore la conversion des données et passe les données telles quelles à la colonne de sortie. Vous pouvez ensuite utiliser une transformation, telle que la conversion de données ou de colonne dérivée, pour convertir les données **DT_BYTES** en un type de données différent, ou vous pouvez écrire un script personnalisé dans une transformation de script pour interpréter les données. Vous pouvez également écrire un composant de flux de données personnalisé pour interpréter les données. Pour plus d’informations sur les types de données vers lesquels vous pouvez convertir **DT_BYTES**, consultez [Cast &#40;expression SSIS&#41;](../../integration-services/expressions/cast-ssis-expression.md).  
  
 Cette source utilise un gestionnaire de connexions de fichiers plats pour accéder au fichier texte. À l'aide des propriétés du gestionnaire de connexions de fichiers plats, vous pouvez indiquer des informations relatives au fichier et à chacune de ses colonnes, ainsi que définir la façon dont la source du fichier plat doit gérer les données figurant dans le fichier texte. Par exemple, vous pouvez spécifier les caractères qui séparent les colonnes et les lignes du fichier, ainsi que le type de données et la longueur de chaque colonne. Pour plus d'informations, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 Cette source comporte une sortie et une sortie d'erreur.  
  
## <a name="configuration-of-the-flat-file-source"></a>Configuration de la source de fichier plat  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des fichiers plats](../../integration-services/data-flow/flat-file-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d’informations sur la définition des propriétés d’un composant de flux de données, consultez [Définir les propriétés d’un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="flat-file-source-editor-connection-manager-page"></a>Éditeur de source de fichier plat (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source de fichier plat** pour sélectionner le gestionnaire de connexions de fichiers plats qui sera utilisé par la source de fichier plat. La source de fichier plat lit les données d'un fichier texte qui peut être au format délimité, à largeur fixe ou mixte.  
  
 Une source de fichier plat peut utiliser l'un des types de gestionnaires de connexions suivants :  
  
-   Un gestionnaire de connexions de fichiers plats si la source est un fichier plat unique. Pour plus d'informations, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
-   Un gestionnaire de connexions de fichiers plats multiples si la source se compose de fichiers plats multiples et si la tâche de flux de données se trouve dans un conteneur de boucles (conteneur de boucles For, par exemple). Dans chaque boucle du conteneur, la source de fichier plat charge les données à partir du nom de fichier suivant fourni par le gestionnaire de connexions de fichiers plats multiples. Pour plus d’informations, consultez [Gestionnaire de connexion de fichiers plats multiples](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Flat file connection manager**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou créez un gestionnaire de connexions en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez un gestionnaire de connexions à l’aide de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** .  
  
 **Conserver les valeurs NULL de la source comme valeurs NULL dans le flux de données**  
 Indique s'il faut conserver les valeurs NULL lorsque les données sont extraites. La valeur par défaut de cette propriété est **False**. Quand cette propriété a la valeur**false**, la source de fichier plat remplace les valeurs NULL des données sources par les valeurs par défaut appropriées pour chaque colonne, par exemple des chaînes vides pour les colonnes de chaînes et zéro pour les colonnes numériques.  
  
 **Aperçu**  
 Affichez un aperçu des résultats à l’aide de la boîte de dialogue **Vue de données** . L'aperçu peut afficher jusqu'à 200 lignes.  
  
## <a name="flat-file-source-editor-columns-page"></a>Éditeur de source de fichier plat (page Colonnes)
  Utilisez le nœud **Colonnes** de la boîte de dialogue **Éditeur de source de fichier plat** pour mapper une colonne de sortie à chaque colonne externe (source).  
  
> [!NOTE]  
>  La propriété **FileNameColumnName** de la source de fichier plat et la propriété **FastParse** de ses colonnes de sortie ne sont pas disponibles dans l' **Éditeur de source de fichier plat**, mais elles peuvent être définie à l'aide de l' **Éditeur avancé**. Pour plus d'informations sur ces propriétés, consultez la section sur la source de fichier plat de [Flat File Custom Properties](../../integration-services/data-flow/flat-file-custom-properties.md).  
  
### <a name="options"></a>Options  
 **Colonnes externes disponibles**  
 Affiche la liste des colonnes externes disponibles dans la source de données. Vous ne pouvez pas ajouter ou supprimer des colonnes à l'aide de cette table.  
  
 **Colonne externe**  
 Affiche les colonnes externes (sources) dans l'ordre de lecture de la tâche. Vous pouvez modifier cet ordre en supprimant d'abord les colonnes sélectionnées dans la table, puis en choisissant des colonnes externes dans la liste selon un ordre différent.  
  
 **Colonne de sortie**  
 Spécifiez un nom unique pour chaque colonne de sortie. Le nom par défaut est celui de la colonne externe (source) sélectionnée ; vous pouvez néanmoins choisir n'importe quel nom unique et significatif. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
## <a name="flat-file-source-editor-error-output-page"></a>Éditeur de source de fichier plat (page Sortie d'erreur)
  Utilisez la page **Sortie d’erreur** de la boîte de dialogue **Éditeur de source de fichier plat** pour sélectionner les options de gestion des erreurs et définir les propriétés des colonnes de sortie d’erreur.\  
  
### <a name="options"></a>Options  
 **Entrée/sortie**  
 Affichez le nom de la source de données.  
  
 **Colonne**  
 Affiche les colonnes externes (sources) que vous avez sélectionnées dans la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source de fichier plat**.  
  
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
  
## <a name="see-also"></a> Voir aussi  
 [Destination de fichier plat](../../integration-services/data-flow/flat-file-destination.md)   
 [Flux de données](../../integration-services/data-flow/data-flow.md)  
  
  
