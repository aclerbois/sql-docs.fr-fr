---
title: Modifier la propriété KeyColumn d’un attribut | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 278780dd8457a90a6ee2fba632216bd1697ec0f7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="attribute-properties---modify-the-keycolumn-property"></a>Propriétés d’attribut - modifier la propriété KeyColumn
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez modifier la propriété **KeyColumns** d'un attribut. Par exemple, vous souhaiterez peut-être spécifier une clé composite plutôt qu'une clé unique comme clé de l'attribut.  
  
### <a name="to-modify-the-keycolumns-property-of-an-attribute"></a>Pour modifier la propriété KeyColumns d'un attribut  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet dont vous souhaitez modifier la propriété **KeyColumns** .  
  
2.  Ouvrez le Concepteur de dimensions en suivant l'une des procédures ci-dessous :  
  
    -   Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur la dimension dans le dossier **Dimensions** , puis cliquez sur **Ouvrir** ou **Concepteur de vues**.  
  
         —ou—  
  
    -   Dans le Concepteur de Cube, sur le **Structure de Cube** onglet, développez la dimension de cube dans le **Dimensions** volet et cliquez sur **modifier \<dimension >**.  
  
3.  Sous l'onglet **Structure de dimension** , dans le volet **Attributs** , cliquez sur l'attribut dont vous souhaitez modifier la propriété **KeyColumns** .  
  
4.  Dans la fenêtre **Propriétés** , cliquez sur la valeur de la propriété **KeyColumns** .  
  
5.  Cliquez sur le bouton Parcourir **(...)** qui apparaît dans la cellule de la valeur de la zone de propriété.  
  
     La boîte de dialogue de **Colonnes clés** s'ouvre.  
  
6.  Pour supprimer une colonne clé existante, sélectionnez la colonne dans la liste **Colonnes clés** , puis cliquez sur le bouton **\<** .  
  
7.  Pour ajouter une colonne clé, sélectionnez la colonne dans la liste **Colonnes disponibles** , puis cliquez sur le bouton **>** .  
  
    > [!NOTE]  
    >  Si vous définissez plusieurs colonnes clés, l'ordre dans lequel ces colonnes apparaissent dans la liste **Colonnes clés** affecte l'ordre d'affichage. Par exemple, un attribut de mois a deux colonnes clés : mois et année. Si la colonne d'année apparaît dans la liste avant la colonne de mois, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] triera par année, puis par mois. Si la colonne de mois apparaît avant la colonne d'année, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] triera par mois, puis par année.  
  
8.  Pour modifier l'ordre des colonnes clés, sélectionnez une colonne, puis cliquez le bouton **Haut** ou **Bas** .  
  
## <a name="see-also"></a>Voir aussi  
 [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
  
  
