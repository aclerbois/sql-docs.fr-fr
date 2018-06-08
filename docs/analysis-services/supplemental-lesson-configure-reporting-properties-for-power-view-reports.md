---
title: Configurer les propriétés de création de rapports pour les rapports Power View | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 27698f0431a11b73c1ebacd532769269458f1225
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="supplemental-lesson---configure-reporting-properties-for-power-view-reports"></a>Supplémentaires leçon - configurer les propriétés de création de rapports pour les rapports Power View
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../includes/ssas-appliesto-sql2016-later-aas.md)]

Dans cette leçon supplémentaire, vous allez définir les propriétés du projet AW Internet Sales. Les propriétés de création de rapports facilitent la sélection et l'affichage des données du modèle dans Power View. Vous définirez également des propriétés permettant de masquer certaines colonnes et tables, et vous créerez des données à utiliser dans des graphiques.   
  
Durée estimée pour effectuer cette leçon : **30 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  
Cette leçon supplémentaire fait partie d'un didacticiel de modélisation tabulaire, qui doit être suivi dans l'ordre. Avant d'effectuer les tâches de cette leçon supplémentaire, vous devez avoir terminé toutes les leçons précédentes.  
Pour pouvoir effectuer cette leçon supplémentaire, les composants suivants doivent également être installés :  
  
-   Le projet AW Internet Sales (terminé ce didacticiel) prêt à être déployé ou déjà déployé sur un serveur Analysis Services.  
  
  
## <a name="model-properties-that-affect-reporting"></a>Propriétés de modèle qui affectent la création de rapports  
Lorsque vous créez un modèle tabulaire, vous pouvez définir certaines propriétés sur des tables et colonnes individuelles afin d'améliorer l'expérience de création de rapports dans Power View. En outre, vous pouvez créer des données de modèle supplémentaires pour prendre en charge la visualisation des données et d'autres fonctionnalités spécifiques au client de création de rapports. Pour l'exemple de modèle Adventure Works Internet Sales, voici quelques-unes des modifications que vous allez apporter :  
  
-   **Ajouter des données** – L’ajout de données dans une colonne calculée en utilisant une formule DAX crée des informations de date dans un format plus facile à utiliser dans des graphiques.  
  
-   **Masquer des tables et colonnes inutiles pour l’utilisateur final** - La propriété **Hidden** indique si les tables et colonnes sont affichées dans le client de création de rapports. Les éléments masqués font toujours partie du modèle et demeurent disponibles pour les requêtes et les calculs.  
  
-   **Activer les tables en un clic** – Par défaut, aucune action ne se produit si un utilisateur final clique sur une table dans la liste de champs. Pour modifier ce comportement de façon à qu'un clic sur une table ajoute la table au rapport, vous devez définir Ensemble de champs par défaut sur chaque colonne à inclure dans la table. Cette propriété est définie sur les colonnes de table que les utilisateurs finaux utiliseront le plus probablement.  
  
-   **Définir le regroupement si nécessaire** - La propriété **Conserver les lignes uniques** indique si les valeurs de la colonne doivent être regroupées par valeurs dans un autre champ, tel qu’un champ d’identificateur. En ce qui concerne les colonnes qui contiennent des valeurs en double, telles que Customer Name (par exemple, plusieurs clients appelés John Smith), il est important de regrouper (conserver les lignes uniques) sur le champ **Identificateur de ligne** pour fournir les résultats appropriés aux utilisateurs finaux.  
  
-   **Définir les types de données et les formats de données** - Par défaut, Power View applique des règles basées sur le type de données de colonne pour déterminer si le champ peut être utilisé comme mesure. Étant donné que chaque visualisation de données dans Power View comprend également des règles indiquant où peuvent être placées les mesures et les non-mesures, il est important de définir le type de données dans le modèle, ou de remplacer la valeur par défaut, pour obtenir le comportement voulu pour l’utilisateur final.  
  
-   **Définir le tri par colonne** - La propriété **Trier par colonne** indique si les valeurs de la colonne doivent être triées par valeurs dans un autre champ. Par exemple, dans la colonne Month Calendar qui contient les noms de mois, triez les noms de mois en fonction de la colonne Month Number.  
  
## <a name="hide-tables-from-client-tools"></a>Masquer des tables des outils clients  
Étant donné qu'il existe déjà une colonne calculée Product Category et une colonne calculée Product Subcategory dans la table Product, il est inutile que les tables Product Category et Product Subcategory soient visibles pour les applications clientes.  
  
