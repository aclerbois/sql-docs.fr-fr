---
title: TDE - BYOK (Bring Your Own Key) - Azure SQL | Microsoft Docs
description: Prise en charge du service BYOK (Bring Your Own Key) pour Transparent Data Encryption (TDE) avec Azure Key Vault pour SQL Database et Data Warehouse. Présentation de TDE avec BYOK, avantages, fonctionnement, éléments à prendre en compte et recommandations.
keywords: ''
services: sql-database
documentationcenter: ''
author: aliceku
manager: craigg
ms.prod: ''
ms.reviewer: ''
ms.suite: sql
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.custom: ''
ms.component: security
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: aliceku
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 2265778ca41dd82a1e55fe01749bd2d5057f5f1c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transparent-data-encryption-with-bring-your-own-key-support-for-azure-sql-database-and-data-warehouse"></a>Transparent Data Encryption avec prise en charge de BYOK pour Azure SQL Database et Data Warehouse
[!INCLUDE[appliesto-xx-asdb-asdw-xxx-md](../../../includes/appliesto-xx-asdb-asdw-xxx-md.md)]

La prise en charge de BYOK (Bring Your Own Key) pour [TDE (Transparent Data Encryption)](transparent-data-encryption.md) vous permet de chiffrer la clé de chiffrement de la base de données (DEK) avec une clé asymétrique appelée protecteur TDE.  Le protecteur TDE est stocké sous votre contrôle dans [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), système de gestion de clés externes basé sur le cloud Azure. Azure Key Vault est le premier service de gestion de clés auquel TDE a intégré la prise en charge de BYOK. La clé DEK TDE, stockée dans la page de démarrage de la base de données, est chiffrée et déchiffrée par le protecteur TDE. Le protecteur TDE est stocké dans Azure Key Vault et ne quitte jamais le coffre de clés. Si l’accès du serveur au coffre de clés est révoqué, la base de données ne peut pas être déchiffrée et lue en mémoire.  Le protecteur TDE est défini au niveau du serveur logique et est hérité de toutes les bases de données associées à ce serveur. 

Avec la prise en charge de BYOK, les utilisateurs peuvent maintenant gérer diverses tâches de gestion des clés, notamment les rotations de clés, les autorisations de coffre de clés, la suppression de clés, ainsi que l’audit et le reporting sur tous les protecteurs TDE avec la fonctionnalité Azure Key Vault. Key Vault propose une gestion centralisée des clés, tire parti des modules de sécurité matériels (HSM) étroitement surveillés et permet la séparation des responsabilités liées à la gestion des clés et des données pour garantir la conformité réglementaire.  


L’utilisation de TDE avec prise en charge de BYOK présente les avantages suivants :
- Une plus grande transparence et un contrôle plus précis avec la possibilité de gérer soi-même le protecteur TDE   
- Une gestion centralisée des protecteurs TDE (avec d’autres clés et secrets utilisés dans d’autres services Azure) en les hébergeant dans Key Vault
- Séparation des responsabilités de gestion des clés et des données dans l’organisation pour permettre la répartition des tâches
- Une sécurité accrue par rapport à vos propres clients, Key Vault étant conçu pour que Microsoft n’ait pas à connaître ou extraire les clés de chiffrement 
- Prise en charge de la rotation des clés

> [!IMPORTANT]
> Si vous utilisez un chiffrement TDE géré par le service et souhaitez commencer à utiliser Key Vault, sachez que TDE reste activé pendant le processus de remplacement par un protecteur TDE dans Key Vault. Ce processus n’entraîne pas de temps d’arrêt ni de rechiffrement des fichiers de la base de données. Le remplacement d’une clé gérée par le service par une clé Key Vault nécessite uniquement de rechiffrer la clé DEK, qui est une opération en ligne rapide. 
>

## <a name="how-does-tde-with-byok-support-work"></a>Comment fonctionne TDE avec prise en charge de BYOK ?
 
