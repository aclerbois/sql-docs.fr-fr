---
title: Actualiser à partir de la base de données (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
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
ms.assetid: 613a8368-b372-443f-8252-fb6dc31a003d
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 932314ef3325862746d2ce09ba0552ce4f4c7d2a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="refresh-from-database-db2tosql"></a>Actualiser à partir de la base de données (DB2ToSQL)
Le **Actualiser à partir de la base de données** boîte de dialogue vous permet de sélectionner les objets à actualiser à partir de la base de données DB2. Lignes dans la boîte de dialogue sont codées par couleur en fonction de l’état des métadonnées :  
  
-   Si les métadonnées de l’objet a été modifié localement et dans la base de données DB2, la ligne est bleue.  
  
-   Si les métadonnées de l’objet a été modifié dans la base de données DB2, mais pas dans SSMA, la ligne est jaune.  
  
-   Si les métadonnées de l’objet a été modifié localement, mais pas dans la base de données DB2, la ligne est verte.  
  
-   Si l’objet est nouveau dans la base de données DB2, la ligne est rose.  
  
Vous pouvez spécifier des paramètres d’actualisation objet par défaut dans le **les paramètres de projet** boîte de dialogue. Pour plus d’informations, consultez [les paramètres de projet&#40;synchronisation&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md).  
  
Pour accéder à la **Actualiser à partir de la base de données** boîte de dialogue, avec le bouton droit, un objet dans l’Explorateur de métadonnées DB2 et cliquez sur **Actualiser à partir de la base de données**.  
  
## <a name="options"></a>Options  
**Réduction (-)**  
Réduire tous les groupes d’objets pour masquer les objets individuels.  
  
**Développement (+)**  
Développez tous les groupes d’objets pour afficher les objets individuels.  
  
**Afficher/masquer les objets égales**  
Masquer les objets dans la liste si les métadonnées de l’objet sont le même dans la base de données DB2 et SSMA.  
  
**Actualiser à partir de la base de données (flèche)**  
Utilisez le bouton fléché pour spécifier que les métadonnées pour les objets sélectionnés doivent être mis à jour dans SSMA.  
  
**Effectuez pas actualiser à partir de la base de données (bouton) X**  
Utilisez le bouton X pour spécifier que les métadonnées pour les objets sélectionnés ne doivent pas mis à jour dans SSMA.  
  
**Légende**  
Affiche un **légende** boîte de dialogue. La légende contient le mappage entre les États de métadonnées et les couleurs de ligne.  
  
Pour conserver le **légende** boîte de dialogue sur le **Actualiser à partir de la base de données** boîte de dialogue, sélectionnez le **afficher en haut** case à cocher.  
  
