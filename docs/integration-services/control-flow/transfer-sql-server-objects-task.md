---
title: Transfert d’objets SQL Server, tâche | Microsoft Docs
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
- sql13.dts.designer.transfersqlserverobjectstask.f1
- sql13.dts.designer.transfersqlserverobjects.general.f1
- sql13.dts.designer.transfersqlserverobjects.objects.f1
helpviewer_keywords:
- Transfer SQL Server Objects task [Integration Services]
ms.assetid: fe86d6e5-e415-406c-88f3-dc3ef71bd5f0
caps.latest.revision: 35
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 09d1dffe9ae8dbb9b94f3738e0b382cd658d35d1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transfer-sql-server-objects-task"></a>Tâche de transfert d'objets SQL Server
  La tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] transfère un ou plusieurs types d’objets d’une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entre des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par exemple, la tâche peut copier des tables et des procédures stockées. Selon la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée comme source, différents types d’objets sont disponibles pour la copie. Par exemple, seule une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut des schémas et des agrégats définis par l’utilisateur.  
  
## <a name="objects-to-transfer"></a>Objets à transférer  
 Les rôles de serveur, les rôles et les utilisateurs de la base de données spécifiée peuvent être copiés, ainsi que les autorisations pour les objets transférés. En copiant les utilisateurs, les rôles et les autorisations associés avec les objets, les objets transférés peuvent être opérationnels immédiatement sur le serveur de destination.  
  
 Le tableau suivant présente la liste des types d'objets qui peuvent être copiés.  
  
|Object|  
|------------|  
|Tables|  
|Vues|  
|Procédures stockées|  
|Fonctions définies par l'utilisateur|  
|Valeurs par défaut|  
|Types de données définis par l’utilisateur|  
|Fonctions de partition|  
|Schémas de partition|  
|Schémas|  
|Assemblys|  
|Agrégats définis par l'utilisateur|  
|Types définis par l'utilisateur|  
|Collection de schémas XML|  
  
 Les types définis par l'utilisateur (UDT) qui ont été créés dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possèdent des dépendances avec les assemblys CLR (Common Language Runtime). Si vous utilisez la tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour transférer des types UDT, vous devez également configurer la tâche de transfert d'objets dépendants. Pour transférer des objets dépendants, définissez la propriété **IncludeDependentObjects** sur **True**.  
  
### <a name="table-options"></a>Options des tables  
 Lors de la copie de tables, vous pouvez indiquer les types des éléments associés aux tables à inclure dans le processus de copie. Les types d'éléments suivants peuvent être copiés avec la table associée :  
  
-   Index  
  
-   Déclencheurs  
  
-   Index de texte intégral  
  
-   Clés primaires  
  
-   Clés étrangères  
  
 Vous pouvez également indiquer si le script généré par la tâche est au format Unicode.  
  
