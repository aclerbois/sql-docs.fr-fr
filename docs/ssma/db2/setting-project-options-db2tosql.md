---
title: Définition des Options de projet (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: f325a606-97ac-48bc-b344-b55f5e086a48
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 82cfce0e662130c38c8c3040d7366045d0d3fccd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-project-options-db2tosql"></a>Définition des Options de projet (DB2ToSQL)
Pour chaque projet SSMA, vous pouvez définir les options de niveau projet. Ces options déterminent la conversion de l’objet, lors du chargement de l’objet de paramètres de migration des données et d’interface utilisateur. Avant de convertir des objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou migrer des données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vérifiez que les options de configuration sont appropriées pour le projet.  
  
SSMA vous permet de configurer les options par défaut pour tous les projets. Ces options sont appliquées à n’importe quel nouveau projet que vous créez. Vous pouvez ensuite personnaliser les options de chaque projet.  
  
## <a name="configuration-options-and-modes"></a>Modes et les Options de configuration  
SSMA a cinq jeux de paramètres du projet :  
  
-   Informations sur le projet  
  
-   Général (Conversion, la Migration, le chargement d’objets)  
  
-   Synchronization  
  
-   INTERFACE GRAPHIQUE UTILISATEUR  
  
-   Mappage de type  
  
Il comprend quatre modes de configuration de ces paramètres :  
  
-   Par défaut  
  
-   Optimistic  
  
-   Complète  
  
-   Custom  
  
Le mode par défaut est recommandé pour la plupart des utilisateurs. Le mode conserve plus de la syntaxe de DB2 actuelle et est plus facile à lire. Toutefois, en conservant la syntaxe actuelle peut-être pas précise. Si la syntaxe de DB2 doit être convertie en équivalent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe, le mode plein effectue la conversion la plus complète, mais le code résultant peut être plus difficile à lire. Dans le mode personnalisé, vous définissez les options.  
  
Pour plus d’informations sur les paramètres et la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet &#40;Conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md)  
  
-   [Paramètres du projet &#40;Migration&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-migration-db2tosql.md)  
  
-   [Paramètres du projet&#40;synchronisation&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md)  
  
-   [Paramètres du projet &#40;GUI&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-gui-db2tosql.md)  
  
-   [Paramètres du projet &#40;le mappage de Type&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md)  
  
## <a name="setting-project-options"></a>Définition des Options de projet  
Dans SSMA, vous pouvez configurer les paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration de SSMA et appliqués à tout nouveau projet que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Sur le **outils** menu, cliquez sur **les paramètres de projet par défaut**.  
  
2.  Dans le **les paramètres de projet par défaut** boîte de dialogue, utilisez une des procédures suivantes :  
  
    -   Sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichées ou modifiées à partir de **Version cible de la Migration** liste déroulante cliquez **général** en bas du volet gauche, puis sélectionnez Conversion ou la Migration.  
  
    -   Pour sélectionner un mode prédéfini, dans le **Mode** zone de liste déroulante, sélectionnez **par défaut**, **Optimistic**, ou **complète**.  
  
    -   Pour spécifier des paramètres personnalisés, sélectionnez ou entrez les nouveaux paramètres ou valeurs.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres pour le projet actuel. Ces paramètres sont enregistrés dans le fichier de projet actuel.  
  
**Pour personnaliser les paramètres pour le projet actuel**  
  
1.  Sur le **outils** menu, cliquez sur **les paramètres de projet**.  
  
2.  Dans le **les paramètres de projet** boîte de dialogue, utilisez une des procédures suivantes :  
  
    -   Pour sélectionner un mode prédéfini, dans le **Mode** zone de liste déroulante, sélectionnez **par défaut**, **Optimistic**, ou **complète**.  
  
    -   Pour spécifier un mode personnalisé, dans le **Mode** boîte, sélectionnez **personnalisé**, puis sélectionnez les paramètres de projet appropriés.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage des types de données source et cible, consultez [DB2 de mappage et les Types de données SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
-   Sinon, vous pouvez convertir les définitions d’objets de base de données DB2 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] définitions d’objets. Pour plus d’informations, consultez [conversion de schémas de DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Mappage de DB2 et des Types de données SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md)  
  
