---
title: Implémentation de la compression de ligne | Microsoft Docs
ms.custom: ''
ms.date: 06/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- compression [SQL Server], row
- row compression [Database Engine]
ms.assetid: dcd97ac1-1c85-4142-9594-9182e62f6832
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 74e3b96d96fa0906e92b3aa6df5f10fcdd248847
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="row-compression-implementation"></a>Implémentation de la compression de ligne
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Cette rubrique récapitule la manière dont le [!INCLUDE[ssDE](../../includes/ssde-md.md)] implémente la compression de ligne. Elle fournit des informations de base qui vous aideront à planifier l'espace de stockage dont vous avez besoin pour vos données.  
  
 L'activation de la compression modifie uniquement le format de stockage physique des données qui sont associées à un type de données, mais pas leur syntaxe ni leur sémantique. Aucune modification de l'application n'est requise lorsqu'une ou plusieurs tables sont activées pour la compression. Le nouveau format de stockage des enregistrements propose les améliorations suivantes :  
  
-   Il réduit la charge mémoire des métadonnées qui est associée à l'enregistrement. Ces métadonnées sont des informations sur les colonnes, leur longueur et leur décalage. Dans certains cas, la charge mémoire des métadonnées peut être plus importante qu'avec l'ancien format de stockage.  
  
-   Il utilise le format de stockage de longueur variable pour les types de données numériques (par exemple **integer**, **decimal**et **float**) et les types reposant sur le type numérique (par exemple **datetime** et **money**).  
  
-   Il stocke les chaînes de caractères de longueur fixe en utilisant le format de longueur variable, mais sans stocker les caractères blancs.  
  
> [!NOTE]  
>  Les valeurs NULL et 0 de tous les types de données sont optimisées et n'occupent aucun octet.  
  
## <a name="how-row-compression-affects-storage"></a>Conséquences de la compression de ligne sur le stockage  
 Le tableau suivant décrit comment la compression de ligne affecte les types existants dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)]. Il n'indique pas les économies qui peuvent être réalisées en utilisant la compression de page.  
  
