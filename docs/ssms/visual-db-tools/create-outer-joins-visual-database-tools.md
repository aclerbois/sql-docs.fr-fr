---
title: Créer des jointures externes (Visual Database Tools) | Microsoft Docs
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
- outer joins
- joins [SQL Server], outer
ms.assetid: 18de47b1-f936-427d-b852-fe6d20334f71
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: eaa656ab6775594477ed4d83470b1c2ebab642c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-outer-joins-visual-database-tools"></a>Créer des jointures externes (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Par défaut, le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) crée une jointure interne entre des tables. Les jointures internes éliminent les lignes qui ne correspondent pas à une ligne de l'autre table. Quant aux jointures externes, elles retournent toutes les lignes d'au moins une des tables ou vues mentionnées dans la clause FROM, pour autant que ces lignes répondent à une des conditions de recherche WHERE ou HAVING. Pour inclure, dans l'ensemble de résultats, des lignes de données qui n'ont pas de correspondances dans la table jointe, vous pouvez créer une jointure externe.  
  
Lorsque vous créez une jointure externe, l'ordre d'affichage des tables dans l'instruction SQL (c.-à-d. telles qu'elles apparaissent dans le volet SQL) est important. La première table que vous ajoutez devient la table « de gauche » et la seconde la table « de droite ». (En revanche, l’ordre d’apparition des tables dans le [volet Schéma](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md) importe peu.) Quand vous spécifiez une jointure externe droite ou gauche, vous faites référence à l’ordre dans lequel les tables ont été ajoutées à la requête et à celui dans lequel elles apparaissent dans l’instruction SQL du [volet SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md).  
  
### <a name="to-create-an-outer-join"></a>Pour créer une jointure externe  
  
1.  Créez la jointure, soit automatiquement, soit manuellement. Pour plus d’informations, consultez [Joindre automatiquement des tables &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-tables-automatically-visual-database-tools.md) ou [Joindre manuellement des tables &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-tables-manually-visual-database-tools.md).  
  
2.  Sélectionnez la ligne de jointure dans le volet Schéma, puis dans le menu **Concepteur de requêtes**, choisissez **Sélectionner toutes les lignes de <tablename>**, en sélectionnant la commande qui ajoute la table dont vous voulez inclure les lignes supplémentaires.  
  
    -   Choisissez la première table pour créer une jointure externe gauche.  
  
    -   Choisissez la seconde table pour créer une jointure externe droite.  
  
    -   Choisissez les deux tables pour créer une jointure externe entière.  
  
Lorsque vous spécifiez une jointure externe, le Concepteur de requêtes et de vues modifie la ligne de jointure pour indiquer qu'il s'agit d'une jointure externe.  
  
En outre, il modifie l'instruction SQL du volet SQL pour refléter la modification du type de jointure, comme l'illustre l'instruction suivante :  
  
```  
SELECT employee.job_id, employee.emp_id,  
   employee.fname, employee.minit, jobs.job_desc  
FROM employee LEFT OUTER JOIN jobs ON   
    employee.job_id = jobs.job_id  
```  
  
Dans la mesure où une jointure externe comprend des lignes sans correspondance, vous pouvez l'utiliser pour trouver des lignes qui ne respectent pas les contraintes de clé étrangère. Pour ce faire, créez une jointure externe, puis ajoutez une condition de recherche afin de rechercher les lignes dans lesquelles la colonne clé primaire de la table à l'extrême droite a la valeur null. Dans l'exemple, la jointure externe ci-dessous trouve des lignes de la table `employee` qui ne possèdent pas de lignes correspondantes dans la table `jobs` :  
  
```  
SELECT employee.emp_id, employee.job_id  
FROM employee LEFT OUTER JOIN jobs   
   ON employee.job_id = jobs.job_id  
WHERE (jobs.job_id IS NULL)  
```  
  
## <a name="see-also"></a> Voir aussi  
[Interroger avec des jointures &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
[Boîte de dialogue Joindre &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-dialog-box-visual-database-tools.md)  
  
