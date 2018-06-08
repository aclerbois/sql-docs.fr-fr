---
title: Utiliser l’Explorateur d’utilitaire pour gérer l’utilitaire SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: maintenance-plans
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 74012c90-b42e-4171-b27a-9c30cf69ff98
caps.latest.revision: 10
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 0ad1044ab981ced7802b050988ab2780c98cce10
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-utility-explorer-to-manage-the-sql-server-utility"></a>Utiliser l'Explorateur de l'utilitaire pour gérer l'Utilitaire SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'Explorateur de l'utilitaire, un composant de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], se connecte aux instances de [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour fournir une arborescence de tous les objets dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le volet Contenu de l'Explorateur de l'utilitaire offre plusieurs moyens de consulter les données de synthèse et détaillées sur l'état d'intégrité des instances gérées de SQL Server. L'Explorateur de l'utilitaire fournit également une interface utilisateur pour afficher et gérer des définitions des stratégies. Les fonctions de l'Explorateur de l'utilitaire varient légèrement en fonction des objets dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mais elles comprennent en général des objets, des données et des stratégies gérés par l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
## <a name="create-utility-control-point"></a>Créer un point de contrôle d'utilitaire  
 Avant de pouvoir utiliser l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez créer un point de contrôle d'utilitaire. Pour plus d’informations, consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md) ou [Créer un point de contrôle de l’utilitaire SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/create-a-sql-server-utility-control-point-sql-server-utility.md).  
  
## <a name="enroll-an-instance-of-sql-server-or-a-data-tier-application-from-utility-explorer"></a>Inscrire une Instance de SQL Server ou une application de la couche Données à partir de l'Explorateur de l'utilitaire  
 Vous pouvez facilement inscrire une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans l’Explorateur d’utilitaire, cliquez avec le bouton droit sur le nœud **Instances gérées** , puis cliquez sur **Ajouter une instance gérée**. Suivez la procédure indiquée par l'Assistant pour effectuer cette opération. Pour plus d’informations, consultez [Inscrire une instance de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/enroll-an-instance-of-sql-server-sql-server-utility.md).  
  
 Pour déployer une application de la couche Données sur une instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cliquez sur l’onglet **Explorateur d’objets**, développez le nœud **Gestion**, puis cliquez avec le bouton droit sur **Applications de la couche Données**. Dans le menu contextuel, sélectionnez **Déployer une application de la couche Données**. Pour plus d’informations, consultez [Déployer une application de la couche Données](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md).  
  
## <a name="viewing-utility-explorer"></a>Affichage de l'Explorateur de l'utilitaire  
 Par défaut, l'Explorateur de l'utilitaire n'est pas visible dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] . Si vous ne pouvez pas afficher l'Explorateur de l'utilitaire dans l'interface utilisateur de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , dans le menu **Affichage** , cliquez sur **Explorateur de l'utilitaire**. Pour afficher le volet Contenu de l'Explorateur de l'utilitaire, dans le menu **Affichage** , cliquez sur **Contenu de l'Explorateur de l'utilitaire.**  
  
## <a name="viewing-objects-in-utility-explorer"></a>Affichage des objets dans l'Explorateur de l'utilitaire  
 Le volet Navigation de l'Explorateur de l'utilitaire et le volet Contenu de l'Explorateur de l'utilitaire affichent des données, des objets et des stratégies gérés par l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilisez le volet Navigation pour spécifier les informations à afficher dans le tableau de bord et les points de vue, puis utilisez le volet Contenu et les onglets Détails pour accéder aux données et aux informations de stratégie pour les objets gérés par l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="sql-server-utility-navigation-pane"></a>Volet Navigation de l'utilitaire SQL Server  
 Le volet Navigation de l'Explorateur de l'utilitaire fournit une arborescence des objets dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , groupés par point de contrôle d'utilitaire. Pour développer des dossiers, cliquez sur le signe plus (+) ou double-cliquez sur le nom de l'UCP dans le volet Navigation de l'Explorateur de l'utilitaire. Cliquez avec le bouton droit sur les dossiers ou les objets pour réaliser des tâches courantes. Les nœuds dans l'arborescence sont les suivants :  
  
-   Le nœud supérieur dans l'arborescence est le point de contrôle d'utilitaire (UCP). Le nom du nœud se compose ainsi : « Utility_Name » (ComputerName\UCP_instance_name). Si vous n'avez pas d'UCP, vous devez en créer un. Si vous n'êtes pas connecté à un utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez le faire. Pour plus d’informations, consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md). Cliquez sur le nom d'UCP dans l'arborescence pour remplir le volet Contenu de l'Explorateur de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec les données dans l'affichage Tableau de bord. Pour plus d’informations, consultez [Tableau de bord de l’utilitaire &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/999eb741-4a60-43f6-ab37-2df7dce845c1).  
  
     Cliquez avec le bouton droit sur le nœud UCP pour actualiser des données dans le tableau de bord.  
  
