---
title: Dupliquer des tables | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- copying tables
- tables [SQL Server], duplicating
- duplicating tables
- table copying [SQL Server]
ms.assetid: c6b07423-d1e5-4e5e-8681-5088921f5df3
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 9b160c5ad5b0b02382d5236cb0d17dac62736030
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="duplicate-tables"></a>Dupliquer des tables
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  Vous pouvez dupliquer une table existante dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)] en créant une table puis en copiant les informations sur les colonnes d'une table existante.  
  
> [!IMPORTANT]  
>  Cette opération permet de dupliquer uniquement la structure d'une table, elle ne duplique aucune ligne de table.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour dupliquer une table à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Requiert l'autorisation CREATE TABLE dans la base de données de destination.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-duplicate-a-table"></a>Pour dupliquer une table  
  
1.  Vérifiez que vous êtes connecté à la base de données dans laquelle vous voulez créer la table et que la base de données est sélectionnée dans l'Explorateur d'objets.  
  
2.  Dans l’Explorateur d'objets, cliquez avec le bouton droit sur **Tables** , puis cliquez sur **Nouvelle table**.  
  
3.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table que vous souhaitez copier et cliquez sur **Conception**.  
  
4.  Sélectionnez les colonnes dans la table existante et, dans le menu **Edition** , cliquez sur **Copier**.  
  
5.  Revenez à la nouvelle table et sélectionnez la première ligne.  
  
6.  Dans le menu **Edition** , cliquez sur **Coller**.  
  
7.  Dans le menu **Fichier**, cliquez sur **Enregistrer***nom de la table*.  
  
8.  Dans la boîte de dialogue **Choisir un nom** , tapez un nom pour la nouvelle table et cliquez sur **OK**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-duplicate-a-table-in-query-editor"></a>Pour dupliquer une table dans l'éditeur de requête  
  
1.  Vérifiez que vous êtes connecté à la base de données dans laquelle vous voulez créer la table et que la base de données est sélectionnée dans l'Explorateur d'objets.  
  
2.  Cliquez avec le bouton droit sur la table que vous souhaitez dupliquer, pointez sur **Générer un script de la table en tant que**, puis pointez sur **CREATE To**et sélectionnez **Nouvelle fenêtre d’éditeur de requête**.  
  
3.  Modifiez le nom de la table.  
  
4.  Supprimez toutes les colonnes qui ne sont pas nécessaires dans la nouvelle table.  
  
5.  Cliquez sur **Exécuter**.  
  
  
