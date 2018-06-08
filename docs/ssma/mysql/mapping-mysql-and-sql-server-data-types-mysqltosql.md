---
title: Mappage des Types de données SQL Server (MySQLToSQL) et de MySQL | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
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
helpviewer_keywords:
- Mapping, customize data type mapping
- Mapping, Type mapping
ms.assetid: 14f98054-13b4-4231-a6b0-2452f3b9941d
caps.latest.revision: 15
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d35530153b4e26f865b6fd8fefa102718752dafa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-mysql-and-sql-server-data-types-mysqltosql"></a>Mappage des Types de données SQL Server (MySQLToSQL) et MySQL
Les types de base de données MySQL diffèrent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou les types de base de données SQL Azure. Lorsque vous convertissez des objets de base de données MySQL à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de SQL Azure, vous devez spécifier le mappage des types de données de MySQL vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les procédures suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA a un jeu de mappages de types de données. Pour obtenir la liste des mappages par défaut, consultez [les paramètres de projet &#40;le mappage de Type&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md).  
  
## <a name="type-mapping-inheritance"></a>Mappage d’héritage de type  
Vous pouvez personnaliser les mappages de type pour le projet au niveau, le niveau de catégorie objet (par exemple, toutes les procédures stockées) ou objet. Les paramètres sont hérités du niveau supérieur, sauf si elles sont remplacées à un niveau inférieur. Par exemple, si vous mappez **smallint** à **int** au niveau du projet, tous les objets dans le projet utilisera ce mappage, sauf si vous personnalisez le mappage au niveau de l’objet ou une catégorie.  
  
Lorsque vous affichez la **le mappage de Type** onglet SSMA, l’arrière-plan est coloré pour afficher lesquelles mappages de type sont héritées. L’arrière-plan d’un mappage de type est jaune pour tout mappage de type hérité et le blanc pour tout mappage est spécifié au niveau actuel.  
  
## <a name="customizing-data-type-mappings"></a>Personnalisation mappages de types de données  
  
-   **Pour mapper les types de données :**  
  
    Les procédures suivantes montrent comment mapper des types de données au niveau du projet, une base de données ou un niveau de l’objet de base de données :  
  
    1.  Pour personnaliser le mappage de type de données pour la totalité du projet, ouvrez le **les paramètres de projet** boîte de dialogue. Dans le menu Outils, sélectionnez **les paramètres de projet**.  
  
        Dans le volet gauche, sélectionnez **le mappage de Type**. Le graphique de mappage de type et les boutons s’affichent dans le volet droit.  
  
    2.  Pour personnaliser les mappages de types de données au niveau de la base de données ou une table, sélectionnez la base de données ou la table dans l’Explorateur de métadonnées de MySQL. Dans l’Explorateur de métadonnées MySQL, sélectionnez le dossier ou un objet à personnaliser.  
  
        Dans le volet droit, cliquez sur **le mappage de Type**.  
  
-   **Pour ajouter un nouveau mappage, procédez comme suit :**  
  
    1.  Dans le volet de mappage de Type, cliquez sur **ajouter** .  
  
    2.  Dans le nouveau Type de boîte de dialogue mappage, sous **type de Source de**, sélectionnez le type de données MySQL à mapper.  
  
    3.  Si le type requiert une longueur, spécifier les longueurs minimale et maximale des données pour le mappage en sélectionnant le **de** et **à** cases à cocher et en entrant les valeurs.  
  
    4.  Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données. Sous **type cible**, sélectionnez la cible SQL Server ou un type de données SQL Azure.  
  
        1.  Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer par** zone, puis cliquez sur **OK**.  
  
        2.  Certains types nécessitent un type de données cible **précision** et **échelle**. Si nécessaire, entrez la nouvelle précision et l’échelle dans le **remplacer par** zone, puis cliquez sur **OK**.  
  
-   **Pour modifier un mappage de type, procédez comme suit :**  
  
    1.  Dans le volet de mappage de Type, cliquez sur **modifier**.  
  
    2.  Dans le mappage de Type liste boîte de dialogue, sous **type de Source de**, sélectionnez le type de données MySQL à mapper.  
  
    3.  Si le type requiert une longueur, spécifier les longueurs minimale et maximale des données pour le mappage en sélectionnant le **de** et **à** cases à cocher et en entrant les valeurs.  
  
    Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données. Sous **type cible**, sélectionnez la cible SQL Server ou un type de données SQL Azure.  
  
    1.  Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer par** zone, puis cliquez sur **OK**.  
  
    2.  Certains types nécessitent un type de données cible **précision** et **échelle** . Si nécessaire, entrez la nouvelle précision et l’échelle dans le **remplacer par** zone, puis cliquez sur **OK** .  
  
-   **Pour supprimer un mappage de type de données, procédez comme suit :**  
  
    1.  Dans le volet de mappage de Type, sélectionnez la ligne dans la liste de mappage de type qui contient le mappage de type de données à supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante du processus de migration consiste à [créer un rapport d’évaluation](http://msdn.microsoft.com/en-us/2a56a003-3b0f-453a-963c-00c9e40933ec) ou [MySQL de convertir les objets de base de données dans SQL Server ou SQL Azure syntaxe](http://msdn.microsoft.com/en-us/ac21850b-fb32-4704-9985-5759b7c688c7). Si vous créez un rapport, les objets de MySQL sont automatiquement convertis durant l’évaluation.  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données de migration de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
