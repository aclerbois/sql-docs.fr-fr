---
title: Masquer les objets système dans l’Explorateur d’objets | Microsoft Docs
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
helpviewer_keywords:
- hiding system objects
- system objects [SQL Server]
- objects [SQL Server], hiding
- Object Explorer, hiding objects
ms.assetid: c01d8804-838c-4f75-b78c-80e41e4fffdc
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 106c73db70f12616f85920067863a25c529810a2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hide-system-objects-in-object-explorer"></a>Masquer les objets système dans l’Explorateur d’objets
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Cette rubrique explique comment masquer les objets système dans l'Explorateur d'objets dans [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)]. Le nœud **Bases de données** de l’Explorateur d’objets contient des objets système tels que les bases de données système. Utilisez les pages **Outils**/**Options** pour masquer les objets système. Certains objets système, tels que les fonctions système et les types de données système, ne sont pas affectés par ce paramètre.  
  
## <a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-hide-system-objects-in-object-explorer"></a>Pour masquer les objets système dans l'Explorateur d'objets  
  
1.  Dans le menu **Outils** , cliquez sur **Options**.  
  
2.  Dans la page **Environnement/Démarrage** , sélectionnez **Masquer les objets système dans l’Explorateur d’objets**, puis cliquez sur **OK**.  
  
3.  Dans la boîte de dialogue **SQL Server Management Studio** , cliquez sur **OK** pour que SQL Server Management Studio redémarre et que la modification prenne effet.  
  
4.  Fermez et rouvrez SQL Server Management Studio.  
  
