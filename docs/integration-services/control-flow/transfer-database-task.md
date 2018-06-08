---
title: Transfert de bases de données, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.transferdatabasetask.f1
- sql13.dts.designer.transferdatabasetask.general.f1
- sql13.dts.designer.transferdatabasetask.database.f1
- sql13.dts.designer.transferdatabasetask.sourcedbfiles.f1
- sql13.dts.designer.transferdatabasetask.destdbfiles.f1
helpviewer_keywords:
- Transfer Database task [Integration Services]
ms.assetid: b9a2e460-cdbc-458f-8df8-06b8b2de3d67
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0b66f3db5c3c4017cc8022e6f1cc30f24347e803
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transfer-database-task"></a>Tâche de transfert de bases de données
  La tâche de transfert de bases de données transfère une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entre deux instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Contrairement aux autres tâches qui ne transfèrent les objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qu'en les copiant, la tâche de transfert de base de données peut copier ou déplacer une base de données. Cette tâche peut également servir à copier une base de données sur le même serveur.  
  
## <a name="offline-and-online-modes"></a>Modes en ligne et hors connexion  
 La base de données peut être transférée à l'aide du mode en ligne ou hors connexion. Lorsque vous utilisez le mode en ligne, la base de données reste attachée et est transférée à l'aide de SMO (SQL Management Object) pour copier les objets de base de données. Lorsque vous utilisez le mode hors connexion, la base de données est détachée, les fichiers de base de données sont copiés ou déplacés, et la base de données est attachée à la destination après que le transfert se soit terminé avec succès. Si la base de données est copiée, elle est automatiquement rattachée à la source si la copie a réussi. En mode hors connexion, la base de données est copiée plus rapidement, mais elle n'est pas disponible aux utilisateurs pendant le transfert.  
  
 Le mode hors connexion requiert la spécification des partages de fichiers réseau sur les serveurs source et destination contenant les fichiers de base de données. Si le dossier est partagé et est accessible à l’utilisateur, vous pouvez référencer le partage réseau à l’aide de la syntaxe \\\nom_ordinateur\Program Files\mon_dossier\\\. Sinon, vous devez utiliser la syntaxe \\\nom_ordinateur\c$\Program Files\mon_dossier\\\. Pour utiliser la dernière syntaxe, l'utilisateur doit avoir un accès en écriture vers les partages réseau source et destination.  
  
## <a name="transfer-of-databases-between-versions-of-sql-server"></a>Transférer des bases de données entre des versions de SQL Server  
 La tâche de transfert de bases de données peut transférer une base de données entre des instances de différentes versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="events"></a>Événements  
 La tâche de transfert de bases de données n'indique pas les stades intermédiaires de l'avancement du transfert des messages d'erreur : elle signale la tâche comme réalisée à 0 % ou à 100 %.  
  
## <a name="execution-value"></a>Valeur d'exécution  
 La valeur d'exécution, définie dans la propriété **ExecutionValue** de la tâche, renvoie la valeur 1, car contrairement aux autres tâches de transfert, la tâche de transfert de bases de données ne peut transférer qu'une seule base de données.  
  
 En affectant une variable définie par l’utilisateur à la propriété **ExecValueVariable** de la tâche de transfert de bases de données, les informations sur le transfert de messages d’erreur peuvent être rendues disponibles aux autres objets du package. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  
  
## <a name="log-entries"></a>Entrées du journal  
 La tâche de transfert de bases de données comporte les entrées de journal personnalisées suivantes :  
  
-   SourceSQLServer    Cette entrée du journal indique le nom du serveur source.  
  
-   DestSQLServer    Cette entrée du journal indique le nom du serveur de destination.  
  
-   SourceDB    Cette entrée du journal indique le nom de la base de données transférée.  
  
 En outre, une entrée de journal pour l'événement **OnInformation** est écrite lorsque la base de données de destination est remplacée.  
  
## <a name="security-and-permissions"></a>Sécurité et autorisations  
 Pour transférer une base de données à l'aide du mode hors connexion, l'utilisateur qui exécute le package doit être un membre du rôle de serveur sysadmin.  
  
 Pour transférer une base de données à l'aide du mode en ligne, l'utilisateur qui exécute le package doit être un membre du rôle de serveur sysadmin ou le propriétaire (dbo) de la base de données sélectionnée.  
  
