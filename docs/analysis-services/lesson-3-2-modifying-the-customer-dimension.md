---
title: Modification de la Dimension Customer | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d9f922dfcc073ce72834515b42691bef2e3c90d3
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-3-2---modifying-the-customer-dimension"></a>Leçon 3-2 : modification de la Dimension Customer
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

Il existe différents moyens de rendre les dimensions d'un cube plus conviviales et plus fonctionnelles. Dans les tâches dans cette rubrique, vous modifiez la dimension Customer.  
  
## <a name="renaming-attributes"></a>Affectation d'un nouveau nom aux attributs  
Vous pouvez modifier les noms d’attributs avec l’onglet **Structure de dimension** du Concepteur de dimensions.  
  
#### <a name="to-rename-an-attribute"></a>Pour renommer un attribut  
  
1.  Basculez vers le **Concepteur de dimensions** pour la dimension Customer dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. Pour cela, double-cliquez sur la dimension **Customer** du nœud **Dimensions** de l’Explorateur de solutions.  
  
2.  Dans le volet **Attributs** , cliquez avec le bouton droit sur **English Country Region Name**et cliquez sur **Renommer**. Remplacez le nom de l’attribut par **Country-Region**.  
  
3.  Modifiez les noms des attributs suivants de la même manière :  
  
    -   Remplacez l’attribut**English Education** par l’attribut **Education**.  
  
    -   Remplacez l’attribut**English Occupation** par l’attribut **Occupation**.  
  
    -   Remplacez l’attribut**State Province Name** par l’attribut **État-Province**.  
  
4.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
## <a name="creating-a-hierarchy"></a>Création d'une hiérarchie  
Vous pouvez créer une hiérarchie en faisant glisser un attribut du volet **Attributes** vers le volet **Hiérarchies** .  
  
#### <a name="to-create-a-hierarchy"></a>Pour créer une hiérarchie  
  
1.  Faites glisser l’attribut **Pays-Région** du volet **Attributs** dans le volet **Hiérarchies** .  
  
2.  Faites glisser l’attribut **State-Province** du volet **Attributs** vers la cellule **<new level>** dans le volet **Hiérarchies** , sous le niveau **Country-Region** .  
  
3.  Faites glisser l’attribut **City** du volet **Attributs** vers la cellule **<new level>** dans le volet **Hiérarchies** , sous le niveau **State-Province** .  
  
4.  Dans le volet **Hiérarchies** de l’onglet **Structure de dimension** , cliquez avec le bouton droit sur la barre de titre de la hiérarchie **Hiérarchie** , sélectionnez **Renommer**et tapez **Customer Geography**.  
  
    Le nom de la hiérarchie est maintenant **Customer Geography**.  
  
5.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
## <a name="adding-a-named-calculation"></a>Ajout d'un calcul nommé  
Vous pouvez ajouter un calcul nommé, c'est-à-dire une expression SQL qui est représentée sous la forme d'une colonne calculée, dans la table d'une vue de source de données. L'expression apparaît et se comporte comme une colonne dans une table. Les calculs nommés permettent d'étendre le schéma relationnel des tables existantes dans une vue de source de données, sans avoir à modifier la table dans la source de données sous-jacente. Pour plus d’informations, consultez [Définir des calculs nommés dans une vue de source de données &#40;Analysis Services&#41;](../analysis-services/multidimensional-models/define-named-calculations-in-a-data-source-view-analysis-services.md)  
  
#### <a name="to-add-a-named-calculation"></a>Pour ajouter un calcul nommé  
  
1.  Ouvrez la vue de source de données **[!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] DW 2012** en double-cliquant dessus dans le dossier **Vues des sources de données** de l’Explorateur de solutions.  
  
2.  Dans le volet **Tables** à gauche, cliquez avec le bouton droit sur **Customer**, puis cliquez sur **Nouveau calcul nommé**.  
  
