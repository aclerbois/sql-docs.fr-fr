---
title: Utilisation des jeux de résultats | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4fc4b1c6-3075-4ad7-9244-865d9ede7ae6
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f1802a6a51773ce552bcd4fcc45745a29e2405db
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-result-sets"></a>Travail sur des jeux de résultats
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Lorsque vous travaillez avec les données contenues dans un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données, une méthode de manipulation des données consiste à utiliser un jeu de résultats. Le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] prend en charge l’utilisation du résultat de jeux via la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet. À l’aide de l’objet SQLServerResultSet, vous pouvez extraire les données retournées à partir d’une instruction SQL ou une procédure stockée, mettre à jour les données en fonction des besoins, puis remettre ces données dans la base de données.  
  
 En outre, l’objet SQLServerResultSet fournit des méthodes de navigation dans ses lignes de données, d’obtention ou de définition des données qu’il contient et pour l’établissement de plusieurs niveaux de sensibilité aux modifications dans la base de données sous-jacente.  
  
> [!NOTE]  
>  Pour plus d’informations sur la gestion des jeux de résultats, y compris leur sensibilité aux modifications, consultez [la gestion des jeux de résultats avec le pilote JDBC](../../../connect/jdbc/managing-result-sets-with-the-jdbc-driver.md).  
  
 Les rubriques de cette section décrivent les différentes méthodes que vous pouvez utiliser un jeu de résultats pour manipuler les données contenues dans un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Exemple de récupération de données du jeu de résultats](../../../connect/jdbc/retrieving-result-set-data-sample.md)|Décrit comment utiliser un jeu de résultats pour récupérer des données d’une [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] de base de données et les afficher.|  
|[Exemple de modification des données du jeu de résultats](../../../connect/jdbc/modifying-result-set-data-sample.md)|Décrit comment utiliser un jeu de résultats pour insérer, extraire et modifier des données dans un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données.|  
|[Exemple de mise en cache de données du jeu de résultats](../../../connect/jdbc/caching-result-set-data-sample.md)|Décrit comment utiliser un jeu de résultats à extraire de grandes quantités de données d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données et pour contrôler la façon dont ces données sont mis en cache sur le client.|  
  
## <a name="see-also"></a>Voir aussi  
 [Exemples d’applications JDBC Driver](../../../connect/jdbc/sample-jdbc-driver-applications.md)  
  
  
