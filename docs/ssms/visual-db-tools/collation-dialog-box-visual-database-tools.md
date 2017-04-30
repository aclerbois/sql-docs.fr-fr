---
title: "Classement, boîte de dialogue (Visual Database Tools) | Microsoft Docs"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- tools-ssms
ms.tgt_pltfrm: 
ms.topic: article
f1_keywords:
- vdt.dlgbox.definecolumncollation
- vdtsql.chm:65561
ms.assetid: e4020f79-7abf-4839-b9b2-984ef7049817
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: jhubbard
translationtype: Human Translation
ms.sourcegitcommit: 2edcce51c6822a89151c3c3c76fbaacb5edd54f4
ms.openlocfilehash: 19926b862a7ec50ca64a9b45718416ab92393cd1
ms.lasthandoff: 04/11/2017

---
# <a name="collation-dialog-box-visual-database-tools"></a>Boîte de dialogue Classement (Visual Database Tools)
Cette boîte de dialogue permet de spécifier une séquence de classement pour la colonne. La séquence de classement est utilisée pour les opérations qui comparent les valeurs de la colonne à une autre colonne ou par rapport à des valeurs constantes. Elle affecte également le comportement de certaines fonctions de chaîne, telles que SUBSTRING et CHARINDEX. Pour une liste complète des effets du paramètre de classement d'une colonne, consultez la documentation sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
Cette boîte de dialogue apparaît :  
  
-   Si vous entrez un nom de classement non valide dans le champ **Classement** de l'onglet **Propriétés des colonnes**  
  
-   Si vous cliquez dans le champ **Classement** de l’onglet **Propriétés des colonnes** , puis sur le bouton de sélection**(...)**à droite du champ.  
  
## <a name="options"></a>Options  
**Classement SQL**  
Choisi parmi les séquences de classement définies par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] dans la liste déroulante.  
  
**Classement Windows**  
Choisi parmi les séquences de classement définies par Windows dans la liste déroulante.  
  
**Tri binaire**  
Utilise les codes binaires des valeurs de caractères pour les comparaisons. Si vous sélectionnez cette option, certaines options de comparaison alphabétique ne seront plus disponibles. Par exemple, les comparaisons qui ne respectent pas la casse ne seront plus disponibles car les majuscules et les minuscules ont des encodages binaires différents. S'applique uniquement si vous sélectionnez **Classement Windows**.  
  
**Tri du dictionnaire**  
Utilise les options de comparaison alphabétique. S'applique uniquement si vous sélectionnez **Classement Windows**. Les options de comparaison alphabétique sont :  
  
-   **Respecter la casse** Sélectionnez cette option si vous souhaitez que les comparaisons différencient les majuscules et les minuscules.  
  
-   **Respecter les accents** Sélectionnez cette option si vous souhaitez que les comparaisons différencient les lettres accentuées et non accentuées. Si vous sélectionnez ceci, les comparaisons différencieront également les lettres différemment accentuées.  
  
-   **Respecter le jeu de caractères Kana** Sélectionnez cette option si vous souhaitez que les comparaisons différencient les syllabes japonaises katakana et hiragana.  
  
-   **Respecter la largeur** Sélectionnez cette option si vous souhaitez que les comparaisons différencient les caractères pleine largeur et demi-largeur.  
  
**Bouton Paramètres par défaut**  
Applique à la colonne la séquence de classement par défaut pour la base de données.  
  
## <a name="see-also"></a>Voir aussi  
[Utiliser des colonnes dans des requêtes d’agrégation &amp;#40;Visual Database Tools&amp;#41;](../../ssms/visual-db-tools/work-with-columns-in-aggregate-queries-visual-database-tools.md)  
  
