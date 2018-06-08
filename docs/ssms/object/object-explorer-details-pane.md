---
title: Volet Détails de l’Explorateur d’objets | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-objects
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.summary.general.f1
- sql13.swb.summary.report.f1
helpviewer_keywords:
- object search [SQL Server], Object Explorer
- Object Explorer
- object search [SQL Server]
- searching objects [SQL Server]
ms.assetid: b963e3c2-dc9e-4d38-bd28-2e00fe9e0e47
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0d59e3448b3e833adacab7b37708725e8ff36864
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="object-explorer-details-pane"></a>Volet Détails de l'Explorateur d'objets
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Détails de l'Explorateur d'objets, un composant de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)], fournit une vue tabulaire de tous les objets se trouvant dans le serveur ainsi qu'une interface utilisateur pour les gérer. Les fonctionnalités de l’Explorateur d’objets varient légèrement en fonction du type de serveur. Il contient toutefois des fonctionnalités de développement pour les bases de données et des fonctionnalités de gestion pour tous les types de serveurs.  
  
Le volet Détails de l’Explorateur d’objets est visible dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] par défaut. Si tel n’est pas le cas, dans le menu **Affichage** , cliquez sur **Détails de l’Explorateur d’objets** ou appuyez sur **F7**.  
  
> [!NOTE]  
> [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] affiche les dates avec le format des Options régionales et linguistiques Microsoft Windows en vigueur lors du démarrage de [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] . Redémarrez [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] pour que les nouveaux paramètres soient pris en compte.  
  
## <a name="object-explorer-details"></a>Détails de l’Explorateur d’objets  
Vous pouvez utiliser Détails de l’Explorateur d’objets pour parcourir les dossiers et les objets de votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] . Sur les systèmes d'exploitation 32 bits, l'Explorateur d'objets peut uniquement afficher 64 000 objets. Vous devez sélectionner une icône pour accéder à des objets supplémentaires.  
  
Détails de l'Explorateur d'objets comprend une barre d'outils contenant les icônes décrites dans le tableau suivant. Les icônes disponibles varient selon la situation.  
  
|Icône|Action|  
|--------|----------|  
|**Précédent**|Passe aux éléments précédents affichés dans Détails de l'Explorateur d'objets. Réexécute une recherche lorsque l'affichage précédent est le résultat d'une opération de recherche.|  
|**Suivant**|Passe à l’écran suivant après une opération **Précédent** .|  
|**Monter**|Passe à l'objet ou au dossier parent.|  
|**Synchroniser**|Définit l'objet sélectionné dans Détails de l'Explorateur d'objets comme focus de l'Explorateur d'objets.|  
|**Filter**|Affiche, si disponible, un sous-ensemble configurable d'objets.|  
|**Actualiser**|Actualise l'affichage dans Détails de l'Explorateur d'objets.|  
|**Recherche**|Fournit une zone pour entrer un terme de recherche pour certains objets de base de données.|  
  
### <a name="column-header-selections"></a>Sélections d'en-tête de colonne  
Détails de l'Explorateur d'objets possède des colonnes sélectionnables. Vous pouvez cliquer avec le bouton droit sur n'importe quel en-tête de colonne et sélectionner les éléments à afficher. Vos sélections sont rendues persistantes à travers les différents objets que vous parcourez. Les sélections pour chaque utilisateur sont conservées lorsque vous quittez et redémarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)].  
  
> [!CAUTION]  
> L'affichage de toutes les colonnes pour certains types d'objets (notamment Database) peut ralentir légèrement le rendu d'affichage pour les jeux d'objets volumineux.  
  
### <a name="sorting"></a>Tri  
Cliquez une fois sur un en-tête de colonne pour trier la grille selon cette colonne. Cliquez à nouveau sur le même en-tête de colonne pour inverser l'ordre de trie par rapport à cette colonne. Les sélections de tri sont conservées pour chaque utilisateur dans tous les objets et dossiers, et au redémarrage de [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] .  
  
### <a name="filtering"></a>Filtrage  
Vous pouvez filtrer certaines listes d’objets affichées dans Détails de l’Explorateur d’objets à l’aide de l’icône **Filtrer** située dans la barre d’outils Détails de l’Explorateur d’objets. L'icône est activée lorsque le filtrage est possible.  
  
