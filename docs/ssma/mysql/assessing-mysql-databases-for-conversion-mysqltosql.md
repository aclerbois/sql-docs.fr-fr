---
title: Évaluation des bases de données MySQL pour la Conversion (MySQLToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Assessment reports
ms.assetid: 2a56a003-3b0f-453a-963c-00c9e40933ec
caps.latest.revision: 10
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 5f87a1260e038cb1421f969983a698989dc66a61
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="assessing-mysql-databases-for-conversion-mysqltosql"></a>Évaluation des bases de données MySQL pour la Conversion (MySQLToSQL)
Avant de charger des objets et de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous devez déterminer combien de temps et de complexité la migration sera prend la migration. SSMA peut créer un rapport d’évaluation qui affiche le pourcentage d’objets qui seront converties correctement. SSMA permet également d’afficher les problèmes spécifiques qui provoquent des échecs de conversion.  
  
## <a name="creating-assessment-reports"></a>Création de rapports d’évaluation  
Lorsqu’il crée ce rapport d’évaluation, SSMA convertit les objets de base de données MySQL sélectionnés à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou syntaxe de SQL Azure, puis affiche les résultats.  
  
**Pour créer un rapport d’évaluation**  
  
1.  Dans l’Explorateur de métadonnées MySQL, sélectionnez les schémas à évaluer.  
  
2.  Pour omettre des objets individuels, désactivez les cases à cocher en regard de ceux.  
  
3.  Avec le bouton droit **schémas**, puis sélectionnez **créer un rapport**.  
  
    Cliquez sur un objet d’analyse des objets individuels. Ensuite, sélectionnez **créer un rapport**.  
  
    SSMA affiche la progression dans la barre d’état en bas de la fenêtre. Si le volet de résultats est visible, vous verrez également les messages dans le volet de sortie.  
  
    Une fois l’évaluation terminée, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant pour MySQL, la fenêtre de rapport d’évaluation s’affiche.  
  
## <a name="using-assessment-reports"></a>À l’aide de rapports d’évaluation  
La fenêtre de rapport d’évaluation contient trois volets :  
  
-   Le volet gauche contient la hiérarchie des objets qui sont inclus dans le rapport d’évaluation. Vous pouvez parcourir la hiérarchie et sélectionner les objets et les catégories d’objets à afficher le code et les statistiques de conversion.  
  
-   Le contenu du volet droit dépend de l’élément est sélectionné dans le volet gauche.  
  
    Si un groupe d’objets est sélectionné, telles que le schéma, le volet droit contient un volet Statistiques de Conversion et les objets par le volet des catégories. Le volet des statistiques de Conversion affiche les statistiques de conversion pour les objets sélectionnés. Les objets par le volet des catégories affiche les statistiques de conversion pour l’objet ou les catégories d’objets.  
  
    Si une fonction, la procédure, la table ou la vue est sélectionnée, le volet de droite contient des statistiques, de code source et de code cible.  
  
    -   La zone supérieure affiche les statistiques globales pour l’objet. Vous devrez peut-être développer **statistiques** pour afficher ces informations.  
  
    -   La zone Source montre le code source de l’objet sélectionné dans le volet gauche. Les zones en surbrillance affichent le code source problématique.  
  
    -   La zone cible affiche le code converti. Texte rouge affiche des messages d’erreur et le code problématiques.  
  
-   Le volet inférieur affiche les messages de la conversion, regroupés par numéro de message. Vous pouvez cliquer sur **erreurs**, **avertissements**, ou **Info** à afficher les catégories de messages, puis développez un groupe de messages. Cliquez sur un message individuel, sélectionnez l’objet dans le volet gauche et afficher les détails dans le volet droit.  
  
## <a name="analyzing-conversion-problems-by-using-the-assessment-report"></a>Analyse des problèmes de Conversion à l’aide de l’état d’évaluation  
Le volet des statistiques de Conversion affiche les statistiques de conversion. Si le pourcentage pour n’importe quelle catégorie est inférieure à 100 %, vous devez déterminer la raison pour laquelle la conversion n’a pas réussi.  
  
**Pour afficher les problèmes de conversion**  
  
1.  Créer le rapport d’évaluation en utilisant les instructions dans la procédure précédente.  
  
2.  Dans le volet gauche, développez les schémas ou des dossiers qui ont une icône d’erreur rouge. Continuez à développer les éléments jusqu'à ce que vous sélectionnez un élément individuel qui Échec de la conversion.  
  
3.  En haut du volet Source, cliquez sur **problème suivant**.  
  
    Le code problématique est mis en surbrillance, comme le code connexe dans le volet de Navigation de la cible.  
  
4.  Passez en revue les messages d’erreur, puis déterminez ce que vous voulez faire avec l’objet qui a provoqué le problème de conversion.  
  
-   Mettre à jour la syntaxe MySQL dans SSMA. Vous pouvez mettre à jour la syntaxe uniquement pour les procédures et fonctions. Pour mettre à jour la syntaxe, sélectionnez l’objet dans le volet Explorateur de métadonnées MySQL, cliquez sur le **SQL** onglet et modifiez le code SQL. Lorsque vous quittez l’élément, vous devrez enregistrer la syntaxe de la mise à jour. Vous pouvez afficher les erreurs signalées pour l’objet sur le **rapport** onglet.  
  
-   Dans MySQL, vous pouvez modifier l’objet MySQL pour supprimer ou modifier tout code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [se connecter à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md).  
  
-   Vous pouvez exclure l’objet de la migration. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de l’Explorateur de métadonnées SQL Azure et l’Explorateur de métadonnées MySQL, désactivez la case à cocher en regard de l’élément avant de charger les objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure et migrer les données de MySQL.  
  
## <a name="next-step"></a>Étape suivante  
[Conversion des bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données de migration de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
