---
title: FILESTREAM (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2018
ms.prod: sql
ms.prod_service: database-engine
ms.component: blob
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-blob
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server]
- FILESTREAM [SQL Server], about
- FILESTREAM [SQL Server], overview
ms.assetid: 9a5a8166-bcbe-4680-916c-26276253eafa
caps.latest.revision: 13
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 378ae9ea900f57a6003337952c0e2ee1e223b5ff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="filestream-sql-server"></a>FILESTREAM (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

FILESTREAM permet aux applications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]de stocker des données non structurées, telles que des documents et des images, dans le système de fichiers. Les applications peuvent tirer parti des API de diffusion et des performances du système de fichiers, et en même temps maintenir la cohérence transactionnelle entre les données non structurées et les données structurées correspondantes.  
  
FILESTREAM intègre le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] avec un système de fichiers NTFS ou ReFS en stockant les données d’objet blob **varbinary(max)** en tant que fichiers dans le système de fichiers. [!INCLUDE[tsql](../../includes/tsql-md.md)] Les instructions peuvent insérer, mettre à jour, interroger, rechercher et sauvegarder des données FILESTREAM. Les interfaces de système de fichiers Win32 fournissent l'accès de diffusion en continu aux données.  
  
FILESTREAM utilise le cache système NT pour mettre en cache les données de fichiers. Cela aide à réduire tout effet que les données FILESTREAM peuvent avoir sur les performances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Le pool de mémoires tampons [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas utilisé ; par conséquent, cette mémoire est disponible pour le traitement de requête.  
  
FILESTREAM n'est pas activé automatiquement lorsque vous installez ou mettez à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous devez activer FILESTREAM à l’aide du Gestionnaire de configuration SQL Server et de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour utiliser FILESTREAM, vous devez créer ou modifier une base de données de sorte qu'elle contienne un type spécial de groupe de fichiers. Ensuite, créez ou modifiez une table afin qu’elle contienne une colonne **varbinary(max)** avec l’attribut FILESTREAM. Après avoir effectué ces tâches, vous pouvez utiliser [!INCLUDE[tsql](../../includes/tsql-md.md)] et Win32 pour gérer les données FILESTREAM.  

## <a name="when-to-use-filestream"></a>À quel moment utiliser FILESTREAM

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les objets blob peuvent être des données **varbinary(max)** standard qui stockent les données dans des tables, ou des objets **varbinary(max)** FILESTREAM qui stockent les données dans le système de fichiers. La taille et l'utilisation des données déterminent si vous devez utiliser du stockage de base de données ou du stockage de système de fichiers. Si les conditions suivantes sont remplies, vous devez envisager d'utiliser FILESTREAM :  

- La taille des objets stockés est, en moyenne, supérieure à 1 Mo.  
- L'accès en lecture rapide est important.
- Vous développez des applications qui utilisent une couche intermédiaire pour la logique d'application.  

Pour les plus petits objets, le stockage des objets blob **varbinary(max)** dans la base de données procure souvent de meilleures performances de diffusion en continu.  

## <a name="filestream-storage"></a>Stockage FILESTREAM

Le stockage FILESTREAM est implémenté en tant que colonne **varbinary(max)** dans laquelle les données sont stockées comme objet blob dans le système de fichiers. Les tailles des objets blob sont limitées uniquement par la taille de volume du système de fichiers. La limitation **varbinary(max)** standard de tailles de fichiers de 2 Go ne s’applique pas aux objets blob stockés dans le système de fichiers.  
  
Pour spécifier qu’une colonne doit stocker des données dans le système de fichiers, spécifiez l’attribut FILESTREAM sur une colonne **varbinary(max)** . Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] stocke alors toutes les données pour cette colonne dans le système de fichiers, mais pas dans le fichier de base de données.  
  
