---
title: Colonne dérivée, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
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
- sql13.dts.designer.derivedcolumntrans.f1
- sql13.dts.designer.derivedcolumntransformation.f1
helpviewer_keywords:
- multiple derived columns
- expressions [Integration Services], derived columns
- derived columns
- columns [Integration Services], derivations
- Derived Column transformation
ms.assetid: 8eba755e-8e48-4233-bd1e-09a46bf2692f
caps.latest.revision: 60
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a3ccc6d50e3365defedffc4345838a109e653505
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="derived-column-transformation"></a>Transformation de colonne dérivée
  La transformation de colonne dérivée crée de nouvelles valeurs de colonne en appliquant des expressions aux colonnes d'entrée de transformation. Une expression peut contenir toute combinaison de variables, de fonctions, d'opérateurs et de colonnes provenant de l'entrée de transformation. Le résultat peut être ajouté en tant que nouvelle colonne ou inséré dans une colonne existante en tant que valeur de remplacement. La transformation de colonne dérivée peut définir plusieurs colonnes dérivées, et toute variable ou colonne d'entrée peut apparaître dans plusieurs expressions.  
  
 Vous pouvez utiliser cette transformation pour réaliser les tâches suivantes :  
  
-   concaténer des données à partir de différentes colonnes en une colonne dérivée. Par exemple, vous pouvez combiner les valeurs des colonnes **FirstName** et **LastName** en une colonne dérivée unique nommée **FullName**à l’aide de l’expression `FirstName + " " + LastName`.  
  
-   extraire des caractères de données de chaîne à l'aide de fonctions telles que SUBSTRING, puis stocker le résultat dans une colonne dérivée. Par exemple, vous pouvez extraire de la colonne **FirstName** l’initiale d’une personne à l’aide de l’expression `SUBSTRING(FirstName,1,1)`.  
  
-   appliquer des fonctions mathématiques à des données numériques et stocker le résultat dans une colonne dérivée. Par exemple, vous pouvez modifier la longueur et la précision d’une colonne numérique, **SalesTax**, en un nombre à deux décimales à l’aide de l’expression `ROUND(SalesTax, 2)`.  
  
-   créer des expressions qui comparent les colonnes d'entrée et les variables. Par exemple, vous pouvez comparer la variable **Version** aux données de la colonne **ProductVersion**et, suivant le résultat de la comparaison, utiliser la valeur de **Version** ou de **ProductVersion**, à l’aide de l’expression `ProductVersion == @Version? ProductVersion : @Version`.  
  
-   extraire des parties d'une valeur de date et d'heure. Par exemple, vous pouvez utiliser les fonctions GETDATE et DATEPART pour extraire l’année actuelle à l’aide de l’expression `DATEPART("year",GETDATE())`.  
  
-   convertir des chaînes de date dans un format spécifique en utilisant une expression.  
  
## <a name="configuration-of-the-derived-column-transformation"></a>Configuration de la transformation de colonne dérivée  
 Vous pouvez configurer la transformation de colonne dérivée comme suit :  
  
-   Indiquez une expression pour chaque colonne d'entrée ou pour chaque nouvelle colonne à modifier. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md).  
  
    > [!NOTE]  
    >  Si une expression référence une colonne d'entrée remplacée par la transformation de colonne dérivée, cette expression utilise la valeur d'origine de la colonne au lieu de la valeur dérivée.  
  
