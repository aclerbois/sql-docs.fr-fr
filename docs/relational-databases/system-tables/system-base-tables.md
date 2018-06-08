---
title: Tables de Base système | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- system base tables [SQL Server]
- hobt [SQL Server]
- base tables
ms.assetid: 31f2df90-651f-4699-8067-19f59b60904f
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c15a0e42091cffb8010cae36ad43322d361f91fe
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="system-base-tables"></a>Tables de base système
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Les tables de base système sont des tables sous-jacentes qui stockent les métadonnées pour une base de données spécifique. Le **master** base de données est spécial à cet égard, car il contient des tables supplémentaires qui ne figurent pas dans un des autres bases de données. Ces tables contiennent des métadonnées persistantes dont l'étendue couvre le serveur.  
  
> [!IMPORTANT]  
>  Les tables de base système sont uniquement utilisées dans [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et ne sont pas destinées à un usage général. Elles peuvent faire l'objet de modifications et la compatibilité n'est pas garantie.  
  
## <a name="system-base-table-metadata"></a>Métadonnées des tables de base système  
 Un bénéficiaire qui a l’autorisation CONTROL, ALTER ou VIEW DEFINITION sur une base de données peut afficher les métadonnées de table de base système dans le **sys.objects** affichage catalogue. Le bénéficiaire peut également résoudre les noms et ID d’objets des tables de base système à l’aide des fonctions intégrées comme [nom_objet](../../t-sql/functions/object-name-transact-sql.md) et [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md).  
  
 Pour créer une liaison avec une table de base système, un utilisateur doit se connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de la connexion administrateur dédiée. Une tentative d'exécution d'une requête SELECT à partir d'une table de base système sans connexion via la DAC provoque une erreur.  
  
> [!IMPORTANT]  
>  L'accès aux tables de base système via la DAC est conçu uniquement pour le personnel [!INCLUDE[msCoName](../../includes/msconame-md.md)] et n'est pas un scénario client pris en charge.  
  
## <a name="system-base-tables"></a>Tables de base système  
 Le tableau suivant répertorie et décrit l'ensemble de chaque table de base système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Table de base| Description|  
|----------------|-----------------|  
|**Sys.sysschobjs**|Existe dans toutes les bases de données. Chaque ligne représente un objet de la base de données.|  
|**Sys.sysbinobjs**|Existe dans toutes les bases de données. Contient une ligne pour chaque entité de Service Broker dans la base de données. Les entités de Service Broker incluent les éléments suivants :<br /><br /> type de message<br /><br /> contrat de service<br /><br /> Service<br /><br /> Les noms et les types utilisent un classement binaire fixe.|  
|**Sys.sysclsobjs**|Existe dans toutes les bases de données. Contient une ligne pour chaque entité classifiée qui partage les mêmes propriétés communes qui incluent les éléments suivants :<br /><br /> Assembly<br /><br /> unité de sauvegarde<br /><br /> Catalogue de texte intégral<br /><br /> Fonction de partition<br /><br /> Schéma de partition<br /><br /> groupe de fichiers<br /><br /> clé d'obfuscation|  
|**Sys.sysnsobjs**|Existe dans toutes les bases de données. Contient une ligne pour chaque entité de l'étendue de l'espace de noms. Cette table est utilisée pour le stockage des entités de collection XML.|  
|**Sys.syscolpars**|Existe dans toutes les bases de données. Contient une ligne pour chaque colonne de table, chaque vue ou chaque fonction table. Contient également des lignes pour chaque paramètre d'une procédure ou d'une fonction.|  
|**Sys.systypedsubobjs**|Existe dans toutes les bases de données. Contient une ligne pour chaque sous-entité typée. Seuls les paramètres de la fonction de partition appartiennent à cette catégorie.|  
|**Sys.sysidxstats**|Existe dans toutes les bases de données. Contient une ligne pour chaque index ou statistique pour les tables et les vues indexées<br /><br /> Remarque : Chaque index (segment excepté) est associé à une statistique qui porte le même nom que l’index.|  
|**Sys.sysiscols**|Existe dans toutes les bases de données. Contient une ligne pour chaque colonne d'index et de statistiques persistante.|  
|**Sys.sysscalartypes**|Existe dans toutes les bases de données. Contient une ligne pour chaque type défini par l'utilisateur ou chaque type système.|  
|**Sys.sysdbreg**|Il existe dans le **master** uniquement la base de données. Contient une ligne pour chaque base de données inscrite.|  
|**Sys.sysxsrvs**|Il existe dans le **master** uniquement la base de données. Contient une ligne pour chaque serveur local, lié ou distant.|  
|**Sys.sysrmtlgns**|Cette table de base système existe dans le **master** uniquement la base de données. Contient une ligne pour chaque mappage d'ouverture de session distante. Cela est utilisé pour mapper les connexions entrantes issues d'un serveur correspondant et accédant à une connexion locale.|  
|**Sys.syslnklgns**|Il existe dans le **master** uniquement la base de données. Contient une ligne pour chaque mappage de connexion liée. Les mappages de connexions liées sont utilisés par des appels de procédure distante et par des requêtes distribuées qui émanent d'un serveur local vers un serveur lié correspondant.|  
|**Sys.sysxlgns**|Il existe dans le **master** uniquement la base de données. Contient une ligne pour chaque principal de serveur.|  
|**Sys.sysdbfiles**|Existe dans toutes les bases de données. Si la colonne **dbid** est égal à zéro, la ligne représente un fichier appartenant à cette base de données. Dans le **master** de base de données, la colonne **dbid** peut être différente de zéro. Lorsque c'est le cas, la ligne représente un fichier maître.|  
|**Sys.sysusermsg**|Il existe dans le **master** uniquement la base de données. Chaque ligne représente un message d'erreur défini par l'utilisateur.|  
|**Sys.sysprivs**|Existe dans toutes les bases de données. Contient une ligne pour chaque autorisation de niveau base de données ou serveur.<br /><br /> Remarque : Les autorisations de niveau serveur sont stockées dans le **master** base de données.|  
|**Sys.sysowners**|Existe dans toutes les bases de données. Chaque ligne représente un principal de base de données.|  
|**Sys.sysobjkeycrypts**|Existe dans toutes les bases de données. Contient une ligne pour chaque clé symétrique, chaque chiffrement ou chaque propriété de chiffrement associé à un objet.|  
|**Sys.syscerts**|Existe dans toutes les bases de données. Contient une ligne pour chaque certificat dans une base de données.|  
|**Sys.sysasymkeys**|Existe dans toutes les bases de données. Chaque ligne représente une clé asymétrique.|  
|**Sys.ftinds**|Existe dans toutes les bases de données. Contient une ligne pour chaque index de texte intégral dans la base de données.|  
|**Sys.sysxprops**|Existe dans toutes les bases de données. Contient une ligne pour chaque propriété étendue.|  
|**Sys.sysallocunits**|Existe dans toutes les bases de données. Contient une ligne pour chaque unité d'allocation de stockage.|  
|**Sys.sysrowsets**|Existe dans toutes les bases de données. Contient une ligne pour chaque ensemble de lignes de partition pour un index ou un segment.|  
|**Sys.sysrowsetrefs**|Existe dans toutes les bases de données. Contient une ligne pour chaque référence d'index à un ensemble de lignes.|  
|**Sys.syslogshippers**|Il existe dans le **master** uniquement la base de données. Contient une ligne pour chaque témoin de mise en miroir de bases de données.|  
|**Sys.sysremsvcbinds**|Existe dans toutes les bases de données. Contient une ligne pour chaque liaison de service distant.|  
|**Sys.sysconvgroup**|Existe dans toutes les bases de données. Contient une ligne pour chaque instance de service dans Service Broker.|  
|**Sys.sysxmitqueue**|Existe dans toutes les bases de données. Contient une ligne pour chaque file d'attente de transmission de Service Broker.|  
|**Sys.sysdesend**|Existe dans toutes les bases de données. Contient une ligne pour chaque point de terminaison d'envoi d'une conversation Service Broker.|  
|**Sys.sysdercv**|Existe dans toutes les bases de données. Contient une ligne pour chaque point de terminaison récepteur d'une conversation Service Broker.|  
|**Sys.sysendpts**|Il existe dans le **master** uniquement la base de données. Contient une ligne pour chaque point de terminaison créé dans le serveur.|  
|**Sys.syswebmethods**|Il existe dans le **master** uniquement la base de données. Contient une ligne pour chaque méthode SOAP définie sur un point de terminaison HTTP SOAP créé sur le serveur.|  
|**Sys.sysqnames**|Existe dans toutes les bases de données. Contient une ligne pour chaque espace de noms ou chaque nom qualifié à un jeton d'ID de 4 octets.|  
|**Sys.sysxmlcomponent**|Existe dans toutes les bases de données. Chaque ligne représente un composant de schéma XML.|  
|**Sys.sysxmlfacet**|Existe dans toutes les bases de données. Contient une ligne pour chaque facette (restriction) XML d'une définition de type XML.|  
|**Sys.sysxmlplacement**|Existe dans toutes les bases de données. Contient une ligne pour chaque emplacement XML pour les composants XML.|  
|**Sys.syssingleobjrefs**|Existe dans toutes les bases de données. Contient une ligne pour chaque référence générale de N à 1.|  
|**Sys.sysmultiobjrefs**|Existe dans toutes les bases de données. Contient une ligne pour chaque référence générale de N à N.|  
|**Sys.sysobjvalues**|Existe dans toutes les bases de données. Contient une ligne pour chaque propriété de valeur générale d'une entité.|  
|**Sys.sysguidrefs**|Existe dans toutes les bases de données. Contient une ligne pour chaque référence d'ID classifiée GUID.|  
  
  