|Type de données|Le stockage est-il affecté ?|Description|  
|---------------|--------------------------|-----------------|  
|**tinyint**|non|1 octet est la quantité de stockage minimale nécessaire.|  
|**smallint**|Oui|Si la valeur rentre dans 1 octet, 1 seul octet est utilisé.|  
|**Int**|Oui|Utilise uniquement les octets nécessaires. Par exemple, si une valeur peut être stockée dans 1 octet, le stockage n'occupe qu'un seul octet.|  
|**bigint**|Oui|Utilise uniquement les octets nécessaires. Par exemple, si une valeur peut être stockée dans 1 octet, le stockage n'occupe qu'un seul octet.|  
|**decimal**|Oui|Ce stockage est exactement le même que le format de stockage vardecimal.|  
|**numeric**|Oui|Ce stockage est exactement le même que le format de stockage vardecimal.|  
|**bit**|Oui|La charge mémoire des métadonnées porte cette valeur à 4 bits.|  
|**smallmoney**|Oui|Utilise la représentation des données de type entier sur un entier de 4 octets. La valeur monétaire est multipliée par 10000 et la valeur entière résultante est stockée en supprimant tous les chiffres après la virgule. L'optimisation du stockage de ce type de données est semblable à celle des types de données entiers.|  
|**money**|Oui|Utilise la représentation des données de type entier sur un entier de 8 octets. La valeur monétaire est multipliée par 10000 et la valeur entière résultante est stockée en supprimant tous les chiffres après la virgule. Ce type a une plus grande plage que **smallmoney**. L'optimisation du stockage de ce type de données est semblable à celle des types de données entiers.|  
|**float**|Oui|Les octets les moins significatifs avec des zéros ne sont pas stockés. La compression**float** est principalement applicable aux valeurs non fractionnaires de la mantisse.|  
|**real**|Oui|Les octets les moins significatifs avec des zéros ne sont pas stockés. La compression**real** est principalement applicable aux valeurs non fractionnaires de la mantisse.|  
|**smalldatetime**|non|Utilise la représentation des données de type entier sur deux entiers de 2 octets. La date occupe 2 octets. Il s'agit du nombre de jours depuis le 1/1/1901. À partir de 1902, deux octets sont nécessaires. Par conséquent, aucune économie n'est réalisée après cette date.<br /><br /> L'heure est le nombre de minutes depuis minuit. Les valeurs d'heure situées légèrement après 16h00 commencent à utiliser le deuxième octet.<br /><br /> Si un **smalldatetime** est utilisé uniquement pour représenter une date (ce qui est souvent le cas), l’heure est 0.0. La compression permet d'économiser 2 octets en stockant l'heure dans le format d'octet le plus significatif pour la compression de ligne.|  
|**datetime**|Oui|Utilise la représentation des données de type entier sur deux entiers de 4 octets. La valeur entière représente le nombre de jours depuis la date de base du 1/1/1900. Les 2 premiers octets peuvent représenter jusqu'à l'année 2079. La compression permet toujours d'économiser 2 octets jusqu'à cette date. Chaque valeur entière représente 3,33 millisecondes. La compression épuise les 2 premiers octets dans les cinq premières minutes et a besoin du quatrième octet après 16h00. La compression permet donc d'économiser uniquement 1 octet après 16h00. Lorsque **datetime** est compressé comme tout autre entier, la compression permet d'économiser 2 octets dans la date.|  
|**date**|non|Utilise la représentation des données de type entier sur 3 octets. Représente la date à compter du 1/1/0001. Pour les dates contemporaines, la compression de ligne utilise les 3 octets. Aucune économie n'est réalisée.|  
|**time**|non|Utilise la représentation des données de type entier sur 3 à 6 octets. Plusieurs précisions démarrent par un chiffre compris entre 0 et 9 et peuvent occuper entre 3 et 6 octets. L'espace compressé est utilisé comme suit :<br /><br /> **Précision = 0. Octets = 3**. Chaque valeur entière représente une seconde. La compression peut représenter l'heure jusqu'à 6h00 sur 2 octets, ce qui permet d'économiser potentiellement 1 octet.<br /><br /> **Précision = 1. Octets = 3**. Chaque valeur entière représente 1/10 seconde. La compression utilise le troisième octet avant 2h00. De petites économies sont ainsi réalisées.<br /><br /> **Précision = 2. Octets = 3**. Semblable au cas précédent, la réalisation d'économies est peu probable.<br /><br /> **Précision = 3. Octets = 4**. Dans la mesure où les 3 premiers octets sont occupés dès 5h00, peu d'économies sont réalisées.<br /><br /> **Précision = 4. Octets = 4**. Les trois premiers octets sont occupés dans les 27 premières secondes. Aucune économie n'est attendue.<br /><br /> **Précision = 5, Octets = 5**. Le cinquième octet sera utilisé après midi.<br /><br /> **Précision = 6 et 7, Octets = 5**. Ne réalise pas d'économies.<br /><br /> **Précision = 8, Octets = 6**. Le sixième octet sera utilisé après 3h00.<br /><br /> <br /><br /> Notez l’absence de modification dans le stockage pour la compression de ligne. Globalement, peu d'économies peuvent être attendues de la compression du type de données **time** .|  
|**datetime2**|Oui|Utilise la représentation des données de type entier sur 6 à 9 octets. Les 4 premiers octets représentent la date. Les octets occupés par la date dépendent de la précision de l'heure spécifiée.<br /><br /> La valeur entière représente le nombre de jours depuis le 1/1/0001, avec une date limite située au 31/31/9999. La représentation d'une date de l'année 2005 occupe 3 octets.<br /><br /> Aucune économie n'est réalisée sur les heures car 2 à 4 octets sont nécessaires pour différentes précisions d'heure. Par conséquent, pour une précision de l'heure à la seconde, la compression utilise 2 octets pour l'heure et occupe le deuxième octet après 255 secondes.|  
|**datetimeoffset**|Oui|Similaire à **datetime2**, mais avec 2 octets de fuseau horaire au format (HH:MM).<br /><br /> Comme **datetime2**, la compression peut permettre d'économiser 2 octets.<br /><br /> Pour les valeurs de fuseau horaire, la valeur MM peut être 0 dans la plupart des cas. Par conséquent, la compression peut permettre d'économiser 1 octet.<br /><br /> Il n'y a aucune modification dans le stockage pour la compression de ligne.|  
|**char**|Oui|Les caractères de remplissage à droite sont supprimés. Notez que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] insère le même caractère de remplissage quel que soit le classement utilisé.|  
|**varchar**|non|Aucun effet.|  
|**texte**|non|Aucun effet.|  
|**nchar**|Oui|Les caractères de remplissage à droite sont supprimés. Notez que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] insère le même caractère de remplissage quel que soit le classement utilisé.|  
|**nvarchar**|non|Aucun effet.|  
|**ntext**|non|Aucun effet.|  
|**binaire**|Oui|Les zéros de droite sont supprimés.|  
|**varbinary**|non|Aucun effet.|  
|**image**|non|Aucun effet.|  
|**cursor**|non|Aucun effet.|  
|**timestamp** / **rowversion**|Oui|Utilise la représentation des données de type entier sur 8 octets. Un compteur d'horodatage, dont la valeur commence à 0, est maintenu pour chaque base de données. Il peut être compressé comme toute autre valeur entière.|  
|**sql_variant**|non|Aucun effet.|  
|**uniqueidentifier**|non|Aucun effet.|  
|**table**|non|Aucun effet.|  
|**xml**|non|Aucun effet.|  
|Types définis par l'utilisateur|non|Représentés en interne sous la forme **varbinary**.|  
|FILESTREAM|non|Représentés en interne sous la forme **varbinary**.|  
  
## <a name="see-also"></a> Voir aussi  
 [Compression de données](../../relational-databases/data-compression/data-compression.md)   
 [Implémentation de la compression de page](../../relational-databases/data-compression/page-compression-implementation.md)  
  
  
