---
title: Octroi de l’accès à une base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
helpviewer_keywords:
- database access
ms.assetid: 686edfe2-3650-48a6-a2da-9d46fa211ad8
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 40071e1d42cbc8906084bab62f625516df653335
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-2---granting-access-to-a-database"></a>Leçon 2-2 - Octroi de l’accès à une base de données
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]
Mary a désormais accès à cette instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], mais elle n'est pas autorisée à accéder aux bases de données. Elle ne peut pas non plus accéder à sa base de données par défaut **TestData** tant que vous ne l'avez pas autorisée en tant qu'utilisateur de base de données.  
  
Pour octroyer l'accès à Mary, basculez vers la base de données **TestData** , et à l'aide de l'instruction CREATE USER mappez sa connexion à un utilisateur appelée Marie.  
  
### <a name="to-create-a-user-in-a-database"></a>Pour créer un utilisateur dans une base de données  
  
1.  Tapez et exécutez les instructions suivantes ( `computer_name` est remplacé par le nom de votre ordinateur) pour octroyer à `Mary` l'accès à la base de données `TestData` .  
  
    ```  
    USE [TestData];  
    GO  
  
    CREATE USER [Mary] FOR LOGIN [computer_name\Mary];  
    GO  
  
    ```  
  
    Mary a désormais accès à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et à la base de données `TestData` .  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Création des vues et des procédures stockées](../t-sql/lesson-2-3-creating-views-and-stored-procedures.md)  
  
  
  
