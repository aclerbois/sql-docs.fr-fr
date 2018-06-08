---
title: 'Procédure pas à pas : ajouter et modifier un schéma de base de données | Microsoft Docs'
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
helpviewer_keywords:
- database diagrams [SQL Server], about database diagrams
- database diagrams [SQL Server], designing
- database diagrams [SQL Server], creating
ms.assetid: 228caa0d-8f24-46ab-86d1-b6d8631322bc
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: a81fc1d6a60bc4bbb1d9ef2f67c3f5c32f60dc7c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="walkthrough-adding-and-changing-a-database-diagram"></a>Procédure pas à pas : ajouter et modifier un schéma de base de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Cette procédure pas à pas montre comment créer et modifier un schéma de base de données et apporter des modifications à la base de données par l'intermédiaire du composant des schémas de base de données. Vous apprendrez à ajouter des tables aux schémas, à créer des relations entre les tables, créer des contraintes et des index sur des colonnes et modifier le niveau des informations qui s'affichent pour chaque table.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
Pour réaliser cette procédure pas à pas, vous aurez besoin des éléments suivants :  
  
-   Accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] avec l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject_md.md)] .  
  
-   Un compte avec des privilèges **dbo** de propriétaire de base de données  
  
> [!NOTE]  
> Si vous tentez d'apporter des changements lorsque vous utilisez un compte sans privilèges suffisants pour modifier les tables, un message d'erreur s'affiche.  
  
## <a name="creating-a-diagram"></a>Création d'un schéma  
  
#### <a name="to-create-a-new-database-diagram"></a>Pour créer un nouveau schéma de base de données  
  
1.  Dans le menu **Affichage** , cliquez sur **Explorateur d’objets**.  
  
2.  Ouvrez le nœud Bases de données puis le nœud [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject_md.md)] .  
  
3.  Cliquez avec le bouton droit sur le nœud Schémas de base de données et sélectionnez **Nouveau schéma de base de données**.  
  
    Si la base de données n'a pas les objets nécessaires pour créer des schémas, le message suivant s'affiche : **Cette base de données ne dispose pas d'au moins un des objets de prise en charge nécessaires pour la fonctionnalité de schémas de base de données. Voulez-vous les créer ?** Cliquez sur **Oui**.  
  
    La boîte de dialogue **Ajouter une table** s’affiche.  
  
4.  Sélectionnez **Type d’adresse (Personne)** et **Adresse (Personne)** et cliquez sur **Ajouter**.  
  
    Deux tables sont ajoutées au schéma.  
  
5.  Fermez la boîte de dialogue **Ajouter une table** .  
  
#### <a name="to-view-different-column-data"></a>Pour afficher différentes données de colonne  
  
1.  Cliquez avec le bouton droit sur la table `Address` . Dans le menu contextuel, pointez sur **Vue Table**, puis cliquez sur **Standard**.  
  
    La grille de table affiche trois colonnes : **Nom de la colonne**, **Type de données**et **Autoriser les valeurs NULL**.  
  
2.  Cliquez avec le bouton droit sur la table `Address` , cliquez sur **Vue Table** et sélectionnez **Clés**.  
  
    La grille de table affiche une colonne et les noms de colonne-table. Seules les colonnes actives dans les index s'affichent.  
  
## <a name="creating-new-tables"></a>Création de nouvelles tables  
  
#### <a name="to-create-tables-within-diagram-designer"></a>Pour créer des tables dans le Concepteur de schémas  
  
1.  Cliquez avec le bouton droit sur le Concepteur de schémas en dehors des tables existantes et choisissez **Nouvelle table**.  
  
2.  Dans la boîte de dialogue **Choisir un nom** , cliquez sur **OK** pour accepter le nom par défaut **Table1**.  
  
    Une nouvelle grille de table s’affiche avec trois colonnes : **Nom de la colonne**, **Type de données**et **Autoriser les valeurs NULL**.  
  
