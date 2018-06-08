---
title: Évaluation des objets de base de données Access pour la Conversion (AccessToSQL) | Documents Microsoft
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
- assessing SQL
- assessing syntax
- assessment reports
- creating assessment reports
- estimating migration effort
- reports
- SQL, assessing
- syntax, assessing
ms.assetid: 8b9e23d6-da62-437a-8c05-8ad2628b9441
caps.latest.revision: 16
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: fb353f5906c1ed5cb45d6075f92cf183ba5e276b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="assessing-access-database-objects-for-conversion-accesstosql"></a>Évaluation des objets de base de données Access pour la Conversion (AccessToSQL)
Avant de charger des objets et de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous devez déterminer la quantité de la migration sera établie, et la durée pendant laquelle la conversion peut prendre. SSMA peut créer un rapport d’évaluation qui affiche le pourcentage des objets qui ont été correctement converti en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou syntaxe de SQL Azure et l’heure des estimations pour effectuer la migration. SSMA permet également d’afficher les problèmes spécifiques à l’origine des échecs de conversion.  
  
## <a name="creating-assessment-reports"></a>Création de rapports d’évaluation  
Lorsqu’il crée un rapport d’évaluation, SSMA convertit les objets de base de données sélectionnées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou syntaxe de SQL Azure, puis affiche les résultats.  
  
**Pour créer un rapport d’évaluation**  
  
1.  Dans l’Explorateur de métadonnées de l’accès, sélectionnez la base de données ou les bases de données que vous souhaitez évaluer.  
  
2.  Pour omettre des objets individuels, désactivez les cases à cocher en regard des objets que vous ne souhaitez pas évaluer.  
  
3.  Avec le bouton droit **bases de données**, puis sélectionnez **créer un rapport**.  
  
    Vous pouvez également analyser les objets individuels en cliquant sur un objet et en sélectionnant **créer un rapport**.  
  
    SSMA affiche la progression dans la barre d’état en bas de la fenêtre. Si le volet de résultats est visible, vous verrez également les messages dans le volet de sortie.  
  
Une fois l’évaluation terminée, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Assistant de Migration pour l’accès : rapport d’évaluation de la fenêtre s’affiche.  
  
## <a name="using-assessment-reports"></a>À l’aide de rapports d’évaluation  
La fenêtre de rapport d’évaluation contient trois volets : un volet de messages, un volet d’informations et un Explorateur.  
  
-   Le volet Explorateur vous permet de parcourir les objets qui ont été évaluées. Vous pouvez cliquer sur les éléments dans ce volet pour Explorer les clés et les index des tables individuelles.  
  
-   Le volet d’informations affiche les statistiques de conversion de l’objet sélectionné.  
  
-   Le volet de message affiche les erreurs, avertissements et messages d’information pour la conversion, et les estimations de temps pour effectuer la migration et les étapes de correction d’erreurs individuels.  
  
Vous devez corriger les erreurs avant de réexécuter le rapport d’évaluation ou convertir des schémas. Pour rechercher des erreurs, cliquez sur le **erreurs** situé dans le volet messages, puis cliquez sur chaque erreur pour afficher une liste d’objets dans laquelle l’erreur s’est produite. Si vous cliquez sur un objet dans le volet messages, toutes les erreurs et avertissements de cet objet seront affiche dans le volet détails.  
  
## <a name="next-step"></a>Étape suivante  
[Convertir des objets de base de données Access](http://msdn.microsoft.com/en-us/e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c)  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
  
