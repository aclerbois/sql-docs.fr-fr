---
title: Paramètres de base de données secondaire | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.logshipping.settings.dest.f1
ms.assetid: f992ffc9-ee42-43fe-acec-512032f0ded1
caps.latest.revision: 32
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 45eb465a92fcb10b49ea68dbefa59454332c3b58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="secondary-database-settings"></a>Paramètres de base de données secondaire
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez cette boîte de dialogue pour configurer et modifier les propriétés d'une base de données secondaire dans la configuration de la copie des journaux de transaction.  
  
 Pour obtenir des explications sur les concepts de copie des journaux de transaction, consultez [À propos de la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
## <a name="options"></a>Options  
 **Instance du serveur secondaire**  
 Affiche le nom de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actuellement configurée comme serveur secondaire dans la configuration de la copie des journaux de transaction.  
  
 **Base de données secondaire**  
 Affiche le nom de la base de données secondaire pour la configuration de la copie des journaux de transaction. Si vous ajoutez une nouvelle base de données secondaire à une configuration de la copie des journaux de transaction, vous pouvez en choisir une dans la liste ou taper son nom dans la zone de texte. Si vous entrez le nom, vous devez sélectionner une option dans l'onglet **Initialisation** afin de restaurer une sauvegarde complète de la base de données primaire dans la base de données secondaire. La nouvelle base de données est créée dans le cadre de l'opération de restauration.  
  
 **Se connecter**  
 Se connecte à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configurée comme serveur secondaire dans la configuration de la copie des journaux de transaction. Le compte utilisé pour se connecter doit être membre du rôle serveur fixe sysadmin sur l'instance de serveur secondaire.  
  
 **Onglet Initialiser**  
 Les options disponibles sont les suivantes :  
  
 **Oui, générer une sauvegarde complète de la base de données primaire et la restaurer dans la base de données secondaire**  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] configurera la base de données secondaire en sauvegardant la base de données primaire et en la restaurant sur le serveur secondaire. Si vous avez spécifié le nom d'une nouvelle base de données dans la zone **Base de données secondaire** , la base de données sera créée dans le cadre de l'opération de restauration.  
  
 **Options de restauration**  
 Cliquez sur cette option si vous voulez restaurer les données et les fichiers journaux de la base de données secondaire ailleurs que dans les emplacements par défaut sur le serveur secondaire.  
  
 Ce bouton permet d'ouvrir la boîte de dialogue **Options de restauration** , où vous pouvez indiquer les chemins d'accès aux dossiers (différents des dossiers par défaut) dans lesquels vous voulez placer la base de données secondaire et son journal. Si vous spécifiez l'un ou l'autre de ces dossiers, vous devez dans ce cas préciser les deux.  
  
 Les chemins d'accès doivent se trouver sur des lecteurs locaux sur le serveur secondaire. Ils doivent en outre commencer par une lettre de lecteur local suivie du signe deux-points (par exemple, `C:`). Les lettres de lecteurs mappés et les chemins réseau ne sont pas valides.  
  
 Si vous cliquez sur le bouton **Options de restauration** et décidez d'utiliser les dossiers par défaut, il est recommandé de fermer la boîte de dialogue **Options de restauration** . Si vous souhaitez utiliser les emplacements par défaut alors que vous avez déjà indiqué des emplacements autres que ceux-ci, cliquez de nouveau sur **Options de restauration** , effacez le contenu des zones de texte et cliquez sur OK.  
  
 **Oui, restaurer une sauvegarde existante de la base de données primaire dans la base de données secondaire**  
 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] utilisera une sauvegarde existante de la base de données primaire pour initialiser la base de données secondaire. Tapez l'emplacement de cette sauvegarde dans la zone **Fichier de sauvegarde** . Si vous avez spécifié le nom d'une nouvelle base de données dans la zone Base de données secondaire, la base de données sera créée dans le cadre de l'opération de restauration.  
  
 **Fichier de sauvegarde**  
 Tapez le chemin d’accès et le nom de fichier de la sauvegarde complète de la base de données qu’il faut utiliser pour initialiser la base de données secondaire si vous avez choisi **Oui, restaurer une sauvegarde existante de la base de données primaire dans la base de données secondaire**.  
  
 **Options de restauration**  
 Une description de ce bouton est fournie plus haut.  
  
 **Non, la base de données secondaire est initialisée**  
 Indique que la base de données secondaire est déjà initialisée et prête à accepter les sauvegardes de journaux des transactions de la base de données primaire. Cette option n'est pas disponible si vous avez tapé le nom d'une nouvelle base de données dans la zone **Base de données secondaire** .  
  
 **Onglet Copier les fichiers**  
 Les options disponibles sont les suivantes :  
  
 **Dossier de destination des fichiers copiés**  
 Tapez le chemin d'accès dans lequel les sauvegardes de journaux des transactions doivent être copiés en vue de la restauration de la base de données secondaire. Ce chemin est en général un chemin d'accès local situé sur le serveur secondaire. Si le dossier se trouve sur un autre serveur, vous devez saisir un chemin UNC vers ce dossier. Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de l'instance de serveur secondaire doit disposer des autorisations d'accès en lecture sur ce dossier. Vous devez en outre accorder des autorisations d'accès en lecture et en écriture sur ce partage réseau aux comptes proxy sous lesquels seront exécutés les travaux de copie et de restauration sur les instances de serveur secondaire. Par défaut, il s'agit du compte de service SQL Server Agent de l'instance de serveur secondaire, mais un administrateur système (sysadmin) peut choisir d'autres comptes proxy pour ces travaux.  
  
 **Supprimer les fichiers copiés après le**  
 Spécifiez le temps pendant lequel les fichiers de sauvegarde du journal des transactions copiés doivent demeurer dans le dossier de destination avant d'être supprimés.  
  
 **Nom du travail**  
 Affiche le nom du travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour copier les fichiers de sauvegarde du journal des transactions du serveur primaire vers le serveur secondaire. Lors de la création de ce travail, vous pouvez modifier son nom en le tapant dans cette zone.  
  
 **Planification**  
 Affiche la planification actuelle du travail Agent SQL Server utilisé pour copier les fichiers de sauvegarde du journal des transactions du serveur primaire vers le serveur secondaire. Vous pouvez la modifier en cliquant sur **Planification…**.  
  
 **Planification…**  
 Modifiez les paramètres du travail Agent SQL Server utilisé pour copier les sauvegardes du journal des transactions du serveur primaire vers le serveur secondaire.  
  
 **Désactiver ce travail**  
 Interrompt le travail de copie de l'Agent SQL Server.  
  
 **Onglet Restaurer le journal des transactions**  
 Les options disponibles sont les suivantes :  
  
 **Déconnecter les utilisateurs de la base de données lors de la restauration des sauvegardes**  
 Les utilisateurs sont automatiquement déconnectés de la base de données secondaire pendant que les sauvegardes du journal des transactions sont restaurées.  
  
 **Aucun mode de récupération**  
 Permet de laisser la base de données secondaire en mode NORECOVERY.  
  
 **Mode Veille**  
 Permet de laisser la base de données secondaire en mode STANDBY. Ce mode permet l'exécution des opérations en lecture seule sur la base de données.  
  
