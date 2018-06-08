---
title: Compatibilité de FileTable avec d’autres fonctionnalités SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
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
- FileTables [SQL Server], using with other features
ms.assetid: f12a17e4-bd3d-42b0-b253-efc36876db37
caps.latest.revision: 19
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e4d74fedccd53867721676f8e070f68bcd463a3b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="filetable-compatibility-with-other-sql-server-features"></a>Compatibilité de FileTable avec d'autres fonctionnalités SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Décrit le fonctionnement des FileTables avec d'autres fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="alwayson"></a> Groupes de disponibilité AlwaysOn et FileTables  
 Lorsque la base de données qui contient des données FILESTREAM ou FileTable appartient à un groupe de disponibilité AlwaysOn :  
  
-   La fonctionnalité FileTable n'est prise en charge que partiellement par [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. Après un basculement, les données FileTable sont accessibles sur le réplica principal, mais pas sur les réplicas secondaires avec accès en lecture.  
  
    > **REMARQUE :**  notez qu’après un basculement, l’intégralité des fonctionnalités FILESTREAM est prise en charge. Les données FILESTREAM sont accessibles à la fois sur les réplicas secondaires avec accès en lecture et sur le nouveau réplica principal.  
  
-   Les fonctions FILESTREAM et FileTable acceptent ou retournent des noms de réseau virtuel (VNN) à la place de noms d'ordinateur. Pour plus d’informations sur ces fonctions, consultez [Fonctions FileStream et FileTable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/filestream-and-filetable-functions-transact-sql.md).  
  
-   Tous les accès à FILESTREAM ou aux données FileTable via les API du système de fichiers doivent utiliser des VNN à la place des noms d'ordinateur. Pour plus d’informations, consultez [FILESTREAM et FileTable avec groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/filestream-and-filetable-with-always-on-availability-groups-sql-server.md).  
  
##  <a name="OtherPartitioning"></a> Partitionnement et FileTables  
 Le partitionnement n'est pas pris en charge sur les FileTables. Avec la prise en charge de plusieurs groupes de fichiers FILESTREAM, les problèmes de montée en puissance parallèle purs peuvent être gérés sans avoir recours au partitionnement dans la plupart des cas (contrairement à FILESTREAM de SQL 2008).  
  
##  <a name="OtherRepl"></a> Réplication et FileTables  
 La réplication et les fonctionnalités associées (notamment la réplication transactionnelle, la réplication de fusion, la capture des données modifiées et le suivi des modifications) ne sont pas prises en charge avec les FileTables.  
  
##  <a name="OtherIsolation"></a> Sémantique de transaction et FileTables  
 **applications Windows**  
 Les applications Windows ne reconnaissent pas les transactions de base de données ; par conséquent, les opérations d'écriture Windows ne fournissent pas les propriétés ACID d'une transaction de base de données. De ce fait, la récupération et les restaurations transactionnelles ne sont pas possibles avec les opérations de mise à jour Windows.  
  
 **Applications Transact-SQL**  
 Pour les applications TSQL opérant sur la colonne FILESTREAM(file_stream) d'un FileTable, la sémantique d'isolation est la même qu'avec le type de données FILESTREAM dans une table utilisateur standard.  
  
##  <a name="OtherQueryNot"></a> Notifications de requête et FileTables  
 La requête ne peut pas contenir de référence à la colonne FILESTREAM du FileTable, dans la clause WHERE ou toute autre partie de la requête.  
  
##  <a name="OtherSelectInto"></a> SELECT INTO et FileTables  
 Les instructions SELECT INTO d'un FileTable ne propagent pas la sémantique FileTable à la table de destination créée (à l'instar des colonnes FILESTREAM d'une table standard). Toutes les colonnes de table de destination se comportent comme des colonnes normales. Elles ne sont pas associées à une sémantique FileTable.  
  
