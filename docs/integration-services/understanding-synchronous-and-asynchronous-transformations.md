---
title: Présentation des transformations synchrones et asynchrones | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- transformations [Integration Services], synchronous and asynchronous
- asynchronous transformations [Integration Services]
- data flow components [Integration Services], synchronous and asynchronous
- synchronous transformations [Integration Services]
ms.assetid: 0bc2bda5-3f8a-49c2-aaf1-01dbe4c3ebba
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 645adce2297c495445b70e727e1a32613d690ad9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-synchronous-and-asynchronous-transformations"></a>Présentation des transformations synchrones et asynchrones
  Pour comprendre la différence entre une transformation synchrone et une transformation asynchrone dans [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], il est plus facile de commencer par examiner une transformation synchrone. Si une transformation synchrone ne répond pas à vos besoins, il est possible que votre conception nécessite une transformation asynchrone.  
  
## <a name="synchronous-transformations"></a>Transformations synchrones  
 Une transformation synchrone traite les lignes entrantes et les transfère dans le flux de données une ligne à la fois. La sortie est synchrone avec l'entrée ; autrement dit, elles se produisent en même temps. Par conséquent, pour traiter une ligne donnée, la transformation n'a pas besoin d'informations sur les autres lignes du jeu de données. Dans l'implémentation réelle, les lignes sont regroupées dans des mémoires tampons lorsqu'elles passent d'un composant à l'autre, mais ces mémoires tampons sont transparentes pour l'utilisateur. Vous pouvez donc considérer que chaque ligne est traitée séparément.  
  
 La transformation de conversion de données est un exemple de transformation synchrone. Pour chaque ligne entrante, elle convertit la valeur dans la colonne spécifiée et transfère la ligne. Chaque opération de conversion discrète est indépendante de toutes les autres lignes dans le jeu de données.  
  
 Au cours d’opérations de script et de programmation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], vous pouvez spécifier une transformation synchrone en recherchant l’ID de l’entrée d’un composant et en l’assignant à la propriété **SynchronousInputID** des sorties du composant. Vous indiquez ainsi au moteur de flux de données de traiter chaque ligne de l'entrée et d'envoyer automatiquement chaque ligne aux sorties spécifiées. Si vous souhaitez diriger chaque ligne vers chaque sortie, vous n'avez pas besoin d'écrire du code supplémentaire pour exporter les données. Si vous utilisez la propriété **ExclusionGroup** pour spécifier que les lignes doivent atteindre un groupe de sorties uniquement, comme dans la transformation de fractionnement conditionnel, vous devez appeler la méthode **DirectRow** afin de sélectionner la destination appropriée pour chaque ligne. Quand vous disposez d’une sortie d’erreur, vous devez appeler **DirectErrorRow** pour que les lignes présentant un problème soient transmises à la sortie d’erreur au lieu de la sortie par défaut.  
  
## <a name="asynchronous-transformations"></a>Transformations asynchrones  
 Vous pouvez décider que votre conception nécessite une transformation asynchrone lorsqu'il n'est pas possible de traiter chaque ligne indépendamment de toutes les autres. En d'autres termes, vous ne pouvez pas transférer chaque ligne dans le flux de données dès lors qu'elle est traitée. Vous devez plutôt exporter les données de façon asynchrone, c'est-à-dire à un autre moment que l'importation. Par exemple, les scénarios suivants requièrent une transformation asynchrone :  
  
-   Le composant doit acquérir plusieurs mémoires tampons de données avant de pouvoir effectuer leur traitement. La transformation de tri est un exemple dans lequel le composant doit traiter l'ensemble des lignes en une seule opération.  
  
-   Le composant doit combiner des lignes provenant de plusieurs entrées. La transformation de fusion est un exemple dans lequel le composant doit examiner plusieurs lignes de chaque entrée avant de les fusionner dans l'ordre de tri.  
  
-   Il n'existe pas de correspondance univoque entre les lignes d'entrée et les lignes de sortie. La transformation d'agrégation est un exemple dans lequel le composant doit ajouter une ligne à la sortie pour contenir les valeurs d'agrégation calculées.  
  
 Au cours d’opérations de script et de programmation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], vous pouvez spécifier une transformation asynchrone en assignant la valeur 0 à la propriété **SynchronousInputID** des sorties du composant. . Vous indiquez ainsi au moteur de flux de données de ne pas envoyer automatiquement chaque ligne aux sorties. Puis, vous devez écrire du code pour envoyer explicitement chaque ligne à la sortie appropriée en l'ajoutant au nouveau tampon de sortie créé pour la sortie d'une transformation asynchrone.  
  
> [!NOTE]  
>  Étant donné qu'un composant source doit également ajouter explicitement chaque ligne qu'il lit à partir de la source de données à ses tampons de sortie, une source ressemble à une transformation avec des sorties asynchrones.  
  
 Il serait également possible de créer une transformation asynchrone qui émule une transformation synchrone en copiant explicitement chaque ligne d'entrée vers la sortie. Cette méthode vous permettrait de renommer des colonnes ou convertir des types ou formats de données. Toutefois, cette méthode dégrade les performances. Vous pouvez parvenir aux mêmes résultats mais en bénéficiant de meilleures performances si vous utilisez des composants Integration Services intégrés, tels que Copie de colonnes ou Conversion de données.  
  
## <a name="see-also"></a> Voir aussi  
 [Création d’une transformation synchrone à l’aide du composant Script](../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)   
 [Création d’une transformation asynchrone à l’aide du composant Script](../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)   
 [Développement d’un composant de transformation personnalisé à sorties synchrones](../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)   
 [Développement d’un composant de transformation personnalisé avec des sorties asynchrones](../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)  
  
  
