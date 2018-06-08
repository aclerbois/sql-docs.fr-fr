---
title: Évaluation des objets de base de données SAP ASE pour la Conversion (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: eb996b7c-1eef-4f73-b5e6-2fa6faf7336c
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 7f28d8f35adacfa4443ed804e1233386f3794606
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="assessing-sap-ase-database-objects-for-conversion-sybasetosql"></a>Évaluation des objets de base de données SAP ASE pour la conversion (SybaseToSQL)
Avant de charger des objets et de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous devez déterminer comment la complexité de la migration et combien de temps il doit prendre. SSMA peut créer un rapport d’évaluation qui affiche le pourcentage des objets et des procédures qui seront correctement converties en [!INCLUDE[tsql](../../includes/tsql_md.md)]. SSMA permet également d’afficher les problèmes spécifiques qui peuvent entraîner des échecs de conversion.  
  
## <a name="create-assessment-reports"></a>Créer des rapports d’évaluation  
Lors de la création de ce rapport d’évaluation, SSMA convertit les objets de base de données SAP Adaptive Server Enterprise (ASE) sélectionnés vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou la syntaxe SQL de Azure, puis affiche les résultats.  
  
**Pour créer un rapport d’évaluation**  
  
1.  Dans l’Explorateur de métadonnées de Sybase, sélectionnez les bases de données que vous souhaitez évaluer.  
  
2.  Pour omettre des objets individuels, désactivez les cases à cocher en regard des objets que vous ne souhaitez pas évaluer.  
  
3.  Avec le bouton droit **bases de données**, puis sélectionnez **créer un rapport**.  
  
    Vous pouvez également analyser les objets individuels en cliquant sur un objet, puis en sélectionnant **créer un rapport**.  
  
    SSMA affiche la progression dans la barre d’état en bas de la fenêtre. Si le volet de résultats est visible, vous verrez également tous les messages connexes.  
  
    Une fois l’évaluation terminée, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant pour Sybase : état de l’évaluation de la fenêtre s’affiche.  
  
## <a name="use-assessment-reports"></a>Utiliser les rapports d’évaluation  
La fenêtre de rapport d’évaluation contient trois volets :  
  
-   Le volet gauche contient la hiérarchie des objets qui sont inclus dans le rapport d’évaluation. Vous pouvez parcourir la hiérarchie et sélectionner des objets et des catégories d’objet pour afficher le code et les statistiques de conversion.  
  
-   Le contenu du volet droit varie en fonction de l’élément est sélectionné dans le volet gauche.  
  
    Si un groupe d’objets (par exemple, un schéma) ou d’une table est sélectionné, le volet droit affiche deux volets. Le **des statistiques de Conversion** volet affiche les statistiques de conversion pour les objets sélectionnés. Le **les objets par catégories** volet affiche les statistiques de conversion de l’objet ou les catégories d’objets.  
  
    Si une procédure stockée, une vue ou un déclencheur est sélectionnée, le volet de droite contient des statistiques, de code source et de code cible.  
  
    -   La zone supérieure affiche les statistiques globales pour l’objet. Vous devrez peut-être développer **statistiques** pour afficher ces informations. 
    -   La zone Source montre le code source de l’objet sélectionné dans le volet gauche. Les zones en surbrillance affichent le code source problématique.  
    -   La zone cible affiche le code converti. Texte rouge affiche des messages d’erreur et le code problématiques.  
  
-   Le volet inférieur affiche les messages de la conversion, regroupés par numéro de message. Sélectionnez **erreurs**, **avertissements**, ou **Info** à afficher les catégories de messages, puis développez un groupe de messages. Cliquez sur un message individuel pour sélectionner l’objet dans le volet de gauche puis d’afficher les détails dans le volet droit.  
  
## <a name="analyze-conversion-problems-by-using-the-assessment-report"></a>Analyser les problèmes de conversion à l’aide du rapport d’évaluation  
Le **les volets des statistiques de Conversion** affichent les statistiques de conversion. Si le pourcentage pour n’importe quelle catégorie est inférieure à 100 %, vous devez déterminer la raison pour laquelle la conversion n’a pas réussi.  
  
**Pour afficher les problèmes de conversion**  
  
1.  Créer le rapport d’évaluation en utilisant les instructions dans la procédure précédente.  
  
2.  Dans le volet gauche, développez les schémas ou des dossiers qui ont une icône d’erreur rouge. Continuez à développer les éléments jusqu'à ce que vous sélectionnez un élément individuel qui Échec de la conversion.  
  
3.  En haut du volet Source, sélectionnez **problème suivant**.  
    Le code problématique est mise en surbrillance, comme le code connexe dans le **cible Navigation** volet.  
  
4.  Passez en revue les messages d’erreur, puis déterminez ce que vous voulez faire avec l’objet qui a provoqué le problème de conversion :  
  
    -   Mettre à jour la syntaxe ASE dans SSMA. Vous pouvez mettre à jour la syntaxe uniquement pour les procédures stockées et les déclencheurs. Pour mettre à jour la syntaxe, sélectionnez l’objet dans le volet Explorateur de métadonnées Sybase, cliquez sur le **SQL** onglet, puis modifier le code SQL. Lorsque vous quittez l’élément, vous êtes invité à enregistrer la syntaxe de la mise à jour. Afficher les erreurs signalées pour l’objet sur le **rapport** onglet.  
  
    -   Dans ASE, vous pouvez modifier l’objet ASE pour supprimer ou modifier tout code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [connexion à Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).  
  
    -   Vous pouvez exclure l’objet de la migration. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de l’Explorateur de métadonnées SQL Azure et l’Explorateur de métadonnées Sybase, désactivez la case à cocher en regard de l’élément avant de charger les objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure et migrer les données à partir de ASE.
  
## <a name="next-steps"></a>Étapes suivantes  
[Convertir des objets de base de données ASE SAP &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration SAP ASE bases de données SQL Server : base de données SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
