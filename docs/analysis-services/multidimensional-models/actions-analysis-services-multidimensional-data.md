---
title: Actions (Analysis Services - données multidimensionnelles) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b7c4d8781aa316ab49fb3730dd783c77e2776441
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="actions-analysis-services---multidimensional-data"></a>Actions (Analysis Services - Données multidimensionnelles)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les actions peuvent être de types différents et doivent être créées en conséquence. Les actions peuvent être :  
  
-   Des actions d'extraction, qui retournent l'ensemble de lignes qui représente les données sous-jacentes des cellules sélectionnées du cube où l'action se produit.  
  
-   Des actions de rapport, qui retournent un rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] associé à la section sélectionnée du cube où l'action se produit.  
  
-   Des actions standard, qui retournent l'élément d'action (URL, HTML, DataSet, ensemble de lignes et autres éléments) associé à la section sélectionnée du cube où l'action se produit.  
  
 Une interface de requête, telle qu'ADOMD.NET, est utilisée par l'application cliente pour récupérer et exposer les actions à l'utilisateur final. Pour plus d’informations, consultez [Développement avec ADOMD.NET](../../analysis-services/multidimensional-models/adomd-net/developing-with-adomd-net.md).  
  
 Un objet <xref:Microsoft.AnalysisServices.Action> simple est composé des informations de base, de la cible où l’action doit se produire, d’une condition pour limiter la portée de l’action et du type. Les informations de base incluent le nom de l'action, la description de l'action, la légende suggérée pour l'action, et d'autres informations.  
  
 La cible est l'emplacement réel dans le cube où l'action doit se produire. La cible est composée d'un type de cible et d'un objet cible. Le type de cible représente le type d'objet, dans le cube, où l'action sera activée. Les types de cible peuvent être des membres de niveau, des cellules, une hiérarchie, des membres de hiérarchie, ou d'autres éléments. L'objet cible est un objet spécifique au type de cible ; si le type de cible est une hiérarchie, l'objet cible est n'importe laquelle des hiérarchies définies dans le cube.  
  
 La condition est une expression MDX **Boolean** évaluée à l’événement d’action. Si la condition prend la valeur **true**, l’action est exécutée. Sinon, l'action n'est pas exécutée.  
  
 Le type est la nature de l'action à exécuter. <xref:Microsoft.AnalysisServices.Action> est une classe abstraite. Par conséquent, pour l’utiliser, vous devez utiliser l’une des classes dérivées. Deux types d'actions sont prédéfinis : extraction et rapport. Ces messages ont des classes dérivées correspondantes : <xref:Microsoft.AnalysisServices.DrillThroughAction> et <xref:Microsoft.AnalysisServices.ReportAction>. D’autres actions sont couvertes dans la classe <xref:Microsoft.AnalysisServices.StandardAction> .  
  
 Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], une action est une instruction MDX stockée qui peut être présentée à des applications clientes et employée par ces applications. En d'autres termes, une action est une commande de client qui est définie et stockée sur le serveur. Une action contient également des informations qui spécifient comment l'application cliente doit afficher et traiter l'instruction MDX et à quel moment. L'opération spécifiée par l'action peut démarrer une application en utilisant les informations de l'action comme paramètre ou elle peut extraire les informations en fonction des critères fournis par l'action.  
  
 Les actions permettent aux utilisateurs professionnels d'agir dès la parution des résultats de leurs analyses. En enregistrant et en réutilisant les actions, les utilisateurs finaux peuvent aller au-delà de l'analyse traditionnelle, qui se termine généralement par la présentation des données, et lancer des solutions aux problèmes et aux carences découverts, ce qui permet d'étendre l'application de décisionnel au-delà du cube. Dépassant le stade de l'outil complexe de présentation des données, les applications clientes peuvent, grâce aux actions, devenir des éléments à part entière du système opérationnel de l'entreprise. Au lieu de se concentrer sur la transmission des données en entrée à des applications opérationnelles, les utilisateurs finaux peuvent « boucler la boucle » dans le processus décisionnel. Cette capacité à transformer des données analytiques en décisions est cruciale pour le succès de l'application de décisionnel.  
  
 Par exemple, un utilisateur professionnel remarque, en explorant un cube, que le niveau actuel du stock d'un certain produit est bas. L'application cliente fournit à l'utilisateur professionnel une liste d'actions, ayant en commun la rupture de stock, qui sont extraites de la base de données Analysis Services. L'utilisateur professionnel sélectionne l'action Commande pour le membre du cube qui représente le produit. Cette action déclenche une nouvelle commande en appelant une procédure stockée dans la base de données opérationnelle. Cette procédure stockée génère les informations appropriées à transmettre au système de traitement des commandes.  
  
 La création d'actions autorise une certaine souplesse : par exemple, une action peut lancer une application ou récupérer des informations dans une base de données. Vous pouvez configurer une action qui est déclenchée depuis presque n'importe quelle partie d'un cube, en particulier les dimensions, les niveaux, les membres et les cellules, ou créer plusieurs actions pour la même partie d'un cube. Vous pouvez également transmettre des paramètres de format chaîne aux applications lancées et spécifier les légendes affichées pour les utilisateurs finaux tandis que l'action s'exécute.  
  
> [!IMPORTANT]  
>  Pour qu'un utilisateur professionnel puisse utiliser des actions, il est nécessaire que son application cliente prenne en charge les actions.  
  
## <a name="types-of-actions"></a>Types d'actions  
 Le tableau suivant répertorie les types d’actions inclus dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]:  
  
|Type d’action|Description|  
|-----------------|-----------------|  
|CommandLine|Exécute une commande à partir de l'invite de commandes.|  
|Dataset|Renvoie un groupe de données à une application cliente.|  
|Extraction|Retourne une instruction d'extraction comme expression que le client exécute pour retourner un ensemble de lignes.|  
|Html|Exécute un script HTML dans un navigateur Internet.|  
|Propriétaire|Effectue une opération en utilisant une interface différente de celles répertoriées dans ce tableau.|  
|Rapport|Soumet une demande paramétrable basée sur une URL à un serveur de rapports et renvoie un rapport à une application cliente.|  
|Ensemble de lignes|Renvoie un ensemble de lignes à une application cliente.|  
|Instruction|Exécute une commande OLE DB.|  
|URL|Affiche une page web dynamique dans un navigateur Internet.|  
  
## <a name="resolving-and-executing-actions"></a>Résolution et exécution des actions  
 Lorsqu'un utilisateur professionnel accède à l'objet pour lequel l'objet de commande est défini, l'instruction associée à l'action est résolue automatiquement, ce qui la met à disposition de l'application cliente, mais l'action n'est pas exécutée automatiquement. L'action n'est exécutée que lorsque l'utilisateur professionnel réalise l'opération spécifique au client qui lance l'action. Par exemple, les applications clientes peuvent afficher une liste des actions sous forme de menu contextuel lorsque l'utilisateur professionnel clique avec le bouton droit sur un membre ou une cellule spécifique.  
  
## <a name="see-also"></a>Voir aussi  
 [Actions dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/actions-in-multidimensional-models.md)  
  
  
