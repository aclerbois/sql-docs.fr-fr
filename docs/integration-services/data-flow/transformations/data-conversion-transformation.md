---
title: Conversion de données, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
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
- sql13.dts.designer.dataconversiontrans.f1
- sql13.dts.designer.dataconversiontransformation.f1
helpviewer_keywords:
- converting data types [Integration Services]
- Data Conversion transformation
- data types [Integration Services], converting
ms.assetid: fd515bbc-6f49-4d0c-ae7f-6ea3c3f24a1c
caps.latest.revision: 53
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5becf5500ee84d30be8ead55cd24b0be0c2b44a2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-conversion-transformation"></a>transformation de conversion de données
  La transformation de conversion de données convertit les données d'une colonne d'entrée en un type de données différent, puis les copie dans une nouvelle colonne de sortie. Par exemple, un package peut extraire des données de plusieurs sources, puis utiliser cette transformation pour convertir des colonnes vers le type de données requis par la banque de données de destination. Vous pouvez appliquer plusieurs conversions à une même colonne d'entrée.  
  
 Cette transformation permet à un package de réaliser les types de conversions de données suivants :  
  
-   Modifier le type de données. Pour plus d’informations, consultez [Types de données Integration Services](../../../integration-services/data-flow/integration-services-data-types.md).  
  
    > [!NOTE]  
    >  Si vous convertissez des données en un type de données date ou datetime, la date de la colonne de sortie est exprimée dans le format ISO, bien que la préférence des paramètres régionaux puisse spécifier un format différent.  
  
-   Définir la longueur de colonne des données de chaîne ainsi que la précision et l'échelle des données numériques. Pour plus d’informations, consultez [Précision, échelle et longueur &#40;Transact-SQL&#41;](../../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
-   Spécifier une page de codes. Pour plus d'informations, voir [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md).  
  
    > [!NOTE]  
    >  Lors d'une copie entre deux colonnes de type de données chaîne, celles-ci doivent utiliser la même page de codes.  
  
 Si une colonne de sortie de données de type chaîne est plus courte que la colonne d'entrée correspondante, les données de sortie sont tronquées. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../../integration-services/data-flow/error-handling-in-data.md).  
  
 Cette transformation a une entrée, une sortie et une sortie d'erreur.  
  
## <a name="related-tasks"></a>Related Tasks  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programme. Pour plus d’informations sur l’utilisation de la transformation de conversion de données dans le Concepteur SSIS, consultez [Convertir des données en un type différent à l’aide de la transformation de conversion de données](../../../integration-services/data-flow/transformations/convert-data-type-by-using-data-conversion-transformation.md). Pour plus d’informations sur la définition des propriétés de cette transformation par programmation, consultez [Propriétés courantes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796) et [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
## <a name="related-content"></a>Contenu associé  
 Entrée de blog, [Performance Comparison between Data Type Conversion Techniques in SSIS 2008](http://go.microsoft.com/fwlink/?LinkId=220823), sur blogs.msdn.com.  
  
## <a name="data-conversion-transformation-editor"></a>Éditeur de transformation de conversion de données
  Utilisez la boîte de dialogue **Éditeur de transformation de conversion de données** pour sélectionner les colonnes à convertir, sélectionner le type de données de conversion des colonnes et définir les attributs de conversion.  
  
> [!NOTE]  
>  La propriété **FastParse** des colonnes de sortie de la transformation de conversion de données n'est pas disponible dans l' **Éditeur de transformation de conversion de données**, mais elle peut être définie à l'aide de l' **Éditeur avancé**. Pour plus d'informations sur cette propriété, consultez la section Transformation de conversion de données dans [Transformation Custom Properties](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Sélectionnez les colonnes à convertir en activant les cases à cocher. Les sélections ajoutent des colonnes d'entrée à la table ci-dessous.  
  
 **Colonne d'entrée**  
 Sélectionnez les colonnes à convertir dans la liste des colonnes d'entrée disponibles. Vos sélections sont reflétées dans les sélections de cases à cocher ci-dessus.  
  
 **Alias de sortie**  
 Tapez un alias pour chaque nouvelle colonne. La valeur par défaut est **Copie de** suivi du nom de la colonne d'entrée. Toutefois, vous pouvez choisir n'importe quel nom descriptif unique.  
  
 **Type de données**  
 Sélectionnez un type de données dans la liste. Pour plus d’informations, consultez [Types de données Integration Services](../../../integration-services/data-flow/integration-services-data-types.md).  
  
 **Longueur**  
 Définissez la longueur de colonne pour les données de chaînes.  
  
 **Précision**  
 Définissez la précision des données numériques.  
  
 **Échelle**  
 Définissez l'échelle des données numériques.  
  
 **Page de codes**  
 Sélectionnez la page de codes appropriée pour les colonnes de type DT_STR.  
  
 **Configurer l'affichage des erreurs**  
 Indiquez la façon dont les erreurs au niveau des lignes sont gérées via la boîte de dialogue [Configurer la sortie d’erreur](http://msdn.microsoft.com/library/5f8da390-fab5-44f8-b268-d8fa313ce4b9) .  
  
## <a name="see-also"></a> Voir aussi  
 [Analyse rapide](http://msdn.microsoft.com/library/6688707d-3c5b-404e-aa2f-e13092ac8d95)   
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
