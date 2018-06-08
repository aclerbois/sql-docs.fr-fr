---
title: Options (Explorateur d’objets SQL Server - Page Script) | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-objects
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.ObjectExplorerScripting
- VS.ToolsOptionsPages.Sql_Server_Object_Explorer.ObjectExplorerScripting
ms.assetid: 6105aec9-1b72-4cb2-bd24-fc35f6d95240
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 037dce27c8c237b28a5575de76d0f9adb36a4054
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="options-sql-server-object-explorer---scripting-page"></a>Options (Explorateur d’objets SQL Server - Page Script)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Utilisez cette page pour définir les options de script qui s’appliquent aux commandes suivantes dans les menus contextuels d’objet de **l’Explorateur d’objets**:  
  
-   Commandes **Edition** pour les vues et tables utilisateur.  
  
-   Commandes **Script <object> en tant que** pour les objets créés par l’utilisateur.  
  
-   Commandes **Modifier** pour les objets créés par l’utilisateur.  
  
-   Cette page définit également les valeurs par défaut des options de script de **l’Assistant Génération de scripts SQL Server**.  
  
## <a name="remarks"></a>Notes   
Les commandes **Edition** et **Modifier** et peuvent produire des résultats qui sont différents de ceux de la commande **Script <object> en tant** que pour le même paramètre d’option. Les commandes **Edition** et **Modifier** sont conçues pour modifier des objets de la base de données active durant une session de l’Éditeur de requête. La commande **Script <object> en tant que** est conçue pour générer un script pour qu’il puisse être utilisé ultérieurement pour créer des objets.  
  
## <a name="options"></a>Options  
Spécifiez les options de script en les sélectionnant parmi les paramètres disponibles dans la liste située à droite de chaque option.  
  
