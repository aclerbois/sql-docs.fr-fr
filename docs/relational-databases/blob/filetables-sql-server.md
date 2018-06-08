---
title: FileTables (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
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
- FileTables [SQL Server], overview
- FileTables [SQL Server]
- FileTable [SQL Server], see FileTables [SQL Server]
- FileTable [SQL Server]
ms.assetid: a57b629c-e9ed-48fd-9a48-ed3787d80c8f
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 41f348d83e3b48a0c9d1fbe0ea23ad863cd8a0e6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="filetables-sql-server"></a>FileTables (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La fonctionnalité FileTable apporte une prise en charge de l'espace de noms de fichier Windows et la compatibilité des applications Windows avec les données de fichier stockées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. FileTable permet à une application d'intégrer ses composants de stockage et de gestion des données, et fournit des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégrés (notamment la recherche sémantique et en texte intégral) sur des données et des métadonnées non structurées.  
  
 En d'autres termes, vous pouvez maintenant stocker des fichiers et des documents dans des tables spéciales dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , appelées FileTables, mais y accéder à partir d'applications Windows comme si ils avaient été stockés dans le système de fichiers, sans apporter de modifications à vos applications clientes.  
  
 La fonctionnalité FileTable s'appuie sur la technologie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] FILESTREAM. Pour en savoir plus sur FILESTREAM, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
##  <a name="Goals"></a> Avantages de la fonctionnalité FileTable  
 Les objectifs de la fonctionnalité FileTable incluent les éléments suivants :  
  
-   Compatibilité des API Windows pour les données de fichier stockées dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La compatibilité d'API Windows inclut ce qui suit :  
  
    -   Accès en continu non transactionnel et mises à jour sur place aux données FILESTREAM.  
  
    -   Espace de noms hiérarchique de répertoires et fichiers.  
  
    -   Stockage d'attributs de fichier, tels que la date de création et la date de modification.  
  
    -   Prise en charge des API de gestion de fichiers et de répertoires Windows.  
  
-   Compatibilité avec d'autres fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] notamment des outils de gestion, des services et des fonctions de requête relationnelles sur FILESTREAM et les données d'attribut de fichier.  
  
 Par conséquent, les FileTables mettent fin à un frein significatif à l'utilisation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le stockage et la gestion de données non structurées qui résident actuellement sous la forme de fichiers sur des serveurs de fichiers. Les entreprises peuvent déplacer ces données depuis des serveurs de fichiers vers des FileTables afin de tirer parti des services et de l'administration intégrés fournis par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En même temps, elles peuvent assurer la compatibilité d'applications Windows pour leurs applications Windows existantes qui considèrent ces données en tant que fichiers dans le système de fichiers.  
 
  
##  <a name="Description"></a> Présentation d'un FileTable  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une **table de fichiers**spéciale, également connue sous le nom de **FileTable**, pour les applications qui nécessitent un stockage de répertoires et de fichiers dans la base de données, avec la compatibilité avec les API Windows et un accès non transactionnel. Un FileTable est une table utilisateur spécialisée avec un schéma prédéfini qui stocke des données FILESTREAM, ainsi que des informations de hiérarchie de fichiers et de répertoires et d'attribut de fichier.  
  
 Un FileTable offre les fonctionnalités suivantes :  
  
-   Un FileTable représente une hiérarchie de répertoires et de fichiers. Il stocke des données relatives à tous les nœuds de cette hiérarchie, à la fois pour les répertoires et les fichiers qu'ils contiennent. Cette hiérarchie démarre à partir d'un répertoire racine que vous spécifiez lors de la création du FileTable.  
  
-   Chaque ligne d'un FileTable représente un fichier ou un répertoire.  
  
-   Chaque ligne contient les éléments suivants. Pour plus d'informations sur le schéma d’un FileTable, consultez [Schéma de FileTable](../../relational-databases/blob/filetable-schema.md).  
  
    -   Une colonne **file_stream** pour les données de flux et un identificateur **stream_id** (GUID). (La colonne **file_stream** a la valeur NULL pour un répertoire.)  
  
    -   Les colonnes **path_locator** et **parent_path_locator** pour la représentation et la maintenance de l’élément actif (fichier ou répertoire) et de la hiérarchie de répertoires.  
  
    -   10 attributs de fichier tels que la date de création et la date de modification utiles avec les API d'E/S de fichier.  
  
    -   Une colonne de type qui prend en charge la recherche en texte intégral et la recherche sémantique sur les fichiers et les documents.  
  
-   Un FileTable applique certains déclencheurs et contraintes définies par le système afin de gérer la sémantique de l'espace de noms de fichier.  
  
-   Lorsque la base de données est configurée pour un accès non transactionnel, la hiérarchie de répertoires et de fichiers représentée dans le FileTable est exposée sous le partage FILESTREAM configuré pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cela fournit l'accès au système de fichiers pour les applications Windows.  
  
 Voici quelques caractéristiques supplémentaires des FileTables :  
  
-   Les données de fichier et de répertoire stockées dans un FileTable sont exposées via un partage Windows pour l'accès aux fichiers non transactionnel pour les applications basées sur des API Windows. Pour une application Windows, cela ressemble à un partage normal avec ses fichiers et répertoires. Les applications peuvent utiliser un ensemble complet d'API Windows pour gérer les fichiers et les répertoires sous ce partage.  
  
-   La hiérarchie de répertoires exposée en surface via le partage est une structure de répertoires purement logique maintenue dans le FileTable.  
  
-   Les appels pour créer ou modifier un fichier ou un répertoire via le partage Windows sont interceptés par un composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et reflétés dans les données relationnelles correspondantes du FileTable.  
  
-   Les opérations d'API Windows ne sont pas transactionnelles par nature et ne sont pas associées à des transactions utilisateur. Toutefois, l'accès transactionnel aux données FILESTREAM stockées dans un FileTable est entièrement pris en charge, comme c'est le cas pour toute colonne FILESTREAM d'une table standard.  
  
-   Les FileTables peuvent également être interrogés et mis à jour via un accès [!INCLUDE[tsql](../../includes/tsql-md.md)] normal. Ils sont également intégrés aux outils de gestion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et à des fonctionnalités telles que la sauvegarde.  
  
##  <a name="additional"></a> Remarques supplémentaires concernant l'utilisation de FileTables  
  
###  <a name="DBA"></a> Considérations d'ordre administratif  
 **À propos de FILESTREAM et des FileTables**  
  
-   Vous configurez des FileTables de manière distincte de FILESTREAM. Par conséquent, vous pouvez continuer à utiliser la fonctionnalité FILESTREAM sans activer l'accès non transactionnel ou créer de FileTables.  
  
-   Il n'existe aucun accès non transactionnel aux données FILESTREAM sauf via des FileTables. Par conséquent, lorsque vous activez l'accès non transactionnel, le comportement des applications et des colonnes FILESTREAM existantes n'est pas affecté.  
  
 **À propos de FileTables et de l'accès non transactionnel**  
  
-   Vous pouvez activer ou désactiver l'accès non transactionnel au niveau de la base de données.  
  
-   Vous pouvez configurer ou définir avec précision l'accès non transactionnel au niveau de la base de données en le désactivant, ou en activant l'accès en lecture seule ou l'accès en lecture/écriture intégral.  
   
###  <a name="memory"></a> Les FileTables ne prennent pas en charge les fichiers mappés en mémoire  
 Les FileTables ne prennent pas en charge les fichiers mappés en mémoire Les applications Bloc-notes et Peinture sont deux exemples classiques d'applications qui utilisent les fichiers mappés en mémoire. Vous ne pouvez pas utiliser ces applications sur le même ordinateur que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour ouvrir des fichiers stockés dans un FileTable. Toutefois, vous pouvez utiliser ces applications à partir d'un ordinateur distant afin d'ouvrir des fichiers stockés dans un FileTable, car dans ces circonstances, la fonctionnalité de mappage en mémoire n'est pas utilisée.  
   
##  <a name="reltasks"></a> Tâches associées  
 [Activer les conditions préalables pour les FileTables](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)  
 Décrit la manière de satisfaire aux conditions préalables en vue de la création et de l'utilisation de FileTables.  
  
 [Créer, modifier et supprimer des FileTables](../../relational-databases/blob/create-alter-and-drop-filetables.md)  
 Décrit la procédure de création d'un nouveau FileTable, ou de modification ou de suppression d'un FileTable existant.  
  
 [Charger des fichiers dans FileTables](../../relational-databases/blob/load-files-into-filetables.md)  
 Explique comment charger ou migrer des fichiers dans FileTables.  
  
 [Travailler avec des répertoires et des chemins d’accès dans des FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
 Décrit la structure de répertoires dans laquelle les fichiers sont stockés dans FileTables.  
  
 [Accéder aux FileTables avec Transact-SQL](../../relational-databases/blob/access-filetables-with-transact-sql.md)  
 Décrit le fonctionnement des commandes DML (langage de manipulation de données) Transact-SQL sur des FileTables.  
  
 [Accéder aux FileTables avec des API d’entrée-sortie de fichier](../../relational-databases/blob/access-filetables-with-file-input-output-apis.md)  
 Décrit le fonctionnement des E/S du système de fichiers sur un FileTable.  
  
 [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md)  
 Décrit les tâches d'administration courantes permettant de gérer des FileTables.  
  
##  <a name="relcontent"></a> Contenu connexe  
 [Schéma de FileTable](../../relational-databases/blob/filetable-schema.md)  
 Décrit le schéma prédéfini et fixe d'un FileTable.  
  
 [Compatibilité de FileTable avec d'autres fonctionnalités SQL Server](../../relational-databases/blob/filetable-compatibility-with-other-sql-server-features.md)  
 Décrit le fonctionnement de FileTables avec d'autres fonctionnalités de SQL Server.  
  
 [DDL, fonctions, procédures stockées et vues FileTable](../../relational-databases/blob/filetable-ddl-functions-stored-procedures-and-views.md)  
 Répertorie les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et les objets de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ont été ajoutés ou modifiés afin de prendre en charge la fonctionnalité FileTable.  

## <a name="see-also"></a> Voir aussi
[Vues de gestion dynamiques Filestream et FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Vues de catalogue Filestream et FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[Procédures stockées Filestream et FileTable (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)


  
  