## <a name="destination-options"></a>Options de destination  
 Vous pouvez configurer la tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour qu'elle inclue des noms de schéma, des données, des propriétés étendues d'objets transférés et des objets dépendants dans le transfert. Si des données sont copiées, la tâche peut remplacer les données existantes ou les conserver.  
  
 Certaines options ne s'appliquent qu'à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par exemple, seul [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les schémas.  
  
## <a name="security-options"></a>Options de sécurité  
 La tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut inclure des utilisateurs et rôles au niveau de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la source, des connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ainsi que les autorisations pour les objets transférés. Par exemple, le transfert peut inclure les autorisations sur les tables transférées.  
  
## <a name="transfer-objects-between-instances-of-sql-server"></a>Transférer des objets entre des instances de SQL Server  
 La tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge une source et une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="events"></a>Événements  
 La tâche génère un événement d'information qui indique l'objet transféré et un événement d'avertissement lorsque qu'un objet est remplacé. Un événement d'information est également généré pour les actions telles que la troncation des tables de base de données.  
  
 La tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’indique pas les stades intermédiaires de l’avancement du transfert des objets : elle signale uniquement la tâche comme réalisée à 0 % ou à 100 %.  
  
## <a name="execution-value"></a>Valeur d'exécution  
 La valeur d’exécution, stockée dans la propriété **ExecutionValue** de la tâche, retourne le nombre d’objets transférés. En affectant une variable définie par l’utilisateur à la propriété **ExecValueVariable** de la tâche de transfert d’objets SQL Server, les informations sur le transfert d’objets peuvent être mises à la disposition d’autres objets du package. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  
  
## <a name="log-entries"></a>Entrées du journal  
 La tâche de transfert d'objets SQL Server comporte les entrées de journal personnalisées suivantes :  
  
-   TransferSqlServerObjectsTaskStartTransferringObjects   Cette entrée du journal indique que le transfert a commencé. L'entrée du journal inclut l'heure de début.  
  
-   TransferSqlServerObjectsTaskFinishedTransferringObjects   Cette entrée du journal indique que le transfert est terminé. L'entrée du journal inclut l'heure de fin.  
  
 En outre, une entrée de journal pour l’événement **OnInformation** indique le nombre d’objets pour les types d’objet qui ont été sélectionnés pour le transfert, le nombre d’objets transférés, ainsi que les actions telles que la troncation de tables lorsque des données sont transférées avec les tables. Une entrée de journal pour l’événement **OnWarning** est écrite pour chaque objet de destination remplacé.  
  
## <a name="security-and-permissions"></a>Sécurité et autorisations  
 L'utilisateur doit avoir l'autorisation de parcourir les objets sur le serveur source et de supprimer ou de créer des objets sur le serveur de destination. L'utilisateur doit en outre avoir accès à la base de données spécifiée et aux objets de base de données.  
  
## <a name="configuration-of-the-transfer-sql-server-objects-task"></a>Configuration de la tâche de transfert d'objets SQL Server  
 La tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être configurée pour transférer tous les objets, tous les objets d'un certain type, ou seulement les objets spécifiés d'un type. Par exemple, vous pouvez choisir de ne copier que les tables sélectionnées dans la base de données AdventureWorks.  
  
 Si la tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] transfère des tables, vous pouvez spécifier les types des objets associés aux tables à copier avec les tables. Par exemple, vous pouvez spécifier que les clés primaires sont copiées avec les tables.  
  
 Pour améliorer la fonctionnalité des objets transférés, vous pouvez configurer la tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour qu'elle inclue des noms de schéma, des données, des propriétés étendues d'objets transférés et des objets dépendants dans le transfert. Lors de la copie de données, vous pouvez spécifier s'il faut remplacer ou ajouter les données existantes.  
  
 À l’exécution, la tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se connecte aux serveurs source et de destination en utilisant deux gestionnaires de connexions SMO. Les gestionnaires de connexions SMO sont configurés indépendamment de la tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis référencés dans la tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les gestionnaires de connexions SMO spécifient le serveur et le mode d'authentification à utiliser lors de l'accès au serveur. Pour plus d'informations, consultez [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="programmatic-configuration-of-the-transfer-sql-server-objects-task"></a>Configuration par programmation de la tâche de transfert d'objets SQL Server  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.TransferSqlServerObjectsTask.TransferSqlServerObjectsTask>  
  
  
## <a name="transfer-sql-server-objects-task-editor-general-page"></a>Éditeur de tâche de transfert d'objets SQL (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de transfert d'objets SQL** pour donner un nom et une description à la tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  L’utilisateur qui crée la tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir les autorisations suffisantes sur les objets du serveur source pour pouvoir les sélectionner pour la copie, ainsi que l’autorisation d’accéder à la base de données du serveur de destination où les objets seront transférés.  
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique à la tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="transfer-sql-server-objects-task-editor-objects-page"></a>Éditeur de tâche de transfert d'objets SQL (page Objets)
  Utilisez la page **Objets** de la boîte de dialogue **Éditeur de tâche de transfert d’objets SQL** pour spécifier les propriétés de copie d’un ou plusieurs objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à une autre. Les tables, les vues, les procédures stockées et les fonctions définies par l’utilisateur représentent quelques exemples des objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous pouvez copier.  
  
> [!NOTE]  
>  L’utilisateur qui crée la tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit disposer des autorisations suffisantes sur les objets du serveur source pour les sélectionner pour la copie, ainsi que de l’autorisation d’accéder à la base de données du serveur de destination où les objets seront transférés.  
  