![Authentification du serveur auprès de Key Vault](./media/transparent-data-encryption-byok-azure-sql/tde-byok-server-authentication-flow.PNG)

Quand TDE est d’abord configuré pour utiliser un protecteur TDE dans Key Vault, le serveur envoie la clé DEK de chaque base de données avec TDE activé à Key Vault pour demander d’inclure la clé. Key Vault retourne la clé de chiffrement de la base de données chiffrée, qui est stockée dans la base de données utilisateur.  

>[!IMPORTANT]
>Il est important de noter qu’**une fois qu’un protecteur TDE est stocké dans Azure Key Vault, il ne quitte jamais Azure Key Vault**. Le serveur logique peut uniquement envoyer des demandes d’opération de clé aux éléments de clé du protecteur TDE contenus dans Key Vault et **jamais n’accède ni ne met en cache le protecteur TDE**. L’administrateur de Key Vault a le droit de révoquer les autorisations Key Vault du serveur à tout moment, auquel cas toutes les connexions au serveur sont interrompues. 
>


## <a name="guidelines-for-configuring-tde-with-byok"></a>Instructions de configuration de TDE pour BYOK

### <a name="general-guidelines"></a>Règles générales
- Vérifiez qu’Azure Key Vault et Azure SQL Database sont dans le même locataire.  Les interactions entre coffre de clés et serveur qui sont dans des locataires différents **ne sont pas prises en charge**.
- Décidez des abonnements qui vont être utilisés pour les ressources requises. Si par la suite vous déplacez le serveur parmi les abonnements, vous devrez réinstaller TDE avec des clés BYOK.
- Lors de la configuration de TDE avec l’option BYOK, il est important de prendre en compte la charge placée sur le coffre de clés par les opérations répétées de type wrap/unwrap. Par exemple, étant donné que toutes les bases de données associées à un serveur logique utilisent le même protecteur TDE, un basculement de ce serveur déclenchera autant d’opérations de clé par rapport au coffre qu’il y a de bases de données sur le serveur. D’après notre expérience et les [limites du service de coffre de clés](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-service-limits) documentées, nous vous recommandons d’associer au maximum 500 bases de données Standard / à usage général ou 200 bases de données Premium / Critiques pour l’entreprise avec un coffre de clés Azure Key Vault dans un même abonnement, afin de garantir une disponibilité toujours aussi élevée lors de l’accès au protecteur TDE dans le coffre. 
- Recommandé : Conservez une copie du protecteur TDE en local.  Pour ce faire, vous devez avoir un appareil HSM pour créer un protecteur TDE en local et un système de dépôt de clés pour stocker une copie locale du protecteur TDE.


### <a name="guidelines-for-configuring-azure-key-vault"></a>Instructions de configuration d’Azure Key Vault

- Créez un coffre de clés avec [soft-delete](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) activé pour éviter de perdre des données en cas de suppression accidentelle d’une clé ou d’un coffre de clés.  Vous devez utiliser [PowerShell pour activer la propriété « soft-delete »](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-soft-delete-powershell) dans le coffre de clés (cette option n’est pas encore disponible dans le portail Azure Key Vault, mais elle est exigée par SQL) :  
  - Les ressources supprimées de manière réversible sont conservées pendant une durée définie, 90 jours, sauf si elles sont récupérées ou vidées.
  - Les actions de **récupération** et de **vidage** ont leurs propres autorisations associées dans une stratégie d’accès au coffre de clés. 