Les données FILESTREAM doivent être stockées dans des groupes de fichiers FILESTREAM. Un groupe de fichiers FILESTREAM est un groupe de fichiers spécial qui contient des répertoires de système de fichiers au lieu des fichiers eux-mêmes. Ces répertoires de système de fichiers portent le nom de *conteneurs de données*. Les conteneurs de données sont l’interface entre le stockage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et le stockage du système de fichiers. 

Lorsque vous utilisez le stockage FILESTREAM, considérez les éléments suivants :  

- Lorsqu'une table contient une colonne FILESTREAM, chaque ligne doit avoir un ID de ligne unique n'acceptant pas la valeur Null.  
- Plusieurs conteneurs de données peuvent être ajoutés à un groupe de fichiers FILESTREAM.  
- Les conteneurs de données FILESTREAM ne peuvent pas être imbriqués.  
- Lorsque vous utilisez le clustering de basculement, les groupes de fichiers FILESTREAM doivent être sur des ressources de disque partagées.  
- Les groupes de fichiers FILESTREAM peuvent être sur des volumes compressés.

### <a name="integrated-management"></a>Gestion intégrée

FILESTREAM étant implémenté en tant que colonne **varbinary(max)** et intégré directement dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)], la plupart des fonctions et outils d’administration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctionnent sans changement pour les données FILESTREAM. Par exemple, vous pouvez utiliser tous les modes de récupération et de sauvegarde avec les données FILESTREAM, et les données FILESTREAM sont sauvegardées avec les données structurées dans la base de données. Si vous ne souhaitez pas sauvegarder les données FILESTREAM avec les données relationnelles, vous pouvez utiliser une sauvegarde partielle pour exclure les groupes de fichiers FILESTREAM.  

### <a name="integrated-security"></a>Sécurité intégrée

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les données FILESTREAM sont sécurisées tout comme les autres données : en accordant des autorisations au niveau des tables ou des colonnes. Si un utilisateur dispose de l'autorisation pour la colonne FILESTREAM dans une table, il peut ouvrir les fichiers associés.  

> [!NOTE]
> Le chiffrement n'est pas pris en charge sur les données FILESTREAM.  

Seul le compte sous lequel le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute dispose des autorisations sur le conteneur FILESTREAM. Nous recommandons de n'accorder des autorisations sur le conteneur de données à aucun autre compte.

> [!NOTE]
> Les connexions SQL ne fonctionnent pas avec les conteneurs FILESTREAM. Seule l’authentification NTFS ou ReFS fonctionne avec les conteneurs FILESTREAM.

## <a name="dual"></a> Accès aux données BLOB avec Transact-SQL et accès en continu au système de fichiers

Après avoir stocké des données dans une colonne FILESTREAM, vous pouvez accéder aux fichiers en utilisant des transactions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou en utilisant des API Win32.  
  
### <a name="transact-sql-access"></a>Accès Transact-SQL

Grâce à [!INCLUDE[tsql](../../includes/tsql-md.md)], vous pouvez insérer, mettre à jour et supprimer des données FILESTREAM :  

- Vous pouvez utiliser une opération d'insertion pour préremplir un champ FILESTREAM avec une valeur NULL, une valeur vide ou des données inline relativement courtes. Toutefois, une grande quantité de données est diffusée en continu plus efficacement dans un fichier qui utilise des interfaces Win32.  
- Lorsque vous mettez à jour un champ FILESTREAM, vous modifiez les données d'objet blob sous-jacentes dans le système de fichiers. Lorsqu'un champ FILESTREAM a la valeur NULL, les données d'objet blob associées au champ sont supprimées. Vous ne pouvez pas utiliser une mise à jour segmentée [!INCLUDE[tsql](../../includes/tsql-md.md)] , implémentée comme UPDATE **.** Write(), pour effectuer des mises à jour partielles des données. 
- Lorsque vous supprimez une ligne ou supprimez ou tronquez une table qui contient des données FILESTREAM, vous supprimez les données d'objet blob sous-jacentes dans le système de fichiers.