## <a name="configuration-of-the-transfer-database-task"></a>Configuration de la tâche de transfert de bases de données  
 Vous pouvez spécifier si la tâche doit tenter de rattacher la base de données source lorsque le transfert de base de données échoue.  
  
 La tâche de transfert de base de données peut également être configurée afin de permettre le remplacement d'une base de données de destination portant le même nom.  
  
 La base de données source peut également être renommée lors du processus de transfert. Si vous voulez transférer une base de données vers une instance de destination de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenant déjà une base de données portant le même nom, le changement de nom de la base de données source permet le transfert de cette base de données. Cependant, les noms de fichiers de base de données doivent également être différents. Si des fichiers de base de données portant le même nom existent déjà sur la destination, la tâche échoue.  
  
 Lorsque vous copiez une base de données, sa taille ne peut pas être inférieure à la taille de la base de données **model** sur le serveur de destination. Vous pouvez soit augmenter la taille de la base de données à copier, soit réduire la taille de la base de données **model**.  
  
 À l'exécution, la tâche de transfert de base de données se connecte aux serveurs source et destination en utilisant un ou deux gestionnaires de connexions SMO. Lorsque vous créez une copie de base de données sur le même serveur, un seul gestionnaire de connexions SMO est requis. Les gestionnaires de connexions SMO sont configurés indépendamment de la tâche de transfert de bases de données, puis sont référencés dans cette tâche. Les gestionnaires de connexions SMO spécifient le serveur et le mode d'authentification à utiliser lorsque la tâche accède au serveur. Pour plus d'informations, consultez [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="programmatic-configuration-of-the-transfer-database-task"></a>Configuration par programmation de la tâche de transfert de bases de données  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.TransferDatabaseTask.TransferDatabaseTask>  
  
## <a name="transfer-database-task-editor-general-page"></a>Éditeur de tâche de transfert de bases de données (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de transfert de bases de données** pour donner un nom et une description à la tâche de transfert de base de données. La tâche de transfert de bases de données copie ou déplace une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entre deux instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette tâche peut également servir à copier une base de données sur le même serveur.   
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique à la tâche de transfert de bases de données. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche de transfert de bases de données.  
  
## <a name="transfer-database-task-editor-databases-page"></a>Éditeur de tâche de transfert de bases de données (page Bases de données)
  Utilisez la page **Bases de données** de la boîte de dialogue **Éditeur de tâche de transfert de bases de données** pour spécifier les propriétés des bases de données source et de destination concernées par la tâche de transfert de bases de données. La tâche de transfert de bases de données copie ou déplace une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entre deux instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette tâche peut également servir à copier une base de données sur le même serveur.  
  
### <a name="options"></a>Options  
 **SourceConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur source.  
  
 **DestinationConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste, ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur de destination.  
  
 **DestinationDatabaseName**  
 Spécifiez le nom de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur de destination.  
  
 Pour remplir automatiquement ce champ avec le nom de la base de données source, spécifiez d’abord **SourceConnection** et **SourceDatabaseName** .  
  
 Pour renommer la base de données sur le serveur de destination, tapez son nouveau nom dans ce champ.  
  
 **DestinationDatabaseFiles**  
 Spécifiez le nom et l'emplacement des fichiers de base de données sur le serveur de destination.  
  
 Pour remplir automatiquement ce champ avec le nom de la base de données source, spécifiez d’abord **SourceConnection**, **SourceDatabaseName**et **SourceDatabaseFiles** .  
  
 Pour renommer les fichiers de base de données ou pour spécifier les nouveaux emplacements sur le serveur de destination, remplissez ce champ avec les informations sur la base de données source, puis cliquez sur le bouton Parcourir. Dans la boîte de dialogue **Fichiers de la base de données de destination** , modifiez **Fichier de destination**, **Dossier de destination**ou **Partage de fichiers réseau**.  
  
> [!NOTE]  
>  Si vous trouvez l’emplacement des fichiers de la base de données à l’aide du bouton Parcourir, l’emplacement des fichiers est entré en utilisant la notation de lecteur local : par exemple, c:\\. Vous devez remplacer cette notation par la notation de partage réseau, qui comporte le nom de l'ordinateur et le nom du partage. Si le partage administratif par défaut est utilisé, vous devez utiliser la notation $ et disposer d'un accès administratif au partage.  
  
 **DestinationOverwrite**  
 Spécifiez si la base de données sur le serveur de destination peut être remplacée.  
  
 Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Remplace la base de données du serveur de destination.|  
|**False**|Ne remplace pas la base de données du serveur de destination.|  
  
