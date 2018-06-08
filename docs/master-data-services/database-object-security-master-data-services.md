---
title: Sécurité de l’objet de base de données (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- database [Master Data Services], object security
- security [Master Data Services], database objects
ms.assetid: dd5ba503-7607-45d9-ad0d-909faaade179
caps.latest.revision: 10
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: b9a6761703f01a1b4a0a3b85580770f57a115b61
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-object-security-master-data-services"></a>Sécurité de l'objet de base de données (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , les données sont stockées dans plusieurs tables de base de données et sont visible dans les vues. Les informations que vous avez sécurisées dans l'application Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] sont visibles aux utilisateurs qui ont accès à la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
 Par exemple, les informations de l'employé concernant les salaires peuvent être contenues dans un modèle Employé, et les informations financières de la société peuvent être contenues dans un modèle Compte. Vous pouvez refuser à un utilisateur l’accès à ces modèles dans l’interface utilisateur [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , mais les utilisateurs qui ont accès à la base de données pourront consulter ces données.  
  
 Vous pouvez accorder des autorisations d'accès aux objets de base de données pour rendre disponibles aux utilisateurs des données spécifiques. Pour plus d’informations sur l’octroi d’autorisations, consultez [Octroi d’autorisations d’objet &#40;Transact-SQL&#41;](../t-sql/statements/grant-object-permissions-transact-sql.md). Pour plus d’informations sur la sécurisation du serveur SQL, consultez [Sécurisation de SQL Server](../relational-databases/security/securing-sql-server.md).  
  
 Les tâches suivantes requièrent l’accès à la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] :  
  
-   [Mise en lots de données](#Staging)  
  
-   [Validation de données par rapport aux règles d’entreprise](#rules)  
  
-   [Suppression de versions](#Versions)  
  
-   [Application immédiate des autorisations des membres de la hiérarchie](#Hierarchy)  
  
-   [Configuration des paramètres système](#SysSettings)  
  
##  <a name="Staging"></a> Mise en lots de données  
 Dans le tableau suivant, le nom de chaque élément sécurisable comporte le terme « name ». Il indique le nom de la table de mise en lots spécifié lors de la création d'une entité. Pour plus d’informations, consultez [Présentation : Importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
|Action|Éléments sécurisables|Autorisations|  
|------------|----------------|-----------------|  
|Créer, mettre à jour et supprimer des membres feuille et leurs attributs.|stg.name_Leaf|Obligatoire : INSERT<br /><br /> Facultatif : SELECT et UPDATE|  
|Chargez les données de la table de mise en lots Feuille dans les tables de base de données MDS appropriées.|stg.udp_name_Leaf|Exécutez|  
|Créer, mettre à jour et supprimer des membres consolidés et leurs attributs.|stg.name_Consolidated|Obligatoire : INSERT<br /><br /> Facultatif : SELECT et UPDATE|  
|Chargez les données de la table de mise en lots Consolidé dans les tables de base de données MDS appropriées.|stg.udp_name_Consolidated|Exécutez|  
|Déplacer des membres dans une hiérarchie explicite.|stg.name_Relationship|Obligatoire : INSERT<br /><br /> Facultatif : SELECT et UPDATE|  
|Chargez les données de la table de mise en lots Relation dans les tables MDS appropriées.|stg.udp_name_Relationship|Exécutez|  
|Affichez les erreurs qui se sont produites lors de l'insertion des données des tables de mise en lots dans les tables de base de données MDS.|stg.udp_name_Relationship|SELECT|  
  
 Pour plus d’informations, consultez [Présentation : Importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
##  <a name="rules"></a> Validation de données par rapport aux règles d’entreprise  
  
|Action|Élément sécurisable|Autorisations|  
|------------|---------------|-----------------|  
|Valider une version des données par rapport aux règles d'entreprise|mdm.udpValidateModel|Exécutez|  
  
 Pour plus d’informations, consultez [Procédure stockée de validation &#40;Master Data Services&#41;](../master-data-services/validation-stored-procedure-master-data-services.md).  
  
##  <a name="Versions"></a> Suppression de versions  
  
|Action|Éléments sécurisables|Autorisations|  
|------------|----------------|-----------------|  
|Déterminer l'ID de la version que vous souhaitez supprimer|mdm.viw_SYSTEM_SCHEMA_VERSION|SELECT|  
|Supprimer une version d'un modèle|mdm.udpVersionDelete|Exécutez|  
  
 Pour plus d’informations, consultez [Supprimer une version &#40;Master Data Services&#41;](../master-data-services/delete-a-version-master-data-services.md).  
  
##  <a name="Hierarchy"></a> Application immédiate des autorisations des membres de la hiérarchie  
  
|Action|Éléments sécurisables|Autorisations|  
|------------|----------------|-----------------|  
|Application immédiate d'autorisations de membre|mdm.udpSecurityMemberProcessRebuildModel|Exécutez|  
  
 Pour plus d’informations, consultez [Appliquer immédiatement des autorisations de membre &#40;Master Data Services&#41;](../master-data-services/immediately-apply-member-permissions-master-data-services.md).  
  
##  <a name="SysSettings"></a> Configuration des paramètres système  
 Vous pouvez configurer certains paramètres système pour contrôler le comportement dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. Vous pouvez configurer ces paramètres dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ou bien, si vous disposez d’un accès UPDATE, vous pouvez les configurer directement dans la table de base de données mdm.tblSystemSetting. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Sécurité &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md)  
  
  
