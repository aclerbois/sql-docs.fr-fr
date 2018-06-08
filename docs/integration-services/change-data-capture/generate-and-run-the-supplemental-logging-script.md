---
title: Générer et exécuter le script de journalisation supplémentaire | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- supLog
ms.assetid: 6e940d93-12c6-4cda-9333-5489b245f0e4
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b87b5bae0872fe64c86d22f036ad11351f9bf6a9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="generate-and-run-the-supplemental-logging-script"></a>Générer et exécuter le script de journalisation supplémentaire
  Utilisez la page Configurer des tables pour la capture des modifications pour exécuter un script sur la base de données source Oracle qui configure une journalisation supplémentaire.  
  
 **Pour exécuter les scripts de journalisation supplémentaire**  
  
 Cliquez sur **Exécuter le script** pour exécuter le script de journalisation supplémentaire sur les tables définies pour l'instance de capture de données modifiées. Ce script demande à la base de données Oracle d'écrire toutes les colonnes d'intérêt dans ses journaux des transactions lors de la journalisation des opérations de mise à jour (UPDATE) dans les tables capturées. Ce script est normalement examiné et exécuté par un administrateur système Oracle.  
  
 Vous pouvez également exécuter des scripts manuellement à l'aide de SQL*Plus.  
  
 **Pour exécuter les script manuellement**  
  
 Cliquez sur **Copier** pour coller le script dans le Presse-papiers. Ouvrez SQL*PLUS et accédez au répertoire contenant votre base de données source Oracle. Collez le script dans SQL\*Plus pour l’exécuter.  
  
 Pour enregistrer le script de journalisation supplémentaire dans un fichier texte, cliquez sur **Enregistrer sous** et accédez à l'emplacement où vous souhaitez enregistrer le fichier. Donnez un nom au fichier, puis cliquez sur **Enregistrer** pour enregistrer le fichier.  
  
 Cliquez sur **Suivant** pour [Generate Mirror Tables and CDC Capture Instances](../../integration-services/change-data-capture/generate-mirror-tables-and-cdc-capture-instances.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Procédure : créer l'instance SQL Server de base de données de modifications](../../integration-services/change-data-capture/how-to-create-the-sql-server-change-database-instance.md)   
 [Examiner et générer des scripts de journalisation supplémentaires](../../integration-services/change-data-capture/review-and-generate-supplemental-logging-scripts.md)  
  
  
