---
title: Afficher des informations sur une étape de travail | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- displaying job step information
- jobs [SQL Server Agent], viewing
- SQL Server Agent jobs, viewing
- viewing job step information
ms.assetid: e3f06492-dc86-4e06-b186-ea58aff6d591
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 01888485033e81c1fa625778f7bca987523af365
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-job-step-information"></a>View Job Step Information
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique décrit comment afficher les détails d'une étape de travail dans la boîte de dialogue Propriétés de l'étape du travail. Elle contient également des informations sur l'affichage de la sortie d'une étape de travail.  
  
-   **Avant de commencer :**  
  
    [Limitations et restrictions](#Restrictions)  
  
    [Sécurité](#Security)  
  
-   **Pour afficher des informations sur une étape de travail, utilisez :**  
  
    [SQL Server Management Studio](#SSMS)  
  
## <a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="Restrictions"></a>Limitations et restrictions  
Si l'étape du travail a été configurée pour écrire sa sortie dans une table ou un fichier et si le travail a été exécuté au moins une fois, vous pouvez afficher sa sortie dans la page **Avancé** de la boîte de dialogue **Propriétés de l'étape du travail** . Lorsqu'un travail ou une étape de travail est supprimé, le journal de sortie est automatiquement supprimé.  
  
### <a name="Security"></a>Sécurité  
  
#### <a name="Permissions"></a>Permissions  
Affichez uniquement les travaux dont vous êtes propriétaire, à moins que vous ne soyez membre du rôle de serveur fixe **sysadmin** . Les membres de ce rôle peuvent afficher tous les travaux et tous les détails d'une étape de travail.  
  
## <a name="SSMS"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-job-step-information"></a>Pour afficher des informations sur une étape de travail  
  
1.  Dans l' **Explorateur d'objets** , connectez-vous à une instance du moteur de base de données [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]et développez-la.  
  
2.  Développez **Agent SQL Server**et **Travaux**, cliquez avec le bouton droit sur le travail contenant l’étape à modifier, puis cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés du travail** , cliquez sur l'onglet **Étapes** .  
  
4.  Cliquez sur l'étape de travail à afficher, puis sur **Modifier**.  
  
5.  Dans la page **Général** de la boîte de dialogue **Propriétés de l'étape du travail** , vous pouvez afficher le type d'étape de travail et sa fonction.  
  
6.  Cliquez sur la page **Avancé** pour afficher les mesures prises par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] si l'étape du travail réussit ou échoue, le nombre de fois où l'étape du travail doit être tentée, l'emplacement où est écrite la sortie de l'étape du travail et l'utilisateur pour lequel elle est exécutée.  
  
#### <a name="to-view-job-step-output"></a>Pour afficher la sortie d'une étape de travail  
  
1.  Dans la boîte de dialogue **Propriétés de l'étape du travail** , cliquez sur la page **Avancé** .  
  
2.  Selon la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] à laquelle vous êtes connecté, vous pouvez afficher le fichier ou la table de sortie de l'étape de travail de la manière suivante :  
  
    -   Si vous êtes connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou une version ultérieure, vous pouvez cliquer sur **Afficher** uniquement lorsque la case à cocher **Enregistrer un journal dans la table** est activée. Dans ce cas, la sortie de la table de travail est écrite dans la table **sysjobstepslogs** de la base de données **msdb** .  
  
    -   Le bouton **Afficher** est désactivé lorsque le résultat de l'étape de travail est écrit dans un fichier. Pour afficher le fichier de sortie d'une étape de travail, utilisez le Bloc-notes.  
  
