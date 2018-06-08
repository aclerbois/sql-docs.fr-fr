---
title: MSSQLSERVER_1203 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 1203 (Database Engine error)
ms.assetid: 33a35f00-98c8-46c6-b432-544b326b6117
caps.latest.revision: 14
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9c283411d8b2508a931a8fb66873e37985ae86cd
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver1203"></a>MSSQLSERVER_1203
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|1203|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LK_NOT|  
|Texte du message|L'ID de processus %d a essayé de déverrouiller une ressource qu'il ne possède pas :%.*ls. Réessayez la transaction, car cette erreur est peut-être provoquée par une condition basée sur le temps. Si le problème persiste, contactez l'administrateur de base de données.|  
  
## <a name="explanation"></a>Explication  
Cette erreur se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est engagé dans une activité autre que le nettoyage de post-traitement standard et que la page qu'il est en train d'essayer de déverrouiller est déjà déverrouillée.  
  
### <a name="possible-causes"></a>Causes possibles  
La cause sous-jacente de cette erreur peut être liée à des problèmes structurels au sein de la base de données concernée. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère l'acquisition et la libération de pages pour maintenir le contrôle de simultanéité dans l'environnement multi-utilisateur. Ce mécanisme est géré à l'aide de diverses structures de verrous internes qui identifient la page et le type de verrou présent. Les verrous sont acquis pour le traitement des pages concernées, puis libérés une fois le traitement terminé.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Exécutez DBCC CHECKDB sur la base de données à laquelle appartient l'objet. Si DBCC CHECKDB n'indique aucune erreur, essayez de rétablir la connexion et d'exécuter la commande.  
  
> [!IMPORTANT]  
> Si l'exécution de DBCC CHECKDB avec l'une des clauses REPAIR ne résout pas le problème d'index ou si vous ne savez pas quelles conséquences l'exécution de DBCC CHECKDB avec une clause REPAIR peut avoir sur vos données, contactez votre fournisseur d'assistance principal.  
  
