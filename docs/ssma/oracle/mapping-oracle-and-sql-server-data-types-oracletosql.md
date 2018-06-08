---
title: Mappage d’Oracle et les Types de données SQL Server (OracleToSQL) | Documents Microsoft
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
- Type Mapping Inheritance
ms.assetid: 05da1495-63b9-47b7-86e2-6746394a2d8a
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 4b17ff13e10f0ec77a8d35e1f051960d6d32a25c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-oracle-and-sql-server-data-types-oracletosql"></a>Mappage d’Oracle et les Types de données SQL Server (OracleToSQL)
Les types de base de données Oracle diffèrent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] types de base de données. Lorsque vous convertissez des objets de base de données Oracle à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des objets, vous devez spécifier le mappage des types de données d’Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les sections suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA a un jeu de mappages de types de données. Pour obtenir la liste des mappages par défaut, consultez [les paramètres de projet &#40;le mappage de Type&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md).  
  
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
  
    Ou, pour personnaliser le type de données mise en correspondance à la base de données, table, vue ou niveau de la procédure stockée, sélectionnez la base de données, une catégorie d’objet ou un objet dans l’Explorateur de métadonnées Oracle :  
  
    1.  Dans l’Explorateur de métadonnées d’Oracle, sélectionnez le dossier ou un objet à personnaliser.  
  
    2.  Dans le volet droit, cliquez sur le **le mappage de Type** onglet.  
  
2.  Pour ajouter un nouveau mappage, procédez comme suit :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Sous **type de Source de**, sélectionnez le type de données Oracle à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur minimale de données pour le mappage dans le **à partir de** case et la longueur maximale des données dans le **à** boîte.  
  
        Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données.  
  
    4.  Sous **type cible**, sélectionnez la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer** boîte.  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok_md.md)]  
  
3.  Pour modifier un mappage de type de données, procédez comme suit :  
  
    1.  Cliquez sur **Modifier**.  
  
    2.  Sous **type de Source de**, sélectionnez le type de données Oracle à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur minimale de données pour le mappage dans le **à partir de** case et la longueur maximale des données dans le **à** boîte.  
  
        Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données.  
  
    4.  Sous **type cible**, sélectionnez la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer par** zone, puis [!INCLUDE[clickOK](../../includes/clickok_md.md)]  
  
4.  Pour supprimer un mappage de type de données personnalisé, procédez comme suit :  
  
    1.  Sélectionnez la ligne dans la liste de mappage de type qui contient le mappage de type de données à supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
        Impossible de supprimer les mappages hérités. Toutefois, héritée de mappages sont remplacés par des mappages personnalisés sur un objet spécifique ou la catégorie d’objet.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration consiste à [créer un rapport d’évaluation](http://msdn.microsoft.com/en-us/4de9bcf6-1346-4740-87f9-7f24a8226357) ou [convertir les objets de base de données Oracle en syntaxe SQL](http://msdn.microsoft.com/en-us/e021182d-31da-443d-b110-937f5db27272). Si vous créez un rapport d’évaluation, les objets Oracle sont automatiquement convertis durant l’évaluation.  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données de migration d’Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
