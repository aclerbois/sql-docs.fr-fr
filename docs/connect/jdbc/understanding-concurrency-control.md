---
title: Présentation de contrôle d’accès concurrentiel | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 98b7dabe-9b12-4e1d-adeb-e5b5cb0c96f3
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 71ca87537cef389ac6cedb47b21a39ce76ba1b97
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-concurrency-control"></a>Fonctionnement du contrôle concurrentiel
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le contrôle concurrentiel fait référence aux différentes techniques utilisées pour conserver l'intégrité de la base de données lorsque plusieurs utilisateurs mettent à jour des lignes en même temps. Une concurrence incorrecte peut entraîner des problèmes tels que des lectures erronées, des lectures fantômes et des lectures non reproductibles. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit des interfaces pour toutes les techniques de concurrence utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour résoudre ces problèmes.  
  
> [!NOTE]  
>  Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] accès simultané, consultez « Gestion aux données » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="remarks"></a>Notes  
 Le pilote JDBC prend en charge les types d'accès simultanés suivants :  
  
|Type d'accès simultané|Caractéristiques|Verrous de lignes| Description|  
|----------------------|---------------------|---------------|-----------------|  
|CONCUR_READ_ONLY|Lecture seule|non|Les mises à jour effectuées à l'aide du curseur ne sont pas autorisées et aucun verrou n'est maintenu sur les lignes constituant le jeu de résultats.|  
|CONCUR_UPDATABLE|Lecture/écriture optimistes|non|La base de données suppose que la contention de ligne est improbable, mais possible. L'intégrité de ligne est vérifiée avec une comparaison d'horodateurs.|  
|CONCUR_SS_SCROLL_LOCKS|Lecture/écriture pessimistes|Oui|La base de données suppose que la contention de ligne est probable. L'intégrité de ligne est garantie avec le verrouillage de ligne.|  
|CONCUR_SS_OPTIMISTIC_CC|Lecture/écriture optimistes|non|La base de données suppose que la contention de ligne est improbable, mais possible. Intégrité de la ligne est vérifiée avec un horodatage.<br /><br /> Pour [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)] et versions ultérieures, le serveur ceci par concur_ss_optimistic_ccval, si la table ne contient pas une colonne timestamp.<br /><br /> Pour [!INCLUDE[ssVersion2000](../../includes/ssversion2000_md.md)], si la table sous-jacente possède une colonne timestamp, OPTIMISTIC WITH ROW VERSIONING est utilisé même si OPTIMISTIC WITH VALUES est spécifié. Si OPTIMISTIC WITH ROW VERSIONING est spécifié et que la table ne possède pas d'horodateurs, OPTIMISTIC WITH VALUES est utilisé.|  
|CONCUR_SS_OPTIMISTIC_CCVAL|Lecture/écriture optimistes|non|La base de données suppose que la contention de ligne est improbable, mais possible. L'intégrité de ligne est vérifiée avec une comparaison des données brutes.|  
  
## <a name="result-sets-that-are-not-updateable"></a>Jeux de résultats qui ne peuvent pas être mis à jour  
 Un jeu de résultats pouvant être mis à jour est un jeu de résultats dans lequel des lignes peuvent être insérées, mises à jour et supprimées. Dans les cas suivants, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ne peut pas créer un curseur modifiable. L'exception générée est « Le jeu de résultat ne peut pas être mis à jour ».  
  
|Cause| Description|Remedy|  
|-----------|-----------------|------------|  
|L'instruction n'est pas créée à l'aide de la syntaxe JDBC 2.0 (ou ultérieure)|JDBC 2.0 introduit de nouvelles méthodes pour créer des instructions. Si la syntaxe JDBC 1.0 est utilisée, le jeu de résultats est par défaut en lecture seule.|Spécifiez le type de jeu de résultats et l'accès simultané lors de la création de l'instruction.|  
|L'instruction est créée à l'aide de TYPE_SCROLL_INSENSITIVE|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Crée un curseur instantané statique. Celui-ci est déconnecté des lignes de table sous-jacentes afin d'aider à protéger le curseur contre les mises à jour de ligne par d'autres utilisateurs.|Utilisez TYPE_SCROLL_SENSITIVE, TYPE_SS_SCROLL_KEYSET, TYPE_SS_SCROLL_DYNAMIC ou TYPE_FORWARD_ONLY avec CONCUR_UPDATABLE pour éviter de créer un curseur statique.|  
|Le mode Création de table n'autorise pas la présence d'un curseur KEYSET ou DYNAMIC|La table sous-jacente n’a pas de clés uniques pour permettre [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour identifier de façon unique une ligne.|Ajoutez des clés uniques à la table afin de fournir une identification unique de chaque ligne.|  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des jeux de résultats avec le pilote JDBC](../../connect/jdbc/managing-result-sets-with-the-jdbc-driver.md)  
  
  
