---
title: Modèle objet et les Concepts AMO | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8f3baad2b8d105c8c23e7f69d487ce2e1f88f3ff
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="amo-concepts-and-object-model"></a>Concepts et modèle objet AMO
  Cette rubrique fournit une définition d’objets AMO (Analysis Management), comment AMO est lié à d’autres outils et les bibliothèques fournies dans l’architecture de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]et une explication conceptuelle de tous les principaux objets AMO.  
  
 AMO est une collection complète de classes de gestion pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], qui peut être utilisée par programme, dans le cadre de l'espace de noms <xref:Microsoft.AnalysisServices>, dans un environnement managé. Les classes sont incluses dans le fichier AnalysisServices.dll, qui se trouve habituellement où le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] le programme d’installation installe les fichiers, sous le dossier \100\SDK\Assemblies\\. Pour utiliser les classes AMO, vous devez inclure une référence à cet assembly dans vos projets.  
  
 En utilisant AMO, vous êtes en mesure de créer, modifier et supprimer des objets tels que des cubes, dimensions, les structures d’exploration de données, et [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] bases de données ; sur tous ces objets, actions peuvent être effectuées à partir de votre application dans le .NET Framework. Vous pouvez également traiter et mettre à jour les informations stockées dans les bases de données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
 Avec AMO, vous ne pouvez pas interroger vos données. Pour interroger vos données, utilisez [développement avec ADOMD.NET](../../../analysis-services/multidimensional-models/adomd-net/developing-with-adomd-net.md).  
  
 Cette rubrique contient les sections suivantes :  
  
 [AMO dans l’Architecture Analysis Services](#AMOintheAnalysisServicesArchitecture)  
  
 [Architecture AMO](#AMOArchitecture)  
  
 [À l’aide d’AMO](#bkmk_UsingAMO)  
  
 [Automatisation des tâches administratives avec AMO](#AutomatingAdministrativeTaskswithAMO)  
  
##  <a name="AMOintheAnalysisServicesArchitecture"></a> AMO dans l’Architecture Analysis Services  
 De par sa conception, AMO se prédestine uniquement à la gestion d'objets et non à l'interrogation de données. Si l’utilisateur a besoin de requête [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] de données à partir d’une application cliente, l’application cliente doit utiliser [développement avec ADOMD.NET](../../../analysis-services/multidimensional-models/adomd-net/developing-with-adomd-net.md).  
  
##  <a name="AMOArchitecture"></a> Architecture AMO  
 AMO est une bibliothèque complète de classes conçue pour gérer une instance de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] à partir d’une application cliente en code managé sous la version 2.0 du .NET Framework.  
  
 La bibliothèque de classes AMO est conçue comme une hiérarchie de classes dans laquelle certaines classes doivent être instanciées avant d'autres pour pouvoir être utilisées dans votre code. Il existe également des classes auxiliaires qui peuvent être instanciées à tout moment dans votre code, mais il est probable que vous aurez instancié une ou plusieurs classes de la hiérarchie avant d'utiliser une classe auxiliaire.  
  
 L'illustration suivante présente une vue détaillée de la hiérarchie AMO qui comprend les classes principales. L'illustration montre la position des classes par rapport à leurs conteneurs et à leurs homologues. Comme vous pouvez le constater, <xref:Microsoft.AnalysisServices.Dimension> appartient à <xref:Microsoft.AnalysisServices.Database> et à <xref:Microsoft.AnalysisServices.Server>, et peut être créé en même temps que <xref:Microsoft.AnalysisServices.DataSource> et que <xref:Microsoft.AnalysisServices.MiningStructure>. Certaines classes homologues doivent être instanciées pour pouvoir en utiliser d'autres. Par exemple, vous devez créer une instance de <xref:Microsoft.AnalysisServices.DataSource> pour pouvoir ajouter une nouvelle classe <xref:Microsoft.AnalysisServices.Dimension> ou <xref:Microsoft.AnalysisServices.MiningStructure>.  
  
 ![Vue d’ensemble des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/media/amo-highlevelview-majorobjectshighlighted.gif "vue d’ensemble des Classes AMO")  
  
 A *objet principal* est une classe qui représente un objet complet, comme une entité entière et non comme une partie d’un autre objet. Les objets <xref:Microsoft.AnalysisServices.Server>, <xref:Microsoft.AnalysisServices.Cube>, <xref:Microsoft.AnalysisServices.Dimension> et <xref:Microsoft.AnalysisServices.MiningStructure> sont des objets principaux en ce sens qu'ils constituent des entités à eux seuls. Toutefois, un objet <xref:Microsoft.AnalysisServices.Level> n'est pas un objet principal, car il est une partie constituante d'un objet <xref:Microsoft.AnalysisServices.Dimension>. Les objets principaux peuvent être créés, supprimés, modifiés ou traités indépendamment des autres objets. Les objets secondaires sont des objets qui ne peuvent être créés que dans le cadre de la création de l'objet principal parent. Les objets secondaires sont généralement créés lors de la création d'un objet principal. Les valeurs des objets secondaires doivent être définies au moment de leur création, car aucune d'entre elles n'est créée par défaut.  
  
 L'illustration suivante montre les objets principaux contenus dans un objet <xref:Microsoft.AnalysisServices.Server>.  
  
 ![Principaux objets AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/media/amo-majorobjects.gif "principaux objets AMO")  
  
 ![Principaux objets AMO mis en surbrillance (2)](../../../analysis-services/multidimensional-models/analysis-management-objects/media/amo-majorobjects-02.gif "principaux objets AMO mis en surbrillance (2)")  
  
 Dans le cas de la programmation avec AMO, l'association entre les classes et les classes contenues utilise des attributs de type de collection tels que, par exemple, <xref:Microsoft.AnalysisServices.Server> et <xref:Microsoft.AnalysisServices.Dimension>. Pour utiliser une instance d'une classe contenue, vous devez d'abord acquérir une référence à un objet collection qui détient ou peut détenir la classe contenue. Vous devez ensuite trouver l'objet spécifique que vous recherchez dans la collection avant de pouvoir obtenir une référence à l'objet et commencer à l'utiliser.  
  
### <a name="amo-classes"></a>Classes AMO  
 AMO est une bibliothèque de classes conçue pour gérer une instance d'[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] à partir d'une application cliente. La bibliothèque AMO peut s'envisager comme des groupes d'objets liés par une relation logique qui sont utilisés pour accomplir une tâche spécifique. Les classes AMO peuvent être rangées dans les catégories suivantes :  
  
|Ensemble de classes|Fonction|  
|---------------|-------------|  
|[Classes fondamentales AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-fundamental-classes.md)|Classes nécessaires à l'utilisation de tout autre ensemble de classes.|  
|[Classes OLAP AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-olap-classes.md)|Classes permettant de gérer les objets OLAP dans [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].|  
|[Classes d’exploration de données AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-data-mining-classes.md)|Classes permettant de gérer les objets d'exploration de données dans [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].|  
|[Classes de sécurité AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-security-classes.md)|Classes permettant de contrôler l'accès aux autres objets et de maintenir la sécurité.|  
|[Autres classes et méthodes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-other-classes-and-methods.md)|Classes et méthodes permettant aux administrateurs OLAP ou d'exploration de données de réaliser leurs tâches quotidiennes.|  
  
##  <a name="bkmk_UsingAMO"></a>À l’aide d’AMO  
 AMO s'avère particulièrement utile pour automatiser les tâches répétitives, telles que la création de nouvelles partitions dans un groupe de mesures sur la base de nouvelles données dans la table de faits, ou le réapprentissage d'un modèle d'exploration de données sur la base de nouvelles données. Ces tâches qui créent de nouveaux objets sont généralement effectuées tous les mois, toutes les semaines ou tous les trimestres, et les nouveaux objets peuvent être facilement nommés par l'application sur la base des nouvelles données.  
  
##### <a name="analysis-services-administrators"></a>Administrateurs Analysis Services  
 Les administrateurs [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] peuvent utiliser AMO pour automatiser le traitement de bases de données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Pour concevoir et déployer des bases de données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], vous devez utiliser [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
##### <a name="developers"></a>Développeurs  
 Les développeurs peuvent utiliser AMO afin de développer des interfaces d'administration pour des ensembles d'utilisateurs spécifiés. Ces interfaces peuvent limiter l'accès aux objets [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] et cantonner les utilisateurs à certaines tâches. Par exemple, en utilisant AMO, vous pouvez créer une application de sauvegarde qui permette à un utilisateur d'afficher tous les objets de base de données, de sélectionner n'importe quelle base de données et de la sauvegarder sur un jeu de périphériques spécifié.  
  
 Les développeurs peuvent également incorporer la logique [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] dans leurs applications. Pour cela, les développeurs peuvent créer des cubes, des dimensions, des structures d'exploration de données et des modèles d'exploration de données sur la base d'entrées utilisateur ou d'autres facteurs.  
  
##### <a name="olap-advanced-users"></a>Utilisateurs expérimentés d'OLAP  
 Les utilisateurs expérimentés d'OLAP sont généralement des analystes de données ou d'autres utilisateurs de données expérimentés qui possèdent une grande expérience en matière de programmation et qui souhaitent améliorer leur analyse de données à travers une utilisation plus rapprochée des objets de données. Pour les utilisateurs contraints de travailler hors connexion, AMO peut s'avérer très utile pour automatiser la création de cubes locaux avant de se déconnecter.  
  
##### <a name="data-mining-advanced-users"></a>Utilisateurs expérimentés de l'exploration de données  
 Pour les utilisateurs expérimentés de l'exploration de données, AMO est particulièrement utile lorsque des ensembles importants de modèles doivent faire l'objet d'un réapprentissage régulier.  
  
##  <a name="AutomatingAdministrativeTaskswithAMO"></a>Automatisation des tâches administratives avec AMO  
 La plupart des tâches répétitives sont conçues, déployées et gérées dans de meilleures conditions si elles sont développées à l'aide d'[!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] que si elles sont développées en tant qu'application dans un langage de votre choix. Toutefois, pour les tâches répétitives qui ne peuvent pas être automatisées à l'aide d'[!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], vous pouvez utiliser AMO. AMO s'avère également utile lorsque vous souhaitez développer une application spécialisée décisionnelle au moyen d'[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
##### <a name="automatic-object-management"></a>Gestion automatique des objets  
 Avec AMO, il est très facile de créer, mettre à jour ou supprimer des objets [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] (par exemple, <xref:Microsoft.AnalysisServices.Database>, <xref:Microsoft.AnalysisServices.Dimension>, <xref:Microsoft.AnalysisServices.Cube>, <xref:Microsoft.AnalysisServices.MiningStructure> et <xref:Microsoft.AnalysisServices.MiningModel> ou <xref:Microsoft.AnalysisServices.Role>) sur la base des entrées utilisateur ou de nouvelles données acquises. AMO est idéal pour les applications d'installation chargées de déployer une solution développée, d'un éditeur de logiciels indépendant vers un client final. L'application d'installation est en mesure de vérifier s'il existe une version antérieure, de mettre à jour la structure, de supprimer les objets devenus superflus et d'en créer de nouveaux. En l'absence d'une version antérieure, elle peut tout créer entièrement.  
  
 AMO peut s'avérer très efficace pour créer de nouvelles partitions basées sur de nouvelles données et supprimer celles, anciennes, qui avaient dépassé l'étendue du projet. Par exemple, pour une solution d'analyse financière qui utilise les données des 36 derniers mois, dès que les données d'un nouveau mois sont reçues, les données du mois le plus antérieur mois peuvent être supprimées. Pour optimiser les performances, de nouvelles agrégations peuvent être conçues en fonction de l'utilisation et être appliquées aux 12 derniers mois.  
  
##### <a name="automatic-object-processing"></a>Traitement automatique des objets  
 Le traitement d'objets et la mise à jour de la disponibilité peuvent être accomplis en utilisant AMO pour répondre à certains événements au delà des données de flux ordinaires et des tâches planifiées qui utilisent [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
##### <a name="automatic-security-management"></a>Gestion automatique de la sécurité  
 La gestion de la sécurité peut être automatisée pour inclure de nouveaux utilisateurs à des rôles et des autorisations ou pour supprimer d'autres utilisateurs dès que leur délai a expiré. De nouvelles interfaces peuvent être créées pour simplifier la gestion de la sécurité pour les administrateurs de la sécurité. Cela peut s'avérer plus simple que d'utiliser [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
##### <a name="automatic-backup-management"></a>Gestion automatique de la sauvegarde  
 La gestion automatique de la sauvegarde peut être assurée en utilisant des tâches [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] ou en créant des applications AMO spécialisées qui s'exécutent automatiquement. En utilisant AMO, vous pouvez développer des interfaces de sauvegarde pour aider les opérateurs dans leurs travaux quotidiens.  
  
##### <a name="tasks-amo-is-not-intended-for"></a>Tâches pour lesquelles AMO n'est pas prévu  
 AMO ne peut pas être utilisé pour interroger les données. Pour interroger des données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], y compris les cubes et les modèles d'exploration de données, utilisez ADOMD.NET à partir d'une application utilisateur. Pour plus d’informations, consultez [développement avec ADOMD.NET](../../../analysis-services/multidimensional-models/adomd-net/developing-with-adomd-net.md).  
  
  
