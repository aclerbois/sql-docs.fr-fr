---
title: Types de données d’Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- modifying data types
- data types [Integration Services], listed
- data types [Integration Services]
- column data types [Integration Services]
- SSIS, data types
- Integration Services, data types
- SQL Server Integration Services, data types
ms.assetid: 896fc3e8-3aa6-4396-ba82-5d7741cffa56
caps.latest.revision: 98
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fce885002fc8dd2870480327ed575b77332ed0d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-data-types"></a>Types de données d'Integration Services
  Quand des données entrent dans un flux de données dans un package, la source qui extrait les données les convertit en type [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Les données numériques se voient attribuer le type de données numeric, les données chaînes le type de données character et les dates le type de données date. Le type de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] approprié est également affecté aux autres données, comme les GUID et les BLOB (Binary Large Object Blocks). Si le type des données ne peut pas être converti en un type de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , une erreur se produit.  
  
 Certains composants de flux de données convertissent les types de données entre les types de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et les types de données managées de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Pour plus d’informations sur le mappage entre [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et les types de données managées, consultez [Utilisation de types de données dans le flux de données](../../integration-services/extending-packages-custom-objects/data-flow/working-with-data-types-in-the-data-flow.md).  
  
 Le tableau suivant énumère les types de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Des informations de précision et d'échelle s'appliquent à certains types de données du tableau. Pour plus d’informations sur la précision et l’échelle, consultez [Précision, échelle et longueur &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
|Type de données|Description|  
|---------------|-----------------|  
|DT_BOOL|Valeur booléenne.|  
|DT_BYTES|Valeur de données binaires. La longueur est variable et ne peut pas dépasser 8 000 octets.|  
|DT_CY|Valeur de devise. Ce type de données est un entier signé de 8 octets avec une échelle de 4 et une précision maximale de 19 chiffres.|  
|DT_DATE|Structure de date comprenant l'année, le mois, le jour, les heures, les minutes, les secondes et les fractions de seconde.  Les fractions de seconde ont une échelle fixe de 7 chiffres.<br /><br /> Le type de données DT_DATE est implémenté à l'aide d'un nombre à virgule flottante à 8 octets. Les jours sont représentés par des incréments de nombres entiers, commençant le 30 décembre 1899, minuit correspondant à l'heure zéro. Les valeurs d'heure sont exprimées sous la forme de la valeur absolue de la partie fractionnaire du nombre. Cependant, une valeur à virgule flottante ne peut pas représenter toutes les valeurs réelles ; des restrictions sont par conséquent imposées quant à la plage des dates pouvant être représentées dans DT_DATE.<br /><br /> D'autre part, DT_DBTIMESTAMP est représenté par une structure comportant en interne des champs individuels pour l'année, le mois, le jour, les heures, les minutes, les secondes et les millisecondes. Ce type de données a des limites plus étendues quant aux plages de dates qu'il peut représenter.|  
|DT_DBDATE|Structure de date comprenant l'année, le mois et le jour.|  
|DT_DBTIME|Structure d'heure comprenant les heures, les minutes et les secondes.|  
|DT_DBTIME2|Structure d'heure comprenant les heures, les minutes, les secondes et les fractions de seconde. Les fractions de seconde ont une échelle maximale de 7 chiffres.|  
|DT_DBTIMESTAMP|Structure d'horodateur comprenant l'année, le mois, le jour, les heures, les minutes, les secondes et les fractions de seconde. Les fractions de seconde ont une échelle maximale de 3 chiffres.|  
|DT_DBTIMESTAMP2|Structure d'horodateur comprenant l'année, le mois, le jour, les heures, les minutes, les secondes et les fractions de seconde. Les fractions de seconde ont une échelle maximale de 7 chiffres.|  
|DT_DBTIMESTAMPOFFSET|Structure d'horodateur comprenant l'année, le mois, le jour, les heures, les minutes, les secondes et les fractions de seconde. Les fractions de seconde ont une échelle maximale de 7 chiffres.<br /><br /> Contrairement aux types de données DT_DBTIMESTAMP et DT_DBTIMESTAMP2, le type de données DT_DBTIMESTAMPOFFSET a un décalage de fuseau horaire. Ce décalage spécifie le nombre d'heures et de minutes de décalage de l'heure par rapport au temps universel coordonné (UTC). Le décalage de fuseau horaire est utilisé par le système pour obtenir l'heure locale.<br /><br /> Le décalage de fuseau horaire doit inclure un signe (plus ou moins) pour indiquer si le décalage est ajouté au temps universel coordonné ou en est soustrait. Le nombre valide de décalage d'heures est compris entre -14 et +14. Le signe du décalage de minutes dépend du signe du décalage d'heures :<br /><br /> Si le signe du décalage d'heures est négatif, le décalage de minutes doit être négatif ou zéro.<br /><br /> Si le signe du décalage d'heures est positif, le décalage de minutes doit être positif ou zéro.<br /><br /> Si le signe du décalage d'heures est zéro, le décalage de minutes peut être n'importe quelle valeur comprise entre -0,59 et + 0,59.|  
|DT_DECIMAL|Valeur numérique exacte avec une précision et une échelle fixes. Ce type de données est un entier non signé de 12 octets avec un signe séparé, une échelle comprise entre 0 et 28 et une précision maximale de 29.|  
|DT_FILETIME|Valeur 64 bits représentant le nombre d'intervalles de 100 nanosecondes depuis le 1er janvier 1601. Les fractions de seconde ont une échelle maximale de 3 chiffres.|  
|DT_GUID|Identificateur global unique (GUID).|  
|DT_I1|Entier signé de 1 octet.|  
|DT_I2|Entier signé de 2 octets.|  
|DT_I4|Entier signé de 4 octets.|  
|DT_I8|Entier signé de 8 octets.|  
|DT_NUMERIC|Valeur numérique exacte avec une précision et une échelle fixes. Ce type de données est un entier non signé de 16 octets avec un signe séparé, une échelle comprise entre 0 et 38 et une précision maximale de 38.|  
|DT_R4|Valeur en virgule flottante simple précision.|  
|DT_R8|Valeur en virgule flottante double précision.|  
|DT_STR|Chaîne de caractères [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]/MBCS se terminant par une valeur Null, d’une longueur maximale de 8 000 caractères. (Si une valeur de colonne contient des indicateurs de fin Null, la chaîne apparaît tronquée dès la première valeur Null.)|  
|DT_UI1|Entier non signé de 1 octet.|  
|DT_UI2|Entier non signé de 2 octets.|  
|DT_UI4|Entier non signé de 4 octets.|  
|DT_UI8|Entier non signé de 8 octets.|  
|DT_WSTR|Chaîne de caractères Unicode se terminant par une valeur Null avec une longueur maximale de 4 000 caractères. (Si une valeur de colonne contient des indicateurs de fin Null, la chaîne apparaît tronquée dès la première valeur Null.)|  
|DT_IMAGE|Valeur binaire avec une taille maximale de 2^31-1 (2 147 483 647) octets. .|  
|DT_NTEXT|Chaîne de caractères Unicode avec une longueur maximale de 2^30-1 (1 073 741 823) caractères.|  
|DT_TEXT|Chaîne de caractères [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]/MBCS d’une longueur maximale de 2^31-1 (2 147 483 647) caractères.|  
  
## <a name="conversion-of-data-types"></a>Conversion de types de données  
 Si les données d'une colonne n'ont pas besoin de toute la largeur qui leur est allouée par le type de données source, vous voudrez peut-être changer le type de données de la colonne. La réduction de la longueur de chaque ligne de données permet d'optimiser les performances lors du transfert de données car plus la ligne est courte, plus les données sont transférées rapidement de la source vers la destination.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] propose un jeu complet de types de données numeric afin que vous puissiez faire correspondre le type de données et la taille des données. Par exemple, si les valeurs d'une colonne dont le type de données est DT_UI8 sont toujours des entiers compris entre 0 et 3000, vous pouvez opter pour le type de données DT_UI2. De même, si une colonne dont le type de données est DT_CY pourrait se satisfaire d'un type de données integer aux vues des données du package, vous pouvez opter pour le type de données DT_I4.  
  
 Vous pouvez modifier les types de données d'une colonne de l'une des manières suivantes :  
  
-   Utilisez une expression pour convertir des types de données de façon implicite. Pour plus d’informations, consultez [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md), [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md) et [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md).  
  
-   Utilisez l'opérateur de conversion pour convertir des types de données. Pour plus d’informations, consultez [Cast &#40;expression SSIS&#41;](../../integration-services/expressions/cast-ssis-expression.md).  
  
-   Utilisez la transformation de conversion de données pour convertir le type de données d'une colonne en un type différent. Pour plus d’informations, voir [Data Conversion Transformation](../../integration-services/data-flow/transformations/data-conversion-transformation.md).  
  
-   Utilisez la transformation de colonne dérivée pour créer une copie d'une colonne ayant un type de données différent de celui de la colonne d'origine. Pour plus d’informations, voir [Derived Column Transformation](../../integration-services/data-flow/transformations/derived-column-transformation.md).  
  
### <a name="converting-between-strings-and-datetime-data-types"></a>Conversion entre les chaînes et les types de données de date et d'heure  
 Le tableau suivant répertorie les résultats de la conversion entre des chaînes et des types de données de date et d'heure :  
  
-   Lorsque vous utilisez l'opérateur de conversion ou la transformation de conversion des données, le type de données de date ou d'heure est converti au format chaîne correspondant. Par exemple, le type de données DT_DBTIME sera converti en une chaîne au format "hh:mm:ss".  
  
-   Lorsque vous souhaitez convertir à partir d'une chaîne vers un type de données de date ou d'heure, la chaîne doit utiliser le format de chaîne qui correspond au type de données de date ou d'heure approprié. Par exemple, pour convertir correctement certaines chaînes de date en type de données DT_DBDATE, ces chaînes de date doivent être au format, "yyyy-mm-dd".  
  
    |Type de données|Format chaîne|  
    |---------------|-------------------|  
    |DT_DBDATE|aaaa-mm-jj|  
    |DT_FILETIME|aaaa-mm-jj hh:mm:ss:fff|  
    |DT_DBTIME|hh:mm:ss|  
    |DT_DBTIME2|hh:mm:ss[.fffffff]|  
    |DT_DBTIMESTAMP|aaaa-mm-jj hh:mm:ss[.fff]|  
    |DT_DBTIMESTAMP2|aaaa-mm-jj hh:mm:ss[.fffffff]|  
    |DT_DBTIMESTAMPOFFSET|aaaa-mm-jj hh:mm:ss[.fffffff] [{+&#124;-} hh:mm]|  
  
 Dans le format de DT_FILETIME et DT_DBTIMESTAMP, « fff » est une valeur comprise entre 0 et 999 représentant les fractions de seconde.  
  
 Dans le format de date de DT_DBTIMESTAMP2, DT_DBTIME2 et DT_DBTIMESTAMPOFFSET, « fffffff » est une valeur comprise entre 0 et 9 999 999 représentant les fractions de seconde.  
  
 Le format de date de DT_DBTIMESTAMPOFFSET inclut également un élément de fuseau horaire. Un espace est présent entre l'élément d'heure et l'élément de fuseau horaire.  
  
### <a name="converting-datetime-data-types"></a>Conversion des types de données de date/heure  
 Vous pouvez changer le type de données d'une colonne contenant des données de date/heure afin d'extraire la partie date ou heure des données. Les tableaux suivants répertorient les résultats du changement d'un type de données de date/heure en un autre type de données de date/heure.  
  
#### <a name="converting-from-dtfiletime"></a>Conversion à partir de DT_FILETIME  
  
|Conversion de DT_FILETIME en|Résultats|  
|-----------------------------|------------|  
|DT_FILETIME|Aucun changement.|  
|DT_DATE|Convertit le type de données.|  
|DT_DBDATE|Supprime la valeur d'heure.|  
|DT_DBTIME|Supprime la valeur de date.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres fractionnaires que le type de données DT_DBTIME peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIME2|Supprime la valeur de date représentée par le type de données DT_FILETIME.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME2 peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMP|Convertit le type de données.|  
|DT_DBTIMESTAMP2|Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMP2 peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMPOFFSET|Définit zéro pour le champ de fuseau horaire dans le type de données DT_DBTIMESTAMPOFFSET.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMPOFFSET peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
  
#### <a name="converting-from-dtdate"></a>Conversion à partir de DT_DATE  
  
|Conversion de DT_DATE en|Résultats|  
|-------------------------|------------|  
|DT_FILETIME|Convertit le type de données.|  
|DT_DATE|Aucun changement.|  
|DT_DBDATE|Supprime la valeur d'heure représentée par le type de données DT_DATA.|  
|DT_DBTIME|Supprime la valeur de date représentée par le type de données DT_DATE.|  
|DT_DBTIME2|Supprime la valeur de date représentée par le type de données DT_DATE.|  
|DT_DBTIMESTAMP|Convertit le type de données.|  
|DT_DBTIMESTAMP2|Convertit le type de données.|  
|DT_DBTIMESTAMPOFFSET|Définit zéro pour le champ de fuseau horaire dans le type de données DT_DBTIMESTAMPOFFSET.|  
  
#### <a name="converting-from-dtdbdate"></a>Conversion à partir de DT_DBDATE  
  
|Conversion de DT_DBDATE en|Résultats|  
|---------------------------|------------|  
|DT_FILETIME|Définit zéro pour les champs d'heure dans le type de données DT_FILETIME.|  
|DT_DATE|Définit zéro pour les champs d'heure dans le type de données DT_DATE.|  
|DT_DBDATE|Aucun changement.|  
|DT_DBTIME|Définit zéro pour les champs d'heure dans le type de données DT_DBTIME.|  
|DT_DBTIME2|Définit zéro pour les champs d'heure dans le type de données DT_DBTIME2.|  
|DT_DBTIMESTAMP|Définit zéro pour les champs d'heure dans le type de données DT_DBTIMESTAMP.|  
|DT_DBTIMESTAMP2|Définit zéro pour les champs d'heure dans le type de données DT_DBTIMESTAMP.|  
|DT_DBTIMESTAMPOFFSET|Définit zéro pour les champs d'heure et de fuseau horaire dans le type de données DT_DBTIMESTAMPOFFSET.|  
  
#### <a name="converting-from-dtdbtime"></a>Conversion à partir de DT_DBTIME  
  
|Conversion de DT_DBTIME en|Résultats|  
|---------------------------|------------|  
|DT_FILETIME|Définit la date actuelle pour le champ de date dans le type de données DT_FILETIME.|  
|DT_DATE|Définit la date actuelle pour le champ de date dans le type de données DT_DATE.|  
|DT_DBDATE|Définit la date actuelle pour le champ de date dans le type de données DT_DBDATE.|  
|DT_DBTIME|Aucun changement.|  
|DT_DBTIME2|Convertit le type de données.|  
|DT_DBTIMESTAMP|Définit la date actuelle pour le champ de date dans le type de données DT_DBTIMESTAMP.|  
|DT_DBTIMESTAMP2|Définit la date actuelle pour le champ de date dans le type de données DT_DBTIMESTAMP2.|  
|DT_DBTIMESTAMPOFFSET|Définit respectivement la date actuelle et zéro pour les champs de date et de fuseau horaire dans le type de données DT_DBTIMESTAMPOFFSET.|  
  
#### <a name="converting-from-dtdbtime2"></a>Conversion à partir de DT_DBTIME2  
  
|Conversion de DT_DBTIME2 en|Résultats|  
|----------------------------|------------|  
|DT_FILETIME|Définit la date actuelle pour le champ de date dans le type de données DT_FILETIME.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_FILETIME peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DATE|Définit la date actuelle pour le champ de date du type de données DT_DATE.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DATE peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBDATE|Définit la date actuelle pour le champ de date du type de données DT_DBDATE.|  
|DT_DBTIME|Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIME2|Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME2 de destination peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMP|Définit la date actuelle pour le champ de date dans le type de données DT_DBTIMESTAMP.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMP peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMP2|Définit la date actuelle pour le champ de date dans le type de données DT_DBTIMESTAMP2.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMP2 peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMPOFFSET|Définit respectivement la date actuelle et zéro pour les champs de date et de fuseau horaire dans le type de données DT_DBTIMESTAMPOFFSET.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMPOFFSET peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
  
#### <a name="converting-from-dtdbtimestamp"></a>Conversion à partir de DT_DBTIMESTAMP  
  
|Conversion de DT_DBTIMESTAMP en|Résultats|  
|--------------------------------|------------|  
|DT_FILETIME|Convertit le type de données.|  
|DT_DATE|Si une valeur représentée par le type de données DT_DBTIMESTAMP dépasse la plage du type de données DT_DATE, l'erreur DB_E_DATAOVERFLOW est retournée. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBDATE|Supprime la valeur d'heure représentée par le type de données DT_DBTIMESTAMP.|  
|DT_DBTIME|Supprime la valeur de date représentée par le type de données DT_DBTIMESTAMP.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIME2|Supprime la valeur de date représentée par le type de données DT_DBTIMESTAMP.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME2 peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMP|Aucun changement.|  
|DT_DBTIMESTAMP2|Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMP2 peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMPOFFSET|Définit zéro pour le champ de fuseau horaire dans le type de données DT_DBTIMESTAMPOFFSET.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMPOFFSET peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
  
#### <a name="converting-from-dtdbtimestamp2"></a>Conversion à partir de DT_DBTIMESTAMP2  
  
|Conversion de DT_DBTIMESTAMP2 en|Résultats|  
|---------------------------------|------------|  
|DT_FILETIME|Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_FILETIME peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DATE|Si une valeur représentée par le type de données DT_DBTIMESTAMP2 dépasse la plage du type de données DT_DATE, l'erreur DB_E_DATAOVERFLOW est retournée. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DATE peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBDATE|Supprime la valeur d'heure représentée par le type de données DT_DBTIMESTAMP2.|  
|DT_DBTIME|Supprime la valeur de date représentée par le type de données DT_DBTIMESTAMP2.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIME2|Supprime la valeur de date représentée par le type de données DT_DBTIMESTAMP2.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME2 peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMP|Si une valeur représentée par le type de données DT_DBTIMESTAMP2 dépasse la plage du type de données DT_DBTIMESTAMP, l'erreur DB_E_DATAOVERFLOW est retournée.<br /><br /> DT_DBTIMESTAMP2 est mappé en type de données datetime2 SQL Server avec une plage comprise entre le 1er janvier de l'an 1 et le 31 décembre 9999. DT_DBTIMESTAMP est mappé en type de données datetime SQL Server, avec une plage plus restreinte comprise entre le 1er janvier 1753 et le 31 décembre 9999.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMP peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données.<br /><br /> Pour plus d’informations sur les erreurs, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMP2|Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMP2 de destination peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMPOFFSET|Définit zéro pour le champ de fuseau horaire dans le type de données DT_DBTIMESTAMPOFFSET.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMPOFFSET peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
  
#### <a name="converting-from-dtdbtimestampoffset"></a>Conversion à partir de DT_DBTIMESTAMPOFFSET  
  
|Conversion de DT_DBTIMESTAMPOFFSET en|Résultats|  
|--------------------------------------|------------|  
|DT_FILETIME|Change la valeur d'heure représentée par le type de données DT_DBTIMESTAMPOFFSET en temps universel coordonné (UTC).<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_FILETIME peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DATE|Change la valeur d’heure représentée par le type de données DT_DBTIMESTAMPOFFSET en temps universel coordonné (UTC).<br /><br /> Si une valeur représentée par le type de données DT_DBTIMESTAMPOFFSET dépasse la plage du type de données DT_DATE, l'erreur DB_E_DATAOVERFLOW est retournée.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DATE peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données.<br /><br /> Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBDATE|Change la valeur d'heure représentée par le type de données DT_DBTIMESTAMPOFFSET en temps universel coordonné (UTC), qui peut affecter la valeur de date. La valeur d'heure est ensuite supprimée.|  
|DT_DBTIME|Change la valeur d’heure représentée par le type de données DT_DBTIMESTAMPOFFSET en temps universel coordonné (UTC).<br /><br /> Supprime la valeur de date représentée par le type de données DT_DBTIMESTAMPEOFFSET.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIME2|Change la valeur d’heure représentée par le type de données DT_DBTIMESTAMPOFFSET en temps universel coordonné (UTC).<br /><br /> Supprime la valeur de date représentée par le type de données DT_DBTIMESTAMPOFFSET.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIME2 peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMP|Change la valeur d’heure représentée par le type de données DT_DBTIMESTAMPOFFSET en temps universel coordonné (UTC).<br /><br /> Si une valeur représentée par le type de données DT_DBTIMESTAMPOFFSET dépasse la plage du type de données DT_DBTIMESTAMP, l'erreur DB_E_DATAOVERFLOW est retournée.<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMP peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données.<br /><br /> Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMP2|Change la valeur d’heure représentée par le type de données DT_DBTIMESTAMPOFFSET en temps universel coordonné (UTC).<br /><br /> Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMP2 peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
|DT_DBTIMESTAMPOFFSET|Supprime la valeur de fraction de seconde lorsque son échelle est supérieure au nombre de chiffres de fraction de seconde que le type de données DT_DBTIMESTAMPOFFSET de destination peut contenir. Après la suppression de la valeur de fraction de seconde, un rapport est généré sur cette troncation de données. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).|  
  
## <a name="mapping-of-integration-services-data-types-to-database-data-types"></a>Mappage des types de données Integration Services en types de données de base de données  
 Le tableau ci-dessous fournit des directives pour le mappage des types de données employés par certaines bases de données avec des types de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Ces mappages sont résumés à partir des fichiers de mappage qu'utilise l'Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsqu'il importe des données depuis ces sources. Pour plus d’informations sur ces fichiers de mappage, consultez [Assistant Importation et Exportation SQL Server](~/integration-services/import-export-data/welcome-to-sql-server-import-and-export-wizard.md).  
  
> [!IMPORTANT]  
>  Ces mappages n'ont pas pour but de proposer une équivalence stricte. Ils sont fournis uniquement à titre d'indication. Dans certaines situations, vous devrez peut-être utiliser un type de données différent de celui utilisé dans ce tableau.  
  
> [!NOTE]  
>  Vous pouvez utiliser les types de données SQL Server pour estimer la taille des types de données date et heure Integration Services correspondants.  
  
|Type de données|SQL Server<br /><br /> (SQLOLEDB; SQLNCLI10)|SQL Server (SqlClient)|Jet|Oracle<br /><br /> (OracleClient)|DB2<br /><br /> (DB2OLEDB)|DB2<br /><br /> (IBMDADB2)|  
|---------------|--------------------------------------------|------------------------------|---------|---------------------------------|--------------------------|--------------------------|  
|DT_BOOL|bit|bit|bit||||  
|DT_BYTES|binary, varbinary, timestamp|binary, varbinary, timestamp|BigBinary, VarBinary|RAW|||  
|DT_CY|smallmoney, money|smallmoney, money|Monétaire (Currency)||||  
|DT_DATE|||||||  
|DT_DBDATE|[date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md)|[date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md)||Date|Date|Date|  
|DT_DBTIME||||TIMESTAMP|time|time|  
|DT_DBTIME2|[time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md)(p)|[time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md) (p)|||||  
|DT_DBTIMESTAMP|[datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md), [smalldatetime &#40;Transact-SQL&#41;](../../t-sql/data-types/smalldatetime-transact-sql.md)|[datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md), [smalldatetime &#40;Transact-SQL&#41;](../../t-sql/data-types/smalldatetime-transact-sql.md)|DateTime|TIMESTAMP, DATE, INTERVAL|TIME, TIMESTAMP, DATE|TIME, TIMESTAMP, DATE|  
|DT_DBTIMESTAMP2|[datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)|[datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)||TIMESTAMP|TIMESTAMP|TIMESTAMP|  
|DT_DBTIMESTAMPOFFSET|[datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)(p)|[datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md) (p)||timestampoffset|timestamp,<br /><br /> varchar|timestamp,<br /><br /> varchar|  
|DT_DECIMAL|||||||  
|DT_FILETIME|||||||  
|DT_GUID|UNIQUEIDENTIFIER|UNIQUEIDENTIFIER|GUID||||  
|DT_I1|||||||  
|DT_I2|SMALLINT|SMALLINT|Short||smallint|SMALLINT|  
|DT_I4|INT|INT|Long||INTEGER|INTEGER|  
|DT_I8|BIGINT|BIGINT|||BIGINT|bigint|  
|DT_NUMERIC|decimal, numeric|decimal, numeric|Décimal|NUMBER, INT|decimal, numeric|decimal, numeric|  
|DT_R4|REAL|REAL|Unique||real|real|  
|DT_R8|float|FLOAT|Double|FLOAT, REAL|FLOAT, DOUBLE|FLOAT, DOUBLE|  
|DT_STR|char, varchar||varchar||char, varchar|char, varchar|  
|DT_UI1|TINYINT|TINYINT|Byte||||  
|DT_UI2|||||||  
|DT_UI4|||||||  
|DT_UI8|||||||  
|DT_WSTR|nchar, nvarchar, sql_variant, xml|char, varchar, nchar, nvarchar, sql_variant, xml|LongText|CHAR, ROWID, VARCHAR2, NVARCHAR2, NCHAR|GRAPHIC, VARGRAPHIC|GRAPHIC, VARGRAPHIC|  
|DT_IMAGE|image|image|LongBinary|LONG RAW, BLOB, LOBLOCATOR, BFILE, VARGRAPHIC, LONG VARGRAPHIC, défini par l'utilisateur|CHAR () FOR BIT DATA, VARCHAR () FOR BIT DATA|CHAR () FOR BIT DATA, VARCHAR () FOR BIT DATA, BLOB|  
|DT_NTEXT|ntext|text, ntext||LONG, CLOB, NCLOB, NVARCHAR, TEXT|LONG VARCHAR, NCHAR, NVARCHAR, TEXT|LONG VARCHAR, DBCLOB, NCHAR, NVARCHAR, TEXT|  
|DT_TEXT|texte||||LONG VARCHAR FOR BIT DATA|LONG VARCHAR FOR BIT DATA, CLOB|  
  
 Pour plus d’informations sur le mappage des types de données dans le flux de données, consultez [Utilisation de types de données dans le flux de données](../../integration-services/extending-packages-custom-objects/data-flow/working-with-data-types-in-the-data-flow.md).  
  
## <a name="related-content"></a>Contenu associé  
 Entrée de blog, [Performance Comparison between Data Type Conversion Techniques in SSIS 2008](http://go.microsoft.com/fwlink/?LinkId=220823), sur blogs.msdn.com.  
  
## <a name="see-also"></a> Voir aussi  
 [Données dans des flux de données](../../integration-services/data-flow/data-in-data-flows.md)  
  
  
