---
title: Programmation d’objets de base OLAP AMO | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: be1452c21d75f90210ddcaea6f1bea31e899e26b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="programming-amo-olap-basic-objects"></a>Programmation d'objets de base OLAP AMO
  La création d'objets [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] complexes ne présente pas de difficultés notoires, mais elle demande de la rigueur. Cette rubrique explique les détails de la programmation d'objets de base OLAP. Cette rubrique contient les sections suivantes :  
  
-   [Objets de dimension](#Dim)  
  
-   [Objets de cube](#Cub)  
  
-   [Objets MeasureGroup](#MG)  
  
-   [Objets de partition](#Part)  
  
-   [Objets d’agrégation](#AD)  
  
##  <a name="Dim"></a> Objets de dimension  
 Pour gérer ou traiter une dimension, vous devez programmer l'objet <xref:Microsoft.AnalysisServices.Dimension>.  
  
### <a name="creating-dropping-and-finding-a-dimension"></a>Création, suppression et recherche d'une dimension  
 La création d'un objet <xref:Microsoft.AnalysisServices.Dimension> s'effectue en quatre étapes :  
  
1.  Créez l'objet dimension et remplissez les attributs de base.  
  
     Les attributs de base sont le nom, le type de dimension, le mode de stockage, la liaison de la source de données, le nom d'attribut de tous les membres, ainsi que d'autres attributs de dimension.  
  
     Avant de créer une dimension, vous devez vérifier qu'elle n'existe pas déjà. Si la dimension existe, elle est supprimée puis recréée.  
  
2.  Créez les attributs qui définissent la dimension.  
  
     Chaque attribut doit être ajouté individuellement au schéma avant d'être utilisé (recherchez la méthode CreateDataItem à la fin de l'exemple de code) et peut ensuite être ajouté à la collection d'attributs de la dimension.  
  
     Les colonnes Key et Name doivent être définies dans tous les attributs.  
  
     L'attribut de clé primaire de la dimension doit être associé à la valeur AttributeUsage.Key pour indiquer clairement que cet attribut est le principal accès à la dimension.  
  
3.  Créez les hiérarchies auxquelles l'utilisateur accèdera pour parcourir la dimension.  
  
     Lorsque vous créez des hiérarchies, l'ordre des niveaux est défini en fonction de leur ordre de création, de haut en bas. Le niveau le plus élevé est celui qui est ajouté en premier à la collection de niveaux de la hiérarchie.  
  
4.  Mettez à jour le serveur en utilisant la méthode Update de la dimension actuelle.  
  
 L’exemple de code suivant crée la dimension Product dans la table des produits Adventure Works dans la base de données.  
  
```  
static void CreateProductDimension(Database db, string datasourceName)  
{  
    // Create the Product dimension  
    Dimension dim = db.Dimensions.FindByName("Product");  
    if ( dim != null)  
       dim.Drop();  
    dim = db.Dimensions.Add("Product");  
    dim.Type = DimensionType.Products;  
    dim.UnknownMember = UnknownMemberBehavior.Hidden;  
    dim.AttributeAllMemberName = "All Products";  
    dim.Source = new DataSourceViewBinding(datasourceName);  
    dim.StorageMode = DimensionStorageMode.Molap;  
  
    #region Create attributes  
  
    DimensionAttribute attr;  
  
    attr = dim.Attributes.Add("Product Name");  
    attr.Usage = AttributeUsage.Key;  
    attr.Type = AttributeType.Product;  
    attr.OrderBy = OrderBy.Name;  
    attr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "DimProduct", "ProductKey"));  
    attr.NameColumn = CreateDataItem(db.DataSourceViews[0], "DimProduct", "EnglishProductName");  
  
    attr = dim.Attributes.Add("Product Line");  
    attr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "DimProduct", "ProductLine"));  
    attr.NameColumn = CreateDataItem(db.DataSourceViews[0], "DimProduct", "ProductLineName");  
  
    attr = dim.Attributes.Add("Model Name");  
    attr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "DimProduct", "ModelName"));  
    attr.AttributeRelationships.Add(new AttributeRelationship("Product Line"));  
    attr.AttributeRelationships.Add(new AttributeRelationship("Subcategory"));  
  
    attr = dim.Attributes.Add("Subcategory");  
    attr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "DimProductSubcategory", "ProductSubcategoryKey"));  
    attr.KeyColumns[0].NullProcessing = NullProcessing.UnknownMember;  
    attr.NameColumn = CreateDataItem(db.DataSourceViews[0], "DimProductSubcategory", "EnglishProductSubcategoryName");  
    attr.AttributeRelationships.Add(new AttributeRelationship("Category"));  
  
    attr = dim.Attributes.Add("Category");  
    attr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "DimProductCategory", "ProductCategoryKey"));  
    attr.NameColumn = CreateDataItem(db.DataSourceViews[0], "DimProductCategory", "EnglishProductCategoryName");  
  
    attr = dim.Attributes.Add("List Price");  
    attr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "DimProduct", "ListPrice"));  
    attr.AttributeHierarchyEnabled = false;  
  
    attr = dim.Attributes.Add("Size");  
    attr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "DimProduct", "Size"));  
    attr.AttributeHierarchyEnabled = false;  
  
    attr = dim.Attributes.Add("Weight");  
    attr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "DimProduct", "Weight"));  
    attr.AttributeHierarchyEnabled = false;  
  
    #endregion  
  
    #region Create hierarchies  
  
    Hierarchy hier;  
  
    hier = dim.Hierarchies.Add("Product Model Categories");  
    hier.AllMemberName = "All Products";  
    hier.Levels.Add("Category").SourceAttributeID = "Category";  
    hier.Levels.Add("Subcategory").SourceAttributeID = "Subcategory";  
    hier.Levels.Add("Model Name").SourceAttributeID = "Model Name";  
  
    hier = dim.Hierarchies.Add("Product Categories");  
    hier.AllMemberName = "All Products";  
    hier.Levels.Add("Category").SourceAttributeID = "Category";  
    hier.Levels.Add("Subcategory").SourceAttributeID = "Subcategory";  
    hier.Levels.Add("Model Name").SourceAttributeID = "Product Name";  
  
    hier = dim.Hierarchies.Add("Product Model Lines");  
    hier.AllMemberName = "All Products";  
    hier.Levels.Add("Subcategory").SourceAttributeID = "Product Line";  
    hier.Levels.Add("Model Name").SourceAttributeID = "Model Name";  
  
    #endregion  
  
    dim.Update();  
}  
  
static DataItem CreateDataItem(DataSourceView dsv, string tableName, string columnName)  
{  
    DataTable dataTable = ((DataSourceView)dsv).Schema.Tables[tableName];  
    DataColumn dataColumn = dataTable.Columns[columnName];  
    return new DataItem(tableName, columnName,  
        OleDbTypeConverter.GetRestrictedOleDbType(dataColumn.DataType));  
}  
  
```  
  
### <a name="processing-a-dimension"></a>Traitement d'une dimension  
 Le traitement d'une dimension est aussi simple que d'utiliser la méthode Process de l'objet <xref:Microsoft.AnalysisServices.Dimension>.  
  
 Le traitement d'une dimension peut affecter tous les cubes qui utilisent la dimension. Pour plus d’informations sur les options de traitement, consultez [du traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md).  
  
 Le code suivant effectue une mise à jour incrémentielle dans toutes les dimensions d'une base de données fournie :  
  
```  
static void UpdateAllDimensions(Database db)  
{  
    foreach (Dimension dim in db.Dimensions)  
        dim.Process(ProcessType.ProcessUpdate);  
}  
```  
  
##  <a name="Cub"></a> Objets de cube  
 Pour gérer ou traiter un cube, vous devez programmer l'objet <xref:Microsoft.AnalysisServices.Cube>.  
  
### <a name="creating-dropping-and-finding-a-cube"></a>Création, suppression et recherche d'un cube  
 La gestion des cubes est comparable à la gestion des dimensions. La création d'un objet <xref:Microsoft.AnalysisServices.Cube> s'effectue en quatre étapes :  
  
1.  Créez l'objet cube et remplissez les attributs de base.  
  
     Les attributs de base sont le nom, le mode de stockage, la liaison de la source de données, la mesure par défaut et d'autres attributs de cube.  
  
     Avant de créer un cube, vous devez vérifier qu'il n'existe pas. Dans l'exemple, si le cube existe, il est supprimé puis recréé.  
  
2.  Ajoutez les dimensions du cube.  
  
     Les dimensions sont ajoutées à la collection de dimensions du cube actuel à partir de la base de données ; les dimensions du cube représentent des références à la collection de dimensions de la base de données. Chaque dimension doit être mappée au cube individuellement. Dans l'exemple, les dimensions sont mappées en fournissant à la dimension de base de données un identificateur interne, un nom pour la dimension du cube et un ID pour la dimension nommée du cube.  
  
     Dans l'exemple de code, notez que la dimension « Date » est ajoutée à trois reprises et qu'à chaque ajout, elle est associée à un nom de dimension du cube différent : Date, Ship Date et Delivery Date. Ces dimensions sont appelées « dimensions de rôle actif ». La dimension de base a beau être la même (Date), dans la table de faits, elle est utilisée dans des « rôles » différents (Order Date, Ship Date, Delivery Date). Consultez la section « Création, suppression et recherche d'un groupe de mesures » plus loin dans ce document pour comprendre comment sont définies les dimensions de rôle actif.  
  
3.  Créez les groupes de mesures auquel l'utilisateur accèdera pour parcourir les données du cube.  
  
     La création des groupes de mesures est expliquée dans la section « Création, suppression et recherche d'un groupe de mesure » plus loin dans ce document. L'exemple englobe la création de groupes de mesures dans différentes méthodes, une pour chaque groupe de mesures.  
  
4.  Mettez à jour le serveur en utilisant la méthode Update du cube actuel.  
  
     La méthode Update est utilisée avec l'option de mise à jour ExpandFull pour faire en sorte que tous les objets soient entièrement mis à jour dans le serveur.  
  
 L'exemple de code suivant crée les différentes parties du cube Adventure Works. Il ne crée pas toutes les dimensions ou les groupes de mesures inclus dans l'exemple de projet Analysis Services Adventure Works.  
  
```  
static void CreateAdventureWorksCube(Database db, string datasourceName)  
{  
    // Create the Adventure Works cube  
    Cube cube = db.Cubes.FindByName("Adventure Works");  
    if ( cube != null)  
       cube.Drop();  
    db.Cubes.Add("Adventure Works");  
    cube.DefaultMeasure = "[Reseller Sales Amount]";  
    cube.Source = new DataSourceViewBinding(datasourceName);  
    cube.StorageMode = StorageMode.Molap;  
  
    #region Create cube dimensions  
  
    Dimension dim;  
  
    dim = db.Dimensions.GetByName("Date");  
    cube.Dimensions.Add(dim.ID, "Date", "Order Date Key - Dim Time");  
    cube.Dimensions.Add(dim.ID, "Ship Date",  
        "Ship Date Key - Dim Time");  
    cube.Dimensions.Add(dim.ID, "Delivery Date",  
        "Delivery Date Key - Dim Time");  
  
    dim = db.Dimensions.GetByName("Customer");  
    cube.Dimensions.Add(dim.ID);  
  
    dim = db.Dimensions.GetByName("Reseller");  
    cube.Dimensions.Add(dim.ID);  
    #endregion  
  
    #region Create measure groups  
  
    CreateSalesReasonsMeasureGroup(cube);  
    CreateInternetSalesMeasureGroup(cube);  
    CreateResellerSalesMeasureGroup(cube);  
    CreateCustomersMeasureGroup(cube);  
    CreateCurrencyRatesMeasureGroup(cube);  
  
    #endregion  
  
    cube.Update(UpdateOptions.ExpandFull);  
}  
```  
  
### <a name="processing-a-cube"></a>Traitement d'un cube  
 Le traitement d'un cube est aussi simple que d'utiliser la méthode Process de l'objet <xref:Microsoft.AnalysisServices.Cube>. Le traitement d'un cube traite également tous les groupes de mesures du cube, ainsi que toutes les partitions du groupe de mesures. Dans un cube, les partitions sont les seuls objets à pouvoir être traités ; dans le cadre du traitement, les groupes de mesures ne sont que des conteneurs de partitions. Le type de traitement spécifié pour le cube est propagé aux partitions. Le traitement de cube et de groupe de mesures en interne se résume au traitement des dimensions et des partitions.  
  
 Pour plus d’informations sur les options de traitement, consultez [le traitement des objets &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/processing-objects-xmla.md), et [du traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md).  
  
 Le code suivant effectuera un traitement complet sur tous les cubes d'une base de données spécifiée :  
  
```  
foreach (Cube cube in db.Cubes)  
             cube.Process(ProcessType.ProcessFull);  
     }  
```  
  
##  <a name="MG"></a>Objets MeasureGroup  
 Pour gérer ou traiter un groupe de mesures, vous devez programmer l'objet <xref:Microsoft.AnalysisServices.MeasureGroup>.  
  
### <a name="creating-dropping-and-finding-a-measuregroup"></a>Création, suppression et recherche d'un groupe de mesures  
 La gestion des groupes de mesures est comparable à la gestion des dimensions et des cubes. La création d'un objet <xref:Microsoft.AnalysisServices.MeasureGroup> s'effectue en procédant comme suit :  
  
1.  Créez l'objet groupe de mesures et remplissez les attributs de base.  
  
     Les attributs de base se composent notamment du nom, du mode de stockage, du mode de traitement, de la mesure par défaut et d'autres attributs de groupe de mesures.  
  
     Avant de créer un groupe de mesures, vérifiez qu'il n'existe pas. Dans l'exemple de code qui suit, si le groupe de mesures existe, celui-ci est supprimé puis recréé.  
  
2.  Créez les mesures du groupe de mesures. Pour chaque mesure créée, les attributs suivants sont assignés : nom, fonction d'agrégation, colonne source, chaîne de format. D'autres attributs peuvent également être assignés. Notez que dans l'exemple de code qui suit, la méthode CreateDataItem ajoute la colonne au schéma.  
  
3.  Ajoutez les dimensions du groupe de mesures.  
  
4.  Les dimensions sont ajoutées à la collection de dimensions du groupe de mesures actuel à partir de la collection de dimensions du cube parent. Dès que la dimension est incluse dans la collection de dimensions du groupe de mesures, une colonne clé de la table de faits peut être mappée à la dimension de sorte que le groupe de mesures puisse être parcouru à travers la dimension.  
  
     Dans l'exemple de code qui suit, consultez les lignes situées en dessous de « Mapping dimension and key column from fact table ». Les dimensions de rôle actif sont implémentées en liant différentes clés de substitution à la même dimension sous des noms différents. À chaque dimension de rôle actif (Date, Ship Date, Delivery Date) est liée une clé de substitution différente (OrderDateKey, ShipDateKey, DueDateKey). Toutes les clés proviennent de la table de faits FactInternetSales.  
  
5.  Ajoutez les partitions conçues du groupe de mesures.  
  
     Dans l'exemple de code qui suit, la création de partition est englobée dans une méthode.  
  
6.  Mettez à jour le serveur en utilisant la méthode Update du groupe de mesures actuel.  
  
     Dans l'exemple de code qui suit, tous les groupes de mesures sont mis à jour en même temps que le cube.  
  
 L'exemple de code suivant crée le groupe de mesures InternetSales de l'exemple de projet Analysis Services Adventure Works.  
  
```  
static void CreateInternetSalesMeasureGroup(Cube cube)  
{  
    // Create the Internet Sales measure group  
    Database db = cube.Parent;  
    MeasureGroup mg = cube.MeasureGroups.FindByName("Internet Sales");  
    if ( mg != null)  
       mg.Drop();  
    mg = cube.MeasureGroups.Add("Internet Sales");  
    mg.StorageMode = StorageMode.Molap;  
    mg.ProcessingMode = ProcessingMode.LazyAggregations;  
    mg.Type = MeasureGroupType.Sales;  
  
    #region Create measures  
  
    Measure meas;  
  
    meas = mg.Measures.Add("Internet Sales Amount");  
    meas.AggregateFunction = AggregationFunction.Sum;  
    meas.FormatString = "Currency";  
    meas.Source = CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "SalesAmount");  
  
    meas = mg.Measures.Add("Internet Order Quantity");  
    meas.AggregateFunction = AggregationFunction.Sum;  
    meas.FormatString = "#,#";  
    meas.Source = CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "OrderQuantity");  
  
    meas = mg.Measures.Add("Internet Unit Price");  
    meas.AggregateFunction = AggregationFunction.Sum;  
    meas.FormatString = "Currency";  
    meas.Visible = false;  
    meas.Source = CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "UnitPrice");  
  
    meas = mg.Measures.Add("Internet Total Product Cost");  
    meas.AggregateFunction = AggregationFunction.Sum;  
    //meas.MeasureExpression = "[Internet Total Product Cost] * [Average Rate]";  
    meas.FormatString = "Currency";  
    meas.Source = CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "TotalProductCost");  
  
    meas = mg.Measures.Add("Internet Order Count");  
    meas.AggregateFunction = AggregationFunction.Count;  
    meas.FormatString = "#,#";  
    meas.Source = CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "ProductKey");  
  
    #endregion  
  
    #region Create measure group dimensions  
  
    CubeDimension cubeDim;  
    RegularMeasureGroupDimension regMgDim;  
    ManyToManyMeasureGroupDimension mmMgDim;  
    MeasureGroupAttribute mgAttr;  
  
    //   Mapping dimension and key column from fact table  
    //      > select dimension and add it to the measure group  
    cubeDim = cube.Dimensions.GetByName("Date");  
    regMgDim = new RegularMeasureGroupDimension(cubeDim.ID);  
    mg.Dimensions.Add(regMgDim);  
  
    //      > add key column from dimension and map it with   
    //        the surrogate key in the fact table  
    mgAttr = regMgDim.Attributes.Add(cubeDim.Dimension.Attributes.GetByName("Date").ID);   // this is dimension key column  
    mgAttr.Type = MeasureGroupAttributeType.Granularity;  
    mgAttr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "OrderDateKey"));   // this surrogate key in fact table  
  
    cubeDim = cube.Dimensions.GetByName("Ship Date");  
    regMgDim = new RegularMeasureGroupDimension(cubeDim.ID);  
    mg.Dimensions.Add(regMgDim);  
    mgAttr = regMgDim.Attributes.Add(cubeDim.Dimension.Attributes.GetByName("Date").ID);  
    mgAttr.Type = MeasureGroupAttributeType.Granularity;  
    mgAttr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "ShipDateKey"));  
  
    cubeDim = cube.Dimensions.GetByName("Delivery Date");  
    regMgDim = new RegularMeasureGroupDimension(cubeDim.ID);  
    mg.Dimensions.Add(regMgDim);  
    mgAttr = regMgDim.Attributes.Add(cubeDim.Dimension.Attributes.GetByName("Date").ID);  
    mgAttr.Type = MeasureGroupAttributeType.Granularity;  
    mgAttr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "DueDateKey"));  
  
    cubeDim = cube.Dimensions.GetByName("Customer");  
    regMgDim = new RegularMeasureGroupDimension(cubeDim.ID);  
    mg.Dimensions.Add(regMgDim);  
    mgAttr = regMgDim.Attributes.Add(cubeDim.Dimension.Attributes.GetByName("Full Name").ID);  
    mgAttr.Type = MeasureGroupAttributeType.Granularity;  
    mgAttr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "CustomerKey"));  
  
    cubeDim = cube.Dimensions.GetByName("Product");  
    regMgDim = new RegularMeasureGroupDimension(cubeDim.ID);  
    mg.Dimensions.Add(regMgDim);  
    mgAttr = regMgDim.Attributes.Add(cubeDim.Dimension.Attributes.GetByName("Product Name").ID);  
    mgAttr.Type = MeasureGroupAttributeType.Granularity;  
    mgAttr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "ProductKey"));  
  
    cubeDim = cube.Dimensions.GetByName("Source Currency");  
    regMgDim = new RegularMeasureGroupDimension(cubeDim.ID);  
    mg.Dimensions.Add(regMgDim);  
    mgAttr = regMgDim.Attributes.Add(cubeDim.Dimension.Attributes.GetByName("Currency").ID);  
    mgAttr.Type = MeasureGroupAttributeType.Granularity;  
    mgAttr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "CurrencyKey"));  
  
    cubeDim = cube.Dimensions.GetByName("Sales Reason");  
    mmMgDim = new ManyToManyMeasureGroupDimension();  
    mmMgDim.CubeDimensionID = cubeDim.ID;  
    mmMgDim.MeasureGroupID = cube.MeasureGroups.GetByName("Sales Reasons").ID;  
    mg.Dimensions.Add(mmMgDim);  
  
    cubeDim = cube.Dimensions.GetByName("Internet Sales Order Details");  
    regMgDim = new RegularMeasureGroupDimension(cubeDim.ID);  
    mg.Dimensions.Add(regMgDim);  
    mgAttr = regMgDim.Attributes.Add(cubeDim.Dimension.Attributes.GetByName("Sales Order Key").ID);  
    mgAttr.Type = MeasureGroupAttributeType.Granularity;  
    mgAttr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "SalesOrderNumber"));  
    mgAttr.KeyColumns.Add(CreateDataItem(db.DataSourceViews[0], "FactInternetSales", "SalesOrderLineNumber"));  
  
    #endregion  
  
    #region Create partitions  
  
    CreateInternetSalesMeasureGroupPartitions( mg)  
  
    #endregion  
}  
```  
  
### <a name="processing-a-measure-group"></a>Traitement d'un groupe de mesures  
 Le traitement d'un groupe de mesures est aussi simple que d'utiliser la méthode Process de l'objet <xref:Microsoft.AnalysisServices.MeasureGroup>. Le traitement d'un groupe de mesures traite toutes les partitions qui appartiennent au groupe de mesures. Le traitement d'un groupe de mesures en interne se résume au traitement des dimensions et des partitions. Consultez [Traitement d'une partition](#ProcPart) dans ce document.  
  
 Pour plus d’informations sur les options de traitement, consultez [le traitement des objets &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/processing-objects-xmla.md), et [du traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md).  
  
 Le code suivant assurera un traitement complet dans tous les groupes de mesures d'un cube fourni.  
  
```  
static void FullProcessAllMeasureGroups(Cube cube)  
{  
    foreach (MeasureGroup mg in cube.MeasureGroups)  
        mg.Process(ProcessType.ProcessFull);  
}  
```  
  
##  <a name="Part"></a>Objets de partition  
 Pour gérer ou traiter une partition, vous devez programmer un objet <xref:Microsoft.AnalysisServices.Partition>.  
  
### <a name="creating-dropping-and-finding-a-partition"></a>Création, suppression et recherche d'une partition  
 Les partitions sont des objets simples qui peuvent être créés en deux étapes.  
  
1.  Créez l'objet partition et remplissez les attributs de base.  
  
     Les attributs de base correspondent au nom, au mode de stockage, à la source de partition, à la tranche, ainsi qu'à d'autres attributs de groupe de mesures. La source de partition définit l'instruction select SQL pour la partition actuelle. La tranche est une expression MDX spécifiant un tuple ou un ensemble qui délimite une partie des dimensions du groupe de mesures parent qui sont contenues dans la partition actuelle. Pour les partitions MOLAP, le découpage par tranches est déterminé automatiquement chaque fois que la partition est traitée.  
  
     Avant de créer une partition, vous devez vérifier qu'elle n'existe pas. Dans l'exemple de code qui suit, si la partition existe, elle est supprimée puis recréé.  
  
2.  Mettez à jour le serveur en utilisant la méthode Update de la partition actuelle.  
  
     Dans l'exemple de code qui suit, toutes les partitions sont mises à jour en même temps que le cube.  
  
 L'exemple de code suivant crée des partitions pour le groupe de mesures « InternetSales ».  
  
```  
static void CreateInternetSalesMeasureGroupPartitions(MeasureGroup mg)  
{  
    Partition part;  
    part = mg.Partitions.FindByName("Internet_Sales_184");  
    if ( part != null)  
       part.Drop();  
    part = mg.Partitions.Add("Internet_Sales_184");  
    part.StorageMode = StorageMode.Molap;  
    part.Source = new QueryBinding(db.DataSources[0].ID, "SELECT * FROM [dbo].[FactInternetSales] WHERE OrderDateKey <= '184'");  
    part.Slice = "[Date].[Calendar Year].&[2001]";  
    part.Annotations.Add("LastOrderDateKey", "184");  
  
    part = mg.Partitions.FindByName("Internet_Sales_549");  
    if ( part != null)  
       part.Drop();  
    part = mg.Partitions.Add("Internet_Sales_549");  
    part.StorageMode = StorageMode.Molap;  
    part.Source = new QueryBinding(db.DataSources[0].ID, "SELECT * FROM [dbo].[FactInternetSales] WHERE OrderDateKey > '184' AND OrderDateKey <= '549'");  
    part.Slice = "[Date].[Calendar Year].&[2002]";  
    part.Annotations.Add("LastOrderDateKey", "549");  
  
    part = mg.Partitions.FindByName("Internet_Sales_914");  
    if ( part != null)  
       part.Drop();  
    part = mg.Partitions.Add("Internet_Sales_914");  
    part.StorageMode = StorageMode.Molap;  
    part.Source = new QueryBinding(db.DataSources[0].ID, "SELECT * FROM [dbo].[FactInternetSales] WHERE OrderDateKey > '549' AND OrderDateKey <= '914'");  
    part.Slice = "[Date].[Calendar Year].&[2003]";  
    part.Annotations.Add("LastOrderDateKey", "914");  
}  
```  
  
###  <a name="ProcPart"></a> Traitement d’une Partition  
 Le traitement d'une partition est aussi simple que d'utiliser la méthode Process de l'objet <xref:Microsoft.AnalysisServices.Partition>.  
  
 Pour plus d’informations sur les options de traitement, consultez [le traitement des objets &#40;XMLA&#41; ](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/processing-objects-xmla.md) et [du traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md).  
  
 L'exemple de code suivant effectue un traitement complet dans toutes les partitions d'un groupe de mesures spécifié.  
  
```  
static void FullProcessAllPartitions(MeasureGroup mg)  
{  
    foreach (Partition part in mg.Partitions)  
        part.Process(ProcessType.ProcessFull);  
}  
```  
  
### <a name="merging-partitions"></a>Fusion de partitions  
 La fusion de partitions est une opération qui consiste à transformer deux partitions ou plus en une seule.  
  
 La fusion de partitions est une méthode de l'objet <xref:Microsoft.AnalysisServices.Partition>. Cette commande fusionne les données d'une ou plusieurs partitions sources dans une partition cible, puis supprime les partitions sources.  
  
 Les partitions ne peuvent être fusionnées que si elles répondent à tous les critères suivants :  
  
-   les partitions se trouvent dans le même groupe de mesures ;  
  
-   les partitions sont stockées dans le même mode (MOLAP, HOLAP et ROLAP) ;  
  
-   les partitions résident sur le même serveur ; les partitions distantes peuvent être fusionnées si elle se trouvent sur le même serveur.  
  
 Contrairement aux versions précédentes, dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] n’est pas nécessaire que toutes les partitions sources aient une conception des agrégations identiques.  
  
 Le jeu d'agrégations obtenu pour la partition cible a le même état que le jeu d'agrégations avant l'exécution de la commande merge.  
  
 L'exemple de code suivant fusionne toutes les partitions d'un groupe de mesures spécifié. Les partitions sont fusionnées dans la première partition du groupe de mesures.  
  
```  
static void MergeAllPartitions(MeasureGroup mg)  
{  
    if (mg.Partitions.Count > 1)  
    {  
        Partition[] partArray = new Partition[mg.Partitions.Count - 1];  
        for (int i = 1; i < mg.Partitions.Count; i++)  
            partArray[i - 1] = mg.Partitions[i];  
        mg.Partitions[0].Merge(partArray);  
        //To have last changes in the server reflected in AMO  
        mg.Refresh();  
    }  
```  
  
##  <a name="AD"></a>Objets d’agrégation  
 Pour concevoir une agrégation et l'appliquer à une ou plusieurs partitions, vous devez programmer l'objet <xref:Microsoft.AnalysisServices.Aggregation>.  
  
### <a name="creating-and-dropping-aggregations"></a>Création et suppression d'agrégations  
 Il est facile de créer des agrégations et de les assigner à des groupes de mesures ou à des partitions à l'aide de la méthode DesignAggregations de l'objet <xref:Microsoft.AnalysisServices.AggregationDesign>. L'objet <xref:Microsoft.AnalysisServices.AggregationDesign> est un objet distinct de la partition ; l'objet <xref:Microsoft.AnalysisServices.AggregationDesign> est contenu dans l'objet <xref:Microsoft.AnalysisServices.MeasureGroup>. Les agrégations peuvent être conçues jusqu'à un certain niveau d'optimisation (de 0 à 100) ou un certain niveau de stockage (en octets). Plusieurs partitions peuvent utiliser une même conception d'agrégation.  
  
 L'exemple de code suivant crée des agrégations pour toutes les partitions d'un groupe de mesures fourni. Les agrégations qui existent éventuellement dans les partitions sont supprimées.  
  
```  
static public String DesignAggregationsOnPartitions(MeasureGroup mg, double optimizationWanted, double maxStorageBytes)  
{  
    double optimization = 0;  
    double storage = 0;  
    long aggCount = 0;  
    bool finished = false;  
    AggregationDesign ad = null;  
    String aggDesignName;  
    String AggregationsDesigned = "";  
    aggDesignName = mg.AggregationPrefix + "_" + mg.Name;  
    ad = mg.AggregationDesigns.Add();  
    ad.Name = aggDesignName;  
    ad.InitializeDesign();  
    while ((!finished) && (optimization < optimizationWanted) && (storage < maxStorageBytes))  
    {  
        ad.DesignAggregations(out optimization, out storage, out aggCount, out finished);  
    }  
    ad.FinalizeDesign();  
    foreach (Partition part in mg.Partitions)  
    {  
        part.AggregationDesignID = ad.ID;  
        AggregationsDesigned += aggDesignName + " = " + aggCount.ToString() + " aggregations designed\r\n\tOptimization: " + optimization.ToString() + "/" + optimizationWanted.ToString() + "\n\r\tStorage: " + storage.ToString() + "/" + maxStorageBytes.ToString() + " ]\n\r";  
     }  
     return AggregationsDesigned;  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices>   
 [Présentation des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-classes-introduction.md)   
 [Classes OLAP AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-olap-classes.md)   
 [Architecture logique & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)   
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)   
 [Installer les exemples de données et des projets pour les didacticiel de modélisation multidimensionnelle Analysis Services](../../../analysis-services/install-sample-data-and-projects.md)  
  
  
