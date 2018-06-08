---
title: Test migration des objets de base de données (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 4937f6b4-86bd-4070-88df-3d216306c33a
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 02cc2deb2721759f938ba8e0244f46b4464b86fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="testing-migrated-database-objects-sybasetosql"></a>Test migration des objets de base de données (SybaseToSQL)
Microsoft SQL Server Migration Assistant pour Sybase Tester (testeur SSMA) teste automatiquement la conversion des objets de base de données et la migration des données effectuées par SSMA. Une fois que toutes les étapes de migration de SSMA sont terminées, utilisez SSMA testeur pour vérifier que les objets convertis fonctionnent de manière identique et que toutes les données a été transféré correctement.  
  
> [!NOTE]  
> Composant de testeur est désactivé dans le cas d’une connectivité Azure.  
  
Vous pouvez tester les types d’objet suivants avec SSMA testeur :  
  
-   Tables  
  
-   Procédures stockées  
  
-   Vues.  
  
-   Instructions autonomes.  
  
SSMA testeur exécute les objets sélectionnés pour le test sur Sybase et leurs équivalents dans SQL Server. Après cela, il compare les résultats selon les critères suivants :  
  
-   Sont identiques les modifications dans les données de la table ?  
  
-   Sont les valeurs des paramètres de sortie des procédures et des fonctions identiques ?  
  
-   Les fonctions retournent les mêmes résultats ?  
  
-   Sont que les jeux de résultats identiques ?  
  
> [!NOTE]  
> Attention ! N’utilisez jamais de SSMA testeur sur les systèmes de production. Lors de l’exécution de Tester le schéma source et les données sont modifiées. Pendant ce temps, la restauration complète de l’état d’origine peut être impossible pour certains types de code testé.  
  
## <a name="prerequisites"></a>Configuration requise  
Si vous souhaitez utiliser le testeur de SSMA, installer SSMA Sybase Extension avec le **installer la base de données testeur** activée.  
  
En outre, vérifiez les éléments suivants :  
  
-   Fournisseur OLE DB pour Sybase est installé sur l’ordinateur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] s’exécute.  
  
-   Intégration du Common Language Runtime (CLR) a été activée sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] moteur de base de données.  
  
Notez que la version actuelle de SSMA testeur ne prend pas en charge l’exécution en parallèle par différents utilisateurs sur le même serveur source ou cible.  
  
## <a name="getting-started"></a>Mise en route  
[Création de cas de Test &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-test-cases-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Installation des composants SSMA sur SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
[Paramètres du projet &#40;Conversion&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
