---
title: Transformer des données avec des transformations | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data flow [Integration Services], transformations
- transformations [Integration Services], about transformations
- transforming data [Integration Services]
ms.assetid: e1340b6f-ef75-4b14-af6f-823586eff0ed
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a255a5cc948afcd71fa121be26653c587a433889
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transform-data-with-transformations"></a>Transformer des données avec des transformations
  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] inclut trois types de composants de flux de données : les sources, les transformations et les destinations.  
  
 Le schéma suivant illustre un flux de données simple qui possède une source, deux transformations et une destination.  
  
 ![Data flow](../../../integration-services/data-flow/media/mw-dts-08.gif "Data flow")  
  
 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] Les transformations procurent la fonctionnalité suivante :  
  
-   Fractionnement, copie et fusion d'ensembles de lignes et opérations de recherche.  
  
-   Mise à jour de valeurs de colonnes et création de colonnes en appliquant des transformations telles que la modification de minuscules en majuscules.  
  
-   Exécution d'opérations Business Intelligence telles que le nettoyage de données, l'exploration de texte et l'exécution de requêtes de prédictions d'exploration de données.  
  
-   Création d'ensembles de lignes constitués de valeurs agrégées ou triées, d'échantillons de données ou de données croisées dynamiques et non croisées dynamiques.  
  
-   Exécution de tâches telles que l'exportation et l'importation de données, l'apport d'informations d'audit et l'utilisation de dimensions à variation lente.  
  
 Pour plus d’informations, consultez [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md).  
  
 Vous pouvez également écrire des transformations personnalisées. Pour plus d’informations, consultez [Développement d’un composant de flux de données personnalisé](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) et [Développement de types spécifiques de composants de flux de données](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-specific-types-of-data-flow-components.md).  
  
 Après avoir ajouté la transformation au concepteur de flux de données, mais avant de configurer la transformation, vous devez connecter la transformation au flux de données en connectant la sortie d'une autre transformation ou source du flux de données à l'entrée de cette transformation. Le connecteur entre ces deux composants de flux de données porte le nom de chemin d'accès. Pour plus d’informations sur la connexion des composants et l’utilisation des chemins d’accès, consultez [Connecter des composants avec des chemins d’accès](http://msdn.microsoft.com/library/05633e4c-1370-4b05-802b-f36b07dd71c8).  
  
### <a name="to-add-a-transformation-to-a-data-flow"></a>Pour ajouter une transformation à un flux de données  
  
-   [Ajouter ou supprimer un composant dans un flux de données](../../../integration-services/data-flow/add-or-delete-a-component-in-a-data-flow.md)  
  
### <a name="to-connect-a-transformation-to-a-data-flow"></a>Pour connecter une transformation à un flux de données  
  
-   [Connecter des composants dans un flux de données](../../../integration-services/data-flow/connect-components-in-a-data-flow.md)  
  
### <a name="to-set-the-properties-of-a-transformation"></a>Pour définir les propriétés d'une transformation  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="see-also"></a> Voir aussi  
 [tâche de flux de données](../../../integration-services/control-flow/data-flow-task.md)   
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Connecter des composants avec des chemins](http://msdn.microsoft.com/library/05633e4c-1370-4b05-802b-f36b07dd71c8)   
 [Gestion des erreurs dans les données](../../../integration-services/data-flow/error-handling-in-data.md)   
 [Flux de données](../../../integration-services/data-flow/data-flow.md)  
  
  
