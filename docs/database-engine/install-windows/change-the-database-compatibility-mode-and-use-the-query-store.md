---
title: Modifier le niveau de compatibilité de la base de données et utiliser le magasin des requêtes | Microsoft Docs
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server], migrating
- upgrading SQL Server, migrating query plans
- plan guides [SQL Server], migrating query plans
ms.assetid: 7e02a137-6867-4f6a-a45a-2b02674f7e65
caps.latest.revision: 19
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e53d4e2fa269c1dea21bdcb33e471af166ccaf09
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771545"
---
# <a name="change-the-database-compatibility-level-and-use-the-query-store"></a>Modifier le niveau de compatibilité de la base de données et utiliser le magasin des requêtes

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], certaines modifications sont activées uniquement une fois que le [niveau de compatibilité de la base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) a été changé. Cette opération a été effectuée pour plusieurs raisons :  
  
- Étant donné que la mise à niveau est une opération unidirectionnelle (il est impossible de faire passer le format de fichier à une version antérieure), le fait de séparer l’activation de nouvelles fonctionnalités pour en faire une opération distincte dans la base de données, comporte un intérêt. Il est possible de rétablir un paramètre à un niveau de compatibilité de la base de données antérieur.  Le nouveau modèle réduit le nombre d’éléments devant se produire lors de l’interruption d’une fenêtre.  
  
- Les modifications apportées au processeur de requêtes peuvent avoir des effets complexes. Même si une « bonne » modification apportée au système peut être intéressante pour la plupart des charges de travail, elle peut provoquer une régression inacceptable sur une requête importante pour d’autres. Le fait de séparer cette logique du processus de mise à niveau permet à des fonctionnalités telles que le magasin des requêtes, d’atténuer rapidement les régressions de choix de plan ou même de les éviter complètement dans les serveurs de production.  
  
> [!IMPORTANT]  
> Voici les comportements auxquels vous pouvez vous attendre avec [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] quand une base de données est attachée ou restaurée et après une mise à niveau sur place :
> - Si le niveau de compatibilité d'une base de données utilisateur est à 100 ou supérieur avant la mise à niveau, il reste le même après la mise à niveau.    
> - Si le niveau de compatibilité d’une base de données utilisateur était à 90 avant la mise à niveau, dans la base de données mise à niveau, le niveau de compatibilité est défini à 100, ce qui correspond au niveau de compatibilité le plus bas pris en charge dans [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].    
> - Les niveaux de compatibilité des bases de données tempdb, model, msdb et Resource databases sont définis sur le niveau de compatibilité actuel après la mise à niveau.   
> - La base de données système master conserve le niveau de compatibilité qu’elle avait avant la mise à niveau.    
  
Le processus de mise à niveau permettant d’activer la nouvelle fonctionnalité du processeur de requêtes concerne le modèle de maintenance post-lancement du produit.  Certains de ces correctifs sont publiés sous [l’indicateur de trace 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199).  Les clients nécessitant des correctifs peuvent les choisir sans provoquer de régressions inattendues pour d’autres clients. Le modèle de maintenance post-lancement des correctifs logiciels du processeur de requêtes est documenté [ici](http://support.microsoft.com/kb/974006). À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], le passage à un nouveau niveau de compatibilité rend l’indicateur de trace 4199 inutile, dans la mesure où ces correctifs ne sont plus activés par défaut dans le tout dernier niveau de compatibilité. Par conséquent, dans le cadre du processus de mise à niveau, il est important de confirmer que 4199 n’est pas activé une fois le processus de mise à niveau terminé.  

> [!NOTE]
> Toutefois, l’indicateur de trace 4199 est toujours nécessaire pour activer les nouveaux correctifs du processeur de requêtes publiés après la version RTM, le cas échéant.
  
Le flux de travail recommandé pour mettre à niveau le processeur de requêtes vers la dernière version du code est documenté dans la [section Maintenir la stabilité des performances lors de la mise à niveau vers une version plus récente de SQL Server de la rubrique Scénarios d’utilisation du Magasin des requêtes](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade) et est illustré ci-dessous.  
  
![query-store-usage-5](../../relational-databases/performance/media/query-store-usage-5.png "query-store-usage-5") 
 
## <a name="see-also"></a> Voir aussi  
[Afficher ou modifier le niveau de compatibilité d’une base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)     
[Scénarios d’utilisation du Magasin des requêtes](../../relational-databases/performance/query-store-usage-scenarios.md)     
[Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
    
  
