---
title: Étapes de l’Assistant Importation et Exportation SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 02/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 816fb1bd-7bb9-450d-ad65-e4c2d02eaff8
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5f79aea129b92293209452bb3dfbd92185e381f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="steps-in-the-sql-server-import-and-export-wizard"></a>Étapes de l’Assistant Importation et Exportation SQL Server
Cette rubrique décrit la série d’étapes permettant d’importer et d’exporter des données avec l’Assistant Importation et Exportation SQL Server. Elle contient également des liens vers les différentes pages de documentation qui décrivent chaque page ou boîte de dialogue s’affichant dans l’Assistant.

Cette rubrique décrit uniquement les **étapes** de l’Assistant. Si ce n’est pas ce que vous recherchez, consultez [Tâches et contenus connexes](#related).

## <a name="steps-for-importing-and-exporting-data"></a>Étapes d’importation et d’exportation de données  
 Le tableau suivant répertorie les étapes liées à l’importation et à l’exportation des données et les pages correspondantes de l’Assistant. En fonction des options que vous sélectionnez dans l’Assistant, il est normal que les pages ne soient pas toutes visibles.  

Pour un rapide aperçu des différents écrans qui s’affichent dans une session normale, étudiez de bout en bout cet exemple facile tenant sur une seule page : [Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md).

|Étape|Pages de l’assistant|  
|----------|------------------|  
|**Bienvenue**<br />Vous n’avez aucune action à effectuer dans cette page.|[Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/welcome-to-sql-server-import-and-export-wizard.md)|  
|**Choisissez la source** des données.|[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)|  
|**Choisissez la destination** des données.|[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)|  
|**Configurez la destination**. (étapes facultatives)<br /><br /> -   Créez une base de données de destination.<br />-   Si vous copiez des données dans un fichier texte, configurer des paramètres supplémentaires.|[Créer une base de données](../../integration-services/import-export-data/create-database-sql-server-import-and-export-wizard.md)<br /><br />[Configurer la destination du fichier plat](../../integration-services/import-export-data/configure-flat-file-destination-sql-server-import-and-export-wizard.md)|  
|**Spécifiez ce que vous souhaitez copier**.|[Spécifier la copie ou l’interrogation de table](../../integration-services/import-export-data/specify-table-copy-or-query-sql-server-import-and-export-wizard.md)<br /><br />[Sélectionner les tables et les vues sources](../../integration-services/import-export-data/select-source-tables-and-views-sql-server-import-and-export-wizard.md)<br /><br />[Fournir une requête source](../../integration-services/import-export-data/provide-a-source-query-sql-server-import-and-export-wizard.md)|  
|**Configurez l’opération de copie**. (étapes facultatives)<br /><br /> -   Créer une table de destination.<br />-   Déterminer la marche à suivre si l’Assistant ne sait pas comment mapper des types de données entre la source et la destination sélectionnées.<br />-   Passer en revue les mappages de colonnes entre la source et de destination.<br />-   Gérer les problèmes de conversion de types de données entre la source et la destination.<br />-   Afficher un aperçu des données à copier.|[Instruction SQL Create Table](../../integration-services/import-export-data/create-table-sql-statement-sql-server-import-and-export-wizard.md)<br /><br />[Convertir les types sans vérifier la conversion](../../integration-services/import-export-data/convert-types-without-conversion-checking-sql-server-import-and-export-wizard.md)<br /><br />[Mappage de colonnes](../../integration-services/import-export-data/column-mappings-sql-server-import-and-export-wizard.md)<br /><br />[Vérifier le mappage de type de données](../../integration-services/import-export-data/review-data-type-mapping-sql-server-import-and-export-wizard.md)<br /><br />[Boîte de dialogue Détails de la conversion de colonne](../../integration-services/import-export-data/column-conversion-details-dialog-box-sql-server-import-and-export-wizard.md)<br /><br />[Boîte de dialogue Aperçu des données](../../integration-services/import-export-data/preview-data-dialog-box-sql-server-import-and-export-wizard.md)|  
|**Copiez les données**.<br /><br /> Si vous le souhaitez, enregistrez vos paramètres comme package SQL Server Integration Services (SSIS).|[Enregistrer et exécuter le package](../../integration-services/import-export-data/save-and-run-package-sql-server-import-and-export-wizard.md)<br /><br />[Enregistrer le package SSIS](../../integration-services/import-export-data/save-ssis-package-sql-server-import-and-export-wizard.md)<br /><br />[Terminer l’Assistant](../../integration-services/import-export-data/complete-the-wizard-sql-server-import-and-export-wizard.md)<br /><br />[Exécution de l’opération](../../integration-services/import-export-data/performing-operation-sql-server-import-and-export-wizard.md)|  

> [!TIP]
> Pour afficher la documentation sur une page ou une boîte de dialogue déterminée de l’Assistant, appuyez sur la touche F1 à partir de cette page.

## <a name="related"></a> Tâches et contenus connexes  
Voici d’autres tâches élémentaires.
-   **Consulter un exemple rapide sur le fonctionnement de l’Assistant.**

    -   **Si vous préférez afficher des captures d’écran.** Jetez un œil à cet exemple simple et complet qui tient sur une seule page : [Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md).

    -   **Si vous préférez regarder une vidéo.** Regardez cette vidéo de quatre minutes sur YouTube qui décrit l’Assistant et explique à l’aide d’instructions claires et simples comment exporter des données vers Excel : [Using the SQL Server Import and Export Wizard to Export to Excel](https://go.microsoft.com/fwlink/?linkid=829049).

-   **Approfondir ses connaissances sur le fonctionnement de l’Assistant.**

    -   **Découvrez-en plus sur l’Assistant.** Si vous recherchez une vue d’ensemble de l’Assistant, consultez [Importer et exporter des données avec l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

    -   **Découvrez comment se connecter à des sources et des destinations de données.** Si vous avez besoin d’informations sur la façon de vous connecter à vos données, sélectionnez la page qui vous intéresse à partir de la liste ici : [Se connecter à des sources de données avec l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/connect-to-data-sources-with-the-sql-server-import-and-export-wizard.md). Il existe une page de documentation propre à chacune des différentes sources de données couramment utilisées. 

-   **Démarrer l’Assistant.** Si vous êtes prêt à exécuter l’Assistant et souhaitez simplement savoir comment le démarrer, consultez [Démarrer l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md).

-     **Procurez-vous l’Assistant.** Si vous souhaitez exécuter l’Assistant, mais [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas installé sur votre ordinateur, vous pouvez installer l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en installant SSDT (SQL Server Data Tools). Pour plus d’informations, consultez [Télécharger SSDT (SQL Server Data Tools)](https://msdn.microsoft.com/library/mt204009.aspx).


