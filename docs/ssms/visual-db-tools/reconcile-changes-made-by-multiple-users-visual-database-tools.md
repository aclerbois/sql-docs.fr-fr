---
title: Rapprocher les modifications effectuées par plusieurs utilisateurs (Visual Database Tools) | Microsoft Docs
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
- table modifications [SQL Server], multiple users
- reconciling changes made by multiple users
- modifications made by multiple users
ms.assetid: fc7ed4f2-ad3d-47fc-a3ef-51e5bb069ef0
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 70dd406f128f9df1cb36ac6c4ddcd0dd18b446aa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reconcile-changes-made-by-multiple-users-visual-database-tools"></a>Rapprocher les modifications effectuées par plusieurs utilisateurs (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Dans un environnement multi-utilisateur, des modifications peuvent être apportées à un même objet par plusieurs utilisateurs simultanément. Cela peut se produire lorsque vous travaillez sur la structure de l'objet dans le Concepteur de tables ou dans le Concepteur de schémas de base de données, ou dans les valeurs des résultats retournés dans le volet Résultats du Concepteur de requêtes et de vues. Cela peut provoquer des conflits que vous souhaiterez résoudre.  
  
## <a name="conflicts-in-the-table-or-database-diagram-designers"></a>Conflits dans le Concepteur de tables ou le Concepteur de schémas de base de données  
Par exemple, un autre utilisateur peut supprimer ou renommer une table pendant que vous l'utilisez ou que vous travaillez avec une table connexe dans le Concepteur de tables. Si vous tentez d’enregistrer votre table, la [Boîte de dialogue Modifications détectées dans la base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/database-changes-detected-dialog-box-visual-database-tools.md) vous avertit que la base de données a été modifiée depuis que vous avez ouvert la table.  
  
Elle affiche également la liste des objets de base de données qui seront affectés si vous enregistrez la table. À ce stade, vous pouvez prendre une des mesures suivantes :  
  
-   Choisissez **Oui** pour enregistrer la table et mettre à jour la base de données avec toutes les modifications de la liste.  
  
    Cette action peut affecter les tables partageant les mêmes objets de base de données. Par exemple, supposons que vous modifiez la colonne `au_id` de la table `titleauthors` alors qu’un autre utilisateur travaille sur la table `authors`, reliée à la table `titleauthors` par la colonne `au_id`. L'enregistrement de votre table affecte la table de l'autre utilisateur. De façon similaire, quelqu'un d'autre a défini une contrainte de validation de la colonne `qty` de la table `sales` . Si vous supprimez la colonne `qty` , puis enregistrez la table `sales` , la contrainte de validation de l'autre utilisateur est affectée.  
  
-   Choisissez **Non** pour annuler l’enregistrement.  
  
    Dans ce cas, vous fermez la table sans l'enregistrer. Lorsque vous la rouvrirez, son contenu correspondra à celui de la base de données.  
  
-   Choisissez **Enregistrer comme fichier texte** pour enregistrer la liste des modifications.  
  
    Cette liste des modifications apportées à la base de données s’affiche dans la boîte de dialogue **Modifications détectées dans la base de données** . Vous pouvez l’enregistrer dans un fichier texte, dans lequel vous pourrez examiner ce qui a motivé ces changements de la part des autres utilisateurs. Par exemple, quelqu'un d'autre a modifié une table sur laquelle vous avez placé un marqueur de suppression et vous souhaitez savoir s'il faut vraiment supprimer la table avant de mettre la base de données à jour.  
  
## <a name="conflicts-in-the-query-and-view-designer"></a>Conflits dans le Concepteur de requêtes et de vues  
Si vous exécutez une requête ou retournez les résultats d’une vue, les données s’affichent dans le [volet Résultats](../../ssms/visual-db-tools/results-pane-visual-database-tools.md). Plusieurs utilisateurs peuvent travailler simultanément sur le même jeu de données, ce qui risque de provoquer des conflits.  
  
Par exemple, supposons qu'une collègue et vous-même exécutiez chacun une requête d'affichage de toutes les données contenues dans la table `titleauthors` . Votre collègue remplace le prénom du premier enregistrement retourné, Barb, par Barbara. À ce stade, la base de données contient Barbara dans ce champ, alors que votre jeu de résultats affiche toujours Barb. À présent, vous tapez Barbara et cliquez en dehors de la ligne. Un message s'affiche pour vous demander de quelle manière vous souhaitez résoudre le conflit.  
  
-   Cliquez sur **Oui** pour mettre à jour la base de données avec vos modifications.  
  
    Cette opération substituera les modifications de votre collègue.  
  
-   Cliquez sur **Non** pour que votre jeu de résultats soit mis à jour afin d’afficher le contenu actuel de la base de données.  
  
    Cette opération remplacera vos modifications par celles de votre collègue.  
  
-   Cliquez sur **Annuler** pour poursuivre la modification sans résoudre le conflit.  
  
    Dans ce cas, vous ne pouvez pas valider vos modifications apportées à la base de données.  
  
## <a name="see-also"></a> Voir aussi  
[Boîte de dialogue Modifications détectées dans la base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/database-changes-detected-dialog-box-visual-database-tools.md)  
  
