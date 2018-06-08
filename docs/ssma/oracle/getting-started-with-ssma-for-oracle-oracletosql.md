---
title: Prise en main de SSMA pour Oracle (OracleToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-oracle
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SSMA for Oracle, Metadata Explorers
- SSMA for Oracle, Toolbars
ms.assetid: df79664c-972e-4bef-865a-ce609789fee7
caps.latest.revision: 9
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 7979226878df0f30983d262c69b74dfc439eaab3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-ssma-for-oracle-oracletosql"></a>Prise en main de SSMA pour Oracle (OracleToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour Oracle vous permet de rapidement convertir des schémas de base de données Oracle à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] schémas, téléchargez les schémas qui en résulte dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et migrer les données d’Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Cette rubrique présente le processus d’installation et vous aide à vous familiariser avec l’interface utilisateur SSMA.  
  
## <a name="installing-ssma"></a>L’installation de SSMA  
Pour utiliser SSMA, vous devez d’abord installer le programme client SSMA sur un ordinateur pouvant accéder à la base de données Oracle source et de l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous devez installer un Pack d’extension et au moins un des fournisseurs Oracle (OLE DB ou [!INCLUDE[vstecado](../../includes/vstecado_md.md)]) sur l’ordinateur qui est en cours d’exécution [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Ces composants prennent en charge la migration des données et l’émulation des fonctions du système Oracle. Pour obtenir des instructions d’installation, consultez [l’installation de SSMA pour Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md).  
  
Pour démarrer SSMA, cliquez sur **Démarrer**, pointez sur **tous les programmes**, pointez sur  **[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant pour Oracle**, puis cliquez sur  **[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant pour Oracle**.  
  
## <a name="ssma-for-oracle-user-interface"></a>SSMA pour l’Interface utilisateur Oracle  
Une fois SSMA est installé, vous pouvez utiliser SSMA pour migrer des bases de données Oracle [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Cela permet de vous familiariser avec l’interface utilisateur SSMA avant de commencer. Le diagramme suivant montre l’interface utilisateur de SSMA, y compris les explorateurs de métadonnées, métadonnées, barres d’outils, sortie et volet Liste d’erreur :  
  
![SSMA pour interface utilisateur d’Oracle](../../ssma/oracle/media/ssma_oracle_ui.jpg "SSMA pour interface utilisateur d’Oracle")  
  
Pour démarrer une migration, vous devez d’abord créer un nouveau projet. Ensuite, vous vous connectez à une base de données Oracle. Après une connexion réussie, les schémas d’Oracle seront affiche dans l’Explorateur de métadonnées d’Oracle. Vous pouvez ensuite les objets avec le bouton droit dans l’Explorateur de métadonnées d’Oracle pour effectuer des tâches telles que créent des rapports qui évaluent les conversions en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez également effectuer ces tâches en utilisant les menus et barres d’outils.  
  
Vous devez également vous connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Après une connexion réussie, une hiérarchie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données apparaissent dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées. Après la conversion de schémas Oracle à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] schémas, sélectionnez ces schémas convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Explorateur de métadonnées, puis synchronisez les schémas avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Après avoir synchronisé des schémas convertis avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez revenir à l’Explorateur de métadonnées d’Oracle et migrer des données à partir des schémas Oracle dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données.  
  
Pour plus d’informations sur ces tâches et comment les exécuter, consultez [migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md).  
  
Les sections suivantes décrivent les fonctionnalités de l’interface utilisateur SSMA.  
  
### <a name="metadata-explorers"></a>Explorateurs de métadonnées  
SSMA contient deux explorateurs de métadonnées pour parcourir et d’effectuer des actions sur Oracle et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données.  
  
#### <a name="oracle-metadata-explorer"></a>Explorateur de métadonnées d’Oracle  
Explorateur de métadonnées Oracle affiche des informations sur les schémas d’Oracle. À l’aide de l’Explorateur de métadonnées Oracle, vous pouvez effectuer les tâches suivantes :  
  
-   Parcourir les objets dans chaque schéma.  
  
-   Sélectionner les objets pour la conversion, puis convertir les objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe. Pour plus d’informations, consultez [conversion des schémas Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
-   Sélectionner des tables pour la migration de données, et migrez les données à partir de ces tables à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations, consultez [migration des données Oracle dans SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>Explorateur de métadonnées SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées affiche des informations sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], SSMA récupère les métadonnées relatives à cette instance et le stocke dans le fichier projet.  
  
Vous pouvez utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour sélectionner des objets de base de données Oracle convertis, puis de synchroniser ces objets avec l’instance de l’Explorateur de métadonnées [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Pour plus d’informations, consultez [le chargement des objets de base de données convertie dans SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
### <a name="metadata"></a>Métadonnées  
À droite de chaque Explorateur de métadonnées des onglets qui décrivent l’objet sélectionné. Par exemple, si vous sélectionnez une table dans l’Explorateur de métadonnées Oracle, six onglets seront affiche : **Table**, **SQL**, **le mappage de Type, le rapport**, **propriétés**, et **données**. Le **rapport** onglet contient des informations uniquement après avoir créé un rapport qui contient l’objet sélectionné. Si vous sélectionnez une table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Explorateur de métadonnées, trois onglets apparaissent : **Table**, **SQL**, et **données**.  
  
La plupart des paramètres de métadonnées sont en lecture seule. Toutefois, vous pouvez modifier les métadonnées suivantes :  
  
-   Dans l’Explorateur de métadonnées d’Oracle, vous pouvez modifier les procédures et mappages de type. Pour convertir les procédures modifiées et mappages de type, apporter des modifications avant de convertir des schémas.  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Explorateur de métadonnées, vous pouvez modifier le [!INCLUDE[tsql](../../includes/tsql_md.md)] pour les procédures stockées. Pour voir ces modifications dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], apporter ces modifications avant de charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Modifications apportées dans l’Explorateur de métadonnées sont répercutées dans les métadonnées de projet, pas dans les bases de données source ou cible.  
  
### <a name="toolbars"></a>Barres d'outils  
SSMA a deux barres d’outils : une barre d’outils du projet et une barre d’outils de migration.  
  
#### <a name="the-project-toolbar"></a>La barre d’outils du projet  
La barre d’outils du projet contient des boutons pour l’utilisation de projets, la connexion à Oracle et la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Ces boutons ressemblent aux commandes sur le **fichier** menu.  
  
#### <a name="migration-toolbar"></a>Barre d’outils de migration  
Le tableau suivant affiche les commandes de la barre d’outils de la migration :  
  
|Bouton|Fonction|  
|------|--------|  
|**Créer des rapports**|Convertit les objets sélectionnés sur Oracle [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe, puis crée un rapport qui indique la réussite de la conversion a.<br /><br />Cette commande est désactivée, sauf si les objets sont sélectionnés dans l’Explorateur de métadonnées d’Oracle.|  
|**Convertir le schéma**|Convertit les objets sélectionnés sur Oracle [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] objets.<br /><br />Cette commande est désactivée, sauf si les objets sont sélectionnés dans l’Explorateur de métadonnées d’Oracle.|  
|**Migrer des données**|Migre les données à partir de la base de données Oracle [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Avant d’exécuter cette commande, vous devez convertir les schémas Oracle à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] schémas, puis charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br />Cette commande est désactivée, sauf si les objets sont sélectionnés dans l’Explorateur de métadonnées d’Oracle.|  
|**Arrêter**|Arrête le processus en cours.|  
  
### <a name="menus"></a>Menus  
Le tableau suivant présente les menus SSMA.  
  
|Menu| Description|  
|----|-----------|  
|**Fichier**|Contient des commandes pour l’utilisation de projets, la connexion à Oracle et la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|**Modifier**|Contient des commandes pour la recherche et utilisation du texte dans les pages Détails, telles que la copie [!INCLUDE[tsql](../../includes/tsql_md.md)] dans le volet SQL. Contient également la **gérer les signets** option, où vous pourrez voir une liste des signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.|  
|**Affichage**|Contient le **synchroniser les explorateurs de métadonnées** commande. Qui synchronise les objets entre l’Explorateur de métadonnées d’Oracle et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées. Contient également des commandes pour afficher et masquer le **sortie** et **liste d’erreurs** volets et une option **dispositions** pour gérer les mises en page.|  
|**Outils**|Contient des commandes pour créer des rapports et de migrer des objets et des données. Permet également d’accéder à la **paramètres globaux** et **les paramètres de projet** boîtes de dialogue.|  
|**Testeur**|Contient des commandes pour créer et utiliser des cas de test, référentiel et le système de gestion de sauvegarde.|  
|**Aide**|Fournit l’accès à l’aide de SSMA et à la **sur** boîte de dialogue.|  
  
### <a name="output-pane-and-error-list-pane"></a>Volets de sortie et liste erreur  
Le **vue** menu fournit des commandes pour activer/désactiver la visibilité du volet sortie et le volet de la liste d’erreurs :  
  
-   Le volet de résultats affiche les messages d’état à partir de SSMA pendant la conversion de l’objet, la synchronisation de l’objet et la migration des données.  
  
-   Le volet de la liste d’erreurs affiche des messages d’information, d’avertissement et erreur dans une liste pouvant être triée.  
  
## <a name="see-also"></a>Voir aussi  
[Migration de données Oracle dans SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md)  
[Référence de l’Interface utilisateur &#40;OracleToSQL&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
