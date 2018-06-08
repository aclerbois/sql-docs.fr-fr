---
title: Définir ou changer le classement du serveur | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: collations
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- server collations [SQL Server]
- collations [SQL Server], server
ms.assetid: 3242deef-6f5f-4051-a121-36b3b4da851d
caps.latest.revision: 34
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: de82c785b6fd7c9ce9fb2b22d5800e3e5e5e5cef
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-or-change-the-server-collation"></a>Définir ou changer le classement du serveur
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le classement du serveur agit en tant que classement par défaut pour toutes les bases de données système installées avec l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ainsi que pour toute base de données utilisateur nouvellement créée. Le classement du serveur est spécifié au cours de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="changing-the-server-collation"></a>Modification du classement du serveur  
 La modification du classement du serveur par défaut pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut s'avérer une opération complexe et implique les étapes suivantes :  
  
-   Vérifiez que vous disposez de toutes les informations ou de tous les scripts nécessaires pour recréer vos bases de données utilisateur et les objets qu'elles contiennent.  
  
-   Exportez toutes vos données à l’aide d’un outil tel que l’[utilitaire bcp](../../tools/bcp-utility.md). Pour plus d’informations, consultez [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
-   Supprimez toutes les bases de données utilisateur.  
  
-   Recréez la base de données MASTER en spécifiant le nouveau classement dans la propriété SQLCOLLATION de la commande **setup** . Exemple :  
  
    ```  
    Setup /QUIET /ACTION=REBUILDDATABASE /INSTANCENAME=InstanceName   
    /SQLSYSADMINACCOUNTS=accounts /[ SAPWD= StrongPassword ]   
    /SQLCOLLATION=CollationName  
    ```  
  
     Pour plus d’informations, consultez [Reconstruire des bases de données système](../../relational-databases/databases/rebuild-system-databases.md).  
  
-   Créez toutes les bases de données et tous les objets qu'elles contiennent.  
  
-   Importez toutes vos données.  
  
> [!NOTE]  
>  Au lieu de changer le classement par défaut d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez spécifier un classement par défaut pour chaque nouvelle base de données que vous créez.  
  
## <a name="see-also"></a> Voir aussi  
 [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md)   
 [Définir ou modifier le classement de la base de données](../../relational-databases/collations/set-or-change-the-database-collation.md)   
 [Définir ou modifier le classement des colonnes](../../relational-databases/collations/set-or-change-the-column-collation.md)   
 [Reconstruire des bases de données système](../../relational-databases/databases/rebuild-system-databases.md)  
  
  
