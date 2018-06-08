---
title: Définition des Options de projet (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Project Options Setting
ms.assetid: 97b70fc8-1f68-4f15-8e22-db5b784ea4ec
caps.latest.revision: 9
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: bd7f7c08b6fbf74b11a99cf9ae662e304ec62869
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-project-options-sybasetosql"></a>Définition des Options de projet (SybaseToSQL)
Pour chaque projet SSMA, vous pouvez définir les options au niveau du projet. Ces options déterminent la conversion de l’objet, lors du chargement de l’objet, SQL azure, l’interface utilisateur et les paramètres de migration de données. Avant de convertir des objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure ou migrer des données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vérifiez que les options de configuration sont appropriées pour le projet.  
  
SSMA vous permet de configurer les options par défaut pour tous les projets. Ces options sont appliquées à n’importe quel nouveau projet que vous créez. Vous pouvez ensuite personnaliser les options de chaque projet.  
  
## <a name="configuration-options-and-modes"></a>Modes et les Options de configuration  
SSMA a cinq jeux de paramètres du projet :  
  
1.  Informations sur le projet  
  
2.  Général (Conversion, Migration et collecte des données)  
  
3.  Synchronization  
  
4.  INTERFACE GRAPHIQUE UTILISATEUR  
  
5.  Mappage de type  
  
Il comprend quatre modes de configuration de ces paramètres :  
  
1.  Par défaut  
  
2.  Optimistic  
  
3.  Complète  
  
4.  Custom  
  
Le mode par défaut est recommandé pour la plupart des utilisateurs. Le mode conserve plus de la syntaxe de Sybase Adaptive Server Enterprise (ASE) actuelle et est plus facile à lire. Toutefois, en conservant la syntaxe actuelle peut-être pas précise. Si la syntaxe ASE doit être convertie en équivalent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou la syntaxe SQL Azure, le mode plein effectue une conversion terminée, mais le code résultant peut être plus difficile à lire. Dans le mode personnalisé, vous définissez les options.  
  
Les paramètres sont décrits dans la section de référence de l’Interface utilisateur de cette documentation. Pour plus d’informations sur les paramètres et la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet &#40;Conversion&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
-   [Paramètres du projet &#40;Migration&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-migration-sybasetosql.md)  
  
-   [Paramètres du projet &#40;synchronisation&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-synchronization-sybasetosql.md)  
  
-   [Paramètres du projet &#40;GUI&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-gui-sybasetosql.md)  
  
-   [Paramètres du projet &#40;le mappage de Type&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md)  
  
-   [Paramètres du projet &#40;base de données SQL Azure &#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-azure-sql-db-sybasetosql.md)  
  
## <a name="setting-project-options"></a>Définition des Options de projet  
Dans SSMA, vous pouvez configurer les paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration de SSMA et appliqués à tout nouveau projet que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**.  
  
2.  Dans le **les paramètres de projet par défaut** boîte de dialogue, utilisez une des procédures suivantes :  
  
    -   Sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichées ou modifiées à partir de **Version cible de la Migration** déplacer vers le bas sur général en bas du volet gauche, puis sélectionnez la Conversion ou la Migration ou SQL Azure.  
  
    -   Pour sélectionner un mode prédéfini, dans le **Mode** zone de liste déroulante, sélectionnez **par défaut**, **Optimistic**, ou **complète**.  
  
    -   Pour spécifier des paramètres personnalisés, activez ou entrez les nouveaux paramètres ou valeurs.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres pour le projet actuel. Ces paramètres sont enregistrés dans le fichier de projet actuel.  
  
**Pour personnaliser les paramètres pour le projet actuel**  
  
1.  Sur le **outils** menu, sélectionnez **les paramètres de projet**.  
  
2.  Dans le **les paramètres de projet** boîte de dialogue, utilisez une des procédures suivantes :  
  
    -   Pour sélectionner un mode prédéfini, dans le **Mode** zone de liste déroulante, sélectionnez **par défaut**, **Optimistic**, ou **complète**.  
  
    -   Pour spécifier un mode personnalisé, dans le **Mode** liste déroulante, sélectionnez **personnalisé**, sélectionnez une option dans le volet gauche, cliquez sur le paramètre ou une valeur dans le volet droit, puis sélectionnez ou entrez le nouveau paramètre ou la valeur.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Si vous souhaitez personnaliser le mappage des types de données source et cible, consultez [mappage Sybase ASE et Types de données SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).  
  
-   Sinon, vous pouvez convertir les définitions d’objets de base de données Sybase dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou définitions d’objets SQL Azure. Pour plus d’informations, consultez [convertir les objets de base de données Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de Sybase ASE à SQL Server - base de données SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
