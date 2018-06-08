---
title: Réutiliser des objets de packages | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- GUID regenerating [Integration Services]
- reusing packages
- templates [Integration Services]
- copying packages
- regenerating package GUID
ms.assetid: 08f723bf-15b5-44bd-9a46-04e8781bfbfb
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d29d76507844756bc979a82bf28c138c6c49ec83
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reuse-of-package-objects"></a>Réutiliser des objets de packages
  Fonctionnalités usuelles de packages que vous souhaitez réutiliser. Si vous avez par exemple créé un ensemble de tâches, il peut être utile de réutiliser des éléments rassemblés en groupe, ou un élément unique, tel qu'un gestionnaire de connexions créé dans un autre projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
## <a name="copy-and-paste"></a>Copier et coller  
 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] et le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] prennent en charge la copie et le collage d'objets de packages, tels que des éléments de flux de contrôle, des éléments de flux de données et des gestionnaires de connexions. Vous pouvez effectuer la copie et le collage entre des projets et entre des packages. Si la solution contient plusieurs projets, vous pouvez effectuer la copie entre des projets, lesquels peuvent être de types différents.  
  
 Si une solution contient plusieurs packages, vous pouvez effectuer la copie et le collage entre eux. Les packages peuvent se trouver dans des projets [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] différents ou de même nature. Les objets de packages peuvent toutefois avoir des dépendances avec d'autres objets sans lesquels ils ne sont pas valides. Par exemple, une tâche d'exécution SQL utilise un gestionnaire de connexions que vous devez également copier pour faire fonctionner la tâche. Certains objets de packages nécessitent également que le package contienne déjà un objet particulier, sans lequel il n'est pas possible de coller convenablement les objets copiés dans un package. Par exemple, il n'est pas possible de coller un flux de données dans un package qui ne possède pas au moins une tâche de flux de données.  
  
 Il peut arriver que vous deviez copier les mêmes packages plusieurs fois. Pour éviter le processus de copie, vous pouvez désigner ces packages en tant que modèles et vous en servir pour créer des packages.  
  
 Quand vous copiez un objet de package, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] attribue automatiquement un nouveau GUID à la propriété **ID** du nouvel objet et met à jour la propriété **Name** .  
  
 Vous ne pouvez pas copier de variables. Si un objet, une tâche par exemple, utilise des variables, vous devez recréer les variables dans le package de destination. À l'inverse, si vous copiez le package complet, les variables du package sont également copiées.  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Copier des objets de packages](../integration-services/copy-package-objects.md)  
  
-   [Copier des éléments de projet](http://msdn.microsoft.com/library/1606c54d-20f9-49f3-a4ef-caad83a772aa)  
  
-   [Enregistrer un package en tant que modèle de package](http://msdn.microsoft.com/library/efe66cec-3933-4f6e-8d35-fe3d300de66c)  
  
  
