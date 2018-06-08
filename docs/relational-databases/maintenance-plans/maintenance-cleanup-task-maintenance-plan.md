---
title: Tâche de nettoyage de maintenance (Plan de maintenance) | Microsoft Docs
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
- sql13.swb.maint.cleanup.f1
helpviewer_keywords:
- Maintenance Cleanup Task dialog box
ms.assetid: 022b679c-6799-4c13-9185-814224a20412
caps.latest.revision: 26
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 689b00b0da037fd5b72d5b8785c337781cc3e0b8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="maintenance-cleanup-task-maintenance-plan"></a>Tâche de nettoyage de maintenance (Plan de maintenance)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la **Tâche de nettoyage de maintenance** pour supprimer les anciens fichiers associés à des plans de maintenance, notamment les rapports texte créés par les plans de maintenance et les fichiers de sauvegarde de base de données.  
  
> [!NOTE]  
>  La tâche de nettoyage de maintenance ne supprime pas automatiquement les fichiers dans les sous-dossiers du répertoire spécifié. Cette fonctionnalité réduit la possibilité d'une attaque malveillante qui utilise la tâche de nettoyage de maintenance pour supprimer des fichiers. Pour supprimer des fichiers dans les sous-dossiers de premier niveau, vous devez sélectionner **Inclure les sous-dossiers de premier niveau**.  
  
## <a name="options"></a>Options  
 **Connexion**  
 Affiche la connexion active.  
  
 **Nouveau**  
 Crée une nouvelle connexion serveur à utiliser pour exécuter la tâche. La boîte de dialogue **Nouvelle connexion** est décrite ci-dessous.  
  
 **Fichiers de sauvegarde**  
 Supprimez les fichiers de sauvegarde.  
  
 **Rapports de texte du plan de maintenance**  
 Supprimez les rapports texte des plans de maintenance exécutés précédemment.  
  
 **Supprimer un fichier spécifique**  
 Supprimez le fichier spécifié dans la zone **Nom de fichier** .  
  
 **Nom de fichier**  
 Chemin d'accès et nom du fichier à supprimer.  
  
 **Rechercher dans le dossier et supprimer les fichiers en fonction de l'extension**  
 Supprimez tous les fichiers contenant l'extension spécifiée dans le dossier spécifié. Utilisez cette option pour supprimer plusieurs fichiers à la fois, par exemple tous les fichiers de sauvegarde possédant l'extension .bak dans le dossier Mardi.  
  
 **Dossier**  
 Chemin d'accès et nom du dossier contenant les fichiers à supprimer.  
  
 **Extension de fichier**  
 Spécifiez l'extension de fichier des fichiers à supprimer.  
  
 **Inclure les sous-dossiers de premier niveau**  
 Supprimez les fichiers portant l’extension spécifiée pour l’option **Extension de fichier** , des sous-dossiers de premier niveau situés dans le dossier défini dans l’option **Dossier**.  
  
 **Supprimer les fichiers en fonction de l'ancienneté du fichier au moment de l'exécution de la tâche**  
 Spécifiez l’ancienneté minimale des fichiers à supprimer en entrant un chiffre et une unité de temps dans la zone **Supprimer les fichiers antérieurs à** .  
  
 **Supprimer les fichiers antérieurs à**  
 Spécifiez l'ancienneté minimale des fichiers à supprimer en fournissant un chiffre et une unité de temps (Jour, Semaine, Mois ou Année). Les fichiers antérieurs au délai spécifié seront supprimés.  
  
 **Vue T-SQL**  
 Affiche les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutées sur le serveur pour cette tâche, selon les options sélectionnées.  
  
> [!NOTE]  
>  Si le nombre d'objets impliqués est élevé, l'affichage des instructions peut prendre un temps considérable.  
  
## <a name="new-connection-dialog-box"></a>Boîte de dialogue Nouvelle connexion  
 **Nom de la connexion**  
 Entrez un nom pour la nouvelle connexion.  
  
 **Sélectionnez ou entrez un nom de serveur.**  
 Sélectionnez un serveur auquel établir la connexion pour exécuter la tâche.  
  
 **…**  
 Sélectionnez cette option pour visualiser la liste des serveurs disponibles.  
  
 **Entrez des informations pour vous connecter au serveur**  
 Spécifiez le mode d'authentification sur le serveur.  
  
 **Utiliser la sécurité intégrée à Windows NT**  
 Permet de se connecter à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en utilisant l'authentification Microsoft Windows.  
  
 **Utiliser un nom d'utilisateur et un mot de passe spécifiques**  
 Permet de se connecter à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] à l'aide de l'authentification SQL Server. Cette option n'est pas disponible.  
  
 **User name**  
 Fournit le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser pour l'authentification. Cette option n'est pas disponible.  
  
 **Mot de passe**  
 Fournit un mot de passe à utiliser pour l'authentification. Cette option n'est pas disponible.  
  
## <a name="see-also"></a> Voir aussi  
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)  
  
  
