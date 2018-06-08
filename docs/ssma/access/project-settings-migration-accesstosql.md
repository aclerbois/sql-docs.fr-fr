---
title: Paramètres (Migration) (AccessToSQL) du projet | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Migration settings
- Project Settings dialog box, Migration
ms.assetid: 4caebc9c-8680-4b99-a8fa-89c43161c95d
caps.latest.revision: 14
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 3fe26ecc9d250982206121d33b35ee5c975e376f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-migration-accesstosql"></a>Paramètres du projet (Migration) (AccessToSQL)
Les paramètres de projet de Migration vous permettent de configurer la façon dont les données sont migrées vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
Le volet de la Migration est disponible dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue.  
  
-   Utilisez le **les paramètres de projet** boîte de dialogue pour définir les options de configuration pour le projet actuel. Pour accéder aux paramètres de la migration, sur le **outils** menu, sélectionnez **les paramètres de projet**, cliquez sur **général** au bas de la gauche, puis cliquez sur **Migration**.  
  
-   Utilisez le **les paramètres de projet par défaut** boîte de dialogue pour définir les options de configuration pour tous les projets. Pour accéder aux paramètres de la migration, sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**, sélectionnez le type de projet dans **Version cible de la Migration** zone de liste déroulante dont vous voulez accéder aux paramètres, cliquez sur **général** au bas de la gauche, puis cliquez sur **Migration**.  
  
## <a name="options"></a>Options  
**Contraintes de validation**  
Spécifie si SSMA doit vérifier les contraintes lors de l’ajout de données aux tables.  
  
-   **Mode par défaut**: False  
  
-   **Mode optimisé**: True  
  
-   **Mode plein**: False  
  
**Exécuter les déclencheurs**  
Spécifie si SSMA doit exécuter les déclencheurs d’insertion lorsqu’il ajoute des données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] tables.  
  
-   **Mode par défaut**: False  
  
-   **Mode optimisé**: True  
  
-   **Mode plein**: False  
  
**Conserver l'identité**  
Spécifie si SSMA conserve les valeurs d’identité accès lorsqu’il ajoute des données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Si cette valeur est False, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] assigne des valeurs d’identité.  
  
-   **Mode par défaut**: True  
  
-   **Mode optimisé**: True  
  
-   **Mode plein**: False  
  
**Conserver les valeurs NULL**  
Spécifie si SSMA conserve les valeurs null dans la source de données lorsqu’il ajoute des données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], indépendamment des valeurs par défaut qui sont spécifiés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
-   **Mode par défaut**: True  
  
-   **Mode optimisé**: False  
  
-   **Mode plein**: True  
  
**Verrous de table**  
Spécifie si SSMA verrouille les tables lorsqu’il ajoute des données aux tables lors de la migration de données. Si la valeur est False, SSMA utilise des verrous de ligne.  
  
-   **Mode par défaut**: True  
  
-   **Mode optimisé**: True  
  
-   **Mode plein**: True  
  
**Remplacez les dates non pris en charge**  
Spécifie si SSMA devrait corriger les dates d’accès qui sont antérieures à la plus ancienne [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] datetime, date (01 janvier 1753).  
  
-   Pour conserver les valeurs de date en cours, sélectionnez **ne rien faire**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] n’accepte pas les dates antérieures au 01 janvier 1753 dans une colonne datetime. Si vous utilisez des dates antérieures, vous devez convertir les valeurs datetime aux valeurs de caractère.  
  
-   Pour convertir les dates antérieures au 01 janvier 1753 avec la valeur NULL, sélectionnez **remplacer par NULL**.  
  
-   Pour remplacer les dates antérieures au 01 janvier 1753 avec une date de prise en charge, sélectionnez **remplacer la plus proche de la date de prise en charge**. Si vous sélectionnez cette valeur, par défaut le plus proche pris en charge de date sélectionnée en tant que 01 janvier 1753.  
  
**Taille de lot**  
Taille de lot utilisée lors de la migration de données. Une transaction est enregistrée une fois chaque lot. Par défaut, la taille de lot pour tous les schémas est 10 000.  
  
## <a name="see-also"></a>Voir aussi  
[Reference(Access) d’Interface utilisateur](http://msdn.microsoft.com/en-us/af24c303-4a41-449b-9c86-d6558a97e839)  
  
