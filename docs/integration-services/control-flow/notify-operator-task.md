---
title: Notifier l’opérateur, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.notifyoperatortask.f1
helpviewer_keywords:
- Notify Operator task
- notifications [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: 6c816c68-c6d6-44e4-bb34-c8e060a958a1
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b7be00181d723d5266450fcc72e2b513774a058c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="notify-operator-task"></a>tâche de notification d'opérateur
  La tâche Notifier l'opérateur envoie des messages de notification aux opérateurs d'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un opérateur d'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un alias d'une personne ou d'un groupe qui peut recevoir des notifications électroniques. Pour plus d'informations sur les opérateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Opérateurs](http://msdn.microsoft.com/library/38e8488f-2669-4cea-b9c3-5f394a663678).  
  
 La tâche Notifier l’opérateur permet à un package de notifier un ou plusieurs opérateurs par e-mail, radiomessagerie ou **envoi réseau**. Chaque opérateur peut être notifié par différentes méthodes. Par exemple, l’opérateur A est notifié par e-mail et radiomessagerie, tandis que l’opérateur B est notifié par radiomessagerie et **envoi réseau**. Les opérateurs qui reçoivent des notifications depuis la tâche de notification d'opérateur doivent être membres de sa collection **OperatorNotify** .  
  
 La tâche Notifier l'opérateur est la seule tâche de maintenance de base de données qui n'encapsule pas d'instruction Transact-SQL ou de commande DBCC.  
  
## <a name="configuration-of-the-notify-operator-task"></a>Configuration de la tâche Notifier l'opérateur  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Cette tâche se trouve dans la section **Tâches du plan de maintenance** de la **boîte à outils** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Tâche Notifier l’opérateur &#40;Plan de maintenance&#41;](../../relational-databases/maintenance-plans/notify-operator-task-maintenance-plan.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
  
