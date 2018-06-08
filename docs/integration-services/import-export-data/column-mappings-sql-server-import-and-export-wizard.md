---
title: Mappage de colonnes (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
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
- sql13.dts.impexpwizard.columnmapandtransform.f1
ms.assetid: eadc54a6-f936-4ffc-91d7-fbfd2bdcab93
caps.latest.revision: 69
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7d61bad35eaf48be5567bdb258e819c477390ada
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-mappings-sql-server-import-and-export-wizard"></a>Mappage de colonnes (Assistant Importation et exportation SQL Server)
  Une fois que vous avez sélectionné les tables et vues existantes pour copier ou vérifier la requête que vous avez fournie, l’Assistant Importation et Exportation **affiche la boîte de dialogue**Mappage de colonnes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si vous cliquez sur **Modifier les mappages** . Dans cette page, vous spécifiez et configurez des colonnes de destination pour recevoir les données copiées à partir des colonnes sources. Souvent, il n’y a pas de modification à apporter à cette page.
  
Si vous ne voulez pas copier toutes les colonnes dans la table sélectionnée, faites la chose suivante : excluez les colonnes que vous ne souhaitez pas dans cette page. Sélectionnez **ignorer** dans la colonne **Destination** de la liste **Mappages** pour les colonnes que vous voulez proscrire.
 
## <a name="screen-shot-of-the-column-mappings-page"></a>Capture d’écran de la page Mappages de colonnes 
 La capture d’écran suivante montre un exemple de la boîte de dialogue **Mappage de colonnes** de l’Assistant. 
 
 Dans cet exemple, vous voyez que l’Assistant va créer une table de destination, car **Créer la table de destination** est sélectionnée. Par défaut, l’Assistant donne à chaque colonne de la nouvelle table de destination le même nom, le même type de données et les mêmes propriétés que la colonne source correspondante. 
  
 ![Page Mappage de colonnes de l’Assistant Importation et Exportation](../../integration-services/import-export-data/media/column-mappings.png "Page Mappage de colonnes de l’Assistant Importation et Exportation")  
  
## <a name="review-the-source-and-destination"></a>Vérifier la source et la destination 
![Page mappage de colonnes, section source et destination.](../../integration-services/import-export-data/media/column-mappings-page-source-and-destination-section.png)

 **Source**  
 La table, vue ou requête sélectionnée.  
  
 **Destination**  
 La table ou vue de destination sélectionnée.  

## <a name="optionally-create-a-new-destination-table"></a>Le cas échéant, créer une table de destination
![Page mappage de colonnes, section nouvelle table](../../integration-services/import-export-data/media/column-mappings-page-new-table-section.png)

 **Créer la table/le fichier de destination**  
 Créez la table de destination si elle n’existe pas encore.    
  
 **Modifier SQL**  
Cliquez sur **Modifier SQL** pour ouvrir la boîte de dialogue **Instruction SQL de création de table** . Utilisez l’instruction CREATE TABLE générée automatiquement ou modifiez-la en fonction de vos besoins. Si vous modifiez manuellement cette instruction, vous devez vérifier que la liste des mappages de colonnes reconnaît vos modifications. Pour plus d’informations, consultez [Instruction SQL Create Table](../../integration-services/import-export-data/create-table-sql-statement-sql-server-import-and-export-wizard.md).  

### <a name="sometimes-these-options-are-disabled"></a>Parfois, ces options sont désactivées
L’option **Créer la table/le fichier de destination** et le bouton **Modifier SQL** sont automatiquement activés ou désactivés.

-   **Activé.** Si vous avez spécifié une **nouvelle** table de destination dans la page **Sélectionner les tables et les vues sources**, l’option **Créer la table de destination** est automatiquement sélectionnée et le bouton **Modifier SQL** est activé.

-   **Désactivé.** Si vous avez spécifié une table de destination **existante** dans la page **Sélectionner les tables et les vues sources**, l’option **Créer la table de destination** et le bouton **Modifier SQL** sont désactivés. Si vous voulez créer une table de destination, revenez à la page **Sélectionner les tables et les vues sources** et entrez le nom d’une **nouvelle** table dans la colonne **Destination**.  

## <a name="what-about-existing-data-in-the-destination"></a>Qu’en est-il des données existantes dans la destination ?
![Page mappage de colonnes, section options](../../integration-services/import-export-data/media/column-mappings-page-options-section.png)

 **Supprimer des lignes dans la table/le fichier de destination**  
 Indiquez si les données d’une table existante doivent être effacées avant d’en charger de nouvelles.  
  
 **Ajouter des lignes à la table/au fichier de destination**  
 Indiquez si les nouvelles données doivent être ajoutées à la suite des données déjà présentes dans une table existante.  
  
 **Supprimer et recréer la table de destination**  
 Choisissez cette option pour remplacer la table de destination. Cette option est disponible uniquement lorsque vous faites appel à l’Assistant pour créer la table de destination. La table de destination est uniquement supprimée et recréée si vous enregistrez le package créé par l’Assistant, puis que vous réexécutez le package. Il s’agit d’une option pratique si vous souhaitez tester vos paramètres plusieurs fois.
  
 **Activer l’insertion d’identité**  
 Permet d'insérer des valeurs d'identité des données source dans une colonne d'identité de la table de destination. Par défaut, la colonne d’identité de destination ne permet généralement pas cela.  
  
> [!TIP]
> En général, si vos clés primaires existantes sont dans une colonne d’identité, une colonne NuméroAuto ou son équivalent, vous devez sélectionner cette option pour conserver les valeurs de vos clés primaires existantes.  Sinon, la colonne d’identité de destination assigne de nouvelles valeurs.  

## <a name="keep-your-autonumber-or-identity-values"></a>Conserver les valeurs de numérotation automatique ou d’identité
Si vous exportez des données qui ont une colonne de numérotation automatique ou une colonne d’identité, par exemple si vous exportez à partir de Microsoft Access, veillez à sélectionner **Activer l’insertion d’identité** comme décrit un peu plus haut.

## <a name="review-column-mappings"></a>Vérifier le mappage de colonnes
![Page mappage de colonnes, section mappages](../../integration-services/import-export-data/media/column-mappings-page-mappings-section.png)

 **Mappages**  
 Affiche la manière dont chaque colonne dans la source de données est mappée à une colonne dans la destination.
 
La liste **Mappages** comprend les colonnes suivantes.  
  
-    **Source**  
     Permet d’afficher chaque colonne source.  
  
-   **Destination**  
    Affichez la colonne de destination mappée ou sélectionnez une autre colonne.
    
    Vous n’êtes pas obligé de copier toutes les colonnes à partir de la table source. Sélectionnez **ignorer** dans cette colonne pour les colonnes que vous souhaitez ignorer. Avant de mapper les colonnes, vous devez ignorer toutes les colonnes qui ne seront pas mappées.  
  
-   **Type**  
    Affichez le type de données de la colonne de destination ou sélectionnez un autre type de données.
  
-   **Nullable**  
    Spécifiez si la colonne de destination autorise une valeur null.  
  
-   **Taille**  
    Spécifiez le nombre de caractères dans la colonne de destination, si applicable.  
  
-    **Précision**  
    Permet de spécifier la précision des données numériques dans la colonne de destination, autrement dit le nombre de chiffres, si applicable.  
  
 -   **Échelle**  
    Spécifiez l’échelle des données numériques dans la colonne de destination, autrement dit le nombre de décimales, si applicable.  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Après avoir vérifié et configuré les colonnes de destination pour qu’elles reçoivent les données copiées depuis les colonnes sources, cliquez ensuite sur **OK** pour quitter la boîte de dialogue **Mappage de colonnes** et revenir à la page **Sélectionner les tables et les vues sources** ou **Configurer la destination du fichier plat**. Pour plus d’informations, consultez [Sélectionner les tables et les vues sources](../../integration-services/import-export-data/select-source-tables-and-views-sql-server-import-and-export-wizard.md) ou [Configurer la destination du fichier plat](../../integration-services/import-export-data/configure-flat-file-destination-sql-server-import-and-export-wizard.md).  
  
 Si vous avez spécifié un mappage qui risque d’échouer dans la liste **Mappages** , la boîte de dialogue **Mappage de colonnes** affiche la page **Vérifier le mappage de type de données** . Dans cette page, vous passez en revue les avertissements, vous spécifiez les options de conversion et vous indiquez aussi comment gérer les erreurs. Pour plus d’informations, consultez [Vérifier le mappage de type de données](../../integration-services/import-export-data/review-data-type-mapping-sql-server-import-and-export-wizard.md).  
 
 ## <a name="see-also"></a>Voir aussi
[Mappage de type de données dans l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/data-type-mapping-in-the-sql-server-import-and-export-wizard.md)  
[Bien démarrer avec cet exemple simple de l’Assistant Importation et Exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)

