---
title: Propriétés de la requête (Visual Database Tools) | Microsoft Docs
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
- vdtsql.chm:69636
- vdt.ppg.querydesigner.query
ms.assetid: 07495669-6ed5-4004-904e-aae1230be5e4
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ecdcf360a7aad1b9468367cc706849d4da2f90c0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="query-properties-visual-database-tools"></a>Propriétés de la requête (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Ces propriétés apparaissent dans la fenêtre Propriétés lorsque vous avez une requête ouverte dans le Concepteur de requêtes et de vues. Sauf indication contraire, vous pouvez modifier ces propriétés dans la fenêtre Propriétés.  
  
> [!NOTE]  
> Les propriétés mentionnées dans cette rubrique sont classées par catégorie et non par ordre alphabétique.  
  
## <a name="options"></a>Options  
**Catégorie Identité**  
Se développe pour afficher la propriété **Nom** .  
  
**Nom**  
Affiche le nom de la requête en cours. Ne peut pas être modifié dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)].  
  
**Database Name**  
Indique le nom de la source de données pour la table sélectionnée.  
  
**Nom de serveur**  
Affiche le nom du serveur de la source de données.  
  
**Catégorie Concepteur de requêtes**  
S'étend pour afficher les propriétés restantes.  
  
**Table de destination**  
Spécifie le nom de la table dans laquelle vous insérez des données. Cette liste s'affiche si vous créez une requête INSERT ou MAKE TABLE. Pour une requête INSERT, sélectionnez un nom de table dans la liste.  
  
Pour une requête MAKE TABLE, tapez le nom de la nouvelle table. Pour créer une table de destination dans une autre source de données, spécifiez un nom de table qualifié complet, y compris le nom de la source de données cible, le propriétaire (le cas échéant) et le nom de la table.  
  
> [!NOTE]  
> Le Concepteur de requêtes ne vérifie pas si le nom est déjà utilisé, ni si vous avez l'autorisation de créer la table.  
  
**Valeurs DISTINCT**  
Spécifie que la requête filtrera les doublons dans le jeu de résultats. Cette option est utile lorsque vous utilisez uniquement certaines colonnes de la table ou des tables et que ces colonnes peuvent contenir des valeurs dupliquées, ou encore lorsque le processus de jointure de plusieurs tables crée des lignes en double dans le jeu de résultats. Choisir cette option équivaut à insérer le mot DISTINCT dans l'instruction à l'intérieur du volet SQL.  
  
**Extension GROUP BY**  
Spécifie que d'autres options sont disponibles pour les requêtes basées sur des requêtes d'agrégation. (S'applique uniquement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].)  
  
**Sélectionne toutes les colonnes**  
Spécifie que l'ensemble des colonnes de toutes les tables dans la requête en cours figureront dans le jeu de résultats. Choisir cette option équivaut à spécifier un astérisque (*) à la place des noms de colonnes individuels après le mot clé SELECT dans l'instruction SQL.  
  
**Liste de paramètres de la requête**  
Affiche les paramètres de la requête. Pour modifier les paramètres, cliquez sur la propriété, puis cliquez sur le bouton de sélection **(…)** situé à droite de la propriété. (S'applique uniquement aux bases de données OLE DB génériques.)  
  
**Commentaire SQL**  
Affiche une description des instructions SQL. Pour afficher l’intégralité de la description, ou la modifier, cliquez sur la description, puis sur le bouton de sélection **(…)** situé à droite de la propriété. Vos commentaires peuvent contenir les noms des utilisateurs de la requête et le moment d'utilisation. (S'applique uniquement aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] version 7.0 ou ultérieure.)  
  
**Catégorie Spécification Top**  
Se développe pour afficher les propriétés **Haut**, **Pour cent**, **Expression**et **Avec liens** .  
  
**(Top)**  
Indique que la requête doit contenir une clause TOP, qui ne renvoie que les *n* premières lignes ou *n* pour cent des lignes du jeu de résultats (en commençant par les premières). Par défaut, la requête retourne les 10 premières lignes dans le jeu de résultats.  
  
Utilisez cette zone pour modifier le nombre de lignes à retourner ou pour spécifier un autre pourcentage. (S'applique uniquement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] version 7.0 ou ultérieure.)  
  
**Expression**  
Spécifie le nombre ou le pourcentage de lignes que la requête doit retourner. Si vous affectez à **Pour cent** la valeur Oui, le nombre indique le pourcentage de lignes que la requête doit retourner ; si vous affectez à **Pour cent** la valeur Non, le nombre représente le nombre de lignes à retourner. (S'applique uniquement aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] version 7.0 ou ultérieure.)  
  
**Pour cent**  
Spécifie que la requête doit retourner uniquement les *n* premiers pour cent de lignes dans le jeu de résultats. (S'applique uniquement aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] version 7.0 ou ultérieure.)  
  
**Avec liens**  
Spécifie que la vue inclura une clause WITH TIES. WITH TIES est utile si une vue inclut une clause ORDER BY et une clause TOP basée sur un pourcentage. Si cette option est activée et si le pourcentage s'arrête au milieu d'un groupe de lignes auxquelles correspondent des valeurs identiques dans la clause ORDER BY, la vue est agrandie de façon à inclure ces lignes. (S'applique uniquement aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] version 7.0 ou ultérieure.)  
  
## <a name="see-also"></a> Voir aussi  
[Requête avec des paramètres &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-parameters-visual-database-tools.md)  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
