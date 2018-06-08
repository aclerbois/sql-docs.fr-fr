---
title: Contraintes de validation, boîte de dialogue (Visual Database Tools) | Microsoft Docs
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
- vdt.dlgbox.checkconstraint
ms.assetid: ad0bbf7f-b0de-406a-bd0a-cb779816b101
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 9b72dfc8b97dfdf80e692abac8f0fc142cec5be9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="check-constraint-dialog-box-visual-database-tools"></a>Boîte de dialogue Contraintes de validation (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Cette boîte de dialogue apparaît quand vous cliquez avec le bouton droit sur une grille de définition de table dans le Concepteur de tables puis cliquez sur **Vérifier les contraintes**. Elle contient un jeu de propriétés pour les contraintes non uniques jointes aux tables de votre base de données. Les propriétés qui s’appliquent aux contraintes uniques apparaissent dans la boîte de dialogue **Index/Clés** .  
  
> [!NOTE]  
> Si la table est publiée pour réplication, vous devez apporter vos modifications au schéma à l’aide de l’instruction Transact-SQL [ALTER TABLE](http://msdn.microsoft.com/en-us/f1745145-182d-4301-a334-18f799d361d1) ou de SMO (SQL Server Management Objects). Lorsque les modifications sont apportées au schéma à l'aide du Concepteur de tables ou du Concepteur de schémas de base de données, celui-ci tente d'abandonner la table et de la recréer. Toutefois, il est impossible d'abandonner les objets publiés, par conséquent les modifications du schéma échoueront.  
  
## <a name="options"></a>Options  
**Contraintes de validation sélectionnées**  
Répertorie les contraintes de validation disponibles. Pour afficher les propriétés d'une contrainte, sélectionnez-la dans la liste.  
  
**Ajouter**  
Crée une nouvelle contrainte pour la table de base de données sélectionnée et fournit un nom par défaut ainsi que d'autres valeurs pour la contrainte. La contrainte ne devient valide que lorsqu'une expression est entrée pour celle-ci.  
  
**Supprimer**  
Supprime la contrainte sélectionnée de la table. Pour annuler l'ajout d'une contrainte de validation, supprimez la contrainte à l'aide de ce bouton.  
  
**Catégorie Général**  
Se développe pour afficher le champ de la propriété **Expression** .  
  
**Expression**  
Affiche l'expression pour la contrainte de validation sélectionnée. Pour les nouvelles contraintes, vous devez entrer l'expression avant de quitter cette zone. Vous pouvez également modifier des contraintes de validation existantes. Pour plus d’informations, consultez [Utilisation des contraintes (Visual Database Tools)](http://msdn.microsoft.com/en-us/637098af-2567-48f8-90f4-b41df059833e).  
  
**Catégorie Identité**  
Se développe pour afficher les propriétés de **Nom** et **Description**.  
  
**Nom**  
Indique le nom de la contrainte de validation sélectionnée. Pour modifier le nom de cette contrainte, tapez directement le texte dans le champ de la propriété.  
  
**Description**  
Description de cette contrainte de validation. Vous pouvez modifier la description en la tapant dans le champ de propriété ou cliquer sur le bouton de sélection (**...**) qui s’affiche à droite du champ de propriété et modifier la description dans la boîte de dialogue **Propriété de la description** .  
  
**Catégorie Concepteur de tables**  
Se développe pour afficher les propriétés de **Vérifier les données existantes à la création ou à la réactivation**, **Appliquer INSERTs et UPDATEs**et **Appliquer la réplication**.  
  
**Check Existing Data On Creation or Re-Enabling**  
Spécifie si toutes les données préexistantes (données qui existaient dans la table avant la création de la contrainte) sont vérifiées par rapport à la contrainte.  
  
**Appliquer INSERTs et UPDATEs**  
Spécifie si la contrainte est appliquée lors de l'insertion ou de la mise à jour de données dans la table.  
  
**Appliquer la réplication**  
Indique si la contrainte doit être appliquée lorsqu'un Agent de réplication effectue une requête Insert ou Update sur cette table.  
  
## <a name="see-also"></a> Voir aussi  
[Utilisation des contraintes (Visual Database Tools)](http://msdn.microsoft.com/en-us/637098af-2567-48f8-90f4-b41df059833e)  
[Boîte de dialogue Index - Clés &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/indexes-keys-dialog-box-visual-database-tools.md)  
  
