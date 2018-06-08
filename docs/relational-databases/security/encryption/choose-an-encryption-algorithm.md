---
title: Choisir un algorithme de chiffrement | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- cryptography [SQL Server], algorithms
- encryption [SQL Server], algorithms
- security [SQL Server], encryption
- algorithms [SQL Server encryption]
ms.assetid: 8227028c-a9c9-489d-bd27-fbf8242634ae
caps.latest.revision: 36
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 87a50c93d0d037063b21ac6de1dda2f8ac2a58c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="choose-an-encryption-algorithm"></a>Choisir un algorithme de chiffrement
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Le chiffrement est l'une des mesures préventives à la disposition des administrateurs qui souhaitent sécuriser une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Les algorithmes de chiffrement définissent les transformations de données qui ne peuvent pas être facilement inversées par les utilisateurs non autorisés. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permet aux administrateurs et aux développeurs de choisir entre plusieurs algorithmes, notamment DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, RC4 128 bits, DESX, AES 128 bits, AES 192 bits et AES 256 bits.  
  
> [!NOTE]  
>  À partir de [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)], tous les algorithmes autres que AES_128, AES_192 et AES_256 sont déconseillés. Pour utiliser des algorithmes plus anciens (ce qui n’est pas recommandé), vous devez affecter le niveau de compatibilité 120 ou un niveau inférieur à la base de données.  
  
 Aucun algorithme unique ne convient pour toutes les situations et l'appréciation des avantages de chaque algorithme dépasse le cadre de la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Cependant, les principes suivants s'appliquent :  
  
-   Un chiffrement renforcé consomme en général davantage de ressources processeur qu'un chiffrement plus faible.  
  
-   Les clés longues produisent en général un chiffrement plus fort que les clés courtes.  
  
-   À longueur de clé égale, le chiffrement asymétrique est plus faible que le chiffrement symétrique, mais il est relativement lent.  
  
-   Le chiffrement par blocs avec des clés longues est plus fort que le chiffrement par flux.  
  
-   Les mots de passe longs et complexes sont plus forts que les mots de passe courts.  
  
-   Si vous chiffrez de grandes quantités de données, vous devez utiliser pour cela une clé symétrique, puis chiffrer cette clé avec une clé asymétrique.  
  
-   Les données chiffrées ne peuvent pas être compressées, mais les données compressées peuvent être chiffrées. Si vous utilisez la compression, vous devez compresser les données avant de les chiffrer.  
  
> [!IMPORTANT]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] et les versions ultérieures, le matériel chiffré à l'aide de RC4 ou de RC4_128 peut être déchiffré dans n'importe quel niveau de compatibilité.  
>   
>  L'utilisation répétée du même RC4 ou RC4_128 KEY_GUID sur différents blocs de données entraîne la même clé RC4 car [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne fournit pas automatiquement de salt. L'utilisation répétée de la même clé RC4 est une erreur connue qui entraîne un chiffrement très faible. Par conséquent, les mots clés RC4 et RC4_128 sont déconseillés. [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)]  
  
 Pour plus d'informations sur les algorithmes et la technologie de chiffrement, consultez la rubrique [Concepts fondamentaux sur la sécurité](http://go.microsoft.com/fwlink/?LinkId=62082) dans le Guide du développeur .NET Framework sur le site MSDN.  
  
 **Éclaircissement concernant les algorithmes DES :**  
  
-   DESX a été nommé incorrectement. Les clés symétriques créées avec ALGORITHM = DESX utilisent en fait le chiffrement TRIPLE DES avec une clé de 192 bits. L'algorithme DESX n'est pas fourni. [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Les clés symétriques créées avec ALGORITHM = TRIPLE_DES_3KEY utilisent TRIPLE DES avec une clé de 192 bits.  
  
-   Les clés symétriques créées avec ALGORITHM = TRIPLE_DES utilisent TRIPLE DES avec une clé de 128 bits.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|||  
|-|-|  
|Chiffrement à l'aide d'une clé symétrique.|[CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)|  
|Chiffrement à l'aide d'une clé asymétrique.|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)|  
|Chiffrement à l'aide d'un certificat.|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)|  
|Chiffrement de fichiers de base de données à l'aide du chiffrement transparent des données.|[Transparent Data Encryption &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md)|  
|Comment chiffrer une colonne d'une table.|[Chiffrer une colonne de données](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Chiffrement SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Hiérarchie de chiffrement](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