### <a name="static-options"></a>Options statiques  
 **SourceConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur source.  
  
 **SourceDatabase**  
 Sélectionnez une base de données sur le serveur source à partir duquel les objets seront copiés.  
  
 **DestinationConnection**  
 Sélectionnez un gestionnaire de connexions SMO dans la liste, ou cliquez sur **\<Nouvelle connexion...>** pour créer une connexion au serveur de destination.  
  
 **DestinationDatabase**  
 Sélectionnez une base de données sur le serveur de destination dans lequel les objets seront copiés.  
  
 **DropObjectsFirst**  
 Déterminez si les objets sélectionnés sont d'abord supprimés du serveur de destination avant la copie.  
  
 **IncludeExtendedProperties**  
 Déterminez si les propriétés étendues sont incluses lorsque les objets sont copiés du serveur source au serveur de destination.  
  
 **CopyData**  
 Déterminez si les données sont incluses lorsque les objets sont copiés du serveur source au serveur de destination.  
  
 **ExistingData**  
 Spécifiez comment les données seront copiées sur le serveur de destination. Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Remplacer**|Les données du serveur de destination seront remplacées.|  
|**Append**|Les données copiées à partir du serveur source seront ajoutées aux données existantes sur le serveur de destination.|  
  
> [!NOTE]  
>  L’option **ExistingData** est disponible uniquement quand **CopyData** a la valeur **True**.  
  
 **CopySchema**  
 Déterminez si le schéma est copié pendant la tâche de transfert d’objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  **CopySchema** est disponible uniquement pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **UseCollation**  
 Déterminez si le transfert des objets doit inclure le classement spécifié sur le serveur source.  
  
 **IncludeDependentObjects**  
 Déterminez si la copie des objets sélectionnés inclut en cascade les autres objets qui dépendent de ces objets sélectionnés pour la copie.  
  
 **CopyAllObjects**  
 Déterminez si la tâche copie tous les objets de la base de données source spécifiée ou uniquement les objets sélectionnés.  Si cette option a la valeur False, vous pouvez sélectionner les objets à transférer et afficher les options dynamiques dans la section **CopyAllObjects**.  
  
 **ObjectsToCopy**  
 Développez **ObjectsToCopy** pour spécifier les objets qui doivent être copiés de la base de données source dans la base de données de destination.  
  
> [!NOTE]  
>  **ObjectsToCopy** est disponible uniquement quand **CopyAllObjects** a la valeur **False**.  
  
 Les options de copie des types d’objets suivants sont prises en charge uniquement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
 Assemblys  
  
 Fonctions de partition  
  
 Schémas de partition  
  
 Schémas  
  
 Fonctions d'agrégation définies par l'utilisateur  
  
 Types définis par l'utilisateur  
  
 Collections de schémas XML  
  
 **CopyDatabaseUsers**  
 Déterminez si les utilisateurs de la base de données doivent être inclus dans le transfert.  
  
 **CopyDatabaseRoles**  
 Déterminez si les rôles de la base de données doivent être inclus dans le transfert.  
  
 **CopySqlServerLogins**  
 Déterminez si les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent être incluses dans le transfert.  
  
 **CopyObjectLevelPermissions**  
 Déterminez si les autorisations au niveau objet doivent être incluses dans le transfert.  
  
 **CopyIndexes**  
 Déterminez si les index doivent être inclus dans le transfert.  
  
 **CopyTriggers**  
 Déterminez si les déclencheurs doivent être inclus dans le transfert.  
  
 **CopyFullTextIndexes**  
 Déterminez si les index de texte intégral doivent être inclus dans le transfert.  
  
 **CopyPrimaryKeys**  
 Déterminez si les clés primaires doivent être incluses dans le transfert.  
  
 **CopyForeignKeys**  
 Déterminez si les clés étrangères doivent être incluses dans le transfert.  
  
 **GenerateScriptsInUnicode**  
 Déterminez si les scripts de transfert créés sont au format Unicode.  
  
### <a name="dynamic-options"></a>Options dynamiques  
  
