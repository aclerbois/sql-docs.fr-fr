---
title: "Test migration des objets de base de données (OracleToSQL) | Documents Microsoft"
ms.prod: sql-non-specified
ms.prod_service: sql-tools
ms.service: 
ms.component: ssma-oracle
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: sql
ms.technology: sql-ssma
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: f03ef5e1-66e6-4c84-ada2-252dd5ada82f
caps.latest.revision: "7"
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.workload: Inactive
ms.openlocfilehash: 0b20c1f5d47388a92e92402faa9017dc6b042a1c
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="testing-migrated-database-objects-oracletosql"></a>Test migration des objets de base de données (OracleToSQL)
[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]Assistant de migration pour Oracle Tester (testeur SSMA) teste automatiquement la conversion des objets de base de données et la migration des données effectuées par SSMA. Une fois que toutes les étapes de migration de SSMA sont terminées, utilisez SSMA testeur pour vérifier que les objets convertis fonctionnent de manière identique et que toutes les données a été transféré correctement.  
  
Vous pouvez tester les types d’objet suivants avec SSMA testeur :  
  
-   Tables  
  
-   Procédures stockées, y compris les procédures de packages.  
  
-   Fonctions définies par l’utilisateur, y compris empaquetées fonctions.  
  
-   Vues.  
  
-   Instructions autonomes.  
  
SSMA testeur exécute les objets sélectionnés pour le test sur Oracle et leurs équivalents dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Après cela, il compare les résultats selon les critères suivants :  
  
-   Sont identiques les modifications dans les données de la table ?  
  
-   Sont les valeurs des paramètres de sortie des procédures et des fonctions identiques ?  
  
-   Les fonctions retournent les mêmes résultats ?  
  
-   Sont que les jeux de résultats identiques ?  
  
> [!NOTE]  
> Attention ! N’utilisez jamais de SSMA testeur sur les systèmes de production. Lors de l’exécution de Tester le schéma source et les données sont modifiées. Pendant ce temps, la restauration complète de l’état d’origine peut être impossible pour certains types de code testé.  
  
## <a name="prerequisites"></a>Prerequisites  
Si vous souhaitez utiliser le testeur de SSMA, installer SSMA Oracle Extension avec le **installer la base de données testeur** activée.  
  
Pour permettre la comparaison des données de la table résultante, définissez la **colonne ROWID de générer** option **Oui** avant le démarrage de la conversion de schéma. SSMA ajoutera une colonne ROWID à toutes les tables lors de l’exécution de la **convertir le schéma** commande.  
  
En outre, vérifiez les éléments suivants :  
  
-   Outils clients Oracle sont installés sur l’ordinateur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] s’exécute.  
  
-   Intégration du Common Language Runtime (CLR) a été activée sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] moteur de base de données.  
  
Notez que la version actuelle de SSMA testeur ne prend pas en charge l’exécution en parallèle par différents utilisateurs sur le même serveur source ou cible.  
  
## <a name="getting-started"></a>Mise en route  
[Création de cas de Test &#40; OracleToSQL &#41;](../../ssma/oracle/creating-test-cases-oracletosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[L’installation des composants SSMA SQL Server &#40; OracleToSQL &#41;](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)  
[Paramètres du projet &#40; Conversion de &#41; &#40; OracleToSQL &#41;](../../ssma/oracle/project-settings-conversion-oracletosql.md)  
  
