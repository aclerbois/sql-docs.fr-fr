---
title: Appliquer les modifications à la destination | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],applying changes
ms.assetid: 338a56db-cb14-4784-a692-468eabd30f41
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9d8ec7ffc3d743a83a8a6a13ff51165cf05a15e4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="apply-the-changes-to-the-destination"></a>Appliquer des modifications à la destination
  Dans le flux d’un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui effectue un chargement incrémentiel des données modifiées, la troisième et dernière tâche consiste à appliquer les modifications à votre destination. Vous aurez besoin de trois composants : un pour appliquer les insertions, un pour appliquer les mises à jour et un pour appliquer les suppressions.  
  
> [!NOTE]  
>  La deuxième tâche pour concevoir le flux de données d'un package qui effectue un chargement incrémentiel des données modifiées consiste à séparer les insertions, les mises à jour et les suppressions. Pour plus d’informations sur ce composant, consultez [Traiter les insertions, les mises à jour et les suppressions](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md). Pour obtenir une description du processus global de création d’un package qui effectue un chargement incrémentiel des données modifiées, consultez [Capture de données modifiées &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="applying-inserts"></a>Application d'insertions  
 Pour appliquer des insertions, vous utilisez une destination OLE DB car les nouvelles lignes ne requièrent pas de traitement spécial.  
  
#### <a name="to-process-inserts-by-using-an-ole-db-destination"></a>Pour traiter des insertions à l'aide d'une destination OLE DB  
  
1.  Sous l’onglet **Flux de données** , ajoutez une destination OLE DB.  
  
2.  Connectez la sortie qui contient les insertions de la transformation de fractionnement conditionnel à la destination OLE DB.  
  
3.  Dans **l’Éditeur de destination OLE DB**, dans la page **Gestionnaire de connexions** , sélectionnez les options suivantes :  
  
    1.  Sélectionnez ou créez un gestionnaire de connexions OLE DB pour la base de données de destination.  
  
    2.  Sélectionnez une option **Mode d’accès aux données** , puis sélectionnez la table de destination ou entrez une instruction SQL qui contient les colonnes de destination.  
  
4.  Dans la page **Mappages** de l’éditeur, mappez les colonnes appropriées des données modifiées à la table de destination.  
  
## <a name="applying-updates"></a>Application de mises à jour  
 Pour appliquer des mises à jour, vous utilisez une transformation de commande OLE DB. Cela s'explique par le fait que vous devez utiliser une instruction UPDATE paramétrable pour mettre à jour une ligne à la fois avec les nouvelles valeurs de colonne.  
  
> [!NOTE]  
>  Vous pouvez aussi utiliser des composants de destination pour appliquer des mises à jour. Lorsque vous utilisez cette approche, vous utilisez les composants de destination pour enregistrer les lignes dans des tables temporaires que vous créez à cet effet. Ensuite, vous utilisez des tâches d'exécution SQL pour effectuer les opérations de mise à jour en bloc et de suppression en bloc sur la destination à partir des tables temporaires.  
  
#### <a name="to-process-updates-by-using-an-ole-db-command-transformation"></a>Pour traiter des mises à jour à l'aide d'une transformation de commande OLE DB  
  
1.  Sous l’onglet **Flux de données** , ajoutez une transformation de commande OLE DB.  
  
2.  Connectez la sortie qui contient les mises à jour de la transformation de fractionnement conditionnel à la transformation de commande OLE DB.  
  
3.  Dans **l’Éditeur avancé pour Commande OLE DB**, sous l’onglet **Gestionnaire de connexions** , sélectionnez ou créez un gestionnaire de connexions OLE DB pour la base de données de destination.  
  
4.  Dans **l’Éditeur avancé pour Commande OLE DB**, sous l’onglet **Propriétés du composant** , pour **SqlCommand**, entrez une instruction UPDATE paramétrable.  
  
     Par exemple, une instruction UPDATE pour une table Customer peut avoir la syntaxe suivante :  
  
    ```sql
    update CDCSample.Customer  
    set TerritoryID  = ?,  
        CustomerType  = ?,  
        rowguid  = ?,  
        ModifiedDate  = ?  
    where CustomerID = ?  
  
    ```  
  
