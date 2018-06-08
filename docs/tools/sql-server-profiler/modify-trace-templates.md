---
title: Modifier des modèles de Trace | Documents Microsoft
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.suite: sql
ms.technology: profiler
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- templates [SQL Server], SQL Server Profiler
- Profiler [SQL Server Profiler], templates
- trace templates [SQL Server]
- modifying trace templates
- SQL Server Profiler, templates
ms.assetid: 75b62a54-8d16-4599-bd2d-c42cfcc209f4
caps.latest.revision: 24
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 28455e8a0af59b60ffa51e41c01726baaa851987
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modify-trace-templates"></a>Modifier des modèles de trace
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez modifier les modèles enregistrés dans un fichier sur l'ordinateur local sur lequel le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] est en cours d'exécution. Vous pouvez également modifier les modèles dérivés de ces fichiers. Quand vous modifiez des modèles existants, vous modifiez leurs propriétés, telles que les classes d’événements et les colonnes de données, en suivant l’ordre de la définition initiale des propriétés, sous l’onglet **Sélection des événements** de la boîte de dialogue **Propriétés de la trace** . Les classes d'événements et les colonnes de données peuvent être ajoutées ou supprimées, et les filtres peuvent être modifiés. Une fois le modèle modifié, un modèle spécifique à l'utilisateur est créé et le modèle système initial demeure intact. Pour plus d’informations, consultez [Enregistrer des traces et des modèles de trace](../../tools/sql-server-profiler/save-traces-and-trace-templates.md).  
  
 Il se peut que vous deviez dériver un modèle d'un fichier de trace existant si vous ne vous rappelez pas (ou si n'avez pas effectué d'enregistrement) du modèle d'origine utilisé pour créer la trace, ou bien si vous voulez exécuter la même trace à une date ultérieure. Lorsque vous utilisez des traces existantes, vous pouvez afficher les propriétés, mais pas les modifier. Pour modifier les propriétés, arrêtez ou suspendez la trace. Pour plus d’informations, consultez [Dériver un modèle à partir d’un fichier de trace ou d’une table de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md) et [Dériver un modèle à partir d’une trace en cours d’exécution &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md).  
  
## <a name="to-modify-a-trace-template"></a>Pour modifier un modèle de trace  
  
1.  Dans le menu **Fichier** , pointez sur **Modèles**, puis cliquez sur **Modifier le modèle**.  
  
2.  Dans la boîte de dialogue **Propriétés du modèle de trace** , sous l’onglet **Général** , vous pouvez modifier le type de serveur et le nom de modèle, ou utiliser un modèle par défaut pour le type de serveur.  
  
3.  Sous l’onglet **Sélection des événements**, utilisez le contrôle de la grille pour ajouter ou supprimer des événements et des colonnes de données dans le fichier de trace, comme suit :  
  
    -   Pour ajouter un événement, développez la catégorie d’événements appropriée dans la colonne **Événements** , puis sélectionnez le nom de l’événement.  
  
    -   Lorsque vous ajoutez un événement, toutes les colonnes de données sont incluses par défaut. Pour supprimer une colonne de données d'un événement dans une trace, désactivez la case à cocher dans la colonne de données de l'événement.  
  
    -   Pour ajouter des filtres, cliquez sur le nom de la colonne de données et définissez les critères de filtrage dans la boîte de dialogue **Modifier le filtre** . Vous pouvez également cliquer avec le bouton droit sur le nom de la colonne de données et cliquer sur **Modifier le filtre des colonnes** pour ouvrir la boîte de dialogue **Modifier le filtre** . Cliquez sur **OK** pour ajouter le filtre.  
  
4.  Cliquez sur **Enregistrer** ou **Enregistrer sous** pour enregistrer le modèle de trace sous un autre nom.  
  
## <a name="next-steps"></a>Étapes suivantes  
[Démarrer une trace](../../tools/sql-server-profiler/start-a-trace.md)  
[Créer une trace](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)  
[Modifier une trace existante avec Transact-SQL](../../relational-databases/sql-trace/modify-an-existing-trace-transact-sql.md)  
[Spécifier les événements et les colonnes de données d’un fichier de trace avec SQL Server Profiler](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)  
[SP-trace-setevent-transact-sql](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