### <a name="file-system-streaming-access"></a>Accès de diffusion en continu au système de fichiers

La prise en charge de diffusion en continu Win32 fonctionne dans le contexte d'une transaction [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans une transaction, vous pouvez utiliser des fonctions FILESTREAM pour obtenir un chemin d'accès de système de fichiers UNC logique d'un fichier. Vous utilisez ensuite l’API OpenSqlFilestream pour obtenir un descripteur de fichier. Ce descripteur peut ensuite être utilisé par des interfaces de diffusion de fichiers en continu Win32, telles que ReadFile() et WriteFile(), afin d’accéder au fichier et de le mettre à jour par le biais du système de fichiers.  

Les opérations de fichiers étant transactionnelles, vous ne pouvez pas supprimer ou renommer des fichiers FILESTREAM par le biais du système de fichiers.  

**Modèle d'instruction**

L'accès au système de fichiers FILESTREAM modèle une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] en utilisant l'ouverture et la fermeture de fichier. L'instruction démarre lorsqu'un descripteur de fichier est ouvert et se termine lorsque le descripteur est fermé. Par exemple, lorsqu'un descripteur d'écriture est fermé, tout déclencheur AFTER possible enregistré sur la table est activé comme si une instruction UPDATE était effectuée.

**Espace de noms de stockage**

Dans FILESTREAM, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] contrôle l'espace de noms du système de fichiers physique d'objet blob. Une nouvelle fonction intrinsèque, [PathName](../../relational-databases/system-functions/pathname-transact-sql.md), fournit le chemin UNC logique de l’objet blob qui correspond à chaque cellule FILESTREAM dans la table. L’application utilise ce chemin logique pour obtenir le descripteur Win32 et opérer sur les données d’objet blob en utilisant des interfaces de système de fichiers Win32 ordinaires. La fonction retourne NULL si la valeur de la colonne FILESTREAM est NULL.  

**Accès au système de fichiers traité**

Une nouvelle fonction intrinsèque, [GET_FILESTREAM_TRANSACTION_CONTEXT()](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md), fournit le jeton qui représente la transaction actuelle à laquelle la session est associée. La transaction doit avoir été démarrée mais pas encore abandonnée ou validée. En obtenant un jeton, l'application lie les opérations de diffusion en continu de système de fichiers FILESTREAM avec une transaction commencée. La fonction retourne NULL si aucune transaction n'est explicitement commencée.  

Tous les descripteurs de fichiers doivent être fermés avant que la transaction ne soit validée ou abandonnée. Si un descripteur est laissé ouvert au-delà de l'étendue de transaction, les lectures supplémentaires contre le descripteur provoqueront un échec ; les écritures supplémentaires contre le descripteur réussiront, mais les données effectives ne seront pas écrites sur le disque. De même, si la base de données ou l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] s'arrête, tous les descripteurs ouverts sont invalidés.  

**Durabilité transactionnelle**

Avec FILESTREAM, lors de la validation des transactions, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] garantit la durabilité des transactions pour les données d'objet blob FILESTREAM modifiées à partir de l'accès en continu au système de fichiers.  

**Sémantique d'isolation**

La sémantique d'isolation est gouvernée par les niveaux d'isolation des transactions du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Le niveau d'isolation de lecture validée est pris en charge pour l'accès [!INCLUDE[tsql](../../includes/tsql-md.md)] et au système de fichiers. Les opérations de lecture renouvelables, ainsi que les isolements de capture instantanée sérialisables, sont pris en charge. La lecture erronée n'est pas prise en charge.  

Les opérations d'ouverture d'accès au système de fichiers n'attendent pas de verrous. Au lieu de cela, les opérations d'ouverture échouent immédiatement si elles ne peuvent pas accéder aux données à cause de l'isolation des transactions. Les appels API de diffusion en continu échouent avec ERROR_SHARING_VIOLATION si l'opération d'ouverture ne peut se poursuivre à cause de la violation d'isolation.  

