---
title: Propriétés des colonnes de table (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65558
- vdtsql.chm:69657
- vdt.ppg.columns
ms.assetid: 09830897-cc10-46b8-95f5-e0e9681b668c
caps.latest.revision: 33
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a435872914c99990d57804aef0e7e869e1d4737e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-column-properties-sql-server-management-studio"></a>Propriétés des colonnes de table (SQL Server Management Studio)
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  Ces propriétés apparaissent dans le volet en bas du Concepteur de tables. Sauf indication contraire, vous pouvez modifier ces propriétés dans la fenêtre Propriétés lorsque la colonne est sélectionnée. Les **Propriétés des colonnes** peuvent être affichées par catégories ou par ordre alphabétique. Un grand nombre de propriétés apparaissent uniquement ou peuvent uniquement être modifiées pour certains types de données.  
  
> [!NOTE]  
>  Si la table est publiée pour réplication, vous devez apporter vos modifications au schéma à l’aide de l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMO (SQL Server Management Objects). Lorsque les modifications sont apportées au schéma à l'aide du Concepteur de tables ou du Concepteur de schémas de base de données, celui-ci tente d'abandonner la table et de la recréer. Toutefois, il est impossible d'abandonner les objets publiés, par conséquent les modifications du schéma échoueront.  
  
 **Général**  
 Peut être développée pour afficher **Nom**, **Autoriser les valeurs NULL**, **Type de données**, **Valeur ou liaison par défaut**, **Longueur**, **Précision**et **Échelle**.  
  
 **Nom**  
 Affiche le nom de la colonne sélectionnée.  
  
 **Autoriser les valeurs NULL**  
 Indique si la colonne accepte la valeur null. Pour modifier cette propriété, activez la case à cocher Null autorisé correspondant à la colonne dans le volet en haut du Concepteur de tables.  
  
 **Type de données**  
 Affiche le type de données pour la colonne sélectionnée. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
 **Valeur ou liaison par défaut**  
 Affiche la valeur par défaut pour cette colonne lorsqu'aucune valeur n'est spécifiée. La valeur dans ce champ correspond soit à la valeur d'une contrainte par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soit le nom d'une contrainte générale à laquelle est liée la colonne. La liste déroulante contient les valeurs globales par défaut définies dans la base de données. Pour lier la colonne à une valeur par défaut globale, sélectionnez-en une dans la liste déroulante. Ou bien, pour créer une contrainte par défaut pour la colonne, entrez la valeur par défaut directement comme un texte.  
  
 **Longueur**  
 Affiche le nombre de caractères autorisés pour les types de données texte. Cette propriété est uniquement disponible pour les types de données basés sur les caractères.  
  
 **Échelle**  
 Affiche le nombre maximal de chiffres autorisé à droite de la virgule décimale pour les valeurs de cette colonne. Cette propriété affiche **0** pour les types de données non numériques.  
  
 **Précision**  
 Affiche le nombre maximal de chiffres autorisé pour les valeurs de cette colonne. Cette propriété affiche **0** pour les types de données non numériques.  
  
 **Concepteur de tables**  
 Développe la section **Concepteur de tables** .  
  
 **Classement**  
 Affiche la séquence de classement que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique par défaut à la colonne lorsque ses valeurs sont utilisées pour trier les lignes de résultat d'une requête. Pour modifier le classement, sélectionnez la propriété et cliquez sur le bouton de sélection (...) à droite de la valeur de la propriété pour afficher la boîte de dialogue **Classement** .  
  
 **Spécification de la colonne calculée**  
 Affiche les informations sur une colonne calculée. La valeur affichée pour la propriété est identique à celle de la propriété enfant **Formule** et affiche la formule pour la colonne calculée.  
  
> [!NOTE]  
>  Pour modifier la valeur affichée pour la propriété **Spécification de la colonne calculée** , vous devez la développer et modifier la propriété enfant **Formula** .  
  
-   **Formule** Affiche la formule de la colonne calculée. Pour modifier cette propriété, tapez la nouvelle valeur directement.  
  
-   **Est persistant** Indique si les résultats de la formule sont stockés. Si cette propriété est définie sur **Non** , seule la formule est stockée et les valeurs sont calculées chaque fois que la colonne est référencée. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
 Pour plus d'informations, consultez [Specify Computed Columns in a Table](../../relational-databases/tables/specify-computed-columns-in-a-table.md).  
  
 **Type de données condensé**  
 Affiche les informations relatives au type de données contenu dans le champ, selon le même format que l'instruction SQL CREATE TABLE. Par exemple, un champ contenant une chaîne de longueur variable ne dépassant pas 20 caractères est représenté sous la forme « varchar(20) ». Pour modifier cette propriété, tapez directement la valeur.  
  
 **Description**  
 Affiche le texte décrivant la colonne. Pour modifier la description, sélectionnez la propriété, cliquez sur le bouton de sélection (...) à droite de la valeur de la propriété, et modifiez la description dans la boîte de dialogue **Propriété de la description** .  
  
 **Déterministe**  
 Indique si le type de données de la colonne sélectionnée peut être déterminé avec certitude.  
  
 **Publiée via DTS**  
 Indique si la colonne est publiée via DTS. ([Data Transformation Services est déconseillé](https://msdn.microsoft.com/library/cc707786(v=sql.130).aspx#Anchor_0)). 
  
 **Spécification de texte intégral**  
 Affiche des informations sur l'index de recherche en texte intégral. La valeur de cette propriété correspond à la valeur de la propriété enfant **Est d’index de recherche en texte intégral** , et indique si cette colonne est indexée en texte intégral.  
  
> [!NOTE]  
>  Pour modifier la valeur affichée pour la propriété **Spécification de texte intégral** , vous devez la développer et modifier la propriété enfant **Est d’index de recherche en texte intégral** .  
  
-   **Est d’index de recherche en texte intégral** Indique si cette colonne est indexée en texte intégral. Cette propriété ne peut prendre la valeur **Oui** que si le type de données de cette colonne peut être soumis à une recherche en texte intégral et si la table contenant cette colonne contient un index de texte intégral. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une valeur.  
  
-   **Colonne de type de texte intégral** Affiche le nom de la colonne à partir de laquelle cette colonne est indexée en texte intégral. Cette propriété doit être définie si la propriété **Type de données** de cette colonne est soit **image** , soit **varbinary**. La colonne nommée dans cette propriété doit être de type **[n]char, [n]varchar** ou **xml**, et la liste déroulante de cette propriété contient uniquement les colonnes avec l’un de ces trois types de données. Les lignes situées dans la colonne nommée par cette propriété indiquent le type de document des lignes correspondantes de la colonne faisant l'objet d'une recherche en texte intégral. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
-   **Langue** Indique la langue utilisée par l'analyseur lexical pour indexer la colonne. La valeur stockée dans la propriété est en réalité l'identificateur de paramètres régionaux pour l'analyseur lexical. Pour plus d'informations sur les analyseurs lexicaux et les LCID, consultez Analyseurs lexicaux et Générateurs de formes dérivées. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
 **Sémantique statistique**  
 Sélectionnez s'il faut activer l'indexation sémantique statistique pour la colonne sélectionnée. Pour plus d’informations, consultez [Recherche sémantique &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
 Si vous sélectionnez une **langue** avant de sélectionner **Sémantique statistique**, et que la langue sélectionnée n'est pas associée à un modèle linguistique sémantique, l'option **Sémantique statistique** est définie avec la valeur **Non** et ne peut pas être modifiée. Si vous sélectionnez **Oui** pour l'option **Sémantique statistique** avant de sélectionner une **langue**, les langues disponibles dans la colonne **Langue** sont limitées à celles pour lesquelles il existe une prise en charge de modèle linguistique sémantique.  
  
 **A un abonné autre que SQL Server**  
 Indique si la colonne est en cours de réplication vers un abonné autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Spécification du compteur**  
 Indique si et comment cette colonne applique l'unicité sur ses valeurs. La valeur de cette propriété indique si cette colonne est une colonne d'identité ou non, et si elle est identique à la valeur de la propriété enfant **Est d'identité**.  
  
> [!NOTE]  
>  Pour modifier la valeur affichée pour la propriété **Spécification du compteur** , vous devez la développer et modifier la propriété enfant **Est d’identité** .  
  
-   **Est une identité** Indique si cette colonne est une colonne d'identité ou non. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
-   **Début du compteur** Affiche la valeur de départ spécifiée au cours de la création de cette colonne d'identité. Cette valeur sera assignée à la première ligne de la table. Si vous laissez cette cellule vide, la valeur 1 est affectée par défaut. Pour modifier cette propriété, tapez directement la nouvelle valeur.  
  
-   **Incrément du compteur** Affiche la valeur de départ spécifiée au cours de la création de cette colonne d'identité. Cette valeur correspond à l'incrément qui sera ajouté à la valeur **Incrément du compteur** à chaque nouvelle ligne. Si vous laissez cette cellule vide, la valeur 1 est affectée par défaut. Pour modifier cette propriété, tapez directement la nouvelle valeur.  
  
 **Indexable**  
 Indique si la colonne sélectionnée peut être indexée. Par exemple, les colonnes calculées non déterministes ne peuvent pas être indexées.  
  
 **Publiée par fusion**  
 Indique si la colonne est publiée par fusion.  
  
 **Pas pour la réplication**  
 Indique si les valeurs d'identité d'origine sont préservées pendant la réplication. Pour plus d'informations sur la réplication, consultez CREATE TABLE. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
 **Répliquée**  
 Indique si cette colonne est répliquée dans un autre emplacement.  
  
 **RowGuid**  
 Indique si SQL Server utilise la colonne comme un ROWGUID. Vous pouvez définir cette valeur sur **Oui** uniquement pour une colonne d'identité unique. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
 **Taille**  
 Affiche la taille en octets autorisée par le type de données de la colonne. Par exemple, un type de données nchar peut avoir une longueur de 10 (nombre de caractères), mais une taille de 20 pour tenir compte des jeux de caractères Unicode.  
  
> [!NOTE]  
>  La longueur des types de données **(max)** varie pour chaque ligne. **sp_help** retourne (-1) comme longueur des colonnes **(max)** . [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] affiche -1 comme taille de colonne.  
  
  
