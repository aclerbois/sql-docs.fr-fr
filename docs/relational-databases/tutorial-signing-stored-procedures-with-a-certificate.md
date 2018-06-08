---
title: 'Didacticiel : signature de procédures stockées à l’aide d’un certificat | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
helpviewer_keywords:
- signing stored procedures tutorial [SQL Server]
ms.assetid: a4b0f23b-bdc8-425f-b0b9-e0621894f47e
caps.latest.revision: 11
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: b0cfd18b508f13499ce5cf7bdf4cc12be4440562
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-signing-stored-procedures-with-a-certificate"></a>Didacticiel : signature de procédures stockées à l'aide d'un certificat
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Ce didacticiel explique comment signer des procédures stockées à l'aide d'un certificat généré par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> Pour exécuter le code de ce didacticiel, vous devez à la fois configurer la sécurité en mode mixte et installer la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] . Scénario  
  
La signature de procédures stockées à l'aide d'un certificat est utile lorsque vous voulez définir des autorisations pour la procédure stockée mais ne souhaitez pas accorder explicitement ces droits à un utilisateur. Bien que d'autres méthodes vous permettent d'accomplir cette tâche, notamment en faisant appel à l'instruction EXECUTE AS, le recours à un certificat vous permet d'utiliser une trace afin de rechercher l'appelant d'origine de la procédure stockée. Cette fonction garantit un degré d'audit élevé, surtout lors d'opérations de sécurité ou d'opérations DDL (Data Definition Language).  
  
Vous pouvez créer un certificat dans la base de données master pour permettre l'usage d'autorisations de niveau serveur ou bien créer un certificat dans n'importe quelle base de données pour des autorisations de niveau base de données. Dans ce scénario, un utilisateur ne bénéficiant d'aucun droit pour les tables de base doit avoir accès à une procédure stockée de la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] et vous souhaitez soumettre le journal d'accès aux objets à un audit. Plutôt que d'utiliser d'autres méthodes de chaînes de propriétés, vous allez créer un compte d'utilisateur de serveur et de base de données sans droits d'accès aux objets de base, puis un compte d'utilisateur de base de données doté de droits d'accès à une table et à une procédure stockée. La procédure stockée et le deuxième compte d'utilisateur de base de données seront tous les deux sécurisés à l'aide d'un certificat. Le deuxième compte de base de données aura accès à tous les objets et un accès à la procédure stockée sera accordé au premier compte d'utilisateur de base de données.  
  
Dans ce scénario, vous allez d'abord créer un certificat de base de données, une procédure stockée et un utilisateur, puis vous testerez le processus en suivant ces étapes :  
  
1.  Configuration de l'environnement  
  
2.  Création d'un certificat  
  
3.  Création et signature d'une procédure stockée à l'aide du certificat  
  
4.  Création d'un compte de certificat à l'aide du certificat  
  
5.  Octroi de droits de base de données au compte de certificat  
  
6.  Affichage du contexte d'accès  
  
7.  Réinitialisation de l'environnement  
  
Chaque bloc de code dans cet exemple est présenté sous forme de lignes. Pour copier l'exemple tout entier, consultez la section [Exemple complet](#CompleteExample) à la fin de ce didacticiel.  
  
## <a name="1-configure-the-environment"></a>1. Configurez l'environnement  
Pour définir le contexte initial de l'exemple, ouvrez une nouvelle requête dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] et exécutez le code suivant pour ouvrir la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)]. Ce code modifie et redéfinit le contexte de la base de données à `AdventureWorks2012` , puis crée une nouvelle connexion serveur et un compte d'utilisateur de base de données (`TestCreditRatingUser`) avec un mot de passe.  
  
```  
USE AdventureWorks2012;  
GO  
-- Set up a login for the test user  
CREATE LOGIN TestCreditRatingUser  
   WITH PASSWORD = 'ASDECd2439587y'  
GO  
CREATE USER TestCreditRatingUser  
FOR LOGIN TestCreditRatingUser;  
GO  
```  
  
