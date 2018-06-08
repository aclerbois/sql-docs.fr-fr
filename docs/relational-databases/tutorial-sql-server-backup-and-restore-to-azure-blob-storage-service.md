---
title: 'Didacticiel : Sauvegarde et restauration SQL Server dans le service Stockage Blob Azure | Microsoft Docs'
ms.custom: ''
ms.date: 04/09/2018
ms.prod: sql
ms.prod_service: database-engine
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016 Preview
ms.assetid: 9e1d94ce-2c93-45d1-ae2a-2a7d1fa094c4
caps.latest.revision: 11
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 1c070d081e622982bbead15826914b01a89179e7
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34585621"
---
# <a name="tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service"></a>Didacticiel : Sauvegarde et restauration SQL Server dans le service Stockage Blob Azure
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Ce didacticiel vous aide à comprendre comment écrire des sauvegardes et les restaurer à partir du service Stockage Blob Azure.  
  
## <a name="what-you-will-learn"></a>Contenu du didacticiel  
Ce didacticiel montre comment créer un compte de stockage et un conteneur d’objets blob, créer des informations d’identification pour accéder au compte de stockage, écrire une sauvegarde dans le service d’objets blob et effectuer une restauration simple. Ce didacticiel est divisé en quatre leçons :  
  
[Leçon 1 : Créer des objets de stockage Azure](http://msdn.microsoft.com/library/74edd1fd-ab00-46f7-9e29-7ba3f1a446c5)  
Dans cette leçon, vous allez créer un compte de stockage Azure et un conteneur d’objets blob.  
  
[Leçon 2 : Créer des informations d’identification SQL Server](http://msdn.microsoft.com/library/64f8805c-1ddc-4c96-a47c-22917d12e1ab)  
Dans cette leçon, vous allez créer des informations d’identification afin de stocker les informations de sécurité utilisées pour accéder au compte de stockage Azure.  
  
[Leçon 3 : Écrire une sauvegarde de base de données complète dans le service Stockage Blob Azure](https://technet.microsoft.com/en-us/library/jj720552\(v=sql.110\).aspx)  
Dans cette leçon, vous allez publier une instruction T-SQL pour écrire une sauvegarde de la base de données AdventureWorks2012 dans le service Stockage Blob Azure.  
  
[Leçon 4 : Effectuer une restauration d’une sauvegarde de base de données complète](http://msdn.microsoft.com/library/580f76e6-9802-4abc-9043-db6de592c733)  
Dans cette leçon, vous allez publier une instruction T-SQL pour effectuer une restauration d'une sauvegarde de la base de données que vous avez créée au cours de la leçon précédente.  
  
### <a name="requirements"></a>Spécifications  
Pour suivre ce didacticiel, vous devez connaître les concepts de sauvegarde et de restauration [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et la syntaxe T-SQL. Pour utiliser ce didacticiel, votre système doit répondre aux spécifications suivantes :  
  
-   Une instance de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] et la base de données AdventureWorks2012 doivent être installées.  
  
    L’instance SQL Server peut être installée sur site ou sur une machine virtuelle Azure.  
  
    Vous pouvez utiliser une base de données utilisateur en remplacement d'AdventureWorks2012, puis modifier la syntaxe TSQL en conséquence.  
  
-   Le compte d’utilisateur utilisé pour émettre les commandes BACKUP et RESTORE doit figurer dans le rôle de base de données **db_backup operator** avec les autorisations **Modifier des informations d’identification**.  
  
### <a name="additional-reading"></a>Lecture supplémentaire  
Les ressources suivantes sont des lectures recommandées afin de comprendre les concepts et les bonnes pratiques pour l’utilisation du service Stockage Blob Azure pour les sauvegardes [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
1.  [Sauvegarde et restauration SQL Server avec le service Stockage Blob Microsoft Azure](../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)  
  
2.  [Bonnes pratiques en matière de sauvegarde SQL Server vers une URL et résolution des problèmes associés](../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)  
  
## <a name="see-also"></a>Voir aussi  
[Sauvegarde et restauration SQL Server avec le service de stockage d’objets blob Microsoft Azure](../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)

