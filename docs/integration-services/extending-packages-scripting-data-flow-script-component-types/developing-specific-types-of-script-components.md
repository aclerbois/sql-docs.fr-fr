---
title: Développement de types spécifiques de composants Script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting-data-flow-script-component-types
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- Script component [Integration Services], examples
ms.assetid: dfbbe959-6b4e-4b47-b9dd-bcc31929482d
caps.latest.revision: 13
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c0616bc2d80157b0e411bd929cba0836ebd84a33
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="developing-specific-types-of-script-components"></a>Développement de types spécifiques de composants Script
  Le composant Script est un outil configurable que vous pouvez utiliser dans le flux de données d'un package pour remplir presque toutes les conditions qui ne sont pas satisfaites par les sources, les transformations et les destinations incluses dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Cette section contient des exemples de code du composant Script qui illustrent les quatre options de configuration du composant Script :  
  
-   en tant que source ;  
  
-   en tant que transformation à sorties synchrones ;  
  
-   en tant que transformation à sorties asynchrones ;  
  
-   en tant que destination.  
  
 Pour obtenir des exemples supplémentaires du composant Script, consultez [Exemples supplémentaires du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/additional-script-component-examples.md).  
  
## <a name="in-this-section"></a>Dans cette section  
 [Création d’une source à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-source-with-the-script-component.md)  
 Explique et montre comment créer une source de flux de données en utilisant le composant Script.  
  
 [Création d’une transformation synchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
 Explique et montre comment créer une transformation de flux de données à sorties synchrones en utilisant le composant Script. Ce type de transformation modifie des lignes de données en place lorsqu'elles franchissent le composant.  
  
 [Création d’une transformation asynchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
 Explique et montre comment créer une transformation de flux de données à sorties asynchrones en utilisant le composant Script. Ce type de transformation doit lire toutes les lignes de données avant de pouvoir ajouter d'autres informations, telles que des agrégats calculés, aux données qui franchissent le composant.  
  
 [Création d’une destination à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)  
 Explique et montre comment créer une destination de flux de données en utilisant le composant Script.  
  
## <a name="see-also"></a> Voir aussi  
 [Comparaison des solutions de script et des objets personnalisés](../../integration-services/extending-packages-scripting/comparing-scripting-solutions-and-custom-objects.md)   
 [Développement de types spécifiques de composants de flux de données](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-specific-types-of-data-flow-components.md)  
  
  
