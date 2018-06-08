---
title: Utilisation des données de grande taille | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 5b93569f-eceb-4f05-b49c-067564cd3c85
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 126775f2b56bdf2cf1847334b0c8faad7cfcfafb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-large-data"></a>Utilisation de données volumineuses
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Le pilote JDBC prend en charge la mise en mémoire tampon adaptative, ce qui vous permet de récupérer tout type de données de grande valeur sans la charge liée au temps de traitement des curseurs côté serveur. Avec la mise en mémoire tampon adaptative, le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] récupère les résultats de l’exécution d’instructions à partir de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] que l’application en a besoin, plutôt que simultanément. Le pilote ignore également les résultats dès que l'application ne peut plus y accéder.  
  
 Dans le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005_md.md)] version 1.2 du pilote JDBC, le mode de mise en mémoire tampon était «**complète**» par défaut. Si votre application n’avez pas défini de la propriété de connexion « responseBuffering » «**adaptive**» dans les propriétés de connexion ou à l’aide de la [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) méthode de la [ SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet, le pilote pris en charge la lecture de l’ensemble de résultats à partir du serveur à la fois. Pour obtenir le comportement de mise en mémoire tampon adaptative, votre application devait définir la propriété de connexion « responseBuffering » «**adaptive**« explicitement.  
  
 Le **adaptive** valeur est la valeur par défaut en mode de mise en mémoire tampon et le pilote JDBC met en mémoire tampon minimale des données lorsque cela est nécessaire. Pour plus d’informations sur l’utilisation de la mise en mémoire tampon adaptative, consultez [à l’aide de mise en mémoire tampon adaptative](../../../connect/jdbc/using-adaptive-buffering.md).  
  
 Les rubriques de cette section décrivent les différentes façons dont vous pouvez utiliser pour récupérer des données de grande valeur d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] base de données.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Exemple de lecture de données volumineuses](../../../connect/jdbc/reading-large-data-sample.md)|Explique comment utiliser une instruction SQL pour récupérer des données de grande valeur.|  
|[Exemple de lecture de données volumineuses avec des procédures stockées](../../../connect/jdbc/reading-large-data-with-stored-procedures-sample.md)|Explique comment récupérer une valeur de paramètre OUT CallableStatement de grande taille.|  
|[Exemple de mise à jour de données volumineuses](../../../connect/jdbc/updating-large-data-sample.md)|Explique comment mettre à jour des données de grande valeur dans une base de données.|  
  
## <a name="see-also"></a>Voir aussi  
 [Exemples d’applications JDBC Driver](../../../connect/jdbc/sample-jdbc-driver-applications.md)  
  
  
