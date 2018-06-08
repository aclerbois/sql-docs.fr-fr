---
title: RESTORE SERVICE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- RESTORE SERVICE MASTER KEY
- RESTORE_SERVICE_MASTER_KEY_TSQL
- LOAD SERVICE MASTER KEY
- LOAD_SERVICE_MASTER_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- importing Service Master Keys
- copying Service Master Keys
- service master key [SQL Server], importing
- RESTORE SERVICE MASTER KEY statement
- transferring Service Master Keys
ms.assetid: a68fd0ee-70ce-4104-aca0-fcae5f41fc38
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a214354c9a68588f81959e8fdde4db3c3c85e537
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="restore-service-master-key-transact-sql"></a>RESTORE SERVICE MASTER KEY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet d'importer une clé principale de service à partir d'un fichier de sauvegarde.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RESTORE SERVICE MASTER KEY FROM FILE = 'path_to_file'   
    DECRYPTION BY PASSWORD = 'password' [FORCE]  
```  
  
## <a name="arguments"></a>Arguments  
 FILE **='***path_to_file***'**  
 Spécifie le chemin d'accès complet, y compris le nom de fichier, de la clé principale de service stockée. *path_to_file* peut être un chemin local ou un chemin UNC d’un emplacement réseau.  
  
 PASSWORD **='***password***'**  
 Spécifie le mot de passe requis pour déchiffrer la clé principale de service à importer à partir d'un fichier.  
  
 FORCE  
 Impose le remplacement de la clé principale de service, même au risque de perdre des données.  
  
## <a name="remarks"></a>Notes   
 Lorsque la clé principale de service est restaurée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déchiffre toutes les clés et les secrets qui ont été chiffrés au moyen de la clé principale de service en cours, puis les chiffre au moyen de la clé principale de service chargée à partir du fichier de sauvegarde.  
  
 Si l'un des déchiffrements échoue, la restauration échoue. Vous pouvez utiliser l'option FORCE pour ignorer les erreurs, mais cette option entraîne la perte de toutes les données ne pouvant pas être déchiffrées.  
  
> [!CAUTION]  
>  La clé principale de service représente la racine de la hiérarchie de chiffrement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La clé principale de service sécurise de manière directe ou indirecte toutes les autres clés de l'arborescence. Si une clé dépendante ne peut pas être déchiffrée au cours d'une restauration forcée, les données sécurisées par cette clé sont perdues.  
  
 La régénération de la hiérarchie de chiffrement est une opération qui consomme beaucoup de ressources. Par conséquent, vous devez planifier cette opération au cours d'une période de faible demande.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER sur le serveur.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, la clé principale de service est restaurée à partir d'un fichier de sauvegarde.  
  
```  
RESTORE SERVICE MASTER KEY   
    FROM FILE = 'c:\temp_backups\keys\service_master_key'   
    DECRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Clé principale du service](../../relational-databases/security/encryption/service-master-key.md)   
 [ALTER SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-service-master-key-transact-sql.md)   
 [BACKUP SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-service-master-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