Pour permettre les mises à jour partielles, l'application peut publier un contrôle FS de périphérique (FSCTL_SQL_FILESTREAM_FETCH_OLD_CONTENT) afin d'extraire l'ancien contenu dans le fichier auquel le descripteur ouvert fait référence. Cela déclenchera une copie de l'ancien contenu côté serveur. Pour de meilleures performances d'application et afin d'éviter des dépassements de délais d'attente potentiels lorsque vous travaillez avec de très grands fichiers, nous vous recommandons d'utiliser des E/S asynchrones.  

Si le FSCTL est publié après l'écriture dans le descripteur, la dernière opération d'écriture persistera et les écritures antérieures effectuées dans le descripteur seront perdues.

**API du système de fichiers et niveaux d'isolation pris en charge**

Lorsqu'une API du système de fichiers ne parvient pas à ouvrir un fichier en raison d'une violation d'isolation, une exception ERROR_SHARING_VIOLATION est retournée. Cette violation d'isolation se produit lorsque deux transactions essaient d'accéder au même fichier. Le résultat de l'opération d'accès dépend du mode dans lequel le fichier a été ouvert et de la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle la transaction s'exécute. Le tableau suivant décrit les résultats possibles pour les deux transactions qui accèdent au même fichier.

|Transaction 1|Transaction 2|Résultat sur SQL Server 2008|Résultat sur SQL Server 2008 R2 et les versions ultérieures|  
|-------------------|-------------------|--------------------------------|------------------------------------------------------|  
|Ouvert pour la lecture.|Ouvert pour la lecture.|Réussite des deux transactions.|Réussite des deux transactions.|  
|Ouvert pour la lecture.|Ouvert pour l'écriture.|Réussite des deux transactions. Les opérations d'écriture sous la transaction 2 n'affectent pas les opérations de lecture effectuées dans la transaction 1.|Réussite des deux transactions. Les opérations d'écriture sous la transaction 2 n'affectent pas les opérations de lecture effectuées dans la transaction 1.|  
|Ouvert pour l'écriture.|Ouvert pour la lecture.|L'opération d'ouverture sur la transaction 2 échoue avec une exception ERROR_SHARING_VIOLATION.|Réussite des deux transactions.|  
|Ouvert pour l'écriture.|Ouvert pour l'écriture.|L'opération d'ouverture sur la transaction 2 échoue avec une exception ERROR_SHARING_VIOLATION.|L'opération d'ouverture sur la transaction 2 échoue avec une exception ERROR_SHARING_VIOLATION.|  
|Ouvert pour la lecture.|Ouvert pour SELECT.|Réussite des deux transactions.|Réussite des deux transactions.|  
|Ouvert pour la lecture.|Ouvert pour UPDATE ou DELETE.|Réussite des deux transactions. Les opérations d'écriture sous la transaction 2 n'affectent pas les opérations de lecture effectuées dans la transaction 1.|Réussite des deux transactions. Les opérations d'écriture sous la transaction 2 n'affectent pas les opérations de lecture effectuées dans la transaction 1.|  
|Ouvert pour l'écriture.|Ouvert pour SELECT.|La transaction 2 se bloque jusqu'à ce que la transaction 1 valide ou termine la transaction, ou l'opération d'obtention d'un verrou pour la transaction se solde par une erreur de délai d'attente.|Réussite des deux transactions.|  
|Ouvert pour l'écriture.|Ouvert pour UPDATE ou DELETE.|La transaction 2 se bloque jusqu'à ce que la transaction 1 valide ou termine la transaction, ou l'opération d'obtention d'un verrou pour la transaction se solde par une erreur de délai d'attente.|La transaction 2 se bloque jusqu'à ce que la transaction 1 valide ou termine la transaction, ou l'opération d'obtention d'un verrou pour la transaction se solde par une erreur de délai d'attente.|  
|Ouvert pour SELECT.|Ouvert pour la lecture.|Réussite des deux transactions.|Réussite des deux transactions.|  
|Ouvert pour SELECT.|Ouvert pour l'écriture.|Réussite des deux transactions. Les opérations d'écriture sous la transaction 2 n'affectent pas la transaction 1.|Réussite des deux transactions. Les opérations d'écriture sous la transaction 2 n'affectent pas la transaction 1.|  
|Ouvert pour UPDATE ou DELETE.|Ouvert pour la lecture.|L'opération d'ouverture sur la transaction 2 échoue avec une exception ERROR_SHARING_VIOLATION.|Réussite des deux transactions.|  
|Ouvert pour UPDATE ou DELETE.|Ouvert pour l'écriture.|L'opération d'ouverture sur la transaction 2 échoue avec une exception ERROR_SHARING_VIOLATION.|L'opération d'ouverture sur la transaction 2 échoue avec une exception ERROR_SHARING_VIOLATION.|  
|Ouvert pour SELECT avec lecture renouvelable.|Ouvert pour la lecture.|Réussite des deux transactions.|Réussite des deux transactions.|  
|Ouvert pour SELECT avec lecture renouvelable.|Ouvert pour l'écriture.|L'opération d'ouverture sur la transaction 2 échoue avec une exception ERROR_SHARING_VIOLATION.|L'opération d'ouverture sur la transaction 2 échoue avec une exception ERROR_SHARING_VIOLATION.|

