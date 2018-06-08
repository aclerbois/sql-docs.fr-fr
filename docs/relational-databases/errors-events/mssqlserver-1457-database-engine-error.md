---
title: MSSQLSERVER_1457 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 1457 (Database Engine error)
ms.assetid: 28434ba1-b033-4866-ab41-111fccef45a2
caps.latest.revision: 14
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 85a3954b37ca6f3467bf1fc0d4fafda7dadb59ad
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver1457"></a>MSSQLSERVER_1457
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|1457|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBM_PAGE_UNDO_PENDING|  
|Texte du message|La synchronisation de la base de données miroir, '%.*ls', a été interrompue : la base de données est donc dans un état incohérent. La commande ALTER DATABASE a échoué. Vérifiez que la base de données miroir est sauvegardée et en ligne, reconnectez l'instance de serveur miroir, puis autorisez la base de données miroir à terminer la synchronisation.|  
  
## <a name="explanation"></a>Explication  
Ce message indique que l’instruction ALTER DATABASE *nom_base_de_données* SET PARTNER OFF a échoué. L'échec de la tentative de suppression de la mise en miroir de bases de données a interrompu la synchronisation de la base de données miroir. La base de données est incohérente.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour résoudre cette erreur, vous pouvez effectuer l'une des actions suivantes :  
  
-   Rétablissez le contact entre le serveur miroir et le serveur principal pour permettre la synchronisation de la base de données miroir.  
  
-   Supprimez la base de données miroir.  
  
    Après avoir supprimé la base de données miroir, vous pouvez en créer une nouvelle à partir de vos sauvegardes.  
  
    > [!NOTE]  
    > Vous pouvez supprimer la base de données miroir lorsque la mise en miroir de bases de données est toujours activée uniquement après l'échec d'une instruction SET PARTNER OFF.  
  
## <a name="see-also"></a> Voir aussi  
[Mise en miroir de bases de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/database-mirroring-sql-server.md)  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)  
[Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
[Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](~/database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
