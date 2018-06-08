---
title: Assistant Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 05/04/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.alwaysencryptedwizard.encryption.f1
- sql13.swb.alwaysencryptedwizard.f1
- sql.swb.alwaysencryptedwizard.masterkey.f1
helpviewer_keywords:
- Wizard, Always Encrypted
ms.assetid: 68daddc9-ce48-49aa-917f-6dec86ad5af5
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: cd3233b7c6ffc70b0fad8ce71ace6501f79d371a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="always-encrypted-wizard"></a>Assistant Always Encrypted
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Utilisez l’ **Assistant Always Encrypted** pour protéger les données sensibles stockées dans une base de données SQL Server. Le Chiffrement intégral permet aux clients de chiffrer des données sensibles dans des applications clientes et de ne jamais révéler les clés de chiffrement à SQL Server. Ainsi, Always Encrypted fournit une séparation entre ceux qui possèdent les données (et qui peuvent les afficher) et ceux qui les gèrent (mais qui ne doivent pas y avoir accès).  Pour obtenir la description complète de la fonctionnalité, consultez [Always Encrypted &#40;moteur de base de données&#41;](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
 
 - Pour une procédure détaillée montrant comment configurer la fonctionnalité Always Encrypted avec l’Assistant et comment l’utiliser dans une application cliente, consultez [Didacticiel sur les bases de données SQL : Protéger les données à l’aide d’Always Encrypted](https://azure.microsoft.com/documentation/articles/sql-database-always-encrypted/).  
 
 - Consultez [Prise en main d’Always Encrypted avec SSMS](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted)pour regarder une vidéo qui traite de l’utilisation de l’Assistant. Consultez également le blog de l’équipe de sécurité [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [Assistant de chiffrement SSMS - Activer Always Encrypted en quelques étapes](http://blogs.msdn.com/b/sqlsecurity/archive/2015/11/01/ssms-encryption-wizard-enabling-always-encrypted-made-easy.aspx).  
 
 - **Autorisations :** pour interroger des colonnes chiffrées et sélectionner des clés à l’aide de cet Assistant, vous devez disposer des autorisations `VIEW ANY COLUMN MASTER KEY DEFINITION` et `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` . Pour créer des clés, vous devez également disposer des autorisations `ALTER ANY COLUMN MASTER KEY` et `ALTER ANY COLUMN ENCRYPTION KEY` .  
 
 #### <a name="to-open-the-always-encrypted-wizard"></a>Pour ouvrir l’Assistant Always Encrypted  
 
 1.  Connectez-vous à votre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le composant Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
   
 2.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, puis sélectionnez **Chiffrer les colonnes**.  
   
 ## <a name="column-selection-page"></a>Page de sélection de la colonne  
 - Recherchez une table et une colonne, puis sélectionnez un type de chiffrement (déterministe ou aléatoire) et une clé de chiffrement pour les colonnes sélectionnées. Pour déchiffrer une colonne chiffrée, sélectionnez **Texte en clair**. Pour modifier une clé de chiffrement de colonne, sélectionnez une clé de chiffrement différente. L’Assistant déchiffrera la colonne et la chiffrera à nouveau avec la nouvelle clé. (Le chiffrement de tables temporelles et en mémoire est pris en charge par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mais ne peut pas être configuré par cet Assistant.)  
 
## <a name="master-key-configuration-page"></a>Page de configuration de la clé principale  
 - Créez une nouvelle clé principale de colonne dans le magasin de certificats Windows ou un coffre de clés Azure. Pour plus d’informations, consultez les liens répertoriés sous Stockage de la clé.  
 
 - Si vous avez choisi une clé de chiffrement de colonne générée automatiquement dans la page de sélection de la colonne, vous devez configurer une clé principale de colonne avec laquelle la clé de chiffrement de colonne générée sera chiffrée. Si vous disposez déjà d’une clé principale de colonne définie dans votre base de données, vous pouvez la sélectionner. (Pour utiliser une clé principale de colonne existante, l’utilisateur doit avoir l’autorisation d’accéder à la clé.) Vous pouvez également générer une clé principale de colonne dans un magasin de clés sélectionné (magasin de certificats Windows ou coffre de clés Azure) et définir la clé dans la base de données.  
 
 ### <a name="key-storage"></a>**Stockage de la clé**  
 
 - Choisissez l’emplacement de stockage de la clé principale de colonne.  
 
   - **Stockage d’une clé principale dans le magasin de certificats Windows** Pour plus d’informations, consultez [Utilisation des magasins de certificats](https://msdn.microsoft.com/library/windows/desktop/aa388160.aspx)  
 
   - **Stockage d’une clé principale dans AKV** Pour plus d’informations, consultez [Prise en main du coffre de clés Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).  
 
 - Pour générer une clé principale de colonne dans le coffre de clés Azure, l’utilisateur doit disposer des autorisations **WrapKey**, **UnwrapKey**, **Verify**et **Sign** dans le coffre de clés. Il est possible que les utilisateurs aient également besoin des autorisations **Get**, **List**, **Create**, **Delete**, **Update**, **Import**, **Backup**et **Restore** . Pour plus d’informations, consultez [Qu’est-ce que le coffre de clés Azure ?](https://azure.microsoft.com/documentation/articles/key-vault-whatis/) et   [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).  
 
 - L’Assistant ne prend en charge que deux options. Les magasins clients et les modules de sécurité matérielle doivent être configurés à l’aide de [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)].  
 
 ## <a name="always-encrypted-terms"></a>Termes relatifs à Always Encrypted  
 
 - Le**chiffrement déterministe** utilise une méthode qui génère toujours la même valeur chiffrée pour une valeur de texte brut donnée. L’utilisation du chiffrement déterministe permet les regroupements, le filtrage par égalité et les jointures de tables sur la base de valeurs chiffrées. Il peut par contre aussi permettre à des utilisateurs non autorisés de deviner des informations sur les valeurs chiffrées en examinant les séquences dans la colonne chiffrée. Cette faille est accrue si l’ensemble de valeurs chiffrées possibles est restreint, par exemple Vrai/Faux ou Région Nord/Sud/Est/Ouest. Le chiffrement déterministe doit utiliser un classement de colonne avec un ordre de tri binaire 2 pour les colonnes de type caractère.  
 
 - Le**chiffrement aléatoire** utilise une méthode qui chiffre les données de manière moins prévisible. Le chiffrement aléatoire est plus sécurisé, mais il empêche les recherches d’égalité, le regroupement, l’indexation et la jointure sur des colonnes chiffrées.  

 - Les**clés principales de colonne** sont des clés de protection utilisées pour chiffrer les clés de chiffrement de colonne. Les clés principales de colonne doivent être stockées dans un magasin de clés approuvé. Les informations sur les clés principales de colonne, y compris sur leur emplacement, sont stockées dans la base de données au sein des affichages catalogue système.  

 - Les**clés de chiffrement de colonne** servent à chiffrer les données sensibles stockées dans des colonnes de base de données. Toutes les valeurs d’une colonne peuvent être chiffrées à l’aide d’une clé de chiffrement de colonne unique. Les valeurs chiffrées des clés de chiffrement de colonne sont stockées dans la base de données au sein des affichages catalogue système. Vous devez stocker les clés de chiffrement de colonne dans un emplacement sécurisé/approuvé pour la sauvegarde.  

 ## <a name="see-also"></a> Voir aussi  
 - [Always Encrypted &#40;moteur de base de données&#41;](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 - [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