#### <a name="to-hide-the-product-category-and-product-subcategory-tables"></a>Pour masquer les tables Product Category et Product Subcategory  
  
1.  Dans le générateur de modèles, cliquez avec le bouton droit sur la table **Product Category** , puis cliquez sur **Masquer dans les outils clients**.  
  
2.  Cliquez avec le bouton droit sur la table **Product Subcategory** , puis cliquez sur **Masquer dans les outils clients**.  
  
## <a name="create-new-data-for-charts"></a>Créer des données pour les graphiques  
Il peut parfois s'avérer nécessaire de créer des données dans votre modèle en utilisant des formules DAX. Dans cette tâche, vous allez ajouter deux colonnes calculées à la table Date. Ces nouvelles colonnes fourniront des champs de date dans un format pratique à utiliser dans les graphiques.  
  
#### <a name="to-create-new-data-for-charts"></a>Pour créer des données pour les graphiques  
  
1.  Dans la table **Date** , faites défiler la table sur la droite, puis cliquez sur **Ajouter une colonne**.  
  
2.  Ajoutez deux colonnes calculées en utilisant les formules suivantes dans la barre de formule :  
  
    |Nom de la colonne|Formule|  
    |---------------|-----------|  
    |Year Quarter|=[Calendar Year] & " Q" & [Calendar Quarter]|  
    |Year Month|=[Calendar Year] & FORMAT([Month],"#00")|  
  
## <a name="default-field-set"></a>Ensemble de champs par défaut  
L’ensemble de champs par défaut est une liste prédéfinie de colonnes et des mesures pour une table qui sont automatiquement ajoutés à un canevas de rapport lorsque la table est activée dans la liste de champs du rapport. Pour l'essentiel, vous pouvez indiquer les colonnes, les mesures et l'ordre des champs par défaut qui seront affichés dans cette table visualisée dans des rapports Power View.  Pour le modèle Internet Sales, vous allez définir un ensemble de champs par défaut et l'ordre pour les tables Customer, Geography et Product. Seules sont incluses les colonnes les plus courantes qui seront affichées lors de l'analyse des données Adventure Works Internet Sales à l'aide de rapports Power View.  
  
Pour obtenir des informations détaillées sur l’ensemble de champs par défaut, consultez [configurer champ défini par défaut pour les rapports Power View](../analysis-services/tabular-models/power-view-configure-default-field-set-for-reports.md) dans la documentation en ligne de SQL Server.  
  
#### <a name="to-set-default-field-set-for-tables"></a>Pour définir un ensemble de champs par défaut pour les tables  
  
1.  Dans le concepteur de modèles, cliquez sur la table (onglet) **Customer** .  
  
2.  Dans la fenêtre **Propriétés** , sous **Propriétés de création de rapports**, dans la propriété **Ensemble de champs par défaut** , cliquez sur **Cliquer pour modifier** pour ouvrir la boîte de dialogue **Ensemble de champs par défaut** .  
  
3.  Dans la boîte de dialogue **Ensemble de champs par défaut** , dans la zone de liste **Champs dans la table** , appuyez sur Ctrl, puis sélectionnez les champs suivants et cliquez sur **Ajouter**.  
  
    **Birth Date**, **Customer Alternate Id**, **First Name**, **Last Name**.  
  
4.  Dans la fenêtre **Champs par défaut, dans l’ordre** , utilisez les boutons Monter et Descendre pour définir l’ordre suivant :  
  
    **Customer Alternate Id**  
  
    **First Name**  
  
    **Last Name**  
  
    **Birth Date**.  
  
5.  Cliquez sur **Ok** pour fermer la boîte de dialogue **Ensemble de champs par défaut** pour la table **Customer** .  
  
6.  Effectuez ces mêmes étapes pour la table **Geography** , en sélectionnant les champs suivants et en les plaçant dans cet ordre.  
  
    **City**, **State Province Code**, **Country Region Code**.  
  
7.  Pour finir, effectuez ces mêmes étapes pour la table **Product** , en sélectionnant les champs suivants et en les plaçant dans cet ordre.  
  
    **Product Alternate Id**, **Product Name**.  
  