3.  Ajoutez les informations suivantes à la **Table1**:  
  
    |**Nom de la colonne**|**Type de données**|**Null autorisé**|  
    |-------------------|-----------------|-------------------|  
    |**T1col1**|**Int**|checked|  
    |**T1col2**|**varchar(50)**|checked|  
    |**T1col3**|**float**|checked|  
  
4.  Cliquez avec le bouton droit sur `T1col1` , puis sélectionnez **Définir la clé primaire**.  
  
    L'icône d'une clé s'affiche en regard du nom de la colonne.  
  
5.  Dans le menu **Fichier** , cliquez sur **Enregistrer Diagram1**.  
  
6.  Dans la boîte de dialogue **Choisir un nom** , cliquez sur **OK** pour accepter le nom par défaut **Diagram1**.  
  
7.  La boîte de dialogue **Enregistrer** s’affiche avec un message indiquant que `Table1` sera enregistré dans la base de données. Cliquez sur **Oui**.  
  
## <a name="modifying-table-structure"></a>Modification de la structure de la table  
Vous pouvez ajouter des contraintes de validation et établir des relations entre les tables dans le Concepteur de schémas.  
  
#### <a name="to-create-check-constraints"></a>Pour créer des contraintes de validation  
  
1.  Dans `Table1`, cliquez avec le bouton droit sur la ligne `T1col3` et cliquez sur **Contraintes de validation**.  
  
    La boîte de dialogue **Contraintes de validation** apparaît.  
  
2.  Cliquez sur **Ajouter**.  
  
    Une nouvelle contrainte s’affiche dans la liste **Contrainte de validation sélectionnée** avec le nom par défaut `CK_Table1`.  
  
3.  Sélectionnez la ligne **Expression** dans la grille et cliquez sur le bouton de sélection.  
  
    La boîte de dialogue **Expression de contrainte de validation** apparaît.  
  
4.  Tapez **T1col3 > 5**, puis cliquez sur **OK**.  
  
    `Table1` contient à présent une contrainte selon laquelle toutes les valeurs entrées dans `T1col3` doivent être supérieures à 5.  
  
5.  Cliquez sur **Fermer**.  
  
#### <a name="to-create-relationships-between-tables"></a>Pour créer des relations entres des tables  
  
1.  Créez une nouvelle table dans le Concepteur de schémas nommée `Table2` et contenant les colonnes suivantes :  
  
    |**Nom de la colonne**|**Type de données**|**Null autorisé**|  
    |-------------------|-----------------|-------------------|  
    |**T2col1**|**Int**|non validé|  
    |**T2col2**|**varchar(50)**|checked|  
    |**T2col3**|**xml**|checked|  
  
    > [!NOTE]  
    > Les colonnes situées du côté clé primaire d'une relation de clé étrangère doivent faire partie d'une clé primaire ou d'une contrainte unique.  
  
2.  Faites glisser `T2col1` vers `T1col1`.  
  
    Deux boîtes de dialogue apparaissent : **Relation de clé étrangère** en arrière-plan et **Tables et colonnes** au premier plan.  
  
3.  Cliquez sur **OK** pour enregistrer la nouvelle relation.  
  
4.  Cliquez de nouveau sur **OK** .  
  
## <a name="creating-indexes"></a>Création des index  
Vous pouvez créer des index sur la plupart des types de données, y compris les données XML.  
  
#### <a name="to-create-a-standard-index"></a>Pour créer un index standard  
  
1.  Cliquez avec le bouton droit sur `Table1` et choisissez **Index/clés**.  
  
    La boîte de dialogue **Index/Clés** s’ouvre.  
  
2.  Cliquez sur **Ajouter**.  
  
    Un nouvel index s’affiche dans la liste **Selected Primary/Unique Key or Index** (Clé ou index primaire/unique sélectionné) avec un nom par défaut similaire à `IX_Table1`.  
  
