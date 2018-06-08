---
title: 4-6-spécification des relations d’attributs dans une hiérarchie définie par l’utilisateur | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 712172f7c74e1c7efa7766ce4c25a9a8fca173f0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="4-6-specifying-attribute-relationships-in-user-defined-hierarchy"></a>4-6-spécification des relations d’attributs dans une hiérarchie définie par l’utilisateur
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

Comme vous l'avez déjà appris au cours de ce didacticiel, vous pouvez organiser les hiérarchies d'attributs en niveaux au sein des hiérarchies utilisateur pour mettre à disposition des utilisateurs d'un cube des chemins de navigation. Une hiérarchie utilisateur peut représenter une hiérarchie naturelle, telle que ville, état et pays, ou simplement un chemin de navigation tel que nom d'employé, titre et nom de division. Du point de vue de l'utilisateur qui navigue au sein d'une hiérarchie, ces deux types de hiérarchies utilisateur sont identiques.  
  
Avec une hiérarchie naturelle, si vous définissez des relations d'attributs entres les attributs qui composent les niveaux, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] peut utiliser l'agrégation d'un attribut pour obtenir les résultats à partir d'un attribut connexe. S'il n'existe aucune relation entre les attributs, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] agrège tous les attributs non clé à partir de l'attribut clé. Par conséquent, si les données sous-jacentes le prennent en charge, il est également conseillé de définir des relations d'attributs entre les attributs. La définition des relations d'attributs améliore les performances des dimensions, des partitions et du traitement des requêtes. Pour plus d’informations, consultez [Définir des relations d’attributs](../analysis-services/multidimensional-models/attribute-relationships-define.md) et [Relations d’attributs](../analysis-services/multidimensional-models-olap-logical-dimension-objects/attribute-relationships.md).  
  
Lorsque vous définissez des relations d'attributs, vous pouvez spécifier si la relation est flexible ou rigide. Si vous définissez une relation rigide, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] conserve les agrégations une fois la dimension mise à jour. Si une relation rigide change, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] génère une erreur au cours du traitement, excepté si la dimension est traitée entièrement. En spécifiant les relations et les propriétés de relations appropriées, vous améliorez les performances de requête et de traitement. Pour plus d’informations, consultez [Définir des relations d’attributs](../analysis-services/multidimensional-models/attribute-relationships-define.md)et [Propriétés de la hiérarchie définie par l’utilisateur](../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies-properties.md).  
  
Au cours des tâches de cette rubrique, vous allez définir des relations d'attributs pour les attributs des hiérarchies utilisateur naturelles dans le projet du didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . Il s’agit de la hiérarchie **Customer Geography** dans la dimension **Customer**, de la hiérarchie **Sales Territory** dans la dimension **Sales Territory** , de la hiérarchie **Product Model Lines** dans la dimension **Product** et des hiérarchies **Fiscal Date** et **Calendar Date** dans la dimension **Date** . Ces hiérarchies utilisateur sont toutes des hiérarchies naturelles.  
  
## <a name="defining-attribute-relationships-for-attributes-in-the-customer-geography-hierarchy"></a>Définition des relations d'attributs pour les attributs dans la hiérarchie Customer Geography  
  
1.  Affichez le Concepteur de dimensions pour la dimension Customer, puis cliquez sur l’onglet **Structure de dimension** .  
  
    Dans le volet **Hiérarchies** , notez les niveaux de la hiérarchie **Customer Geography** définie par l’utilisateur. Cette hiérarchie est simplement un chemin d'exploration pour les utilisateurs, aucune relation entre les niveaux et les attributs n'étant définie.  
  
2.  Cliquez sur l’onglet **Relations d’attributs**.  
  
    Notez les quatre relations d’attributs qui lient les attributs non-clé de la table **Geography** à l’attribut clé de la table **Geography** . L’attribut **Geography** est mis en rapport avec l’attribut **Nom complet** . L’attribut **Code postal** est indirectement lié à l’attribut **Nom complet** via l’attribut **Géographie** , car **Code postal** est lié à l’attribut **Géographie** et **Géographie** est lié à l’attribut **Nom complet** . Ensuite, nous modifierons les relations d’attributs afin qu’ils n’utilisent pas l’attribut **Géographie** .  
  
