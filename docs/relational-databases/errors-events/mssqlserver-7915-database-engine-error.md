---
title: MSSQLSERVER_7915 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 7915 (Database Engine error)
ms.assetid: 63338587-7dd3-40e6-b70e-d8ae18fff47b
caps.latest.revision: 16
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 21c43e914c933463028e6815f698ee6c1c71a5de
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver7915"></a>MSSQLSERVER_7915
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|7915|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC2_REPAIR_IAM_CHAIN_REBUILT|  
|Texte du message|Réparation : la chaîne IAM pour l'ID d'objet O_ID, ID d'index I_ID, ID de partition PN_ID, ID d'unité d'allocation A_ID (type TYPE), a été tronquée avant la page P_ID et sera reconstruite.|  
  
## <a name="explanation"></a>Explication  
Ceci est un message d'information envoyé par REPAIR, qui indique que la chaîne IAM (Index Allocation Map) spécifiée a été corrigée afin de pouvoir être reconstruite. Cette correction peut avoir requis l'allocation d'une nouvelle tête de la chaîne IAM ou la suppression de pages incorrectes de la chaîne.  
  
## <a name="user-action"></a>Action de l'utilisateur  
None  
  
