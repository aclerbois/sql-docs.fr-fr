---
title: Destination d’apprentissage du modèle d’exploration de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- sql13.dts.designer.dataminingmodeltrainingdest.f1
- sql13.dts.designer.dmmtrainingtransformation.connection.f1
- sql13.dts.designer.dmmtrainingtransformation.columns.f1
helpviewer_keywords:
- destinations [Integration Services], Data Mining Model Training
- Data Mining Model Training destination
- mining models [Analysis Services], training
- training mining models
ms.assetid: 6bc8cbe2-46af-4f7b-93d6-86779313c9d7
caps.latest.revision: 46
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 482e2bd27acedaed01acf4ef3d324af5b7e09b65
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-mining-model-training-destination"></a>Destination d’apprentissage du modèle d’exploration de données
  La destination d'apprentissage du modèle d'exploration de données exerce les modèles d'exploration de données en transmettant les données que la destination reçoit par le biais des algorithmes de modèles d'exploration de données. Plusieurs modèles d'exploration de données peuvent faire l'objet d'un apprentissage par une même destination si les modèles sont construits sur la même structure d'exploration des données. Pour plus d’informations, consultez [Colonnes de structure d’exploration de données](../../analysis-services/data-mining/mining-structure-columns.md) et [Colonnes d’un modèle d’exploration de données](../../analysis-services/data-mining/mining-model-columns.md).  
  
## <a name="configuration-of-the-data-mining-model-training-destination"></a>Configuration de la destination d'apprentissage du modèle d'exploration de données  
 Si une colonne de niveau de cas de la structure cible et les modèles créés sur cette structure ont un contenu de type KEY TIME ou KEY SEQUENCE, les données d'entrée doivent être triées sur cette colonne. Par exemple, les modèles créés à l'aide de l'algorithme Microsoft Time Series utilisent le type de contenu KEY TIME. Si les données d'entrée ne sont pas triées, le traitement du modèle risque d'échouer. Si les données doivent être triées, vous pouvez utiliser une transformation de tri plus haut dans le flux de données afin de trier les données. Ceci ne s'applique pas aux colonnes dont le type de contenu est KEY. Pour plus d’informations, consultez [Types de contenu &#40;exploration de données&#41;](../../analysis-services/data-mining/content-types-data-mining.md) et [Transformation de tri](../../integration-services/data-flow/transformations/sort-transformation.md).  
  
> [!NOTE]  
>  L'entrée de la destination d'apprentissage du modèle d'exploration de données doit être triée. Pour trier les données, vous pouvez inclure une destination de tri en amont de la destination d'apprentissage du modèle d'exploration de données dans le flux de données. Pour plus d’informations, voir [Sort Transformation](../../integration-services/data-flow/transformations/sort-transformation.md).  
  
 La destination comporte une entrée et aucune sortie.  
  
 La destination d’apprentissage du modèle d’exploration de données utilise un gestionnaire de connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour se connecter au projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou à l’instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui contient la structure et les modèles d’exploration de données dont la destination assure l’apprentissage. Pour plus d'informations, consultez [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md).  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées de la destination d'apprentissage du modèle d'exploration de données](../../integration-services/data-flow/data-mining-model-training-destination-custom-properties.md)  
  
 Pour plus d’informations sur la façon de définir les propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="data-mining-model-training-editor-connection-tab"></a>Éditeur d'apprentissage du modèle d'exploration de données (onglet Connexion)
  Utilisez la page **Connexion** de la boîte de dialogue **Éditeur d'apprentissage du modèle d'exploration de données** pour sélectionner un modèle d'exploration de données pour l'apprentissage.  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions**  
 Sélectionnez une connexion dans la liste des connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existantes ou créez une nouvelle connexion [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l’aide du bouton **Nouveau** décrit ci-après.  
  
 **Nouveau**  
 Créez une connexion à l’aide de la boîte de dialogue **Ajout d’un gestionnaire de connexions Analysis Services** .  
  
 **Structure d'exploration de données**  
 Sélectionnez une structure dans la liste de structures d’exploration de données disponibles ou créez une nouvelle structure en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez une structure d’exploration de données et un modèle d’exploration de données à l’aide de **l’Assistant Exploration de données**.  
  
 **Modèles d'exploration de données**  
 Affiche la liste des modèles d'exploration de données associés à la structure d'exploration de données sélectionnée.  
  
## <a name="data-mining-model-training-editor-columns-tab"></a>Éditeur d'apprentissage du modèle d'exploration de données (onglet Colonnes)
  Utilisez la page **Colonnes** de la boîte de dialogue **Éditeur d'apprentissage du modèle d'exploration de données** pour établir une correspondance entre les colonnes d'entrée et les colonnes de la structure d'exploration de données.  
  
## <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Affichez la liste des colonnes d'entrée disponibles. Faites glisser les colonnes d'entrée pour les mapper sur les colonnes de la structure d'exploration de données.  
  
 **Colonnes de structure d'exploration de données**  
 Affiche la liste des colonnes de la structure d'exploration de données. Faites glisser les colonnes de la structure d'exploration de données pour les mapper sur les colonnes d'entrée disponibles.  
  
 **Colonne d'entrée**  
 Affiche les colonnes d'entrée sélectionnées dans le tableau ci-dessus. Pour modifier ou supprimer une sélection de mappage, utilisez la liste **Colonnes d'entrée disponibles**.  
  
 **Colonnes de structure d’exploration de données**  
 Affiche chaque colonne de destination disponible, qu’elle soit mappée ou non.  
  
