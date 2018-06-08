---
title: Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation | Microsoft Docs
ms.custom: ''
ms.date: 02/15/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: get-started-article
ms.assetid: ea3db39b-698b-4a74-8eb8-21dc7252dc1a
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 20a36a42ca3ce3cd7511dbf36896827f156b6bd3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-this-simple-example-of-the-import-and-export-wizard"></a>Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation
Découvrez la série des opérations de l’Assistant Importation et exportation SQL Server en suivant un scénario courant : l’importation de données à partir d’une feuille de calcul Excel dans une base de données SQL Server. Même si vous envisagez d’utiliser une source et une destination différentes, cette rubrique vous montre l’essentiel de ce que vous devez savoir sur l’exécution de l’Assistant.

## <a name="prerequisite---is-the-wizard-installed-on-your-computer"></a>Prérequis - l’Assistant est-il installé sur votre ordinateur ?
Si vous souhaitez exécuter l’Assistant, mais [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas installé sur votre ordinateur, vous pouvez installer l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en installant SSDT (SQL Server Data Tools). Pour plus d’informations, consultez [Télécharger SSDT (SQL Server Data Tools)](https://msdn.microsoft.com/library/mt204009.aspx).

## <a name="heres-the-excel-source-data-for-this-example"></a>Voici les données sources Excel pour cet exemple
Voici les données sources que vous allez copier : une petite table de deux colonnes dans la feuille de calcul WizardWalkthrough du classeur Excel WizardWalkthrough.xlsx.

![Données sources Excel](../../integration-services/import-export-data/media/excel-source-data.jpg)

## <a name="heres-the-sql-server-destination-database-for-this-example"></a>Voici la base de données de destination SQL Server pour cet exemple
Voici (dans SQL Server Management Studio) la base de données de destination SQL Server dans laquelle vous souhaitez copier les données sources. La table de destination n’existant pas, vous allez utiliser l’Assistant pour qu’il crée cette table pour vous.

![Base de données de destination SQL Server](../../integration-services/import-export-data/media/sql-server-destination-database.jpg)

## <a name="step-1---start-the-wizard"></a>Étape 1 - Démarrage de l’Assistant
Vous démarrez l’Assistant à partir du groupe Microsoft SQL Server 2016 dans le menu Démarrer de Windows.

![Démarrer l’Assistant](../../integration-services/import-export-data/media/start-wizard.jpg)

> [!NOTE]
> Pour cet exemple, vous choisissez l’Assistant 32 bits, car vous utilisez la version 32 bits de Microsoft Office. Vous devez donc utiliser le fournisseur de données 32 bits pour vous connecter à Excel. En général, pour de nombreuses autres sources de données, vous pouvez choisir l’Assistant 64 bits.
>
> Pour utiliser la version 64 bits de l’Assistant Importation et Exportation SQL Server, vous devez installer SQL Server. SQL Server Data Tools (SSDT) et SQL Server Management Studio (SSMS) sont des applications 32 bits qui installent uniquement des fichiers 32 bits, y compris la version 32 bits de l’Assistant.

Pour plus d’informations, consultez [Démarrer l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md).

## <a name="step-2---view-the-welcome-page"></a>Étape 2 - Affichage de la page d’accueil
La première page de l’Assistant est la page **Bienvenue**. 

Vous ne souhaitez probablement plus afficher cette page, ainsi n’hésitez pas à cliquer sur **Ne plus afficher cette page de démarrage**.

![Bienvenue dans l’Assistant](../../integration-services/import-export-data/media/welcome-to-the-wizard.jpg)

## <a name="step-3---pick-excel-as-your-data-source"></a>Étape 3 - Choix d’Excel comme source de données
À la page suivante, **Choisir une source de données**, vous sélectionnez Microsoft Excel comme source de données. Vous recherchez ensuite le fichier Excel pour le sélectionner. Vous spécifiez enfin la version d’Excel que vous avez utilisée pour créer le fichier.

> [!IMPORTANT]
> Pour obtenir des informations détaillées sur la connexion à des fichiers Excel, et sur les limitations et les problèmes connus liés au chargement de données depuis ou vers des fichiers Excel, consultez [Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md).

![Choix de la source de données Excel](../../integration-services/import-export-data/media/choose-the-excel-data-source.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md).

## <a name="step-4---pick-sql-server-as-your-destination"></a>Étape 4 - Choix de SQL Server en tant que destination
À la page suivante, **Choisir une destination**, vous sélectionnez Microsoft SQL Server comme destination en désignant dans la liste un fournisseur de données qui se connecte à SQL Server. Dans cet exemple, vous sélectionnez le **Fournisseur de données .NET Framework pour SQL Server**.

La page affiche une liste de propriétés du fournisseur. Bon nombre d’entre elles comportent des noms inconnus et des paramètres inhabituels. Heureusement, pour vous connecter à une base de données d’entreprise, vous ne devez généralement fournir que trois éléments d’information. Vous pouvez ignorer les valeurs par défaut des autres paramètres.

|Informations nécessaires|Propriété du fournisseur de données .NET Framework pour SQL Server|
|---|---|
|Nom du serveur|**Source de données**|
|Informations d’authentification (connexion)|**Sécurité intégrée** ; ou **ID d’utilisateur** et **Mot de passe**<br/>Si vous voulez afficher la liste déroulante des bases de données sur le serveur, vous devez d’abord fournir des informations de connexion valides.|
|Nom de la base de données|**Catalogue initial**|

![Choix de la destination SQL Server](../../integration-services/import-export-data/media/choose-the-sql-server-destination.jpg)

Pour plus d’informations sur la connexion à SQL Server, consultez [Se connecter à une source de données SQL Server](../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md). Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md).

## <a name="step-5---copy-a-table-instead-of-writing-a-query"></a>Étape 5 - Copie d’une table au lieu d’écrire une requête
À la page suivante, **Spécifier la copie ou l’interrogation de table**, vous spécifiez que vous souhaitez copier la table de la source de données dans son intégralité. Vous ne voulez pas écrire de requête en langage SQL pour sélectionner les données à copier.

![Activation de l’option de copie d’une table](../../integration-services/import-export-data/media/specify-to-copy-a-table.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Spécifier la copie ou l’interrogation de table](../../integration-services/import-export-data/specify-table-copy-or-query-sql-server-import-and-export-wizard.md).

## <a name="step-6---pick-the-table-to-copy"></a>Étape 6 - Choix de la table à copier
À la page suivante, **Sélectionner les tables et les vues sources**, vous choisissez la ou les tables que vous souhaitez copier à partir de la source de données. Vous mappez ensuite la table source sélectionnée à la table de destination nouvelle ou existante.

Dans cet exemple, par défaut l’Assistant a mappé la feuille de calcul **WizardWalkthrough$** de la colonne **Source** à une nouvelle table portant le même nom dans la destination SQL Server. (Le classeur Excel ne contient qu’une feuille de calcul.)
-   Le symbole dollar ($) dans le nom de la table source indique une feuille de calcul Excel. (Une plage nommée dans Excel est représentée par son seul nom.)
-   L’étoile sur l’icône de la table de destination indique que l’Assistant va créer une table de destination.

![Sélection de la table (avant le changement de nom)](../../integration-services/import-export-data/media/select-the-table-before-renaming.jpg)

Vous voulez probablement supprimer le signe dollar ($) dans le nom de la nouvelle table de destination.

![Sélection de la table (après le changement de nom)](../../integration-services/import-export-data/media/select-the-table-after-renaming.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Sélectionner les tables et les vues sources](../../integration-services/import-export-data/select-source-tables-and-views-sql-server-import-and-export-wizard.md).

## <a name="optional-step-7---review-the-column-mappings"></a>Étape facultative 7 - Vérification du mappage de colonnes
Avant de quitter la page **Sélectionner les tables et les vues sources**, le cas échéant cliquez sur le bouton **Modifier les mappages** pour ouvrir la boîte de dialogue **Mappage de colonnes**. Ici, dans la table **Mappages**, vous voyez comment l’Assistant va mapper les colonnes de la feuille de calcul source avec les colonnes de la nouvelle table de destination.

![Affichage du mappage de colonnes](../../integration-services/import-export-data/media/view-column-mappings.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Mappage de colonnes](../../integration-services/import-export-data/column-mappings-sql-server-import-and-export-wizard.md).

## <a name="optional-step-8---review-the-create-table-statement"></a>Étape facultative 8 - Vérification de l’instruction CREATE TABLE
Tandis que la boîte de dialogue **Mappage de colonnes** est ouverte, cliquez le cas échéant sur le bouton **Modifier SQL** pour ouvrir la boîte de dialogue **Instruction SQL de création de table**. Vous affichez ici l’instruction **CREATE TABLE** qui est générée par l’Assistant pour créer la table de destination. Généralement, vous n’avez pas besoin de modifier l’instruction.

![Affichage de l’instruction CREATE TABLE](../../integration-services/import-export-data/media/view-create-table-statement.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Instruction SQL de création de table](../../integration-services/import-export-data/create-table-sql-statement-sql-server-import-and-export-wizard.md).

## <a name="optional-step-9---preview-the-data-to-copy"></a>Étape facultative 9 - Affichage d’un aperçu des données à copier
Après avoir cliqué sur **OK** pour fermer la boîte de dialogue **Instruction SQL de création de table**, puis à nouveau sur **OK** pour fermer la boîte de dialogue **Mappage de colonnes**, vous êtes de retour à la page **Sélectionner les tables et les vues sources**. Si vous le voulez, vous pouvez cliquer sur le bouton **Aperçu** pour afficher un échantillon des données que l’Assistant va copier. Dans cet exemple, tout est normal.

![Affichage d’un aperçu des données à copier](../../integration-services/import-export-data/media/preview-data-to-copy.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Aperçu des données](../../integration-services/import-export-data/preview-data-dialog-box-sql-server-import-and-export-wizard.md).

## <a name="step-10---yes-you-want-to-run-the-import-export-operation"></a>Étape 10 - Oui, vous souhaitez exécuter l’opération d’importation ou d’exportation
À la page suivante, **Enregistrer et exécuter le package**, vous conservez l’option **Exécuter immédiatement** activée pour copier les données lorsque vous cliquez sur **Terminer** à la page suivante. Sinon, vous pouvez ignorer la page suivante en cliquant sur **Terminer** dans la page **Enregistrer et exécuter le Package**.

![Exécution du package](../../integration-services/import-export-data/media/run-the-package.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Enregistrer et exécuter le Package](../../integration-services/import-export-data/save-and-run-package-sql-server-import-and-export-wizard.md).

## <a name="step-11---finish-the-wizard-and-run-the-import-export-operation"></a>Étape 11 - Fin de l’Assistant et exécution de l’opération d’importation ou d’exportation
Si vous avez cliqué sur **Suivant** au lieu de **Terminer** dans la page **Enregistrer et exécuter le Package**, dans la page suivante, **Terminer l’Assistant**, vous voyez un récapitulatif des opérations que l’Assistant va réaliser. Cliquez sur **Terminer** pour exécuter l’opération d’importation ou d’exportation.

![Terminer l’Assistant](../../integration-services/import-export-data/media/complete-the-wizard.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Terminer l’Assistant](../../integration-services/import-export-data/complete-the-wizard-sql-server-import-and-export-wizard.md).

## <a name="step-12---review-what-the-wizard-did"></a>Étape 12 - Vérification des tâches effectuées par l’Assistant
À la dernière page, remarquez chaque tâche s’afficher au fur et à mesure qu’elles sont achevées par l’Assistant et contrôlez les résultats. La ligne mise en évidence indique que l’Assistant a correctement copié vos données. Vous avez terminé !

![Exécution de l’Assistant réussie](../../integration-services/import-export-data/media/the-wizard-succeeded.jpg)

Pour obtenir plus d’informations sur cette page de l’Assistant, consultez [Exécution de l’opération](../../integration-services/import-export-data/performing-operation-sql-server-import-and-export-wizard.md).

## <a name="heres-the-new-table-of-data-copied-to-sql-server"></a>Voici la nouvelle table de données copiée dans SQL Server
Ici (dans SQL Server Management Studio), vous voyez la nouvelle table de destination créée par l’Assistant dans SQL Server.

![Données copiées dans SQL Server](../../integration-services/import-export-data/media/data-copied-to-sql-server.jpg)

Ici (de nouveau dans SSMS), vous voyez les données que l’Assistant a copiées dans SQL Server.

![Données copiées dans SQL Server 2](../../integration-services/import-export-data/media/data-copied-to-sql-server-2.jpg)

## <a name="learn-more"></a>En savoir plus  
Approfondir ses connaissances sur le fonctionnement de l’Assistant.
-   **Découvrez-en plus sur l’Assistant.** Si vous recherchez une vue d’ensemble de l’Assistant, consultez [Importer et exporter des données avec l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

-   **Découvrez-en plus sur les étapes de l’Assistant.** Si vous recherchez des informations sur les étapes de l’Assistant, sélectionnez la page qui vous intéresse à partir de la liste ici : [Étapes de l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard.md). Il existe aussi une page de documentation propre à chaque page de l’Assistant.

-   **Découvrez comment se connecter à des sources et des destinations de données.** Si vous avez besoin d’informations sur la façon de vous connecter à vos données, sélectionnez la page qui vous intéresse à partir de la liste ici : [Se connecter à des sources de données avec l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/connect-to-data-sources-with-the-sql-server-import-and-export-wizard.md). Il existe une page de documentation propre à chacune des différentes sources de données couramment utilisées.

-   **Découvrez-en plus sur le chargement de données depuis et vers Excel.** Pour obtenir des informations sur la connexion à des fichiers Excel, et sur les limitations et les problèmes connus liés au chargement de données depuis ou vers des fichiers Excel, consultez [Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md).