-   Cliquez sur le nœud **Applications de la couche Données déployées** dans l’arborescence pour accéder aux données en mode Liste dans le volet Contenu de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les onglets Détails en bas du volet Contenu fournissent des données sur l'utilisation du processeur et du stockage, ainsi qu'un accès aux définitions des stratégies et aux informations détaillées des propriétés concernant les applications de la couche Données individuelles dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Détails des applications de la couche Données déployées &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/79c41dd9-abcb-434e-9326-00a341d5c867).  
  
     Cliquez avec le bouton droit sur le nœud **Applications de la couche Données déployées** dans l’arborescence pour accéder aux paramètres de filtrage ou actualiser les données en mode Liste.  
  
-   Cliquez sur le nœud **Instances gérées** dans l'arborescence pour afficher les données en mode Liste dans le volet Contenu de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les onglets Détails en bas du volet de contenu fournissent des données sur l'utilisation du processeur et du volume de stockage, ainsi qu'un accès aux définitions des stratégies et aux informations détaillées sur les propriétés concernant les instances gérées individuelles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Détails de l’instance gérée &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/6e51b7bb-a733-4852-8c33-7f4dbdf931c2).  
  
     Cliquez avec le bouton droit sur le nœud **Instances gérées** dans l’arborescence pour ajouter des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pour accéder aux paramètres de filtrage ou actualiser les données en mode Liste.  
  
-   Cliquez sur le nœud **Administration de l’utilitaire** dans l’arborescence pour accéder aux définitions des stratégies globales de toutes les instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux applications de la couche Données déployées dans l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pour consulter les informations destinées aux administrateurs d’UCP et accéder aux paramètres de configuration de l’entrepôt de données de gestion de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Administration de l’utilitaire &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/3e5a00c3-8905-40f0-9ddc-d924df9c2f0d). Vous pouvez également utiliser des contrôles sous l'onglet **Stratégie** pour modifier les critères de création de rapports de violation de stratégie. Pour plus d’informations, consultez [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md).  
  
     Cliquez avec le bouton droit sur le nœud **Administration de l’utilitaire** dans l’arborescence pour actualiser des données dans le volet Contenu.  
  
### <a name="sql-server-utility-dashboard"></a>Tableau de bord de l'utilitaire SQL Server  
 La sélection du nœud UCP dans l'arborescence de l'Explorateur de l'utilitaire remplit le tableau de bord de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le volet Contenu de l'Explorateur de l'utilitaire. Le tableau de bord fournit en un coup d’œil un résumé de l’état de toutes les instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de toutes les applications de la couche Données dans l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ainsi que l’utilisation totale du stockage pour les objets gérés par l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Tableau de bord de l’utilitaire &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/999eb741-4a60-43f6-ab37-2df7dce845c1). Pour inscrire ou supprimer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Inscrire une instance de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/enroll-an-instance-of-sql-server-sql-server-utility.md), [Déployer une application de la couche Données](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md) ou [Supprimer une instance de SQL Server de l’utilitaire SQL Server](../../relational-databases/manage/remove-an-instance-of-sql-server-from-the-sql-server-utility.md).  
  
### <a name="filtering-the-list-of-objects-in-utility-explorer-contents"></a>Filtrage de la liste d'objets dans Contenu de l'Explorateur de l'utilitaire  
 Lorsqu'un nœud contient un grand nombre d'objets, il peut être difficile de trouver l'objet que vous recherchez. Dans ce cas, utilisez la fonctionnalité de filtre de l'Explorateur de l'utilitaire pour réduire la taille de la liste. Par exemple, vous pouvez rechercher une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou seulement les ordinateurs avec un espace de fichier sous-exploité. Cliquez avec le bouton droit sur le dossier à filtrer, cliquez sur le bouton de filtre, puis sur **Paramètres du filtre** pour ouvrir la boîte de dialogue Paramètres du filtre de l’Explorateur d’utilitaire. Vous pouvez filtrer la liste par nom, processeur de l'ordinateur, processeur des instances, espace de fichier, espace de volume, paramètres de substitution de stratégie ou dernière heure signalée. Les colonnes **Operator** et **Value** fournissent des opérateurs de filtrage supplémentaires dans une liste déroulante.  
  
### <a name="starting-powershell"></a>Démarrage de PowerShell  
 Vous pouvez démarrer une session PowerShell en cliquant avec le bouton droit sur la plupart des dossiers et objets dans l’arborescence de l’Explorateur d’objets et en sélectionnant **Démarrer PowerShell**. Cette opération démarre une session PowerShell dont la prise en charge [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell activée, et le chemin d'accès défini sur l'objet sur lequel vous avez cliqué avec le bouton droit sur dans l'Explorateur d'objets. Vous pouvez entrer ensuite des commandes PowerShell dans un environnement PowerShell interactif. Pour en savoir plus, voir [SQL Server PowerShell](../../relational-databases/scripting/sql-server-powershell.md).  
  
 PowerShell n’offre pas d’aide accessible via la touche F1, mais inclut une applet de commande **Get-Help** qui fournit des informations sur l’utilisation de PowerShell. Pour plus d’informations sur l’utilisation de Get-Help, consultez [Obtenir de l’aide sur SQL Server PowerShell](../../relational-databases/scripting/get-help-sql-server-powershell.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités et tâches de l'utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Configurer des stratégies de contrôle d’intégrité &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/configure-health-policies-sql-server-utility.md)   
 [l’Explorateur d’objets](http://msdn.microsoft.com/library/469ea8e2-79b9-44c8-bb6f-f0e1c5dbf0f2)  
  
  