##  <a name="OtherTriggers"></a> Déclencheurs et FileTables  
 **Déclencheurs DDL (langage de définition de données)**  
 Il n'y a pas d'éléments spéciaux à prendre en considération pour les déclencheurs DDL utilisés avec les FileTables. Les déclencheurs DDL habituels sont activés lors des opérations de création/modification de base de données, ainsi que lors des opérations CREATE/ALTER TABLE sur les FileTables. Les déclencheurs peuvent récupérer les données d'événement réelles, notamment le texte de la commande DDL et d'autres informations en appelant la fonction EVENTDATA(). Aucun nouvel événement ou changement n'a été apporté au schéma Eventdata existant.  
  
 **Déclencheurs DML (langage de manipulation de données)**  
 Ces restrictions s'appliquent lors de l'opération DDL afin de créer des déclencheurs.  
  
-   Les FileTables ne prennent PAS en charge les déclencheurs INSTEAD OF pour les opérations DML. Il s'agit d'une restriction existante sur toutes les tables qui contiennent des colonnes FILESTREAM.  
  
-   Les FileTables prennent en charge les déclencheurs AFTER pour les opérations DML.  
  
-   Les déclencheurs définis sur un FileTable ne peuvent pas mettre à jour de FileTables (notamment le FileTable parent). Cette restriction existe principalement pour empêcher un déclencheur d'entrer en conflit de verrouillage avec les verrous maintenus par l'accès au système de fichiers dans la même transaction.  
  
 **Accès non transactionnel et ses effets sur les déclencheurs**  
 -   Lorsqu'un accès de mise à jour non transactionnel est autorisé dans une base de données, il est possible d'effectuer une mise à jour sur place des données FILESTREAM dans toutes les tables, notamment un FileTable dans cette base de données. En raison de cette possibilité, l'image Avant du contenu FILESTREAM risque de ne pas être disponible pour le déclencheur.  
  
-   Pour les opérations de mise à jour non transactionnelles via le système de fichiers, SQL Server crée une transaction interne pour capturer l'opération CloseHandle et les éventuels déclencheurs DML définis peuvent être activés dans le cadre de cette transaction. Une restauration, telle qu'une transaction dans le corps du déclencheur, si elle n'est pas empêchée, ne restaure pas pour autant les modifications apportées au FILESTREAM.  Cette restauration peut également empêcher l'activation des déclencheurs de mise à jour, bien que le contenu FILESTREAM soit modifié.  
  