**Double écriture à partir de clients distants**

L'accès de système de fichiers distant aux données FILESTREAM est activé sur le protocole SMB (Block Server Message). Si le client est distant, aucune opération d'écriture n'est mise en cache par le côté client. Les opérations d'écriture seront toujours envoyées au serveur. Les données peuvent être mises en cache du côté serveur. Nous recommandons que les applications qui s'exécutent sur des clients distants consolident les petites opérations d'écriture afin d'effectuer moins d'opérations d'écriture avec une taille de données supérieure.  

La création de vues mappées en mémoire (E/S mappées en mémoire) à l'aide d'un descripteur FILESTREAM n'est pas prise en charge. Si le mappage mémoire est utilisé pour les données FILESTREAM, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas garantir la cohérence et la durabilité des données, ni l’intégrité de la base de données.  

## <a name="related-tasks"></a>Related Tasks

[Activer et configurer FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)  
[Créer une base de données compatible FILESTREAM](../../relational-databases/blob/create-a-filestream-enabled-database.md)  
[Créer une table pour le stockage de données FILESTREAM](../../relational-databases/blob/create-a-table-for-storing-filestream-data.md)  
[Accéder aux données FILESTREAM avec Transact-SQL](../../relational-databases/blob/access-filestream-data-with-transact-sql.md) [Créer des applications clientes pour les données FILESTREAM](../../relational-databases/blob/create-client-applications-for-filestream-data.md)  
[Accéder à des données FILESTREAM avec OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
[Effectuer des mises à jour partielles de données FILESTREAM](../../relational-databases/blob/make-partial-updates-to-filestream-data.md)  
[Éviter les conflits avec les opérations de base de données dans les applications FILESTREAM](../../relational-databases/blob/avoid-conflicts-with-database-operations-in-filestream-applications.md)  
[Déplacer une base de données compatible FILESTREAM](../../relational-databases/blob/move-a-filestream-enabled-database.md)  
[Configurer FILESTREAM sur un cluster de basculement](../../relational-databases/blob/set-up-filestream-on-a-failover-cluster.md)  
[Configurer un pare-feu pour l'accès FILESTREAM](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)

## <a name="related-content"></a>Contenu associé

[Compatibilité de FILESTREAM avec d’autres fonctionnalités SQL Server](../../relational-databases/blob/filestream-compatibility-with-other-sql-server-features.md)
<br>[Vues de gestion dynamiques Filestream et FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Vues de catalogue Filestream et FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[Procédures stockées Filestream et FileTable (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)

