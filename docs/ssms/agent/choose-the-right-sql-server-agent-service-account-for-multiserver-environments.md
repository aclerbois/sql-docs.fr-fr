---
title: Choisir le compte Agent pour les environnements multiserveurs | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: sql-tools
ms.service: ''
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology:
- tools-ssms
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- SQL Server Agent, service accounts
- multiserver environments [SQL Server], SQL Server Agent service account behavior
ms.assetid: a07e2f38-281c-495b-965b-13fad03ba548
caps.latest.revision: ''
author: stevestein
ms.author: sstein
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 52af160c6296bd049b095df689cc324560dbb27e
ms.sourcegitcommit: 34766933e3832ca36181641db4493a0d2f4d05c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="choose-the-right-sql-server-agent-service-account-for-multiserver-environments"></a>Choisir le compte de service SQL Server Agent correct pour les environnements multiserveurs
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Le compte Windows que vous choisissez pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent peut affecter le comportement d'un environnement multiserveur de la manière suivante :  
  
-   Si vous exécutez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent avec un compte n'appartenant pas au groupe Administrateurs local de Windows, l'inscription des serveurs cibles en serveurs maîtres risque d'échouer. Si tel est le cas, le message d'erreur suivant apparaît :  
  
    « Échec de l'inscription ».  
  
    Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent pour résoudre ce problème.  
  
-   Lorsque vous exécutez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent avec le compte système local, les opérations de serveur maître/serveur cible sont prises en charge uniquement si les serveurs maître et cible résident sur le même ordinateur. Si vous optez pour cette configuration, le message suivant apparaît lorsque vous inscrivez des serveurs cibles sur un serveur maître :  
  
    « Vérifiez que le compte de démarrage de l'agent pour *<nom_ordinateur_serveur_cible>* dispose des autorisations pour se connecter en tant que serveur cible. »  
  
    Vous pouvez ignorer ce message d'information. L'opération d'enregistrement doit se terminer correctement.  
  
Pour plus d’informations sur le choix d’un compte pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent, consultez [Sélectionner un compte pour le service SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md).  
  