#### <a name="copyallobjects--false"></a>CopyAllObjects = False  
 **CopyAllTables**  
 Déterminez si la tâche copie toutes les tables de la base de données source spécifiée ou uniquement les tables sélectionnées.  
  
 **TablesList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner des tables** .  
  
 **CopyAllViews**  
 Déterminez si la tâche copie toutes les vues de la base de données source spécifiée ou uniquement les vues sélectionnées.  
  
 **ViewsList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner des vues** .  
  
 **CopyAllStoredProcedures**  
 Déterminez si la tâche copie toutes les procédures stockées définies par l'utilisateur dans la base de données source spécifiée ou uniquement celles sélectionnées.  
  
 **StoredProceduresList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner des procédures stockées** .  
  
 **CopyAllUserDefinedFunctions**  
 Déterminez si la tâche copie toutes les fonctions définies par l'utilisateur dans la base de données source spécifiée ou uniquement celles sélectionnées.  
  
 **UserDefinedFunctionsList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les fonctions définies par l’utilisateur** .  
  
 **CopyAllDefaults**  
 Déterminez si la tâche copie toutes les valeurs par défaut de la base de données source spécifiée ou uniquement les valeurs par défaut sélectionnées.  
  
 **DefaultsList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les valeurs par défaut** .  
  
 **CopyAllUserDefinedDataTypes**  
 Déterminez si la tâche copie tous les types de données définis par l'utilisateur dans la base de données source spécifiée ou uniquement ceux sélectionnés.  
  
 **UserDefinedDataTypesList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les types de données définis par l’utilisateur** .  
  
 **CopyAllPartitionFunctions**  
 Déterminez si la tâche copie toutes les fonctions de partition définies par l'utilisateur dans la base de données source spécifiée ou uniquement celles sélectionnées. Cette option est prise en charge uniquement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **PartitionFunctionsList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les fonctions de partition** .  
  
 **CopyAllPartitionSchemes**  
 Déterminez si la tâche copie tous les schémas de partition de la base de données source spécifiée ou uniquement ceux sélectionnés. Cette option est prise en charge uniquement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **PartitionSchemesList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les schémas de partition** .  
  
 **CopyAllSchemas**  
 Déterminez si la tâche copie tous les schémas de la base de données source spécifiée ou uniquement les schémas sélectionnés. Cette option est prise en charge uniquement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **SchemasList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les schémas** .  
  
 **CopyAllSqlAssemblies**  
 Déterminez si la tâche copie tous les assemblys SQL de la base de données source spécifiée ou uniquement les assemblys SQL sélectionnés. Cette option est prise en charge uniquement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **SqlAssembliesList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les assemblys SQL** .  
  
 **CopyAllUserDefinedAggregates**  
 Déterminez si la tâche copie toutes les fonctions d'agrégation définies par l'utilisateur dans la base de données source spécifiée ou uniquement celles sélectionnées. Cette option est prise en charge uniquement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **UserDefinedAggregatesList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les fonctions d’agrégation définies par l’utilisateur** .  
  
 **CopyAllUserDefinedTypes**  
 Déterminez si la tâche copie tous les types définis par l'utilisateur dans la base de données source spécifiée ou uniquement ceux sélectionnés. Cette option est prise en charge uniquement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **UserDefinedTypes**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les types définis par l’utilisateur** .  
  
 **CopyAllXmlSchemaCollections**  
 Déterminez si la tâche copie toutes les collections de schémas XML de la base de données source spécifiée ou uniquement celles sélectionnées. Cette option est prise en charge uniquement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **XmlSchemaCollectionsList**  
 Cliquez pour ouvrir la boîte de dialogue **Sélectionner les collections du schéma XML** .  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des messages propres à Integration Services](../../integration-services/integration-services-error-and-message-reference.md)   
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Éditeur de tâche de transfert d’objets SQL Server &#40;page Général&#41;](../../integration-services/control-flow/transfer-sql-server-objects-task-editor-general-page.md)   
 [Page Expressions](../../integration-services/expressions/expressions-page.md)   
 [Formats de données pour l’importation ou l’exportation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)   
 [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