3.  Dans la boîte de dialogue **Créer un calcul nommé** , tapez **FullName** dans la zone **Nom de la colonne** , puis tapez ou copiez-collez l’instruction **CASE** suivante dans la zone **Expression** :  
  
    ```  
    CASE  
       WHEN MiddleName IS NULL THEN  
       FirstName + ' ' + LastName  
       ELSE  
       FirstName + ' ' + MiddleName + ' ' + LastName  
    END  
    ```  
  
    L’instruction **CASE** concatène les colonnes **FirstName**, **MiddleName**et **LastName** en une seule colonne, que vous allez utiliser dans la dimension Customer comme nom de l’attribut **Customer** .  
  
4.  Cliquez sur **OK**, puis développez **Customer** dans le volet **Tables** .  
  
    Le calcul nommé **FullName** apparaît dans la liste des colonnes de la table Customer, accompagné d’une icône indiquant qu’il s’agit d’un calcul nommé.  
  
5.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
6.  Dans le volet **Tables** , cliquez avec le bouton droit sur **Customer**, puis cliquez sur **Explorer les données**.  
  
7.  Vérifiez la dernière colonne de la vue **Explorer la table Customer** .  
  
    Notez que la colonne **FullName** apparaît dans la vue de source de données et que les données sont correctement concaténées à partir de plusieurs colonnes de la source de données sous-jacente, sans que la source de données d’origine ait été modifiée.  
  
8.  Fermez l'onglet **Explorer la table Customer** .  
  
## <a name="using-the-named-calculation-for-member-names"></a>Utilisation du calcul nommé pour les noms des membres  
Après avoir créé un calcul nommé dans la vue de la source de données, vous pouvez utiliser le calcul nommé comme propriété d'un attribut.  
  
#### <a name="to-use-the-named-calculation-for-member-names"></a>Pour utiliser le calcul nommé pour les noms des membres  
  
1.  Affichez le Concepteur de dimensions pour la dimension Customer.  
  
2.  Dans le volet **Attributs** de l’onglet **Structure de dimension** , cliquez sur l’attribut **Customer Key** .  
  
3.  Ouvrez la fenêtre Propriétés et cliquez sur le bouton **Masquer automatiquement** de la barre de titre pour qu’elle reste ouverte.  
  
4.  Dans le champ de propriété **Name** , tapez **Nom complet**.  
  
5.  Cliquez dans le champ de propriété **NameColumn** en bas, puis cliquez sur le bouton de navigation (**...**) pour ouvrir la boîte de dialogue **Colonne de nom** .  
  
6.  Sélectionnez **SimpleDate** dans la liste **Colonne source** , puis cliquez sur **OK**.  
  
7.  Sous l’onglet Structure de dimension, faites glisser l’attribut **Nom complet** du volet **Attributs** vers la cellule **<new level>** dans le volet **Hiérarchies** , sous le niveau **City** .  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
## <a name="defining-display-folders"></a>Définition de dossiers d'affichage  
Vous pouvez utiliser les dossiers d'affichage pour grouper les hiérarchies d'utilisateur et d'attributs dans des arborescences afin d'augmenter la convivialité.  
  
#### <a name="to-define-display-folders"></a>Pour définir les dossiers d'affichage  
  
1.  Ouvrez l’onglet **Structure de dimension** pour la dimension Customer.  
  
2.  Dans le volet **Attributs** , sélectionnez les attributs suivants en maintenant la touche Ctrl enfoncée pendant que vous cliquez sur chacun d’eux :  
  
    -   **City**  
  
    -   **Country-Region**  
  
    -   **Postal Code**  
  
    -   **État-Province**  
  
3.  Dans la fenêtre Propriétés, cliquez sur le champ de propriété **AttributeHierarchyDisplayFolder** en haut (vous devrez peut-être pointer dessus pour afficher le nom complet), puis tapez **Location**.  
  
4.  Dans le volet **Hiérarchies** , sélectionnez **Customer Geography**, puis sélectionnez **Location** comme valeur pour la propriété **DisplayFolder** dans la fenêtre Propriétés.  
  
