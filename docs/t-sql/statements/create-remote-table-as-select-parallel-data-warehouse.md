---
title: CREATE REMOTE TABLE AS SELECT (Parallel Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: ''
ms.prod_service: pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.component: t-sql|statements
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 16ef8191-7587-45a3-9ee9-7d99b7088de3
caps.latest.revision: 9
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 619fe7055ef67a7be88dfa6fbf08ceeb8dae66ee
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="create-remote-table-as-select-parallel-data-warehouse"></a>CREATE REMOTE TABLE AS SELECT (Parallel Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]

  Sélectionne des données à partir d’une base de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] et copie ces données vers une nouvelle table dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP sur un serveur distant. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] utilise l’appliance, avec tous les avantages offerts par le traitement des requêtes MPP, pour sélectionner les données pour la copie distante. Ceci convient aux scénarios qui nécessitent une fonctionnalité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour configurer le serveur distant, consultez « Copie de table distante » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE REMOTE TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name     AT ('<connection_string>')  
    [ WITH ( BATCH_SIZE = batch_size ) ]  
    AS <select_statement>  
[;]  
  
<connection_string> ::=   
    Data Source = { IP_address | hostname } [, port ]; User ID = user_name ;Password = password;  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Base de données dans laquelle créer la table distante. *database_name* est une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur par défaut est la base de données par défaut pour la connexion utilisateur sur l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destination.  
  
 *schema_name*  
 Schéma de la nouvelle table. La valeur par défaut est le schéma par défaut pour la connexion utilisateur sur l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destination.  
  
 *table_name*  
 Nom de la nouvelle table. Pour plus d’informations sur les noms de tables autorisés, consultez les règles de nommage d’objets dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 La table distante est créée en tant que segment. Elle n’a pas de contraintes de validation ou de déclencheurs. Le classement des colonnes de la table distante est identique à celui des colonnes de la table source. Cela s’applique aux colonnes de type **char**, **nchar**, **varchar** et **nvarchar**.  
  
 *connection_string*  
 Chaîne de caractères qui spécifie les paramètres `Data Source`, `User ID` et `Password` pour la connexion au serveur distant et à la base de données.  
  
 La chaîne de connexion est une liste délimitée par des points-virgules de paires clé / valeur. Les mots clés ne respectent pas la casse. Les espaces entre les paires clé / valeur sont ignorés. Toutefois, les valeurs peuvent respecter la casse, en fonction de la source de données.  
  
 *Source de données*  
 Paramètre qui spécifie le nom ou l’adresse IP, et le numéro de port TCP pour l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP distant.  
  
 *hostname* ou *IP_address*  
 Nom de l’ordinateur serveur distant ou adresse IPv4 du serveur distant. Les adresses IPv6 ne sont pas prises en charge. Vous pouvez spécifier une instance nommée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au format **nom_ordinateur\nom_instance** ou **adresse_IP\nom_instance**. Le serveur doit être distant et ne peut donc pas être spécifié comme (local).  
  
 Numéro de *port* TCP  
 Numéro de port TCP pour la connexion. Vous pouvez spécifier un numéro de port TCP compris entre 0 et 65 535 pour une instance de SQL Server qui n’est pas à l’écoute sur le port par défaut 1433. Par exemple : **ServerA, 1450** ou **10.192.14.27,1435**  
  
> [!NOTE]  
>  Nous vous recommandons de vous connecter à un serveur distant à l’aide de l’adresse IP. En fonction de votre configuration réseau, la connexion à l’aide du nom de l’ordinateur peut nécessiter des étapes supplémentaires afin d’utiliser votre serveur DNS non-appliance pour résoudre le nom au serveur approprié. Cette étape n’est pas nécessaire lors de la connexion avec une adresse IP. Pour plus d’informations, consultez « Utiliser un redirecteur DNS pour résoudre les noms DNS non-appliance (Analytics Platform System) » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 *user_name*  
 Nom de connexion d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide. Le nombre maximal de caractères autorisé est 128.  
  
 *password*  
 Mot de passe de connexion. Le nombre maximal de caractères autorisé est 128.  
  
 *batch_size*  
 Nombre maximal de lignes par lot. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] envoie les lignes par lots au serveur de destination. *Batch_size* est un entier positif >= 0. La valeur par défaut est 0.  
  
 WITH *common_table_expression*  
 Spécifie un jeu de résultats nommé temporaire, désigné par le terme d'expression de table commune (CTE, Common Table Expression). Pour plus d’informations, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 SELECT \<select_criteria> Prédicat de requête qui spécifie les données qui rempliront la table distante. Pour plus d’informations sur l’instruction SELECT, consultez [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite :  
  
-   L’autorisation SELECT sur chaque objet dans la clause SELECT.  
  
-   L’autorisation CREATE TABLE sur la base de données SMP de destination.  
  
-   Les autorisations ALTER, INSERT et SELECT sur le schéma SMP de destination.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Si la copie des données dans la base de données distante échoue, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] abandonne l’opération, enregistre une erreur dans le journal et essaie de supprimer la table distante. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne garantit pas un nettoyage réussi de la nouvelle table.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 **Serveur de destination distant** :  
  
