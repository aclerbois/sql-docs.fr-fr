---
title: Sélectionner les tables et les vues sources (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.selectsourcetablesandviews.f1
ms.assetid: f60e1a19-2ea6-403c-89ab-3e60ac533ea0
caps.latest.revision: 96
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8a5ebd644020b4c2bb6e07aff74e11e4e2474338
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="select-source-tables-and-views-sql-server-import-and-export-wizard"></a>Sélectionner les tables et les vues sources (Assistant Importation et Exportation SQL Server)
  Une fois que vous avez indiqué que vous voulez copier la totalité d’une table ou après avoir fourni une requête, l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche la page **Sélectionner les tables et les vues sources**. Dans cette page, vous sélectionnez les tables et les vues existantes à copier. Vous mappez ensuite les tables sources aux tables de destination nouvelles ou existantes. Si vous le souhaitez, vous pouvez aussi passer en revue le mappage des colonnes et afficher un aperçu des exemples de données.

> [!TIP]
> Si vous devez copier plusieurs bases de données SQL Server ou des objets de base de données SQL Server autres que des tables et des vues, utilisez l’Assistant Copie de base de données au lieu de l’Assistant Importation et exportation. Pour plus d’informations, consultez [Utiliser l’Assistant Copie de base de données](../../relational-databases/databases/use-the-copy-database-wizard.md).  
  
## <a name="screen-shot---if-youre-going-to-copy-tables"></a>Capture d’écran - Si vous allez copier des tables  
 La capture d’écran suivante montre un exemple de la page **Sélectionner les tables et les vues sources** de l’Assistant quand vous avez sélectionné auparavant l’option **Copier les données à partir d’une ou plusieurs tables ou vues** dans la page **Spécifier la copie ou l’interrogation de table**. La liste indique toutes les tables et vues disponibles à partir de la source de données.
 
Dans cet exemple, la liste **Source** contient toutes les tables dans l’exemple de base de données AdventureWorks. La ligne sélectionnée montre que l’utilisateur souhaite copier la table **Sales.Customer** à partir de la source vers la nouvelle table **Sales.CustomerNew** dans la destination. 
   
 ![Page Sélectionner les tables de l’Assistant Importation et Exportation](../../integration-services/import-export-data/media/select-tables1.png "Page Sélectionner les tables de l’Assistant Importation et Exportation")
  
## <a name="screen-shot---if-you-provided-a-query"></a>Capture d’écran - Si vous avez fourni une requête  
 La capture d’écran suivante montre un exemple de la page **Sélectionner les tables et les vues sources** de l’Assistant lorsque vous avez sélectionné auparavant l’option **Écrire une requête pour spécifier les données à transférer** dans la page **Spécifier la copie ou l’interrogation de table**. La liste **Source** ne contient qu’une seule ligne, dans laquelle l’élément nommé `[Query]` représente la requête que vous avez indiquée dans la page **Fournir une requête source**.
 
Dans cet exemple, l’utilisateur souhaite copier les résultats de la requête à partir de la source vers la table **Sales.CustomerNew** dans la destination.  
    
 ![Page Sélectionner les tables de l’Assistant Importation et Exportation](../../integration-services/import-export-data/media/select-tables2.png "Page Sélectionner les tables de l’Assistant Importation et Exportation")  

## <a name="select-source-and-destination-tables"></a>Sélectionner des tables sources et de destination 
**Source**  
Activez les cases à cocher pour sélectionner dans la liste les tables et les vues à copier. Par défaut, les données de la source de données sont copiées sans modification. Si vous créez une table de destination, le schéma de la nouvelle table - autrement dit, la liste des colonnes et leurs propriétés - est également copié sans modification de la source de données.

Si vous avez fourni une requête, la liste ne contient qu’un élément portant le nom `[Query]`. 

**Destination**  
 Sélectionnez une table de destination dans la liste pour chaque requête ou table source, ou entrez le nom d’une nouvelle table, que l’Assistant créera pour vous. Si vous sélectionnez une table de destination existante, la table doit avoir des colonnes contenant des types de données compatibles avec les données sources.  

> [!NOTE]
> Si vous faites une pause à ce stade de l’Assistant pour créer une table manuellement dans la base de données de destination à l’aide d’un outil externe (tel que  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]), la nouvelle table n’est pas visible immédiatement dans la liste des tables de destination disponibles. Pour actualiser la liste des tables de destination, revenez à la page **Choisir une destination** , resélectionnez la base de données de destination pour actualiser la liste des tables et vues disponibles, puis avancez de nouveau jusqu’à la page **Sélectionner les tables et les vues sources** .  

