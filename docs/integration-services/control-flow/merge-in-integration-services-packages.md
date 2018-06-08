---
title: MERGE dans les packages Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- MERGE statement [SQL Server]
ms.assetid: 7e44a5c2-e6d6-4fe2-a079-4f95ccdb147b
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c7d3c911b6e8518abe50ce896fe41fbc3eac3286
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="merge-in-integration-services-packages"></a>MERGE in Integration Services Packages
  Dans la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], l’instruction SQL d’une tâche d’exécution SQL peut contenir une instruction MERGE. Cette instruction MERGE vous permet d'accomplir plusieurs opérations INSERT, UPDATE et DELETE dans une même instruction.  
  
 Pour utiliser l'instruction MERGE dans un package, procédez comme suit :  
  
-   Créez une tâche de flux de données qui charge, transforme et enregistre les données sources dans une table temporaire ou intermédiaire.  
  
-   Créez une tâche d'exécution SQL contenant l'instruction MERGE.  
  
-   Connectez la tâche de flux de données à la tâche d'exécution SQL et utilisez les données de la table intermédiaire comme entrée pour l'instruction MERGE.  
  
    > [!NOTE]  
    >  Bien qu'une instruction MERGE requière en général une table intermédiaire dans ce scénario, les performances de l'instruction MERGE dépassent habituellement celles de la recherche ligne par ligne effectuée par la transformation de recherche. L'instruction MERGE est également utile lorsque la grande taille d'une table de recherche pourrait tester la mémoire mise à la disposition de la transformation de recherche pour mettre en cache sa table de référence.  
  
 Pour obtenir un exemple de composant de destination prenant en charge l'utilisation de l'instruction MERGE, consultez l'exemple de la communauté CodePlex, [Destination de l'instruction MERGE](http://go.microsoft.com/fwlink/?LinkId=141215).  
  
## <a name="using-merge"></a>Utilisation de MERGE  
 En général, vous utilisez l'instruction MERGE lorsque vous souhaitez appliquer des modifications qui incluent des insertions, des mises à jour et des suppressions d'une table à une autre table. Avant [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], ce processus requérait à la fois une transformation de recherche et plusieurs transformations de commande OLE DB. La transformation de recherche effectuait une recherche ligne par ligne pour déterminer si chaque ligne était nouvelle ou modifiée. Les transformations de commande OLE DB effectuaient alors les opérations INSERT, UPDATE et DELETE nécessaires. À compter de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], une instruction MERGE unique peut remplacer la transformation de recherche et les transformations de commande OLE DB correspondantes réunies.  
  
### <a name="merge-with-incremental-loads"></a>MERGE avec des charges incrémentielles  
 Les fonctionnalités de capture de données modifiées, nouvelles dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , permettent d'effectuer de manière fiable des charges incrémentielles dans un entrepôt de données. Comme alternative à l'utilisation de transformations de commande OLE DB paramétrables pour effectuer les insertions et les mises à jour, vous pouvez utiliser l'instruction MERGE pour associer ces deux types d'opérations.  
  
 Pour plus d’informations, consultez [Appliquer les modifications à la destination](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md).  
  
#### <a name="merge-in-other-scenarios"></a>MERGE dans d'autres scénarios  
 Dans les scénarios suivants, vous pouvez utiliser l'instruction MERGE à l'extérieur ou à l'intérieur d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Toutefois, un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est souvent requis pour charger les données à partir de plusieurs sources hétérogènes, puis pour associer et assainir ces données. Par conséquent, vous pouvez envisager d'utiliser l'instruction MERGE dans un package pour le côté pratique et pour faciliter la maintenance.  
  
##### <a name="track-buying-habits"></a>Assurer le suivi des habitudes d'achat  
 La table FactBuyingHabits incluse dans l'entrepôt de données indique la dernière date à laquelle un client a acheté un produit donné. La table se compose des colonnes ProductID, CustomerID et PurchaseDate. Chaque semaine, la base de données transactionnelle génère une table PurchaseRecords qui inclut les achats effectués pendant la semaine. L'objectif est d'utiliser une instruction MERGE unique pour fusionner les informations de la table PurchaseRecords dans la table FactBuyingHabits. Pour les paires client–produit qui n'existent pas, l'instruction MERGE insère de nouvelles lignes. Pour les paires client–produit qui existent, l'instruction MERGE met à jour la date d'achat la plus récente.  
  
###### <a name="track-price-history"></a>Assurer le suivi de l'historique des prix  
 La table DimBook représente la liste des livres dans l'inventaire d'un libraire et identifie l'historique des prix de chaque ouvrage. Cette table inclut les colonnes : ISBN, ProductID, Price, Shelf et IsCurrent. Cette table possède également une ligne pour chaque prix que le livre a eu. L'une de ces lignes contient le prix actuel. Pour indiquer la ligne qui contient le prix actuel, la valeur de la colonne IsCurrent est égale à 1 pour cette ligne.  
  
 Chaque semaine, la base de données génère une table WeeklyChanges qui contient les modifications des prix pour la semaine et les nouveaux livres qui ont été ajoutés pendant la semaine. En utilisant une instruction MERGE unique, vous pouvez appliquer les modifications présentes dans la table WeeklyChanges à la table DimBook. L'instruction MERGE insère de nouvelles lignes pour les nouveaux livres ajoutés et met à jour la colonne IsCurrent en spécifiant 0 pour les lignes des livres existants dont le prix a changé. L'instruction MERGE insère également de nouvelles lignes pour les livres dont le prix a changé et, pour ces nouvelles lignes, elle affecte la valeur 1 à la colonne IsCurrent.  
  
### <a name="merge-a-table-with-new-data-against-the-old-table"></a>Fusionner une table avec des données nouvelles par rapport à l'ancienne table  
 La base de données modélise les propriétés d'un objet en utilisant un « schéma ouvert », ce qui signifie qu'une table contient des paires nom–valeur pour chaque propriété. La table Properties a trois colonnes : EntityID, PropertyID et Value. Une table NewProperties qui est une version plus récente de la table doit être synchronisée avec la table Properties. Pour synchroniser ces deux tables, vous pouvez utiliser une instruction MERGE unique pour effectuer les opérations suivantes :  
  
-   supprimer des propriétés de la table Properties si elles ne figurent pas dans la table NewProperties ;  
  
-   mettre à jour les valeurs des propriétés dans la table Properties avec les nouvelles valeurs présentes dans la table NewProperties ;  
  
-   insérer de nouvelles propriétés pour les propriétés présentes dans la table NewProperties mais qui ne figurent pas dans la table Properties.  
  
 Cette approche est utile dans les scénarios qui ressemblent à des scénarios de réplication, où l'objectif est de maintenir la synchronisation entre les données de deux tables sur deux serveurs.  
  
### <a name="track-inventory"></a>Assurer le suivi du stock  
 La base de données Inventory possède une table ProductsInventory qui inclut les colonnes ProductID et StockOnHand. Une table Shipments dotée des colonnes ProductID, CustomerID et Quantity établit le suivi des livraisons de produits aux clients. La table ProductInventory doit être mise à jour quotidiennement en fonction des informations de la table Shipments. Une instruction MERGE unique peut réduire l'inventaire dans la table ProductInventory en fonction des livraisons effectuées. Si l'inventaire d'un produit a été réduit à 0, cette instruction MERGE peut également supprimer cette ligne de produit de la table ProductInventory.  
  
  
