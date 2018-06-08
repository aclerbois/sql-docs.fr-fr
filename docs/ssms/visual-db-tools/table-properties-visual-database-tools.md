---
title: Propriétés de la table (Visual Database Tools) | Microsoft Docs
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
- vdt.tabledesigner
- vdt.designers.properties.Table
ms.assetid: cc392987-1aab-45f5-b5af-a26be53409bf
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1140258639056876b8de6b195f5d6df9a2b69fd6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-properties-visual-database-tools"></a>Propriétés de la table (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Ces propriétés apparaissent dans la fenêtre Propriétés lorsque vous cliquez avec le bouton droit dans le Concepteur de tables puis sélectionnez Propriétés. Sauf indication contraire, vous pouvez modifier ces propriétés dans la fenêtre Propriétés lorsque la table est sélectionnée.  
  
> [!NOTE]  
> Si la table est publiée pour réplication, vous devez apporter vos modifications au schéma à l’aide de l’instruction Transact-SQL [ALTER TABLE](http://msdn.microsoft.com/en-us/f1745145-182d-4301-a334-18f799d361d1) ou de SMO (SQL Server Management Objects). Lorsque les modifications sont apportées au schéma à l'aide du Concepteur de tables ou du Concepteur de schémas de base de données, celui-ci tente d'abandonner la table et de la recréer. Toutefois, il est impossible d'abandonner les objets publiés, par conséquent les modifications du schéma échoueront.  
  
## <a name="show-table-properties-in-table-designer"></a>Affichage des propriétés de la table dans le Concepteur de tables  
  
> [!NOTE]  
> Les propriétés mentionnées dans cette rubrique sont classées par catégorie et non par ordre alphabétique.  
  
**Catégorie Identité**  
Se développe pour afficher les propriétés de **Nom**, **Description**et **Schéma**.  
  
**Nom**  
Affiche le nom de la table. Pour modifier le nom, tapez-le dans la zone de texte.  
  
> [!CAUTION]  
> En effet, s’il existe des requêtes, des vues, des fonctions définies par l’utilisateur, des procédures stockées ou des programmes qui font référence à la table, le changement de nom rend tous ces objets non valides.  
  
**Nom de la base de données**  
Affiche le nom de la source de données de la table sélectionnée.  
  
**Description**  
Indique la description de la table sélectionnée. Pour afficher l’intégralité de la description, ou la modifier, cliquez sur la description, puis sur le bouton de sélection **(...)** situé à droite de la propriété.  
  
**Schéma**  
Affiche le nom du schéma auquel cette table appartient. (S'applique uniquement à Microsoft SQL Server.)  
  
**Nom de serveur**  
Affiche le nom du serveur de la source de données.  
  
**Catégorie Concepteur de tables**  
Se développe pour afficher des propriétés pour **Colonne d’identité**, **Is Indexable**et **Is Replicated**.  
  
**Colonne d’identité**  
Affiche la colonne utilisée comme colonne d'identité de la table. Pour modifier l'identité d'une colonne, choisissez-la dans la liste déroulante. Seules les colonnes d'un type de données numérique s'affichent dans la liste.  
  
**Is Indexable**  
Indique si la table peut être indexée. Si la table n'est pas indexable, cela peut être dû au fait que vous n'êtes pas le propriétaire de la table ou que la table contient des colonnes possédant des types de données texte, ntext ou image.  
  
**Is Replicated**  
Indique si la table est répliquée à un autre emplacement.  
  
**Catégorie Spécification d'espace de données régulière**  
Se développe pour afficher des propriétés pour **(Type d’espace de données)**, **Nom du schéma de partition ou du groupe de fichiers**et **Liste des colonnes de partition**.  
  
**(Type d’espace de données)**  
Indique si cette table est stockée à l'aide d'un groupe de fichiers ou d'un schéma de partition.  
  
**Nom du schéma de partition ou du groupe de fichiers**  
Affiche le nom du groupe de fichiers ou du schéma de partition.  
  
**Liste des colonnes de partition**  
Donne accès à la boîte de dialogue **Liste des colonnes de partition** .  
  
**Colonne RowGuid**  
Affiche la colonne utilisée par Microsoft SQL Server comme colonne ROWGUID de la table. Pour changer de colonne ROWGUID, choisissez-la dans la liste déroulante. (S'applique uniquement à SQL Server 7.0 ou version ultérieure.)  
  
**Groupe de fichiers Text/Image**  
Fournit une liste déroulante dans laquelle vous pouvez choisir le groupe de fichiers des colonnes possédant des types de données texte ou image. Si la table est stockée à l'aide d'un schéma de partition, laissez ce champ vierge.  
  
## <a name="see-also"></a> Voir aussi  
[Concevoir des tables (Visual Database Tools)](../../ssms/visual-db-tools/design-tables-visual-database-tools.md)  
  
