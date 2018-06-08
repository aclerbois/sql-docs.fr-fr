---
title: Sécurité de la table temporelle | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 60e5d6f6-a26d-4bba-aada-42e382bbcd38
caps.latest.revision: 9
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8ee27537dd58803a79291ea75a47e51d1cd53936
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="temporal-table-security"></a>Sécurité de la table temporelle
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Pour comprendre la sécurité appliquée aux tables temporelles, il est important de comprendre les principes de sécurité qui s’appliquent aux tables temporelles. Une fois que vous avez compris ces principes de sécurité, vous serez prêt à approfondir la sécurité concernant les instructions **CREATE TABLE**, **ALTER TABLE**et **SELECT** .  
  
## <a name="security-principles"></a>Principes de sécurité  
 Le tableau suivant décrit les principes de sécurité qui s’appliquent aux tables temporelles :  
  
|Principe|Description|  
|---------------|-----------------|  
|L’activation/la désactivation du contrôle de version système requiert des autorisations maximales sur les objets affectés|L’activation et la désactivation de SYSTEM_VERSIONING nécessitent l’autorisation CONTROL sur la table en cours et la table d’historique|  
|Les données d’historique ne peuvent pas être modifiées directement|Lorsque SYSTEM_VERSIONING est défini sur ON, les utilisateurs ne peuvent pas modifier les données d’historique, quelles que soient leurs autorisations sur la table en cours ou la table d’historique. Cela inclut les modifications de schéma et des données.|  
|L’interrogation des données d’historique nécessite l’autorisation **SELECT** sur la table d’historique|Même si un utilisateur dispose de l’autorisation **SELECT** sur la table en cours, cela ne signifie pas qu’il dispose de l’autorisation **SELECT** sur la table d’historique.|  
|Opérations de surfaces d’audit qui affectent la table d’historique d’une manière spécifique :|L’audit sur la table d’historique capture régulièrement toutes les tentatives directes d’accès aux données (même si elles ont échoué).<br /><br /> **SELECT** avec une extension de requête temporelle indique que la table d’historique a été affectée par cette opération.<br /><br /> La table temporelle**CREATE/ALTER** expose les informations relatives à la vérification d’autorisation sur la table d’historique également. Le fichier d’audit contient un enregistrement supplémentaire pour la table d’historique.<br /><br /> Les opérations DML sur la table en cours indiquent que la table d’historique a été affectée, mais additional_info fournit le contexte nécessaire (DML était le résultat de system_versioning).|  
  
## <a name="performing-schema-operations"></a>Exécution d’opérations de schéma  
 Lorsque SYSTEM_VERSIONING est défini sur ON, les opérations de modification de schéma sont limitées.  
  
### <a name="disallowed-alter-schema-operations"></a>Opérations de schéma ALTER interdites  
  
|Opération|Table en cours|Table d’historique|  
|---------------|-------------------|-------------------|  
|**DROP TABLE**|Interdit|Interdit|  
|**ALTER TABLE…SWITCH PARTITION**|SWITCH IN uniquement (consultez [Partitionnement des tables temporelles](../../relational-databases/tables/partitioning-with-temporal-tables.md))|SWITCH OUT uniquement (consultez [Partitionnement des tables temporelles](../../relational-databases/tables/partitioning-with-temporal-tables.md))|  
|**ALTER TABLE…DROP PERIOD**|Interdit|-|  
|**ALTER TABLE…ADD PERIOD**|-|Interdit|  
  
## <a name="allowed-alter-table-operations"></a>Opérations ALTER TABLE autorisées  
  
|Opération|Current|Historique|  
|---------------|-------------|-------------|  
|**ALTER TABLE…REBUILD**|Autorisé (indépendamment)|Autorisé (indépendamment)|  
|**CREATE INDEX**|Autorisé (indépendamment)|Autorisé (indépendamment)|  
|**CREATE STATISTICS**|Autorisé (indépendamment)|Autorisé (indépendamment)|  
  
## <a name="security-of-the-create-temporal-table-statement"></a>Sécurité de l’instruction CREATE Temporal TABLE  
  
||Créer une nouvelle table d’historique|Réutiliser la table d’historique existante|  
|-|------------------------------|----------------------------------|  
|Autorisations requises|Autorisation**CREATE TABLE** dans la base de données<br /><br /> Autorisation**ALTER** sur les schémas dans lesquels les tables en cours et d’historique sont créées|Autorisation**CREATE TABLE** dans la base de données<br /><br /> Autorisation**ALTER** sur le schéma dans lequel la table en cours va être créée<br /><br /> Autorisation**CONTROL** sur la table d’historique spécifiée dans le cadre de l’instruction **CREATE TABLE** qui crée la table temporelle|  
|Audit|L’audit révèle que les utilisateurs ont essayé de créer deux objets. L’opération peut échouer en raison d’autorisations insuffisantes pour créer une table dans la base de données ou en raison d’autorisations insuffisantes pour modifier les schémas des tables.|L’audit indique que la table temporelle a été créée. L’opération peut échouer en raison d’autorisations insuffisantes pour créer une table dans la base de données, en raison d’autorisations insuffisantes pour modifier le schéma de la table temporelle, ou en raison d’autorisations insuffisantes sur la table d’historique.|  
  
## <a name="security-of-the-alter-temporal-table-set-systemversioning-onoff-statement"></a>Sécurité de l’instruction ALTER Temporal TABLE SET (SYSTEM_VERSIONING ON/OFF)  
  
||Créer une nouvelle table d’historique|Réutiliser la table d’historique existante|  
|-|------------------------------|----------------------------------|  
|Autorisations requises|Autorisation**CONTROL** dans la base de données<br /><br /> Autorisation**CREATE TABLE** dans la base de données<br /><br /> Autorisation**ALTER** sur les schémas dans lesquels la table d’historique est créée|Autorisation**CONTROL** sur la table d’origine qui est modifiée<br /><br /> Autorisation**CONTROL** sur la table d’historique spécifiée dans le cadre de l’instruction **ALTER TABLE** |  
|Audit|L’audit indique que la table temporelle a été modifiée et que la table d’historique a été créée en même temps. L’opération peut échouer en raison d’autorisations insuffisantes pour créer une table dans la base de données, en raison d’autorisations insuffisantes pour modifier le schéma de la table d’historique, ou en raison d’autorisations insuffisantes pour modifier la table temporelle.|L’audit indique que la table temporelle a été modifiée, mais que l’opération nécessitait l’accès à la table d’historique. L’opération peut échouer en raison d’autorisations insuffisantes sur la table d’historique ou d’autorisations insuffisantes sur la table en cours.|  
  
## <a name="security-of-select-statement"></a>Sécurité de l’instruction SELECT  
 L’autorisation**SELECT** reste inchangée pour les instructions **SELECT** qui n’affectent pas la table d’historique. Pour les instructions **SELECT** qui affectent la table d’historique, l’autorisation **SELECT** est nécessaire sur la table en cours et la table d’historique.  
  
## <a name="see-also"></a> Voir aussi  
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)   
 [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)   
 [Vérifications de cohérence système des tables temporelles](../../relational-databases/tables/temporal-table-system-consistency-checks.md)   
 [Partitionnement des tables temporelles](../../relational-databases/tables/partitioning-with-temporal-tables.md)   
 [Considérations et limitations liées aux tables temporelles](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)   
 [Gérer la rétention des données d’historique dans les tables temporelles avec version gérée par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)   
 [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)   
 [Vues et fonctions de métadonnées de table temporelle](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)  
  
  
