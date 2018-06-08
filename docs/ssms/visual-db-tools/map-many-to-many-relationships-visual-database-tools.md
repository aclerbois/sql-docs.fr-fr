---
title: Mapper des relations plusieurs-à-plusieurs (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-visual-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- relationships [SQL Server], many-to-many
- junction tables [SQL Server]
- many-to-many relationships [SQL Server]
- mapping many-to-many relationships [SQL Server]
- database diagrams [SQL Server], relationships
ms.assetid: 2977cf92-98b5-48b2-b0fd-8fbc7040f2b4
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1a8ce11021e45d363b9713c2ae7bafc7d2c1b58e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="map-many-to-many-relationships-visual-database-tools"></a>Mapper des relations plusieurs-à-plusieurs (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Les relations plusieurs-à-plusieurs vous permettent de mettre chaque ligne d'une table en relation avec plusieurs lignes d'une autre table, et vice versa. Par exemple, vous pouvez créer une relation plusieurs-à-plusieurs entre la table `authors` et la table `titles` pour établir une correspondance entre chaque auteur et tous ses livres, entre chaque livre et tous ses auteurs. Si vous choisissiez de créer une relation un-à-plusieurs à partir de l'une ou l'autre table, chaque livre ne pourrait renvoyer qu'à un seul auteur ou chaque auteur qu'à un seul livre.  
  
Les relations plusieurs-à-plusieurs entre les tables sont enregistrées dans les bases de données au moyen de tables de jonctions. Une table de jonction contient les colonnes clés primaire des deux tables à mettre en relation. Vous créez ensuite une relation entre les colonnes clés primaire de chacune des deux tables et les colonnes correspondantes dans la table de jonction. Dans la base de données pubs, la table `titleauthor` est une table de jonction.  
  
### <a name="to-create-a-many-to-many-relationship-between-tables"></a>Pour créer une relation plusieurs-à-plusieurs entre des tables  
  
1.  Dans votre schéma de base de données, ajoutez les tables entre lesquelles vous voulez créer une relation plusieurs-à-plusieurs.  
  
2.  Créez une troisième table : cliquez avec le bouton droit sur le schéma, puis, dans le menu contextuel, cliquez sur **Nouvelle table** . Cette table sera la table de jonction.  
  
3.  Dans la boîte de dialogue **Choisir un nom** , changez le nom de la table assigné par le système. Par exemple, la table de jonction entre les tables `titles` et `authors` s'intitule à présent `titleauthors`.  
  
4.  Copiez les colonnes clés primaire de chacune des deux autres tables vers la table de jonction. Vous pouvez ajouter d'autres colonnes à cette table comme à n'importe quelle autre table.  
  
5.  Dans la table de jonction, configurez la clé primaire de façon à inclure toutes les colonnes clés primaire des deux autres tables. Pour plus d’informations, consultez [Procédure : créer des clés primaires (Visual Database Tools)](http://msdn.microsoft.com/en-us/85c623ca-4656-4d70-a9db-ee4d897cd214).  
  
6.  Définissez une relation un-à-plusieurs entre chacune des deux tables primaires et la table de jonction. La table de jonction doit se trouver du côté « plusieurs » des deux relations créées. Pour plus d’informations, consultez [Procédure : créer des relations entre tables (Visual Database Tools)](http://msdn.microsoft.com/en-us/867a54b8-5be4-46e6-9702-49ae6dabf67c).  
  
    > [!NOTE]  
    > Lorsque vous créez une table de jonction dans un schéma de base de données, les données des tables connexes ne sont pas insérées dans la table de jonction. Pour plus d’informations sur l’insertion de données dans une table, consultez [Créer des requêtes Insert Results &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-insert-results-queries-visual-database-tools.md).  
  
## <a name="see-also"></a> Voir aussi  
[Utiliser des schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
  
