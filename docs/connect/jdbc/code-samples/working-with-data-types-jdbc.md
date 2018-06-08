---
title: Utilisation des Types de données (JDBC) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b39f44d0-3710-4bc6-880c-35bd8c10a734
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6ccecd482516979a2f3ace2a4ce2c039af3ad1c4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-data-types-jdbc"></a>Travail sur des types de données (JDBC)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  La fonction principale de la [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] est de permettre aux développeurs Java d’accéder aux données contenues dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] bases de données. Pour ce faire, le pilote JDBC modère la conversion entre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] des types de données et les types de langage Java et les objets.  
  
> [!NOTE]  
>  Pour une présentation détaillée de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] et types de données de pilote JDBC, y compris leurs différences et comment ils sont convertis en types de données du langage Java, consultez [présentation des Types de données du pilote JDBC](../../../connect/jdbc/understanding-the-jdbc-driver-data-types.md).  
  
 Afin de travailler avec les types de données SQL Server, le pilote JDBC fournit get\<Type > et définissez\<Type > méthodes pour le [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) et [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) get fournit des classes et il\<Type > et mettre à jour\<Type > méthodes pour le [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) classe. La méthode que vous utilisez dépend du type de données avec lequel vous travaillez et si vous utilisez des jeux de résultats ou des requêtes.  
  
 Les rubriques de cette section décrivent comment utiliser les types de données du pilote JDBC pour accéder à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] les données dans vos applications Java.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Exemple de types de données de base](../../../connect/jdbc/basic-data-types-sample.md)|Décrit comment utiliser les méthodes getter du jeu de résultats pour récupérer la base [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] valeurs et comment utiliser des méthodes de mise à jour de jeu de résultats pour mettre à jour ces valeurs de type de données.|  
|[Exemple de type de données SQLXML](../../../connect/jdbc/sqlxml-data-type-sample.md)|Décrit comment stocker des données XML dans une base de données relationnelle, comment récupérer des données XML à partir d’une base de données et comment analyser des données XML avec le **SQLXML** type de données Java.|  
  
## <a name="see-also"></a>Voir aussi  
 [Exemples d’applications JDBC Driver](../../../connect/jdbc/sample-jdbc-driver-applications.md)  
  
  
