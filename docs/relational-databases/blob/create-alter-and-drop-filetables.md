---
title: Créer, modifier et supprimer des FileTables | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
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
- FileTables [SQL Server], altering
- FileTables [SQL Server], dropping
- FileTables [SQL Server], creating
ms.assetid: 47d69e37-8778-4630-809b-2261b5c41c2c
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ba8ada2a67db9c0f6ea4882fb6fd04b26caa4f9f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-alter-and-drop-filetables"></a>Créer, modifier et supprimer des FileTables
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Décrit la procédure de création d'un nouveau FileTable, ou de modification ou de suppression d'un FileTable existant.  
  
##  <a name="BasicsCreate"></a> Création d'un FileTable  
 Un FileTable est une table utilisateur spécialisée qui a un schéma prédéfini et fixe. Ce schéma stocke des données FILESTREAM, des informations de répertoire et de fichier, ainsi que des attributs de fichier. Pour plus d'informations sur le schéma de FileTable, consultez [FileTable Schema](../../relational-databases/blob/filetable-schema.md).  
  
 Vous pouvez créer un FileTable à l'aide de Transact-SQL ou de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Puisqu'un FileTable dispose d'un schéma fixe, vous n'avez pas à spécifier une liste de colonnes. La syntaxe simple pour créer un FileTable vous permet de spécifier :  
  
-   Un nom de répertoire. Dans la hiérarchie des dossiers FileTable, ce répertoire de niveau table devient l'enfant du répertoire de base de données spécifié au niveau de la base de données, et le parent des fichiers ou répertoires stocké dans la table.  
  
-   Nom du classement à utiliser pour les noms de fichier dans la colonne **Nom** de FileTable.  
  
-   Noms à utiliser pour les 3 contraintes uniques et de clé primaire qui sont créées automatiquement.  
  
###  <a name="HowToCreate"></a> Procédure : créer un FileTable  
 **Créer un FileTable à l'aide de Transact-SQL**  
 Créez un FileTable en appelant l’instruction [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md) avec l’option **AS FileTable**. Puisqu'un FileTable dispose d'un schéma fixe, vous n'avez pas à spécifier une liste de colonnes. Vous pouvez spécifier les paramètres suivants pour le nouveau FileTable :  
  
1.  **FILETABLE_DIRECTORY**. Spécifie le répertoire qui sert de répertoire racine à tous les fichiers et répertoires stockés dans le FileTable. Ce nom doit être unique parmi tous les noms de répertoire FileTable de la base de données. La comparaison d'unicité n'est pas sensible à la casse, indépendamment des paramètres de classement actuels.  
  
    -   Cette valeur a le type de données **nvarchar(255)** et utilise le classement fixe **Latin1_General_CI_AS_KS_WS**.  
  
    -   Le nom de répertoire que vous fournissez doit se conformer aux configurations requises du système de fichiers concernant les noms de répertoire valides.  
  
    -   Ce nom doit être unique parmi tous les noms de répertoire FileTable de la base de données. La comparaison d'unicité n'est pas sensible à la casse, indépendamment des paramètres de classement actuels.  
  
    -   Si vous ne fournissez pas un nom de répertoire lorsque vous créez le FileTable, le nom du FileTable lui-même est utilisé comme nom de répertoire.  
  
2.  **FILETABLE_COLLATE_FILENAME**. Spécifie le nom du classement à appliquer à la colonne **Name** du FileTable.  
  
    1.  Le classement spécifié **ne doit pas être sensible à la casse** pour des raisons de conformité avec la sémantique de nom de fichier Windows.  
  
    2.  Si vous ne fournissez pas de valeur pour **FILETABLE_COLLATE_FILENAME**ou spécifiez **database_default**, la colonne hérite du classement de la base de données active. Si le classement de la base de données active respecte la casse, une erreur est générée et l’opération **CREATE TABLE** échoue.  
  
3.  Vous pouvez également spécifier les noms à utiliser pour les 3 contraintes uniques et de clé primaire qui sont créées automatiquement. Si vous ne fournissez pas de noms, le système génère des noms comme décrit plus loin dans cette rubrique.  
  
    -   **FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME**  
  
    -   **FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME**  
  
    -   **FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME**  
  
 **Exemples**  
  
 L’exemple suivant crée un FileTable et spécifie les valeurs définies par l’utilisateur pour **FILETABLE_DIRECTORY** et **FILETABLE_COLLATE_FILENAME**.  
  
