---
title: "Basculer un point d’arrêt | Microsoft Docs"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: sql-tools
ms.service: 
ms.component: ssms-scripting
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: c477ab89-a1cd-4f2c-aa7c-40525041100f
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 43894ff831132deaaa572a931c7b418a62a2d900
ms.sourcegitcommit: a0aa5e611a0e6ebb74ac1e2f613e8916dc7a7617
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="toggle-a-breakpoint"></a>Basculer un point d'arrêt
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)] Le fait de définir un point d’arrêt sur une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] s’appelle le basculement d’un point d’arrêt.  
  
## <a name="breakpoints"></a>Points d’arrêt  
 Une fois le point d'arrêt défini, il est représenté par une icône dans la barre grise à gauche de l'instruction. Cette icône s'appelle un glyphe de point d'arrêt. [!INCLUDE[tsql](../../includes/tsql-md.md)] Les points d’arrêt sont appliqués à une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] complète. Lorsqu'un point d'arrêt est activé, le débogueur met en surbrillance l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] associée.  
  
 S'il existe plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sur une ligne, vous pouvez basculer un point d'arrêt pour chaque instruction. Cliquer dans la barre grise à gauche de la fenêtre bascule un point d'arrêt sur la première instruction de la ligne. Vous pouvez basculer un point d’arrêt dans une instruction suivante en mettant en surbrillance une partie de l’instruction ou en plaçant le curseur dans l’instruction, puis en appuyant sur F9 ou en cliquant sur **Basculer le point d’arrêt** dans le menu **Déboguer** . Si plusieurs points d'arrêt se trouvent sur une ligne, un seul glyphe de point d'arrêt se trouve dans la barre grise à gauche.  
  
 Une fois qu'un point d'arrêt a été basculé, vous pouvez effectuer plusieurs actions sur celui-ci, par exemple modifier ses propriétés ou les désactiver temporairement. Pour plus d’informations, consultez [Points d’arrêt Transact-SQL](../../relational-databases/scripting/transact-sql-breakpoints.md).  
  
## <a name="toggle-a-breakpoint"></a>Basculer un point d'arrêt  
 **Pour basculer un point d'arrêt sur une instruction Transact-SQL**  
  
1.  Cliquez sur la barre grise à gauche de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
2.  Vous pouvez également mettre en surbrillance une partie quelconque de l'instruction ou placer le curseur sur l'instruction, puis effectuer l'une des actions suivantes :  
  
    -   Appuyez sur F9.  
  
    -   Dans le menu **Déboguer** , cliquez sur **Basculer le point d’arrêt**.  
  
  
