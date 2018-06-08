---
title: Mappage de Source et les Types de données cible (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
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
- customizing data type mappings
- data types, mapping
- mapping, data types
- source data types
- target data types
ms.assetid: b362a075-16e7-423f-b63f-e1e9f02844a9
caps.latest.revision: 14
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: af10741f0041525b058341f398b86c5e62f7a7e3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-source-and-target-data-types-accesstosql"></a>Source de mappage et les Types de données cible (AccessToSQL)
Les types de base de données Access diffèrent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] types de base de données. Lorsque vous convertissez des objets de base de données Access à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des objets, vous devez spécifier le mappage des types de données à partir de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les procédures suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA a un jeu de mappages de types de données. Pour obtenir la liste des mappages par défaut, consultez [les paramètres de projet (Type de mappage)](http://msdn.microsoft.com/en-us/b87b9683-abed-4677-8c50-18bdba704655).  
  
## <a name="customizing-data-type-mappings"></a>Personnalisation mappages de types de données  
À l’aide de la **les paramètres de projet** boîte de dialogue, vous pouvez personnaliser la façon dont les types sont mappés pour toutes les bases de données et les objets de base de données dans un projet. Les mappages de type pour un projet s’appliquent à toutes les bases de données et les objets de base de données qui n’ont pas de mappages de types personnalisés.  
  
Vous pouvez également personnaliser le mappage des types de données au niveau de la base de données ou une table.  
  
La procédure suivante montre comment mapper des types de données au niveau du projet, une base de données ou un niveau de l’objet de base de données.  
  
**Pour mapper les types de données**  
  
1.  Pour personnaliser le mappage de type de données pour la totalité du projet, ouvrez le **les paramètres de projet** boîte de dialogue :  
  
    1.  Sur le **outils** menu, sélectionnez **les paramètres de projet**.  
  
    2.  Dans le volet gauche, sélectionnez **le mappage de Type**.  
  
        Le graphique de mappage de type et les boutons s’affichent dans le volet droit.  
  
    Ou, pour personnaliser le mappage des types de données au niveau de la base de données ou une table, sélectionnez la base de données ou la table dans le volet Explorateur de métadonnées d’accès :  
  
    1.  Dans le volet Explorateur de métadonnées d’accès, développez **accès à la métabase**, puis développez **bases de données**.  
  
    2.  Sélectionnez la base de données ou la table pour laquelle vous souhaitez personnaliser le mappage de type de données.  
  
    3.  Dans le volet droit, cliquez sur **le mappage de Type**.  
  
2.  Pour ajouter un nouveau mappage, procédez comme suit :  
  
    1.  Dans le volet de mappage de Type, cliquez sur **ajouter**.  
  
    2.  Dans le **nouveau mappage de Type** boîte de dialogue **type de Source de**, sélectionnez le type de données Access à mapper.  
  
    3.  Si le type requiert une longueur, spécifier les longueurs minimale et maximale des données pour le mappage en sélectionnant le **de** et **à** cases à cocher et en entrant les valeurs.  
  
        Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données.  
  
    4.  Sous **type cible**, sélectionnez la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer par** zone, puis cliquez sur **OK**.  
  
3.  Pour modifier un mappage de type de données, procédez comme suit :  
  
    1.  Dans le volet de mappage de Type, cliquez sur **modifier**.  
  
    2.  Dans le **liste de mappage de Type** boîte de dialogue **type de Source de**, sélectionnez le type de données Access à mapper.  
  
    3.  Si le type requiert une longueur, spécifier les longueurs minimale et maximale des données pour le mappage en sélectionnant le **de** et **à** cases à cocher et en entrant les valeurs.  
  
        Cela vous permet de personnaliser le mappage de données pour les valeurs inférieures et supérieures du même type de données.  
  
    4.  Sous **type cible**, sélectionnez la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur de données dans le **remplacer par** zone, puis cliquez sur **OK**.  
  
4.  Pour supprimer un mappage de type de données, procédez comme suit :  
  
    1.  Dans le volet de mappage de Type, sélectionnez la ligne dans la liste de mappage de type qui contient le mappage de type de données à supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration est [convertir des objets de base de données de l’accès aux objets SQL Server](http://msdn.microsoft.com/en-us/e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c)  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
  