3.  Sélectionnez la ligne **Colonnes** et cliquez sur le bouton de sélection.  
  
    La boîte de dialogue **Colonnes d’index** s’affiche.  
  
4.  Cliquez sur la flèche de déroulement sous **Nom de la colonne** et sélectionnez `T1col2`.  
  
    > [!NOTE]  
    > Vous pouvez ajouter des colonnes supplémentaires à cet index en sélectionnant la cellule sous `T1col2` et en choisissant un autre nom de colonne.  
  
5.  Cliquez sur **OK** pour enregistrer cet index.  
  
6.  Cliquez sur **Fermer** dans la boîte de dialogue **Index/Clés** .  
  
#### <a name="to-create-an-xml-index"></a>Pour créer un index XML  
  
1.  Cliquez avec le bouton droit sur `T2col1` et sélectionnez **Définir la clé primaire**.  
  
    > [!NOTE]  
    > L'ajout d'un index XML nécessite qu'une autre colonne de la table soit définie comme clé cluster primaire.  
  
2.  Cliquez avec le bouton droit sur la ligne `T2col3` dans `Table2` , et cliquez sur **Index XML**.  
  
    La boîte de dialogue **Index XML** s’ouvre.  
  
3.  Cliquez sur **Ajouter**.  
  
    Un index XML contenant des valeurs par défaut sera ajouté à la liste **Index XML sélectionné** .  
  
4.  Cliquez sur **Fermer**.  
  
    > [!NOTE]  
    > Les index XML sont créés par colonne. Le premier index XML est primaire et les index supplémentaires sont secondaires.  
  
## <a name="saving-the-diagram"></a>Enregistrement du schéma  
Toutes les modifications apportées au schéma ne sont publiées dans la base de données qu'une fois celle-ci enregistrée. En cas de problèmes ou de conflits, une boîte de dialogue s'affiche avec plus d'informations.  
  
#### <a name="to-save-a-database-diagram"></a>Pour enregistrer un schéma de base de données  
  
1.  Dans le menu **Fichier** , sélectionnez **Enregistrer Diagram1**.  
  
    La boîte de dialogue **Enregistrer** s’affiche. Si **Signaler les tables affectées** est activé, les informations sur les tables nouvelles ou modifiées s’affichent.  
  
2.  Cliquez sur **OK**.  
  
3.  En cas d’erreurs, la boîte de dialogue **Notifications post-enregistrement** affiche les erreurs et leurs origines. Corrigez les erreurs et réenregistrez le schéma.  
  
## <a name="next-steps"></a>Next Steps  
Il s'agit d'un schéma de base contenant deux nouvelles tables et deux tables existantes, mais il illustre la possibilité de créer un schéma d'une base de données existante ou de créer un nouveau schéma visuel. Suggestions pour des recherches approfondies :  
  
-   Créer des nouveaux schémas contenant des groupes de tables associées  
  
-   Personnaliser la quantité d'informations affichées dans chaque table  
  
-   Modifier la présentation et ajouter des annotations  
  
-   Copier le schéma dans un bitmap  
  
## <a name="see-also"></a> Voir aussi  
[Personnaliser la quantité d’informations affichées dans les schémas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/customize-the-amount-of-information-displayed-in-diagrams-visual-database-tools.md)  
[Configurer le Concepteur de schémas de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/set-up-database-diagram-designer-visual-database-tools.md)  
[Ajouter des tables à des schémas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-diagrams-visual-database-tools.md)  
[Créer des relations entre des tables sur un diagramme &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-relationships-between-tables-on-a-diagram-visual-database-tools.md)  
[Créer des index XML](http://msdn.microsoft.com/en-us/6ecac598-355d-4408-baf7-1b2e8d4cf7c1)  
[Copier une image d’un schéma de base de données dans le Presse-papiers &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/copy-an-image-of-a-database-diagram-to-the-clipboard-visual-database-tools.md)  
[Utiliser une disposition de schémas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-diagram-layout-visual-database-tools.md)  
  