> [!IMPORTANT]  
>  Si vous modifiez le mode de récupération d’une base de données secondaire existante, par exemple en remplaçant **Mode sans récupération** par **Mode veille**, la modification entre en vigueur une fois seulement après que la sauvegarde de journal suivante a été restaurée dans la base de données.  
  
 **Retarder la restauration des sauvegardes d'au moins**  
 Indiquez le délai à l'issue duquel les sauvegardes du journal des transactions sont restaurées dans la base de données secondaire le cas échéant.  
  
 **Envoyer une alerte si aucune restauration ne se produit dans l'espace de**  
 Définit le temps devant s'écouler lors de la copie des journaux de transaction avant qu'une alerte signale l'absence de restauration du journal des transactions.  
  
 **Nom du travail**  
 Affiche le nom du travail Agent SQL Server utilisé pour restaurer les fichiers de sauvegarde du journal des transactions dans la base de données secondaire. Lors de la création de ce travail, vous pouvez modifier son nom en le tapant dans cette zone.  
  
 **Planification**  
 Affiche la planification actuelle du travail Agent SQL Server utilisé pour restaurer les fichiers de sauvegarde du journal des transactions dans la base de données secondaire. Vous pouvez la modifier en cliquant sur **Planification…**.  
  
 **Planification…**  
 Modifiez les paramètres de planification associés au travail de restauration de l'Agent SQL Server.  
  
 **Désactiver ce travail**  
 Interrompt les opérations de restauration dans la base de données secondaire.  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [À propos de la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
  