5.  Sous l’onglet **Mappage de colonnes** de l’éditeur, mappez les colonnes appropriées des données modifiées aux paramètres dans l’instruction UPDATE.  
  
## <a name="applying-deletes"></a>Application de suppressions  
 Pour appliquer des suppressions, vous utilisez une transformation de commande OLE DB. Cela s'explique par le fait que vous devez utiliser une instruction DELETE paramétrable qui supprime une ligne à la fois en fonction de la valeur de colonne qui identifie la ligne de façon unique.  
  
> [!NOTE]  
>  Vous pouvez aussi utiliser des composants de destination pour appliquer des suppressions. Lorsque vous utilisez cette approche, vous utilisez les composants de destination pour enregistrer les lignes dans des tables temporaires que vous créez à cet effet. Ensuite, vous utilisez des tâches d'exécution SQL pour effectuer les opérations de mise à jour en bloc et de suppression en bloc sur la destination à partir des tables temporaires.  
  
#### <a name="to-process-deletes-by-using-an-ole-db-command-transformation"></a>Pour traiter des suppressions à l'aide d'une transformation de commande OLE DB  
  
1.  Sous l’onglet **Flux de données** , ajoutez une transformation de commande OLE DB au flux.  
  
2.  Connectez la sortie qui contient les suppressions de la transformation de fractionnement conditionnel à la transformation de commande OLE DB.  
  
3.  Ouvrez l'Éditeur avancé pour configurer la transformation.  
  
4.  Dans **l’Éditeur avancé pour Commande OLE DB**, sous l’onglet **Gestionnaire de connexions** , sélectionnez ou créez un gestionnaire de connexions OLE DB pour la base de données de destination.  
  
5.  Dans **l’Éditeur avancé pour Commande OLE DB**, sous l’onglet **Propriétés du composant** de l’éditeur, pour **SqlCommand**, entrez une instruction DELETE paramétrable.  
  
     Par exemple, une instruction DELETE pour une table Customer peut avoir la syntaxe suivante :  
  
    ```sql
    delete from Customer where CustomerID = ?  
  
    ```  
  
6.  Sous l’onglet **Mappage de colonnes** de l’éditeur, mappez la colonne appropriée des données modifiées au paramètre dans l’instruction DELETE.  
  
## <a name="optimizing-inserts-and-updates-by-using-merge-functionality"></a>Optimisation des insertions et des mises à jour à l'aide de la fonctionnalité MERGE  
 Vous pouvez optimiser le traitement des insertions et des mises à jour en combinant certaines options de capture de données modifiées avec l'utilisation du mot clé MERGE Transact-SQL. Pour plus d’informations sur le mot clé MERGE, consultez [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md).  
  
 Dans l’instruction Transact-SQL qui récupère les données modifiées, vous pouvez spécifier *all with merge* comme valeur du paramètre *row_filter_option* quand vous appelez la fonction **cdc.fn_cdc_get_net_changes_<instance_capture>**. Cette fonction de capture de données modifiées fonctionne plus efficacement lorsqu'elle ne doit pas effectuer le traitement supplémentaire requis pour différencier les insertions des mises à jour. Quand vous spécifiez la valeur de paramètre *all with merge* , la valeur **__$operation** des données modifiées est 1 pour les suppressions ou 5 pour les modifications résultant d’insertions ou de mises à jour. Pour plus d’informations sur la fonction Transact-SQL utilisée pour récupérer les données modifiées, consultez [Récupérer et comprendre les données modifiées](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md). Après avoir extrait les modifications avec la valeur de paramètre *all with merge* , vous pouvez appliquer des suppressions et générer en sortie les lignes restantes dans une table temporaire ou une table de transit. Ensuite, dans une tâche d'exécution SQL en aval, vous pouvez utiliser une instruction MERGE unique pour appliquer toutes les insertions ou mises à jour de la table intermédiaire à la destination.  
  
  