Pour plus d’informations sur l’instruction CREATE USER, consultez [CREATE USER &#40;Transact-SQL&#41;](../t-sql/statements/create-user-transact-sql.md). Pour plus d’informations sur l’instruction CREATE LOGIN, consultez [CREATE LOGIN &#40;Transact-SQL&#41;](../t-sql/statements/create-login-transact-sql.md).  
  
## <a name="2-create-a-certificate"></a>2. Créez un certificat  
Vous pouvez créer des certificats sur le serveur à l'aide de la base de données master comme contexte, d'une base de données d'utilisateur ou bien des deux. Il existe plusieurs options pour sécuriser le certificat. Pour plus d’informations sur les certificats, consultez [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../t-sql/statements/create-certificate-transact-sql.md).  
  
Exécutez ce code pour créer un certificat de base de données et sécurisez-le avec un mot de passe.  
  
```  
CREATE CERTIFICATE TestCreditRatingCer  
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
      WITH SUBJECT = 'Credit Rating Records Access',   
      EXPIRY_DATE = '12/05/2014';  
GO  
```  
  
## <a name="3-create-and-sign-a-stored-procedure-using-the-certificate"></a>3. Créez et signez une procédure stockée à l'aide du certificat  
Utilisez le code suivant pour créer une procédure stockée qui sélectionne des données de la table `Vendor` dans le schéma de la base de données `Purchasing`, ce qui limite l'accès uniquement aux sociétés qui ont une cote de solvabilité de 1. Notez que la première section de la procédure stockée affiche le contexte du compte d'utilisateur qui exécute la procédure stockée, à des fins de démonstration des concepts uniquement. Il n'est pas nécessaire pour répondre aux conditions requises.  
  
```  
CREATE PROCEDURE TestCreditRatingSP  
AS  
BEGIN  
   -- Show who is running the stored procedure  
   SELECT SYSTEM_USER 'system Login'  
   , USER AS 'Database Login'  
   , NAME AS 'Context'  
   , TYPE  
   , USAGE   
   FROM sys.user_token     
  
   -- Now get the data  
   SELECT AccountNumber, Name, CreditRating   
   FROM Purchasing.Vendor  
   WHERE CreditRating = 1  
END  
GO  
```  
  
Exécutez ce code pour signer la procédure stockée avec le certificat de base de données en utilisant un mot de passe.  
  
```  
ADD SIGNATURE TO TestCreditRatingSP   
   BY CERTIFICATE TestCreditRatingCer  
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y';  
GO  
```  
  
Pour plus d’informations sur les procédures stockées, consultez [Procédures stockées &#40;Moteur de base de données&#41;](../relational-databases/stored-procedures/stored-procedures-database-engine.md).  
  
Pour plus d’informations sur la signature de procédures stockées, consultez [ADD SIGNATURE &#40;Transact-SQL&#41;](../t-sql/statements/add-signature-transact-sql.md).  
  
## <a name="4-create-a-certificate-account-using-the-certificate"></a>4. Créez un compte de certificat à l'aide du certificat  
Exécutez ce code pour créer un utilisateur de base de données (`TestCreditRatingcertificateAccount`) à partir du certificat. Ce compte ne dispose pas d'une connexion serveur et doit en fin de compte contrôler l'accès aux tables sous-jacentes.  
  
```  
USE AdventureWorks2012;  
GO  
CREATE USER TestCreditRatingcertificateAccount  
   FROM CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
## <a name="5-grant-the-certificate-account-database-rights"></a>5. Accordez des droits de base de données au compte de certificat  
Exécutez ce code pour accorder des droits `TestCreditRatingcertificateAccount` à la table de base et à la procédure stockée.  
  
```  
GRANT SELECT   
   ON Purchasing.Vendor   
   TO TestCreditRatingcertificateAccount;  
GO  
  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingcertificateAccount;  
GO  
```  
  
Pour plus d’informations sur l’accord d’autorisations à des objets, consultez [GRANT &#40;Transact-SQL&#41;](../t-sql/statements/grant-transact-sql.md).  
  
## <a name="6-display-the-access-context"></a>6. Affichez le contexte d'accès  
Pour afficher les droits associés à l'accès de la procédure stockée, exécutez le code suivant pour accorder des droits d'exécution de la procédure stockée à l'utilisateur `TestCreditRatingUser`.  
  
```  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingUser;  
GO  
```  
  
Ensuite, exécutez le code ci-dessous pour exécuter la procédure stockée en tant que connexion dbo utilisée sur le serveur. Observez la sortie des informations de contexte utilisateur. Il affiche le compte dbo en tant que contexte avec ses propres droits et non par le biais d'une appartenance à un groupe.  
  
```  
EXECUTE TestCreditRatingSP;  
GO  
```  
  
Exécutez le code suivant pour utiliser l'instruction `EXECUTE AS` en tant que compte `TestCreditRatingUser` et exécutez la procédure stockée. Cette fois, le contexte utilisateur apparaît défini sur le contexte USER MAPPED TO CERTIFICATE.  
  
```  
EXECUTE AS LOGIN = 'TestCreditRatingUser';  
GO  
EXECUTE TestCreditRatingSP;  
GO  
```  
  
Ce résultat vous montre l'audit disponible puisque vous avez signé la procédure stockée.  
  
> [!NOTE]  
> Utilisez EXECUTE AS pour basculer entre les contextes au sein d'une base de données.  
  
## <a name="7-reset-the-environment"></a>7. Réinitialisez l'environnement  
Le code ci-dessous utilise l'instruction `REVERT` pour retourner le contexte du compte actuel à dbo, puis réinitialise l'environnement.  
  
```  
REVERT;  
GO  
DROP PROCEDURE TestCreditRatingSP;  
GO  
DROP USER TestCreditRatingcertificateAccount;  
GO  
DROP USER TestCreditRatingUser;  
GO  
DROP LOGIN TestCreditRatingUser;  
GO  
DROP CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
Pour plus d’informations sur l’instruction REVERT, consultez [REVERT &#40;Transact-SQL&#41;](../t-sql/statements/revert-transact-sql.md).  
  
## <a name="CompleteExample"></a>Exemple complet  
Cette section affiche l'exemple de code dans son intégralité.  
  
```  
/* Step 1 - Open the AdventureWorks2012 database */  
USE AdventureWorks2012;  
GO  
-- Set up a login for the test user  
CREATE LOGIN TestCreditRatingUser  
   WITH PASSWORD = 'ASDECd2439587y'  
GO  
CREATE USER TestCreditRatingUser  
FOR LOGIN TestCreditRatingUser;  
GO  
  
/* Step 2 - Create a certificate in the AdventureWorks2012 database */  
CREATE CERTIFICATE TestCreditRatingCer  
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
      WITH SUBJECT = 'Credit Rating Records Access',   
      EXPIRY_DATE = '12/05/2014';  
GO  
  
/* Step 3 - Create a stored procedure and  
sign it using the certificate */  
CREATE PROCEDURE TestCreditRatingSP  
AS  
BEGIN  
   -- Shows who is running the stored procedure  
   SELECT SYSTEM_USER 'system Login'  
   , USER AS 'Database Login'  
   , NAME AS 'Context'  
   , TYPE  
   , USAGE   
   FROM sys.user_token;     
  
   -- Now get the data  
   SELECT AccountNumber, Name, CreditRating   
   FROM Purchasing.Vendor  
   WHERE CreditRating = 1;  
END  
GO  
  
ADD SIGNATURE TO TestCreditRatingSP   
   BY CERTIFICATE TestCreditRatingCer  
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y';  
GO  
  
/* Step 4 - Create a database user for the certificate.   
This user has the ownership chain associated with it. */  
USE AdventureWorks2012;  
GO  
CREATE USER TestCreditRatingcertificateAccount  
   FROM CERTIFICATE TestCreditRatingCer;  
GO  
  
/* Step 5 - Grant the user database rights */  
GRANT SELECT   
   ON Purchasing.Vendor   
   TO TestCreditRatingcertificateAccount;  
GO  
  
GRANT EXECUTE  
   ON TestCreditRatingSP   
   TO TestCreditRatingcertificateAccount;  
GO  
  
/* Step 6 - Test, using the EXECUTE AS statement */  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingUser;  
GO  
  
-- Run the procedure as the dbo user, notice the output for the type  
EXEC TestCreditRatingSP;  
GO  
  
EXECUTE AS LOGIN = 'TestCreditRatingUser';  
GO  
EXEC TestCreditRatingSP;  
GO  
  
/* Step 7 - Clean up the example */  
REVERT;  
GO  
DROP PROCEDURE TestCreditRatingSP;  
GO  
DROP USER TestCreditRatingcertificateAccount;  
GO  
DROP USER TestCreditRatingUser;  
GO  
DROP LOGIN TestCreditRatingUser;  
GO  
DROP CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[Centre de sécurité pour le moteur de base de données SQL Server et la base de données SQL Azure](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
  