## <a name="table-behavior"></a>Comportement de la table  
La propriété Comportement de la table, vous permet de modifier le comportement par défaut des différents types de visualisation et le comportement de regroupement des tables utilisées dans les rapports Power View. Cela produit un meilleur placement par défaut des informations d'identification, telles que les noms, photos ou titres, dans les mises en page de mosaïque, de carte et de graphique.  
  
Pour plus d’informations sur les propriétés de comportement de la Table, consultez [configurer des propriétés de comportement de Table pour les rapports Power View](../analysis-services/tabular-models/power-view-configure-table-behavior-properties-for-reports.md) dans la documentation en ligne de SQL Server.  
  
#### <a name="to-set-table-behavior"></a>Pour définir le comportement de la table 
  
1.  Dans le concepteur de modèles, cliquez sur la table (onglet) **Customer** .  
  
2.  Dans la fenêtre **Propriétés** , dans la propriété **Comportement de la table** , cliquez sur **Cliquer pour modifier**pour ouvrir la boîte de dialogue **Comportement de la table** .  
  
3.  Dans la boîte de dialogue **Comportement de la table** , dans la liste déroulante **Identificateur de ligne** , sélectionnez la colonne **Customer Id** .  
  
4.  Dans zone de liste **Conserver les lignes uniques** , sélectionnez **Prénom** et **Nom**.  
  
    Ce paramètre de propriété spécifie les colonnes qui fournissent des valeurs qui doivent être traitées comme uniques même en cas de doublons (par exemple, dans les cas où deux employés ou plus portent le même nom).  
  
5.  Dans la zone de liste déroulante **Étiquette par défaut** , sélectionnez la colonne **Nom** .  
  
    Ce paramètre de propriété spécifie que cette colonne fournit un nom d'affichage pour représenter les données de ligne.  
  
6.  Répétez ces étapes pour la table **Geography** , en sélectionnant la colonne **Geography Id** comme identificateur de ligne et la colonne **City** dans la zone de liste **Conserver les lignes uniques** . Il n'est pas nécessaire de définir une étiquette par défaut pour cette table.  
  
7.  Répétez ces étapes pour la table **Product** , en sélectionnant la colonne **Product Id** comme identificateur de ligne et la colonne **Product Name** dans la zone de liste **Conserver les lignes uniques** . Pour **Étiquette par défaut**, sélectionnez **Product Alternate Id**.  
  
## <a name="reporting-properties-for-columns"></a>Propriétés de création de rapports pour les colonnes  
Il existe plusieurs propriétés de colonne de base et propriétés de création de rapports spécifiques sur les colonnes que vous pouvez définir pour améliorer l'expérience de création de rapports d'un modèle. Par exemple, il n'est peut-être pas nécessaire que toutes les colonnes s'affichent dans chacune des tables. Tout comme vous avez masqué les tables Product Category et Product Subcategory précédemment, vous pouvez masquer des colonnes spécifiques d'une table en utilisant la propriété Hidden d'une colonne. D'autres propriétés, telles que le Format de date et Trier par colonne, peuvent également affecter le mode d'affichage des données de colonne dans les rapports. Vous allez maintenant définir certaines de ces propriétés sur des colonnes spécifiques. D'autres colonnes ne nécessitent aucune intervention, et ne figurent pas ci-dessous.  
  
Vous n'allez définir que quelques propriétés de colonne, mais il en existe de nombreuses autres. Pour plus d’informations sur les propriétés de colonne, consultez [propriétés de la colonne](../analysis-services/tabular-models/column-properties-ssas-tabular.md) dans la documentation en ligne de SQL Server.  
  
#### <a name="to-set-properties-for-columns"></a>Pour définir les propriétés des colonnes  
  
1.  Dans le concepteur de modèles, cliquez sur la table (onglet) **Customer** .  
  
2.  Cliquez sur la colonne **Customer Id** pour afficher les propriétés de la colonne dans la fenêtre **Propriétés** .  
  
3.  Dans la fenêtre **Propriétés** , affectez la valeur True à la propriété **Hidden** . La colonne **Customer Id** s’affiche grisée dans le concepteur de modèles.  
  
