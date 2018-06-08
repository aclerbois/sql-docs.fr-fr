---
title: Valeurs pour les déclarations &lt;xsd:simpleType&gt; | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- xsd:simpleType declarations
ms.assetid: 557b972d-3af9-40bf-8382-72b05c9de1c1
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 88285809430b9865183ba94181fca3ec54179f0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="values-for-ltxsdsimpletypegt-declarations"></a>Valeurs pour les déclarations &lt;xsd:simpleType&gt;
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Le tableau suivant décrit les restrictions appliquées, sur la base de toutes les énumérations de types simples XSD reconnus.  
  
 De plus, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge l’utilisation de la valeur NaN dans les déclarations **\<xsd:simpleType>**. Les schémas incluant cette valeur sont rejetés par le serveur.  
  
|Type simple|Limitation|  
|-----------------|----------------|  
|**duration**|La partie année doit figurer dans la plage comprise entre -2^31 et 2^31-1. Le mois, le jour, l'heure, la minute et la seconde doivent tous être compris dans la plage de 0 à 9999. La partie secondes possède trois chiffres supplémentaires de précision à droite de la virgule décimale.|  
|**dateTime**|La partie heure dans le sous-champ de fuseau horaire doit être comprise dans la plage acceptée de -14 à +14. La partie année doit figurer dans la plage comprise entre 1 et 9999. La partie mois doit figurer dans la plage comprise entre 1 et 12. La partie jour doit figurer dans la plage comprise entre 1 et 31 et doit être une date calendaire valide. Par exemple, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détecte une date non valide telle que 1974-02-31 et retourne une erreur puisque le mois de février ne contient pas 31 jours.<br /><br /> Le composant seconde prend en charge une précision de 100 nanosecondes. L'indication de fuseau horaire est facultative.<br /><br /> SQL Server 2005 prenait en charge les années comprises dans la plage de -9999 à 9999. SQL Server prend maintenant en charge une plage d'années plus restreinte. Pour plus d’informations, consultez [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).|  
|**date**|La partie année doit figurer dans la plage comprise entre 1 et 9999. La partie mois doit figurer dans la plage comprise entre 1 et 12. La partie jour doit figurer dans la plage comprise entre 1 et 31 et doit être une date calendaire valide. Par exemple, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détecte une date non valide telle que 1974-02-31 et retourne une erreur puisque le mois de février ne contient pas 31 jours.<br /><br /> SQL Server 2005 prenait en charge les années comprises dans la plage de -9999 à 9999. SQL Server prend maintenant en charge une plage d'années plus restreinte. Pour plus d’informations, consultez [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).|  
|**gYearMonth**|La partie année doit figurer dans la plage comprise entre -9999 et 9999.|  
|**gYear**|La partie année doit figurer dans la plage comprise entre -9999 et 9999.|  
|**gMonthDay**|La partie mois doit figurer dans la plage comprise entre 1 et 12. La partie jour doit figurer dans la plage comprise entre 1 et 31.|  
|**gDay**|La partie jour doit figurer dans la plage comprise entre 1 et 31.|  
|**gMonth**|La partie mois doit figurer dans la plage comprise entre 1 et 12.|  
|**decimal**|Les valeurs de ce type doivent être conformes au format du type numérique SQL. Ce type représente en interne la prise en charge des nombres jusqu'à un total de 38 chiffres, 10 de ces positions étant réservées à la précision fractionnelle.|  
|**float**|Les valeurs de ce type doivent être conformes au format du type **real** SQL.|  
|**double**|Les valeurs de ce type doivent être conformes au format du type **float** SQL.|  
|**chaîne**|Les valeurs de ce type doivent être conformes au format du type **nvarchar(max)** SQL.|  
|**anyURI**|Les valeurs de ce type ne peuvent excéder 4 000 caractères Unicode.|  
  
## <a name="see-also"></a> Voir aussi  
 [Spécifications et limitations relatives aux collections de schémas XML sur le serveur](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