> [!CAUTION]  
>  Les données de la base de données du serveur de destination seront remplacées si vous spécifiez **True** pour **DestinationOverwrite**, ce qui peut provoquer la perte de données. Pour l'éviter, sauvegardez la base de données du serveur de destination dans un autre emplacement avant d'exécuter la tâche de transfert de bases de données.  
  
 **Action**  
 Spécifiez si la tâche doit **Copier** ou **Déplacer** la base de données sur le serveur de destination.  
  
 **Méthode**  
 Spécifiez si la tâche est exécutée pendant que la base de données sur le serveur source est en ligne ou en mode hors connexion.  
  
 Pour transférer une base de données en mode hors connexion, l’utilisateur qui exécute le package doit être membre du rôle serveur fixe **sysadmin** .  
  
 Pour transférer une base de données en mode en ligne, l’utilisateur qui exécute le package doit être membre du rôle serveur fixe **sysadmin** ou propriétaire de la base de données sélectionnée (**dbo**).  
  
 **SourceDatabaseName**  
 Sélectionnez le nom de la base de données à copier ou à déplacer.  
  
 **SourceDatabaseFiles**  
 Cliquez sur le bouton Parcourir pour sélectionner les fichiers de base de données.  
  
 **ReattachSourceDatabase**  
 Spécifiez si la tâche va tenter de rattacher la base de données source en cas d'échec.  
  
 Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Rattache la base de données source.|  
|**False**|Ne rattache pas la base de données source.|  

## <a name="source-database-files"></a>Fichiers de la base de données source
  Utilisez la boîte de dialogue **Fichiers de la base de données source** pour afficher le nom et l'emplacement des fichiers de la base de données sur le serveur source ou pour spécifier un emplacement de partage de fichiers réseau pour la tâche de transfert de bases de données.   
  
 Pour remplir cette boîte de dialogue avec le nom et l'emplacement des fichiers de base de données sur le serveur source, spécifiez d'abord **SourceConnection** et **SourceDatabaseName** dans la page **Bases de données** de la boîte de dialogue **Éditeur de tâche de transfert de bases de données** .  
  
### <a name="options"></a>Options  
 **Source File**  
 Nom des fichiers de base de données sur le serveur source qui seront transférés. Le**Fichier source** est en lecture seule.  
  
 **Dossier source**  
 Dossier du serveur source où se trouvent les fichiers de base de données à transférer. Le**Dossier source** est en lecture seule.  
  
 **Partage de fichiers réseau**  
 Dossier réseau partagé du serveur source à partir duquel les fichiers de base de données seront transférés. Utilisez le **Partage de fichiers réseau** lors du transfert d'une base de données en mode hors connexion en spécifiant **DatabaseOffline** pour **Méthode** dans la page **Bases de données** de la boîte de dialogue **Éditeur de tâche de transfert de bases de données** .  
  
 Entrez l’emplacement du partage de fichiers réseau ou cliquez sur le bouton Parcourir **(…)** pour le rechercher.  
  
 Lors du transfert d'une base de données en mode hors connexion, les fichiers de base de données sont copiés dans l'emplacement du **Partage de fichiers réseau** sur le serveur source avant d'être transférés sur le serveur de destination.  

## <a name="destination-database-files"></a>Fichiers de la base de données de destination
  Utilisez la boîte de dialogue **Fichiers de la base de données de destination** pour afficher ou modifier le nom et l'emplacement des fichiers de la base de données sur le serveur de destination ou pour spécifier un emplacement de fichier réseau pour la tâche de transfert de bases de données.  
  
 Pour remplir automatiquement cette boîte de dialogue avec le nom et l'emplacement des fichiers de base de données sur le serveur source, spécifiez d'abord **SourceConnection**, **SourceDatabaseName**et **SourceDatabaseFiles** dans la page **Bases de données** de la boîte de dialogue **Éditeur de tâche de transfert de bases de données** .  
  
### <a name="options"></a>Options  
 **Fichier de destination**  
 Nom des fichiers de la base de données transférés sur le serveur de destination.  
  
 Entrez le nom du fichier ou cliquez dessus pour le modifier.  
  
 **Dossier de destination**  
 Dossier du serveur de destination où les fichiers de la base de données seront transférés.  
  
 Entrez le chemin d'accès au dossier, cliquez dessus pour le modifier ou cliquez sur Parcourir pour rechercher le dossier dans lequel transférer les fichiers de la base de données sur le serveur de destination.  
  
 **Partage de fichiers réseau**  
 Dossier réseau partagé du serveur de destination où les fichiers de la base de données seront transférés. Utilisez le **Partage de fichiers réseau** lors du transfert d'une base de données en mode hors connexion en spécifiant **DatabaseOffline** pour **Méthode** dans la page **Bases de données** de la boîte de dialogue **Éditeur de tâche de transfert de bases de données** .  
  
 Entrez l'emplacement du partage de fichiers réseau ou cliquez sur le bouton Parcourir pour le rechercher.  
  
 Lors du transfert d'une base de données en mode hors connexion, les fichiers de base de données sont copiés dans l'emplacement du **Partage de fichiers réseau** avant d'être transférés dans l'emplacement **Dossier de destination** .  
