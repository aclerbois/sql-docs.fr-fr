---
title: Chiffrement SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], about encryption
- security [SQL Server], encryption
- cryptography [SQL Server], about cryptography
ms.assetid: ead0150e-4943-4ad5-84c8-36f85c7278f4
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d1e12ed415b2f827a20d4b31e36115a63164ac98
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34707747"
---
# <a name="sql-server-encryption"></a>Chiffrement SQL Server
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Le chiffrement est un processus visant à rendre des données inintelligibles à l'aide d'une clé ou d'un mot de passe. Les données sont alors inutiles en l'absence du mot de passe ou de la clé de déchiffrement correspondante. Le chiffrement ne résout pas les problèmes de contrôle d'accès. Toutefois, il améliore la sécurité en limitant les pertes de données même si les contrôles d'accès sont contournés. Par exemple, si l'ordinateur hôte de la base de données est mal configuré et qu'un pirate parvient à se procurer des données sensibles, les données subtilisées seront vraisemblablement inexploitables si elles sont chiffrées.  
  

> [!IMPORTANT]  
>  Bien que le chiffrement soit un outil précieux qui aide à garantir la sécurité, il ne doit pas être envisagé pour toutes les données et connexions. Lorsque vous décidez de mettre en œuvre ou non le chiffrement, tenez compte de la manière dont les utilisateurs accèderont aux données. Si les utilisateurs accèdent aux données via un réseau public, le chiffrement des données peut être requis pour augmenter la sécurité. Toutefois, si tous les accès impliquent une configuration intranet sécurisée, le chiffrement peut s'avérer superflu. Toute utilisation du chiffrement doit également inclure une stratégie de maintenance pour les mots de passe, les clés et les certificats.  
  
> [!NOTE]  
>  Les dernières informations sur la sécurité de niveau transport (TSL1.2) sont disponibles dans [Prise en charge de TLS 1.2 pour Microsoft SQL Server](https://support.microsoft.com/kb/3135244).  

Vous pouvez utiliser le chiffrement dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour les connexions, les données et les procédures stockées. Les rubriques suivantes contiennent davantage d’informations sur le chiffrement dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

 [Hiérarchie de chiffrement](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
 Informations sur la hiérarchie de chiffrement dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [Choisir un algorithme de chiffrement](../../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
 Informations sur la manière de sélectionner un algorithme de chiffrement efficace.  
  
 [Transparent Data Encryption &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
 Informations générales sur la manière de chiffrer des données de façon transparente.  
  
 [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  
 Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les clés de chiffrement incluent une combinaison d'une clé publique, d'une clé privée et d'une clé symétrique, dont le but est de protéger les données sensibles. Cette section explique comment implémenter et gérer des clés de chiffrement.  
  
 [Always Encrypted &#40;moteur de base de données&#41;](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
 Empêche les administrateurs de base de données locaux, les opérateurs de base de données cloud ou les autres utilisateurs dotés de privilèges élevés, mais non autorisés, d’accéder aux données chiffrées.  
  
 [Masquage dynamique des données](../../../relational-databases/security/dynamic-data-masking.md)  
 Limite l’exposition des données sensibles en les masquant aux utilisateurs sans privilège.  
  
 [Certificats et clés asymétriques SQL Server](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)  
 Informations sur l’utilisation du chiffrement à clé publique.  
  
## <a name="related-content"></a>Contenu associé  
 [Sécurisation de SQL Server](../../../relational-databases/security/securing-sql-server.md)  
 Vue d'ensemble du processus permettant de mieux sécuriser la plateforme [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et du travail avec des utilisateurs et des objets sécurisables.  
  
 [Fonctions de chiffrement &#40;Transact-SQL&#41;](../../../t-sql/functions/cryptographic-functions-transact-sql.md)  
 Informations sur la manière d'implémenter des fonctions de chiffrement.  
  
 [ENCRYPTBYPASSPHRASE &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbypassphrase-transact-sql.md)  
 Informations sur l'utilisation d'un mot de passe pour chiffrer des données.  
  
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbykey-transact-sql.md)  
 Informations sur l'utilisation d'une clé symétrique pour chiffrer des données.  
  
 [ENCRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbyasymkey-transact-sql.md)  
 Informations sur l'utilisation d'une clé asymétrique pour chiffrer des données.  
  
 [ENCRYPTBYCERT &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbycert-transact-sql.md)  
 Informations sur l'utilisation d'un certificat pour chiffrer des données.  
  
## <a name="external-resources"></a>Ressources externes  
 [Microsoft TechNet : SQL Server TechCenter : SQL Server 2012 - Sécurité et protection](http://download.microsoft.com/download/8/F/A/8FABACD7-803E-40FC-ADF8-355E7D218F4C/SQL_Server_2012_Security_Best_Practice_Whitepaper_Apr2012.docx)  
 Informations à jour sur la sécurité [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a> Voir aussi  
 [sys.key_encryptions &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)   
 [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Sauvegarder et restaurer les clés de chiffrement Reporting Services](../../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)  
  
  
