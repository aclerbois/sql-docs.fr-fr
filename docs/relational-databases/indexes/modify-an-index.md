---
title: Modifier un index | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], modifying
- modifying indexes
- index changes [SQL Server]
ms.assetid: 97e3110d-fde7-4f5d-9309-dc1697960aeb
caps.latest.revision: 19
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ee158d87b9554ab75fea0462fe5e0444195c9f5d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modify-an-index"></a>Modifier un index
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Cette rubrique explique comment modifier un index dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!IMPORTANT]  
>  Les index résultant d'une contrainte PRIMARY KEY ou UNIQUE ne peuvent pas être modifiés au moyen de cette méthode. Dans ce cas, c'est la contrainte qui doit être modifiée.  
  
 **Dans cette rubrique**  
  
-   **Pour modifier un index à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-modify-an-index"></a>Pour modifier un index  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, développez la base de données à laquelle appartient la table, puis cliquez sur **Tables**.  
  
3.  Développez la table à laquelle l'index appartient, puis développez **Index**.  
  
4.  Cliquez avec le bouton droit sur l’index à modifier, puis cliquez sur **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés de l'index** , apportez les modifications souhaitées. Par exemple, vous pouvez ajouter ou supprimer une colonne de la clé d'index, ou modifier le paramètre d'une option d'index.  
  
#### <a name="to-modify-index-columns"></a>Pour modifier des colonnes d'index  
  
1.  Pour ajouter, supprimer ou déplacer une colonne d'index, cliquez sur la page **Général** dans la boîte de dialogue **Propriétés de l'index** .  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-modify-an-index"></a>Pour modifier un index  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple supprime et recrée un index existant sur la colonne `ProductID` de la table `Production.WorkOrder` avec l'option `DROP_EXISTING` . Les options `FILLFACTOR` et `PAD_INDEX` sont également définies.  
  
     [!code-sql[IndexDDL#CreateIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_1.sql)]  
  
     L'exemple suivant utilise ALTER INDEX pour définir plusieurs options de l'index `AK_SalesOrderHeader_SalesOrderNumber`.  
  
     [!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_2.sql)]  
  
#### <a name="to-modify-index-columns"></a>Pour modifier des colonnes d'index  
  
1.  Pour ajouter, supprimer, ou modifier la position d'une colonne d'index, vous devez supprimer et recréer l'index.  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [Définir les options d'index](../../relational-databases/indexes/set-index-options.md)   
 [Renommer des index](../../relational-databases/indexes/rename-indexes.md)  
  
  
