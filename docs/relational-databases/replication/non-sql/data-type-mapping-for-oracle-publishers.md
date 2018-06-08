---
title: Mappage de type de données pour les serveurs de publication Oracle | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Oracle publishing [SQL Server replication], data type mapping
- data types [SQL Server replication], Oracle publishing
- mapping data types [SQL Server replication]
ms.assetid: 6da0e4f4-f252-4b7e-ba60-d2e912aa278e
caps.latest.revision: 47
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 32889f88a4e055800c02286e819b9ce1f7422175
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-mapping-for-oracle-publishers"></a>Mappage de type de données pour les serveurs de publication Oracle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les types de données Oracle et [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne correspondent pas toujours exactement. Dans la mesure du possible, le type de données correspondant est sélectionné automatiquement lors de la publication d'une table Oracle. Dans les cas où un mappage de type de données simple n'est pas clair, des mappages de type de données de remplacement sont fournis. Pour plus d'informations sur la sélection de mappages de remplacement, consultez la section « Spécification de mappages de type de données de remplacement » plus loin dans cette rubrique.  
  
 Le tableau suivant indique comment les types de données sont mappés par défaut entre Oracle et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lorsque les données sont déplacées du serveur de publication Oracle sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La colonne Alternatives indique si des mappages de remplacement sont disponibles.  
  
|Type de données Oracle|Type de données SQL Server|Alternatives|  
|----------------------|--------------------------|------------------|  
|BFILE|VARBINARY(MAX)|Oui|  
|BLOB|VARBINARY(MAX)|Oui|  
|CHAR([1-2000])|CHAR([1-2000])|Oui|  
|CLOB|VARCHAR(MAX)|Oui|  
|DATE|DATETIME|Oui|  
|FLOAT|FLOAT|non|  
|FLOAT([1-53])|FLOAT([1-53])|non|  
|FLOAT([54-126])|FLOAT|non|  
|INT|NUMERIC(38)|Oui|  
|INTERVAL|DATETIME|Oui|  
|LONG|VARCHAR(MAX)|Oui|  
|LONG RAW|IMAGE|Oui|  
|NCHAR([1-1000])|NCHAR([1-1000])|non|  
|NCLOB|NVARCHAR(MAX)|Oui|  
|NUMBER|FLOAT|Oui|  
|NUMBER([1-38])|NUMERIC([1-38])|non|  
|NUMBER([0-38],[1-38])|NUMERIC([0-38],[1-38])|Oui|  
|NVARCHAR2([1-2000])|NVARCHAR([1-2000])|non|  
|RAW([1-2000])|VARBINARY([1-2000])|non|  
|real|FLOAT|non|  
|ROWID|CHAR(18)|non|  
|TIMESTAMP|DATETIME|Oui|  
|TIMESTAMP(0-7)|DATETIME|Oui|  
|TIMESTAMP(8-9)|DATETIME|Oui|  
|TIMESTAMP(0-7) WITH TIME ZONE|VARCHAR(37)|Oui|  
|TIMESTAMP(8-9) WITH TIME ZONE|VARCHAR(37)|non|  
|TIMESTAMP(0-7) WITH LOCAL TIME ZONE|VARCHAR(37)|Oui|  
|TIMESTAMP(8-9) WITH LOCAL TIME ZONE|VARCHAR(37)|non|  
|UROWID|CHAR(18)|non|  
|VARCHAR2([1-4000])|VARCHAR([1-4000])|Oui|  
  
## <a name="considerations-for-data-type-mapping"></a>Règles de mappage des types de données  
 Tenez compte des problèmes de type de données suivants lors de la réplication de données d'une base de données Oracle.  
  
### <a name="unsupported-data-types"></a>Types de données non pris en charge  
 Les types de données suivants ne sont pas pris en charge ; les colonnes de ces types ne peuvent pas être répliquées :  
  
-   Types d'objets  
  
-   Types XML  
  
-   Varrays  
  
-   Tables imbriquées  
  
-   Colonnes utilisant REF  
  
### <a name="the-date-data-type"></a>Le type de données DATE  
 Les [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les dates vont de 1753 apr. J.C. à 9999 apr. J.C., alors que dans Oracle, elles vont de 4712 av. J.C. à 4712 apr. J.C. Si une colonne de type DATE contient des valeurs qui sont en dehors de la page de dates de SQL Server, sélectionnez l'autre type de données pour la colonne, qui est VARCHAR(19).  
  
### <a name="float-and-number-types"></a>Types FLOAT et NUMBER  
 L'échelle et la précision spécifiées lors du mappage des types de données FLOAT et NUMBER dépendent de l'échelle et de la précision spécifiées pour la colonne utilisant le type de données dans la base de données Oracle. La précision est le nombre de chiffres qui composent un nombre. L'échelle est le nombre de chiffres à droite du séparateur décimal dans un nombre. Par exemple, le nombre 123,45 a une précision de 5 et une échelle de 2.  
  
 Oracle permet de définir des nombres dont l'échelle est supérieure à la précision, par exemple NUMBER(4,5), mais [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nécessite une précision supérieure ou égale à l'échelle. Afin d'éviter les troncations de données, si l'échelle est supérieure à la précision sur le serveur de publication Oracle, la précision est définie comme étant égale à l'échelle lorsque le type de données est mappé : NUMBER(4,5) sera mappé comme NUMERIC(5,5).  
  
> [!NOTE]  
>  Si vous n'indiquez pas d'échelle ni de précision pour NUMBER, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise par défaut les valeurs maximales d'échelle (8) et de précision (38). Nous vous recommandons de définir une échelle et une précision spécifiques dans Oracle, afin d'optimiser le stockage et les performances lorsque les données sont répliquées.  
  
### <a name="large-object-types"></a>Types d'objets volumineux  
 Oracle prend en charge jusqu'à 4 gigaoctets (Go), alors que SQL Server prend en charge jusqu'à 2 Go. Les données répliquées au-delà de 2 Go sont tronquées.  
  
 Si une table Oracle comporte une colonne BFILE, les données de la colonne sont stockées dans le système de fichiers. Le compte d'utilisateur d'administration de réplication doit recevoir le droit d'accès au répertoire où les données sont stockées, via la syntaxe suivante :  
  
 `GRANT READ ON DIRECTORY <directory_name> TO <replication_administrative_user_schema>`  
  
 Pour plus d’informations sur les types d’objets volumineux, consultez la section « Observations sur les objets volumineux » dans la rubrique [Problèmes et limitations de conception des serveurs de publication Oracle](../../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md).  
  
## <a name="specifying-alternative-data-type-mappings"></a>Spécification de mappages de type de données de remplacement  
 En général, le mappage de type de données par défaut est approprié, mais pour un certain nombre de types de données Oracle, vous pouvez sélectionner un mappage de type de données à partir d'un ensemble de mappages de remplacement au lieu d'utiliser les mappages par défaut. Il existe deux façons de spécifier des mappages de remplacement :  
  
-   Remplacer la valeur par défaut par article à l'aide de procédures stockées ou de l'Assistant Nouvelle publication.  
  
-   Remplacer en totalité toutes les valeurs par défaut pour les prochains articles à l'aide de procédures stockées (les valeurs par défaut restent inchangées pour les articles existants).  
  
 Pour spécifier des mappages de types de données de remplacement, consultez [Specify Data Type Mappings for an Oracle Publisher](../../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [Problèmes et limitations de conception des serveurs de publication Oracle](../../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md)   
 [Vue d’ensemble de la publication Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)  
  
  
