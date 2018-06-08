---
title: Créer des clés symétriques identiques sur deux serveurs | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- symmetric keys [SQL Server], creating
ms.assetid: a13d0b21-a43b-43c0-9c22-7ba8f3d15e80
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2739da6e51a378308287bdd21ea06a9a6b966697
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-identical-symmetric-keys-on-two-servers"></a>Créer des clés symétriques identiques sur deux serveurs
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique décrit comment créer des clés symétriques identiques sur deux serveurs différents dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Pour pouvoir déchiffrer du texte chiffré, vous devez posséder la clé qui a été utilisée pour le chiffrer. Quand le chiffrement et le déchiffrement interviennent dans une seule base de données, la clé est stockée dans la base de données et demeure disponible, en fonction des autorisations, pour le chiffrement et le déchiffrement. Cependant, lorsque le chiffrement et le déchiffrement se produisent dans des bases de données distinctes ou sur des serveurs séparés, la clé stockée dans une base de données ne peut pas être utilisée dans l'autre base de données.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   [Pour créer des clés symétriques identiques sur deux serveurs différents, à l'aide de Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Lorsqu'une clé symétrique est créée, elle doit être chiffrée à l'aide de l'un des éléments suivants au moins : certificat, mot de passe, clé symétrique, clé asymétrique ou PROVIDER. La clé peut être soumise à plusieurs chiffrements de chaque type. En d'autres termes, une clé symétrique unique peut être chiffrée à l'aide de plusieurs certificats, mots de passe, clés symétriques et clés asymétriques à la fois.  
  
-   Lorsqu'une clé symétrique est chiffrée à l'aide d'un mot de passe à la place de la clé publique de la clé principale de base de données, l'algorithme de chiffrement TRIPLE_DES est utilisé. Pour cette raison, les clés créées à l'aide d'un algorithme de chiffrement renforcé, tel qu'AES, sont elles-mêmes sécurisées par un algorithme plus faible.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Requiert l'autorisation ALTER ANY SYMMETRIC KEY sur la base de données. Si la clause AUTHORIZATION est spécifiée, l'autorisation IMPERSONATE sur l'utilisateur de base de données ou l'autorisation ALTER sur le rôle d'application est requise. Si le chiffrement s'effectue par certificat ou clé asymétrique, l'autorisation VIEW DEFINITION est requise sur le certificat ou la clé asymétrique. Les connexions Windows, les connexions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et les rôles d'application sont les seuls à pouvoir posséder des clés symétriques. Les groupes et les rôles ne peuvent pas posséder de clés symétriques.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-identical-symmetric-keys-on-two-different-servers"></a>Pour créer des clés symétriques identiques sur deux serveurs différents  
  
1.  Dans l'**Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Créez une clé en exécutant les instructions CREATE MASTER KEY, CREATE CERTIFICATE et CREATE SYMMETRIC KEY suivantes.  
  
    ```  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'My p@55w0Rd';  
    GO  
    CREATE CERTIFICATE [cert_keyProtection] WITH SUBJECT = 'Key Protection';  
    GO  
    CREATE SYMMETRIC KEY [key_DataShare] WITH  
        KEY_SOURCE = 'My key generation bits. This is a shared secret!',  
        ALGORITHM = AES_256,   
        IDENTITY_VALUE = 'Key Identity generation bits. Also a shared secret'  
        ENCRYPTION BY CERTIFICATE [cert_keyProtection];  
    GO  
    ```  
  
4.  Connectez-vous à une instance de serveur distincte, ouvrez une fenêtre de requête différente et exécutez les instructions SQL ci-dessus pour créer la même clé sur le deuxième serveur.  
  
5.  Testez les clés en exécutant d'abord les instructions OPEN SYMMETRIC KEY et SELECT ci-dessous sur le premier serveur.  
  
    ```  
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    SELECT encryptbykey(key_guid('key_DataShare'), 'MyData' )  
    GO  
    -- For example, the output might look like this: 0x2152F8DA8A500A9EDC2FAE26D15C302DA70D25563DAE7D5D1102E3056CE9EF95CA3E7289F7F4D0523ED0376B155FE9C3  
    ```  
  
6.  Sur le second serveur, collez le résultat de l'instruction SELECT précédente dans le code suivant comme valeur de `@blob` et exécutez le code suivant pour vérifier que la clé dupliquée peut déchiffrer le texte chiffré.  
  
    ```  
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    DECLARE @blob varbinary(8000);  
    SET @blob = SELECT CONVERT(varchar(8000), decryptbykey(@blob));  
    GO  
    ```  
  
7.  Fermez les clés symétriques sur les deux serveurs.  
  
    ```  
    CLOSE SYMMETRIC KEY [key_DataShare];  
    GO  
    ```  
  
 Pour plus d'informations, consultez les documents suivants :  
  
-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbykey-transact-sql.md)  
  
-   [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/decryptbykey-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
  
  
