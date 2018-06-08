---
title: Tableau croisé dynamique, transformation | Microsoft Docs
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
- sql13.dts.designer.pivottrans.f1
helpviewer_keywords:
- Pivot transformation
- normalized data [Integration Services]
- PivotUsage property
- datasets [Integration Services], normalized data
- less normalized data set [Integration Services]
ms.assetid: 55f5db6e-6777-435f-8a06-b68c129f8437
caps.latest.revision: 55
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f67b7eca9e4d3c5adbe20021784dfb82a6b96e69
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pivot-transformation"></a>transformation de tableau croisé dynamique
  La transformation de tableau croisé dynamique transforme un dataset normalisé en une version moins normalisée mais plus compacte en croisant dynamiquement les données d'entrée sur une valeur de colonne. Par exemple, un dataset **Commandes** normalisé comprenant le nom de client, le produit et la quantité achetée contient généralement plusieurs lignes pour un même client ayant acheté plusieurs produits ; chaque ligne indiquant les détails de commande d’un produit différent. En croisant dynamiquement le dataset sur la colonne de produit, la transformation de tableau croisé dynamique peut sortir un dataset contenant une seule ligne par client. Cette ligne unique indique tous les achats du client ; le nom des produits est indiqué sous forme de nom de colonne et la quantité sous forme de valeur de la colonne de produit. Dans la mesure où tous les clients n'achètent pas chacun des produits, de nombreuses colonnes peuvent contenir des valeurs null.  
  
 Lorsqu'un dataset est croisé dynamiquement, les colonnes d'entrée jouent des rôles différents dans le processus de croisement dynamique. Une colonne peut jouer les rôles suivants :  
  
-   La colonne est transmise à la sortie sans subir aucune modification. De nombreuses lignes d'entrées pouvant se traduire par une seule ligne de sortie, la transformation copie uniquement la première valeur d'entrée de la colonne.  
  
-   La colonne joue le rôle de clé ou de partie de la clé qui identifie un ensemble d'enregistrements.  
  
-   La colonne définit le tableau croisé dynamique. Les valeurs de cette colonne sont associées aux colonnes du dataset croisé dynamiquement.  
  
-   La colonne contient des valeurs qui sont placées dans les colonnes créées par le tableau croisé dynamique.  
  
 Cette transformation a une entrée, une sortie standard et une sortie d'erreur.  
  
## <a name="sort-and-duplicate-rows"></a>Trier et dupliquer les lignes  
 Pour une plus grande efficacité du croisement dynamique des données, autrement dit pour créer aussi peu d'enregistrements dans le dataset de sortie que possible, les données d'entrée doivent être triées sur la colonne tableau croisé dynamique. Si les données ne sont pas triées, la transformation de tableau croisé dynamique risque de générer plusieurs enregistrements pour chaque valeur de la clé d'ensemble, qui est la colonne définissant l'appartenance à l'ensemble. Par exemple, si un dataset est croisé dynamiquement sur une colonne **Nom** , mais que les noms ne sont pas triés, le dataset de sortie risque de contenir plus d’une ligne pour chaque client car un croisement dynamique est effectué chaque fois que la valeur de la colonne **Nom** change.  
  
 Les données d'entrée peuvent contenir des lignes en double, ce qui entraîne l'échec de la transformation de tableau croisé dynamique. L'expression « lignes en double » désigne des lignes qui ont les mêmes valeurs dans les colonnes clés d'ensemble et dans les colonnes tableau croisé dynamique. Pour éviter cet échec, vous pouvez soit configurer la transformation de manière à rediriger les lignes d'erreur vers une sortie d'erreur, soit préagréger les valeurs de manière à ce qu'il n'y ait pas de lignes en double.  
  
