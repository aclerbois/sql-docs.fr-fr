---
title: RESTORE MASTER KEY (Transact-SQL) | Microsoft Docs
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
- RESTORE_MASTER_KEY_TSQL
- RESTORE MASTER KEY
- LOAD_MASTER_KEY_TSQL
- LOAD MASTER KEY
dev_langs:
- TSQL
helpviewer_keywords:
- database master key [SQL Server], importing
- encryption [SQL Server], Database Master Key
- copying Database Master Keys
- importing Database Master Keys
- cryptography [SQL Server], Database Master Key
- transferring Database Master Keys
- RESTORE MASTER KEY statement
ms.assetid: 70ceb951-31a2-4fc4-a0c1-e6c18eeb3ae7
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 863477cc362d072a4bf2b5378d70f81ea241d684
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="restore-master-key-transact-sql"></a>RESTORE MASTER KEY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet d'importer la clé principale d'une base de données à partir d'un fichier de sauvegarde.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RESTORE MASTER KEY FROM FILE = 'path_to_file'   
    DECRYPTION BY PASSWORD = 'password'  
    ENCRYPTION BY PASSWORD = 'password'  
    [ FORCE ]  
```  
  
## <a name="arguments"></a>Arguments  
 FILE ='*path_to_file*'  
 Spécifie le chemin d'accès complet, y compris le nom de fichier, de la clé principale de base de données stockée. *path_to_file* peut être un chemin local ou un chemin UNC d’un emplacement réseau.  
  
 DECRYPTION BY PASSWORD ='*password*'  
 Spécifie le mot de passe requis pour déchiffrer la clé principale de base de données à importer à partir d'un fichier.  
  
 ENCRYPTION BY PASSWORD ='*password*'  
 Spécifie le mot de passe utilisé pour chiffrer la clé principale de base de données après son chargement dans la base de données.  
  
 FORCE  
 Indique que le processus RESTORE doit continuer, même si la clé principale de base de données en cours n'est pas ouverte ou si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas déchiffrer certaines clés privées chiffrées au moyen de cette clé.  
  
## <a name="remarks"></a>Notes   
 Une fois la clé principale restaurée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déchiffre toutes les clés chiffrées au moyen de la clé principale active, puis chiffre ces clés au moyen de la clé principale restaurée. Cette opération gourmande en ressources doit être planifiée au cours d'une période de faible demande. Si la clé principale de base de données en cours n'est pas ouverte ou ne peut pas être ouverte, ou si une clé chiffrée à l'aide de cette clé principale ne peut pas être déchiffrée, l'opération de restauration échoue.  
  
 Utilisez l'option FORCE seulement si la clé principale ne peut pas être récupérée ou si le déchiffrement échoue. Les données chiffrées uniquement à l'aide d'une clé irrécupérable sont perdues.  
  
 Si la clé principale a été chiffrée à l'aide de la clé principale du service, la clé principale restaurée sera également chiffrée à l'aide de la clé principale du service.  
  
 S'il n'existe aucune clé principale dans la base de données en cours, l'instruction RESTORE MASTER KEY crée une clé principale. La nouvelle clé principale n'est pas automatiquement chiffrée au moyen de la clé principale du service.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur la base de données.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, la clé principale de la base de données `AdventureWorks2012` est restaurée.  
  
```  
USE AdventureWorks2012;  
RESTORE MASTER KEY   
    FROM FILE = 'c:\backups\keys\AdventureWorks2012_master_key'   
    DECRYPTION BY PASSWORD = '3dH85Hhk003#GHkf02597gheij04'   
    ENCRYPTION BY PASSWORD = '259087M#MyjkFkjhywiyedfgGDFD';  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
