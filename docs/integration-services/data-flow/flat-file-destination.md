---
title: Destination de fichier plat | Microsoft Docs
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
- sql13.dts.designer.flatfiledest.f1
- sql13.dts.designer.flatfiledestadapter.connection.f1
- sql13.dts.designer.flatfiledestadapter.mappings.f1
helpviewer_keywords:
- flat files
- Flat File destination
- text file writing [Integration Services]
- destinations [Integration Services], Flat File
ms.assetid: e0d6e356-8db4-48aa-ba66-029397f98f61
caps.latest.revision: 49
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 73241b60f4ad826e0492372e83f83eb58a52c321
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="flat-file-destination"></a>Destination de fichier plat
  La destination de fichier plat écrit des données dans un fichier texte. Le fichier texte peut se présenter dans un format délimité, à largeur fixe, à largeur fixe avec séparateur de ligne ou en drapeau à droite.  
  
 Vous pouvez configurer la destination de fichier plat de plusieurs manières :  
  
-   Fournissez un bloc de texte qui est inséré dans le fichier avant l'écriture des données. Le texte peut fournir des informations telles que des en-têtes de colonnes.  
  
-   Spécifiez s'il faut remplacer des données dans un fichier de destination qui porte le même nom.  
  
 Cette destination utilise un gestionnaire de connexions de fichier plat pour accéder au fichier texte. En définissant les propriétés du gestionnaire de connexions de fichier plat utilisé par la destination de fichier plat, vous pouvez spécifier la façon dont elle met en forme et écrit le fichier texte. Lors de la configuration du gestionnaire de connexions de fichier plat, vous spécifiez des informations sur le fichier et sur chacune de ses colonnes. Par exemple, vous spécifiez les caractères qui délimitent les colonnes et les lignes du fichier, ainsi que le type de données et la longueur de chaque colonne. Pour plus d'informations, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 La destination de fichier plat inclut la propriété personnalisée Header. La propriété peut être mise à jour par une expression de propriété lors du chargement du package. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md), [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md) et [Propriétés personnalisées des fichiers plats](../../integration-services/data-flow/flat-file-custom-properties.md).  
  
 Cette destination possède une sortie. Elle ne prend pas en charge de sortie d'erreur.  
  
## <a name="configuration-of-the-flat-file-destination"></a>Configuration de la destination de fichier plat  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des fichiers plats](../../integration-services/data-flow/flat-file-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d’informations sur la définition des propriétés d’un composant de flux de données, consultez [Définir les propriétés d’un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="flat-file-destination-editor-connection-manager-page"></a>Éditeur de destination de fichier plat (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de destination de fichier plat** pour sélectionner la connexion de fichier plat de la destination et spécifier si elle doit remplacer le fichier de destination existant ou lui être ajoutée. La destination de fichier plat écrit ses données dans un fichier texte. Ce fichier texte peut être d'un format délimité, à largeur fixe avec séparateur de lignes, ou en drapeau à droite.  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions de fichiers plats**  
 Sélectionnez un gestionnaire de connexions existant dans la zone de liste ou créez une connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez une connexion à l’aide des boîtes de dialogue **Format de fichier plat** et **Éditeur du gestionnaire de connexions de fichiers plats** .  
  
 En plus des formats de fichier plat standard (délimité, largeur fixe et en drapeau à droite), la boîte de dialogue **Format de fichier plat** contient une quatrième option : **Largeur fixe avec séparateurs de lignes**. Cette option représente un cas particulier du format en drapeau à droite dans lequel [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ajoute une colonne factice comme dernière colonne de données. Cette colonne factice garantit que la dernière colonne a une largeur fixe.  
  
 L'option **Largeur fixe avec séparateurs de lignes** n'est pas disponible dans l' **éditeur du gestionnaire de connexions de fichiers plats**. Le cas échéant, vous pouvez émuler cette option dans l'éditeur. Pour émuler cette option, dans la page **Général** de l' **éditeur du gestionnaire de connexions de fichiers plats**, pour **Format**, sélectionnez **En drapeau à droite**. Puis, dans la page **Avancé** de l'éditeur, ajoutez une colonne factice comme dernière colonne de données.  
  
 **Remplacer les données du fichier**  
 Précisez si le fichier existant doit être remplacé ou si les données doivent lui être ajoutées.  
  
 **En-tête**  
 Tapez un bloc de texte à insérer dans le fichier avant l'écriture des données. Utilisez cette option pour inclure des informations supplémentaires, telles que des titres de colonne.  
  
 **Aperçu**  
 Affichez un aperçu des résultats à l’aide de la boîte de dialogue **Vue de données** . L'aperçu peut afficher jusqu'à 200 lignes.  
  
## <a name="flat-file-destination-editor-mappings-page"></a>Éditeur de destination de fichier plat (page Mappages)
  La page **Mappages** de la boîte de dialogue **Éditeur de destination de fichier plat** permet de mapper des colonnes d’entrée à des colonnes de destination.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Affichez la liste des colonnes d'entrée disponibles. Au moyen d'une opération glisser-déplacer, mappez les colonnes d'entrée disponibles aux colonnes de destination.  
  
 **Colonnes de destination disponibles**  
 Affichez la liste des colonnes de destination disponibles. Au moyen d'une opération glisser-déplacer, mappez les colonnes de destination disponibles aux colonnes d'entrée.  
  
 **Colonne d'entrée**  
 Affichez les colonnes d'entrée sélectionnées précédemment. Vous pouvez modifier les mappages au moyen de la liste **Colonnes d'entrée disponibles**. Sélectionnez **\<ignorer>** pour exclure la colonne de la sortie.  
  
 **Colonne de destination**  
 Affiche chaque colonne de destination disponible, qu'elle soit mappée ou non.  
  
## <a name="see-also"></a> Voir aussi  
 [Source de fichier plat](../../integration-services/data-flow/flat-file-source.md)   
 [Flux de données](../../integration-services/data-flow/data-flow.md)  
  
  
