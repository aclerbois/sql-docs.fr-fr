---
title: Créer une requête DMX dans SQL Server Management Studio | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9842dd19b9f613c7b10c86e3596ddda96a99f59b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-dmx-query-in-sql-server-management-studio"></a>Créer une requête DMX dans SQL Server Management Studio
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]fournit un ensemble de fonctionnalités pour vous aider à créer des requêtes de prédiction, les requêtes de contenu et les requêtes de définition de données sur les modèles d’exploration de données et les structures d’exploration de données.  
  
-   Le générateur de requêtes de prédiction graphique est disponible dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]pour simplifier le processus d’écriture des requêtes de prédiction et de mappage de jeux de données à un modèle.  
  
-   Les modèles de requête fournis dans l'Explorateur de modèles accélèrent la création de plusieurs types de requêtes DMX, notamment plusieurs types de requêtes de prédiction. Des modèles sont fournis pour les requêtes de contenu, les requêtes utilisées dans des jeux de données imbriqués, les requêtes qui retournent des cas de structure d'exploration de données et même des requêtes de définition de données.  
  
-   L'explorateur de métadonnées des volets de requête MDX et DMX fournit une liste des modèles et des structures disponibles que vous pouvez faire glisser et déplacer dans le générateur de requêtes, ainsi qu'une liste des fonctions DMX. Cette fonctionnalité simplifie l'obtention des noms corrects d'objets, sans saisie.  
  
 Cette rubrique décrit la génération d'une requête DMX à l'aide de l'explorateur de métadonnées et de l'éditeur de requête DMX.  
  
##  <a name="BKMK_Templates"></a> Modèles de requête DMX  
 Des modèles permettant de créer des requêtes DMX de base sont disponibles dans l'Explorateur de modèles. Le dossier **DMX** contient des modèles d’exploration de données, divisés en catégories comme suit :  
  
-   **Contenu du modèle**  
  
-   **Gestion de modèles**  
  
-   **Requêtes de prédiction**  
  
-   **Structure de contenu**  
  
 Vous pouvez également créer des modèles personnalisés, pour les requêtes ou les commandes que vous exécutez fréquemment.  
  
## <a name="xmla-query-templates"></a>Modèles de requête XMLA  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]fournit également des modèles pour les requêtes XMLA.  
  
 Les types de requêtes se chevauchent selon qu'ils sont effectués à l'aide de XMLA ou DMX. Par exemple, vous pouvez créer des requêtes de contenu de modèle à l'aide de DMX ou des ensembles de lignes de schéma d'exploration de données, mais les ensembles de lignes de schéma contiennent parfois des informations qui ne sont pas exposés dans les requêtes de contenu DMX.  
  
 Il existe également des différences essentielles dans la façon dont les opérations sont gérées dans DMX et dans XMLA. Par exemple, vous pouvez utiliser XMLA pour effectuer des opérations administratives telles que la sauvegarde d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] entière, mais si vous souhaitez sauvegarder un modèle d’exploration de données unique, DMX fournit une commande simple, [EXPORT &#40;DMX&#41;](../../dmx/export-dmx.md), mieux adaptée à cette fin.  
  
##  <a name="BKMK_Building_Queries"></a> Générer et exécuter une requête DMX.  
  
#### <a name="open-a-new-dmx-query-window"></a>Ouvrir une fenêtre de nouvelle requête DMX.  
  
1.  Cliquez sur **Nouvelle requête** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis sélectionnez **Nouvelle requête DMX Analysis Services**.  
  
2.  Lorsque la boîte de dialogue **Se connecter au serveur** s’affiche, sélectionnez l’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui contient les modèles d’exploration de données que vous voulez utiliser.  
  
#### <a name="open-template-explorer"></a>Ouvrir l'Explorateur de modèles  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans le menu **Affichage** , sélectionnez **Explorateur de modèles**.  
  
2.  Cliquez sur **Serveur d’analyse** pour afficher une arborescence des modèles qui s’appliquent à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
#### <a name="apply-a-template-to-build-a-query"></a>Appliquez un modèle pour générer une requête  
  
-   Cliquez avec le bouton droit sur le type de requête approprié et sélectionnez **Ouvrir**.  
  
-   Ou, faites glisser le modèle dans l'éditeur de requête.  
  
-   Vous pouvez également remplir les paramètres de la requête à l’aide de l’option **Spécifier les valeurs pour les paramètres**du menu **Requête** .  
  
 Pour obtenir des exemples de création de types spécifiques de requêtes à partir de modèles, consultez les rubriques suivantes :  
  
 [Créer une requête de prédiction Singleton à partir d’un modèle](../../analysis-services/data-mining/create-a-singleton-prediction-query-from-a-template.md)  
  
 [Créer une requête de contenu sur un modèle d’exploration de données](../../analysis-services/data-mining/create-a-content-query-on-a-mining-model.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Outils de requête d'exploration de données](../../analysis-services/data-mining/data-mining-query-tools.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../../dmx/data-mining-extensions-dmx-reference.md)  
  
  
