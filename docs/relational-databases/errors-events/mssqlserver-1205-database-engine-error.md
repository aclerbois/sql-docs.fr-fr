---
title: MSSQLSERVER_1205 | Microsoft Docs
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
- 1205 (Database Engine error)
ms.assetid: 9fe3f67c-df3c-4642-a3a4-ccc0e138b632
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 7c9351a3040a2e4f362b00d647a72989a8877973
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="mssqlserver1205"></a>MSSQLSERVER_1205
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|1205|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LK_VICTIM|  
|Texte du message|La transaction (ID de processus %d) a été bloquée sur les ressources %.*ls par un autre processus et a été choisie comme victime. Relancez la transaction.|  
  
## <a name="explanation"></a>Explication  
L'accès aux ressources s'effectue dans un ordre conflictuel sur des transactions distinctes, ce qui cause un interblocage. Exemple :  
  
-   Transaction1 met à jour **Table1.Row1**, tandis que Transaction2 met à jour **Table2.Row2**.  
  
-   Transaction1 essaie de mettre à jour **Table2.Row2** mais est bloquée car Transaction2 n’est pas encore validée.  
  
-   Transaction2 essaie alors de mettre à jour **Table1.Row1** mais est bloquée car Transaction1 n’est pas encore validée.  
  
-   Un blocage survient car Transaction1 attend que Transaction2 se termine, alors que Transaction2 attend que Transaction1 se termine.  
  
Le système détectera ce blocage et choisira l'une des transactions impliquées comme 'victime' et émettra ce message, tout en annulant la transaction de la victime.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Exécutez de nouveau la transaction. Vous pouvez également réviser l'application pour éviter les blocages. La transaction qui a été choisie comme victime peut être retentée et réussira probablement, en fonction des opérations qui sont exécutées simultanément.  
  
Pour empêcher ou éviter l’apparition de blocages, faites en sorte que toutes les transactions accèdent aux lignes dans le même ordre (**Table1**, puis **Table2**) ; de cette manière, bien qu’un blocage puisse survenir, aucun interblocage ne se produira.  
  