5.  Dans le volet **Attributs** , sélectionnez les attributs suivants en maintenant la touche Ctrl enfoncée pendant que vous cliquez sur chacun d’eux :  
  
    -   **Commute Distance**  
  
    -   **Education**  
  
    -   **Gender**  
  
    -   **House Owner Flag**  
  
    -   **Marital Status**  
  
    -   **Number Cars Owned**  
  
    -   **Number Children At Home**  
  
    -   **Occupation**  
  
    -   **Total Children**  
  
    -   **Yearly Income**  
  
6.  Dans la fenêtre des propriétés, cliquez sur le champ de propriété **AttributeHierarchyDisplayFolder** en haut et tapez **Demographic**.  
  
7.  Dans le volet **Attributs** , sélectionnez les attributs suivants en maintenant la touche Ctrl enfoncée pendant que vous cliquez sur chacun d’eux :  
  
    -   **Email Address**  
  
    -   **Phone**  
  
8.  Dans la fenêtre des propriétés, cliquez sur le champ de propriété **AttributeHierarchyDisplayFolder** et tapez **Contacts**.  
  
9. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
## <a name="defining-composite-keycolumns"></a>Définition de KeyColumns composite  
La propriété **KeyColumns** contient la colonne ou les colonnes qui représentent la clé pour l’attribut. Dans cette leçon, vous créez une clé composite pour les attributs **City** et **State-Province** . Les clés composites peuvent être utiles lorsque vous devez identifier un attribut de façon unique. Par exemple, quand vous définirez ultérieurement des relations d’attributs dans ce didacticiel, un attribut **City** devra identifier un attribut **State-Province** de façon unique. Toutefois, il peut y avoir plusieurs villes avec le même nom dans différents états. Pour cette raison, vous allez créer une clé composite composée des colonnes **StateProvinceName** et **City** pour l’attribut **City** . Pour plus d’informations, consultez [Modifier la propriété KeyColumn d’un attribut](../analysis-services/multidimensional-models/attribute-properties-modify-the-keycolumn-property.md).  
  
#### <a name="to-define-composite-keycolumns-for-the-city-attribute"></a>Pour définir KeyColumns composite pour l'attribut Ville  
  
1.  Ouvrez l’onglet **Structure de dimension** pour la dimension Customer.  
  
2.  Dans le volet **Attributs** , cliquez sur l’attribut **City** .  
  
3.  Dans la fenêtre **Propriétés** , cliquez dans le champ **KeyColumns** en bas, puis cliquez sur le bouton de navigation (**...**).  
  
4.  Dans la boîte de dialogue **Colonnes clés** , dans la liste **Colonnes disponibles** , sélectionnez la colonne **StateProvinceName**, puis cliquez sur le bouton **>** .  
  
    Les colonnes **City** et **StateProvinceName** s’affichent maintenant dans la liste **Colonnes clés** .  
  
5.  Cliquez sur **OK**.  
  
6.  Pour définir la propriété **NameColumn** de l’attribut **City** , cliquez dans le champ **NameColumn** de la fenêtre Propriétés, puis cliquez sur le bouton de navigation (**...**).  
  
7.  Dans la boîte de dialogue **Colonne de nom** , dans la liste **Colonne source** , sélectionnez **City**, puis cliquez sur **OK**.  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
#### <a name="to-define-composite-keycolumns-for-the-state-province-attribute"></a>Pour définir KeyColumns composite pour l'attribut État-Province  
  
1.  Vérifiez que l’onglet **Structure de dimension** de la dimension Customer est ouvert.  
  
2.  Dans le volet **Attributs** , cliquez sur l’attribut **State-Province** .  
  
3.  Dans la fenêtre **Propriétés** , cliquez dans le champ **KeyColumns** , puis cliquez sur le bouton Parcourir (**...**).  
  
4.  Dans la boîte de dialogue **Colonnes clés** , dans la liste **Colonnes disponibles** , sélectionnez la colonne **EnglishCountryRegionName**, puis cliquez sur le bouton **>** .  
  
    Les colonnes **EnglishCountryRegionName** et **StateProvinceName** s’affichent maintenant dans la liste **Colonnes clés** .  
  
5.  Cliquez sur **OK**.  
  
6.  Pour définir la propriété **NameColumn** de l’attribut **State-Province** , cliquez sur le champ **NameColumn** dans la fenêtre Propriétés, puis cliquez sur le bouton de navigation (**...**).  
  
