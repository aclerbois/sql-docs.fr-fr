---
title: Restaurer la base de données master - système de plateforme Analytique | Documents Microsoft
description: Restaurez la base de données master dans le système de plateforme d’Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 184184f332225e76e152c2d909cfff788b4fea91
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="restore-the-master-database-in-analytics-platform-system"></a>Restaurer la base de données master dans le système de plateforme Analytique
Le **Restore Master** page du Gestionnaire de Configuration PDW SQL Server vous permet de restaurer la base de données master à partir d’une sauvegarde.  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
> [!IMPORTANT]  
> Pour effectuer la restauration, SQL Server PDW doit supprimer la base de données principale actuelle, qui contient les informations de sécurité d’utilisateur et le catalogue de base de données. Nous vous recommandons d’effectuer une sauvegarde de la base de données principale actuelle avant d’effectuer la restauration.  
  
## <a name="to-restore-the-master-database"></a>Pour restaurer la base de données master  
  
1.  Lancez le Gestionnaire de Configuration. Pour plus d’informations, consultez [lancer le Gestionnaire de Configuration &#40;système de plateforme Analytique&#41;](launch-the-configuration-manager.md).  
  
2.  Dans le volet gauche du Gestionnaire de Configuration, cliquez sur **Restore Master**.  
  
3.  Sélectionnez la sauvegarde de master à restaurer.  
  
4.  Cliquez sur **Appliquer**.  
  
5.  Pour effectuer la restauration, SQL Server PDW s’arrêter tous les services d’application et déconnecter tous les utilisateurs. Une fois la restauration terminée, SQL Server PDW redémarre les services d’application.  
  
![DWConfig Appliance Restore master PDW](./media/restore-the-master-database/SQL_Server_PDW_DWConfig_ApplPDWRestore.png "SQL_Server_PDW_DWConfig_ApplPDWRestore")  
  