```sql  
CREATE TABLE DocumentStore AS FileTable  
    WITH (   
          FileTable_Directory = 'DocumentTable',  
          FileTable_Collate_Filename = database_default  
         );  
GO  
```  
  
 L'exemple suivant crée également un nouveau FileTable. Étant donné que les valeurs définies par l’utilisateur ne sont pas spécifiées, la valeur de **FILETABLE_DIRECTORY** devient le nom du FileTable, la valeur de **FILETABLE_COLLATE_FILENAME** devient database_default et les contraintes uniques et de clé primaire reçoivent des noms générés par le système.  
  
```sql  
CREATE TABLE DocumentStore AS FileTable;  
GO  
```  
  
 **Créer un FileTable à l'aide de SQL Server Management Studio**  
 Dans l’Explorateur d’objets, développez les objets sous la base de données sélectionnée, cliquez avec le bouton droit sur le dossier **Tables** , puis sélectionnez **Nouveau FileTable**.  
  
 Cette option ouvre une nouvelle fenêtre de script qui contient un modèle de script Transact-SQL que vous pouvez personnaliser et exécuter pour créer un FileTable. Utilisez l'option **Spécifier les valeurs des paramètres du modèle** dans le menu **Requête** pour personnaliser le script facilement.  
  
###  <a name="ReqCreate"></a> Exigences et restrictions concernant la création d'un FileTable  
  
-   Vous ne pouvez pas modifier une table existante pour la convertir en FileTable.  
  
-   Le répertoire parent précédemment spécifié au niveau de la base de données doit avoir une valeur non Null. Pour plus d’informations sur la spécification du répertoire au niveau de la base de données, consultez [Activer les conditions préalables pour les FileTables](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md).  
  
-   Puisqu'un FileTable contient une colonne FILESTREAM, un FileTable requiert un groupe de fichiers FILESTREAM valide. Vous pouvez éventuellement spécifier un groupe de fichiers FILESTREAM valide dans le cadre de la commande **CREATE TABLE** pour la création d'un FileTable. Si vous ne spécifiez pas de groupe de fichiers, le FileTable utilise le groupe de fichiers FILESTREAM par défaut pour la base de données. Si la base de données n'a pas de groupe de fichiers FILESTREAM, une erreur est générée.  
  
-   Vous ne pouvez pas créer de contrainte de table dans le cadre d'une instruction **CREATE TABLE…AS FILETABLE** . Toutefois, vous pouvez ajouter la contrainte ultérieurement à l'aide d'une instruction **ALTER TABLE** .  
  
-   Vous ne pouvez pas créer de FileTable dans la base de données **tempdb** ni dans aucune des autres bases de données système.  
  
-   Vous ne pouvez pas créer de FileTable comme table temporaire.  
  
##  <a name="BasicsAlter"></a> Modification d'un FileTable  
 Puisqu'un FileTable dispose d'un schéma prédéfini et fixe, vous ne pouvez pas ajouter ni modifier ses colonnes. Toutefois, vous pouvez ajouter des index personnalisés, des déclencheurs, des contraintes et d'autres options à un FileTable.  
  
 Pour plus d’informations sur l’utilisation de l’instruction ALTER TABLE pour activer ou désactiver l’espace de noms FileTable, dont les contraintes définies par le système, consultez [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md).  
  
###  <a name="HowToChange"></a> Procédure : modifier le répertoire d'un FileTable  
 **Modifier le répertoire d'un FileTable à l'aide de Transact-SQL**  
 Appelez l’instruction ALTER TABLE et fournissez une nouvelle valeur valide pour l’option SET **FILETABLE_DIRECTORY** .  
  
 **Exemple**  
  
```sql  
ALTER TABLE filetable_name  
    SET ( FILETABLE_DIRECTORY = N'directory_name' );  
GO  
```  
  
 **Lodifier le répertoire d'un FileTables à l'aide de SQL Server Management Studio**  
 Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le FileTable et sélectionnez **Propriétés** pour ouvrir la boîte de dialogue **Propriétés d’une table** . Dans la page **FileTable** , entrez la valeur pour **Nom du répertoire FileTable**.  
  
###  <a name="ReqAlter"></a> Exigences et restrictions concernant la modification d'un FileTable  
  
