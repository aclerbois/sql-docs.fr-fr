---
title: MSSQLSERVER_8966 | Microsoft Docs
ms.custom: 
ms.date: 04/04/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: errors-events
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
helpviewer_keywords:
- 8966 (Database Engine error)
ms.assetid: 6b1150fd-9dfd-4df9-8f08-8eca237667db
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: bb2f83db75535b7960d110a17ef6bbc26dc7f968
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="mssqlserver8966"></a>MSSQLSERVER_8966
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|8966|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC3_FAILED_TO_READ_AND_LATCH_PAGE|  
|Texte du message|Impossible de lire et de verrouiller la page P_ID avec le type de verrou TYPE. Échec de OPERATION.|  
  
## <a name="explanation"></a>Explication  
La lecture de la page a échoué ou un verrou n'a pas pu être pris en compte sur une page PFS ou GAM. Le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut contenir des messages relatifs au dépassement de délai d’attente de verrous ou d’autres messages associés.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Vérifiez les éventuels messages associés dans le journal des erreurs SQL et résolvez ces erreurs.  
  