## <a name="optionally-review-column-mappings-and-preview-data"></a>Éventuellement, vérifier le mappage de colonnes et afficher un aperçu des données
**Modifier les mappages**   
Cliquez sur **Modifier les mappages** pour afficher la boîte de dialogue **Mappage de colonnes** de la table sélectionnée. Utilisez la boîte de dialogue **Mappage de colonnes** pour effectuer les opérations suivantes.
-   Passer en revue le mappage des colonnes entre la source et la destination.
-   Vous pouvez copier uniquement un sous-ensemble de colonnes en sélectionnant **ignorer** pour les colonnes que vous souhaitez ignorer.

Pour plus d’informations, consultez [Mappages de colonnes](../../integration-services/import-export-data/column-mappings-sql-server-import-and-export-wizard.md).  

**Aperçu**  
Cliquez sur **Aperçu** pour afficher un aperçu de 200 lignes d’exemples de données maximum dans la boîte de dialogue **Aperçu des données**. Cela confirme que l’Assistant va copier les données que vous souhaitez copier. Pour plus d’informations, consultez [Aperçu des données](../../integration-services/import-export-data/preview-data-dialog-box-sql-server-import-and-export-wizard.md).  
  
Après avoir affiché un aperçu des données, vous souhaiterez peut-être modifier les options que vous avez sélectionnées dans les pages précédentes de l’Assistant. Pour effectuer ces modifications, retournez dans la page **Sélectionner les tables et les vues sources** , puis cliquez sur **Précédent** pour retourner aux pages précédentes où vous pouvez modifier vos sélections.  

## <a name="select-source-and-destination-tables-for-excel"></a>Sélectionner des tables sources et de destination pour Excel

> [!IMPORTANT]
> Pour obtenir des informations détaillées sur la connexion à des fichiers Excel, et sur les limitations et les problèmes connus liés au chargement de données depuis ou vers des fichiers Excel, consultez [Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md).

### <a name="excel-source-tables"></a>Tables sources Excel
La liste des tables et des vues sources d’une source de données Excel comprend deux types d’objets Excel.
-   **Feuilles de calcul**. Les noms de feuille de calcul sont suivis du signe dollar ($) : par exemple, **« Sheet1$ »**.
-   **Plages nommées.** Les plages nommées, le cas échéant, sont répertoriées par nom.

Si vous souhaitez charger des données à partir d’une plage de cellules spécifique, sans nom ou vers celle-ci (par exemple, **[Sheet1$A1:B4]**), vous devez écrire une requête. Retournez dans la page **Spécifier la copie ou l’interrogation de table** et sélectionnez **Écrire une requête pour spécifier les données à transférer**.

### <a name="excel-destination-tables"></a>Tables de destination Excel
Si vous exportez des données vers Excel, vous pouvez spécifier la destination par l’une des trois méthodes suivantes.
-   **Feuille de calcul.** Pour spécifier une feuille de calcul, ajouter le caractère $ à la fin du nom de la feuille et ajouter des délimiteurs autour de la chaîne. Par exemple, **[Sheet1$]**.
-   **Plage nommée.** Pour spécifier une plage nommée, utilisez simplement le nom de la plage. Par exemple, **MyDataRange**.
-   **Plage sans nom.** Pour spécifier une plage de cellules que vous n’avez pas nommée, ajoutez le caractère $ à la fin du nom de la feuille, ajoutez la spécification de plage ainsi que des délimiteurs autour de la chaîne. Par exemple, **[Sheet1$A1:B4]**.

> [!TIP]
> Quand vous utilisez Excel comme source ou destination, cliquez sur **Modifier les mappages** et vérifiez les mappages de types de données dans la page **Mappages de colonnes** . 

## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Une fois que vous avez sélectionné les tables et vues existantes à copier et que vous les avez mappées à leurs destinations, la page suivante est **Enregistrer et exécuter le package**. Dans cette page, vous spécifiez si vous souhaitez exécuter l’opération de copie immédiatement. Selon votre configuration, vous pouvez également être en mesure d’enregistrer le package [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] créé par l’Assistant pour le personnaliser et le réutiliser ultérieurement. Pour plus d’informations, consultez [Enregistrer et exécuter le package](../../integration-services/import-export-data/save-and-run-package-sql-server-import-and-export-wizard.md).
 
 ## <a name="see-also"></a>Voir aussi
[Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)  
[Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md)



