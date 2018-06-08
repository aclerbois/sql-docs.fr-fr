---
title: Partitions de modèles tabulaires | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 55e056a0703e9f81d02138f2942d4782d81ee9b2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tabular-model-partitions"></a>Partitions de modèle tabulaire 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les partitions divisent une table en sections logiques. Chaque partition peut ensuite être traitée (actualisée) indépendamment d'autres partitions. Les partitions définies pour un modèle au cours de la création de modèles sont dupliquées dans un modèle déployé. Une fois le déploiement terminé, vous pouvez gérer ces partitions et créer de nouvelles partitions à l'aide de la boîte de dialogue **Partitions** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou à l'aide d'un script. Les informations fournies dans cette rubrique décrivent des partitions dans une base de données de modèle tabulaire déployée. Pour plus d’informations sur la création et la gestion des partitions pendant la création du modèle, consultez [Partitions](../../analysis-services/tabular-models/partitions-ssas-tabular.md).  
  
 Sections de cette rubrique :  
  
-   [Avantages](#bkmk_benefits)  
  
-   [Permissions](#bkmk_permissions)  
  
-   [Traiter les partitions](#bkmk_process_partitions)  
  
-   [Traitement parallèle](#bkmk_parallelProc)  
  
-   [Tâches associées](#bkmk_related_tasks)  
  
##  <a name="bkmk_benefits"></a> Avantages  
 La création de modèles efficaces fait appel à des partitions permettant d'éliminer tout traitement inutile et la charge qui en résulte au niveau du processeur sur les serveurs Analysis Services, tout en veillant en même temps à ce que les données soient traitées et actualisées suffisamment souvent pour refléter les données les plus récentes des sources de données.  
  
 Par exemple, un modèle tabulaire peut avoir une table des ventes qui inclut des données de ventes pour l'exercice fiscal 2011 et chacun des exercices précédents. La table des ventes du modèle possède les trois partitions suivantes :  
  
|Partition|Données de|  
|---------------|---------------|  
|Sales2011|Exercice fiscal en cours|  
|Sales2010-2001|Exercices fiscaux 2001, 2002, 2003, 2004, 2005, 2006. 2007, 2008, 2009, 2010|  
|SalesOld|Tous les exercices fiscaux antérieurs aux dix dernières années.|  
  
 À mesure que de nouvelles données de ventes sont ajoutées pour l'exercice fiscal actuel 2011, ces données doivent être traitées quotidiennement afin de se refléter exactement dans l'analyse de données de ventes de l'exercice fiscal en cours. Par conséquent, la partition Sales2011 est traitée de nuit.  
  
 Il est inutile de traiter les données de la partition Sales2010-2001 de nuit ; toutefois, étant donné que les chiffres des ventes pour les dix exercices fiscaux précédents peuvent néanmoins occasionnellement changer en raison de retours de produit et d'autres ajustements, ils doivent toujours être traités régulièrement. De ce fait, les données de la partition Sales2010-2001 sont traitées sur une base mensuelle. Les données de la partition SalesOld ne changent jamais ; par conséquent, elles sont traitées une fois par an uniquement.  
  
 À l'approche de l'exercice fiscal 2012, une nouvelle partition Sales2012 est ajoutée à la table des ventes du mode. La partition Sales2011 peut ensuite être fusionnée avec la partition Sales2010-2001 et renommée en Sales2011-2002. Les données de l'exercice fiscal 2001 sont éliminées de la nouvelle partition Sales2011-2002 et déplacées vers la partition SalesOld. Toutes les partitions sont ensuite traitées pour refléter les modifications.  
  
 La procédure d'implémentation d'une stratégie de partition pour les modèles tabulaires de votre organisation dépend principalement de vos besoins et de vos ressources disponibles pour le traitement des données de modèles.  
  
##  <a name="bkmk_permissions"></a> Permissions  
 Pour créer, gérer et traiter des partitions dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous devez disposer des autorisations appropriées pour Analysis Services définies dans un rôle de sécurité. Chaque rôle de sécurité dispose d'une des autorisations suivantes :  
  
|Autorisation|Actions|  
|----------------|-------------|  
|Administrateur|Lire, traiter, créer, copier, fusionner, supprimer|  
|Traiter|Lire, traiter|  
|Lecture seule|Lecture|  
  
 Pour en savoir plus sur la création de rôles pendant la création du modèle à l’aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], consultez [rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md). Pour en savoir plus sur la gestion des membres du rôle de déployé les rôles de modèle tabulaire à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], consultez [des rôles de modèle tabulaire](../../analysis-services/tabular-models/tabular-model-roles-ssas-tabular.md).  
  
##  <a name="bkmk_parallelProc"></a> Traitement parallèle  
Analysis Services inclut le traitement parallèle pour les tables avec deux partitions ou plus, l’augmentation des performances de traitement. Il n’y a pas de paramètres de configuration pour le traitement parallèle (voir les remarques). Ce type de traitement se produit par défaut lorsque vous traitez une table ou sélectionnez plusieurs partitions pour la même table et le même processus. Vous pouvez toujours choisir de traiter les partitions d’une table de manière indépendante.  
  
> [!NOTE]  
>  Pour spécifier si les opérations d’actualisation s’exécutent séquentiellement ou en parallèle, vous pouvez utiliser l’option de propriété **maxParallism** avec la [commande Sequence (TMSL)](../../analysis-services/tabular-models-scripting-language-commands/sequence-command-tmsl.md).

> [!NOTE]  
>  Si un nouvel encodage est détecté, le traitement parallèle peut entraîner une augmentation du taux d’utilisation des ressources système. En effet, les opérations de partition multiples doivent être interrompues, puis redémarrées, le nouvel encodage étant en parallèle.  
  
##  <a name="bkmk_process_partitions"></a> Traiter les partitions  
 Les partitions peuvent être traitées (actualisées) indépendamment d’autres partitions à l’aide de la boîte de dialogue **Partitions** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou à l’aide d’un script. Les options suivantes sont disponibles pour le traitement :  
  
|Mode|Description|  
|----------|-----------------|  
|Traiter par défaut|Détecte l'état de traitement d'un objet de partition et effectue le traitement nécessaire pour faire passer les objets de partition non traités ou traités partiellement dans un état de traitement complet. Les données des partitions et des tables vides sont chargées ; les hiérarchies, les colonnes calculées et les relations sont créées ou reconstruites.|  
|Traiter entièrement|Traite un objet de partition et tous les objets qu'il contient. Lorsque la commande Traiter entièrement est exécutée pour un objet qui a déjà été traité, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] supprime toutes les données de l'objet, puis traite l'objet. Ce type de traitement est obligatoire lorsqu'une modification structurelle a été apportée à un objet.|  
|Traiter les données|Chargez les données dans une partition ou une table sans reconstruire les hiérarchies ou les relations ni recalculer les colonnes calculées et les mesures.|  
|Traiter l'effacement|Supprime toutes les données d'une partition.|  
|Traiter l'ajout|Mise à jour incrémentielle de la partition avec de nouvelles données.|  
  
##  <a name="bkmk_related_tasks"></a> Tâches associées  
  
|Tâche| Description|  
|----------|-----------------|  
|[Créer et gérer des partitions de modèles tabulaires](../../analysis-services/tabular-models/create-and-manage-tabular-model-partitions-ssas-tabular.md)|Explique comment créer et gérer des partitions dans un modèle tabulaire déployé à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].|  
|[Traiter les partitions de modèles tabulaires](../../analysis-services/tabular-models/process-tabular-model-partitions-ssas-tabular.md)|Explique comment traiter des partitions dans un modèle tabulaire déployé à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].|  
  
  