4.  Répétez ces étapes, en définissant les propriétés de création de rapports et de colonne suivantes pour toutes les tables spécifiées. Conservez les valeurs par défaut de toutes les autres propriétés.  
  
    Remarque : Pour toutes les colonnes de dates, assurez-vous que **Type de données** est défini sur **Date**.  
  
    **Customer**  
  
    |Colonne|Propriété|Valeur|  
    |----------|------------|---------|  
    |Geography Id|Hidden|True|  
    |Birth Date|Format de données|Date courte|  
  
    **Date**  
  
    > [!NOTE]  
    > Étant donné que la table Date a été sélectionnée comme table de date du modèle à l'aide du paramètre Marquer en tant que table de dates, dans la leçon 7 : Marquer en tant que table de dates, et la colonne Date de la table Date comme la colonne à utiliser en tant qu'identificateur unique, la propriété Identificateur de ligne de la colonne Date se verra automatiquement affecter la valeur True, et ne pourra pas être modifiée. Lorsque vous utilisez les fonctions Time Intelligence dans des formules DAX, vous devez spécifier une table de dates. Dans ce modèle, vous avez créé plusieurs mesures à l'aide de fonctions Time Intelligence pour calculer les données de vente de plusieurs périodes, telles que les trimestres précédents et actuel, ainsi qu'à des fins d'utilisation dans des indicateurs de performance clés. Pour plus d’informations sur la spécification d’une table de dates, consultez [spécifier la marque comme Table de dates à utiliser avec Time Intelligence](../analysis-services/tabular-models/specify-mark-as-date-table-for-use-with-time-intelligence-ssas-tabular.md) dans la documentation en ligne de SQL Server.  
  
    |Colonne|Propriété|Valeur|  
    |----------|------------|---------|  
    |Date|Format de données|Date courte|  
    |Day Number of Week|Hidden|True|  
    |Day Name|Trier par colonne|Day Number of Week|  
    |Day of Week|Hidden|True|  
    |Day of Month|Hidden|True|  
    |Day of Year|Hidden|True|  
    |Month Name|Trier par colonne|Mois|  
    |Month|Hidden|True|  
    |Month Calendar|Hidden|True|  
    |Fiscal Quarter|Hidden|True|  
    |Fiscal Year|Hidden|True|  
    |Fiscal Semester|Hidden|True|  
  
    **Geography**  
  
    |Colonne|Propriété|Valeur|  
    |----------|------------|---------|  
    |Geography Id|Hidden|True|  
    |Sales Territory Id|Hidden|True|  
  
    **Product**  
  
    |Colonne|Propriété|Valeur|  
    |----------|------------|---------|  
    |Product Id|Hidden|True|  
    |Product Alternate Id|Étiquette par défaut|True|  
    |Product Subcategory Id|Hidden|True|  
    |Product Start Date|Format de données|Date courte|  
    |Product End Date|Format de données|Date courte|  
  
    **Internet Sales**  
  
    |Colonne|Propriété|Valeur|  
    |----------|------------|---------|  
    |Product Id|Hidden|True|  
    |Customer Id|Hidden|True|  
    |Promotion Id|Hidden|True|  
    |Currency Id|Hidden|True|  
    |Sales Territory Id|Hidden|True|  
    |Order Quantity|Type de données<br /><br />Format de données<br /><br />Nombre de décimales|Nombre décimal<br /><br />Nombre décimal<br /><br />0|  
    |Order Date|Format de données|Date courte|  
    |Due Date|Format de données|Date courte|  
    |Ship Date|Format de données|Date courte|  
  
## <a name="redeploy-the-adventure-works-internet-sales-tabular-model"></a>Redéployer le modèle tabulaire Internet Sales Adventure Works  
Étant donné que vous avez modifié le modèle, vous devez le redéployer.  
  
#### <a name="to-redeploy-the-adventure-works-internet-sales-tabular-model"></a>Pour redéployer le modèle tabulaire Internet Sales Adventure Works  
  
-   Dans SSDT, cliquez sur le **générer** menu, puis sur **déployer Adventure Works Internet Sales Model**.  
  
    La boîte de dialogue **Déployer** apparaît et affiche l’état de déploiement des métadonnées, ainsi que de chaque table incluse dans le modèle.  
  
## <a name="next-steps"></a>Étapes suivantes  
Utilisez maintenant Power View pour visualiser les données du modèle. Assurez-vous que les comptes Analysis Services et Reporting Services sur le site SharePoint disposent des autorisations en lecture sur l'instance Analysis Services sur laquelle vous avez déployé le modèle.  
  
Pour créer une source de données de rapport Reporting Services qui pointe vers votre modèle, consultez [Type de connexion de modèle de table (SSRS)](http://msdn.microsoft.com/library/hh270317%28v=SQL.110%29.aspx).  
  
  
  