- Accordez l’accès au serveur logique au Key Vault avec son identité Azure AD (Azure Active Directory).  Quand vous utilisez l’interface utilisateur du portail, l’identité Azure AD est automatiquement créée et les autorisations d’accès au Key Vault sont accordées au serveur.  Si vous utilisez PowerShell pour configurer TDE avec BYOK, l’identité Azure AD doit être créée et vérifiée. Consultez [Configurer TDE avecBYOK](transparent-data-encryption-byok-azure-sql-configure.md) pour obtenir des instructions détaillées sur l’utilisation de PowerShell.

  >[!NOTE]
  >Si l’identité Azure AD **est accidentellement supprimée ou si les autorisations du serveur sont révoquées** avec la stratégie d’accès du Key Vault, le serveur perd l’accès au Key Vault.
  >
  
- Permettre l’audit et le reporting sur toutes les clés de chiffrement : Key Vault fournit des fichiers journaux que vous pouvez facilement injecter dans d’autres outils de gestion des événements et des informations de sécurité (SIEM). Le service [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault) d’OMS (Operations Management Suite) est un exemple de service déjà intégré.
- Pour assurer une haute disponibilité des bases de données chiffrées, configurez chaque serveur logique avec deux coffres de clé Azure de différentes régions.


### <a name="guidelines-for-configuring-the-tde-protector-asymmetric-key-stored-in-azure-key-vault"></a>Instructions de configuration du protecteur TDE (clé asymétrique) stocké dans Azure Key Vault

- Créer votre clé de chiffrement localement sur un appareil HSM local. Vérifiez qu’il s’agit d’une clé RSA 2048 bits asymétrique, et donc stockable dans Azure Key Vault.
- Déposez la clé dans un système de dépôt de clés.  
- Importez le fichier de clé de chiffrement (.pfx, .byok ou .backup) dans Azure Key Vault. 
    

>[!NOTE] 
    >Pour des tests, il est possible de créer une clé avec Azure Key Vault. Toutefois, cette clé ne peut pas être déposée, car la clé privée ne peut jamais quitter le coffre de clés.  Sauvegardez et déposez toujours les clés utilisées pour chiffrer les données de production, car la perte de la clé (suppression accidentelle dans le coffre de clés, expiration, etc.) entraîne la perte définitive des données.
    >
    
