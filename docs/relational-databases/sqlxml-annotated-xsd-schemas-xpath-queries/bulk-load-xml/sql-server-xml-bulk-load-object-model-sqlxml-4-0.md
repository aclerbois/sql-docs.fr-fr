---
title: SQL Server en bloc charge modèle d’objet XML (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- bulk load [SQLXML], object model
- ErrorLogFile property
- SGDropTables property
- SGUseID property
- KeepNulls property
- ConnectionCommand property
- SchemaGen property
- XMLFragment property
- SQLXMLBulkLoad object
- ForceTableLock property
- CheckConstraints property
- BulkLoad property
- TempFilePath property
- IgnoreDuplicateKeys property
- Transaction property
- KeepIdentity property
- ConnectionString property
- FireTriggers property
- Execute method
- XML Bulk Load [SQLXML], object model
ms.assetid: a9efbbde-ed2b-4929-acc1-261acaaed19d
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 43ca8f3b345f5db9c0d11217caef744e9172bd22
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-xml-bulk-load-object-model-sqlxml-40"></a>Modèle objet de chargement en masse XML de SQL Server (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] modèle d’objet de chargement en masse XML se compose de l’objet SQLXMLBulkLoad. Cet objet prend en charge les méthodes et les propriétés suivantes.  
  
## <a name="methods"></a>Méthodes  
 Execute  
 Procède au chargement en masse des données à l'aide du fichier de schéma et du fichier de données (ou flux) fournis en guise de paramètres.  
  
## <a name="properties"></a>Propriétés  
 Chargement en masse  
 Spécifie si un chargement en masse doit avoir lieu. Cette propriété est utile si vous souhaitez générer uniquement les schémas (consultez les propriétés SchemaGen, SGDropTables et SGUseID qui suivent) et de pas effectuer un chargement en masse. Il s'agit d'une propriété booléenne. Lorsque la propriété est définie sur TRUE, le processus de chargement en masse XML s'exécute. Lorsqu'elle est définie sur FALSE, aucun processus de chargement en masse XML n'a lieu.  
  
 La valeur par défaut est TRUE.  
  
 CheckConstraints  
 Spécifie si les contraintes (telles que les contraintes découlant de la relation entre clé primaire et clé étrangère dans les colonnes) spécifiées sur la colonne doivent être vérifiées lorsque le chargement en masse XML insère des données dans les colonnes. Il s'agit d'une propriété booléenne.  
  
 Lorsque la propriété est définie sur TRUE, le chargement en masse XML recherche chaque valeur insérée dans les contraintes (ce qui signifie qu'une violation de contrainte entraîne une erreur).  
  
> [!NOTE]  
>  Pour laisser cette propriété définie sur FALSE, vous devez avoir **ALTER TABLE** les autorisations sur les tables cibles. Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md).  
  
 La valeur par défaut est FALSE. Lorsqu'il est défini sur FALSE, le chargement en masse XML ignore les contraintes au cours d'une opération d'insertion. Dans l'implémentation actuelle, vous devez définir les tables dans l'ordre des relations entre clés primaires et clés étrangères dans le schéma de mappage. Autrement dit, une table dotée d'une clé primaire doit être définie avant la table avec clé étrangère correspondante sans quoi le chargement en masse XML se soldera par un échec.  
  
 Notez que si vous procédez à une propagation d'identité, cette option ne s'applique pas et la vérification des contraintes restera activée. Ceci se produit lorsque `KeepIdentity=False` et qu'il existe une relation où le parent est un champ d'identité et la valeur est attribuée à l'enfant telle qu'elle est générée.  
  
 ConnectionCommand  
 Identifie un objet de connexion existant (par exemple, l’objet command ADO ou ICommand) que le chargement en masse XML doit utiliser. Vous pouvez utiliser la propriété ConnectionCommand au lieu de spécifier une chaîne de connexion avec la propriété ConnectionString. La propriété de Transaction doit être définie sur TRUE si vous utilisez ConnectionCommand.  
  
 Si vous utilisez les propriétés ConnectionString et de ConnectionCommand, chargement en masse XML utilise la dernière propriété spécifiée.  
  
 La valeur par défaut est NULL.  
  
 ConnectionString  
 Identifie la chaîne de connexion OLE DB qui fournit les informations nécessaires pour établir une connexion à une instance de la base de données. Si vous utilisez les propriétés ConnectionString et de ConnectionCommand, chargement en masse XML utilise la dernière propriété spécifiée.  
  
 La valeur par défaut est NULL.  
  
 ErrorLogFile  
 Spécifie le nom de fichier dans lequel le chargement en masse XML enregistre des erreurs et des messages. La valeur par défaut est une chaîne vide, auquel cas aucun enregistrement n'a lieu.  
  
 FireTriggers  
 Spécifie si les déclencheurs définis dans les tables cibles doivent être déclenchés pendant l'opération de chargement en masse. La valeur par défaut est FALSE.  
  
 Si la valeur est TRUE, les déclencheurs seront activés comme d'habitude lors des opérations d'insertion.  
  
> [!NOTE]  
>  Pour laisser cette propriété définie sur FALSE, vous devez avoir **ALTER TABLE** les autorisations sur les tables cibles. Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md).  
  
 Notez que si vous ne procédez à aucune propagation d'identité, cette option ne s'applique pas et les déclencheurs resteront activés. Ceci se produit lorsque `KeepIdentity=False` et qu'il existe une relation où le parent est un champ d'identité et la valeur est attribuée à l'enfant telle qu'elle est générée.  
  
 ForceTableLock  
 Spécifie si les tables dans lesquelles le chargement en masse XML copie des données doivent être verrouillées pour la durée du chargement en masse. Il s'agit d'une propriété booléenne. Lorsque la propriété est définie sur TRUE, le chargement en masse XML acquiert des verrous de table pendant toute la durée du chargement en masse. Si elle est définie sur FALSE, le chargement en masse XML acquiert un verrou de table chaque fois qu'il insère un enregistrement dans une table.  
  
 La valeur par défaut est FALSE.  
  
 IgnoreDuplicateKeys  
 Spécifie la démarche à suivre en cas de tentative d'insertion de valeurs dupliquées dans une colonne clé. Si cette propriété est définie sur TRUE et si une tentative d'insertion d'un enregistrement avec valeur dupliquée dans une colonne clé a lieu, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'insère pas cet enregistrement. En revanche, il insère l'enregistrement suivant ; l'opération de chargement en masse ne peut donc pas aboutir à un échec. Si cette propriété est définie sur FALSE, le chargement en masse échoue dès qu'une tentative d'insertion d'une valeur dupliquée dans une colonne clé a lieu.  
  
 Lorsque la propriété IgnoreDuplicateKeys est définie sur TRUE, une instruction COMMIT est émise pour chaque enregistrement inséré dans la table. ce qui ralentit les performances. La propriété peut être définie sur « true » uniquement lorsque la propriété Transaction a la valeur False, car le comportement transactionnel est implémenté à l’aide de fichiers.  
  
 La valeur par défaut est FALSE.  
  
 KeepIdentity  
 Spécifie comment traiter les valeurs pour une colonne de type d'identité dans le fichier source. Il s'agit d'une propriété booléenne. Lorsque la propriété est définie sur TRUE, le chargement en masse XML affecte les valeurs spécifiées dans le fichier source à la colonne d'identité. Lorsque la propriété est définie sur FALSE, l'opération de chargement en masse ignore les valeurs de la colonne d'identité spécifiées dans la source. Dans ce cas, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] affecte une valeur à la colonne d'identité.  
  
 Si le chargement en masse implique une colonne désignant une clé étrangère qui fait référence à une colonne d'identité dans laquelle les valeurs générées par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont stockées, le chargement en masse propage comme il se doit ces valeurs d'identité dans la colonne de clé étrangère.  
  
 La valeur de cette propriété s'applique à toutes les colonnes qui interviennent dans le chargement en masse. La valeur par défaut est TRUE.  
  
