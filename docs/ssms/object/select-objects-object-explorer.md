---
title: Sélectionner des objets (Explorateur d’objets) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-objects
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.common.selectobjects.f1
ms.assetid: 692477fe-dd7c-403d-acd2-bb108b6077f1
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 57d472c063bd4b2cf697186323942842f8d037c0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="select-objects-object-explorer"></a>Sélectionner des objets (Explorateur d'objets)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
La boîte de dialogue **Sélectionner des objets** vous permet d’ajouter un objet dans une liste d’une autre boîte de dialogue. Le titre de la boîte de dialogue et les options disponibles dans cette boîte de dialogue dépendent de la manière dont elle a été ouverte. Seules les options disponibles apparaissent ; par exemple, seuls des noms de connexion sont disponibles lorsque vous sélectionnez un propriétaire pour un nouvel objet.  
  
## <a name="options"></a>Options  
**Sélectionnez ces types d'objets**  
Affiche la liste des types auxquels les objets à sélectionner appartiennent. Les types incluent les principaux et éléments sécurisables de niveau de base de données et de niveau [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] . Cette zone est remplie à partir des sélections effectuées dans la boîte de dialogue **Sélectionner les types d’objets** , accessible à l’aide du bouton **Types d’objets** .  
  
**Entrez les noms d'objets à sélectionner**  
Entrez la liste séparée par des points-virgules des noms des objets à sélectionner. Les objets à sélectionner doivent être d’un type répertorié dans la zone **Sélectionnez ces types d’objets** . Les objets peuvent être sélectionnés dans une liste accessible en cliquant sur le bouton **Parcourir** .  
  
**Types d'objets**  
Affiche une liste de types d'objets. Sélectionnez un ou plusieurs types en activant les cases à cocher correspondantes.  
  
**Vérifier les noms**  
Valide les noms d’objets dans la zone **Entrez les noms d’objets à sélectionner** . Si un nom d’objet non valide est répertorié, la boîte de dialogue **Nom introuvable** s’affiche. Cette boîte de dialogue permet de corriger ou supprimer le nom dans la liste des objets à sélectionner.  
  
**Parcourir**  
Affiche la boîte de dialogue **Rechercher des objets** . Celle-ci contient la liste des objets des types répertoriés dans la zone **Sélectionnez ces types d’objets** . Sélectionnez des objets dans cette liste en activant les cases à cocher correspondantes.  
  
