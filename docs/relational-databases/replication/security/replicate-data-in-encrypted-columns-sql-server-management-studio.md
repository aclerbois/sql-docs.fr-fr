---
title: Répliquer des données dans des colonnes chiffrées (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], replicating data
- encryption [SQL Server replication]
- publishing [SQL Server replication], encrypted columns
ms.assetid: d1f8f586-e5a3-4a71-9391-11198d42bfa3
caps.latest.revision: 8
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9960c196d08cc38f376bad003a30cc4a7791ca08
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replicate-data-in-encrypted-columns-sql-server-management-studio"></a>Répliquer des données dans des colonnes chiffrées (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication permet de publier des données de colonnes chiffrées. Pour déchiffrer et exploiter ces données sur l'Abonné, la clé employée pour chiffrer les données sur le serveur de publication doit également être présente sur l'Abonné. La réplication n'offre pas un mécanisme sécurisé de transport des clés de chiffrement. Vous devez manuellement recréer la clé de chiffrement sur l'Abonné. Cette rubrique explique comment chiffrer une colonne sur le serveur de publication et vous assurer que la clé de chiffrement est disponible au niveau de l'Abonné.  
  
 Les étapes de base sont les suivantes :  
  
1.  Créez la clé symétrique sur le serveur de publication.  
  
2.  Chiffrez les données de la colonne avec la clé symétrique.  
  
3.  Publiez la table avec la colonne chiffrée.  
  
4.  Abonnez-vous à la publication.  
  
5.  Initialisez l'abonnement.  
  
6.  Recréez la clé symétrique sur l'Abonné en utilisant les mêmes valeurs ALGORITHM, KEY_SOURCE et IDENTITY_VALUE qu'à l'étape 1.  
  
7.  Accédez aux données chiffrées de la colonne.  
  
> [!NOTE]  
>  Il est préférable que vous utilisiez une clé symétrique pour chiffrer les données de la colonne. La clé symétrique elle-même peut être sécurisée selon différents moyens sur le serveur de publication et sur l'Abonné.  
  
### <a name="to-create-and-replicate-encrypted-column-data"></a>Pour créer et répliquer les données chiffrées de la colonne  
  
1.  Sur le serveur de publication, exécutez [CREATE SYMMETRIC KEY](../../../t-sql/statements/create-symmetric-key-transact-sql.md).  
  
    > [!IMPORTANT]  
    >  La valeur KEY_SOURCE est un ensemble de données précieuses qu'il est possible d'utiliser pour recréer la clé symétrique et déchiffrer des données. La valeur KEY_SOURCE doit toujours être stockée et transportée de manière sécurisée.  
  
2.  Exécutez [OPEN SYMMETRIC KEY](../../../t-sql/statements/open-symmetric-key-transact-sql.md) pour ouvrir la nouvelle clé.  
  
3.  Utilisez la fonction [EncryptByKey](../../../t-sql/functions/encryptbykey-transact-sql.md) pour chiffrer les données de la colonne sur le serveur de publication.  
  
4.  Exécutez [CLOSE SYMMETRIC KEY](../../../t-sql/statements/close-symmetric-key-transact-sql.md) pour fermer la clé.  
  
5.  Publiez la table contenant la colonne chiffrée. Pour plus d’informations, consultez [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
6.  Abonnez-vous à la publication. Pour plus d’informations, consultez [Créer un abonnement par extraction de données (pull)](../../../relational-databases/replication/create-a-pull-subscription.md) ou [Créer un abonnement par émission (push)](../../../relational-databases/replication/create-a-push-subscription.md).  
  
7.  Initialisez l'abonnement. Pour plus d’informations, consultez [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
8.  Sur l'abonné, exécutez [CREATE SYMMETRIC KEY](../../../t-sql/statements/create-symmetric-key-transact-sql.md) en utilisant les mêmes valeurs pour ALGORITHM, KEY_SOURCE et IDENTITY_VALUE qu'à l'étape 1. Vous pouvez spécifier une valeur différente pour la clause ENCRYPTION BY.  
  
    > [!IMPORTANT]  
    >  La valeur KEY_SOURCE est un ensemble de données précieuses qu'il est possible d'utiliser pour recréer la clé symétrique et déchiffrer des données. La valeur KEY_SOURCE doit toujours être stockée et transportée de manière sécurisée.  
  
9. Exécutez [OPEN SYMMETRIC KEY](../../../t-sql/statements/open-symmetric-key-transact-sql.md) pour ouvrir la nouvelle clé.  
  
10. Utilisez la fonction [DecryptByKey](../../../t-sql/functions/decryptbykey-transact-sql.md) pour déchiffrer les données répliquées sur l'Abonné.  
  
11. Exécutez [CLOSE SYMMETRIC KEY](../../../t-sql/statements/close-symmetric-key-transact-sql.md) pour fermer la clé.  
  
## <a name="example"></a> Exemple  
 Cet exemple crée une clé symétrique, un certificat servant à sécuriser la clé symétrique et une clé principale. Ces clés sont créées dans la base de données de publication. Elles sont ensuite utilisées pour créer une colonne chiffrée (EncryptedCreditCardApprovalCode) dans la table `SalesOrderHeader` . Cette colonne est publiée dans la publication AdvWorksSalesOrdersMerge au lieu de la colonne non chiffrée CreditCardApprovalCode. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [!code-sql[HowTo#sp_PublishEncryptedColumn](../../../relational-databases/replication/codesnippet/tsql/replicate-data-in-encryp_1.sql)]  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/replicate-data-in-encryp_2.sql)]  
  
## <a name="example"></a> Exemple  
 Cet exemple recrée la même clé symétrique dans la base de données d'abonnement en utilisant les mêmes valeurs ALGORITHM, KEY_SOURCE et IDENTITY_VALUE que dans le premier exemple. Cet exemple part du principe que vous avez déjà initialisé un abonnement dans la publication AdvWorksSalesOrdersMerge dans le but de répliquer la colonne chiffrée. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher tout accès non autorisé.  
  
 [!code-sql[HowTo#sp_SubscriberEncryptedColumn](../../../relational-databases/replication/codesnippet/tsql/replicate-data-in-encryp_3.sql)]  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble de la sécurité &#40;réplication&#41;](../../../relational-databases/replication/security/security-overview-replication.md)   
 [Créer des clés symétriques identiques sur deux serveurs](../../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
  
