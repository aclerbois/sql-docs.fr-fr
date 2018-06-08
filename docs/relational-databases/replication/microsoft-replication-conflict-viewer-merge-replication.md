---
title: Outil de résolution des conflits de réplication Microsoft (réplication de fusion) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.replconflictviewer.cvmerge.f1
ms.assetid: bfef5e21-ac04-4bc5-a55e-595421e34923
caps.latest.revision: 24
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: c0a46f863c810f41422c96267e2be21b0a5f47ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-replication-conflict-viewer-merge-replication"></a>Outil de résolution des conflits de réplication Microsoft (réplication de fusion)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'outil de résolution des conflits de réplication permet d'afficher et de résoudre les conflits qui se sont produits pendant la synchronisation de la réplication. Des conflits ont lieu lorsque les mêmes données sont modifiées sur deux serveurs différents, par exemple sur un Éditeur et un Abonné ou sur deux Abonnés différents. La réplication résout automatiquement les conflits à l'aide du résolveur de conflits sélectionné lors de la création de l'article. Cependant, l'outil de résolution des conflits permet de choisir une résolution différente des conflits lorsque c'est nécessaire. Les conflits suivants peuvent se produire :  
  
-   Conflits de mise à jour. Les conflits de mise à jour se produisent lorsque les mêmes données sont modifiées dans deux emplacements. L'un « gagne », l'autre « perd ». Vous avez le choix entre conserver les données existantes (les gagnantes) et remplacer les données existantes par celles qui sont en conflit (les perdantes), ou fusionner les données gagnantes et perdantes et mettre à jour les données existantes.  
  
-   Conflits d'insertion. Les conflits d'insertion se produisent lorsqu'une ligne est insérée à un emplacement qui ne respecte pas une règle de cohérence des données lorsqu'elles sont fusionnées avec des modifications à d'autres emplacements. Vous avez le choix entre conserver les données existantes (les gagnantes) et remplacer les données existantes par celles qui sont en conflit (les perdantes), ou fusionner les données gagnantes et perdantes et mettre à jour les données existantes.  
  
-   Conflits de suppression. Ce type de conflits survient lorsque la même ligne est supprimée à un endroit et modifiée à un autre.  
  
 Lorsque les conflits sont résolus pendant la synchronisation, les données provenant de la ligne perdante sont écrites dans une table de conflits. Si vous acceptez la résolution d'origine ou choisissez une autre résolution du conflit, la ligne en conflit journalisée est supprimée de la table des conflits. Vous devez examiner régulièrement les conflits afin de réduire la taille des tables de suivi des conflits.  
  
> [!NOTE]  
>  Les conflits qui concernent des enregistrements logiques ne sont pas affichés dans l'Outil de résolution des conflits. Pour afficher des informations sur ces conflits, utilisez des procédures stockées de réplication. Pour plus d’informations, consultez [Afficher les informations relatives aux conflits pour les publications de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/view-conflict-information-for-merge-publications.md).  
  
## <a name="options"></a>Options  
 L'Outil de résolution des conflits comporte deux parties. La partie supérieure de la boîte de dialogue affiche la liste des conflits de la table sélectionnée. Lorsque vous cliquez sur un élément de cette liste, les informations sur le conflit s'affichent dans la partie inférieure de la boîte de dialogue.  
  
 Les informations décrivant la cause du conflit (par exemple, une même ligne mise à jour dans le serveur de publication et dans l'abonné) s'affichent dans la partie inférieure de la boîte de dialogue. Les données en conflit de la partie inférieure sont affichées dans les deux colonnes correspondantes (**Gagnant du conflit** et **Perdant du conflit**). Si le conflit existe entre des données mises à jour et des données supprimées, il est possible qu'aucune donnée ne soit affichée pour le côté supprimé du conflit. Dans ce cas, l'outil de résolution des conflits affiche un message dans une des colonnes qui indique que la ligne a été supprimée à un emplacement et mise à jour à un autre. Il indique également la résolution suggérée.  
  
 Les données qu'il n'est pas possible de modifier dans l'Outil de résolution des conflits (ex. données **rowguid** ) s'affichent en lecture seule dans une case ombrée.  
  
 **Sauvegarde de la base de données**  
 Choisissez une base de données qui comporte des publications faisant l'objet de conflits.  
  
 **Publication**  
 Choisissez une publication qui comporte des tables faisant l'objet de conflits.  
  
 **Table**  
 Choisissez une table qui comporte des conflits.  
  
 **Définir le filtre**  
 Cliquez sur ce bouton pour ouvrir la boîte de dialogue **Définir les filtres** .  
  
 **Appliquer ou supprimer le filtre**  
 Applique ou supprime un filtre défini dans la boîte de dialogue **Définir les filtres** .  
  
 **Tout sélectionner**  
 Sélectionne tous les conflits de la grille.  
  
 **Aucune sélection**  
 Désélectionne tous les conflits de la grille.  
  
 **Supprimer**  
 Supprime les conflits sélectionnés et leurs métadonnées associées des tables système de réplication. Vous obtenez le même résultat en cliquant sur le bouton Soumettre le gagnant (sans modifier les données) de chaque conflit sélectionné.  
  
 **Afficher toutes les colonnes**  
 Sélectionnez cette option pour afficher toutes les colonnes de la table.  
  
 **Afficher les cinq premières colonnes et les autres colonnes qui contiennent des données conflictuelles**  
 Sélectionnez cette option pour afficher les cinq premières colonnes et toute colonne qui comporte des conflits. Cette option est utile lorsque la table comporte de nombreuses colonnes si vous voulez afficher uniquement les colonnes les plus pertinentes pour la résolution du conflit. Les cinq premières colonnes figurent toujours dans cette vue du fait que les champs qui identifient une ligne (par exemple la clé primaire ou les noms des champs) se trouvent souvent parmi les premières colonnes de la table.  
  
 **Informations sur la colonne** (**…**)  
 Affiche les informations sur la colonne : **Nom de la table**, **Nom de la colonne**, **Type de données**et **Valeur de la colonne**. Le champ**Valeur de la colonne** est modifiable à moins que la valeur ne soit affichée en lecture seule.  
  
 **Soumettre le gagnant**  
 Conserve la ligne que l'outil de résolution des conflits a déterminée gagnante. Vous pouvez modifier la valeur de n'importe quelle colonne qui n'est pas affichée en lecture seule avant de cliquer sur ce bouton.  
  
 **Soumettre le perdant**  
 Accepte la ligne que l'outil de résolution des conflits a déterminée perdante. Vous pouvez modifier la valeur de n'importe quelle colonne qui n'est pas affichée en lecture seule avant de cliquer sur ce bouton.  
  
 **Consigner les détails de ce conflit**  
 Activez cette case pour enregistrer les détails du conflit dans un fichier. Pour spécifier l'emplacement du fichier, pointez sur le menu **Affichage** et cliquez sur **Options**. Entrez une valeur ou cliquez sur le bouton d'exploration (**...**) et allez au fichier voulu. Cliquez sur **OK** pour fermer la boîte de dialogue **Options** .  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et résoudre les conflits de données pour les publications de fusion &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [Détection et résolution des conflits de réplication de fusion avancée](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
