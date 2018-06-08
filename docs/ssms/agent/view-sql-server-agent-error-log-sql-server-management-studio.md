---
title: Afficher le journal des erreurs de SQL Server Agent (SQL Server Management Studio) | Microsoft Docs
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
- logs [SQL Server], SQL Server Agent
- viewing SQL Server Agent error logs
- displaying SQL Server Agent error logs
- SQL Server Agent, errors
- errors [SQL Server Agent]
ms.assetid: de920425-fa44-469f-b83d-49e3f97e97f4
caps.latest.revision: 6
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 280678c3f8fe75577d32501e63df697e157f79a9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-sql-server-agent-error-log-sql-server-management-studio"></a>View SQL Server Agent Error Log (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique décrit comment afficher le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent dans [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)].  
  
La visionneuse du fichier journal affiche les informations de journalisation de nombreux composants différents. Après avoir ouvert la visionneuse du fichier journal, utilisez le volet **Sélectionner les journaux** pour sélectionner les journaux à afficher. Chaque journal affiche des colonnes appropriées à ce type de journal. Les journaux disponibles dépendent de la manière dont la visionneuse du fichier journal est ouverte.  
  
**Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
    [Limitations et restrictions](#Restrictions)  
  
    [Sécurité](#Security)  
  
-   [Pour afficher le journal des erreurs de l'Agent SQL Server à l'aide de SQL Server Management Studio](#SSMSProcedure)  
  
## <a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="Restrictions"></a>Limitations et restrictions  
Cependant, l'Explorateur d'objets affiche le nœud de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent uniquement si vous avez l'autorisation de l'utiliser.  
  
### <a name="Security"></a>Sécurité  
  
#### <a name="Permissions"></a>Permissions  
Pour exécuter ses fonctions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent doit être configuré pour utiliser les informations d'identification d'un compte qui est membre du rôle serveur fixe **sysadmin** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Le compte doit avoir les autorisations Windows suivantes :  
  
-   Ouvrir une session en tant que service (SeServiceLogonRight)  
  
-   Remplacer un jeton de niveau processus (SeAssignPrimaryTokenPrivilege)  
  
-   Outrepasser le contrôle de parcours (SeChangeNotifyPrivilege)  
  
-   Changer les quotas de mémoire d'un processus (SeIncreaseQuotaPrivilege)  
  
Pour plus d’informations sur les autorisations Windows requises pour le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent, consultez [Sélection d’un compte pour le service SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) et [Configuration des comptes de service Windows](http://msdn.microsoft.com/en-us/309b9dac-0b3a-4617-85ef-c4519ce9d014).  
  
## <a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-the-includessnoversionincludesssnoversionmdmd-agent-error-log"></a>Pour afficher le journal des erreurs de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur qui contient le journal des erreurs de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] que vous souhaitez afficher.  
  
2.  Cliquez sur le signe plus (+) pour développer **Agent SQL Server**.  
  
3.  Cliquez sur le signe plus (+) pour développer le dossier **Journaux d'erreurs** .  
  
4.  Cliquez avec le bouton droit sur le journal d’erreurs que vous souhaitez afficher et sélectionnez **Afficher le journal de l’Agent**.  
  
    Les options suivantes sont disponibles dans la boîte de dialogue **Visionneuse du fichier journal –***nom_serveur* :  
  
    **Charger le journal**  
    Ouvre une boîte de dialogue dans laquelle vous pouvez spécifier un fichier journal à charger.  
  
    **Exporter**  
    Ouvre une boîte de dialogue qui vous permet d’exporter les informations figurant dans la grille **Résumé du fichier journal** vers un fichier texte.  
  
    **Actualiser**  
    Actualise l'affichage des journaux sélectionnés. Le bouton **Actualiser** permet de relire les journaux sélectionnés à partir du serveur cible lors de l'application des paramètres de filtre.  
  
    **Filtre**  
    Ouvre une boîte de dialogue qui vous permet de spécifier les paramètres utilisés pour filtrer le fichier journal, notamment **Connexion**, **Date**et d’autres critères de filtre **Général** .  
  
    **Recherche**  
    Permet de rechercher un texte spécifique dans le fichier journal. La recherche des caractères génériques n'est pas prise en charge.  
  
    **Arrêter**  
    Arrête le chargement des entrées du fichier-journal. Par exemple, vous pouvez utiliser cette option si un fichier de journal distant ou hors connexion est long à charger, et que vous souhaitez seulement consulter les entrées les plus récentes.  
  
    **Résumé du fichier journal**  
    Ce volet d'informations affiche un résumé du filtrage du fichier journal. Si le fichier n'est pas filtré, le texte suivant s'affiche : **Aucun filtre appliqué**. Si un filtre est appliqué au journal, le texte suivant s’affiche : **Filtrer les entrées du journal pour :** <filter criteria>.  
  
    **Détails de la ligne sélectionnée**  
    Sélectionnez une ligne pour afficher des détails supplémentaires sur la ligne d'événement sélectionnée en bas de la page. Vous pouvez changer l'ordre des colonnes en les faisant glisser sur la grille. Vous pouvez redimensionner les colonnes en faisant glisser les barres de séparation des colonnes dans l'en-tête de la grille vers la gauche ou la droite. Double-cliquez sur les barres de séparation des colonnes dans l'en-tête de la grille pour ajuster automatiquement la largeur de la colonne au contenu.  
  
    **Instance**  
    Nom de l'instance pour laquelle l'événement s'est produit. Il est affiché sous la forme *nom de l'ordinateur*\\*nom de l'instance*.  
  
    **Date**  
    Affiche la date de l'événement.  
  
    **Source**  
    Affiche la fonctionnalité source à partir de laquelle l'événement est créé, par exemple le nom du service (comme MSSQLSERVER). Ceci n'apparaît pas pour tous les types de journaux.  
  
    **Message**  
    Affiche les messages associés à l'événement.  
  
    **Type de journal**  
    Affiche le type de journal auquel appartient l'événement. Tous les journaux sélectionnés s'affichent dans la fenêtre de résumé du fichier journal.  
  
    **Source du journal**  
    Affiche une description du journal source dans lequel l'événement est capturé.  
  
5.  Lorsque vous avez terminé, cliquez sur **Fermer**.  
  
