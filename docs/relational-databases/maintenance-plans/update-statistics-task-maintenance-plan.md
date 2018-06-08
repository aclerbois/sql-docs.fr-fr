---
title: Tâche Mettre à jour les statistiques (Plan de maintenance) | Microsoft Docs
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
f1_keywords:
- sql13.swb.maint.statistics.f1
helpviewer_keywords:
- Updates Statistics Task dialog box
ms.assetid: 22902fd0-eb39-4f18-af94-3fcb69d2a3a4
caps.latest.revision: 25
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2f8bbeeb45f6d49589dc2b20a32bd8614ae82f71
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="update-statistics-task-maintenance-plan"></a>Tâche Mettre à jour les statistiques (Plan de maintenance)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la boîte de dialogue **Tâche Mettre à jour les statistiques** pour mettre à jour les informations [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relatives aux données des tables et des index. Cette tâche rééchantillonne les statistiques de distribution de chaque index créé dans les tables utilisateur de la base de données. Les statistiques de distribution servent à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour optimiser la navigation dans les tables pendant le traitement des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Pour collecter automatiquement les statistiques de distribution, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] échantillonne périodiquement les données de la table correspondant à chaque index. La taille de cet échantillonnage est calculée en fonction du nombre de lignes de la table et de la fréquence de modification des données. Utilisez cette option pour effectuer un échantillonnage supplémentaire à l'aide du pourcentage spécifié de données des tables. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise ces informations pour créer des plans de requête améliorés.  
  
 Cette tâche exécute l'instruction UPDATE STATISTICS.  
  
## <a name="options"></a>Options  
 **Connexion**  
 Sélectionnez la connexion serveur à utiliser pour exécuter la tâche.  
  
 **Nouveau**  
 Crée une nouvelle connexion serveur à utiliser pour exécuter la tâche. La boîte de dialogue **Nouvelle connexion** est décrite ci-dessous.  
  
 **Bases de données**  
 Spécifie les bases de données faisant l'objet de cette tâche.  
  
-   **Toutes les bases de données**  
  
     Génère un plan de maintenance qui exécute les tâches de maintenance sur toutes les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , à l'exception de tempdb.  
  
-   **Toutes les bases de données système**  
  
     Génère un plan de maintenance qui exécute des tâches de maintenance sur chaque base de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , à l'exception de tempdb. Aucune tâche de maintenance n'est exécutée sur les bases de données créées par l'utilisateur.  
  
-   **Toutes les bases de données utilisateur**  
  
     Génère un plan de maintenance qui exécute des tâches de maintenance sur toutes les bases de données créées par l'utilisateur. Aucune tâche de maintenance n'est exécutée sur les bases de données système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Ces bases de données**  
  
     Génère un plan de maintenance qui n'exécute les tâches de maintenance que sur les bases de données sélectionnées. Si vous choisissez cette option, sélectionnez au moins une base de données.  
  
 **Remarque** Les plans de maintenance sont exécutés uniquement sur des bases de données définies avec un niveau de compatibilité 80 ou plus. Les bases de données définies au niveau de compatibilité 70 ou moins ne sont pas affichées.  
  
 **Objet**  
 Limite la grille de **Sélection** à l’affichage des tables et/ou des vues.  
  
 **Sélection**  
 Spécifie les tables ou les index faisant l'objet de cette tâche. Non disponible quand **Tables et vues** est sélectionné dans la zone Objet.  
  
 **Toutes les statistiques existantes**  
 Met à jour les statistiques pour les colonnes et les index.  
  
 **Statistiques de colonnes uniquement**  
 Met à jour les statistiques de colonnes uniquement.  
  
 **Statistiques d'index uniquement**  
 Met à jour les statistiques d'index uniquement.  
  
 **Type d'analyse**  
 Type d'analyse destinée à la collecte des statistiques mises à jour.  
  
 **Analyse complète**  
 Lit toutes les lignes de la table ou de la vue pour rassembler les statistiques.  
  
 **Exemple par**  
 Spécifie le pourcentage de la table ou de la vue indexée, ou le nombre de lignes à échantillonner lors de la collecte des statistiques de tables ou vues volumineuses.  
  
 **Vue T-SQL**  
 Affiche les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutées sur le serveur pour cette tâche, selon les options sélectionnées.  
  
> [!NOTE]  
>  Si le nombre d'objets impliqués est élevé, l'affichage des instructions peut prendre un temps considérable.  
  
## <a name="new-connection-dialog-box"></a>Boîte de dialogue Nouvelle connexion  
 **Nom de la connexion**  
 Entrez un nom pour la nouvelle connexion.  
  
 **Sélectionnez ou entrez un nom de serveur.**  
 Sélectionnez un serveur auquel établir la connexion pour exécuter la tâche.  
  
 **Actualiser**  
 Actualise la liste des serveurs disponibles.  
  
 **Entrez des informations pour vous connecter au serveur**  
 Spécifiez le mode d'authentification sur le serveur.  
  
 **Utiliser la sécurité intégrée à Windows NT**  
 Permet de se connecter à une instance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] à l’aide de l’authentification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Utiliser un nom d'utilisateur et un mot de passe spécifiques**  
 Permet de se connecter à une instance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] en utilisant l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option n'est pas disponible.  
  
 **User name**  
 Fournit le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser pour l'authentification. Cette option n'est pas disponible.  
  
 **Mot de passe**  
 Fournit un mot de passe à utiliser pour l'authentification. Cette option n'est pas disponible.  
  
## <a name="see-also"></a> Voir aussi  
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)  
  
  