3.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Nom complet** puis sélectionnez **Nouvelle relation d’attribut**.  
  
4.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Nom complet**. Définissez **l’Attribut associé** avec la valeur **Code postal**. Dans la liste **Type de relation** , laissez le type de relation défini sur **Flexible** , car les relations entre les membres peuvent changer au fil du temps.  
  
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    Une icône d'avertissement apparaît dans le diagramme parce que la relation est redondante. La relation **Nom complet** -> **Géographie**-> **Code postal** existe déjà, et vous venez de créer la relation **Nom complet** -> **Code postal**. La relation **Géographie**-> **Code postal** étant maintenant redondante, nous allons la supprimer.  
  
6.  Dans le volet **Relations d’attributs** , cliquez avec le bouton droit sur **Géographie**-> **Code postal** , puis cliquez sur **Supprimer**.  
  
7.  Quand la boîte de dialogue **Supprimer les objets** apparaît, cliquez sur **OK**.  
  
8.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Code postal** puis sélectionnez **Nouvelle relation d’attribut**.  
  
9. Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Code postal**. Définissez **l’Attribut associé** sur **Ville**. Dans la liste **Type de relation** , laissez le type de relation défini sur **Flexible**.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    La relation **Géographie**-> **Ville** étant maintenant redondante, nous la supprimerons.  
  
11. Dans le volet Relations d’attributs, cliquez avec le bouton droit sur **Géographie**-> **Ville** , puis cliquez sur **Supprimer**.  
  
12. Quand la boîte de dialogue **Supprimer les objets** apparaît, cliquez sur **OK**.  
  
13. Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Ville** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
14. Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Ville**. Définissez **l’Attribut associé** sur **État-Province**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide** , car la relation entre une ville et un état ne change pas au fil du temps.  
  
15. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
16. Cliquez avec le bouton droit sur la flèche entre **Géographie** et **État-Province** , puis cliquez sur **Supprimer**.  
  
17. Quand la boîte de dialogue **Supprimer les objets** apparaît, cliquez sur **OK**.  
  
18. Dans le diagramme, cliquez avec le bouton droit sur l’attribut **État-Province**, puis sélectionnez **Nouvelle relation d’attribut**.  
  
19. Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **State-Province**. Définissez **l’Attribut associé** sur **Pays-Région**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide** , car la relation entre un état-province et un pays-région ne change pas au fil du temps.  
  
20. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
21. Dans le volet Relations d’attributs, cliquez avec le bouton droit sur **Géographie**-> **Pays-Région** , puis cliquez sur **Supprimer**.  
  
22. Quand la boîte de dialogue **Supprimer les objets** apparaît, cliquez sur **OK**.  
  
23. Cliquez sur l’onglet **Structure de dimension** .  
  
    Notez que quand vous supprimez la dernière relation d’attribut entre **Géographie** et d’autres attributs, cet attribut **Géographie** est lui-même supprimé. Cela est dû au fait que l'attribut n'est plus utilisé.  
  
24. Dans le menu Fichier, cliquez sur **Enregistrer tout**.  
  
## <a name="defining-attribute-relationships-for-attributes-in-the-sales-territory-hierarchy"></a>Définition des relations d'attributs pour les attributs dans la hiérarchie Sales Territory  
  
1.  Affichez le Concepteur de dimensions pour la dimension **Sales Territory** , puis cliquez sur l’onglet **Relations d’attributs** .  
  
2.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Pays du secteur de vente** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
3.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Pays du secteur de vente**. Définissez **l’Attribut associé** sur **Groupe du secteur de vente**. Dans la liste **Type de relation** , laissez le type de relation défini sur **Flexible**.  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    L’attribut**Groupe du secteur de vente** est maintenant lié à l’attribut **Pays du secteur de vente**et l’attribut **Pays du secteur de vente** est maintenant lié à l’attribut **Région du secteur de vente**. La propriété **RelationshipType** de chacune de ces relations doit avoir la valeur **Flexible** , car le regroupement des régions d’un pays peut changer dans le temps, de même que le regroupement des pays dans les groupes.  
  
