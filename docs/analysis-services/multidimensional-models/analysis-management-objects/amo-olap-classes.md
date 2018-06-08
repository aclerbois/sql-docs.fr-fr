---
title: Des Classes OLAP AMO | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b1d457c26a0b2d33058a5eb496825a6dc03dbc71
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="amo-olap-classes"></a>Classes OLAP AMO
  Les classes OLAP AMO (Analysis Management Objects) permettent de créer, modifier, supprimer et traiter les cubes, les dimensions et les objets connexes tels que les indicateurs de performance clés, les actions et la mise en cache proactive.  
  
 Pour plus d’informations sur la configuration de l’environnement de programmation AMO, comment établir une connexion avec un serveur, l’accès à une base de données ou la définition des données sources et les vues de source de données, consultez [des Classes fondamentales AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-fundamental-classes.md).  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Objets de dimension](#Dimensions)  
  
-   [Objets de cube](#Cubes)  
  
-   [Objets MeasureGroup](#MeasureGroups)  
  
-   [Objets de partition](#Partition)  
  
-   [Objets AggregationDesign](#AggregationDesign)  
  
-   [Objets d’agrégation](#Aggregation)  
  
-   [Objets action](#Action)  
  
-   [Objets KPI](#KPI)  
  
-   [Objets de perspective](#Perspective)  
  
-   [Objets Translation](#Translation)  
  
-   [Objets ProactiveCaching](#ProactiveCaching)  
  
 L'illustration suivante montre la relation qui existe entre les classes décrites dans cette rubrique.  
  
 ![Classes OLAP dans AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/media/amo-olapclasses.gif "Classes OLAP dans AMO")  
  
## <a name="basic-classes"></a>Classes Basic  
  
###  <a name="Dimensions"></a> Objets de dimension  
 Pour créer une dimension, il convient de l'ajouter à la collection de dimensions de la base de données parente, et de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Dimension> sur le serveur à l'aide de la méthode Update.  
  
 Pour supprimer une dimension, il est nécessaire d'utiliser la méthode Drop de l'objet <xref:Microsoft.AnalysisServices.Dimension>. Le fait de supprimer un objet <xref:Microsoft.AnalysisServices.Dimension> de la collection de dimensions de la base de données à l'aide de la méthode Remove n'entraîne pas sa suppression sur le serveur ; il est seulement supprimé dans le modèle objet AMO.  
  
 Un objet <xref:Microsoft.AnalysisServices.Dimension> peut être traité après avoir été créé. L'objet <xref:Microsoft.AnalysisServices.Dimension> peut être traité avec sa propre méthode Process ou avec celle de l'objet parent au moment où celui-ci est traité.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Dimension> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="Cubes"></a> Objets de cube  
 Pour créer un cube, il convient de l'ajouter à la collection de cubes de la base de données parente, puis de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Cube> sur le serveur à l'aide de la méthode Update. La méthode Update du cube peut inclure le paramètre UpdateOptions.ExpandFull, qui garantit que tous les objets du cube qui ont été modifiés seront mis à jour sur le serveur dans le cadre de cette action de mise à jour.  
  
 Pour supprimer un cube, il est nécessaire d'utiliser la méthode Drop de l'objet <xref:Microsoft.AnalysisServices.Cube>. La suppression d'un cube de la collection n'a aucun effet sur le serveur.  
  
 Un objet <xref:Microsoft.AnalysisServices.Cube> peut être traité après avoir été créé. Le <xref:Microsoft.AnalysisServices.Cube> peuvent être traitées à l’aide de sa propre méthode process ou elle peut être traitée lorsqu’un objet parent lui-même traite avec sa propre méthode Process.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Cube> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="MeasureGroups"></a>Objets MeasureGroup  
 Pour créer un groupe de mesures, il convient de l'ajouter à la collection de groupes de mesures du cube, puis de mettre à jour l'objet <xref:Microsoft.AnalysisServices.MeasureGroup> sur le serveur à l'aide de sa propre méthode Update. Pour supprimer un objet <xref:Microsoft.AnalysisServices.MeasureGroup>, il convient d'utiliser sa propre méthode Drop.  
  
 Un objet <xref:Microsoft.AnalysisServices.MeasureGroup> peut être traité après avoir été créé. Le <xref:Microsoft.AnalysisServices.MeasureGroup> peuvent être traités à l’aide de sa propre méthode Process ou elle peut être traitée lorsqu’un objet parent lui-même traite avec sa propre méthode Process.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.MeasureGroup> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="Partition"></a>Objets de partition  
 Pour créer un objet <xref:Microsoft.AnalysisServices.Partition>, il convient de l'ajouter à la collection de partitions du groupe de mesures parent, puis de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Partition> sur le serveur à l'aide de la méthode Update. Pour supprimer un objet <xref:Microsoft.AnalysisServices.Partition>, il convient d'utiliser la méthode Drop.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Partition> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="AggregationDesign"></a>Objets AggregationDesign  
 Les conceptions d'agrégation sont construites à l'aide de la méthode AggregationDesign d'un objet <xref:Microsoft.AnalysisServices.AggregationDesign>.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.AggregationDesign> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="Aggregation"></a>Objets d’agrégation  
 Pour créer un objet <xref:Microsoft.AnalysisServices.Aggregation>, il convient de l'ajouter à la collection de conceptions d'agrégation du groupe de mesures parent, puis de mettre à jour l'objet groupe de mesures parent sur le serveur à l'aide de la méthode Update. Pour supprimer une agrégation de l'objet <xref:Microsoft.AnalysisServices.AggregationCollection>, il est nécessaire d'utiliser la méthode Remove ou la méthode RemoveAt.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Aggregation> dans <xref:Microsoft.AnalysisServices>.  
  
## <a name="advanced-classes"></a>Classes Advanced  
 Les classes Advanced fournissent des fonctionnalités OLAP qui vont au delà de la génération et de l'exploration d'un cube. Voici quelques-unes des classes avancées et des avantages qu'elles procurent :  
  
-   Les classes Action sont utilisées pour créer une réponse active lors de l'exploration de certaines zones du cube.  
  
-   Les indicateurs de performance clés permettent l'analyse comparative de valeurs de données.  
  
-   Les perspectives fournissent des vues sélectionnées d'un cube unique pour permettre aux utilisateurs de se concentrer sur ce qu'ils jugent important.  
  
-   Les traductions permettent de personnaliser un cube en fonction des paramètres régionaux de l'utilisateur.  
  
-   Les classes de mise en cache proactive peuvent offrir un compromis entre les performances améliorées du stockage MOLAP et l'immédiateté du stockage ROLAP. Elles fournissent par ailleurs un traitement planifié des partitions.  
  
 AMO permet de définir ce comportement amélioré, mais l'expérience réelle est définie par le client d'exploration qui implémente toutes ces améliorations.  
  
###  <a name="Action">Objets action</a>  
 Pour créer un objet <xref:Microsoft.AnalysisServices.Action>, il convient de l'ajouter à la collection d'actions du cube, puis de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Cube> sur le serveur à l'aide de la méthode Update. La méthode Update du cube peut inclure le paramètre UpdateOptions.ExpandFull, qui garantit que tous les objets du cube qui ont été modifiés seront mis à jour sur le serveur dans le cadre de cette action de mise à jour.  
  
 Pour supprimer un <xref:Microsoft.AnalysisServices.Action> de l’objet, il doit être supprimé de la collection et le cube parent doit être mis à jour.  
  
 Le cube doit être mis à jour et traité avant que l'action puisse être utilisée à partir du client.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Action> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="KPI"></a> Kpi Objects  
 Pour créer un objet <xref:Microsoft.AnalysisServices.Kpi>, il convient de l'ajouter à la collection d'indicateurs de performance clés du cube, puis de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Cube> sur le serveur à l'aide de la méthode Update. La méthode Update du cube peut inclure le paramètre UpdateOptions.ExpandFull, qui garantit que tous les objets du cube qui ont été modifiés seront mis à jour sur le serveur dans le cadre de cette action de mise à jour.  
  
 Pour supprimer un <xref:Microsoft.AnalysisServices.Kpi> de l’objet, il doit être supprimé de la collection, et le cube parent doit être mis à jour.  
  
 Le cube doit être mis à jour et traité pour que l'indicateur de performance clé puisse être utilisé.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Kpi> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="Perspective"></a>Objets de perspective  
 Pour créer un objet <xref:Microsoft.AnalysisServices.Perspective>, il convient de l'ajouter à la collection de perspectives du cube, puis de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Cube> sur le serveur à l'aide de la méthode Update. La méthode Update du cube peut inclure le paramètre UpdateOptions.ExpandFull, qui garantit que tous les objets du cube qui ont été modifiés seront mis à jour sur le serveur dans le cadre de cette action de mise à jour.  
  
 Pour supprimer un objet <xref:Microsoft.AnalysisServices.Perspective>, il doit être supprimé de la collection et le cube parent doit être mis à jour.  
  
 Un cube doit être mis à jour et traité avant que la perspective ne puisse être utilisée.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Perspective> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="Translation"></a>Objets Translation  
 Pour créer un objet <xref:Microsoft.AnalysisServices.Translation>, il convient de l'ajouter à la collection de traductions de l'objet souhaité, puis de mettre à jour l'objet parent principal le plus proche sur le serveur à l'aide de la méthode Update. La méthode Update de l'objet parent le plus proche peut inclure le paramètre UpdateOptions.ExpandFull, qui garantit que tous les objets enfants qui ont été modifiés seront mis à jour sur le serveur dans le cadre de cette action de mise à jour.  
  
 Pour supprimer un objet <xref:Microsoft.AnalysisServices.Translation>, il doit être supprimé de la collection et l'objet parent le plus proche doit être mis à jour.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Translation> dans <xref:Microsoft.AnalysisServices>.  
  
###  <a name="ProactiveCaching"></a>Objets ProactiveCaching  
 Pour créer un objet <xref:Microsoft.AnalysisServices.ProactiveCaching>, il convient de l'ajouter à la collection d'objets de mise en cache proactive de la dimension ou de la partition, puis de mettre à l'objet dimension ou partition sur le serveur à l'aide de la méthode Update.  
  
 Pour supprimer un objet <xref:Microsoft.AnalysisServices.ProactiveCaching>, il doit être supprimé de la collection et l'objet parent doit être mis à jour.  
  
 La dimension ou la partition doit être mise à jour et traitée pour que la mise en cache proactive soit activée et prête à être utilisée.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.ProactiveCaching> dans <xref:Microsoft.AnalysisServices>.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices>   
 [Présentation des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-classes-introduction.md)   
 [Programmation d’objets de base OLAP AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/programming-amo-olap-basic-objects.md)   
 [Objets OLAP de programmation AMO avancés](../../../analysis-services/multidimensional-models/analysis-management-objects/programming-amo-olap-advanced-objects.md)   
 [Architecture logique & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)  
  
  