-   Si les résultats sont insérés dans de nouvelles colonnes et que le type de données est **string**, spécifiez une page de codes. Pour plus d'informations, voir [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md).  
  
 La transformation de colonne dérivée inclut la propriété personnalisée FriendlyExpression. La propriété peut être mise à jour par une expression de propriété lors du chargement du package. Pour plus d’informations, consultez [Expressions de propriété dans des packages](../../../integration-services/expressions/use-property-expressions-in-packages.md)et [Propriétés personnalisées des transformation](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
 Cette transformation a une entrée, une sortie standard et une sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d'informations sur la définition des propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Dériver les valeurs de colonnes à l’aide de la transformation de colonne dérivée](../../../integration-services/data-flow/transformations/derive-column-values-by-using-the-derived-column-transformation.md)  
  
## <a name="derived-column-transformation-editor"></a>Éditeur de transformation de colonne dérivée
  Utilisez la boîte de dialogue **Éditeur de transformation de colonne dérivée** pour créer des expressions qui remplissent de nouvelles colonnes ou des colonnes de remplacement.  
  
### <a name="options"></a>Options  
 **Variables et colonnes**  
 Pour créer une expression qui utilise une variable ou une colonne d'entrée, faites glisser la variable ou la colonne à partir de la liste des variables et colonnes disponibles vers une ligne d'une table existante dans le volet inférieur ou vers une nouvelle ligne au bas de la liste.  
  
 **Fonctions et opérateurs**  
 Pour créer une expression qui utilise une fonction ou un opérateur dans le but d'évaluer des données d'entrée ou de diriger des données de sortie, faites-les glisser à partir de la liste dans le volet inférieur.  
  
 **Nom de la colonne dérivée**  
 Fournissez un nom pour la colonne dérivée. Par défaut, il s'agit d'une liste numérotée de colonnes dérivées ; vous pouvez néanmoins choisir un nom unique et descriptif.  
  
 **Colonne dérivée**  
 Sélectionnez une colonne dérivée dans la liste. Choisissez d'ajouter la colonne dérivée comme nouvelle colonne de sortie ou de remplacer les données d'une colonne existante.  
  
 **Expression**  
 Tapez une expression ou créez-en une en faisant glisser la liste précédente des colonnes, variables et fonctions disponibles.  
  
 Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.  
  
 **Rubriques connexes :** [Expressions Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md), [Opérateurs &#40;expression SSIS&#41;](../../../integration-services/expressions/operators-ssis-expression.md) et [Fonctions &#40;expression SSIS&#41;](../../../integration-services/expressions/functions-ssis-expression.md)  
  
 **Type de données**  
 Si vous ajoutez des données à une nouvelle colonne, la boîte de dialogue **Éditeur de transformation de colonne dérivée** évalue automatiquement l’expression et définit correctement le type de données. La valeur de cette colonne est en lecture seule. Pour plus d’informations, consultez [Types de données Integration Services](../../../integration-services/data-flow/integration-services-data-types.md).  
  
 **Longueur**  
 Si vous ajoutez des données à une nouvelle colonne, la boîte de dialogue **Éditeur de transformation de colonne dérivée** évalue automatiquement l’expression et définit la longueur de colonne des données de chaînes. La valeur de cette colonne est en lecture seule.  
  
 **Précision**  
 Si vous ajoutez des données à une nouvelle colonne, la boîte de dialogue **Éditeur de transformation de colonne dérivée** définit automatiquement la précision des données numériques en fonction du type de données. La valeur de cette colonne est en lecture seule.  
  
 **Échelle**  
 Si vous ajoutez des données à une nouvelle colonne, la boîte de dialogue **Éditeur de transformation de colonne dérivée** définit automatiquement l’échelle des données numériques en fonction du type de données. La valeur de cette colonne est en lecture seule.  
  
 **Page de codes**  
 Si vous ajoutez des données à une nouvelle colonne, la boîte de dialogue **Éditeur de transformation de colonne dérivée** définit automatiquement la page de codes pour le type de données DT_STR. Vous pouvez mettre à jour **Page de codes**.  
  
 **Configurer l'affichage des erreurs**  
 Spécifiez comment gérer les erreurs dans la boîte de dialogue [Configurer la sortie d’erreur](http://msdn.microsoft.com/library/5f8da390-fab5-44f8-b268-d8fa313ce4b9) .  
  
## <a name="related-content"></a>Contenu associé  
 Article technique, [SSIS Expression Examples](http://go.microsoft.com/fwlink/?LinkId=220761), sur social.technet.microsoft.com  