## <a name="defining-attribute-relationships-for-attributes-in-the-product-model-lines-hierarchy"></a>Définition des relations d'attributs pour les attributs dans la hiérarchie Product Model Lines  
  
1.  Affichez le Concepteur de dimensions pour la dimension **Product** , puis cliquez sur l’onglet **Relations d’attributs** .  
  
2.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Nom du modèle** puis sélectionnez **Nouvelle relation d’attribut**.  
  
3.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Model Name**. Définissez **l’Attribut associé** sur **Product Line**. Dans la liste **Type de relation**, laissez le type de relation défini sur **Flexible**.  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="defining-attribute-relationships-for-attributes-in-the-fiscal-date-hierarchy"></a>Définition des relations d'attributs pour les attributs dans la hiérarchie Fiscal Date  
  
1.  Affichez le Concepteur de dimensions pour la dimension **Date** , puis cliquez sur l’onglet **Relations d’attributs** .  
  
2.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Month Name** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
3.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Month Name**. Définissez **l’Attribut associé** sur **Fiscal Quarter**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Fiscal Quarter** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
6.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Fiscal Quarter**. Définissez **l’Attribut associé** sur **Fiscal Semester**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
8.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Fiscal Semester** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
9. Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Fiscal Semester**. Définissez **l’Attribut associé** sur **Fiscal Year**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="defining-attribute-relationships-for-attributes-in-the-calendar-date-hierarchy"></a>Définition des relations d'attributs pour les attributs dans la hiérarchie Calendar Date  
  
1.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Month Name** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
2.  Dans la boîte de dialogue **Créer une relation d’attribut**, **l’Attribut source** est **Month Name**. Définissez **l’Attribut associé** avec la valeur **Calendar Quarter**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
3.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
4.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Calendar Quarter** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
5.  Dans la boîte de dialogue **Créer une relation d’attribut**, **l’Attribut source** est **Calendar Quarter**. Définissez **l’Attribut associé** avec la valeur **Calendar Semester**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Calendar Semester** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
8.  Dans la boîte de dialogue **Créer une relation d’attribut**, **l’Attribut source** est **Calendar Semester**. Définissez **l’Attribut associé** avec la valeur **Calendar Year**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
9. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="defining-attribute-relationships-for-attributes-in-the-geography-hierarchy"></a>Définition des relations d'attributs pour les attributs dans la hiérarchie Geography  
  
1.  Affichez le Concepteur de dimensions pour la dimension Geography, puis cliquez sur l’onglet **Relations d’attributs** .  
  
2.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Code postal** puis sélectionnez **Nouvelle relation d’attribut**.  
  
3.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Code postal**. Définissez **l’Attribut associé** sur **Ville**. Dans la liste **Type de relation** , définissez le type de relation sur **Flexible**.  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Ville** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
6.  Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **Ville**. Affectez la valeur **State-Province** à **Attribut associé**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
8.  Dans le diagramme, cliquez avec le bouton droit sur l’attribut **État-Province** , puis sélectionnez **Nouvelle relation d’attribut**.  
  
9. Dans la boîte de dialogue **Créer une relation d’attribut** , **l’Attribut source** est **State-Province**. Affectez la valeur **Country-Region** à **Attribut associé**. Dans la liste **Type de relation** , définissez le type de relation sur **Rigide**.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. Dans le diagramme, cliquez avec le bouton droit sur l’attribut **Clé de zone géographique** , puis sélectionnez **Propriétés**.  
  
12. Définissez la propriété **AttributeHierarchyOptimizedState** sur **NotOptimized**, la propriété **AttributeHierarchyOrdered** sur **False**et la propriété **AttributeHierarchyVisible** sur **False**.  
  
13. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
14. Dans le menu **Générer** de [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], cliquez sur **Déployer Analysis Services Tutorial**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Définition des propriétés de traitement des valeurs Null et membre inconnu](../analysis-services/lesson-4-7-defining-the-unknown-member-and-null-processing-properties.md)  
  
## <a name="see-also"></a>Voir aussi  
[Définir des relations d’attributs](../analysis-services/multidimensional-models/attribute-relationships-define.md)  
[Propriétés de la hiérarchie définie par l’utilisateur](../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies-properties.md)  
  
  
  
