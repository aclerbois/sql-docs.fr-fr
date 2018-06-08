---
title: Définir des partitions dans les modèles DirectQuery | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 599e2f97991bc6256132861f335ac7bd0b0aa592
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="define-partitions-in-directquery-models"></a>Définition de partitions dans les modèles DirectQuery
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Cette section explique comment sont utilisées les partitions dans les modèles DirectQuery. Pour plus d’informations sur les partitions dans les modèles tabulaires, consultez [Partitions](../../analysis-services/tabular-models/partitions-ssas-tabular.md).  
  
> [!NOTE]  
>  Bien qu’une table puissent contenir plusieurs partitions, en mode DirectQuery, seul l’une d’entre elles peut être désignée pour être utilisée dans l’exécution des requêtes. La spécification de partition unique s’applique aux modèles DirectQuery à tous les niveaux de compatibilité.  
  
## <a name="using-partitions-in-directquery-mode"></a>Utilisation de partitions en mode DirectQuery  
 Pour chaque table, vous devez spécifier une seule partition à utiliser en tant que source de données DirectQuery.  S'il existe plusieurs partitions, lorsque vous basculez le modèle pour activer le mode DirectQuery, par défaut la première partition qui a été créée dans la table est marquée en tant que partition DirectQuery. Vous pouvez modifier ce comportement ultérieurement à l'aide du Gestionnaire de partitions de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Pourquoi autoriser une seule partition en mode DirectQuery ?  
  
 Dans les modèles tabulaires (comme dans les modèles OLAP), les partitions d'une table sont définies par des requêtes SQL. Le développeur qui crée la définition de partition doit s'assurer que les partitions ne se chevauchent pas. Analysis Services ne vérifie pas si les enregistrements appartiennent à une ou plusieurs partitions.  
  
 Les partitions dans un modèle tabulaire mis en cache se comportent de la même manière. Si vous utilisez un modèle en mémoire, lors de l'accès au cache, les formules DAX sont évaluées pour chaque partition et les résultats sont associés. Toutefois, lorsqu'un modèle tabulaire utilise le mode DirectQuery, il est impossible d'évaluer plusieurs partitions, de combiner les résultats et de les convertir en une instruction SQL pour l'envoi à la banque de données relationnelle. Cela peut entraîner une perte inacceptable de performances, ainsi que des inexactitudes potentielles à mesure que les résultats sont agrégés.  
  
 Par conséquent, pour les requêtes ayant obtenu une réponse en mode DirectQuery, le serveur utilise une seule partition qui a été marquée comme partition principale pour l’accès DirectQuery, que l’on appelle *partition DirectQuery*.  La requête SQL spécifiée dans la définition de cette partition définit l’ensemble complet des données qui peuvent être utilisées pour répondre aux requêtes en mode DirectQuery.  
  
 Si vous ne définissez aucune partition de manière explicite, le moteur transmet simplement une requête SQL à l'ensemble de la source de données relationnelle, effectue toutes les opérations reposant sur un jeu dictées par la formule DAX et retourne les résultats de la requête.  
  
  