> [!NOTE]  
>  Pour laisser cette propriété définie sur TRUE, vous devez avoir **ALTER TABLE** les autorisations sur les tables cibles. Sinon, vous devez lui attribuer la valeur FALSE. Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md).  
  
 KeepNulls  
 Spécifie la valeur à utiliser pour une colonne pour laquelle un attribut ou un élément enfant correspondant est manquant dans le document XML. Il s'agit d'une propriété booléenne. Lorsque la propriété est définie sur TRUE, le chargement en masse XML affecte une valeur NULL à la colonne. Il n'affecte pas la valeur par défaut de la colonne (le cas échéant) telle que définie sur le serveur. La valeur de cette propriété s'applique à toutes les colonnes qui interviennent dans le chargement en masse.  
  
 La valeur par défaut est FALSE.  
  
 SchemaGen  
 Spécifie si les tables requises doivent être créées avant une opération de chargement en masse. Il s'agit d'une propriété booléenne. Si cette propriété est définie sur TRUE, les tables identifiées dans le schéma de mappage sont créées (la base de données doit exister). Si un ou plusieurs des tables existent déjà dans la base de données, la sgdroptables, propriété détermine si ces tables préexistantes doivent être supprimées et recréées.  
  
 La valeur par défaut pour la propriété SchemaGen est FALSE. SchemaGen ne crée pas de contraintes de clé primaire sur les tables nouvellement créés. SchemaGen est le cas, toutefois, créer des contraintes FOREIGN KEY dans la base de données s’il peut trouver de correspondance **SQL : Relationship** et **SQL : Key-champs** annotations dans le schéma de mappage et si le champ clé se compose d’une seule colonne.  
  
 Notez que si vous définissez la propriété SchemaGen sur TRUE, le chargement en masse XML effectue les opérations suivantes :  
  
