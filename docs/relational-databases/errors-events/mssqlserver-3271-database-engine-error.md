---
title: MSSQLSERVER_3271 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 3271 (Database Engine error)
ms.assetid: 21b8de4b-6624-4163-9561-1a6cc8fe3d51
caps.latest.revision: 19
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9f8c1cd40c3e81e71bd71600705cf8a172adfb30
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver3271"></a>MSSQLSERVER_3271
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|3271|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DMPIO_IO_ERROR|  
|Texte du message|Une erreur d'E/S non récupérable s'est produite dans le fichier "%ls" : %ls|  
  
## <a name="explanation"></a>Explication  
Il s'agit d'une erreur générale qui survient lorsque le système d'exploitation déclenche une erreur en effectuant une E/S au cours d'une opération de sauvegarde ou de restauration. La cause la plus courante est simplement que le support de sauvegarde est plein.  
  
L'erreur peut inclure un texte supplémentaire du système d'exploitation indiquant que le disque est plein. Lorsque vous effectuez une opération de sauvegarde ou de restauration à l'aide d'un logiciel tiers de sauvegarde, un message supplémentaire peut s'afficher pour signaler l'échec de la sauvegarde. Il peut se formuler comme suit :  
  
```  
"2005-08-02 16:05:16.04 spid55 Error: 18210, Severity: 16, State: 1.  
 2005-08-02 16:05:16.04 spid55 BackupVirtualDeviceFile  
::RequestDurableMedia: Flush failure on backup device 'VDINULL'.   
Operating system error 995(The I/O operation has been aborted because   
of either a thread exit or an application request.)."  
```  
  
Ceci indique que le logiciel de sauvegarde a demandé l'arrêt de l'opération de sauvegarde ou de restauration.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Effectuez les tâches suivantes comme il convient :  
  
-   Consultez les messages d'erreur du système sous-jacent et de SQL Server qui ont précédé, afin d'identifier la cause de la défaillance.  
  
-   Assurez-vous que le support de sauvegarde et de restauration possède suffisamment d'espace.  
  
-   Corrigez toutes les erreurs déclenchées par le logiciel tiers de sauvegarde et de restauration.  
  