## <a name="change-a-directquery-partition"></a>Modification d’une partition DirectQuery  
 Dans la mesure où une seule partition d'une table peut être désignée en tant que partition DirectQuery, par défaut, Analysis Services utilise la première partition qui a été créée dans la table. Pendant la création du projet de modèle, vous pouvez modifier la partition DirectQuery à l'aide de la boîte de dialogue Gestionnaire de partitions dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Pour les modèles déployés, vous pouvez modifier la partition DirectQuery à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="change-the-directquery-partition-for-a-tabular-model-project"></a>Modifier la partition DirectQuery pour un projet de modèle tabulaire  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], dans le générateur de modèles, cliquez sur la table (l'onglet) qui contient la table partitionnée.  
  
2.  Cliquez sur le menu **Table** , puis sur **Partitions**.  
  
3.  Dans le **Gestionnaire de partitions**, la partition correspondant à la partition de requête directe active est indiquée par le préfixe **(DirectQuery)** dans le nom de la partition.  
  
     Sélectionnez une autre partition dans la liste **Partitions** , puis cliquez sur **Définir comme DirectQuery**. Le bouton **Définir comme DirectQuery** n'est pas activé lorsque la partition DirectQuery active est sélectionnée, et n'est pas visible si le modèle n'a pas été activé pour le mode de requête directe.  
  
4.  Si nécessaire, modifiez les options de traitement, puis cliquez sur **OK**.  
  
#### <a name="change-the-directquery-partition-for-a-deployed-tabular-model"></a>Modifier la partition DirectQuery pour un modèle tabulaire déployé  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez la base de données model dans l'Explorateur d'objets.  
  
2.  Développez le nœud **Tables** , cliquez avec le bouton droit sur la table partitionnée, puis sélectionnez **Partitions**.  
  
     La partition qui est indiquée pour être utilisée avec le mode DirectQuery a le préfixe (DirectQuery) sur le nom de partition.  
  
3.  Pour passer à une autre partition, cliquez sur l'icône de la barre d'outils **Requête directe** pour ouvrir la boîte de dialogue **Définir une partition DirectQuery** . L'icône de la barre d'outils DirectQuery n'est pas disponible sur les modèles qui n'ont pas été activés pour la requête directe.  
  
4.  Choisissez une autre partition dans la liste déroulante **Nom de la partition** , puis modifiez les options de traitement sur la partition, si nécessaire.  
  
## <a name="partitions-in-cached-models-and-in-directquery-models"></a>Partitions dans les modèles mis en cache et dans les modèles DirectQuery  
 Lorsque vous configurez une partition DirectQuery, vous devez spécifier les options de traitement pour la partition.  
  
 Il existe deux options de traitement pour la partition DirectQuery. Pour définir cette propriété, utilisez le **Gestionnaire de partitions** de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis sélectionnez la propriété **Option de traitement** . Le tableau suivant répertorie les valeurs de cette propriété, et décrit les effets de chaque valeur lorsqu'elle est associée à la propriété DirectQueryUsage sur la chaîne de connexion :  
  
|Propriété**Chaîne de connexion** |Propriété**Option de traitement** |Remarques|  
|------------------------------------|------------------------------------|-----------|  
|DirectQuery|Ne jamais traiter cette partition|Lorsque le modèle utilise DirectQuery uniquement, le traitement n'est jamais nécessaire.<br /><br /> Dans les modèles hybrides, vous pouvez configurer la partition DirectQuery de façon à ce qu'elle ne soit jamais traitée. Par exemple, si vous effectuez une opération sur un jeu de données très volumineux et vous ne souhaitez pas que les résultats complets soient ajoutés dans le cache, vous pouvez spécifier que la partition DirectQuery inclut l'union des résultats pour toutes les autres partitions dans la table, et ne jamais traiter l'union. Les requêtes transmises à la source relationnelle ne sont pas affectées, et les requêtes sur des données en mémoire cache associent les données des autres partitions.|  
|DataView=Sample<br /><br /> S’applique aux modèles tabulaires à l’aide des exemples de vues de données|Autoriser le traitement de la partition|Si le modèle utilise des exemples de données, vous pouvez traiter la table pour retourner un jeu de données filtré qui fournit des signaux visuels pendant la conception du modèle.|  
|DirectQueryUsage=InMemory With DirectQuery<br /><br /> S’applique aux modèles tabulaires 1100 1103 qui s’exécutent à la fois en mémoire et en mode DirectQuery|Autoriser le traitement de la partition|Si le modèle utilise le mode hybride, vous devez utiliser la même partition pour les requêtes sur la source de données en mémoire et sur la source de données DirectQuery.|  
  
## <a name="see-also"></a>Voir aussi  
 [Partitions](../../analysis-services/tabular-models/partitions-ssas-tabular.md)  
  
  
