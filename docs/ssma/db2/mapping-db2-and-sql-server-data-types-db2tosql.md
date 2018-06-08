---
title: Mappage de DB2 et des Types de données SQL Server (DB2ToSQL) | Documents Microsoft
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
ms.assetid: e7e939a8-5e76-4509-beaf-5acd1cab505e
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: e9f470aca25b8f42473ccb8a453edc0d3da911a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-db2-and-sql-server-data-types-db2tosql"></a>Mappage de DB2 et des Types de données SQL Server (DB2ToSQL)
Les types de base de données DB2 diffèrent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] types de base de données. Lorsque vous convertissez des objets de base de données DB2 à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des objets, vous devez spécifier le mappage des types de données à partir de DB2 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les sections suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA a un jeu de mappages de types de données. Pour obtenir la liste des mappages par défaut, consultez [les paramètres de projet &#40;le mappage de Type&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md).  
  
## <a name="type-mapping-inheritance"></a>Mappage d’héritage de type  
Vous pouvez personnaliser les mappages de type pour le projet au niveau, le niveau de catégorie objet (par exemple, toutes les procédures stockées) ou objet. Les paramètres sont hérités du niveau supérieur, sauf si elles sont remplacées à un niveau inférieur. Par exemple, si vous mappez **smallmoney** à **money** au niveau du projet, tous les objets dans le projet utilisera ce mappage, sauf si vous personnalisez le mappage au niveau de l’objet ou une catégorie.  
  
Lorsque vous affichez la **le mappage de Type** onglet SSMA, l’arrière-plan est coloré pour afficher lesquelles mappages de type sont héritées. L’arrière-plan d’un mappage de type est jaune pour tout mappage de type hérité et le blanc pour tout mappage est spécifié au niveau actuel.  
  
## <a name="customizing-data-type-mappings"></a>Personnalisation mappages de types de données  
La procédure suivante montre comment mapper des types de données au niveau du projet, une base de données ou un niveau de l’objet :  
  
**Pour mapper les types de données**  
  
1.  Pour personnaliser le mappage de type de données pour la totalité du projet, ouvrez le **les paramètres de projet** boîte de dialogue :  
  
    1.  Sur le **outils** menu, sélectionnez **les paramètres de projet**.  
  
    2.  Dans le volet gauche, sélectionnez **le mappage de Type**.  
  
        Le graphique de mappage de type et les boutons s’affichent dans le volet droit.  
  
    Ou, pour personnaliser le type de données mise en correspondance à la base de données, table, vue ou niveau de la procédure stockée, sélectionnez la base de données, une catégorie d’objet ou un objet dans l’Explorateur de métadonnées DB2 :  
  
    1.  Dans l’Explorateur de métadonnées de DB2, sélectionnez le dossier ou un objet à personnaliser.  
  
    2.  Dans le volet droit, cliquez sur le **le mappage de Type** onglet.  
  
2.  Pour ajouter un nouveau mappage, procédez comme suit :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Sous **type de Source de**, sélectionnez le type de données DB2 à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur minimale de données pour le mappage dans le **à partir de** case et la longueur maximale des données dans le **à** boîte.  
  
        Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données.  
  
    4.  Sous **type cible**, sélectionnez la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer** boîte.  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok_md.md)]  
  
3.  Pour modifier un mappage de type de données, procédez comme suit :  
  
    1.  Cliquez sur **Modifier**.  
  
    2.  Sous **type de Source de**, sélectionnez le type de données DB2 à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur minimale de données pour le mappage dans le **à partir de** case et la longueur maximale des données dans le **à** boîte.  
  
        Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données.  
  
    4.  Sous **type cible**, sélectionnez la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer par** zone, puis [!INCLUDE[clickOK](../../includes/clickok_md.md)]  
  
4.  Pour supprimer un mappage de type de données personnalisé, procédez comme suit :  
  
    1.  Sélectionnez la ligne dans la liste de mappage de type qui contient le mappage de type de données à supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
        Impossible de supprimer les mappages hérités. Toutefois, héritée de mappages sont remplacés par des mappages personnalisés sur un objet spécifique ou la catégorie d’objet.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration consiste à [rapport d’évaluation &#40;DB2ToSQL&#41; ](../../ssma/db2/assessment-report-db2tosql.md) ou [conversion de schémas de DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md). Si vous créez un rapport d’évaluation, les objets DB2 sont automatiquement convertis durant l’évaluation.  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données DB2 migration vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
