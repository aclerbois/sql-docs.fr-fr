---
title: Audit, transformation | Microsoft Docs
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
- sql13.dts.designer.audittrans.f1
- sql13.dts.designer.audittransformation.f1
helpviewer_keywords:
- environment data in packages [Integration Services]
- Audit transformation
ms.assetid: 8c143682-9c81-4150-83d6-1d9678151d37
caps.latest.revision: 46
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a7479bfd0d246bf3e70f7dc65a089844362f9b6e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="audit-transformation"></a>transformation d'audit
  La transformation d'audit permet au flux de données d'un package de contenir des données relatives à l'environnement d'exécution du package. Par exemple, le nom du package, de l'ordinateur et de l'opérateur peuvent être ajoutés au flux de données. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] comprend des variables système qui fournissent ces informations.  
  
## <a name="system-variables"></a>Variables système  
 Le tableau suivant décrit les variables système utilisables par la transformation d'audit.  
  
|Variable système|Index|Description|  
|---------------------|-----------|-----------------|  
|**ExecutionInstanceGUID**|0|GUID identifiant l'instance d'exécution du package.|  
|**PackageID**| 1|Identificateur unique du package.|  
|**PackageName**|2|Nom du package.|  
|**VersionID**|3|Version du package.|  
|**ExecutionStartTime**|4|Heure à laquelle l'exécution du package a commencé.|  
|**MachineName**|5|Nom de l'ordinateur.|  
|**UserName**|6|Nom de connexion de la personne qui a démarré le package.|  
|**TaskName**|7|Nom de la tâche de flux de données à laquelle la transformation d'audit est associée.|  
|**TaskId**|8|Identificateur unique de la tâche de flux de données.|  
  
## <a name="configuration-of-the-audit-transformation"></a>Configuration de la transformation d'audit  
 Pour configurer la transformation d'audit, vous devez indiquer le nom d'une nouvelle colonne de sortie à ajouter à la sortie de la transformation, puis mapper la variable système avec la colonne de sortie. Vous pouvez mapper une même variable système avec plusieurs colonnes.  
  
 Cette transformation a une entrée et une sortie. Elle ne prend pas en charge de sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d’informations sur la définition des propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="audit-transformation-editor"></a>Éditeur de transformation d'audit
  La transformation d'audit permet au flux de données d'un package de contenir des données relatives à l'environnement d'exécution du package. Par exemple, le nom du package, de l'ordinateur et de l'opérateur peuvent être ajoutés au flux de données. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] comprend des variables système qui fournissent ces informations.  
  
### <a name="options"></a>Options  
 **Nom de colonne de sortie**  
 Indique le nom de la nouvelle colonne de sortie qui va contenir les informations d'audit.  
  
 **Type d'audit**  
 Sélectionne une variable système disponible pour fournir les informations d'audit.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**GUID d'instance d'exécution**|Insérez le GUID qui identifie de manière unique l'instance d'exécution du package.|  
|**ID du package**|Insérez le GUID qui identifie de manière unique le package.|  
|**Nom du package**|Insérez le nom du package.|  
|**ID de version**|Insérez le GUID qui identifie de manière unique la version du package.|  
|**Heure de début de l'exécution**|Insérez l'heure à laquelle l'exécution du package a commencé.|  
|**Nom de l'ordinateur**|Insérez le nom de l'ordinateur sur lequel le package a été lancé.|  
|**User name**|Insérez le nom de connexion de l'utilisateur qui a lancé le package.|  
|**Nom de la tâche**|Insérez le nom de la tâche de flux de données à laquelle la transformation d'audit est associée.|  
|**ID de la tâche**|Insérez le GUID qui identifie de manière unique le flux de données à laquelle la transformation d'audit est associée.|  
  
  
