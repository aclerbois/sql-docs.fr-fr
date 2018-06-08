---
title: Programmation OLAP AMO avancés objets | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 03114f3f88d53efa01580e07db164c679b324661
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="programming-amo-olap-advanced-objects"></a>Programmation d'objets OLAP AMO avancés
  Cette rubrique explique en détails la programation d'objets OLAP AMO (Analysis Management Objects) avancés. Cette rubrique contient les sections suivantes :  
  
-   [Objets action](#Action)  
  
-   [Objets KPI](#KPI)  
  
-   [Objets de perspective](#Persp)  
  
-   [Objets ProactiveCaching](#PC)  
  
-   [Objets Translation](#Transl)  
  
##  <a name="Action"></a>Objets action  
 Les classes Action sont utilisées pour créer une réponse active lors de l'exploration de certaines zones du cube. Les objets Action peuvent être définis à l'aide d'objets AMO, mais ils sont utilisés à partir de l'application cliente qui explore les données. Les actions peuvent être de différents types et doivent être créées en fonction de leur type. Les actions peuvent être :  
  
-   Des actions d'extraction, qui retournent l'ensemble de lignes qui représente les données sous-jacentes des cellules sélectionnées du cube où l'action se produit.  
  
-   Des actions de rapport, qui retournent un rapport [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] associé à la section sélectionnée du cube où l'action se produit.  
  
-   Des actions standard, qui retournent l'élément d'action (URL, HTML, jeu de données, ensemble de lignes et autres éléments) associé à la section sélectionnée du cube où l'action se produit.  
  
 La création d'un objet Action passe par les étapes suivantes :  
  
1.  Créez l'objet action dérivé et remplissez les attributs de base.  
  
     Les attributs de base sont constitués du type d'action, du type ou de la section cible du cube, de la zone cible ou spécifique du cube où l'action est disponible, de la légende et de l'emplacement où celle-ci est une expression MDX.  
  
2.  Remplissez les attributs spécifiques du type d'action.  
  
     Les attributs sont différents pour les trois types d'actions ; consultez l'exemple de code qui suit pour examiner les paramètres.  
  
3.  Ajoutez l'action à la collection du cube et mettez ce dernier à jour. L'action est un objet qui ne peut pas être mis à jour.  
  
 Pour tester l'action, vous avez besoin d'une application différente. Vous pouvez tester votre action dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. Tout d’abord, vous devez installer [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] exemples, consultez [du traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md).  
  
 L'exemple de code suivant réplique trois actions différentes de l'exemple de projet Analysis Services Adventure Works. Vous pouvez distinguer les actions les unes des autres, car celles que vous introduisez au moyen du code suivant commencent par « My ».  
  
```  
static public void CreateActions(Cube cube)  
{  
    #region Adding a drillthrough action  
    // Verify That action exists and drop it  
    if (cube.Actions.ContainsName("My Reseller Details"))  
        cube.Actions.Remove("My Drillthrough Action",true);  
  
    //Create a Drillthrough action  
    DrillThroughAction dtaction = new DrillThroughAction("My Reseller Details", "My Drillthrough Action");  
  
    //Define the Action  
    dtaction.Type = ActionType.DrillThrough;  
    dtaction.TargetType = ActionTargetType.Cells;  
    dtaction.Target = "MeasureGroupMeasures(\"Reseller Sales\")";  
    dtaction.Caption = "My Drillthrough...";  
    dtaction.CaptionIsMdx = false;  
  
    #region create drillthrough action specifics  
    //Adding Drillthrough columns  
    //Adding Measure columns to the drillthrough  
    MeasureGroup mg = cube.MeasureGroups.FindByName("Reseller Sales");  
    MeasureBinding mb1 = new MeasureBinding();  
    mb1.MeasureID = mg.Measures.FindByName( "Reseller Sales Amount").ID;  
    dtaction.Columns.Add(mb1);  
  
    MeasureBinding mb2 = new MeasureBinding();  
    mb2.MeasureID = mg.Measures.FindByName("Reseller Order Quantity").ID;  
    dtaction.Columns.Add(mb2);  
  
    MeasureBinding mb3 = new MeasureBinding();  
    mb3.MeasureID = mg.Measures.FindByName("Reseller Unit Price").ID;  
    dtaction.Columns.Add(mb3);  
  
    //Adding Dimension Columns to the drillthrough  
    CubeAttributeBinding cb1 = new CubeAttributeBinding();  
    cb1.CubeID = cube.ID;  
    cb1.CubeDimensionID = cube.Dimensions.FindByName("Reseller").ID;  
    cb1.AttributeID = "Reseller Name";  
    cb1.Type = AttributeBindingType.All;  
    dtaction.Columns.Add(cb1);  
  
    CubeAttributeBinding cb2 = new CubeAttributeBinding();  
    cb2.CubeID = cube.ID;  
    cb2.CubeDimensionID = cube.Dimensions.FindByName("Product").ID;  
    cb2.AttributeID = "Product Name";  
    cb2.Type = AttributeBindingType.All;  
    dtaction.Columns.Add(cb2);  
    #endregion  
  
    //Add the defined action to the cube  
    cube.Actions.Add(dtaction);  
    #endregion  
  
    #region Adding a Standard action  
    // Verify That action exists and drop it  
    if (cube.Actions.ContainsName("My City Map"))  
        cube.Actions.Remove("My Action", true);  
  
    //Create a Drillthrough action  
    StandardAction stdaction = new StandardAction("My City Map", "My Action");  
  
    //Define the Action  
    stdaction.Type = ActionType.Url;  
    stdaction.TargetType = ActionTargetType.AttributeMembers;  
    stdaction.Target = "[Geography].[City]";  
    stdaction.Caption = "\"My View Map for \" + [Geography].[City].CurrentMember.Member_Caption + \"...\"";  
    stdaction.CaptionIsMdx = true;  
  
    #region create standard action specifics  
    stdaction.Expression = "\"http://maps.msn.com/home.aspx?plce1=\" + " +  
        "[Geography].[City].CurrentMember.Name + \",\" + " +  
        "[Geography].[State-Province].CurrentMember.Name + \",\" + " +  
        "[Geography].[Country].CurrentMember.Name + " +  
        "\"&regn1=\" + " +  
        "Case " +  
            "When [Geography].[Country].CurrentMember Is " +  
                    "[Geography].[Country].&[Australia] " +  
                "Then \"3\" " +  
            "When [Geography].[Country].CurrentMember Is " +  
                    "[Geography].[Country].&[Canada] " +  
                "Or [Geography].[Country].CurrentMember Is " +  
                    "[Geography].[Country].&[United States] " +  
                "Then \"0\" " +  
                "Else \"1\" " +  
        "End ";  
    #endregion  
  
    //Add the defined action to the cube  
    cube.Actions.Add(stdaction);  
  
    #endregion  
  
    #region Adding a Reporting action  
    // Verify That action exists and drop it  
    if (cube.Actions.ContainsName("My Sales Reason Comparisons"))  
        cube.Actions.Remove("My Report Action", true);  
  
    //Create a Report action  
    ReportAction rsaction = new ReportAction("My Sales Reason Comparisonsp", "My Report Action");  
  
    //Define the Action  
    rsaction.Type = ActionType.Report;  
    rsaction.TargetType = ActionTargetType.AttributeMembers;  
    rsaction.Target = "[Product].[Category]";  
    rsaction.Caption = "\"My Sales Reason Comparisons for \" + [Product].[Category].CurrentMember.Member_Caption + \"...\"";  
    rsaction.CaptionIsMdx = true;  
  
    #region create Report action specifics  
    rsaction.ReportServer = "MyRSSamplesServer";  
    rsaction.Path = "ReportServer?/AdventureWorks Sample Reports/Sales Reason Comparisons";  
    rsaction.ReportParameters.Add("ProductCategory", "UrlEscapeFragment( [Product].[Category].CurrentMember.UniqueName )");  
    rsaction.ReportFormatParameters.Add("rs:Command", "Render");  
    rsaction.ReportFormatParameters.Add("rs:Renderer", "HTML5");  
    #endregion  
  
    //Add the defined action to the cube  
    cube.Actions.Add(rsaction);  
  
    #endregion  
}  
```  
  
##  <a name="KPI"></a> Kpi Objects  
 Un indicateur de performance clé (KPI) est un ensemble de calculs associés à un groupe de mesures dans un cube qui servent à évaluer les performances de l'entreprise. Les objets <xref:Microsoft.AnalysisServices.Kpi> peuvent être définis par AMO, mais ils sont utilisés à partir de l'application cliente qui explore les données.  
  
 La création d'un objet <xref:Microsoft.AnalysisServices.Kpi> passe par les étapes suivantes :  
  
1.  Créez l'objet <xref:Microsoft.AnalysisServices.Kpi> et remplissez les attributs de base.  
  
     Voici une liste d'attributs de base : Description, Afficher le dossier, Groupe de mesures associé et Valeur. Afficher le dossier indique à l'application cliente là où l'indicateur de performance clé doit se situer pour que l'utilisateur final puisse le trouver. L'attribut Groupe de mesures associé indique le groupe de mesures dans lequel tous les calculs MDX doivent être mentionnés. L'attribut Valeur présente la valeur réelle de l'indicateur de performance sous forme d'expression MDX.  
  
2.  Définissez les indicateurs de performance clé : Objectif (Goal), État (Status) et Tendance (Trend).  
  
     Les indicateurs sont des expressions MDX dont les valeurs sont normalement comprises entre -1 et 1, mais c'est l'application d'exploration qui définit la plage de valeurs des indicateurs.  
  
3.  Lorsque vous parcourez des indicateurs de performance clés dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], les valeurs inférieures à -1 sont assimilées à -1 et les valeurs supérieures à 1 sont assimilées à 1.  
  
4.  Définissez des images graphiques.  
  
     Les images graphiques sont des valeurs de chaîne utilisées comme référence dans l'application cliente pour identifier l'ensemble d'images qu'il convient d'afficher. Une chaîne d'image graphique définit également le comportement de la fonction d'affichage. En général, la plage est scindée en un nombre impair d'états, de mauvais à bon, et une image du jeu est assignée à chaque état.  
  
     Si vous utilisez [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] pour parcourir vos indicateurs de performance clés, selon les noms, la plage d'indicateurs est scindée en trois ou cinq états. De plus, pour certains noms, la plage est inversée. Autrement dit, -1 correspond à « Bon » et 1 à « Mauvais ». Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], les trois états contenus dans la plage sont les suivants :  
  
    -   Mauvais = -1 à -0,5  
  
    -   OK = -0,4999 à 0,4999  
  
    -   Bon = 0,50 à 1  
  
     Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], les cinq états contenus dans la plage sont les suivants :  
  
    -   Mauvais = -1 à -0,75  
  
    -   Risque = -0,7499 à -0,25  
  
    -   OK = -0,2499 à 0,2499  
  
    -   En hausse = 0,25 à 0,7499  
  
    -   Bon = 0,75 à 1  
  
 Le tableau suivant présente l'utilisation, le nom et le nombre d'états associés à l'image.  
  
|Utilisation de l'image|Nom de l'image|Nombre d'états|  
|-----------------|----------------|----------------------|  
|État|Formes|3|  
|État|Feu de circulation|3|  
|État|Panneaux de signalisation|3|  
|État|Jauge|3|  
|État|Jauge inversée|5|  
|État|Thermomètre|3|  
|État|Cylindre|3|  
|État|Faces|3|  
|État|Flèche de variance|3|  
|Tendance|Flèche standard|3|  
|Tendance|Flèche d'état|3|  
|Tendance|Flèche d'état inversée|5|  
|Tendance|Faces|3|  
  
1.  Ajoutez l'indicateur de performance clé à la collection du cube et mettez ce dernier à jour, car l'indicateur de performance clé est un objet qui ne peut pas être mis à jour.  
  
 Pour tester l'indicateur de performance clé, vous avez besoin d'une application différente. Vous pouvez le tester dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
 L'exemple de code suivant crée un indicateur de performance clé dans le dossier « Financial Perpective/Grow Revenue » du cube Adventure Works inclus dans l'exemple de projet Analysis Services Adventure Works.  
  
```  
static public void CreateKPIs(Cube cube)  
{  
    Kpi kpi = cube.Kpis.Add("My Internet Revenue", "My Internet Revenue");  
    kpi.Description = "(My) Revenue achieved through direct sales via Interner";  
    kpi.DisplayFolder = "\\Financial Perspective\\Grow Revenue";  
    kpi.AssociatedMeasureGroupID = "Internet Sales";  
    kpi.Value = "[Measures].[Internet Sales Amount]";  
    #region Goal  
    kpi.Goal = "Case" +  
               "     When IsEmpty" +  
               "          (" +  
               "            ParallelPeriod" +  
               "            (" +  
               "              [Date].[Fiscal Time].[Fiscal Year]," +  
               "              1," +  
               "              [Date].[Fiscal Time].CurrentMember" +  
               "            )" +  
               "          )" +   
               "     Then [Measures].[Internet Sales Amount]" +  
               "     Else 1.10 *" +  
               "          (" +  
               "            [Measures].[Internet Sales Amount]," +  
               "            ParallelPeriod" +  
               "            (" +  
               "              [Date].[Fiscal Time].[Fiscal Year]," +  
               "              1," +  
               "              [Date].[Fiscal Time].CurrentMember" +  
               "            )" +  
               "          ) " +  
               " End";  
    #endregion  
    #region Status  
    kpi.Status = "Case" +  
                 "   When KpiValue( \"Internet Revenue\" ) / KpiGoal ( \"Internet Revenue\" ) >= .95 " +  
                 "   Then 1 " +  
                 "   When KpiValue( \"Internet Revenue\" ) / KpiGoal ( \"Internet Revenue\" ) <  .95 " +  
                 "        And  " +  
                 "        KpiValue( \"Internet Revenue\" ) / KpiGoal ( \"Internet Revenue\" ) >= .85 " +  
                 "   Then 0 " +  
                 "   Else -1 " +  
                 "End";  
    #endregion  
    #region Trend  
    kpi.Trend = "Case " +  
                "    When VBA!Abs " +  
                "         ( " +  
                "           KpiValue( \"Internet Revenue\" ) -  " +  
                "           ( " +  
                "             KpiValue ( \"Internet Revenue\" ), " +  
                "             ParallelPeriod " +  
                "             (  " +  
                "               [Date].[Fiscal Time].[Fiscal Year], " +  
                "               1, " +  
                "               [Date].[Fiscal Time].CurrentMember " +  
                "             ) " +  
                "           ) / " +  
                "           ( " +  
                "             KpiValue ( \"Internet Revenue\" ), " +  
                "             ParallelPeriod " +  
                "             (  " +  
                "               [Date].[Fiscal Time].[Fiscal Year], " +  
                "               1, " +  
                "               [Date].[Fiscal Time].CurrentMember " +  
                "             ) " +  
                "           )   " +  
                "         ) <=.02  " +  
                "    Then 0 " +  
                "    When KpiValue( \"Internet Revenue\" ) -  " +  
                "         ( " +  
                "           KpiValue ( \"Internet Revenue\" ), " +  
                "           ParallelPeriod " +  
                "           (  " +  
                "             [Date].[Fiscal Time].[Fiscal Year], " +  
                "             1, " +  
                "             [Date].[Fiscal Time].CurrentMember " +  
                "           ) " +  
                "         ) / " +  
                "         ( " +  
                "           KpiValue ( \"Internet Revenue\" ), " +  
                "           ParallelPeriod " +  
                "           (  " +  
                "             [Date].[Fiscal Time].[Fiscal Year], " +  
                "             1, " +  
                "             [Date].[Fiscal Time].CurrentMember " +  
                "           ) " +  
                "         )  >.02 " +  
                "    Then 1 " +  
                "    Else -1 " +  
                "End";  
    #endregion  
    kpi.TrendGraphic = "Standard Arrow";  
    kpi.StatusGraphic = "Cylinder";  
}.  
```  
  
##  <a name="Persp"></a>Objets de perspective  
 Les objets <xref:Microsoft.AnalysisServices.Perspective> peuvent être définis par AMO, mais ils sont utilisés à partir de l'application cliente qui explore les données.  
  
 La création d'un objet <xref:Microsoft.AnalysisServices.Perspective> passe par les étapes suivantes :  
  
1.  Créez l'objet <xref:Microsoft.AnalysisServices.Perspective> et remplissez les attributs de base.  
  
     Voici une liste d'attributs de base : Nom, Mesure par défaut, Description et Annotations.  
  
2.  Ajoutez tous les objets du cube parent à révéler à l'utilisateur final.  
  
     Ajoutez les dimensions du cube (attributs et hiérarchies), les groupes de mesure (mesure et groupe de mesures), les actions, les indicateurs de performance clés et les calculs.  
  
3.  Ajoutez la perspective à la collection du cube et mettez ce dernier à jour, car la perspective est un objet qui ne peut pas être mis à jour.  
  
 Pour tester la perspective, vous avez besoin d'une application différente. Vous pouvez la tester dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
 L'exemple de code suivant crée une perspective nommée « Direct Sales » pour le cube fourni.  
  
```  
static public void CreatePerspectives(Cube cube)  
{  
    Perspective perspective = cube.Perspectives.Add("Direct Sales", "Direct Sales");  
    CubeDimension dim1 = cube.Dimensions.GetByName("Date");  
    PerspectiveDimension pdim1 = perspective.Dimensions.Add(dim1.DimensionID);  
    pdim1.Attributes.Add("Date");  
    pdim1.Attributes.Add("Calendar Year");  
    pdim1.Attributes.Add("Fiscal Year");  
    pdim1.Attributes.Add("Calendar Quarter");  
    pdim1.Attributes.Add("Fiscal Quarter");  
    pdim1.Attributes.Add("Calendar Month Number");  
    pdim1.Attributes.Add("Fiscal Month Number");  
    pdim1.Hierarchies.Add("Calendar Time");  
    pdim1.Hierarchies.Add("Fiscal Time");  
  
    CubeDimension dim2 = cube.Dimensions.GetByName("Product");  
    PerspectiveDimension pdim2 = perspective.Dimensions.Add(dim2.DimensionID);  
    pdim2.Attributes.Add("Product Name");  
    pdim2.Attributes.Add("Product Line");  
    pdim2.Attributes.Add("Model Name");  
    pdim2.Attributes.Add("List Price");  
    pdim2.Attributes.Add("Size");  
    pdim2.Attributes.Add("Weight");  
    pdim2.Hierarchies.Add("Product Model Categories");  
    pdim2.Hierarchies.Add("Product Categories");  
  
    PerspectiveMeasureGroup pmg = perspective.MeasureGroups.Add("Internet Sales");  
    pmg.Measures.Add("Internet Sales Amount");  
    pmg.Measures.Add("Internet Order Quantity");  
    pmg.Measures.Add("Internet Unit Price");  
  
    pmg = perspective.MeasureGroups.Add("Reseller Sales");  
    pmg.Measures.Add("Reseler Sales Amount");  
    pmg.Measures.Add("Reseller Order Quantity");  
    pmg.Measures.Add("Reseller Unit Price");  
  
    PerspectiveAction pact = perspective.Actions.Add("Drillthrough Action");  
  
    PerspectiveKpi pkpi = perspective.Kpis.Add("Internet Revenue");  
    Cube.Update();  
}  
```  
  
##  <a name="PC"></a>Objets ProactiveCaching  
 Les objets <xref:Microsoft.AnalysisServices.ProactiveCaching> peuvent être définis par AMO.  
  
 La création d'un objet <xref:Microsoft.AnalysisServices.ProactiveCaching> passe par les étapes suivantes :  
  
1.  Créez l'objet <xref:Microsoft.AnalysisServices.ProactiveCaching>.  
  
     Il n'y a aucun attribut de base à définir.  
  
2.  Ajoutez des spécifications de cache.  
  
|Spécification| Description|  
|-------------------|-----------------|  
|AggregationStorage|Type de stockage pour les agrégations.<br /><br /> S'applique uniquement aux partitions. Sur la dimension, doit être **Regular.**|  
|SilenceInterval|Durée d'existence minimale du cache avant que ne démarre le processus de création d'images MOLAP.|  
|Latence|Laps de temps qui s'écoule entre la première notification et le moment où les images MOLAP sont détruites.|  
|SilenceOverrideInterval|Laps de temps après une notification initiale à l'issue duquel le processus de création d'images MOLAP entre en action sans condition.|  
|ForceRebuildInterval|Laps de temps (débutant à la suite de la suppression d'une image MOLAP nouvelle) à l'issue duquel le processus de création d'images démarre sans condition (aucune notification).|  
|OnlineMode|Période de disponibilité de l'image MOLAP.<br /><br /> Peut être **Immediate** ou **OnCacheComplete**.|  
  
1.  Ajoutez l'objet <xref:Microsoft.AnalysisServices.ProactiveCaching> à la collection parente. Vous devrez mettre à jour le parent, car <xref:Microsoft.AnalysisServices.ProactiveCaching> est un objet qui ne peut pas être mis à jour.  
  
 L'exemple de code suivant crée un objet <xref:Microsoft.AnalysisServices.ProactiveCaching> dans toutes les partitions à partir du groupe de mesures Internet Sales du cube Adventure Works d'une base de données spécifiée.  
  
```  
static public void SetProactiveCachingSettings(Database db)  
{  
    ProactiveCaching pc;  
    if (db.Cubes.ContainsName("Adventure Works") && db.Cubes.FindByName("Adventure Works").MeasureGroups.ContainsName("Internet Sales"))  
    {  
        ProactiveCachingTablesBinding pctb;  
        TableNotification tn;  
  
        MeasureGroup mg = db.Cubes.FindByName("Adventure Works").MeasureGroups.FindByName("Internet Sales");  
        foreach(Partition part in mg.Partitions)  
        {  
            pc = new ProactiveCaching();  
            pc.AggregationStorage = ProactiveCachingAggregationStorage.MolapOnly;  
            pc.SilenceInterval = TimeSpan.FromSeconds(10);  
            pc.Latency = TimeSpan.FromSeconds(-1);  
            pc.SilenceOverrideInterval = TimeSpan.FromMinutes(10);  
            pc.ForceRebuildInterval = TimeSpan.FromSeconds(-1);  
            pc.Enabled = true;  
            pc.OnlineMode = ProactiveCachingOnlineMode.OnCacheComplete;  
            pctb = new ProactiveCachingTablesBinding();  
            pctb.NotificationTechnique = NotificationTechnique.Server;  
            tn = new TableNotification("[FactInternetSales]", "dbo");  
            pctb.TableNotifications.Add( tn);  
            pc.Source = pctb;  
  
            part.ProactiveCaching = pc;  
            part.Update();  
        }  
    }  
}  
```  
  
##  <a name="Transl"></a>Objets Translation  
 Les objets Translation peuvent être définis par AMO, mais ils sont utilisés à partir de l'application cliente qui explore les données. Les objets Translation sont des objets simples à coder. Les traductions de légendes d'objets sont fournies par les paires Identificateur de paramètres régionaux et Légende traduite. Plusieurs traductions peuvent être activées pour une même légende. Des traductions peuvent être fournies pour la plupart des objets [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], tels que les dimensions, les attributs, les hiérarchies, les cubes, les groupes de mesures, les mesures, etc.  
  
 L'exemple de code suivant fournit une traduction espagnole pour le nom de l'attribut Product Name.  
  
```  
static public void CreateTranslations(Database db)  
{  
    //Spanish Tranlations for Product Category in Product Dimension  
    Dimension dim = db.Dimensions["Product"];  
    DimensionAttribute atr = dim.Attributes["Product Name"];  
    Translation tran = atr.Translations.Add(3082);  
    tran.Caption = "Nombre Producto";  
  
    dim.Update(UpdateOptions.ExpandFull);  
  
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
  
  
