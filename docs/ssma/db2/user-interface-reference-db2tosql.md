---
title: Référence de l’Interface utilisateur (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 98ecc4ff-9416-48a2-af0f-86852cf69dab
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 7b2f512f062909cbb92611bac1e34ad13d508d34
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="user-interface-reference-db2tosql"></a>Référence de l’Interface utilisateur (DB2ToSQL)
Cette section inclut des rubriques d’aide pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour DB2.  
  
## <a name="in-this-section"></a>Dans cette section  
Le tableau suivant répertorie les boîtes de dialogue SSMA :  
  
|||  
|-|-|  
|Rubrique| Description|  
|[Avancé de sélection d’un objet &#40;DB2ToSQL&#41;](../../ssma/db2/advanced-object-selection-db2tosql.md)|Utilisez le **avancé le sélectionnez objet** boîte de dialogue pour rechercher des objets de base de données à l’aide de critères de filtre, puis activez ou désactivez ces objets.|  
|[Rapport d’évaluation &#40;DB2ToSQL&#41;](../../ssma/db2/assessment-report-db2tosql.md)|Utilisez le rapport d’évaluation pour afficher les résultats de la conversion d’objets DB2 à [!INCLUDE[tsql](../../includes/tsql_md.md)] syntaxe et pour estimer le temps et la complexité d’une migration vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|[Connexion à la base de données DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)|Utilisez le **se connecter à DB2** boîte de dialogue se connecter à la base de données DB2 que vous souhaitez migrer.|  
|[Se connecter à SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connect-to-sql-server-db2tosql.md)|Utilisez le **se connecter à SQL Server** boîte de dialogue se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour lequel vous souhaitez migrer.|  
|[Rapport de Migration de données &#40;DB2ToSQL&#41;](../../ssma/db2/data-migration-report-db2tosql.md)|Affiche les résultats de migration des données à partir de DB2 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|[Paramètres de migration de données](http://msdn.microsoft.com/en-us/573e673e-a194-4cb2-9aba-aaac6e1a225c)|Utilisez le **paramètres de Migration de données étendus** tab pour écrire des requêtes personnalisées pour la migration de données.|  
|[Modifier le mappage de Type &#40;DB2ToSQL&#41;](../../ssma/db2/edit-type-mapping-db2tosql.md)|Utilisez le **nouveau mappage de Type** ou **modifier un mappage de Type** boîtes de dialogue pour créer ou modifier le mappage des types de données entre les bases de données source et cible et les objets de base de données.|  
|[Paramètres globaux &#40;éditeur&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/global-settings-editor-db2tosql.md)|Utilisez la page de l’éditeur de la **paramètres globaux** boîte de dialogue pour configurer les options de l’éditeur de code.|  
|[Paramètres globaux &#40;boîtes de dialogue&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/global-settings-dialogs-db2tosql.md)|Utilisez la page de boîtes de dialogue de la **paramètres globaux** boîte de dialogue pour configurer les paramètres de l’avertissement et de la boîte de dialogue par défaut.|  
|[Paramètres globaux &#40;journalisation&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/global-settings-logging-db2tosql.md)|Utilisez la page Journalisation de le **paramètres globaux** boîte de dialogue pour configurer la journalisation.|  
|[Paramètres globaux &#40;fenêtre sortie&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/global-settings-output-window-db2tosql.md)|Utilisez le **paramètres globaux** boîte de dialogue pour définir les préférences de SSMA pour interface utilisateur de DB2.|  
|[Nouveau projet &#40;DB2ToSQL&#41;](../../ssma/db2/new-project-db2tosql.md)|Utilisez le **nouveau projet** boîte de dialogue pour créer un nouveau SSMA pour le projet de DB2.|  
|[Paramètres du projet &#40;Conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md)|Utilisez la page de Conversion de la **les paramètres de projet** boîte de dialogue pour spécifier comment SSMA pour DB2 convertit les fonctions et variables globales.|  
|[Paramètres du projet &#40;GUI&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-gui-db2tosql.md)|Utilisez la page de l’interface graphique utilisateur de la **les paramètres de projet** boîte de dialogue pour spécifier la quantité de données s’affiche sur le **données** onglet.|  
|[Paramètres du projet &#40;Migration&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-migration-db2tosql.md)|Utilisez la page de la Migration de le **les paramètres de projet** boîte de dialogue Personnaliser comment SSMA pour DB2 migre les données à partir de DB2 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|[Paramètres du projet&#40;synchronisation&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md)|Utilisez la page de synchronisation de la **les paramètres de projet** boîte de dialogue pour personnaliser la façon dont SSMA pour DB2 crée ou modifie migrer des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|[Paramètres du projet&#40;objets système de chargement&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-loading-system-objects-db2tosql.md)|Utilisez la page de chargement des objets de système de la **les paramètres de projet** boîte de dialogue pour spécifier quel système DB2 objets SSMA convertit et les charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|[Paramètres du projet &#40;le mappage de Type&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md)|Utilisez la page mappage de Type de la **les paramètres de projet** boîte de dialogue pour spécifier les mappages de type par défaut pour toutes les bases de données et les objets de base de données de SSMA pour le projet de DB2.|  
|[Actualiser à partir de la base de données &#40;DB2ToSQL&#41;](../../ssma/db2/refresh-from-database-db2tosql.md)|Utilisez le **Actualiser à partir de la base de données** boîte de dialogue pour sélectionner les objets à actualiser à partir de la base de données DB2.|  
|[Enregistrer les métadonnées &#40;DB2ToSQL&#41;](../../ssma/db2/save-metadata-db2tosql.md)|Le **enregistrer les métadonnées** boîte de dialogue apparaît lorsque vous enregistrez un projet auquel il manque des métadonnées.|  
  
## <a name="see-also"></a>Voir aussi  
[Prise en main de SSMA pour DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/getting-started-with-ssma-for-db2-db2tosql.md)  
[Bases de données DB2 migration vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
