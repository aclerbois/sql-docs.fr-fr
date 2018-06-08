---
title: Propriétés de la colonne (Visual Database Tools) | Microsoft Docs
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
- vdt.designers.properties.Column.ColumnIdentitySpec
- vdt.designers.properties.Column
- vdt.tablecolumn
- vdt.designers.properties.Column.ColumnComputedColumnSpec
- vdt.designers.properties.Column.ColumnFulltextSpec
ms.assetid: e549a2a8-4154-4ec8-b146-614564169b39
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2e43a42c781ae0133f7d1644d6ea0c82e3197bef
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-properties-visual-database-tools"></a>Propriétés de la colonne (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Il existe deux ensembles de propriétés pour les colonnes : un jeu complet que vous pouvez consulter sous l’onglet **Propriétés de la colonne** dans le Concepteur de tables (disponible uniquement pour les bases de données [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ) et un sous-ensemble que vous pouvez consulter dans la fenêtre Propriétés à l’aide de l’Explorateur de serveurs.  
  
> [!NOTE]  
> Les propriétés mentionnées dans cette rubrique sont classées par catégorie et non par ordre alphabétique.  
  
> [!NOTE]  
> Les boîtes de dialogue et les commandes de menu affichées peuvent différer de celles décrites dans l'Aide selon les paramètres actifs ou le mode d'édition. Pour modifier vos paramètres, choisissez **Paramètres d'importation et d'exportation** dans le menu **Outils** .  
  
## <a name="properties-window"></a>Fenêtre Propriétés  
Ces propriétés s'affichent dans la fenêtre Propriétés lorsque vous sélectionnez une colonne dans l'Explorateur de serveurs.  
  
> [!NOTE]  
> Ces propriétés, accessibles dans l'Explorateur de serveurs, sont en lecture seule. Pour modifier les propriétés d'une colonne pour les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , sélectionnez la colonne dans le Concepteur de tables. Ces propriétés sont décrites plus loin dans cette rubrique.  
  
**Catégorie Identité**  
Peut être développée pour afficher les propriétés **Nom** et **Base de données** .  
  
**Nom**  
Indique le nom de la colonne.  
  
