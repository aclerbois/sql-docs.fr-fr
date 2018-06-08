---
title: Extraire des données modifiées à l’aide de la source de capture de données modifiées | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 604fbafb-15fa-4d11-8487-77d7b626eed8
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 23bb38ca1c57b230ac7f0e86d2216d6b31d487fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="extract-change-data-using-the-cdc-source"></a>Extraire des données modifiées à l'aide de la source de capture de données modifiées
  Pour pouvoir ajouter et configurer une source CDC, le package doit inclure au moins une tâche de flux de données et une tache de contrôle de capture de données modifiées.  
  
 Pour plus d’informations sur la tâche de contrôle de capture de données modifiées, consultez [Tâche de contrôle de capture de données modifiées](../../integration-services/control-flow/cdc-control-task.md).  
  
 Pour plus d'informations sur la source CDC, consultez [CDC Source](../../integration-services/data-flow/cdc-source.md).  
  
### <a name="to-extract-change-data-using-a-cdc-source"></a>Pour extraire des données modifiées à l'aide d'une source CDC  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le projet [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] contenant le package souhaité.  
  
2.  Dans l’Explorateur de solutions, double-cliquez sur le package pour l’ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de données** puis, dans la **Boîte à outils**, faites glisser la source CDC vers l’aire de conception.  
  
4.  Double-cliquez sur la source CDC.  
  
5.  Dans la boîte de dialogue **Éditeur de source CDC** , dans la page **Gestionnaire de connexions** , sélectionnez un gestionnaire de connexions ADO.NET existant dans la liste ou cliquez sur **Nouveau** pour créer un gestionnaire de connexions. La connexion doit être établie avec une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient les tables de modifications à lire.  
  
6.  Sélectionnez la **table CDC** dans laquelle vous souhaitez traiter les modifications.  
  
7.  Sélectionnez ou tapez le nom de **l’instance de capture de données modifiées** contenant la table CDC à lire.  
  
     Une table source capturée peut contenir une ou deux instances capturées pour gérer la transition transparente de la définition de table lors des modifications de schéma. Si plusieurs instances de capture sont définies pour la table source qui est capturée, sélectionnez l'instance de capture à utiliser ici. Le nom par défaut de l’instance de capture pour une table [schema].[table] est \<schéma>_\<table>, mais le nom réel utilisé pour cette instance de capture peut être différent. La table réelle dans laquelle les données sont lues est la table CDC **cdc .\<instance-capture>_CT**.  
  
8.  Sélectionnez le mode de traitement le plus adapté pour la gestion de vos besoins de traitement. Les options possibles sont les suivantes :  
  
    -   **Tout**: retourne les modifications apportées à la plage de capture de données modifiées actuelle sans les valeurs **Avant la mise à jour** .  
  
    -   **Tout avec les anciennes valeurs**: retourne les modifications apportées à la plage de traitement de capture de données modifiées actuelle, dont les anciennes valeurs (**Avant la mise à jour**). Chaque opération de mise à jour utilise deux lignes, une avec les valeurs avant la mise à jour et une avec la valeur après la mise à jour.  
  
    -   **Net**: retourne une seule ligne de modification par ligne source modifiée dans la plage de capture de données modifiées actuelle. Si une ligne source a été mise à jour plusieurs fois, la modification associée est appliquée (par exemple, l'insertion et la mise à jour sont considérées comme une mise à jour unique, et la mise à jour et la suppression sont considérées comme une suppression unique). Lorsque vous travaillez dans le mode de traitement de modifications Net, il est possible de fractionner les modifications apportées aux sorties de suppression, d'insertion et de mise à jour et de les traiter en parallèle car la ligne source apparaît dans plusieurs sorties.  
  
    -   **Net avec masque de mise à jour** : ce mode est semblable au mode Net standard, à ceci près qu’il ajoute des colonnes booléennes au modèle de nom **__$\<nom-colonne>\__Modifié** qui indique les colonnes modifiées dans la ligne de modification active.  
  
    -   **Net avec fusion**: ce mode est semblable au mode Net standard, à ceci près que les opérations d’insertion et de mise à jour sont fusionnées en une seule opération de fusion (UPSERT).  
  
9. Sélectionnez la variable de package de chaîne SSIS qui gère l'état de capture de données modifiées pour le contexte de capture de données modifiées actuel. Pour plus d’informations sur la variable d’état CDC, consultez [Définir une variable d’état](../../integration-services/data-flow/define-a-state-variable.md).  
  
10. Cochez la case **Inclure la colonne de l’indicateur de retraitement** pour créer une colonne de sortie spéciale appelée **__$reprocessing**. Cette colonne a la valeur **true** quand la plage de traitement CDC chevauche la plage de traitement initiale (la plage de NSE correspondant à la période de charge initiale) ou quand une plage de traitement CDC est retraitée suite à une erreur lors d’une exécution précédente. Cette colonne d'indicateur permet au développeur SSIS de gérer les erreurs différemment lors du retraitement des modifications (par exemple, les actions telles que la suppression d'une ligne inexistante et une insertion ayant échoué sur une clé dupliquée peuvent être ignorées).  
  
     Pour plus d’informations, consultez [Propriétés personnalisées des sources CDC](../../integration-services/data-flow/cdc-source-custom-properties.md).  
  
11. Pour mettre à jour le mappage entre les colonnes externes et les colonnes de sortie, cliquez sur **Colonnes** et sélectionnez des colonnes dans la liste **Colonne externe** .  
  
12. Si vous le souhaitez, mettez à jour les valeurs des colonnes de sortie en supprimant les valeurs dans la liste **Colonne de sortie** .  
  
13. Pour configurer l'affichage des erreurs, cliquez sur **Sortie d'erreur**.  
  
14. Vous pouvez cliquer sur **Aperçu** pour afficher jusqu’à 200 lignes de données extraites par la source CDC.  
  
15. Cliquez sur **OK**.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de source CDC &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/cdc-source-editor-connection-manager-page.md)   
 [Éditeur de source CDC &#40;page Colonnes&#41;](../../integration-services/data-flow/cdc-source-editor-columns-page.md)   
 [Éditeur de source CDC &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/cdc-source-editor-error-output-page.md)  
  
  