##  <a name="options"></a> Options de la boîte de dialogue Tableau croisé dynamique  
 Configurez l’opération d’ajout de tableau croisé dynamique en définissant les options de la boîte de dialogue **Tableau croisé dynamique** . Pour ouvrir la boîte de dialogue **Tableau croisé dynamique** , ajoutez la transformation de tableau croisé dynamique au package dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], puis cliquez avec le bouton droit sur le composant et cliquez sur **Modifier**.  
  
 La liste suivante décrit les options de la boîte de dialogue **Tableau croisé dynamique** .  
  
 **Clé de tableau croisé dynamique**  
 Spécifie la colonne à utiliser pour les valeurs de la ligne supérieure (ligne d'en-tête) du tableau.  
  
 **Définir la clé**  
 Spécifie la colonne à utiliser pour les valeurs de la colonne gauche du tableau. La date d'entrée doit être triée sur cette colonne.  
  
 **Valeur de tableau croisé dynamique**  
 Spécifie la colonne à utiliser pour les valeurs du tableau, à l'exception des valeurs de la ligne d'en-tête et de la colonne gauche.  
  
 **Ignorer les valeurs clés de tableau croisé dynamique sans correspondance et les signaler après l'exécution de DataFlow**  
 Sélectionnez cette option pour configurer la transformation de tableau croisé dynamique afin d’ignorer les lignes contenant des valeurs non reconnues dans la colonne **Clé de tableau croisé dynamique** et de générer toutes les valeurs clés de tableau croisé dynamique dans un message de journal, lorsque le package est exécuté.  
  
 Vous pouvez également configurer la transformation pour générer les valeurs en affectant à la propriété personnalisée **PassThroughUnmatchedPivotKeys** la valeur **True**.  
  
 **Générer des colonnes de sortie de tableau croisé dynamique à partir de valeurs**  
 Entrez les valeurs clés de tableau croisé dynamique dans cette zone pour permettre à la transformation de tableau croisé dynamique de créer des colonnes de sortie pour chaque valeur. Vous pouvez soit entrer les valeurs avant d'exécuter le package, soit procéder comme suit.  
  
1.  Sélectionnez l’option **Ignorer les valeurs clés de tableau croisé dynamique sans correspondance et les signaler après l’exécution de DataFlow** , puis cliquez sur **OK** dans la boîte de dialogue **Tableau croisé dynamique** pour enregistrer les modifications apportées à la transformation de tableau croisé dynamique.  
  
2.  Exécutez le package.  
  
3.  Une fois l’exécution du package réussie, cliquez sur l’onglet **Progression** et recherchez le message de journal des informations à partir de la transformation de tableau croisé dynamique qui contient les valeurs clés de tableau croisé dynamique.  
  
4.  Cliquez avec le bouton droit sur le message, puis cliquez sur **Copier le texte du message**.  
  
5.  Cliquez sur **Arrêter le débogage** dans le menu **Débogage** pour basculer en mode design.  
  
6.  Cliquez avec le bouton droit sur la transformation de tableau croisé dynamique, puis cliquez sur **Modifier**.  
  
7.  Désactivez l’option **Ignorer les valeurs clés de tableau croisé dynamique sans correspondance et les signaler après l’exécution de DataFlow** , puis collez les valeurs clés de tableau croisé dynamique dans la zone **Générer des colonnes de sortie de tableau croisé dynamique à partir de valeurs** en utilisant le format suivant.  
  
     [valeur1],[valeur2],[valeur3]  
  
 **Générer des colonnes maintenant**  
 Cliquez pour créer une colonne de sortie pour chaque valeur clé de tableau croisé dynamique listée dans la zone **Générer des colonnes de sortie de tableau croisé dynamique à partir de valeurs** .  
  
 Les colonnes de sortie apparaissent dans la zone **Colonnes de sorties croisées dynamiquement existantes** .  
  
 **Colonnes de sorties croisées dynamiquement existantes**  
 Liste les colonnes de sortie des valeurs clés de tableau croisé dynamique  
  
 Le tableau suivant illustre un jeu de données avant que les données n’aient été croisées dynamiquement dans la colonne **Year** .  
  
|Year|Nom du produit|Total|  
|----------|------------------|-----------|  
|2004|HL Mountain Tire|1504884.15|  
|2003|Road Tire Tube|35920.50|  
|2004|Water Bottle – 30 oz.|2805.00|  
|2002|Touring Tire|62364.225|  
  
 Le tableau suivant illustre un jeu de données après que les données ont été croisées dynamiquement dans la colonne **Year** .  
  
||2002|2003|2004|  
|-|----------|----------|----------|  
|HL Mountain Tire|141164.10|446297.775|1504884.15|  
|Road Tire Tube|3592.05|35920.50|89801.25|  
|Water Bottle – 30 oz.|*NULL*|*NULL*|2805.00|  
|Touring Tire|62364.225|375051.60|1041810.00|  
  
 Pour croiser dynamiquement les données de la colonne **Year** , comme indiqué ci-dessus, vous devez définir les options suivantes dans la boîte de dialogue **Tableau croisé dynamique** .  
  
-   Year est sélectionné dans la zone de liste **Clé de tableau croisé dynamique** .  
  
-   Product Name est sélectionné dans la zone de liste **Définir la clé** .  
  
-   Total est sélectionné dans la zone de liste **Valeur de tableau croisé dynamique** .  
  
-   Les valeurs suivantes sont entrées dans la zone **Générer des colonnes de sortie de tableau croisé dynamique à partir de valeurs** .  
  
     [2002],[2003],[2004]  
  
## <a name="configuration-of-the-pivot-transformation"></a>Configuration de la transformation Pivot  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** , cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
## <a name="related-content"></a>Contenu associé  
 Pour plus d’informations sur la définition des propriétés de ce composant, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Transformation Unpivot](../../../integration-services/data-flow/transformations/unpivot-transformation.md)   
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
