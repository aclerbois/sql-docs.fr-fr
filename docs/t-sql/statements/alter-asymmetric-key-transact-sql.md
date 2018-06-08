---
title: ALTER ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER_ASYMMETRIC_KEY_TSQL
- ALTER ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- passwords [SQL Server], asymmetric keys
- encryption [SQL Server], asymmetric keys
- DECRYPTION BY PASSWORD option
- ALTER ASYMMETRIC KEY statement
- asymmetric keys [SQL Server], modifying
ms.assetid: 958e95d6-fbe6-43e8-abbd-ccedbac2dbac
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f746dd302ef4ce303225f6fe822173a6f6b37b86
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-asymmetric-key-transact-sql"></a>ALTER ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Permet de modifier les propriétés d'une clé asymétrique.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ALTER ASYMMETRIC KEY Asym_Key_Name <alter_option>  
  
<alter_option> ::=  
      <password_change_option>   
    | REMOVE PRIVATE KEY   

<password_change_option> ::=  
    WITH PRIVATE KEY ( <password_option> [ , <password_option> ] )  

<password_option> ::=  
      ENCRYPTION BY PASSWORD = 'strongPassword'  
    | DECRYPTION BY PASSWORD = 'oldPassword'  
```  
  
## <a name="arguments"></a>Arguments  
 *Asym_Key_Name*  
 Nom sous lequel la clé asymétrique est connue dans la base de données.  
  
 REMOVE PRIVATE KEY  
 Permet de supprimer la clé privée de la clé asymétrique, sans supprimer la clé publique.  
  
 WITH PRIVATE KEY  
 Modifie la protection de la clé privée.  
  
 ENCRYPTION BY PASSWORD **='***stongPassword***'**  
 Spécifie un nouveau mot de passe pour protéger la clé privée. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si cette option est omise, la clé privée sera chiffrée à l'aide de la clé principale de base de données.  
  
 DECRYPTION BY PASSWORD **='***oldPassword***'**  
 Spécifie l'ancien mot de passe au moyen duquel la clé privée est actuellement protégée. Cette option n'est pas requise si la clé privée est chiffrée à l'aide de la clé principale de base de données.  
  
## <a name="remarks"></a>Notes   
 Si aucune clé principale de base de données n'existe, l'option ENCRYPTION BY PASSWORD est requise et l'opération échouera si aucun mot de passe n'est fourni. Pour plus d’informations sur la création d’une clé principale de base de données, consultez [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md).  
  
 Vous pouvez utiliser ALTER ASYMMETRIC KEY pour modifier la protection de la clé privée en spécifiant des options PRIVATE KEY, comme l'illustre le tableau ci-dessous.  
  
|Changer la protection|ENCRYPTION BY PASSWORD|DECRYPTION BY PASSWORD|  
|----------------------------|----------------------------|----------------------------|  
|De l'ancien mot de passe au nouveau mot de passe|Requis|Requis|  
|Du mot de passe à la clé principale|Omettre|Requis|  
|De la clé principale au mot de passe|Requis|Omettre|  
  
 La clé principale de base de données doit être ouverte pour pouvoir être utilisée pour protéger une clé privée. Pour plus d’informations, consultez [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md).  
  
 Pour modifier la propriété d’une clé asymétrique, utilisez [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur la clé asymétrique si la clé privée doit être supprimée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-password-of-the-private-key"></a>A. Modification du mot de passe de la clé privée  
 Dans l'exemple ci-dessous, le mot de passe utilisé pour protéger la clé privée de la clé asymétrique `PacificSales09` est modifié. Le nouveau mot de passe est `<enterStrongPasswordHere>`.  
  
```  
ALTER ASYMMETRIC KEY PacificSales09   
    WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<oldPassword>',  
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>');  
GO  
```  
  
### <a name="b-removing-the-private-key-from-an-asymmetric-key"></a>B. Suppression de la clé privée d'une clé asymétrique  
 Dans l'exemple ci-dessous, la clé privée est supprimée de `PacificSales19` et seule la clé publique demeure.  
  
```  
ALTER ASYMMETRIC KEY PacificSales19 REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="c-removing-password-protection-from-a-private-key"></a>C. Suppression de la protection par mot de passe d'une clé privée  
 Dans l'exemple ci-dessous, la protection par mot de passe d'une clé privée est supprimée et la clé est protégée au moyen de la clé principale de base de données.  
  
```  
OPEN MASTER KEY;  
ALTER ASYMMETRIC KEY PacificSales09 WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<enterStrongPasswordHere>' );  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md)   
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
