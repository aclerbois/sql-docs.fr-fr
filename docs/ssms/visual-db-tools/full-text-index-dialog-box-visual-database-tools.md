---
title: Index de texte intégral, boîte de dialogue (Visual Database Tools) | Microsoft Docs
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
- vdt.dlgbox.fulltextindex
ms.assetid: ef45b585-2567-4abe-b421-9fd0994e0146
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 901703f5347cb7b2e1d152c69868d6442da76a7e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="full-text-index-dialog-box-visual-database-tools"></a>Boîte de dialogue Index de texte intégral (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Cette boîte de dialogue permet de créer un index de texte intégral si vous souhaitez effectuer des recherches en texte intégral sur les colonnes de type texte de vos tables de base de données. Un index de texte intégral se base sur un index normal ; vous devez donc d'abord le créer. L'index normal doit être créé sur une colonne unique, non null ; il est conseillé de choisir une colonne contenant des petites valeurs plutôt que des grandes.  
  
> [!NOTE]  
> Pour créer un index de texte intégral, vous devez d'abord créer un catalogue de texte intégral pour la base de données à l'aide d'un outil extérieur, tel que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] ou Enterprise Manager.  
  
> [!NOTE]  
> La fonctionnalité d’index de texte intégral n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2012](http://msdn.microsoft.com/en-us/5da61ff5-12b9-48e6-b3c8-0dacca1751c4).  
  
## <a name="options"></a>Options  
**Index de texte intégral sélectionné**  
Répertorie les index de texte intégral existants. Sélectionnez un index pour afficher ses propriétés dans la partie droite de la grille. Si la liste est vide, aucune relation de texte intégral n'est définie pour la table.  
  
**Ajouter**  
Crée un nouvel index de texte intégral.  
  
**Supprimer**  
Supprime l’index de texte intégral sélectionné dans la liste **Index de texte intégral sélectionné** .  
  
**Catégorie Général**  
Développée, elle affiche les propriétés **Colonnes** et **Nom de catalogue de texte intégral**.  
  
**Colonnes**  
Affiche une liste avec la virgule comme séparateur des noms de colonnes pouvant faire l'objet d'une recherche en texte intégral. Pour afficher la liste complète, cliquez sur le bouton de sélection **(...)**, à gauche du champ de propriété.  
  
**Full-Text Catalog Name**  
Affiche le nom du catalogue de texte intégral dans lequel cet index de texte intégral est stocké. Pour stocker l'index dans un autre catalogue, cliquez sur le nom du catalogue et choisissez-en un autre dans la liste déroulante.  
  
> [!NOTE]  
> Le catalogue doit d'abord être créé dans un outil extérieur, tel que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] ou Enterprise Manager.  
  
**Catégorie Identité**  
Développée, elle affiche le champ Nom de cet index.  
  
**Nom**  
Affiche le nom spécifié par le système de cet index de texte intégral.  
  
**Catégorie Concepteur de tables**  
Développée, elle affiche les propriétés qui dictent le mode d'exécution de l'index.  
  
**Actif**  
Indique si vous pouvez actuellement exécuter une recherche en texte intégral à l'aide de cet index de texte intégral.  
  
**Paramètre du suivi des modifications**  
Décrit le statut du suivi des modifications pour cet index : Manuel, Auto ou Inactif.  
  
**Analyse terminée**  
Indique si l'analyse la plus récente est terminée. Si cette propriété a la valeur Non, une analyse est actuellement en cours.  
  
**Date et heure de fin de l'analyse actuelle ou de la dernière analyse**  
Affiche la date et l'heure de l'achèvement de l'analyse la plus récente.  
  
**Erreurs dans l'analyse actuelle ou dans la dernière analyse**  
Affiche le nombre d'erreurs dans analyse actuelle ou dans la dernière analyse.  
  
**Version d'index**  
Affiche la version du schéma de la table au moment du démarrage de l'analyse.  
  
**Ligne dans l'analyse actuelle ou dans la dernière analyse**  
Affiche le nombre de lignes mises à jour dans l'analyse actuelle ou dans la dernière analyse.  
  
**Date et heure de début de l'analyse actuelle ou de la dernière analyse**  
Affiche la date et l'heure de démarrage de l'analyse actuelle ou de la dernière analyse.  
  
**Horodatage de la prochaine analyse**  
Affiche la date et l'heure de démarrage de la prochaine analyse.  
  
**Type de l'analyse actuelle ou de la dernière analyse**  
Affiche le type de l'analyse actuelle ou de la dernière analyse : Complet, Incrémentiel, Mettre à jour ou Propagation automatique.  
  
**Nom d'index unique**  
Affiche la liste de tous les noms de colonnes de cette base de données qui possèdent des index uniques à une seule colonne. Ces colonnes peuvent être utilisées pour créer un index de texte intégral.  
  
## <a name="see-also"></a> Voir aussi  
[Utiliser l'Assistant Indexation de texte intégral](http://msdn.microsoft.com/en-us/3e9d9605-6525-4781-9168-fdaa06db3459)  
[CREATE FULLTEXT INDEX (Transact-SQL)](http://msdn.microsoft.com/en-us/8b80390f-5f8b-4e66-9bcc-cabd653c19fd)  
  
