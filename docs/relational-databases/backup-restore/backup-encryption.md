---
title: Chiffrement de sauvegarde | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 334b95a8-6061-4fe0-9e34-b32c9f1706ce
caps.latest.revision: 18
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 4024cb96f79d8132dafb44046b084756d847c238
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="backup-encryption"></a>Chiffrement de sauvegarde
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique fournit une présentation des options de chiffrement pour les sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Elle contient des informations détaillées sur l'utilisation, les avantages et les méthodes recommandées de chiffrement pendant la sauvegarde.  
  
  
##  <a name="Overview"></a> Vue d'ensemble  
 À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], SQL Server a la possibilité de chiffrer les données lors de la création d'une sauvegarde. En spécifiant l'algorithme de chiffrement et le chiffreur (un certificat ou une clé asymétrique) lors de la création d'une sauvegarde, vous créez un fichier de sauvegarde chiffré. Toutes les destinations de stockage : le stockage local et Windows Azure sont pris en charge. Par ailleurs, les options de chiffrement peuvent être configurées pour les opérations de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] , une nouvelle fonctionnalité introduite dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Pour chiffrer pendant la sauvegarde, vous devez spécifier un algorithme de chiffrement, et un chiffreur pour sécuriser la clé de chiffrement. Les options de chiffrement suivantes sont prises en charge :  
  
-   **Algorithme de chiffrement :** Les algorithmes de chiffrement pris en charge sont : AES 128, AES 192, AES 256 et Triple DES  
  
-   **Encryptor :** certificat ou clé asymétrique.  
  
> [!CAUTION]  
>  Il est très important de sauvegarder le certificat ou la clé asymétrique, et de préférence dans un emplacement autre que le fichier de sauvegarde pour lequel il a été utilisé pour le chiffrement. Sans certificat ou clé asymétrique, vous ne pouvez pas restaurer la sauvegarde, ce qui rend le fichier de sauvegarde inutilisable.  
  
 **Restauration de la sauvegarde chiffrée :** Une restauration SQL Server ne nécessite pas de paramètres de chiffrement lors des restaurations. Elle nécessite que le certificat ou la clé asymétrique qui a servi à chiffrer le fichier de sauvegarde soit disponible sur l'instance sur laquelle vous effectuez la restauration. Le compte d'utilisateur qui effectue la restauration doit avoir l'autorisation **VIEW DEFINITION** sur le certificat ou la clé. Si vous restaurez la sauvegarde chiffrée dans une autre instance, vous devez vous assurer que le certificat est disponible sur cette instance.  
  
 Si vous restaurez une sauvegarde d'une base de données chiffrée par chiffrement transparent des données (TDE), le certificat de chiffrement transparent des données doit être disponible sur l'instance sur laquelle vous effectuez la restauration.  
  
##  <a name="Benefits"></a> Avantages  
  
1.  Le chiffrement des sauvegardes de base de données vous aide à protéger les données : SQL Server fournit l'option permettant de chiffrer les données de sauvegarde lors de la création d'une sauvegarde.  
  
2.  Le chiffrement sert également pour les bases de données qui sont chiffrées à l'aide du chiffrement transparent des données.  
  
3.  Le chiffrement est pris en charge pour les sauvegardes effectuées par [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)], qui procure une sécurité supplémentaire pour les sauvegardes hors site.  
  
4.  Cette fonctionnalité prend en charge plusieurs algorithmes de chiffrement jusqu'à AES 256 bits. Cela vous permet de sélectionner un algorithme qui s'aligne avec vos besoins.  
  
5.  Vous pouvez intégrer des clés de chiffrement avec les fournisseurs EKM (Gestion de clés extensible).  
  
  
##  <a name="Prerequisites"></a> Conditions préalables  
 Voici les conditions requises pour chiffrer une sauvegarde :  
  
1.  **Créez une clé principale de base de données pour la base de données master :** la clé principale de base de données est une clé symétrique qui permet de protéger les clés privées des certificats et des clés asymétriques présentes dans la base de données. Pour plus d’informations, consultez [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md).  
  
2.  Créez un certificat ou une clé asymétrique à utiliser pour le chiffrement de la sauvegarde. Pour plus d’informations sur la création d’un certificat, consultez [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md). Pour plus d’informations sur la création d’une clé asymétrique, consultez [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md).  
  
    > [!IMPORTANT]  
    >  Seules les clés asymétriques résidant dans la gestion de clés extensible (EKM) sont prises en charge.  
  
##  <a name="Restrictions"></a> Restrictions  
 Les restrictions suivantes s'appliquent aux options de chiffrement :  
  
-   Si vous utilisez une clé asymétrique pour chiffrer les données de sauvegarde, seules les clés asymétriques résidant dans le fournisseur EKM sont prises en charge.  
  
-   SQL Server Express et SQL Server Web ne prennent pas en charge le chiffrement pendant la sauvegarde. Cependant la restauration d'une sauvegarde chiffrée sur une instance de SQL Server Express ou SQL Server Web est prise en charge.  
  
-   Les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas lire les sauvegardes chiffrées.  
  