- Utilisez une clé sans date d’expiration et ne définissez jamais une date d’expiration sur une clé déjà en cours d’utilisation : **une fois que la clé expire, les bases de données chiffrées perdent l’accès à leur protecteur TDE et sont supprimées dans les 24 heures**.
- Vérifiez que la clé est activée et qu’elle dispose des autorisations *get*, *wrap key* et *unwrap key*.
- Créez une sauvegarde de la clé Azure Key Vault avant d’utiliser la clé dans Azure Key Vault pour la première fois. En savoir plus sur la commande [Backup-AzureKeyVaultKey](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1) .
- Créez une sauvegarde chaque fois que des modifications sont apportées à la clé (par exemple, ajout de listes de contrôle d’accès, de balises ou d’attributs de clé).
- **Conservez les versions précédentes** de la clé dans le coffre de clés lors de la rotation de clés pour pouvoir restaurer d’anciennes sauvegardes. Quand vous changez de protecteur TDE pour une base de données, les anciennes sauvegardes de la base de données **ne sont pas mises à jour** pour utiliser le protecteur TDE le plus récent.  Chaque sauvegarde nécessite le protecteur TDE qu’elle a créé lors de la restauration. Vous pouvez effectuer des rotations de clés en suivant les instructions dans [Effectuer une rotation du protecteur TDE à l’aide de PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- Conservez toutes les clés déjà utilisées dans Azure Key Vault après avoir changé les clés gérées par le service.  Vous pourrez ainsi restaurer des sauvegardes de base de données avec les protecteurs TDE stockés dans Azure Key Vault.  Les protecteurs TDE créés avec Azure Key Vault doivent être conservés jusqu'à ce que toutes les sauvegardes stockées soient créées avec les clés gérées par le service.  
- Procédez à des copies de sauvegarde récupérables de ces clés avec [Backup-AzureKeyVaultKey](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1).
- Pour supprimer une clé potentiellement compromise pendant un incident de sécurité sans risquer de perdre des données, suivez les étapes dans [Supprimer une clé potentiellement compromise](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md).


## <a name="high-availability-geo-replication-and-backup--restore"></a>Haute disponibilité, géoréplication et sauvegarde/restauration

### <a name="high-availability-and-disaster-recovery"></a>Haute disponibilité et récupération d’urgence

Le mode de configuration de la haute disponibilité avec Azure Key Vault dépend de la configuration de votre base de données et de votre serveur logique. Voici les configurations recommandées pour les deux cas distincts.  Le premier cas est un serveur logique ou une base de données autonome sans géoredondance configurée.  Le deuxième cas est une base de données ou un serveur logique configuré avec des groupes de basculement ou la géoredondance, où vous devez veiller à ce que chaque copie géoredondante ait un coffre de clés Azure local dans le groupe de basculement pour que les géobasculements fonctionnent. Dans le premier cas, si vous avez besoin d’une base de données et d’un serveur logique avec une haute disponibilité sans géoredondance configurée, il est vivement recommandé de configurer le serveur de sorte à utiliser deux coffres de clés distincts dans deux régions distinctes avec les mêmes éléments de clé.  Pour ce faire, vous pouvez créer un protecteur TDE avec le coffre de clés principal situé dans la même région que le serveur logique, puis cloner la clé dans un coffre de clés d’une autre région Azure pour que le serveur ait accès au second coffre de clés si jamais le coffre de clés principal tombe en panne alors que la base de données fonctionne. Utilisez l’applet de commande Backup-AzureKeyVaultKey pour récupérer la clé dans un format chiffré dans le coffre de clé principal, puis utilisez l’applet de commande Restore-AzureKeyVaultKey et spécifiez un coffre de clés dans la seconde région.


![Serveur unique à haute disponibilité et aucune géorécupération d’urgence](./media/transparent-data-encryption-byok-azure-sql/SingleServer_HA_Config.PNG)

## <a name="how-to-configure-geo-dr-with-azure-key-vault"></a>Comment configurer la géoreprise d’activité avec Azure Key Vault

Afin de maintenir une haute disponibilité des protecteurs TDE pour les bases de données chiffrées, vous devez configurer des coffres de clés Azure Key Vault redondants basés sur les groupes de basculement SQL Database existants ou nouveaux, ou les instances de la géoréplication active.  Chaque serveur géorépliqué nécessite un coffre de clés distinct, qui doit être colocalisé avec le serveur dans la même région Azure. Si une base de données primaire devient inaccessible en raison d’une panne dans une région et qu’un basculement est déclenché, la base de données secondaire peut prendre le relais avec le coffre de clés secondaire. 
 
Pour des bases de données Azure SQL géorépliquées, Azure Key Vault doit être configuré de la manière suivante :
- Une base de données primaire avec un coffre de clés dans une région et une base de données secondaire avec un coffre de clés dans l’autre région. 
- Au moins une base de données secondaire (jusqu’à quatre sont prises en charge). 
- Le chaînage entre bases de données secondaires n’est pas pris en charge.

La section suivante explique en détail les étapes d’installation et de configuration. 

### <a name="azure-key-vault-configuration-steps"></a>Étapes de configuration d’Azure Key Vault

- Installer [PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-5.6.0) 
- Créez deux coffres de clés Azure Key Vault dans deux régions différentes en utilisant [PowerShell pour activer la propriété « soft-delete »](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-soft-delete-powershell) dans les coffres de clés (cette option n’est pas encore disponible dans le portail Azure Key Vault, mais elle est exigée par SQL).
- Les coffres de clés Azure doivent se trouver dans les deux régions disponibles de la zone géographique Azure pour que la sauvegarde et la restauration des clés fonctionnent.  Si vous avez besoin que les coffres de clés se trouvent dans des zones géographiques différentes pour répondre aux exigences de la reprise d’activité géographique SQL, suivez le [processus BYOK](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-hsm-protected-keys) qui permet d’importer des clés à partir d’un HSM local.
- Créez une clé dans le premier coffre de clés :  
  - Clé RSA/RSA-HSA 2048 
  - Pas de date d’expiration 
  - Clé activée et disposant des autorisations pour les opérations get, wrap key et unwrap key 
- Sauvegardez la clé primaire et restaurez la clé dans le deuxième coffre de clés.  Consultez [BackupAzureKeyVaultKey](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1) et [Restore-AzureKeyVaultKey](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-5.5.0). 

### <a name="azure-sql-database-configuration-steps"></a>Étapes de configuration d’Azure SQL Database

Les étapes de configuration décrites ci-dessous diffèrent selon que vous démarrez avec un nouveau déploiement SQL ou que vous utilisez un déploiement de géoréplication SQL existant.  Nous allons d’abord expliquer les étapes de configuration pour un nouveau déploiement. Ensuite, nous expliquerons comment assigner des protecteurs TDE stockés dans Azure Key Vault à un déploiement existant où un lien de géoreprise d’activité est déjà établi. 

Étapes pour un nouveau déploiement :
- Créez les deux serveurs SQL logiques dans les deux mêmes régions où ont été créés les coffres de clés précédemment. 
- Sélectionnez le volet TDE des serveurs logiques et effectuez le opérations suivantes pour chaque serveur SQL logique :  
   - Sélectionnez le coffre de clés AKV dans la même région. 
   - Sélectionnez la clé à utiliser comme protecteur TDE (chaque serveur utilisera la copie locale du protecteur TDE). 
   - Quand vous effectuez cette opération dans le portail, un [AppID](https://docs.microsoft.com/en-us/azure/active-directory/managed-service-identity/overview) est créé pour le serveur SQL logique. Ne supprimez pas cette identité, car elle est utilisée pour assigner au serveur SQL logique les autorisations d’accès au coffre de clés.  Vous pouvez révoquer l’accès en supprimant les autorisations dans Azure Key Vault. Ne supprimez pas l’identité du serveur SQL logique, car elle est utilisée pour assigner au serveur SQL logique les autorisations d’accès au coffre de clés.
- Créez la base de données primaire. 
- Suivez les [conseils sur la géoréplication active](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview) pour terminer le scénario. Cette étape crée la base de données secondaire.

![Groupes de basculement et géorécupération d’urgence](./media/transparent-data-encryption-byok-azure-sql/Geo_DR_Config.PNG)

>[!NOTE]
>Il est important de vérifier que les deux coffres de clés contiennent les mêmes protecteurs TDE avant d’établir le lien de géoréplication entre les bases de données.
>

Étapes pour un déploiement existant de bases de données SQL avec la géoreprise d’activité :

Étant donné que les serveurs SQL logiques existent déjà et que les bases de données primaires et secondaires sont déjà assignées, les étapes de configuration d’Azure Key Vault doivent être effectuées dans l’ordre suivant : 
- Tout d’abord, effectuez les opérations suivantes sur le serveur SQL logique qui héberge la base de données secondaire : 
   - Assignez le coffre de clés situé dans la même région 
   - Assignez le protecteur TDE 
- Ensuite, effectuez l’opération suivante sur le serveur SQL logique qui héberge la base de données primaire : 
   - Sélectionnez le même protecteur TDE que celui utilisé pour la base de données secondaire
   
![Groupes de basculement et géorécupération d’urgence](./media/transparent-data-encryption-byok-azure-sql/geo_DR_ex_config.PNG)

>[!NOTE]
>Quand vous assignez le coffre de clés au serveur, vous devez commencer par le serveur secondaire.  Dans la deuxième étape, assignez le coffre de clés au serveur principal et mettez à jour le protecteur TDE. Le lien de géoreprise d’activité reste opérationnel, car à ce stade, le protecteur TDE utilisé par la base de données répliquée est disponible pour les deux serveurs.
>

Avant d’activer les protecteurs TDE avec des clés managées par l’utilisateur dans Azure Key Vault pour un scénario de géoreprise d’activité des bases de données SQL, il est important de créer et de conserver deux coffres de clés Azure Key Vault avec un contenu identique dans les mêmes régions que celles utilisées pour la géoréplication des bases de données SQL.  Plus précisément, un « contenu identique » signifie que les deux coffres de clés doivent contenir les copies des mêmes protecteurs TDE pour que les deux serveurs puissent accéder aux protecteurs TDE utilisés par toutes les bases de données.  Vous devez également vous assurer que les deux coffres de clés restent toujours synchronisés, c’est-à-dire qu’ils doivent contenir les mêmes copies des protecteurs TDE après chaque rotation de clés. Veillez aussi à conserver les anciennes versions des clés utilisées pour les fichiers journaux ou les sauvegardes. Les protecteurs TDE doivent conserver les mêmes propriétés de clés et les coffres de clés doivent conserver les mêmes autorisations d’accès pour SQL.  
 
Suivez les étapes décrites dans [Vue d’ensemble : groupes de basculement et géoréplication active](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) pour tester et déclencher un basculement. Vous devez effectuer ces étapes régulièrement pour vérifier que les autorisations d’accès de SQL aux deux coffres de clés ont été conservées. 


### <a name="backup-and-restore"></a>Sauvegarde et restauration

Une fois qu’une base de données est chiffrée avec TDE et une clé Key Vault, toutes les sauvegardes effectuées ensuite sont également chiffrées avec le même protecteur TDE.

Pour restaurer une sauvegarde chiffrée avec un protecteur TDE de Key Vault, assurez-vous que les éléments de clé se trouvent toujours dans le coffre Key Vault d’origine sous le nom de clé initial. Quand le protecteur TDE est substitué pour une base de données, les anciennes sauvegardes de la base de données **ne sont pas** mises à jour pour utiliser le nouveau protecteur TDE. C’est pourquoi nous vous recommandons de conserver toutes les anciennes versions du protecteur TDE dans Key Vault pour pouvoir restaurer des sauvegardes de la base de données en cas de besoin. 

Si une clé nécessaire pour restaurer une sauvegarde n’est plus dans le coffre Key Vault d’origine, un message d’erreur similaire à celui-ci est retourné : « Le serveur cible <Servername> n’a pas accès à tous les URI AKV créés entre le <Timestamp #1> et le <Timestamp #2>. Restaurez tous les URI AKV, puis réessayez l’opération. »

Pour corriger cela, exécutez l’applet de commande [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) afin d’obtenir la liste des clés du coffre de clés qui ont été ajoutées au serveur (sauf si elles ont été supprimées par un utilisateur). Pour vous assurer que toutes les sauvegardes peuvent être restaurées, vérifiez que le serveur cible de sauvegarde a accès à l’ensemble de ces clés.

   ```powershell
   Get-AzureRmSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
Pour en savoir plus sur la récupération d’une sauvegarde pour SQL Database, consultez [Récupérer une base de données Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups). Pour en savoir plus sur la récupération d’une sauvegarde pour SQL Data Warehouse, consultez [Récupérer un entrepôt de données Azure SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-overview).


Considération supplémentaire relative aux fichiers journaux sauvegardés : Les fichiers journaux sauvegardés restent chiffrés avec le Chiffreur TDE d’origine, même si celui-ci a subi une permutation et que la base de données utilise maintenant un nouveau protecteur TDE.  Au moment de la restauration, les deux clés seront nécessaires pour restaurer la base de données.  Si le fichier journal utilise un protecteur TDE stocké dans Azure Key Vault, cette clé sera nécessaire au moment de la restauration, même si la base de données a entre temps été modifiée de façon à utiliser le chiffrement transparent des données géré par le service.


