---
title: Créer des schémas de base de données (Visual Database Tools) | Microsoft Docs
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
f1_keywords:
- vdtsql.chm:65536
- vdt.DatabaseDesigner
helpviewer_keywords:
- Database Diagram Designer
- Visual Database Tools [SQL Server], database diagrams
- database diagrams [SQL Server], designing
- diagrams [SQL Server], designing
ms.assetid: 6d2c14e1-3d73-4d10-ae5b-7f2b5d6c4ea8
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2b84ab228993294300fa02c65e76e562bea345c6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="design-database-diagrams-visual-database-tools"></a>Créer des schémas de base de données (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Outil visuel Concepteur de base de données permettant de concevoir et de visualiser une base de données à laquelle vous êtes connecté. Lors du design d'une base de données, le Concepteur de bases de données permet de créer, de modifier ou de supprimer des tables, des colonnes, des clés, des index, des relations et des contraintes. Pour visualiser une base de données, vous pouvez créer un ou plusieurs schémas illustrant toutes les tables, colonnes, clés et relations, ou quelques-unes d'entre elles.  
  
![Schéma de base de données illustrant les relations entre tables](../../ssms/visual-db-tools/media/dv3w7c1.gif "Schéma de base de données illustrant les relations entre tables")  
  
Pour n'importe quelle base de données, vous pouvez créer autant de schémas de base de données que vous le souhaitez ; chaque table de la base de données peut figurer dans un ou plusieurs schémas. Cela permet de créer différents schémas pour visualiser diverses parties de la base de données ou accentuer divers aspects du design. Par exemple, vous pouvez créer un schéma général pour montrer toutes les tables et toutes les colonnes et un schéma plus petit pour ne montrer que les tables sans les colonnes.  
  
Chaque schéma de base de données que vous créez est stocké dans la base de données associée.  
  
## <a name="tables-and-columns-in-a-database-diagram"></a>Tables et colonnes dans un schéma de base de données  
Dans un schéma de base de données, chaque table peut être affichée avec trois fonctionnalités distinctes : une barre de titre, un sélecteur de ligne et un ensemble de colonnes de propriétés.  
  
**Barre de titre** Elle affiche le nom de la table  
  
Si vous avez modifié une table sans l'enregistrer, un astérisque (*) s'ajoute à la fin du nom de la table pour signaler les modifications non encore enregistrées. Pour plus d’informations sur l’enregistrement de tables et de schémas modifiés, consultez [Utiliser des schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
  
**Sélecteur de ligne** Vous pouvez cliquer sur le sélecteur de ligne pour sélectionner une colonne de la table. Le sélecteur de ligne affiche le symbole d'une clé si la colonne fait partie de la clé primaire de la table. Pour plus d’informations sur les clés primaires, consultez [Working with Keys (Visual Database Tools)](http://msdn.microsoft.com/en-us/31fbcc9f-2dc5-4bf9-aa50-ed70ec7b5bcd)(Utilisation des clés (Visual Database Tools)).  
  
**Colonnes de propriétés** Cet ensemble de colonnes n’est visible que dans certaines vues de votre table. Il est possible de choisir entre cinq vues différentes d'une même table, ce qui permet de mieux gérer la taille et la présentation du schéma.  
  
Pour plus d’informations sur les vues des tables, consultez [Personnaliser la quantité d’informations affichées dans les schémas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/customize-the-amount-of-information-displayed-in-diagrams-visual-database-tools.md).  
  
## <a name="relationships-in-a-database-diagram"></a>Relations dans un schéma de base de données  
Dans un schéma de base de données, chaque relation peut être affichée avec trois fonctionnalités distinctes : extrémités, style de ligne et tables en relation.  
  
**Extrémités** Situées au bout des lignes, elles indiquent si la relation est de type un-à-un ou un-à-plusieurs. Si une relation a une clé à une extrémité et le symbole infini à l'autre, elle est de type un-à-plusieurs. Si elle a une clé aux deux extrémités, elle est de type un-à-un.  
  
**Style de ligne** La ligne (et non ses extrémités) indique si le système de gestion de base de données (SGBD) met en œuvre l’intégrité référentielle de la relation quand de nouvelles données sont ajoutées à la table de clé étrangère. Si la ligne est pleine, le SGBD met en œuvre l'intégrité référentielle de la relation lorsque des lignes sont ajoutées ou modifiées dans la table de clé étrangère. Si la ligne est en pointillés, le SGBD ne met pas en œuvre l'intégrité référentielle de la relation lorsque des lignes sont ajoutées ou modifiées dans la table de clé étrangère.  
  
**Tables en relation** La ligne indique si une relation de clé étrangère existe entre une table et une autre. Dans une relation un-à-plusieurs, la table de clé étrangère est celle qui se trouve près du symbole chiffre huit. Si les deux extrémités de la ligne s'attachent à la même table, il s'agit d'une relation réflexive. Pour plus d’informations, consultez [Établir des relations réflexives &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/draw-reflexive-relationships-visual-database-tools.md).  
  
## <a name="in-this-section"></a>Dans cette section  
[Comprendre la propriété du schéma de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/understand-database-diagram-ownership-visual-database-tools.md)  
  
[Naviguer dans le Concepteur de schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/navigate-in-database-diagram-designer-visual-database-tools.md)  
  
[Configurer le Concepteur de schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/set-up-database-diagram-designer-visual-database-tools.md)  
  
[Mettre à niveau des schémas de base de données d’éditions antérieures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/upgrade-database-diagrams-from-previous-editions-visual-database-tools.md)  
  
[Ouvrir le Concepteur de schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/open-database-diagram-designer-visual-database-tools.md)  
  
## <a name="see-also"></a> Voir aussi  
[Utiliser des schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
[Utiliser des tables dans les schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-tables-in-database-diagram-visual-database-tools.md)  
[Utiliser une disposition de schémas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-diagram-layout-visual-database-tools.md)  
  