-   Vous ne pouvez pas modifier la valeur de **FILETABLE_COLLATE_FILENAME**.  
  
-   Vous ne pouvez pas changer, supprimer ni désactiver les colonnes définies par le système d'un FileTable.  
  
-   Vous ne pouvez pas ajouter de nouvelles colonnes d'utilisateur, de colonnes calculées ou de colonnes calculées persistantes à un FileTable.  
  
##  <a name="BasicsDrop"></a> Suppression d'un FileTable  
 Vous pouvez supprimer un FileTable en utilisant la syntaxe ordinaire de l’instruction [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md).  
  
 Lorsque vous supprimez un FileTable, les objets suivants sont également supprimés :  
  
-   Toutes les colonnes du FileTable et tous les objets associés à la table, tels que les index, les contraintes et les déclencheurs, sont également supprimés.  
  
-   Le répertoire FileTable et les sous-répertoires qu'il contenait disparaissent de la hiérarchie de répertoires et de fichiers FILESTREAM de la base de données.  
  
 La commande DROP TABLE échoue si des descripteurs de fichiers sont ouverts dans l'espace de noms du fichier du FileTable. Pour plus d’informations sur la fermeture de descripteurs ouverts, consultez [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md).  
  
##  <a name="BasicsOtherObjects"></a> D'autres objets de base de données sont créés lorsque vous créez un FileTable  
 Lorsque vous créez un nouveau FileTable, quelques contraintes et index définis par le système sont également créés. Vous ne pouvez pas modifier ou supprimer ces objets ; ils disparaissent uniquement lorsque le FileTable lui-même est supprimé. Pour consulter la liste de ces objets, interrogez l’affichage catalogue [sys.filetable_system_defined_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filetable-system-defined-objects-transact-sql.md).  
  
```sql  
--View all objects for all filetables, unsorted  
SELECT * FROM sys.filetable_system_defined_objects;  
GO  
  
--View sorted list with friendly names  
SELECT OBJECT_NAME(parent_object_id) AS 'FileTable', OBJECT_NAME(object_id) AS 'System-defined Object'  
    FROM sys.filetable_system_defined_objects  
    ORDER BY FileTable, 'System-defined Object';  
GO  
```  
  
 **Index créés lorsque vous créez un FileTable**  
 Lorsque vous créez un FileTable, les index définis par le système suivants sont également créés :  
  
|||  
|-|-|  
|**Columns**|**Type d'index**|  
|[path_locator] ASC|Clé primaire, non cluster|  
|[parent_path_locator] ASC,<br /><br /> [name] ASC|Unique, non cluster|  
|[stream_id] ASC|Unique, non cluster|  
  
 **Contraintes créées lorsque vous créez un FileTable**  
 Lorsque vous créez un FileTable, les contraintes définies par le système suivantes sont également créées :  
  
|Contraintes|Applique|  
|-----------------|--------------|  
|Contraintes par défaut sur les colonnes suivantes :<br /><br /> creation_time<br /><br /> is_archive<br /><br /> is_directory<br /><br /> is_hidden<br /><br /> is_offline<br /><br /> is_readonly<br /><br /> is_system<br /><br /> is_temporary<br /><br /> last_access_time<br /><br /> last_write_time<br /><br /> path_locator<br /><br /> stream_id|Les contraintes par défaut définies par le système appliquent les valeurs par défaut pour les colonnes spécifiées.|  
|Contraintes de validation|Les contraintes de validation définies par le système appliquent les spécifications requises suivantes :<br /><br /> Noms de fichier valides.<br /><br /> Attributs de fichier valides.<br /><br /> L'objet parent doit être un répertoire.<br /><br /> La hiérarchie d'espace de noms est verrouillée pendant la manipulation de fichier.|  
  
 **Convention d'affectation des noms pour les contraintes définies par le système**  
 Les contraintes définies par le système décrites ci-dessus sont nommées selon le format **\<type_contrainte>_\<nom_table>[\_\<nom_colonne>]\_\<générateur_de_nom_unique>** où :  
  
-   *<type_contrainte>* est CK (contrainte de validation), DF (contrainte par défaut), FK (clé étrangère), PK (clé primaire) ou UQ (contrainte unique).  
  
-   *\<générateur_de_nom_unique>* est une chaîne générée par le système pour rendre le nom unique. Cette chaîne peut contenir le nom du FileTable et un identificateur unique.  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md)  
  
  
