---
title: Ouvrir la Visionneuse du fichier journal | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Log File Viewer, opening
ms.assetid: a86b89cb-0432-4648-895a-05ecc5450e45
caps.latest.revision: 29
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 19dfb08572ae79a3ca2d82609641426791957924
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="open-log-file-viewer"></a>Ouvrir la Visionneuse du fichier journal
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez utiliser la Visionneuse du fichier journal dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour accéder aux informations sur les erreurs et événements capturés dans les journaux suivants :  
  
-   Collection d'audits  
  
-   Collecte de données  
  
-   Messagerie de base de données  
  
-   Historique des travaux  
  
-   SQL Server  
  
-   Agent SQL Server  
  
-   Événements Windows (ces événements Windows sont également accessibles à partir de l'observateur d'événements).  
  
 Depuis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], vous pouvez utiliser les serveurs inscrits pour consulter les fichiers journaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] depuis des instances locales ou distantes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En utilisant les serveurs inscrits, vous pouvez consulter les fichiers journaux lorsque les instances sont ou en ligne ou hors connexion. Pour plus d’informations sur l’accès en ligne, consultez la procédure « Pour consulter des fichiers journaux en ligne à partir des serveurs inscrits », plus loin dans cette rubrique. Pour plus d’informations sur les modalités d’accès aux fichiers journaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hors connexion, consultez [Afficher les fichiers journaux hors connexion](../../relational-databases/logs/view-offline-log-files.md).  
  
 Vous pouvez ouvrir la Visionneuse du fichier journal de différentes façons, selon les informations que vous souhaitez consulter.  
  
##  <a name="BeforeYouBegin"></a> Autorisations  
 Pour accéder aux fichiers journaux pour les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont en ligne, l’appartenance au rôle serveur fixe securityadmin est requise.  
  
 L’accès aux fichiers journaux des instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hors connexion nécessite un accès en lecture à l’espace de noms WMI **Root\Microsoft\SqlServer\ComputerManagement10** et au dossier où sont stockés les fichiers journaux. Pour plus d’informations, consultez la section Sécurité de la rubrique [Afficher les fichiers journaux hors connexion](../../relational-databases/logs/view-offline-log-files.md).  
  
### <a name="security"></a>Sécurité  
 Nécessite l'appartenance en tant que membre au rôle serveur fixe securityadmin.  
  
### <a name="view-log-files"></a>Afficher les fichiers journaux  
  
##### <a name="to-view-logs-that-are-related-to-general-sql-server-activity"></a>Pour afficher les journaux relatifs à l'activité générale de SQL Server  
  
1.  Dans l’Explorateur d’objets, développez **Gestion**.  
  
2.  Effectuez l'une des opérations suivantes :  
  
    -   Cliquez avec le bouton droit sur **Journaux SQL Server**, pointez sur **Afficher**, puis cliquez sur **Journal SQL Server** ou sur **Journal de SQL Server et de Windows**.  
  
    -   Développez **Journaux SQL Server**, cliquez avec le bouton droit sur un fichier journal, puis cliquez sur **Afficher le journal SQL Server**. Vous pouvez également double-cliquer sur un fichier journal.  
  
     Les journaux incluent **Messagerie de base de données**, **SQL Server**, **SQL Server Agent**et **Windows NT**.  
  
##### <a name="to-view-logs-that-are-related-to-jobs"></a>Pour afficher les journaux relatifs aux travaux  
  
-   Dans l’Explorateur d’objets, développez **SQL Server Agent**, cliquez avec le bouton droit sur **Travaux**, puis cliquez sur **Afficher l’historique**.  
  
     Les journaux incluent **Messagerie de base de données**, **Historique des travaux**et **SQL Server Agent**.  
  
##### <a name="to-view-logs-that-are-related-to-maintenance-plans"></a>Pour afficher les journaux relatifs aux plans de maintenance  
  
-   Dans l’Explorateur d’objets, développez **Gestion**, cliquez avec le bouton droit sur **Plans de maintenance**, puis cliquez sur **Afficher l’historique**.  
  
     Les journaux incluent **Messagerie de base de données**, **Historique du travail**, **Plans de maintenance**, **Plans de maintenance distants**et **SQL Server Agent**.  
  
##### <a name="to-view-logs-that-are-related-to-data-collection"></a>Pour afficher les journaux relatifs à la collecte de données  
  
-   Dans l’Explorateur d’objets, développez **Gestion**, cliquez avec le bouton droit sur **Collecte de données**, puis cliquez sur **Afficher les journaux**.  
  
     Les journaux incluent **Collecte de données**, **Historique du travail**et **SQL Server Agent**.  
  
##### <a name="to-view-logs-that-are-related-to-database-mail"></a>Pour afficher les journaux relatifs à la messagerie de base de données  
  
-   Dans l’Explorateur d’objets, développez **Gestion**, cliquez avec le bouton droit sur **Messagerie de base de données**, puis cliquez sur **Afficher le journal de la messagerie de base de données**.  
  
     Les journaux incluent **Messagerie de base de données, Historique du travail**, **Plans de maintenance**, **Plans de maintenance distants**, **SQL Server**, **SQL Server Agent**et **Windows NT**.  
  
##### <a name="to-view-logs-that-are-related-to-audits-collections"></a>Pour afficher les journaux relatifs aux collections d'audits  
  
-   Dans l’Explorateur d’objets, développez **Sécurité**et **Audits**, cliquez avec le bouton droit sur un audit, puis sélectionnez **Afficher les journaux d’audit**.  
  
     Les journaux incluent **Collection d’audits** et **Windows NT**.  
  
##### <a name="to-view-logs-that-are-related-to-audits-collections"></a>Pour afficher les journaux relatifs aux collections d'audits  
  
-   Dans l’Explorateur d’objets, développez **Sécurité**et **Audits**, cliquez avec le bouton droit sur un audit, puis sélectionnez **Afficher les journaux d’audit**.  
  
     Les journaux incluent **Collection d’audits** et **Windows NT**.  
  
## <a name="see-also"></a> Voir aussi  
 [Visionneuse du fichier journal](../../relational-databases/logs/log-file-viewer.md)   
 [SQL Server Audit &#40moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)   
 [Afficher les fichiers journaux hors connexion](../../relational-databases/logs/view-offline-log-files.md)  
  
  
