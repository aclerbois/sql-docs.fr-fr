---
title: Mappage de Sybase ASE et Types de données SQL Server (SybaseToSQL) | Documents Microsoft
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
- Mapping Sybase ASE Schemas to SQL Server Schemas
- Type Mapping Settings
ms.assetid: 784365d3-df4e-47ab-8ee0-d8392b52f510
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 8015121c04b75b0f99d56503715a43539db8dceb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-sybase-ase-and-sql-server-data-types-sybasetosql"></a>Mappage de Sybase ASE et Types de données SQL Server (SybaseToSQL)
Les types de base de données Sybase Adaptive Server Enterprise (ASE) diffèrent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou les types de base de données SQL Azure. Lorsque vous convertissez des objets de base de données ASE à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de SQL Azure, vous devez spécifier le mappage des types de données à partir de ASE à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les sections suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA a un jeu de mappages de types de données. Pour obtenir la liste des mappages par défaut, consultez [les paramètres de projet &#40;le mappage de Type&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md).  
  
## <a name="type-mapping-inheritance"></a>Mappage d’héritage de type  
Vous pouvez personnaliser les mappages de type pour le projet au niveau, le niveau de catégorie objet (par exemple, toutes les procédures stockées) ou objet. Les paramètres sont héritées du niveau supérieur, sauf substitution à un niveau inférieur. Par exemple, si vous mappez **smallmoney** à **money** au niveau des projets, tous les objets dans le projet utilisera ce mappage, sauf si vous personnalisez le mappage au niveau de l’objet du niveau de catégorie d’objet ou.  
  
Lorsque vous affichez la **le mappage de Type** onglet SSMA, l’arrière-plan est coloré pour afficher lesquelles mappages de type sont héritées. L’arrière-plan d’un mappage de type est jaune pour tout mappage de type hérité et le blanc pour tout mappage spécifié au niveau actuel.  
  
## <a name="customizing-data-type-mappings"></a>Personnalisation mappages de types de données  
La procédure suivante montre comment mapper des types de données au niveau du projet, une base de données ou un niveau de l’objet.  
  
**Pour mapper les types de données**  
  
1.  Pour personnaliser le mappage de type de données pour la totalité du projet, ouvrez le **les paramètres de projet** boîte de dialogue :  
  
    1.  Sur le **outils** menu, sélectionnez **les paramètres de projet**.  
  
    2.  Dans le volet gauche, sélectionnez **le mappage de Type**.  
  
        Le graphique de mappage de type et les boutons s’affichent dans le volet droit.  
  
    Ou, pour personnaliser le type de données mise en correspondance à la base de données, table, vue ou niveau de la procédure stockée, sélectionnez la base de données, une catégorie d’objet ou un objet dans l’Explorateur de métadonnées de Sybase :  
  
    1.  Dans l’Explorateur de métadonnées de Sybase, sélectionnez le dossier ou l’objet que vous souhaitez personnaliser.  
  
    2.  Dans le volet droit, cliquez sur le **le mappage de Type** onglet.  
  
2.  Pour ajouter un nouveau mappage, procédez comme suit :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Sous **type de Source de**, sélectionnez le type de données ASE à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur minimale de données pour le mappage dans le **à partir de** zone et spécifiez la longueur maximale des données pour le mappage dans le **à** boîte.  
  
        Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données.  
  
    4.  Sous **type cible**, sélectionnez la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou type de données SQL Azure.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer** boîte.  
  
    5.  Cliquez sur **OK**.  
  
3.  Pour modifier un mappage de type de données, procédez comme suit :  
  
    1.  Cliquez sur **Modifier**.  
  
    2.  Sous **type de Source de**, sélectionnez le type de données ASE à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur minimale de données pour le mappage dans le **à partir de** zone et spécifiez la longueur maximale des données pour le mappage dans le **à** boîte.  
  
        Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données.  
  
    4.  Sous **type cible**, sélectionnez la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou type de données SQL Azure.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer** zone, puis cliquez sur **OK**.  
  
4.  Pour supprimer un mappage de type de données personnalisé, procédez comme suit :  
  
    1.  Sélectionnez la ligne dans la liste de mappage de type qui contient le mappage de type de données à supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
        Impossible de supprimer les mappages hérités. Toutefois, héritée de mappages sont remplacés par des mappages personnalisés sur un objet spécifique ou la catégorie d’objet.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration consiste à [créer un rapport d’évaluation](http://msdn.microsoft.com/en-us/eb996b7c-1eef-4f73-b5e6-2fa6faf7336c) ou [objets de base de données de convertir le ASE Sybase à la syntaxe SQL Server ou SQL Azure](http://msdn.microsoft.com/en-us/509cb65d-2f54-427a-83d7-37919cc4e3e3). Si vous créez un rapport d’évaluation, les objets de Sybase ASE sont automatiquement convertis durant l’évaluation.  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de Sybase ASE à SQL Server - base de données SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