-   L'option Ajouter au jeu de sauvegarde existant n'est pas prise en charge pour les sauvegardes chiffrées.  
  
  
##  <a name="Permissions"></a> Permissions  
 **Pour chiffrer une sauvegarde ou restaurer à partir d'une sauvegarde chiffrée :**  
  
 **VIEW DEFINITION** sur le certificat ou la clé asymétrique utilisée pour chiffrer la sauvegarde de la base de données.  
  
> [!NOTE]  
>  L'accès au certificat de chiffrement transparent des données n'est pas nécessaire pour la sauvegarde ou la restauration d'une base de données protégée par chiffrement transparent des données.  
  
##  <a name="Methods"></a> Méthodes de chiffrement des sauvegardes  
 Les sections ci-dessous fournissent une brève introduction aux étapes de chiffrement des données pendant la sauvegarde. Pour une procédure pas à pas complète des différentes étapes de chiffrement de votre sauvegarde à l’aide de Transact-SQL, consultez [Créer une sauvegarde chiffrée](../../relational-databases/backup-restore/create-an-encrypted-backup.md).  
  
### <a name="using-sql-server-management-studio"></a>Utilisation de SQL Server Management Studio  
 Chiffrez une sauvegarde lors de la création de la sauvegarde d'une base de données dans l'une des boîtes de dialogue suivantes :  
  
1.  [Sauvegarder la base de données &#40;page Options de sauvegarde&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md) Dans la page **Options de sauvegarde**, sélectionnez **Chiffrement**, ainsi que l’algorithme de chiffrement et le certificat ou la clé asymétrique à utiliser pour le chiffrement.  
  
2.  [Utilisation de l’Assistant Plan de maintenance](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure) Lorsque vous sélectionnez une tâche de sauvegarde, sous l’onglet **Options** de la page **Définir la tâche Sauvegarder**, sélectionnez **Chiffrement de sauvegarde**, puis indiquez l’algorithme de chiffrement et le certificat ou la clé à utiliser pour le chiffrement.  
  
### <a name="using-transact-sql"></a>Utilisation de Transact-SQL  
 Voici un exemple d'instruction Transact-SQL pour chiffrer le fichier de sauvegarde :  
  
```sql  
BACKUP DATABASE [MYTestDB]  
TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MyTestDB.bak'  
WITH  
  COMPRESSION,  
  ENCRYPTION   
   (  
   ALGORITHM = AES_256,  
   SERVER CERTIFICATE = BackupEncryptCert  
   ),  
  STATS = 10  
GO  
  
```  
  
 Pour connaître la syntaxe complète de l’instruction Transact-SQL, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
### <a name="using-powershell"></a>Utilisation de PowerShell  
 Cet exemple crée les options de chiffrement et les utilise en tant que valeur de paramètre dans **Backup-SqlDatabase** pour créer une sauvegarde chiffrée.  
  
```  
C:\PS>$encryptionOption = New-SqlBackupEncryptionOption -Algorithm Aes256 -EncryptorType ServerCertificate -EncryptorName "BackupCert"  
```  
  
```  
C:\PS>Backup-SqlDatabase -ServerInstance . -Database "MyTestDB" -BackupFile "MyTestDB.bak" -CompressionOption On -EncryptionOption $encryptionOption  
```  
  
##  <a name="RecommendedPractices"></a> Méthodes recommandées  
 Créez une sauvegarde du certificat de chiffrement et des clés dans un emplacement autre que votre ordinateur local sur lequel est installée l'instance. Pour prendre en compte les scénarios de récupération d'urgence, envisagez de stocker une sauvegarde du certificat ou de la clé dans un emplacement hors site. Vous ne pouvez pas restaurer une sauvegarde chiffrée sans le certificat utilisé pour chiffrer la sauvegarde.  
  
 Pour restaurer une sauvegarde chiffrée, le certificat d'origine utilisé lorsque la sauvegarde a été effectuée avec l'empreinte numérique correspondante doit être disponible sur l'instance sur laquelle vous restaurez. Par conséquent, le certificat ne doit pas être renouvelé à expiration ou être modifié de quelque manière que ce soit. Le renouvellement peut entraîner la mise à jour du certificat et déclencher la modification de l'empreinte numérique, ce qui rend donc le certificat non valide pour le fichier de sauvegarde. Le compte qui effectue la restauration doit disposer des autorisations VIEW DEFINITION sur le certificat ou la clé asymétrique utilisée pour chiffrer pendant la sauvegarde.  
  
 Les sauvegardes de base de données du groupe de disponibilité sont généralement effectuées sur le réplica de sauvegarde par défaut.  Si vous restaurez une sauvegarde sur un réplica autre que celui où la sauvegarde a été effectuée, vérifiez que le certificat d'origine utilisé pour la sauvegarde est disponible sur le réplica vers lequel vous restaurez.  
  
 Si la base de données est activée pour le chiffrement transparent des données, sélectionnez différents certificats ou clés asymétriques pour chiffrer la base de données et la sauvegarde afin de renforcer la sécurité.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
|Rubrique/tâche|Description|  
|-----------------|-----------------|  
|[Créer une sauvegarde chiffrée](../../relational-databases/backup-restore/create-an-encrypted-backup.md)|Décrit les étapes de base requises pour créer une sauvegarde chiffrée.|  
|[Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)|Fournit un exemple de création d'une sauvegarde chiffrée protégée par des clés dans le coffre de clés Azure.|  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)  
  
  