-   Il crée les tables nécessaires à partir des noms d'élément et d'attribut. C'est pourquoi il est primordial que vous n'utilisiez pas de mots réservés [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour les noms d'élément et d'attribut dans le schéma.  
  
-   Retourne les données de toutes les colonnes désignées à l’aide de dépassement de capacité du [SQL : Overflow-champ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-overflow-field.md) dans [type de données xml](../../../t-sql/xml/xml-transact-sql.md) format.  
  
 SGDropTables  
 Spécifie si les tables existantes doivent être supprimées et recréées. Vous utilisez cette propriété lorsque la propriété SchemaGen est définie sur TRUE. Si SGDropTables est FALSE, les tables existantes sont conservées. Les tables existantes sont supprimées et recréées lorsque cette propriété a la valeur TRUE.  
  
 La valeur par défaut est FALSE.  
  
 SGUseID  
 Spécifie si l’attribut dans le schéma de mappage qui est identifié en tant que **id** type peut être utilisé pour la création d’une contrainte PRIMARY KEY lorsque la table est créée. Utilisez cette propriété lorsque la propriété SchemaGen est définie sur TRUE. Si SGUseID est TRUE, l’utilitaire SchemaGen utilise un attribut pour lequel **dt : type = « id »** est spécifié en tant que colonne de clé primaire et ajoute la contrainte PRIMARY KEY appropriée lors de la création de la table.  
  
 La valeur par défaut est FALSE.  
  
 TempFilePath  
 Spécifie le chemin d'accès où le chargement en masse XML crée les fichiers temporaires pour un chargement en masse transactionnel. (Cette propriété est utile uniquement lorsque la propriété Transaction a la valeur TRUE.) Vous devez vous assurer que le compte [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] employé pour le chargement en masse XML a accès à ce chemin. Si cette propriété n'est pas définie, le chargement en masse XML stocke les fichiers temporaires à l'emplacement spécifié dans la variable d'environnement TEMP.  
  
 Transaction  
 Spécifie si le chargement en masse doit avoir lieu sous la forme d'une transaction, auquel cas la restauration est garantie si le chargement en masse échoue. Il s'agit d'une propriété booléenne. Si la propriété est définie sur TRUE, le chargement en masse intervient dans un contexte transactionnel. La propriété TempFilePath est utile uniquement lorsque la Transaction est définie sur TRUE.  
  
> [!NOTE]  
>  Si vous chargez des données binaires (par exemple, l’image bin.hex et bin.base64 aux types de données XML du fichier binaire, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] des types de données), la propriété de Transaction doit être définie sur FALSE.  
  
 La valeur par défaut est FALSE.  
  
 XMLFragment  
 Spécifie si les données source sont un fragment XML. Un fragment XML est un document XML sans élément unique de niveau supérieur (racine). Il s'agit d'une propriété booléenne. Cette propriété doit être définie sur TRUE si le fichier source consiste en un fragment XML.  
  
 La valeur par défaut est FALSE.  
  
  
