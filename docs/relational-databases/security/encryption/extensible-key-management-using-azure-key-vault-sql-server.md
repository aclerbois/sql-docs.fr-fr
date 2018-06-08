---
title: Gestion de clés extensible à l’aide d’Azure Key Vault (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Extensible Key Management with key vault
- Transparent Data Encryption, using EKM and key vault
- EKM, with key vault
- TDE, EKM and key vault
- Key Management with key vault
- SQL Server Connector, about
ms.assetid: 3efdc48a-8064-4ea6-a828-3fbf758ef97c
caps.latest.revision: 66
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 81cb6d551d9e734435b36cd9d1fc4427c4f83eae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="extensible-key-management-using-azure-key-vault-sql-server"></a>Gestion de clés extensible à l'aide d'Azure Key Vault (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Azure Key Vault permet au chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’utiliser le service Azure Key Vault comme fournisseur de [gestion de clés extensible &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md) pour protéger les clés de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Cette rubrique décrit le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Des informations supplémentaires sont disponibles dans les rubriques [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md), [Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)et [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).  
  
##  <a name="Uses"></a> Qu’est-ce que la gestion de clés extensible et pourquoi l’utiliser ?  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournit plusieurs types de chiffrement qui permettent de protéger les données sensibles, notamment le [chiffrement transparent des données &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md), le [chiffrement au niveau colonne](../../../t-sql/functions/cryptographic-functions-transact-sql.md) (CLE) et le [chiffrement de sauvegarde](../../../relational-databases/backup-restore/backup-encryption.md). Dans tous ces cas, dans cette hiérarchie de clés classique, les données sont chiffrées à l’aide d’une clé de chiffrement de données symétrique. La clé de chiffrement de données symétrique est ensuite protégée en la chiffrant avec une hiérarchie de clés stockées dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Au lieu de ce modèle, l’alternative est le modèle de fournisseur EKM. L’utilisation de l’architecture du fournisseur EKM permet à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de protéger les clés de chiffrement de données à l’aide d’une clé asymétrique stockée en dehors de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , dans un fournisseur de services de chiffrement externe. Ce modèle ajoute une couche de sécurité supplémentaire et sépare la gestion des clés et des données.  
   
 L’illustration suivante compare la hiérarchie de clés de gestion de service classique au système Azure Key Vault.  
  
 ![ekm-key-hierarchy-traditional](../../../relational-databases/security/encryption/media/ekm-key-hierarchy-traditional.png "ekm-key-hierarchy-traditional")  
  
   
 Le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sert de pont entre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et Azure Key Vault. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut donc optimiser l’extensibilité, les hautes performances et la haute disponibilité du service Azure Key Vault. L’illustration suivante représente le fonctionnement de la hiérarchie de clé dans l’architecture du fournisseur EKM avec Azure Key Vault et le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
  Azure Key Vault peut être utilisé avec des installations de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur des machines virtuelles [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Azure et sur des serveurs locaux. Le service de coffre de clés offre également la possibilité d'utiliser des modules de sécurité matériels étroitement contrôlés et surveillés pour augmenter le niveau de protection des clés de chiffrement asymétriques. Pour plus d’informations sur le coffre de clés, consultez [Azure Key Vault](http://go.microsoft.com/fwlink/?LinkId=521401).  
  
 L'illustration suivante résume le flux du processus de la gestion de clés extensible à l'aide du coffre de clés. (Les numéros des étapes du processus dans l’image ne correspondent pas aux numéros des étapes d’installation qui suivent l’image.)  
  
 ![Gestion de clés extensible (EKM) SQL Server avec Azure Key Vault](../../../relational-databases/security/encryption/media/ekm-using-azure-key-vault.png "Gestion de clés extensible (EKM) SQL Server avec Azure Key Vault")  

> [!NOTE]  
>  Les versions 1.0.0.440 et antérieures ont été remplacées et ne sont plus prises en charge dans les environnements de production. Effectuez la mise à niveau vers la version 1.0.1.0 ou ultérieure en accédant au [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=45344) et en utilisant les instructions de la page [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md) sous « Mise à niveau du connecteur SQL Server ».
  
 Pour l’étape suivante, consultez [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md).  
  
 Pour les scénarios d’utilisation, consultez [Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)  
  
  
