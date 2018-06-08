---
title: Types de données SQL Server et SSIS pris en charge pour les domaines DQS | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2011
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4931143a-b84d-478b-9b45-174128d36ed3
caps.latest.revision: 7
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 37ea6eb8e6d6ee9d10dc90a9a7fa54d6b086d027
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="supported-sql-server-and-ssis-data-types-for-dqs-domains"></a>Types de données SQL Server et SSIS pris en charge pour les domaines DQS

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Il existe de nombreux types de données dans SQL Server et SQL Server Integration Services (SSIS), mais seulement quatre types de données pour les domaines DQS : Date, Décimal, Entier et Chaîne. Les types de données SQL Server et SSIS ne sont pas tous pris en charge dans DQS. Vous ne pouvez mapper vos données source à un domaine DQS en vue d'y effectuer des activités portant sur la qualité des données que si le type de données source est pris en charge dans DQS et qu'il correspond au type de données du domaine DQS. Cette rubrique fournit des informations relatives aux types de données SQL Server et SSIS qui sont pris en charge et disponibles en vue d'un mappage à chacun des quatre types de données de domaine dans DQS.  
  
> [!NOTE]  
>  Dans les fichiers .xlsx et .xls, le type de données de la colonne source est déterminé par le type de données prédominant des huit premières lignes. Si une cellule n'est pas conforme à ce type de données, elle recevra une valeur NULL. De la même manière, dans les fichiers .csv, le type de données de la colonne source est déterminé par le type de données prédominant des huit premières lignes.  
  
##  <a name="SQLServer"></a> Types de données SQL Server pris en charge  
 Le tableau suivant fournit des informations sur les types de données SQL Server pris en charge pour chaque type de données de domaine DQS :  
  
|Type de données de domaine DQS|Type de données SQL Server pris en charge|  
|--------------------------|------------------------------------|  
|Date|Date|  
|Décimal|Décimal<br /><br /> FLOAT<br /><br /> money<br /><br /> NUMERIC<br /><br /> REAL<br /><br /> SMALLMONEY|  
|Entier|BIGINT<br /><br /> INT<br /><br /> smallint<br /><br /> TINYINT|  
|String|char<br /><br /> NCHAR<br /><br /> NVARCHAR<br /><br /> varchar|  
  
 Les autres types de données SQL Server ne sont pas pris en charge dans DQS. Pour plus d’informations sur les types de données SQL Server, consultez [Types de données &#40;Transact-SQL&#41;](../t-sql/data-types/data-types-transact-sql.md).  
  
##  <a name="SSIS"></a> Types de données SSIS pris en charge  
 Le tableau suivant fournit des informations sur les types de données SSIS pris en charge pour chaque type de données de domaine DQS :  
  
|Type de données de domaine DQS|Type de données SSIS pris en charge|  
|--------------------------|------------------------------|  
|Date|DT_DATE|  
|Décimal|DT_DECIMAL<br /><br /> DT_NUMERIC<br /><br /> DT_R4<br /><br /> DT_R8|  
|Entier|DT_I1<br /><br /> DT_I2<br /><br /> DT_I4<br /><br /> DT_I8<br /><br /> DT_U1<br /><br /> DT_U2<br /><br /> DT_U4<br /><br /> DT_U8|  
|String|DT_STR<br /><br /> DT_WSTR|  
  
 Les autres types de données SSIS ne sont pas pris en charge dans DQS Pour plus d'informations sur tous les types de données SSIS, consultez [Integration Services Data Types](../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Gestion d’un domaine](../data-quality-services/managing-a-domain.md)  
  
  
