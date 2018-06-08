---
title: Paramètres (Conversion) (AccessToSQL) du projet | Documents Microsoft
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
- conversion, options described
- Project Settings dialog box, Conversion
ms.assetid: bcebc635-c638-4ddb-924c-b9ccfef86388
caps.latest.revision: 16
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: f5e2bb08277adfcb2a4d609a92a726db6220552d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-conversion-accesstosql"></a>Paramètres du projet (Conversion) (AccessToSQL)
Les paramètres de projet de Conversion vous permettent de configurer la façon dont les objets sont convertis à partir des objets de base de données Access à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de base de données SQL Azure.  
  
Le volet de la Conversion est disponible dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue.  
  
-   Utilisez le **les paramètres de projet** boîte de dialogue pour définir les options de configuration pour le projet actuel. Pour accéder aux paramètres de conversion, sur le **outils** menu, sélectionnez **les paramètres de projet**, cliquez sur **général** en bas du volet gauche, puis **Conversion**.  
  
-   Utilisez le **les paramètres de projet par défaut** boîte de dialogue pour définir les options de configuration pour tous les projets. Pour accéder aux paramètres de conversion, sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichés / a été remplacée par **Version cible de la Migration** liste déroulante, cliquez sur **général** en bas du volet gauche, puis **Conversion**.  
  
## <a name="options"></a>Options  
**Ajouter une clé primaire**  
Crée une nouvelle clé primaire dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou une table SQL Azure si une table Access n’a aucune clé primaire ou un index unique.  
  
-   **Mode par défaut**: False  
  
-   **Mode optimisé**: False  
  
-   **Mode plein**: True  
  
Lorsqu’il est connecté à SQL Azure, il est par défaut la valeur True. **Ajouter des colonnes timestamp**  
Spécifie si SSMA doit créer une valeur d’horodatage si nécessaire.  
  
-   **Mode par défaut**: permettent de SSMA décider  
  
-   **Mode optimisé**: jamais  
  
-   **Mode plein**: permettent de SSMA décider  
  
**Inclure un rapport d’évaluation de données avec les rapports d’évaluation de conversion**  
Inclut une évaluation des données dans le rapport d’évaluation.  
  
-   **Mode par défaut**: True  
  
-   **Mode optimisé**: False  
  
-   **Mode plein**: True  
  
**Type de message lorsqu’une clé primaire contient des colonnes acceptant les valeurs null**  
Spécifie le type de message (avertissement, erreur ou rien) SSMA affiche dans le volet de sortie lorsqu’il détecte des clés primaires avec des colonnes acceptant les valeurs NULL.  
  
-   **Mode par défaut**: avertissement  
  
-   **Mode optimisé**: aucun message  
  
-   **Mode plein**: erreur  
  
**Type de message lorsque les colonnes clés étrangères sont de tailles différentes**  
Spécifie le type de message (avertissement, erreur ou rien) SSMA affiche dans le volet de sortie lorsqu’il trouve une clé étrangère incorrecte de texte.  
  
-   **Mode par défaut**: avertissement  
  
-   **Mode optimisé**: aucun message  
  
-   **Mode plein**: erreur  
  
**Type de message lorsque les colonnes Mémo sont indexées.**  
Spécifie le type de message (avertissement, erreur ou rien) SSMA affiche dans le volet de sortie lorsqu’il détecte un index contenant un **avoir** colonne.  
  
-   **Mode par défaut**: avertissement  
  
-   **Mode optimisé**: aucun message  
  
-   **Mode plein**: erreur  
  
**Avertir en cas d’une requête complexe utilise un caractère générique (\&#42 ;)**  
Affiche un avertissement dans le volet de sortie et liste d’erreurs quand un nom de colonne dans une instruction SELECT est un caractère générique (*).  
  
-   **Mode par défaut**: True  
  
-   **Mode optimisé**: False  
  
-   **Mode plein**: True  
  
**Avertir lors de la modification du nom d’identificateur**  
Affiche un message dans le rapport d’évaluation et dans le volet de sortie lorsque le nom d’identificateur d’objet est modifié par SSMA.  
  
-   **Mode par défaut**: True  
  
-   **Mode optimisé**: False  
  
-   **Mode plein**: True  
  
**Avertir en cas d’identificateur sera être mis entre guillemets.**  
Affiche un message dans le rapport d’évaluation et dans le volet de sortie lorsque le nom d’identificateur d’objet figurent entre guillemets par SSMA. Les identificateurs ne sont nécessaire lorsque le nom est un mot clé ou contient des caractères spéciaux.  
  
-   **Mode par défaut**: True  
  
-   **Mode optimisé**: False  
  
-   **Mode plein**: True  
  
## <a name="see-also"></a>Voir aussi  
[Reference(Access) d’Interface utilisateur](http://msdn.microsoft.com/en-us/af24c303-4a41-449b-9c86-d6558a97e839)  
  