7.  Dans la boîte de dialogue **Colonne de nom** , dans la liste **Colonne source** , sélectionnez **StateProvinceName**, puis cliquez sur **OK**.  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
## <a name="defining-attribute-relationships"></a>Définition des relations d'attributs  
Si les données sous-jacentes le prennent en charge, il est également conseillé de définir des relations d'attributs entre les attributs. La définition de relations d'attributs accélère le traitement des dimensions, des partitions et des requêtes. Pour plus d’informations, consultez [Définir des relations d’attributs](../analysis-services/multidimensional-models/attribute-relationships-define.md) et [Relations d’attributs](../analysis-services/multidimensional-models-olap-logical-dimension-objects/attribute-relationships.md).  
  
#### <a name="to-define-attribute-relationships"></a>Pour définir les relations d'attributs  
  
1.  Dans le **Concepteur de dimensions** pour la dimension Customer, cliquez sur l’onglet **Relations d’attributs** . Cela peut prendre un certain temps.  
  
2.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **City** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
3.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **City**. Affectez la valeur **State-Province** à **Attribut associé**.  
  
4.  Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
    Le type de relation est **Rigide** car les relations entre les membres ne changeront pas au fil du temps. Par exemple, il serait exceptionnel qu'une ville fasse partie d'un autre état ou d'une autre province.  
  
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
6.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **State-Province** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
7.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **State-Province**. Affectez la valeur **Country-Region** à **Attribut associé**.  
  
8.  Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
9. Cliquez sur **OK**.  
  
10. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
## <a name="deploying-changes-processing-the-objects-and-viewing-the-changes"></a>Déploiement des modifications, traitement des objets et affichage des modifications  
Une fois les attributs et les hiérarchies modifiés, vous devez déployer les modifications et retraiter les objets associés avant de pouvoir visualiser ces modifications.  
  
#### <a name="to-deploy-the-changes-process-the-objects-and-view-the-changes"></a>Pour déployer les modifications, traiter les objets et visualiser les modifications  
  
1.  Dans le menu **Générer** de [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], cliquez sur **Déployer Analysis Services Tutorial**.  
  
2.  Après avoir reçu le message **Le déploiement est terminé** , cliquez sur l’onglet **Navigateur** du Concepteur de dimensions pour la dimension Customer, puis cliquez sur le bouton Reconnecter, à gauche de la barre d’outils du Concepteur.  
  
3.  Vérifiez si **Customer Geography** est sélectionné dans la liste **Hiérarchie** puis, dans le volet du navigateur, développez **All**, **Australia**, **New South Wales**puis **Coffs Harbour**.  
  
    Le navigateur affiche les clients dans la ville.  
  
4.  Basculez vers le **Concepteur de cube** pour le cube [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] Tutorial. Pour cela, double-cliquez sur le cube **Analysis Services Tutorial** dans le nœud **Cubes** de **l’Explorateur de solutions**.  
  
5.  Cliquez sur l’onglet **Navigateur** , puis cliquez sur le bouton Reconnecter dans la barre d’outils du Concepteur.  
  
6.  Dans le volet **Groupe de mesures** , développez **Customer**.  
  
    Notez que sous Customer n'apparaît plus une longue liste d'attributs, mais uniquement les dossiers d'affichage et les attributs pour lesquels aucune valeur n'a été affectée pour les dossiers d'affichage.  
  
7.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Modification de la dimension Product](../analysis-services/lesson-3-3-modifying-the-product-dimension.md)  
  
## <a name="see-also"></a>Voir aussi  
[Référence des propriétés d'attribut de dimension](../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
[Supprimer un attribut d'une dimension](../analysis-services/multidimensional-models/attribute-properties-remove-an-attribute-from-a-dimension.md)  
[Renommer un attribut](../analysis-services/multidimensional-models/attribute-properties-rename-an-attribute.md)  
[Définir des calculs nommés dans une vue de source de données &#40;Analysis Services&#41;](../analysis-services/multidimensional-models/define-named-calculations-in-a-data-source-view-analysis-services.md)  
  