-   TCP est le protocole par défaut et le seul protocole pris en charge pour la connexion à un serveur distant.  
  
-   Le serveur de destination doit être un serveur non-appliance. Vous ne pouvez pas utiliser CREATE REMOTE TABLE pour copier des données d’une appliance vers une autre.  
  
-   L’instruction CREATE REMOTE TABLE crée seulement de nouvelles tables. Par conséquent, la nouvelle table ne peut pas déjà exister. La base de données et le schéma distants doivent déjà exister.  
  
-   Le serveur distant doit disposer d’espace pour stocker les données qui sont transférées de l’appliance vers la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distante.  
  
 **Instruction SELECT** :  
  
-   Les clauses ORDER BY et TOP ne sont pas prises en charge dans les critères de sélection.  
  
-   Vous ne pouvez pas exécuter CREATE REMOTE TABLE à l’intérieur d’une transaction active, ni quand le paramètre AUTOCOMMIT OFF est actif pour la session.  
  
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md) ne produit aucun effet sur cette instruction. Pour obtenir un comportement similaire, utilisez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
## <a name="locking-behavior"></a>Comportement de verrouillage  
 Une fois la table distante créée, la table de destination n’est verrouillée que quand la copie commence. Ainsi, il est possible qu’un autre processus supprime la table distante après sa création et avant le démarrage de la copie. Quand cela se produit, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] génère une erreur et la copie échoue.  
  
## <a name="metadata"></a>Métadonnées  
 Utilisez [sys.dm_pdw_dms_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql.md) pour afficher la progression de la copie des données sélectionnées vers le serveur SMP distant. Les lignes avec le type PARALLEL_COPY_READER contiennent ces informations.  
  
## <a name="security"></a>Sécurité  
 L’instruction CREATE REMOTE TABLE utilise l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour se connecter à l’instance distante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elle n’utilise pas l’authentification Windows.  
  
 Le réseau externe [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] doit être protégé par un pare-feu, sauf les ports [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les ports d’administration et les ports de gestion.  
  
 Pour aider à prévenir toute altération ou perte accidentelle de données, le compte d’utilisateur servant à effectuer la copie de l’appliance vers la base de données de destination doit avoir uniquement les autorisations minimales requises sur la base de données de destination.  
  
 Les paramètres de connexion vous permettent de vous connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP avec SSL, protégeant ainsi les données de nom d’utilisateur et mot de passe, mais avec les données proprement dites envoyées en texte clair. Quand cela se produit, un utilisateur malveillant pourrait intercepter le texte de l’instruction CREATE REMOTE TABLE, qui contient le nom d’utilisateur et le mot de passe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP. Pour éviter ce risque, utilisez le chiffrement des données sur la connexion à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP.  
  
##  <a name="Examples"></a> Exemples  
  
### <a name="a-creating-a-remote-table"></a>A. Création d’une table distante  
 Cet exemple crée une table distante SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommée `MyOrdersTable` sur la base de données `OrderReporting` et le schéma `Orders`. La base de données `OrderReporting` se trouve sur un serveur nommé `SQLA` à l’écoute sur le port par défaut 1433. La connexion au serveur utilise les informations d’identification de l’utilisateur `David`, dont le mot de passe est `e4n8@3`.  
  
```  
CREATE REMOTE TABLE OrderReporting.Orders.MyOrdersTable  
AT ( 'Data Source = SQLA, 1433; User ID = David; Password = e4n8@3;' )  
AS SELECT <select_criteria>;  
```  
  
### <a name="b-querying-the-sysdmpdwdmsworkers-dmv-for-remote-table-copy-status"></a>B. Interrogation de la vue de gestion dynamique sys.dm_pdw_dms_workers pour l’état de copie de table distante  
 Cette requête montre comment afficher l’état de la copie d’une table distante.  
  
```  
SELECT * FROM sys.dm_pdw_dms_workers   
WHERE type = 'PARALLEL_COPY_READER';  
```  
  
### <a name="c-using-a-query-join-hint-with-create-remote-table"></a>C. Utilisation d’un indicateur de jointure de requête avec CREATE REMOTE TABLE  
 Cette requête présente la syntaxe de base pour utiliser un indicateur de jointure de requête avec CREATE REMOTE TABLE. Une fois la requête soumise au nœud de contrôle, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en cours d’exécution sur les nœuds de calcul, applique la stratégie de jointure hachée lors de la génération du plan de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur les indicateurs de jointure et sur l’utilisation de la clause OPTION, consultez [Clause OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).  
  
```  
USE ssawPDW;  
CREATE REMOTE TABLE OrderReporting.Orders.MyOrdersTable  
AT ( 'Data Source = SQLA, 1433; User ID = David; Password = e4n8@3;' )  
    AS SELECT T1.* FROM OrderReporting.Orders.MyOrdersTable T1   
    JOIN OrderReporting.Order.Customer T2  
    ON T1.CustomerID=T2.CustomerID OPTION (HASH JOIN);  
```  
  
  