**Sauvegarde de la base de données**  
Affiche le nom de la source de données pour la colonne sélectionnée. (S'applique uniquement à OLE DB.)  
  
**Catégorie Divers**  
S'étend pour afficher les propriétés restantes.  
  
**Type de données**  
Affiche le type de données de la colonne sélectionnée. Pour plus d’informations, consultez [Types de données (Transact-SQL)](http://msdn.microsoft.com/en-us/a54f7373-b247-4d61-8fb8-7f2ec7a8d0a4).  
  
**Incrément d'identité**  
Indique l’incrément qui sera ajouté à **Valeur initiale de la propriété Identity** pour chaque ligne ultérieure de la colonne d’identité. (S'applique uniquement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].)  
  
**Valeur initiale de la propriété Identity**  
Affiche la valeur de départ affectée à la première ligne de la table pour la colonne d'identité. (S'applique uniquement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].)  
  
**Est d'identité**  
Détermine si la colonne sélectionnée est la colonne identité de la table. (S'applique uniquement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].)  
  
**Longueur**  
Affiche le nombre de caractères autorisés pour les types de données texte.  
  
**Nullable**  
Indique si la colonne autorise ou non les valeurs Null.  
  
**Précision**  
Affiche le nombre maximal de chiffres autorisés pour les types de données numériques. Cette propriété affiche **0** pour les types de données non numériques.  
  
**Échelle**  
Affiche le nombre maximal de chiffres qui peuvent apparaître à droite de la virgule décimale pour les types de données numériques. Cette valeur doit être inférieure ou égale à la précision. Cette propriété affiche **0** pour les types de données non numériques.  
  
## <a name="column-properties-tab"></a>Onglet Propriétés de la colonne  
Pour accéder à ces propriétés, dans l’Explorateur de serveurs, cliquez avec le bouton droit sur la table contenant la colonne, choisissez **Ouvrir la définition de table**, puis sélectionnez la ligne dans la grille de table du Concepteur de tables.  
  
> [!NOTE]  
> Ces propriétés s'appliquent uniquement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
**Catégorie Général**  
Peut être développée pour afficher **Nom**, **Autoriser les valeurs NULL**, **Type de données**, **Valeur ou liaison par défaut**, **Longueur**, **Précision**et **Échelle**.  
  
**Nom**  
Affiche le nom de la colonne. Pour modifier le nom, tapez-le dans la zone de texte.  
  
> [!CAUTION]  
> S'il existe des requêtes, des vues, des fonctions définies par l'utilisateur, des procédures stockées ou des programmes qui font référence à la colonne, la modification du nom rend ces objets non valides.  
  
**Null autorisé**  
Précise si le type de données de la colonne autorise les valeurs NULL.  
  
**Type de données**  
Affiche le type de données de la colonne sélectionnée. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur. Pour plus d’informations, consultez [Types de données (Transact-SQL)](http://msdn.microsoft.com/en-us/a54f7373-b247-4d61-8fb8-7f2ec7a8d0a4).  
  
**Valeur ou liaison par défaut**  
Affiche la valeur par défaut de cette colonne quand aucune valeur n'y est spécifiée. La liste déroulante contient toutes les valeurs par défaut globales définies dans la source de données. Pour lier la colonne à une valeur par défaut globale, sélectionnez-en une dans la liste déroulante. Ou bien, pour créer une contrainte par défaut pour la colonne, entrez la valeur par défaut directement comme un texte.  
  
**Longueur**  
Affiche le nombre de caractères autorisés pour les types de données texte. Cette propriété est disponible uniquement pour les types de données basés sur les caractères.  
  
**Précision**  
Affiche le nombre maximal de chiffres autorisés pour les types de données numériques. Cette propriété affiche **0** pour les types de données non numériques. Cette propriété est disponible uniquement pour les types de données numériques.  
  
**Échelle**  
Affiche le nombre maximal de chiffres qui peuvent apparaître à droite de la virgule décimale pour les types de données numériques. Cette valeur doit être inférieure ou égale à la précision. Cette propriété affiche **0** pour les types de données non numériques. Cette propriété est disponible uniquement pour les types de données numériques.  
  
**Catégorie Concepteur de tables**  
S'étend pour afficher les propriétés restantes.  
  
**Classement**  
Affiche le paramètre de classement pour la colonne sélectionnée. Pour modifier ce paramètre, cliquez sur **Classement** , puis sur les points de suspension **(…)** à droite de la valeur.  
  
**Catégorie Spécification de la colonne calculée**  
Peut être développée pour afficher les propriétés **Formule** et **Est persistant**. Si la colonne est calculée, la formule est également affichée. Pour modifier la formule, développez cette catégorie et modifiez-la dans la propriété **Formule** .  
  
**Formule**  
Affiche la formule utilisée par la colonne sélectionnée s'il s'agit d'une colonne calculée. Vous pouvez entrer ou modifier une formule dans ce champ.  
  
**Est persistant**  
Permet d'enregistrer la colonne calculée avec la source de données. Une colonne calculée persistante peut être indexée.  
  
**Type de données condensé**  
Affiche les informations relatives au type de données contenu dans le champ, selon le même format que l'instruction SQL CREATE TABLE. Par exemple, un champ contenant une chaîne de longueur variable d'une longueur maximale de 20 caractères serait représenté comme « varchar(20) ». Pour modifier cette propriété, tapez directement la valeur.  
  
**Description**  
Affiche la description de la colonne. Pour afficher la description complète ou pour la modifier, cliquez sur Description, puis sur les points de suspension **(…)** à droite de la propriété.  
  
**Catégorie Spécification de texte intégral**  
Peut être développée pour afficher les propriétés spécifiques aux colonnes de texte intégral.  
  
**Est d'index de texte intégral**  
Indique si cette colonne est indexée en texte intégral. Cette propriété ne peut prendre la valeur **Oui** que si le type de données de cette colonne peut être soumis à une recherche en texte intégral et si la table contenant cette colonne contient un index de texte intégral. Pour modifier cette valeur, cliquez dessus, développez la liste déroulante et choisissez une autre valeur.  
  
**Colonne de type de texte intégral**  
Affiche la colonne utilisée pour définir le type de document d'une colonne de type image. Le type de données image peut être utilisé pour stocker des documents allant des fichiers .doc aux fichiers xml.  
  
**Langage**  
Indique la langue utilisée pour indexer la colonne.  
  
**Sémantique statistique**  
Sélectionnez s'il faut activer l'indexation sémantique statistique pour la colonne sélectionnée. Pour plus d’informations, consultez [Recherche sémantique (SQL Server)](http://msdn.microsoft.com/en-us/cd8faa9d-07db-420d-93f4-a2ea7c974b97).  
  
Si vous sélectionnez une **langue** avant de sélectionner **Sémantique statistique**, et que la langue sélectionnée n'est pas associée à un modèle linguistique sémantique, l'option **Sémantique statistique** est définie avec la valeur **Non** et ne peut pas être modifiée. Si vous sélectionnez **Oui** pour l'option **Sémantique statistique** avant de sélectionner une **langue**, les langues disponibles dans la colonne **Langue** sont limitées à celles pour lesquelles il existe une prise en charge de modèle linguistique sémantique.  
  
**A un abonné autre que SQL Server**  
Indique si la colonne a un abonné autre que Microsoft SQL Server.  
  
**Catégorie Spécification du compteur**  
Développez pour afficher les propriétés **Est d’identité**, **Incrément d’identité**et **Valeur initiale de la propriété Identity**.  
  
**Est d'identité**  
Détermine si la colonne sélectionnée est la colonne identité de la table. Pour modifier la propriété, ouvrez la table dans le Concepteur de tables et modifiez-la dans la fenêtre **Propriétés** . Ce paramètre s’applique uniquement aux colonnes dont le type de données est basé sur les nombres, comme *int*.  
  
**Incrément d'identité**  
Affiche l’incrément qui sera ajouté à **Valeur initiale de la propriété Identity** à chaque nouvelle ligne. Si vous laissez cette cellule vide, la valeur 1 est affectée par défaut. Pour modifier cette propriété, tapez directement la nouvelle valeur.  
  
**Valeur initiale de la propriété Identity**  
Affiche la valeur attribuée à la première ligne de la table. Si vous laissez cette cellule vide, la valeur 1 est affectée par défaut. Pour modifier cette propriété, tapez directement la nouvelle valeur.  
  
**Est déterministe**  
Indique si le type de données de la colonne sélectionnée peut être déterminé avec certitude.  
  
**Publiée via DTS**  
Indique si la colonne est publiée via DTS.  
  
**Indexable**  
Indique si la colonne sélectionnée peut être indexée. Par exemple, les colonnes calculées non déterministes ne peuvent pas être indexées.  
  
**Publiée par fusion**  
Indique si la colonne est publiée par fusion.  
  
**Pas pour la réplication**  
Indique si les valeurs d'identité d'origine sont préservées pendant la réplication. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
**Répliquée**  
Indique si cette colonne est répliquée dans un autre emplacement.  
  
**Est RowGuid**  
Indique si [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] utilise la colonne en tant que ROWGUID. Vous pouvez définir cette valeur sur **Oui** uniquement pour une colonne dont le type de données est **uniqueidentifier**. Pour modifier cette propriété, cliquez sur sa valeur, développez la liste déroulante et choisissez une autre valeur.  
  
**Taille**  
Affiche la taille en octets autorisée par le type de données de la colonne. Par exemple, un type de données **nchar** peut avoir une longueur de 10 (nombre de caractères), mais une taille de 20 pour tenir compte des jeux de caractères Unicode.  
  
> [!NOTE]  
> La longueur d’un type de données **varchar(max)** varie pour chaque ligne. sp_help retourne (-1) comme longueur de colonne **varchar(max)** . [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] affiche -1 comme taille de colonne.  
  