-   En plus de ces impacts, les déclencheurs sur les FileTables doivent gérer quelques comportements supplémentaires :  
  
    -   En cas d'opérations de mise à jour non transactionnelles sur le FileTable via le système de fichiers, il est possible que le contenu FILESTREAM soit verrouillé exclusivement par d'autres opérations Win32 et ne puisse pas être accessible en lecture/écriture via le corps du déclencheur. Dans ce cas, toute tentative d'accès au contenu FILESTREAM dans le corps du déclencheur peut provoquer une erreur de violation de partage. Les déclencheurs doivent être conçus pour gérer ces erreurs convenablement.  
  
    -   L'image AFTER du FILESTREAM peut ne pas être stable, car dans certains cas, d'autres mises à jour non transactionnelles peuvent y écrire de manière active en même temps (en raison des modes de partage autorisés dans l'accès au système de fichiers).  
  
-   Un arrêt anormal de descripteurs Win32, comme la suppression explicite de descripteurs Win32 par un administrateur OU un incident de base de données, n'exécute pas de déclencheurs d'utilisateur au cours des opérations de récupération, bien que le contenu FILESTREAM ait pu être modifié par l'application Win32 arrêtée anormalement.  
  
##  <a name="OtherViews"></a> Vues et FileTables  
 **Vues**  
 Une vue peut être créée sur un FileTable comme sur toute autre table. Toutefois, les considérations suivantes s'appliquent à une vue créée sur un FileTable :  
  
-   La vue n'aura pas de sémantique FileTable. autrement dit les colonnes de l’affichage (notamment les colonnes d’attributs de fichier) se comportent comme des colonnes d’affichage normales sans sémantique spéciale. Ceci est également vrai pour les lignes qui représentent des fichiers/répertoires.  
  
-   La vue peut être modifiable selon la sémantique de la « vue modifiable », mais les contraintes de table sous-jacentes peuvent refuser les mises à jour comme dans la table.  
  
-   Le chemin d'accès à un fichier peut être visualisé dans la vue en l'ajoutant en tant que colonne explicite dans la vue. Exemple :  
  
     `CREATE VIEW MP3FILES AS SELECT column1, column2, …, GetFileNamespacePath() AS PATH, column3,…  FROM Documents`  
  
 **Vues indexées**  
 Actuellement, les vues indexées ne peuvent pas inclure de colonnes FILESTREAM ni de colonnes calculées/calculées persistantes qui dépendent des colonnes FILESTREAM. Ce comportement reste également inchangé avec les vues définies sur le FileTable.  
  
##  <a name="OtherSnapshots"></a> Isolement de capture instantanée et FileTables  
 L'isolement de capture instantanée de lecture validée (RCSI) et l'isolement de capture instantanée (SI) comptent sur la possibilité d'avoir un instantané des données disponible pour les lecteurs même lorsque des opérations de mise à jour se produisent sur les données. Cependant, les FileTables autorisent l'accès en écriture non transactionnel aux données FILESTREAM. Par conséquent, les restrictions suivantes s'appliquent à l'utilisation de ces fonctionnalités dans les bases de données qui contiennent des FileTables :  
  
-   Une base de données qui contient des FileTables peut être modifiée pour activer l'isolement RCSI/SI.  
  
-   Lorsque l'accès non_transactional est défini sur FULL pour la base de données, une transaction s'exécutant sous RCSI ou SI a le comportement suivant :  
  
    -   Toute lecture [!INCLUDE[tsql](../../includes/tsql-md.md)] de la colonne file_stream du FileTable échoue. Toute opération INSERT et UPDATE sur la colonne aboutit tant qu'elle n'effectue pas de lecture depuis la colonne file_stream.  
  
    -   Si l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] spécifie des indicateurs de table READCOMMITTEDLOCK, les lectures aboutissent et prennent des verrous sur les lignes, plutôt que d'utiliser le contrôle de version de ligne.  
  
    -   Les requêtes d'ouverture du FileStream Win32 transactionnel échouent également.  
  
    -   L'accès à FileTable Win32 non transactionnel aboutit. Toutes les requêtes internes effectuées par FileTable ne sont pas affectées.  
  
    -   L'indexation de texte intégral réussit toujours, quelles que soient les options de base de données (READ_COMMITTED_SNAPSHOT ou ALLOW_SNAPSHOT_ISOLATION).  
  
##  <a name="readsec"></a> Bases de données secondaires accessibles en lecture  
 Les mêmes remarques s'appliquent aux bases de données secondaires accessibles en lecture et aux instantanés, comme décrit dans la section précédente, [Isolement de capture instantanée et FileTables](#OtherSnapshots).  
  
##  <a name="CDB"></a> Bases de données à relation contenant-contenu et FileTables  
 La fonctionnalité FILESTREAM, de laquelle la fonctionnalité FileTable dépend, requiert une configuration spécifique hors de la base de données. Par conséquent, une base de données qui utilise FILESTREAM ou FileTable n'est pas entièrement contenue.  
  
 Vous pouvez définir la relation contenant-contenu de la base de données sur PARTIAL si vous souhaitez utiliser certaines fonctionnalités des bases de données à relation contenant-contenu, telles que les utilisateurs contenus. Dans ce cas, toutefois, vous devez savoir qu'une partie des paramètres de la base de données ne sont pas contenus dans la base de données et ne sont pas automatiquement déplacés avec celle-ci.  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md)  
  
  
