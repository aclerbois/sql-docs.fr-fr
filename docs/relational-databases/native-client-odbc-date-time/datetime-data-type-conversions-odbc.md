---
title: DateTime, Conversions de Type de données (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-date-time
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- conversions [ODBC]
- bindings [ODBC]
- ODBC, bindings and conversions
ms.assetid: 66b9d282-c88d-40e5-93c2-fd5499a74458
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 39a6c79dee9d84ee85efd3b9d8b97b1ae82d081e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datetime-data-type-conversions-odbc"></a>Conversions du type de données datetime (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Les conversions suivantes sont déjà définies par ODBC ou constituent une extension cohérente d'ODBC. Les conversions fournies par chaque fournisseur sont déterminées par la communauté servie par le fournisseur, et en conséquence il existe souvent des incohérences entre les fournisseurs. Les valeurs entre crochets ([ et ]) sont facultatives.  
  
-   Le format des chaînes datetime est 'aaaa-mm-jj[ hh:mm:ss[.9999999][ plus/moins hh:mm]]'  
  
-   Le format des chaînes time est 'hh:mm:ss[.9999999]'  
  
-   Le format des chaînes date est 'aaaa-mm-jj'  
  
 Les conversions de chaînes autorisent une souplesse en matière d'espace et de largeur de champ. Pour plus d’informations, consultez la section « Formats de données : chaînes et littéraux » de [prise en charge du Type de données de Date ODBC et les améliorations apportées au](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md).  
  
 Les règles suivantes sont les règles générales de conversion :  
  
-   Si aucune heure n'est fournie mais le récepteur peut stocker l'heure, l'heure est définie avec la valeur zéro.  
  
-   Si aucune date n'est présente mais que le récepteur peut stocker la date, la date courante est utilisée.  
  
-   Si aucun fuseau n'est présent dans le type de données que le client utilise mais que le serveur peut stocker le fuseau, la date est stockée dans le fuseau horaire du client. Notez la différence avec le comportement du serveur.  
  
-   Si aucun fuseau n'est présent dans le type de serveur mais que le type de client possède un fuseau, l'heure est convertie en heure UTC avant d'être stockée sur le serveur.  
  
-   Si l'heure est présente mais que le récepteur ne peut pas la stocker, le composant heure est ignoré.  
  
-   Si une date est présente mais que le récepteur ne peut pas la stocker, le composant date est ignoré.  
  
-   Si la troncation de secondes ou de fractions de seconde se produit lors de la conversion de C en SQL, un enregistrement de diagnostic est généré avec la valeur SQLSTATE 22008 et le message « Dépassement de la capacité du champ datetime ».  
  
-   Si la troncation de secondes ou de fractions de seconde se produit lors de la conversion de SQL en C, un enregistrement de diagnostic est généré avec la valeur SQLSTATE 01S07 et le message « Troncation fractionnelle ».  
  
## <a name="in-this-section"></a>Dans cette section  
 [Conversions de C en SQL](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-c-to-sql.md)  
 Répertorie les problèmes à prendre en compte lorsque vous effectuez une conversion de types C en types [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] date/time.  
  
 [Conversions à partir de SQL à C](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-sql-to-c.md)  
 Répertorie les problèmes à prendre en compte lorsque vous effectuez une conversion de types [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] date/time en types C.  
  
## <a name="see-also"></a>Voir aussi  
 [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
  
  
