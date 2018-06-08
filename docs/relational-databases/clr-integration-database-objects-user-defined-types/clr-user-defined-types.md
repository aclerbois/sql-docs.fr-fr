---
title: Types CLR définis par l’utilisateur | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- validation [CLR integration]
- types [CLR integration]
- UserDefined serialization format [CLR integration]
- null values [CLR integration]
- serialization
- Native serialization format [CLR integration]
- databases [CLR integration]
- building database objects [CLR integration], user-defined types
- user-defined types [CLR integration]
- common language runtime [SQL Server], user-defined types
- UDTs [CLR integration]
- database objects [CLR integration], user-defined types
- turning on CLR functionality
- customizing UDT expression return types [CLR integration]
- UDTs [CLR integration], about UDTs
- comparing UDT values
- annotations [CLR integration]
- user-defined types [CLR integration], about UDTs
- variables [CLR integration]
- invoking UDT methods
- indexes [CLR integration]
ms.assetid: 27c4889b-c543-47a8-a630-ad06804f92df
caps.latest.revision: 67
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 962f3ea6789f0a4dd9fa5ad6169801b3dd589223
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clr-user-defined-types"></a>Types CLR définis par l'utilisateur
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez créer des objets de base de données programmés à partir d'un assembly créé dans le CLR (Common Language Runtime) .NET Framework. Les objets de base de données pouvant tirer parti du modèle de programmation évolué fourni par le CLR comprennent les déclencheurs, les procédures stockées, les fonctions, les fonctions d'agrégation et les types.  
  
> [!NOTE]  
>  La possibilité d’exécuter du code CLR est définie sur OFF par défaut dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le CLR peut être activé à l’aide de la **sp_configure** procédure stockée système.  
  
 À partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], vous pouvez utiliser des types définis par l’utilisateur (UDT) pour étendre le système de type scalaire du serveur, permettant ainsi le stockage d’objets CLR dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données. Les types UDT peuvent contenir plusieurs éléments et avoir des comportements, ce qui les différencie des types de données alias traditionnels qui se composent d'un seul type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les types UDT étant accessibles au système dans son ensemble, leur utilisation pour des types de données complexes peut nuire aux performances. Les données complexes sont généralement mieux modélisées au moyen de lignes et de tables traditionnelles. Les types UDT dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont bien adaptés aux éléments suivants :  
  
-   Date, heure, devise et types numériques étendus  
  
-   Applications géographiques  
  
-   Données encodées ou chiffrées  
  
 Le processus de développement de types UDT dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprend les étapes suivantes :  
  
1.  **Le code et générer l’assembly qui définit l’UDT.** Les UDT sont définis à l’aide d’un des langages pris en charge par le.NET Framework common language runtime (CLR) de produisent du code vérifiable. notamment Visual C# et Visual Basic .NET. Les données sont exposées en tant que champs et propriétés d'une classe ou d'une structure .NET Framework, et les comportements sont définis par des méthodes de la classe ou de la structure.  
  
2.  **Inscrire l’assembly.** Les types UDT peuvent être déployés par l'intermédiaire de l'interface utilisateur Visual Studio dans un projet de base de données, ou à l'aide de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE ASSEMBLY, qui copie l'assembly contenant la classe ou la structure dans une base de données.  
  
3.  **Créer l’UDT dans SQL Server.** Une fois qu'un assembly est chargé dans une base de données hôte, vous utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE TYPE pour créer un type UDT et exposer les membres de la classe ou de la structure en tant que membres du type UDT. Les types UDT existent uniquement dans le contexte d'une base de données unique et, une fois inscrits, n'ont pas de dépendances vis-à-vis des fichiers externes à partir desquels ils ont été créés.  
  
    > [!NOTE]  
    >  Dans les versions antérieures à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], les types UDT créés à partir d'assemblys .NET Framework n'étaient pas pris en charge. Toutefois, vous pouvez toujours utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données alias à l’aide de **sp_addtype**. La syntaxe CREATE TYPE peut être utilisée pour créer à la fois des types de données définis par l'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] natifs et des types UDT.  
  
4.  **Créer des tables, des variables ou des paramètres à l’aide de l’UDT** commençant par [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], un type défini par l’utilisateur peut être utilisé en tant que définition de colonne d’une table, en tant que variable dans un [!INCLUDE[tsql](../../includes/tsql-md.md)] lot, ou en tant qu’argument d’un [!INCLUDE[tsql](../../includes/tsql-md.md)] de fonction ou stockées procédure.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Création d’un Type défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types.md)  
 Décrit comment créer des types UDT.  
  
 [Inscription de types définis par l’utilisateur dans SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/registering-user-defined-types-in-sql-server.md)  
 Décrit comment inscrire et gérer des types UDT dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Utilisation des Types définis par l’utilisateur dans SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)  
 Décrit comment créer des requêtes à l'aide de types UDT.  
  
 [L’accès à des Types définis par l’utilisateur dans ADO.NET](../../relational-databases/clr-integration-database-objects-user-defined-types/accessing-user-defined-types-in-ado-net.md)  
 Décrit comment utiliser des types UDT à l'aide du fournisseur de données .NET Framework pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans ADO.NET.  
  
  