### <a name="details-pane"></a>Volet Détails  
La zone inférieure de Détails de l'Explorateur d'objets contient un panneau qui affiche certains détails su l'objet sélectionné. Si vous sélectionnez plusieurs objets, seul le nombre d'objets est affiché. Quand des éléments sont sélectionnés dans le volet, cliquez sur l’icône **Copier** pour copier le texte affiché dans le Presse-papiers.  
  
La touche de direction Bas déplace la sélection vers l'élément suivant de la liste. La touche de direction Haut déplace la sélection vers l'élément précédent de la liste. Maintenez la touche de direction Bas enfoncée pour accélérer le déplacement entre les éléments. La sélection s'arrête en haut ou en bas de la liste de propriétés.  
  
Taper le premier caractère du nom d'une propriété déplace la sélection vers la propriété suivante qui commence par ce caractère.  
  
Lorsque les propriétés sont disposées en plusieurs colonnes, utilisez la flèche gauche et la flèche droite pour passer aux colonnes adjacentes.  
  
Pour copier des valeurs de propriété dans le Presse-papiers, utilisez les combinaisons de touches du clavier suivantes :  
  
-   Ctrl+C copie la valeur de propriété.  
  
-   Ctrl+Maj+C copie le nom et la valeur de la propriété, séparés par une tabulation.  
  
Utilisez le menu contextuel dans le volet de propriétés Détails de l'Explorateur d'objets pour copier toutes les propriétés ou tous les noms et propriétés dans le Presse-papiers.  
  
Lorsque la largeur du champ ne permet pas d'afficher complètement une valeur de propriété, pointez le curseur de la souris sur la valeur ou mettez le focus sur la valeur de propriété pour activer une info-bulle indiquant la valeur de propriété dans son intégralité. Les lecteurs d'écran d'accessibilité indiquent la valeur de propriété complète lorsque l'utilisateur met le focus sur la longue valeur de propriété.  
  
Si le nombre de propriétés dans le volet de propriétés dépasse le nombre qui tient dans la zone de contenu, une barre de défilement s'affiche à droite du volet de propriétés. Utilisez la barre de défilement pour ajuster la vue des valeurs de propriété dans la zone de contenu.  
  
### <a name="multiple-object-selection"></a>Sélection de plusieurs objets  
Détails de l'Explorateur d'objets prend en charge la sélection de plusieurs objets. Par exemple, si vous sélectionnez des tables dans l’Explorateur d’objets, puis que vous maintenez la touche **Ctrl** enfoncée dans la fenêtre Détails de l’Explorateur d’objets, vous pouvez sélectionner plusieurs tables, cliquer dessus avec le bouton droit, puis sélectionner **Supprimer** ou **Générer un script de la table en tant que** pour effectuer immédiatement l’action sur toutes les tables sélectionnées. Les commandes de copie standard copient le texte affiché dans le Presse-papiers.  
  
## <a name="sql-server-object-search"></a>Recherche d'objets SQL Server  
Caractères génériques  
  
-   Les caractères génériques standard sont pris en charge. Par exemple, la chaîne de recherche **dm_os%counters** retourne dm_os_memory_cache_counters et dm_os_performance_counters. Pour plus d’informations, consultez [Procédure : exécution d’une recherche avec des caractères génériques](http://msdn.microsoft.com/en-us/449600f8-cc87-4b3f-878a-59c158a88a40).  
  
Étendue de recherche  
  
-   L'étendue de la recherche correspond au focus actuel dans l'arborescence de l'Explorateur d'objets. Par exemple, si le focus se trouve sur une base de données dans l’Explorateur d’objets, la chaîne de recherche **dm_os%counters** retourne dm_os_memory_cache_counters et dm_os_performance_counters dans cette base de données. Si le focus de l'Explorateur d'objets se trouve sur le nœud Bases de données, toutes les bases de données sont explorées et plusieurs instances des vues dynamiques sont retournées.  
  
Jeux volumineux  
  
-   Les recherches portant sur des jeux d'objets volumineux peut prendre du temps et nuire aux performances du serveur.  
  
## <a name="see-also"></a> Voir aussi  
[Explorateur d'objets](../../ssms/object/object-explorer.md)  
  
