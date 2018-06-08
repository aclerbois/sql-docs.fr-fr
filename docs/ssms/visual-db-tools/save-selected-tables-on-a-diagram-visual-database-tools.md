---
title: Enregistrer des tables sélectionnées dans un schéma (Visual Database Tools) | Microsoft Docs
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
- saving tables
ms.assetid: 86943b49-48f3-432c-8021-928c13edfbcf
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 91097f2e675ffd554bff6a62c620e9770739a63d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="save-selected-tables-on-a-diagram-visual-database-tools"></a>Enregistrer des tables sélectionnées dans un schéma (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Si vous ne souhaitez pas enregistrer toutes les modifications que vous avez apportées à un schéma de base de données, vous pouvez enregistrer une table ou un groupe de tables spécifique.  
  
### <a name="to-save-selected-tables"></a>Pour enregistrer les tables sélectionnées  
  
1.  Dans votre schéma de base de données, sélectionnez les tables que vous voulez enregistrer.  
  
2.  Dans le menu **Fichier** , cliquez sur **Enregistrer la sélection**.  
  
3.  La boîte de dialogue **Enregistrer** affiche la liste des tables qui seront mises à jour dans la base de données quand vous enregistrerez votre sélection.  
  
    Choisissez **Enregistrer comme fichier texte** si vous voulez enregistrer la liste des tables dans un fichier texte à l’intérieur du répertoire du projet avant de continuer.  
  
4.  Dans la boîte de dialogue **Enregistrer** , vérifiez la liste des tables et choisissez **Oui** pour enregistrer ces tables.  
  
    > [!NOTE]  
    > La liste des tables peut contenir d'autres tables en plus de celles que vous avez sélectionnées. Par exemple, si vous changez le type de données d'une colonne qui participe à une relation avec une autre table, les deux tables seront incluses dans cette liste.  
  
## <a name="see-also"></a> Voir aussi  
[Utiliser des schémas de base de données (Visual Database Tools)](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
  