### <a name="general-scripting-options"></a>Options de script générales  
**Délimiter des instructions individuelles**  
Sépare les instructions [!INCLUDE[tsql](../../includes/tsql_md.md)] par un délimiteur de traitement. Pour modifier le délimiteur de traitement par défaut de **l’Éditeur de requête**, sélectionnez **Outils**/**Options**/**Exécution de la requête**/**SQL Server**/**Général**/**Délimiteur de traitement**. La valeur par défaut est FALSE. Pour plus d’informations, consultez [GO (Transact-SQL)](https://msdn.microsoft.com/b2ca6791-3a07-4209-ba8e-2248a92dd738).  
  
**Inclure des en-têtes descriptifs**  
Ajoute des commentaires descriptifs au script en séparant le script en sections pour chaque objet. La valeur par défaut est True. Pour plus d’informations, consultez [/*...*/ (Commentaire) (Transact-SQL)](https://msdn.microsoft.com/4d9ab1b2-4bbb-4c16-beb1-cafc1af7417c).  
  
**Inclure l’activation de la compression de vardecimal**  
Inclut les options de stockage VarDecimal. La valeur par défaut est FALSE. Pour plus d’informations, consultez [sp_db_vardecimal_storage_format (Transact-SQL)](https://msdn.microsoft.com/9920b2f7-b802-4003-913c-978c17ae4542).  
  
**Générer le script de suivi des modifications**  
Inclut des informations de suivi des modifications dans le script.  
  
**Générer un script pour les catalogues de texte intégral**  
Inclut un script pour les catalogues de texte intégral. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE FULLTEXT CATALOG (Transact-SQL)](https://msdn.microsoft.com/d7a8bd93-e2d7-4a40-82ef-39069e65523b).  
  
**Générer un script USE <database>**  
Ajoute l’instruction USE DATABASE au script pour créer des objets de base de données dans le contexte de la base de données de **l’Explorateur d’objets** active. Lorsqu'il est prévu que le script sera utilisé dans une base de données différente, sélectionnez False afin d'omettre l'instruction. La valeur par défaut est True. Pour plus d’informations, consultez [USE (Transact-SQL)](https://msdn.microsoft.com/c05acac8-c063-4770-8e36-d7f71d500b10).  
  
### <a name="object-scripting-options"></a>Options de scripts d’objets  

**Vérifier l’existence de l’objet** Vérifiez qu’un objet avec le nom donné existe avant de le supprimer ou le modifier, ou qu’un objet avec le nom donné n’existe pas avant de le créer. Pour plus d’informations, consultez [IF... ELSE (Transact-SQL)](https://msdn.microsoft.com/676c881f-dee1-417a-bc51-55da62398e81) et [EXISTS (Transact-SQL)](https://msdn.microsoft.com/b6510a65-ac38-4296-a3d5-640db0c27631).

**Générer un script pour les objets dépendants**  
Génère un script pour les objets supplémentaires qui sont requis lorsque le script de l'objet sélectionné est exécuté. La valeur par défaut est FALSE.  
  
**Noms d'objet de qualification de schéma**  
Qualifie les noms d'objets avec le schéma de l'objet. La valeur par défaut est FALSE. Pour plus d’informations, consultez [Créer un schéma de base de données](https://msdn.microsoft.com/ed2a5522-f4d2-4111-95a4-d3e1e5081739).  

**Options de compression des données de script** Inclut les options de compression de données dans le script. La valeur par défaut est FALSE.

**Générer un script pour les propriétés étendues**  
Inclut les propriétés étendues dans le script, si l'objet en possède. La valeur par défaut est FALSE. Pour plus d’informations, consultez [sp_addextendedproperty (Transact-SQL)](https://msdn.microsoft.com/565483ea-875b-4133-b327-d0006d2d7b4c).  
  
**Propriétaire de script**  
Inclut le propriétaire dans le script généré. La valeur par défaut est FALSE.  
  
**Générer un script pour les autorisations**  
Inclut les autorisations sur les objets de base de données dans le script. La valeur par défaut est True. Pour plus d’informations, consultez [Autorisations](https://msdn.microsoft.com/f28e3dea-24e6-4a81-877b-02ec4c7e36b9).  
  
### <a name="tableview-options"></a>Options de table/vue  
Les options suivantes s'appliquent uniquement aux scripts des tables et des vues.  
  
**Convertir les types de données définis par l'utilisateur en types de base**  
Convertit les types de données définis par l'utilisateur en types de base à partir desquels ils ont été créés. Utilisez True lorsque les types de données définis par l'utilisateur de la base de données source n'existent pas dans la base de données où le script sera exécuté. Utilisez False pour conserver les types de données définis par l'utilisateur. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE TYPE (Transact-SQL)](https://msdn.microsoft.com/2202236b-e09f-40a1-bbc7-b8cff7488905).  
  
**Générer des commandes SET ANSI PADDING**  
Ajoute l'instruction SET ANSI_PADDING avant et après chaque instruction CREATE TABLE. La valeur par défaut est True. Pour plus d’informations, consultez [SET ANSI_PADDING (Transact-SQL)](https://msdn.microsoft.com/92bd29a3-9beb-410e-b7e0-7bc1dc1ae6d0).  
  
**Inclure un classement**  
Inclut un classement dans la définition de colonne. La valeur par défaut est True. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](https://msdn.microsoft.com/92d34f48-fa2b-47c5-89d3-a4c39b0f39eb).  
  
**Inclure la propriété IDENTITY**  
Inclut des définitions pour la valeur de départ IDENTITY et l'incrément IDENTITY. La valeur par défaut est True. Pour plus d’informations, consultez [IDENTITY (propriété) (Transact-SQL)](https://msdn.microsoft.com/8429134f-c821-4033-a07c-f782a48d501c).  
  
**Références aux clés étrangères de qualification de schéma**  
Ajoute le nom de schéma aux références de table des contraintes FOREIGN KEY. La valeur par défaut est True.  
  
**Valeurs par défaut et règles liées aux scripts**  
Inclut les appels aux procédures stockées liées **sp_bindefault** et **sp_bindrule** . La valeur par défaut est True. Pour plus d’informations, consultez [sp_bindefault (Transact-SQL)](https://msdn.microsoft.com/3da70c10-68d0-4c16-94a5-9e84c4a520f6) et [sp_bindrule (Transact-SQL)](https://msdn.microsoft.com/2606073e-c52f-498d-a923-5026b9d97e67).  
  
**Générer un script pour les contraintes CHECK**  
Ajoute des [contraintes CHECK](https://msdn.microsoft.com/637098af-2567-48f8-90f4-b41df059833e) au script. La valeur par défaut est True.  
  
**Valeurs de script par défaut**  
Inclut les valeurs de colonne par défaut dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE DEFAULT (Transact-SQL)](https://msdn.microsoft.com/08475db4-7d90-486a-814c-01a99d783d41).  
  
**Générer un script pour les groupes de fichiers**  
Spécifie le groupe de fichiers dans la clause ON pour des définitions de table. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE TABLE (Transact-SQL)](https://msdn.microsoft.com/1e068443-b9ea-486a-804f-ce7b6e048e8b).  
  
**Générer un script pour les clés étrangères**  
Inclut des [contraintes FOREIGN KEY](https://msdn.microsoft.com/31fbcc9f-2dc5-4bf9-aa50-ed70ec7b5bcd) dans le script. La valeur par défaut est FALSE.  
  
**Générer un script pour les index de recherche en texte intégral**  
Inclut les index de recherche en texte intégral dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE FULLTEXT INDEX (Transact-SQL)](https://msdn.microsoft.com/8b80390f-5f8b-4e66-9bcc-cabd653c19fd).  
  
**Générer un script pour les index**  
Inclut des index cluster, non cluster et XML dans le script. La valeur par défaut est True. Pour plus d’informations, consultez [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/d2297805-412b-47b5-aeeb-53388349a5b9).  
  
**Générer un script pour les schémas de partition**  
Inclut des schémas de partition de table dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE PARTITION SCHEME (Transact-SQL)](https://msdn.microsoft.com/5b21c53a-b4f4-4988-89a2-801f512126e4).  
  
**Générer un script pour les clés primaires**  
Inclut des [contraintes de clé primaire et de clé étrangère](https://msdn.microsoft.com/31fbcc9f-2dc5-4bf9-aa50-ed70ec7b5bcd) dans le script. La valeur par défaut est True.  
  
**Générer un script pour les statistiques**  
Inclut des statistiques définies par l'utilisateur dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE STATISTICS (Transact-SQL)](https://msdn.microsoft.com/b23e2f6b-076c-4e6d-9281-764bdb616ad2).  
  
**Générer un script pour les déclencheurs**  
Inclut des déclencheurs dans le script. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE TRIGGER (Transact-SQL)](https://msdn.microsoft.com/edeced03-decd-44c3-8c74-2c02f801d3e7).  
  
**Générer un script pour les clés uniques**  
Inclut des [contraintes uniques et des contraintes de validation](https://msdn.microsoft.com/637098af-2567-48f8-90f4-b41df059833e) dans le script. La valeur par défaut est FALSE.  
  
**Générer un script pour les colonnes de vue**  
Déclare des colonnes de vue dans les en-têtes de vue. La valeur par défaut est FALSE. Pour plus d’informations, consultez [CREATE VIEW (Transact-SQL)](https://msdn.microsoft.com/aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9).  
  
**Inclure les noms de système DRI**  
Inclut les noms de contraintes générés par le système pour appliquer l'intégrité référentielle déclarative. La valeur par défaut est FALSE. Pour plus d’informations, consultez [REFERENTIAL_CONSTRAINTS (Transact-SQL)](https://msdn.microsoft.com/5d358f18-0a85-4b55-af4b-98d5f4cd1020).  
  
### <a name="version-options"></a>Options de version

**Faire correspondre les paramètres de script à la source** Si cette option est activée, l’édition et le type de moteur des scripts générés sont définies sur les valeurs du serveur pour lequel l’objet fait l’objet d’un script. Cette option désactive (et ignore) les autres options de version. 

**Script pour l’édition du moteur de base de données** Les scripts générés sont ciblés sur l’[édition de moteur](https://msdn.microsoft.com/library/microsoft.sqlserver.management.smo.edition.aspx) spécifiée.

**Script pour le type du moteur de base de données** Les scripts générés sont ciblés sur le [type de moteur de base de données](https://msdn.microsoft.com/library/microsoft.sqlserver.management.common.databaseenginetype.aspx) spécifié.

**Générer un script pour la version du serveur**  
Les scripts générés sont ciblés sur la version spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Les fonctionnalités qui sont des nouveautés de [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)] ne peuvent pas faire l'objet d'un script pour les versions antérieures. Certains scripts qui sont créés pour [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)] ne peuvent pas être exécutés sur les serveurs exécutant une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]ou sur une base de données qui possède un [paramètre de niveau de compatibilité de base de données](https://msdn.microsoft.com/ca5fd220-d5ea-4182-8950-55d4101a86f6)antérieur.  

## <a name="see-also"></a>Voir aussi  
[Générer des scripts (SQL Server Management Studio)](https://msdn.microsoft.com/9711c617-3c68-4e5a-aea3-befc64d51524)  
  
