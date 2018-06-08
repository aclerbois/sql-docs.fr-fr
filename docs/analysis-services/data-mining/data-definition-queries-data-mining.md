---
title: Requêtes de définition de données (exploration de données) | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 562a3c0fd3b73c60f3e729afe926d61d68e55b1b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-definition-queries-data-mining"></a>Requêtes de définition des données (Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Pour l’exploration de données, la catégorie *requête Définition des données* signifie des instructions DMX ou des commandes XMLA qui effectuent les opérations suivantes :  
  
-   Créer, modifier, ou manipuler des objets d'exploration de données, tels qu'un modèle.  
  
-   Définir la source de données à utiliser dans l'apprentissage ou pour la prédiction.  
  
-   Exporter ou importer des modèles et des structures d'exploration de données.  
  
 [Création de requêtes de définition de données](#bkmk_Create)  
  
-   [Requêtes de définition des données dans les outils de données SQL Server](#bkmk_ssdt)  
  
-   [Requêtes de définition des données dans SQL Server Management Studio](#bkmk_SSMS)  
  
 [Scripts d'instructions de définition des données (DDL)](#bkmk_Scripts)  
  
 [Scripts d'instructions de définition des données (DDL)](#bkmk_Export)  
  
##  <a name="bkmk_Create"></a> Création de requêtes de définition de données  
 Vous pouvez créer des requêtes Définition des données (instructions) à l’aide du Générateur de requêtes de prédiction dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ou à l’aide de la fenêtre de requête DMX dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Les instructions de définition de données dans DMX font partie du langage de définition de données (DDL) d'Analysis Services.  
  
 Pour plus d’informations sur la syntaxe des instructions de définition de données spécifiques, consultez [Guide de référence du langage DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-reference.md).  
  
###  <a name="bkmk_ssdt"></a> Requêtes de définition des données dans les outils de données SQL Server  
 L'Assistant Exploration de données est l'outil par défaut dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour créer et modifier des modèles et des structures d'exploration de données, et pour définir les sources de données utilisées dans les requêtes de prédiction et pour l'apprentissage.  
  
 Toutefois, si vous souhaitez savoir quelles instructions sont envoyées au serveur par l'Assistant pour créer des structures de données ou des modèles d'exploration de données, vous pouvez utiliser SQL Server Profiler pour capturer les instructions de définition de données. Pour plus d’informations, consultez [Utiliser SQL Server Profiler pour contrôler Analysis Services](../../analysis-services/instances/use-sql-server-profiler-to-monitor-analysis-services.md).  
  
 Pour afficher les instructions utilisées pour définir des sources de données pour l’apprentissage ou la prédiction, vous pouvez sélectionner **Vue SQL** dans le Générateur de requêtes de prédiction. Il peut parfois être utile de créer des requêtes de base pour l'apprentissage et le test des modèles à l'aide du Générateur de requêtes de prédiction, afin d'utiliser la syntaxe correcte. Vous pouvez ensuite basculer vers **Vue SQL** et modifier manuellement la requête. Pour plus d’informations, consultez [Modifier manuellement une requête de prédiction](../../analysis-services/data-mining/manually-edit-a-prediction-query.md).  
  
###  <a name="bkmk_SSMS"></a> Requêtes de définition des données dans SQL Server Management Studio  
 Pour les objets d'exploration de données, vous pouvez utiliser des requêtes de définition des données afin d'effectuer les actions suivantes :  
  
-   Créer des types spécifiques de modèles, tels qu’un modèle de clustering ou un modèle d’arbre de décision, à l’aide de [CREATE MINING MODEL &#40;DMX&#41;](../../dmx/create-mining-model-dmx.md).  
  
-   Modifier une structure d’exploration de données existante en ajoutant un modèle ou en modifiant les colonnes, à l’aide de [ALTER MINING STRUCTURE &#40;DMX&#41;](../../dmx/alter-mining-structure-dmx.md). Notez que vous ne pouvez pas modifier un modèle d'exploration de données à l'aide de DMX ; vous ajoutez uniquement de nouveaux modèles à une structure existante.  
  
-   Effectuer une copie d’un modèle d’exploration de données, puis la modifier à l’aide de [SELECT INTO &#40;DMX&#41;](../../dmx/select-into-dmx.md).  
  
-   Définir le jeu de données utilisé pour l’apprentissage d’un modèle, en utilisant [INSERT INTO &#40;DMX&#41;](../../dmx/insert-into-dmx.md) avec une requête de source de données comme OPENROWSET.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fournit les modèles de requête qui peuvent vous aider à créer des requêtes de définition des données. Pour plus d’informations, consultez [Utiliser des modèles Analysis Services dans SQL Server Management Studio](../../analysis-services/instances/use-analysis-services-templates-in-sql-server-management-studio.md).  
  
 En général, les modèles qui sont fournis pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] contiennent uniquement la définition générale de syntaxe, que vous devez personnaliser, soit en tapant dans la fenêtre **Requête** , soit en utilisant la boîte de dialogue fournie pour entrer des paramètres.  
  
 Pour obtenir un exemple de la manière d’entrer des paramètres à l’aide de l’interface, consultez [Créer une requête singleton de prédiction à partir d’un modèle](../../analysis-services/data-mining/create-a-singleton-prediction-query-from-a-template.md).  
  
###  <a name="bkmk_Scripts"></a> Scripts d'instructions de définition des données (DDL)  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit plusieurs scripts et langages de programmation que vous pouvez utiliser pour créer ou modifier des objets d'exploration de données, ou pour définir des sources de données.  Bien que DMX soit conçu pour accélérer les tâches d'exploration de données, vous pouvez également utiliser XMLA et AMO pour manipuler des objets dans les scripts ou dans du code personnalisé.  
  
 Le complément d’exploration de données pour Excel inclut également de nombreux modèles de requête et fournit un **éditeur de requête avancé**, qui vous aide à composer des instructions DMX complexes. Vous pouvez générer une requête de manière interactive, puis basculer vers la vue SQL pour capturer l'instruction DMX.  
  
##  <a name="bkmk_Export"></a> Exportation et importation de modèles  
 Vous pouvez utiliser des instructions de définition de données dans DMX afin d'exporter la définition d'un modèle et sa structure et sources de données requises, puis importer cette définition dans un autre serveur. Le recours à l'exportation et à l'importation est le moyen le plus rapide et le plus simple pour déplacer des modèles et des structures d'exploration de données entre des instances de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [Gestion des solutions et des objets d’exploration de données](../../analysis-services/data-mining/management-of-data-mining-solutions-and-objects.md).  
  
> [!WARNING]  
>  Si votre modèle est basé sur des données d'une source de données de cube, vous ne pouvez pas utiliser DMX pour exporter le modèle et vous devez recourir à la place à la sauvegarde et à la restauration.  
  
##  <a name="bkmk_Tasks"></a> Tâches associées  
 Le tableau suivant fournit des liens vers des tâches liées aux requêtes de définition des données.  
  
|||  
|-|-|  
|Utiliser des modèles pour des requêtes DMX.|[Utiliser des modèles Analysis Services dans SQL Server Management Studio](../../analysis-services/instances/use-analysis-services-templates-in-sql-server-management-studio.md)|  
|Concevoir des requêtes de tous types, à l'aide du Générateur de requêtes de prédiction.|[Créer une requête de prédiction à l’aide du Générateur de requêtes de prédiction](../../analysis-services/data-mining/create-a-prediction-query-using-the-prediction-query-builder.md)|  
|Capturer des définitions de requêtes à l'aide de SQL Server Profiler, puis utiliser les traces pour surveiller [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|[Utiliser SQL Server Profiler pour surveiller Analysis Services](../../analysis-services/instances/use-sql-server-profiler-to-monitor-analysis-services.md)|  
|En savoir plus sur les langages de script et les langages de programmation fournis pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|[XML for Analysis & #40 ; XMLA & #41 ; Référence](../../analysis-services/xmla/xml-for-analysis-xmla-reference.md)<br /><br /> [Développement avec Analysis Management Objects & #40 ; AMO & #41 ;](../../analysis-services/multidimensional-models/analysis-management-objects/developing-with-analysis-management-objects-amo.md)|  
|Apprendre à gérer les modèles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|[Exporter et importer des objets d’exploration de données](../../analysis-services/data-mining/export-and-import-data-mining-objects.md)<br /><br /> [EXPORTATION & #40 ; DMX & #41 ;](../../dmx/export-dmx.md)<br /><br /> [IMPORTATION & #40 ; DMX & #41 ;](../../dmx/import-dmx.md)|  
|En savoir plus sur OPENROWSET et d'autres méthodes pour interroger des données externes.|[&#60;requête de données source&#62;](../../dmx/source-data-query.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Assistant exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/data-mining-wizard-analysis-services-data-mining.md)  
  
  
