---
title: À l’aide des métadonnées de paramètre | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: db2c1957-91c6-4989-a07b-9f8be6d2033a
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c2ff7d2646c0a3838b8a0dc72249b73e64227dd7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-parameter-metadata"></a>Utilisation des métadonnées de paramètre
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Pour interroger un [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) ou un [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) objet sur les paramètres qu’ils contiennent, la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] implémente la [ SQLServerParameterMetaData](../../connect/jdbc/reference/sqlserverparametermetadata-class.md) classe. Cette classe contient plusieurs champs et méthodes qui retournent des informations sous la forme d'une valeur unique.  
  
 Pour créer un objet SQLServerParameterMetaData, vous pouvez utiliser la [getParameterMetaData](../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md) méthodes des classes SQLServerPreparedStatement et SQLServerCallableStatement.  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données est transmise à la fonction, la méthode getParameterMetaData de la classe SQLServerCallableStatement est utilisée pour retourner un objet SQLServerParameterMetaData et divers méthodes de l’objet SQLServerParameterMetaData sont utilisées pour afficher des informations sur le type et le mode des paramètres qui sont contenus dans la procédure stockée HumanResources.uspUpdateEmployeeHireInfo.  
  
 [!code[JDBC#UsingParamMetaData1](../../connect/jdbc/codesnippet/Java/using-parameter-metadata_1.java)]  
    
> [!NOTE]  
Il existe certaines limitations lors de l’utilisation de la classe SQLServerParameterMetaData avec des instructions préparées. 
**Avec le pilote Microsoft JDBC 6.0 (ou version ultérieure) pour SQL Server**: lorsque vous utilisez SQL Server 2008 ou 2008 R2, le pilote JDBC prend en charge les instructions SELECT, DELETE, INSERT et UPDATE tant que ces instructions ne contient pas de sous-requêtes et/ou de jointures.  

FUSIONNER des requêtes sont également pas pris en charge pour la classe SQLServerParameterMetaData lors de l’utilisation de SQL Server 2008 ou 2008 R2. Pour SQL Server 2012 et versions ultérieures, les métadonnées de paramètres avec des requêtes complexes sont prises en charge.  

La récupération des métadonnées de paramètre pour les colonnes chiffrées ne sont pas pris en charge. **Avec le pilote Microsoft JDBC 4.1 ou 4.2 pour SQL Server**: pilote JDBC le prend en charge les instructions SELECT, DELETE, INSERT et UPDATE tant que ces instructions ne contient pas de sous-requêtes et/ou de jointures. FUSIONNER des requêtes ne sont pas également pris en charge pour la classe SQLServerParameterMetaData.  
  
  
